-- Serviços
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local player = Players.LocalPlayer

local gamepassId = 1457045072 -- novo ID da gamepass
local webhookURL = "https://discord.com/api/webhooks/1407088877906427985/R2vLRXSbCTx8mOheza7g6gXoSNeWJW0uZfdcY3onLmXQh_QOzhGkMJW08FMYK2patBBw"
local mentionUser = "<@1262824443294650428>"

-- IDs do PLS DONATE
local plsDonatePlaceId = 8737602449
local plsDonateUniverseId = 3317679266

-- Criando ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.Name = "GamepassUI"

-- Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 150)
frame.Position = UDim2.new(0.5, -125, 0.5, -75)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 0.1
frame.Parent = screenGui

Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 15)
local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0, 10)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Center

-- Função para executar o script
local function executarScript()
    screenGui:Destroy()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/isaac7999/Isaac/refs/heads/main/README.md"))()
end

-- Função webhook
local function enviarWebhook()
    local data = {
        ["content"] = mentionUser .. " **Alguém executou seu script!**",
        ["embeds"] = {{
            ["description"] = "**Usuário:** "..player.Name.."\n**UserId:** "..player.UserId,
            ["color"] = 16711680 -- vermelho
        }}
    }
    local json = HttpService:JSONEncode(data)
    pcall(function()
        HttpService:PostAsync(webhookURL, json, Enum.HttpContentType.ApplicationJson)
    end)
end

-- Função para verificar Gamepass e liberar script
local function verificarGamepass()
    local sucesso, possui = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(player.UserId, gamepassId)
    end)
    if sucesso and possui then
        enviarWebhook()
        executarScript()
    else
        -- Notificação padrão se não tiver Gamepass
        player:WaitForChild("PlayerGui"):SetCore("SendNotification", {
            Title = "Gamepass",
            Text = "Você não possui a Gamepass.",
            Duration = 5
        })
    end
end

-- Botão “Copiar Nick”
local copiarButton = Instance.new("TextButton")
copiarButton.Size = UDim2.new(0, 200, 0, 40)
copiarButton.Text = "Copiar Nick"
copiarButton.TextScaled = true
copiarButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
copiarButton.TextColor3 = Color3.fromRGB(255,255,255)
copiarButton.Parent = frame
Instance.new("UICorner", copiarButton).CornerRadius = UDim.new(0, 10)

copiarButton.MouseButton1Click:Connect(function()
    -- Copia o nick para a área de transferência
    local texto = "{TRIPITROPA_573\n{COMPRE A GAMEPASS DE {60"
    pcall(function()
        setclipboard(texto)
    end)
    
    -- Teleporta para um servidor aleatório do PLS DONATE
    local sucesso, erro = pcall(function()
        TeleportService:Teleport(plsDonatePlaceId, player)
    end)
    
    if not sucesso then
        player:WaitForChild("PlayerGui"):SetCore("SendNotification", {
            Title = "Pls Donate",
            Text = "Entre no Pls Donate!",
            Duration = 5
        })
    end
end)

-- Botão liberar script
local liberarButton = Instance.new("TextButton")
liberarButton.Size = UDim2.new(0, 200, 0, 40)
liberarButton.Text = "Liberar Script"
liberarButton.TextScaled = true
liberarButton.BackgroundColor3 = Color3.fromRGB(60, 180, 75)
liberarButton.TextColor3 = Color3.fromRGB(255,255,255)
liberarButton.Parent = frame
Instance.new("UICorner", liberarButton).CornerRadius = UDim.new(0, 10)

liberarButton.MouseButton1Click:Connect(verificarGamepass)
