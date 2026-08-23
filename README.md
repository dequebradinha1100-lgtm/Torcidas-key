--// RAYFIELD UI — INTERFACE SEM LÓGICA
--// Compatível com Delta / Mobile
--// Todos os botões são apenas visuais.

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Window = Rayfield:CreateWindow({
    Name = "ANONYMUS 7",
    LoadingTitle = "ANONYMUS 7",
    LoadingSubtitle = "Mobile Interface",
    ConfigurationSaving = {
        Enabled = false
    },
    Discord = {
        Enabled = false
    },
    KeySystem = false
})

--// FARM
local FarmTab = Window:CreateTab("Farm", 4483362458)

FarmTab:CreateSection("Farm")

FarmTab:CreateButton({
    Name = "Auto Farm",
    Callback = function()
        -- Sem lógica
    end
})

FarmTab:CreateButton({
    Name = "Farm de money",
    Callback = function()
        -- Sem lógica
    end
})

FarmTab:CreateButton({
    Name = "Farm de itens",
    Callback = function()
        -- Sem lógica
    end
})

FarmTab:CreateButton({
    Name = "Coletar automaticamente",
    Callback = function()
        -- Sem lógica
    end
})

--// SPAWN
local SpawnTab = Window:CreateTab("Spawn", 4483362458)

SpawnTab:CreateSection("Spawns")

SpawnTab:CreateButton({
    Name = "Spawn 1",
    Callback = function()
        -- Sem lógica
    end
})

SpawnTab:CreateButton({
    Name = "Spawn 2",
    Callback = function()
        -- Sem lógica
    end
})

SpawnTab:CreateButton({
    Name = "Spawn 3",
    Callback = function()
        -- Sem lógica
    end
})

SpawnTab:CreateButton({
    Name = "Spawn 4",
    Callback = function()
        -- Sem lógica
    end
})

--// DINHEIRO
local MoneyTab = Window:CreateTab("Dinheiro", 4483362458)

MoneyTab:CreateSection("Dinheiro")

MoneyTab:CreateButton({
    Name = "Coletar dinheiro",
    Callback = function()
        -- Sem lógica
    end
})

MoneyTab:CreateButton({
    Name = "Recompensa diária",
    Callback = function()
        -- Sem lógica
    end
})

MoneyTab:CreateButton({
    Name = "Vender itens",
    Callback = function()
        -- Sem lógica
    end
})

MoneyTab:CreateButton({
    Name = "Multiplicador",
    Callback = function()
        -- Sem lógica
    end
})

--// CONFIG
local ConfigTab = Window:CreateTab("Config", 4483362458)

ConfigTab:CreateSection("Configurações")

ConfigTab:CreateButton({
    Name = "Notificações",
    Callback = function()
        -- Sem lógica
    end
})

ConfigTab:CreateButton({
    Name = "Tema",
    Callback = function()
        -- Sem lógica
    end
})

ConfigTab:CreateButton({
    Name = "Recarregar interface",
    Callback = function()
        -- Sem lógica
    end
})

Rayfield:Notify({
    Title = "ANONYMUS 7",
    Content = "Interface carregada!",
    Duration = 3
})
