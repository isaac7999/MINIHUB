-- Serviços
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer
local gamepassId = 1456418289

-- Criando ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.Name = "GamepassUI"

-- Frame pequeno e bonito
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 120)
frame.Position = UDim2.new(0.5, -110, 0.5, -60)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 0.1
frame.Parent = screenGui

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 15)

local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0, 10)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Center

-- Botão Script por Gamepass
local scriptButton = Instance.new("TextButton")
scriptButton.Size = UDim2.new(0, 180, 0, 40)
scriptButton.Text = "SCRIPT POR GAMEPASS"
scriptButton.TextScaled = true
scriptButton.BackgroundColor3 = Color3.fromRGB(60, 180, 75)
scriptButton.TextColor3 = Color3.fromRGB(255, 255, 255)
scriptButton.Parent = frame
Instance.new("UICorner", scriptButton).CornerRadius = UDim.new(0, 10)

-- Botão Comprar
local buyButton = Instance.new("TextButton")
buyButton.Size = UDim2.new(0, 180, 0, 40)
buyButton.Text = "COMPRAR"
buyButton.TextScaled = true
buyButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
buyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
buyButton.Parent = frame
Instance.new("UICorner", buyButton).CornerRadius = UDim.new(0, 10)

-- Função para executar o script
local function executarScript()
    screenGui:Destroy()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/isaac7999/Isaac/refs/heads/main/README.md"))()
end

-- Clicar no botão Script por Gamepass
scriptButton.MouseButton1Click:Connect(function()
    local sucesso, possui = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(player.UserId, gamepassId)
    end)

    if sucesso and possui then
        executarScript()
    else
        screenGui:Destroy()
    end
end)

-- Clicar no botão Comprar
buyButton.MouseButton1Click:Connect(function()
    MarketplaceService:PromptGamePassPurchase(player, gamepassId)
end)

-- Detectar compra finalizada
MarketplaceService.PromptGamePassPurchaseFinished:Connect(function(plr, id, comprado)
    if plr == player and id == gamepassId and comprado then
        executarScript()
    end
end)
