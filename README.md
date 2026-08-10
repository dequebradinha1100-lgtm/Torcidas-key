lag = "PlayerInfJump",
    Callback = function(Value)
        Modules.Player.InfJump = Value
    end,
})

TabPlayer:CreateToggle({
    Name = "NoClip",
    CurrentValue = false,
    Flag = "PlayerNoclip",
    Callback = function(Value)
        Modules.Player.Noclip = Value
    end,
})

TabPlayer:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Flag = "PlayerFly",
    Callback = function(Value)
        Modules.Player.Fly = Value
    end,
})

local SectionVisuals = TabPlayer:CreateSection("Visuais & Utilitários")

TabPlayer:CreateToggle({
    Name = "Player ESP",
    CurrentValue = false,
    Flag = "PlayerESP",
    Callback = function(Value)
        Modules.Player.ESP = Value
        if not Value then
            for _, p in pairs(Players:GetPlayers()) do
                if p.Character and p.Character:FindFirstChild("ESPHighlight") then
                    p.Character.ESPHighlight:Destroy()
                end
            end
        end
    end,
})

TabPlayer:CreateButton({
    Name = "Reset Character",
    Callback = function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.Health = 0
        end
    end,
})

TabPlayer:CreateButton({
    Name = "Teleport para Spawn",
    Callback = function()
        local spawns = Workspace:FindFirstChild("SpawnLocation", true)
        if spawns and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = spawns.CFrame + Vector3.new(0, 5, 0)
        end
    end,
})

TabPlayer:CreateButton({
    Name = "Rejoin Server",
    Callback = function()
        TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
    end,
})

local SectionConfig = TabSettings:CreateSection("Configurações do Hub")

TabSettings:CreateToggle({
    Name = "Notificações do Sistema",
    CurrentValue = true,
    Flag = "SettingsNotif",
    Callback = function(Value)
        Modules.Player.Notifications = Value
    end,
})

TabSettings:CreateButton({
    Name = "Salvar Configurações",
    Callback = function()
        Rayfield:SaveConfiguration()
        Notify("Configurações", "Salvas com sucesso!")
    end,
})

TabSettings:CreateButton({
    Name = "Carregar Configurações",
    Callback = function()
        Rayfield:LoadConfiguration()
        Notify("Configurações", "Carregadas com sucesso!")
    end,
})

TabSettings:CreateButton({
    Name = "Resetar Configurações",
    Callback = function()
        delfile("Torcidas7Hub/Config.txt")
        Notify("Configurações", "Arquivo de configuração deletado.")
    end,
})

TabSettings:CreateDropdown({
    Name = "Trocar Tema (Requer Re-execução)",
    Options = {"Default", "DarkBlue", "Light", "Ocean"},
    CurrentOption = {"Default"},
    MultipleOptions = false,
    Flag = "SettingsTheme",
    Callback = function(Option)
        Notify("Tema", "Alterado para " .. Option[1] .. ". Salve e re-execute.")
    end,
})

TabSettings:CreateButton({
    Name = "Copiar Link do Discord",
    Callback = function()
        setclipboard("https://discord.gg/torcidas7")
        Notify("Discord", "Link copiado para a área de transferência!")
    end,
})

TabSettings:CreateButton({
    Name = "Minimizar Interface (Ou aperte K)",
    Callback = function()
        -- Minimizar é controlado nativamente pelo Keybind principal do Rayfield.
    end,
})

TabSettings:CreateButton({
    Name = "Destruir GUI",
    Callback = function()
        CleanUp()
        Rayfield:Destroy()
    end,
})

local SectionInfo = TabSettings:CreateSection("Sobre")
TabSettings:CreateLabel("Hub: TORCIDAS 7")
TabSettings:CreateLabel("Versão: 1.0.0 (Stable)")
TabSettings:CreateLabel("Status: Undetected")

Modules.Connections.RenderStepped = RunService.RenderStepped:Connect(function()
    FOVCircle.Position = UserInputService:GetMouseLocation()
    
    if Modules.Aimbot.Enabled then
        local target = GetClosestPlayer()
        if target and target.Character and target.Character:FindFirstChild(Modules.Aimbot.TargetPart) then
            local targetPos = target.Character[Modules.Aimbot.TargetPart].Position
            local currentCamCFrame = Camera.CFrame
            local targetCamCFrame = CFrame.new(currentCamCFrame.Position, targetPos)
            if Modules.Aimbot.Smoothness > 1 then
                Camera.CFrame = currentCamCFrame:Lerp(targetCamCFrame, 1 / Modules.Aimbot.Smoothness)
            else
                Camera.CFrame = targetCamCFrame
            end
        end
    end

    if Modules.Player.ESP then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local highlight = p.Character:FindFirstChild("ESPHighlight")
                if not highlight then
                    highlight = Instance.new("Highlight")
                    highlight.Name = "ESPHighlight"
                    highlight.Parent = p.Character
                    highlight.FillColor = Color3.fromRGB(255, 255, 255)
                    highlight.OutlineColor = Color3.fromRGB(255, 0, 0)
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                end
                highlight.FillColor = IsEnemy(p) and Color3.fromRGB(255,0,0) or Color3.fromRGB(0,255,0)
            end
        end
    end
end)

Modules.Connections.Stepped = RunService.Stepped:Connect(function()
    if Modules.Hitbox.Enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and IsEnemy(player) and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = player.Character.HumanoidRootPart
                if not Modules.Hitbox.OriginalStates[player.Name] then
                    Modules.Hitbox.OriginalStates[player.Name] = {
                        Size = hrp.Size,
                        Transparency = hrp.Transparency,
                        Color = hrp.Color,
                        Material = hrp.Material,
                        CanCollide = hrp.CanCollide
                    }
                end
                hrp.Size = Vector3.new(Modules.Hitbox.Size, Modules.Hitbox.Size, Modules.Hitbox.Size)
                hrp.Transparency = Modules.Hitbox.Transparency
                hrp.Color = Modules.Hitbox.Color
                hrp.Material = Enum.Material.ForceField
                hrp.CanCollide = false
            end
        end
    end

    if Modules.Player.Noclip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

Modules.Connections.Heartbeat = RunService.Heartbeat:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local velocity = LocalPlayer.Character.HumanoidRootPart.Velocity.Magnitude
        SpeedLabel:Set("Velocidade Atual: " .. math.floor(velocity))
        
        if Modules.Player.Fly then
            local hrp = LocalPlayer.Character.HumanoidRootPart
            hrp.Velocity = Vector3.new(0,0,0)
            local moveDir = LocalPlayer.Character.Humanoid.MoveDirection
            if moveDir.Magnitude > 0 then
                hrp.CFrame = hrp.CFrame + (Camera.CFrame.LookVector * (moveDir.Z * -1) * (Modules.Player.FlySpeed/100))
                hrp.CFrame = hrp.CFrame + (Camera.CFrame.RightVector * moveDir.X * (Modules.Player.FlySpeed/100))
            end
        end
    end
end)

Modules.Connections.JumpRequest = UserInputService.JumpRequest:Connect(function()
    if Modules.Player.InfJump and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

Modules.Connections.AntiAFK = LocalPlayer.Idled:Connect(function()
    VirtualUser:Button2Down(Vector2.new(0,0), Camera.CFrame)
    task.wait(1)
    VirtualUser:Button2Up(Vector2.new(0,0), Camera.CFrame)
end)

Rayfield:LoadConfiguration()
Notify("TORCIDAS 7", "Injetado com sucesso!")
