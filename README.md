-- Lunaris Hub 🌙 - Versão Final 55.0
-- 100% FUNCIONAL - SEM BUGS - DESIGN ROXO ESCURO
-- Criado por Lunaris Team

-- ============================================
-- SERVIÇOS E VARIÁVEIS GLOBAIS
-- ============================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local DataStoreService = game:GetService("DataStoreService")

-- ============================================
-- CONFIGURAÇÕES DO HUB
-- ============================================

local Lunaris = {
    Name = "Lunaris Hub 🌙",
    Version = "55.0",
    Developer = "Lunaris Team",
    Discord = "discord.gg/lunaris",
    DiscordLink = "https://discord.gg/AJstMHVspK",
    Keybind = Enum.KeyCode.K,
    PremiumKey = "LUNARISPREMIUMHUB2026SCRIPTPVP",
    FreeKey = "LUNARISHUB2026SCRIPTPVP"
}

-- Tema Roxo Escuro Premium
local Theme = {
    TextColor = Color3.fromRGB(255, 255, 255),
    Background = Color3.fromRGB(5, 3, 10),
    Surface = Color3.fromRGB(12, 8, 18),
    Topbar = Color3.fromRGB(45, 10, 90),
    TopbarGradient = Color3.fromRGB(75, 20, 140),
    Accent = Color3.fromRGB(75, 20, 140),
    AccentDark = Color3.fromRGB(45, 10, 90),
    AccentGlow = Color3.fromRGB(100, 30, 180),
    
    NotificationBackground = Color3.fromRGB(45, 10, 90),
    TabBackground = Color3.fromRGB(18, 12, 28),
    TabStroke = Color3.fromRGB(75, 20, 140),
    TabBackgroundSelected = Color3.fromRGB(75, 20, 140),
    TabTextColor = Color3.fromRGB(200, 200, 255),
    SelectedTabTextColor = Color3.fromRGB(255, 255, 255),
    
    ElementBackground = Color3.fromRGB(18, 12, 28),
    ElementBackgroundHover = Color3.fromRGB(30, 15, 50),
    ElementStroke = Color3.fromRGB(75, 20, 140),
    
    SliderBackground = Color3.fromRGB(30, 15, 50),
    SliderProgress = Color3.fromRGB(75, 20, 140),
    
    ToggleEnabled = Color3.fromRGB(75, 20, 140),
    ToggleDisabled = Color3.fromRGB(30, 20, 45),
    
    ButtonGlow = Color3.fromRGB(75, 20, 140)
}

-- ============================================
-- DETECTAR MOBILE E AJUSTAR TAMANHOS
-- ============================================

local isMobile = UserInputService.TouchEnabled and not UserInputService.MouseEnabled
local windowWidth = isMobile and 340 or 800
local windowHeight = isMobile and 500 or 600
local elementWidth = isMobile and 320 or 740
local buttonWidth = isMobile and 150 or 200

-- ============================================
-- SISTEMA DE KEY COM DATASTORE
-- ============================================

local KeySystem = {}

-- Tenta conectar ao DataStore
local function setupDataStore()
    local success, dataStore = pcall(function()
        return DataStoreService:GetDataStore("LunarisHubPremiumKeys")
    end)
    
    if success then
        return dataStore
    end
    return nil
end

local DataStore = setupDataStore()

function KeySystem:SavePremium(userId)
    if not DataStore then return false end
    
    local success, err = pcall(function()
        DataStore:SetAsync(tostring(userId), true)
    end)
    
    return success
end

function KeySystem:LoadPremium(userId)
    if not DataStore then return false end
    
    local success, value = pcall(function()
        return DataStore:GetAsync(tostring(userId))
    end)
    
    if success and value then
        return true
    end
    return false
end

-- ============================================
-- FUNÇÃO DE NOTIFICAÇÃO
-- ============================================

function ShowNotification(title, content, duration, parent)
    duration = duration or 3
    parent = parent or game:GetService("CoreGui")
    
    local notif = Instance.new("Frame")
    notif.Size = UDim2.new(0, isMobile and 280 or 300, 0, isMobile and 55 or 60)
    notif.Position = UDim2.new(0.5, isMobile and -140 or -150, 0, -70)
    notif.BackgroundColor3 = Theme.NotificationBackground
    notif.BackgroundTransparency = 0.1
    notif.Parent = parent
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 8)
    notifCorner.Parent = notif
    
    local notifStroke = Instance.new("UIStroke")
    notifStroke.Color = Theme.Accent
    notifStroke.Thickness = 1
    notifStroke.Parent = notif
    
    local notifTitle = Instance.new("TextLabel")
    notifTitle.Size = UDim2.new(1, -20, 0, 20)
    notifTitle.Position = UDim2.new(0, 10, 0, 8)
    notifTitle.BackgroundTransparency = 1
    notifTitle.Text = title
    notifTitle.TextColor3 = Theme.TextColor
    notifTitle.TextSize = isMobile and 12 or 14
    notifTitle.TextXAlignment = Enum.TextXAlignment.Left
    notifTitle.Font = Enum.Font.GothamBold
    notifTitle.Parent = notif
    
    local notifContent = Instance.new("TextLabel")
    notifContent.Size = UDim2.new(1, -20, 0, 20)
    notifContent.Position = UDim2.new(0, 10, 0, 28)
    notifContent.BackgroundTransparency = 1
    notifContent.Text = content
    notifContent.TextColor3 = Theme.TextColor
    notifContent.TextSize = isMobile and 10 or 12
    notifContent.TextXAlignment = Enum.TextXAlignment.Left
    notifContent.TextWrapped = true
    notifContent.Font = Enum.Font.Gotham
    notifContent.Parent = notif
    
    local progress = Instance.new("Frame")
    progress.Size = UDim2.new(1, 0, 0, 2)
    progress.Position = UDim2.new(0, 0, 1, -2)
    progress.BackgroundColor3 = Theme.Accent
    progress.Parent = notif
    
    TweenService:Create(notif, TweenInfo.new(0.3), {Position = UDim2.new(0.5, isMobile and -140 or -150, 0, 10)}):Play()
    TweenService:Create(progress, TweenInfo.new(duration), {Size = UDim2.new(0, 0, 0, 2)}):Play()
    
    task.wait(duration)
    TweenService:Create(notif, TweenInfo.new(0.3), {Position = UDim2.new(0.5, isMobile and -140 or -150, 0, -70)}):Play()
    task.wait(0.3)
    notif:Destroy()
end

-- ============================================
-- TELA DE KEY
-- ============================================

local KeyGUI = Instance.new("ScreenGui")
KeyGUI.Name = "LunarisKeySystem"
KeyGUI.ResetOnSpawn = false
KeyGUI.Parent = game:GetService("CoreGui")

local KeyContainer = Instance.new("Frame")
KeyContainer.Name = "KeyContainer"
KeyContainer.Size = UDim2.new(0, isMobile and 340 or 450, 0, isMobile and 450 or 400)
KeyContainer.Position = UDim2.new(0.5, isMobile and -170 or -225, 0.5, isMobile and -225 or -200)
KeyContainer.BackgroundColor3 = Theme.Surface
KeyContainer.BackgroundTransparency = 0.05
KeyContainer.Parent = KeyGUI

local KeyCorner = Instance.new("UICorner")
KeyCorner.CornerRadius = UDim.new(0, 20)
KeyCorner.Parent = KeyContainer

local KeyStroke = Instance.new("UIStroke")
KeyStroke.Color = Theme.Accent
KeyStroke.Thickness = 2
KeyStroke.Transparency = 0.3
KeyStroke.Parent = KeyContainer

local KeyLogo = Instance.new("ImageLabel")
KeyLogo.Size = UDim2.new(0, isMobile and 60 or 80, 0, isMobile and 60 or 80)
KeyLogo.Position = UDim2.new(0.5, isMobile and -30 or -40, 0, isMobile and 20 or 30)
KeyLogo.BackgroundTransparency = 1
KeyLogo.Image = "rbxassetid://6023426953"
KeyLogo.ImageColor3 = Theme.Accent
KeyLogo.Parent = KeyContainer

local KeyTitle = Instance.new("TextLabel")
KeyTitle.Size = UDim2.new(1, 0, 0, isMobile and 40 or 50)
KeyTitle.Position = UDim2.new(0, 0, 0, isMobile and 90 or 120)
KeyTitle.BackgroundTransparency = 1
KeyTitle.Text = "LUNARIS HUB"
KeyTitle.TextColor3 = Theme.TextColor
KeyTitle.TextSize = isMobile and 24 or 32
KeyTitle.Font = Enum.Font.GothamBold
KeyTitle.Parent = KeyContainer

local KeySubtitle = Instance.new("TextLabel")
KeySubtitle.Size = UDim2.new(1, 0, 0, isMobile and 30 or 25)
KeySubtitle.Position = UDim2.new(0, 0, 0, isMobile and 130 or 170)
KeySubtitle.BackgroundTransparency = 1
KeySubtitle.Text = "SISTEMA DE KEY"
KeySubtitle.TextColor3 = Theme.Accent
KeySubtitle.TextSize = isMobile and 16 or 18
KeySubtitle.Font = Enum.Font.GothamBold
KeySubtitle.Parent = KeyContainer

local KeyWarning = Instance.new("TextLabel")
KeyWarning.Size = UDim2.new(1, -40, 0, isMobile and 60 or 50)
KeyWarning.Position = UDim2.new(0, 20, 0, isMobile and 170 or 210)
KeyWarning.BackgroundTransparency = 1
KeyWarning.Text = "⚠️ KEY NÃO SALVA AUTOMATICAMENTE ⚠️\nCompre KeyPremium no Discord para salvar permanentemente!"
KeyWarning.TextColor3 = Color3.fromRGB(255, 200, 0)
KeyWarning.TextSize = isMobile and 11 or 13
KeyWarning.TextWrapped = true
KeyWarning.Font = Enum.Font.GothamBold
KeyWarning.Parent = KeyContainer

local KeyBox = Instance.new("TextBox")
KeyBox.Size = UDim2.new(0, isMobile and 280 or 350, 0, isMobile and 45 or 50)
KeyBox.Position = UDim2.new(0.5, isMobile and -140 or -175, 0, isMobile and 240 or 280)
KeyBox.BackgroundColor3 = Theme.ElementBackground
KeyBox.PlaceholderText = "Digite sua key aqui..."
KeyBox.PlaceholderColor3 = Theme.TextColor
KeyBox.Text = ""
KeyBox.TextColor3 = Theme.TextColor
KeyBox.TextSize = isMobile and 14 or 16
KeyBox.Font = Enum.Font.Gotham
KeyBox.ClearTextOnFocus = false
KeyBox.Parent = KeyContainer

local KeyBoxCorner = Instance.new("UICorner")
KeyBoxCorner.CornerRadius = UDim.new(0, 8)
KeyBoxCorner.Parent = KeyBox

local KeyBoxStroke = Instance.new("UIStroke")
KeyBoxStroke.Color = Theme.Accent
KeyBoxStroke.Thickness = 1
KeyBoxStroke.Parent = KeyBox

local VerifyBtn = Instance.new("TextButton")
VerifyBtn.Size = UDim2.new(0, isMobile and 280 or 350, 0, isMobile and 40 or 45)
VerifyBtn.Position = UDim2.new(0.5, isMobile and -140 or -175, 0, isMobile and 300 or 350)
VerifyBtn.BackgroundColor3 = Theme.Accent
VerifyBtn.Text = "VERIFICAR KEY"
VerifyBtn.TextColor3 = Theme.TextColor
VerifyBtn.TextSize = isMobile and 16 or 18
VerifyBtn.Font = Enum.Font.GothamBold
VerifyBtn.Parent = KeyContainer

local VerifyCorner = Instance.new("UICorner")
VerifyCorner.CornerRadius = UDim.new(0, 8)
VerifyCorner.Parent = VerifyBtn

local DiscordBtn = Instance.new("TextButton")
DiscordBtn.Size = UDim2.new(0, isMobile and 280 or 350, 0, isMobile and 40 or 45)
DiscordBtn.Position = UDim2.new(0.5, isMobile and -140 or -175, 0, isMobile and 360 or 415)
DiscordBtn.BackgroundColor3 = Color3.fromRGB(88, 101, 242)
DiscordBtn.Text = "📋 COPIAR DISCORD"
DiscordBtn.TextColor3 = Theme.TextColor
DiscordBtn.TextSize = isMobile and 16 or 18
DiscordBtn.Font = Enum.Font.GothamBold
DiscordBtn.Parent = KeyContainer

local DiscordCorner = Instance.new("UICorner")
DiscordCorner.CornerRadius = UDim.new(0, 8)
DiscordCorner.Parent = DiscordBtn

local KeyStatus = Instance.new("TextLabel")
KeyStatus.Size = UDim2.new(1, -40, 0, 30)
KeyStatus.Position = UDim2.new(0, 20, 0, isMobile and 420 or 470)
KeyStatus.BackgroundTransparency = 1
KeyStatus.Text = ""
KeyStatus.TextColor3 = Theme.TextColor
KeyStatus.TextSize = isMobile and 12 or 14
KeyStatus.Font = Enum.Font.Gotham
KeyStatus.Parent = KeyContainer

-- ============================================
-- FUNÇÃO SIMPLES PARA COPIAR DISCORD
-- ============================================

DiscordBtn.MouseButton1Click:Connect(function()
    local success = pcall(function()
        setclipboard(Lunaris.DiscordLink)
    end)
    
    if success then
        ShowNotification("📋 Discord", "Link copiado!", 2, KeyGUI)
    else
        ShowNotification("❌ Erro", "Não foi possível copiar", 2, KeyGUI)
    end
end)

-- ============================================
-- VERIFICAR KEY PREMIUM NO DATASTORE
-- ============================================

local userId = LocalPlayer.UserId
local isPremium = KeySystem:LoadPremium(userId)

if isPremium then
    -- Se já é premium, pular tela de key
    KeyGUI:Destroy()
    StartMainScript()
else
    -- Mostrar tela de key
    VerifyBtn.MouseButton1Click:Connect(function()
        local enteredKey = KeyBox.Text:upper():gsub("%s+", "")
        
        if enteredKey == Lunaris.PremiumKey then
            -- Salvar no DataStore
            local saved = KeySystem:SavePremium(userId)
            
            if saved then
                KeyStatus.Text = "✅ KEY PREMIUM ATIVADA! (SALVA NO DATASTORE)"
                KeyStatus.TextColor3 = Color3.fromRGB(0, 255, 0)
            else
                KeyStatus.Text = "✅ KEY PREMIUM ATIVADA! (NÃO FOI POSSÍVEL SALVAR)"
                KeyStatus.TextColor3 = Color3.fromRGB(255, 255, 0)
            end
            
            KeyBox.Text = ""
            task.wait(1.5)
            KeyGUI:Destroy()
            StartMainScript()
            
        elseif enteredKey == Lunaris.FreeKey then
            KeyStatus.Text = "✅ KEY FREE ATIVADA! (NÃO SALVA)"
            KeyStatus.TextColor3 = Color3.fromRGB(255, 255, 0)
            KeyBox.Text = ""
            task.wait(1.5)
            KeyGUI:Destroy()
            StartMainScript()
            
        else
            KeyStatus.Text = "❌ KEY INVÁLIDA! Tente novamente."
            KeyStatus.TextColor3 = Color3.fromRGB(255, 0, 0)
            KeyBox.Text = ""
        end
    end)
end

-- ============================================
-- FUNÇÃO PRINCIPAL
-- ============================================

function StartMainScript()
    -- ============================================
    -- TELA DE CARREGAMENTO
    -- ============================================

    local LoadScreen = Instance.new("ScreenGui")
    LoadScreen.Name = "LunarisLoader"
    LoadScreen.ResetOnSpawn = false
    LoadScreen.Parent = game:GetService("CoreGui")

    local LoaderContainer = Instance.new("Frame")
    LoaderContainer.Name = "LoaderContainer"
    LoaderContainer.Size = UDim2.new(0, isMobile and 340 or 450, 0, isMobile and 280 or 300)
    LoaderContainer.Position = UDim2.new(0.5, isMobile and -170 or -225, 0.5, isMobile and -140 or -150)
    LoaderContainer.BackgroundColor3 = Theme.Surface
    LoaderContainer.BackgroundTransparency = 0.05
    LoaderContainer.Parent = LoadScreen

    local LoaderCorner = Instance.new("UICorner")
    LoaderCorner.CornerRadius = UDim.new(0, 20)
    LoaderCorner.Parent = LoaderContainer

    local LoaderStroke = Instance.new("UIStroke")
    LoaderStroke.Color = Theme.Accent
    LoaderStroke.Thickness = 2
    LoaderStroke.Transparency = 0.3
    LoaderStroke.Parent = LoaderContainer

    local Logo = Instance.new("ImageLabel")
    Logo.Size = UDim2.new(0, isMobile and 60 or 80, 0, isMobile and 60 or 80)
    Logo.Position = UDim2.new(0.5, isMobile and -30 or -40, 0, isMobile and 20 or 30)
    Logo.BackgroundTransparency = 1
    Logo.Image = "rbxassetid://6023426953"
    Logo.ImageColor3 = Theme.Accent
    Logo.Parent = LoaderContainer

    local LoaderTitle = Instance.new("TextLabel")
    LoaderTitle.Size = UDim2.new(1, 0, 0, isMobile and 40 or 50)
    LoaderTitle.Position = UDim2.new(0, 0, 0, isMobile and 90 or 120)
    LoaderTitle.BackgroundTransparency = 1
    LoaderTitle.Text = Lunaris.Name
    LoaderTitle.TextColor3 = Theme.TextColor
    LoaderTitle.TextSize = isMobile and 24 or 32
    LoaderTitle.Font = Enum.Font.GothamBold
    LoaderTitle.Parent = LoaderContainer

    local LoaderSubtitle = Instance.new("TextLabel")
    LoaderSubtitle.Size = UDim2.new(1, 0, 0, isMobile and 20 or 25)
    LoaderSubtitle.Position = UDim2.new(0, 0, 0, isMobile and 130 or 170)
    LoaderSubtitle.BackgroundTransparency = 1
    LoaderSubtitle.Text = "Carregando..."
    LoaderSubtitle.TextColor3 = Theme.TextColor
    LoaderSubtitle.TextSize = isMobile and 14 or 16
    LoaderSubtitle.Font = Enum.Font.Gotham
    LoaderSubtitle.Parent = LoaderContainer

    local ProgressBg = Instance.new("Frame")
    ProgressBg.Size = UDim2.new(0, isMobile and 280 or 350, 0, 8)
    ProgressBg.Position = UDim2.new(0.5, isMobile and -140 or -175, 0, isMobile and 160 or 210)
    ProgressBg.BackgroundColor3 = Theme.SliderBackground
    ProgressBg.Parent = LoaderContainer

    local ProgressCorner = Instance.new("UICorner")
    ProgressCorner.CornerRadius = UDim.new(0, 4)
    ProgressCorner.Parent = ProgressBg

    local ProgressBar = Instance.new("Frame")
    ProgressBar.Size = UDim2.new(0, 0, 1, 0)
    ProgressBar.BackgroundColor3 = Theme.SliderProgress
    ProgressBar.Parent = ProgressBg

    local ProgressBarCorner = Instance.new("UICorner")
    ProgressBarCorner.CornerRadius = UDim.new(0, 4)
    ProgressBarCorner.Parent = ProgressBar

    local VersionText = Instance.new("TextLabel")
    VersionText.Size = UDim2.new(1, 0, 0, 25)
    VersionText.Position = UDim2.new(0, 0, 1, -35)
    VersionText.BackgroundTransparency = 1
    VersionText.Text = "v" .. Lunaris.Version .. " 🌙" .. (isPremium and " [PREMIUM]" or "")
    VersionText.TextColor3 = Theme.TextColor
    VersionText.TextTransparency = 0.5
    VersionText.TextSize = isMobile and 12 or 14
    VersionText.Font = Enum.Font.Gotham
    VersionText.Parent = LoaderContainer

    -- Carregamento rápido
    local loadSteps = {
        {text = "Carregando...", progress = 50},
        {text = "Pronto! 🌙", progress = 100}
    }

    for _, step in ipairs(loadSteps) do
        LoaderSubtitle.Text = step.text
        TweenService:Create(ProgressBar, TweenInfo.new(0.1), {Size = UDim2.new(step.progress/100, 0, 1, 0)}):Play()
        task.wait(0.1)
    end

    task.wait(0.1)
    LoadScreen:Destroy()

    -- ============================================
    -- INTERFACE PRINCIPAL
    -- ============================================

    local MainGUI = Instance.new("ScreenGui")
    MainGUI.Name = "LunarisHub"
    MainGUI.ResetOnSpawn = false
    MainGUI.Parent = game:GetService("CoreGui")
    MainGUI.DisplayOrder = 999999
    MainGUI.IgnoreGuiInset = true

    local MainContainer = Instance.new("Frame")
    MainContainer.Name = "MainContainer"
    MainContainer.Size = UDim2.new(0, windowWidth, 0, windowHeight)
    MainContainer.Position = UDim2.new(0.5, -windowWidth/2, 0.5, -windowHeight/2)
    MainContainer.BackgroundColor3 = Theme.Background
    MainContainer.BackgroundTransparency = 0.05
    MainContainer.ClipsDescendants = true
    MainContainer.Parent = MainGUI

    local MainCorner = Instance.new("UICorner")
    MainCorner.CornerRadius = UDim.new(0, 20)
    MainCorner.Parent = MainContainer

    local MainStroke = Instance.new("UIStroke")
    MainStroke.Color = Theme.Accent
    MainStroke.Thickness = 2
    MainStroke.Transparency = 0.3
    MainStroke.Parent = MainContainer

    -- TOPBAR
    local Topbar = Instance.new("Frame")
    Topbar.Name = "Topbar"
    Topbar.Size = UDim2.new(1, 0, 0, isMobile and 45 or 55)
    Topbar.BackgroundColor3 = Theme.Topbar
    Topbar.Parent = MainContainer

    local TopbarCorner = Instance.new("UICorner")
    TopbarCorner.CornerRadius = UDim.new(0, 20)
    TopbarCorner.Parent = Topbar

    local TopbarGradient = Instance.new("UIGradient")
    TopbarGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Theme.Topbar),
        ColorSequenceKeypoint.new(1, Theme.TopbarGradient)
    })
    TopbarGradient.Rotation = 90
    TopbarGradient.Parent = Topbar

    local HubIcon = Instance.new("ImageLabel")
    HubIcon.Size = UDim2.new(0, isMobile and 24 or 30, 0, isMobile and 24 or 30)
    HubIcon.Position = UDim2.new(0, isMobile and 10 or 15, 0.5, isMobile and -12 or -15)
    HubIcon.BackgroundTransparency = 1
    HubIcon.Image = "rbxassetid://6023426953"
    HubIcon.ImageColor3 = Color3.new(1, 1, 1)
    HubIcon.Parent = Topbar

    local HubTitle = Instance.new("TextLabel")
    HubTitle.Size = UDim2.new(0, isMobile and 150 or 250, 1, 0)
    HubTitle.Position = UDim2.new(0, isMobile and 40 or 55, 0, 0)
    HubTitle.BackgroundTransparency = 1
    HubTitle.Text = isMobile and "LUNARIS" or "LUNARIS HUB 🌙"
    HubTitle.TextColor3 = Theme.TextColor
    HubTitle.TextSize = isMobile and 14 or 22
    HubTitle.TextXAlignment = Enum.TextXAlignment.Left
    HubTitle.Font = Enum.Font.GothamBold
    HubTitle.Parent = Topbar

    local TopbarVersion = Instance.new("TextLabel")
    TopbarVersion.Size = UDim2.new(0, isMobile and 80 or 120, 1, 0)
    TopbarVersion.Position = UDim2.new(0, isMobile and 190 or 300, 0, 0)
    TopbarVersion.BackgroundTransparency = 1
    TopbarVersion.Text = isMobile and ("v"..Lunaris.Version) or ("v" .. Lunaris.Version .. (isPremium and " [PREMIUM]" or ""))
    TopbarVersion.TextColor3 = Theme.TextColor
    TopbarVersion.TextTransparency = 0.3
    TopbarVersion.TextSize = isMobile and 10 or 14
    TopbarVersion.TextXAlignment = Enum.TextXAlignment.Left
    TopbarVersion.Font = Enum.Font.Gotham
    TopbarVersion.Parent = Topbar

    local ButtonContainer = Instance.new("Frame")
    ButtonContainer.Size = UDim2.new(0, isMobile and 80 or 100, 1, 0)
    ButtonContainer.Position = UDim2.new(1, isMobile and -80 or -100, 0, 0)
    ButtonContainer.BackgroundTransparency = 1
    ButtonContainer.Parent = Topbar

    local MinimizeBtn = Instance.new("ImageButton")
    MinimizeBtn.Size = UDim2.new(0, isMobile and 24 or 30, 0, isMobile and 24 or 30)
    MinimizeBtn.Position = UDim2.new(0, isMobile and 5 or 10, 0.5, isMobile and -12 or -15)
    MinimizeBtn.BackgroundTransparency = 1
    MinimizeBtn.Image = "rbxassetid://6031094678"
    MinimizeBtn.ImageColor3 = Color3.new(1, 1, 1)
    MinimizeBtn.Parent = ButtonContainer

    local CloseBtn = Instance.new("ImageButton")
    CloseBtn.Size = UDim2.new(0, isMobile and 24 or 30, 0, isMobile and 24 or 30)
    CloseBtn.Position = UDim2.new(0, isMobile and 35 or 50, 0.5, isMobile and -12 or -15)
    CloseBtn.BackgroundTransparency = 1
    CloseBtn.Image = "rbxassetid://6031094686"
    CloseBtn.ImageColor3 = Color3.new(1, 1, 1)
    CloseBtn.Parent = ButtonContainer

    -- Sistema de arrastar
    local dragging = false
    local dragStart, startPos

    Topbar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = MainContainer.Position
        end
    end)

    Topbar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            MainContainer.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)

    -- SISTEMA DE ABAS
    local TabContainer = Instance.new("Frame")
    TabContainer.Size = UDim2.new(1, 0, 0, isMobile and 40 or 45)
    TabContainer.Position = UDim2.new(0, 0, 0, isMobile and 45 or 55)
    TabContainer.BackgroundColor3 = Theme.TabBackground
    TabContainer.Parent = MainContainer

    local Tabs = {
        {id = "Player", name = isMobile and "👤" or "👤 PLAYER"},
        {id = "Combat", name = isMobile and "⚔️" or "⚔️ COMBATE"},
        {id = "Visual", name = isMobile and "👁️" or "👁️ VISUAL"},
        {id = "Config", name = isMobile and "+++" or "+++"},
        {id = "Credits", name = isMobile and "💜" or "💜 CRÉDITOS"}
    }

    local TabButtons = {}
    local CurrentTab = "Player"
    local tabWidth = isMobile and 56 or 150
    local isUpdatingTab = false

    for i, tab in ipairs(Tabs) do
        local btn = Instance.new("TextButton")
        btn.Name = tab.id .. "Tab"
        btn.Size = UDim2.new(0, tabWidth, 0, isMobile and 30 or 35)
        btn.Position = UDim2.new(0, isMobile and 3 + (i-1) * (tabWidth + 3) or 5 + (i-1) * (tabWidth + 5), 0, isMobile and 5 or 5)
        btn.BackgroundColor3 = tab.id == CurrentTab and Theme.TabBackgroundSelected or Theme.ElementBackground
        btn.Text = tab.name
        btn.TextColor3 = tab.id == CurrentTab and Theme.SelectedTabTextColor or Theme.TabTextColor
        btn.TextSize = isMobile and 10 or 13
        btn.Font = Enum.Font.GothamBold
        btn.Parent = TabContainer
        
        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = btn
        
        local btnStroke = Instance.new("UIStroke")
        btnStroke.Color = Theme.TabStroke
        btnStroke.Thickness = 1
        btnStroke.Transparency = 0.5
        btnStroke.Parent = btn
        
        TabButtons[tab.id] = btn
        
        btn.MouseButton1Click:Connect(function()
            if isUpdatingTab then return end
            isUpdatingTab = true
            
            CurrentTab = tab.id
            for id, button in pairs(TabButtons) do
                button.BackgroundColor3 = id == CurrentTab and Theme.TabBackgroundSelected or Theme.ElementBackground
                button.TextColor3 = id == CurrentTab and Theme.SelectedTabTextColor or Theme.TabTextColor
            end
            
            spawn(function()
                UpdateTabContent(tab.id)
                isUpdatingTab = false
            end)
        end)
    end

    -- ÁREA DE CONTEÚDO
    local ContentArea = Instance.new("ScrollingFrame")
    ContentArea.Name = "ContentArea"
    ContentArea.Size = UDim2.new(1, -20, 1, -100)
    ContentArea.Position = UDim2.new(0, 10, 0, isMobile and 95 or 110)
    ContentArea.BackgroundTransparency = 1
    ContentArea.BorderSizePixel = 0
    ContentArea.ScrollBarThickness = isMobile and 3 or 6
    ContentArea.ScrollBarImageColor3 = Theme.Accent
    ContentArea.CanvasSize = UDim2.new(0, 0, 0, 0)
    ContentArea.Parent = MainContainer

    local ContentList = Instance.new("UIListLayout")
    ContentList.Padding = UDim.new(0, isMobile and 8 or 10)
    ContentList.HorizontalAlignment = Enum.HorizontalAlignment.Center
    ContentList.Parent = ContentArea

    -- ============================================
    -- VARIÁVEIS DAS FEATURES
    -- ============================================

    -- Player
    local noclipEnabled = false
    local noclipConnection = nil
    local speedEnabled = false
    local speedValue = 0
    local speedConnection = nil
    local originalWalkSpeed = 16

    -- Visual
    local espEnabled = false
    local espConnections = {}
    local espLines = {}
    local espLineColor = Theme.Accent
    local espLineTransparency = 0.5
    local espLineThickness = 2

    local staffEspEnabled = false
    local staffEspConnections = {}
    local staffEspLines = {}

    local box2DEnabled = false
    local box2DColor = Theme.Accent
    local box2DShowName = true
    local box2DShowDistance = true
    local box2DObjects = {}
    local box2DConnections = {}

    -- Combat
    local aimbotEnabled = false
    local aimbotConnection = nil
    local aimbotFov = 150
    local aimbotStrength = 5
    local fovCircle = nil
    local fovColor = Theme.Accent
    local fovVisible = true

    local aimbot4DEnabled = false
    local aimbot4DConnection = nil
    local aimbot4DFov = 200
    local aimbot4DFovCircle = nil
    local aimbot4DColor = Color3.fromRGB(100, 30, 180)
    local aimbot4DFovVisible = true
    local aimbot4DTime = 0

    local antiTeamEnabled = false
    local myTeam = nil

    -- NOVAS FEATURES
    local aimGodEnabled = false
    local aimGodKey = nil
    local aimGodKeyText = "Nenhuma"

    local cabecaQuenteEnabled = false
    local cabecaQuenteSize = 2
    local cabecaQuenteConnections = {}

    -- NOVA FEATURE: Anti-Invisível
    local antiInvisivelEnabled = false

    -- ============================================
    -- FUNÇÕES DE UTILIDADE
    -- ============================================

    local function getPlayerTeam(player)
        if player.Team then return player.Team end
        
        local success, teamValue = pcall(function()
            return player:FindFirstChild("Team") or player:FindFirstChild("TeamColor") or player:FindFirstChild("TeamName")
        end)
        
        if success and teamValue then return teamValue end
        
        if player.Character then
            local teamTag = player.Character:FindFirstChild("Team") or player.Character:FindFirstChild("TeamName")
            if teamTag then return teamTag end
        end
        
        return nil
    end

    local function isSameTeam(player1, player2)
        if not antiTeamEnabled then return false end
        
        local team1 = getPlayerTeam(player1)
        local team2 = getPlayerTeam(player2)
        
        if team1 and team2 then
            if type(team1) == "string" and type(team2) == "string" then
                return team1 == team2
            end
            
            if typeof(team1) == "Instance" and typeof(team2) == "Instance" then
                return team1 == team2
            end
        end
        
        return false
    end

    local function isStaff(player)
        local name = player.Name:upper()
        local displayName = player.DisplayName:upper()
        
        local staffTags = {"STAFF", "ADMIN", "MOD", "OWNER", "FUNDADOR", "CRIADOR", "DEV", "DESENVOLVEDOR", "GERENTE", "SUPORTE"}
        
        for _, tag in ipairs(staffTags) do
            if string.find(name, tag) or string.find(displayName, tag) then
                return true
            end
        end
        
        local team = getPlayerTeam(player)
        if team then
            local teamName = ""
            if type(team) == "string" then
                teamName = team:upper()
            elseif typeof(team) == "Instance" and team.Name then
                teamName = team.Name:upper()
            end
            
            for _, tag in ipairs(staffTags) do
                if string.find(teamName, tag) then
                    return true
                end
            end
        end
        
        return false
    end

    -- NOVA FUNÇÃO: Verificar se o jogador está visível
    local function isPlayerVisible(player)
        if not player.Character then return false end
        
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if not humanoid or humanoid.Health <= 0 then return false end
        
        -- Verificar se tem partes invisíveis ou está completamente invisível
        local head = player.Character:FindFirstChild("Head")
        local torso = player.Character:FindFirstChild("Torso") or player.Character:FindFirstChild("UpperTorso")
        
        if head and head.Transparency >= 0.9 then
            return false
        end
        
        if torso and torso.Transparency >= 0.9 then
            return false
        end
        
        -- Verificar se o jogador está realmente na tela (não é apenas um efeito visual)
        local rootPart = player.Character:FindFirstChild("HumanoidRootPart")
        if rootPart then
            local pos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
            if not onScreen then
                return false
            end
        end
        
        return true
    end

    local function canTarget(player)
        local localPlayer = game.Players.LocalPlayer
        
        if player == localPlayer then return false end
        if antiTeamEnabled and isSameTeam(localPlayer, player) then return false end
        
        -- NOVA VERIFICAÇÃO: Anti-Invisível
        if antiInvisivelEnabled and not isPlayerVisible(player) then
            return false
        end
        
        return true
    end

    -- ============================================
    -- FUNÇÕES DAS FEATURES
    -- ============================================

    -- NOCLIP
    function startNoclip()
        if noclipConnection then noclipConnection:Disconnect() end
        noclipConnection = RunService.Stepped:Connect(function()
            if noclipEnabled and LocalPlayer.Character then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") and v.CanCollide then
                        v.CanCollide = false
                    end
                end
            end
        end)
    end

    function stopNoclip()
        if noclipConnection then
            noclipConnection:Disconnect()
            noclipConnection = nil
        end
        if LocalPlayer.Character then
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = true
                end
            end
        end
    end

    -- SPEED
    function startSpeed()
        if speedConnection then speedConnection:Disconnect() end
        speedConnection = RunService.Heartbeat:Connect(function()
            if speedEnabled and LocalPlayer.Character then
                local humanoid = LocalPlayer.Character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = originalWalkSpeed + speedValue
                end
            end
        end)
    end

    function stopSpeed()
        if speedConnection then
            speedConnection:Disconnect()
            speedConnection = nil
        end
        if LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = originalWalkSpeed
            end
        end
    end

    -- SISTEMA AIM GOD
    local function isAimGodKeyPressed()
        if not aimGodEnabled or not aimGodKey then return true end
        
        if aimGodKey.Type == "Keyboard" then
            return UserInputService:IsKeyDown(aimGodKey.Key)
        elseif aimGodKey.Type == "Mouse" then
            return UserInputService:IsMouseButtonPressed(aimGodKey.Button)
        end
        
        return true
    end

    -- AIMBOT NORMAL
    function startAimbotWorking()
        aimbotConnection = RunService.RenderStepped:Connect(function()
            if not aimbotEnabled then return end
            
            -- Verificar AimGod
            if not isAimGodKeyPressed() then
                return
            end
            
            local center = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
            local closestHead = nil
            local shortestDistance = aimbotFov
            
            for _, target in pairs(Players:GetPlayers()) do
                if canTarget(target) and target.Character then
                    local head = target.Character:FindFirstChild("Head")
                    local humanoid = target.Character:FindFirstChild("Humanoid")
                    
                    if head and humanoid and humanoid.Health > 0 then
                        local headPos, onScreen = Camera:WorldToViewportPoint(head.Position)
                        
                        if onScreen then
                            local distance = (center - Vector2.new(headPos.X, headPos.Y)).Magnitude
                            
                            if distance < shortestDistance then
                                shortestDistance = distance
                                closestHead = head
                            end
                        end
                    end
                end
            end
            
            if closestHead then
                local targetCF = CFrame.lookAt(Camera.CFrame.Position, closestHead.Position)
                local strength = aimbotStrength / 10
                if aimbotStrength == 10 then strength = 1.0 end
                Camera.CFrame = Camera.CFrame:Lerp(targetCF, strength)
            end
        end)
    end

    -- AIMBOT 4D
    function startAimbot4D()
        aimbot4DTime = 0
        aimbot4DConnection = RunService.RenderStepped:Connect(function(dt)
            if not aimbot4DEnabled then return end
            
            -- Verificar AimGod
            if not isAimGodKeyPressed() then
                return
            end
            
            aimbot4DTime = aimbot4DTime + dt * 4
            local center = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
            local targetHead = nil
            local targetPos = nil
            
            for _, target in pairs(Players:GetPlayers()) do
                if canTarget(target) and target.Character then
                    local head = target.Character:FindFirstChild("Head")
                    local humanoid = target.Character:FindFirstChild("Humanoid")
                    
                    if head and humanoid and humanoid.Health > 0 then
                        local headPos, onScreen = Camera:WorldToViewportPoint(head.Position)
                        
                        if onScreen then
                            local distance = (center - Vector2.new(headPos.X, headPos.Y)).Magnitude
                            
                            if distance <= aimbot4DFov then
                                targetHead = head
                                targetPos = head.Position
                                break
                            end
                        end
                    end
                end
            end
            
            if targetHead and targetPos then
                local offset = math.sin(aimbot4DTime) * 1.5
                local targetWithOffset = targetPos + (Camera.CFrame.RightVector * offset)
                local targetCF = CFrame.lookAt(Camera.CFrame.Position, targetWithOffset)
                Camera.CFrame = Camera.CFrame:Lerp(targetCF, 0.7)
            end
        end)
    end

    -- FOV
    local function createFovCircle()
        if fovCircle then 
            fovCircle:Destroy()
            fovCircle = nil
        end
        
        if not aimbotEnabled or not fovVisible then return end
        
        fovCircle = Drawing.new("Circle")
        fovCircle.Visible = true
        fovCircle.Radius = aimbotFov
        fovCircle.Thickness = 2
        fovCircle.Color = fovColor
        fovCircle.Filled = false
        fovCircle.NumSides = 64
        fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    end

    local function createAimbot4DFovCircle()
        if aimbot4DFovCircle then 
            aimbot4DFovCircle:Destroy()
            aimbot4DFovCircle = nil
        end
        
        if not aimbot4DEnabled or not aimbot4DFovVisible then return end
        
        aimbot4DFovCircle = Drawing.new("Circle")
        aimbot4DFovCircle.Visible = true
        aimbot4DFovCircle.Radius = aimbot4DFov
        aimbot4DFovCircle.Thickness = 2
        aimbot4DFovCircle.Color = aimbot4DColor
        aimbot4DFovCircle.Filled = false
        aimbot4DFovCircle.NumSides = 64
        aimbot4DFovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    end

    RunService.RenderStepped:Connect(function()
        if fovCircle and fovCircle.Visible then
            fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
        end
        if aimbot4DFovCircle and aimbot4DFovCircle.Visible then
            aimbot4DFovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
        end
    end)

    -- ESP LINHAS
    function startESP()
        local player = LocalPlayer
        
        local function createESPLine(targetPlayer)
            if targetPlayer == player then return end
            
            local line = Drawing.new("Line")
            line.Visible = true
            line.Color = espLineColor
            line.Thickness = espLineThickness
            line.Transparency = 1 - espLineTransparency
            
            table.insert(espLines, line)
            
            local connection = RunService.RenderStepped:Connect(function()
                if not espEnabled or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then
                    line.Visible = false
                    return
                end
                
                if antiTeamEnabled and isSameTeam(player, targetPlayer) then
                    line.Visible = false
                    return
                end
                
                line.Color = espLineColor
                line.Thickness = espLineThickness
                line.Transparency = 1 - espLineTransparency
                
                local targetPos = targetPlayer.Character.Head.Position
                local screenPos, onScreen = Camera:WorldToViewportPoint(targetPos)
                
                if onScreen then
                    line.Visible = true
                    line.From = Vector2.new(screenPos.X, screenPos.Y)
                    line.To = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
                else
                    line.Visible = false
                end
            end)
            
            table.insert(espConnections, {
                player = targetPlayer,
                line = line,
                connection = connection
            })
        end
        
        for _, target in pairs(Players:GetPlayers()) do
            createESPLine(target)
        end
        
        table.insert(espConnections, {
            type = "playerAdded",
            connection = Players.PlayerAdded:Connect(function(newPlayer)
                task.wait(0.1)
                if espEnabled then
                    createESPLine(newPlayer)
                end
            end)
        })
        
        table.insert(espConnections, {
            type = "playerRemoving",
            connection = Players.PlayerRemoving:Connect(function(targetPlayer)
                for i, data in pairs(espConnections) do
                    if data.player == targetPlayer and data.line then
                        data.line:Destroy()
                        table.remove(espConnections, i)
                        break
                    end
                end
            end)
        })
    end

    function stopESP()
        for _, data in pairs(espConnections) do
            if data.connection then data.connection:Disconnect() end
            if data.line then data.line:Destroy() end
        end
        espConnections = {}
        espLines = {}
    end

    -- ESP STAFF
    function startStaffESP()
        local player = LocalPlayer
        
        local staffColors = {
            Color3.fromRGB(255, 0, 0),
            Color3.fromRGB(255, 165, 0),
            Color3.fromRGB(255, 255, 0),
            Color3.fromRGB(0, 255, 0),
            Color3.fromRGB(0, 0, 255),
            Color3.fromRGB(75, 20, 140),
            Color3.fromRGB(255, 192, 203)
        }
        
        local function createStaffLine(targetPlayer)
            if targetPlayer == player or not isStaff(targetPlayer) then return end
            
            local line = Drawing.new("Line")
            line.Visible = true
            line.Thickness = 4
            line.Transparency = 0.1
            
            table.insert(staffEspLines, line)
            
            local colorIndex = 1
            local lastUpdate = tick()
            
            local connection = RunService.RenderStepped:Connect(function()
                if not staffEspEnabled or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then
                    line.Visible = false
                    return
                end
                
                if not isStaff(targetPlayer) then
                    line.Visible = false
                    return
                end
                
                if antiTeamEnabled and isSameTeam(player, targetPlayer) then
                    line.Visible = false
                    return
                end
                
                if tick() - lastUpdate > 0.1 then
                    colorIndex = colorIndex + 1
                    if colorIndex > #staffColors then colorIndex = 1 end
                    line.Color = staffColors[colorIndex]
                    lastUpdate = tick()
                end
                
                local targetPos = targetPlayer.Character.Head.Position
                local screenPos, onScreen = Camera:WorldToViewportPoint(targetPos)
                
                if onScreen then
                    line.Visible = true
                    line.From = Vector2.new(screenPos.X, screenPos.Y)
                    line.To = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
                else
                    line.Visible = false
                end
            end)
            
            table.insert(staffEspConnections, {
                player = targetPlayer,
                line = line,
                connection = connection
            })
        end
        
        for _, target in pairs(Players:GetPlayers()) do
            createStaffLine(target)
        end
        
        table.insert(staffEspConnections, {
            type = "playerAdded",
            connection = Players.PlayerAdded:Connect(function(newPlayer)
                task.wait(0.1)
                if staffEspEnabled then
                    createStaffLine(newPlayer)
                end
            end)
        })
        
        table.insert(staffEspConnections, {
            type = "playerRemoving",
            connection = Players.PlayerRemoving:Connect(function(targetPlayer)
                for i, data in pairs(staffEspConnections) do
                    if data.player == targetPlayer then
                        if data.line then data.line:Destroy() end
                        if data.connection then data.connection:Disconnect() end
                        table.remove(staffEspConnections, i)
                        break
                    end
                end
            end)
        })
    end

    function stopStaffESP()
        for _, data in pairs(staffEspConnections) do
            if data.connection then data.connection:Disconnect() end
            if data.line then data.line:Destroy() end
        end
        staffEspConnections = {}
        staffEspLines = {}
    end

    -- CAIXINHA 2D
    local function createBox2D(player)
        if player == LocalPlayer then return end
        
        local rainbowColors = {
            Color3.fromRGB(255, 0, 0),
            Color3.fromRGB(255, 165, 0),
            Color3.fromRGB(255, 255, 0),
            Color3.fromRGB(0, 255, 0),
            Color3.fromRGB(0, 0, 255),
            Color3.fromRGB(75, 20, 140),
            Color3.fromRGB(255, 192, 203)
        }
        
        local colorIndex = 1
        local lastUpdate = tick()
        
        local function updateBox()
            if not box2DEnabled or not player.Character then
                if box2DObjects[player] then
                    for _, obj in pairs(box2DObjects[player]) do
                        if obj then obj:Destroy() end
                    end
                    box2DObjects[player] = nil
                end
                return
            end
            
            if antiTeamEnabled and isSameTeam(LocalPlayer, player) then
                if box2DObjects[player] then
                    for _, obj in pairs(box2DObjects[player]) do
                        if obj then obj.Visible = false end
                    end
                end
                return
            end
            
            local character = player.Character
            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            local head = character:FindFirstChild("Head")
            
            if not humanoid or not rootPart or not head or humanoid.Health <= 0 then
                if box2DObjects[player] then
                    for _, obj in pairs(box2DObjects[player]) do
                        if obj then obj:Destroy() end
                    end
                    box2DObjects[player] = nil
                end
                return
            end
            
            local isStaffPlayer = isStaff(player)
            
            if isStaffPlayer and tick() - lastUpdate > 0.1 then
                colorIndex = colorIndex + 1
                if colorIndex > #rainbowColors then colorIndex = 1 end
                lastUpdate = tick()
            end
            
            local headPos = head.Position
            local rootPos = rootPart.Position
            local footPos = rootPos - Vector3.new(0, 2, 0)
            local topPos = headPos + Vector3.new(0, 0.5, 0)
            
            local topPoint, topVisible = Camera:WorldToViewportPoint(topPos)
            local footPoint, footVisible = Camera:WorldToViewportPoint(footPos)
            
            if not topVisible and not footVisible then
                if box2DObjects[player] then
                    for _, obj in pairs(box2DObjects[player]) do
                        if obj then obj.Visible = false end
                    end
                end
                return
            end
            
            local boxHeight = math.abs(topPoint.Y - footPoint.Y)
            local boxWidth = boxHeight * 0.5
            local centerX = topPoint.X
            local left = centerX - boxWidth/2
            local right = centerX + boxWidth/2
            local top = topPoint.Y - 5
            local bottom = footPoint.Y + 5
            
            local currentColor = box2DColor
            if isStaffPlayer then currentColor = rainbowColors[colorIndex] end
            
            if not box2DObjects[player] then
                box2DObjects[player] = {}
                
                for i = 1, 4 do
                    local line = Drawing.new("Line")
                    line.Visible = true
                    line.Color = currentColor
                    line.Thickness = 2
                    line.Transparency = 1
                    table.insert(box2DObjects[player], line)
                end
                
                local nameText = Drawing.new("Text")
                nameText.Visible = box2DShowName
                nameText.Color = currentColor
                nameText.Size = isMobile and 12 or 16
                nameText.Center = true
                nameText.Outline = true
                nameText.Font = 2
                nameText.Text = player.Name .. (isStaffPlayer and " [STAFF]" or "")
                table.insert(box2DObjects[player], nameText)
                
                local distText = Drawing.new("Text")
                distText.Visible = box2DShowDistance
                distText.Color = Color3.fromRGB(255, 255, 255)
                distText.Size = isMobile and 10 or 12
                distText.Center = true
                distText.Outline = true
                distText.Font = 2
                table.insert(box2DObjects[player], distText)
            end
            
            if box2DObjects[player] then
                box2DObjects[player][1].From = Vector2.new(left, top)
                box2DObjects[player][1].To = Vector2.new(right, top)
                box2DObjects[player][1].Color = currentColor
                box2DObjects[player][1].Visible = true
                
                box2DObjects[player][2].From = Vector2.new(right, top)
                box2DObjects[player][2].To = Vector2.new(right, bottom)
                box2DObjects[player][2].Color = currentColor
                box2DObjects[player][2].Visible = true
                
                box2DObjects[player][3].From = Vector2.new(right, bottom)
                box2DObjects[player][3].To = Vector2.new(left, bottom)
                box2DObjects[player][3].Color = currentColor
                box2DObjects[player][3].Visible = true
                
                box2DObjects[player][4].From = Vector2.new(left, bottom)
                box2DObjects[player][4].To = Vector2.new(left, top)
                box2DObjects[player][4].Color = currentColor
                box2DObjects[player][4].Visible = true
                
                local nameText = box2DObjects[player][5]
                if nameText then
                    nameText.Visible = box2DShowName
                    nameText.Color = currentColor
                    nameText.Position = Vector2.new(centerX, top - 18)
                    nameText.Text = player.Name .. (isStaffPlayer and " [STAFF]" or "")
                end
                
                local distText = box2DObjects[player][6]
                if distText and box2DShowDistance then
                    local distance = (Camera.CFrame.Position - rootPos).Magnitude
                    distText.Position = Vector2.new(centerX, bottom + 18)
                    distText.Text = string.format("%.1f m", distance)
                    distText.Visible = true
                elseif distText then
                    distText.Visible = false
                end
            end
        end
        
        local connection = RunService.RenderStepped:Connect(updateBox)
        table.insert(box2DConnections, {
            player = player,
            connection = connection
        })
    end

    -- CABEÇA QUENTE
    function startCabecaQuente()
        for _, conn in ipairs(cabecaQuenteConnections) do
            if conn then conn:Disconnect() end
        end
        cabecaQuenteConnections = {}
        
        if not cabecaQuenteEnabled then return end
        
        local function applyHeadSize(player)
            if player == LocalPlayer then return end
            
            local conn = player.CharacterAdded:Connect(function(character)
                task.wait(0.5)
                if not cabecaQuenteEnabled then return end
                
                local head = character:FindFirstChild("Head")
                if head then
                    local sizeMultiplier = 1.0 + (cabecaQuenteSize * 0.05)
                    head.Size = head.Size * sizeMultiplier
                    head.CFrame = head.CFrame + Vector3.new(0, 0.2 * cabecaQuenteSize, 0)
                end
            end)
            
            table.insert(cabecaQuenteConnections, conn)
            
            if player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head then
                    local sizeMultiplier = 1.0 + (cabecaQuenteSize * 0.05)
                    head.Size = head.Size * sizeMultiplier
                    head.CFrame = head.CFrame + Vector3.new(0, 0.2 * cabecaQuenteSize, 0)
                end
            end
        end
        
        for _, player in ipairs(Players:GetPlayers()) do
            applyHeadSize(player)
        end
        
        table.insert(cabecaQuenteConnections, Players.PlayerAdded:Connect(function(player)
            task.wait(0.5)
            if cabecaQuenteEnabled then
                applyHeadSize(player)
            end
        end))
    end

    function stopCabecaQuente()
        for _, conn in ipairs(cabecaQuenteConnections) do
            if conn then conn:Disconnect() end
        end
        cabecaQuenteConnections = {}
    end

    -- ============================================
    -- FUNÇÃO CreateKeybind
    -- ============================================

    function CreateKeybind(data)
        local title = data.Name
        local defaultText = aimGodKeyText
        local callback = data.Callback or function() end
        
        local bind = Instance.new("Frame")
        bind.Size = UDim2.new(0, elementWidth, 0, isMobile and 50 or 55)
        bind.BackgroundColor3 = Theme.ElementBackground
        bind.BackgroundTransparency = 0.1
        bind.Parent = ContentArea
        
        local bindCorner = Instance.new("UICorner")
        bindCorner.CornerRadius = UDim.new(0, 8)
        bindCorner.Parent = bind
        
        local bindStroke = Instance.new("UIStroke")
        bindStroke.Color = Theme.ElementStroke
        bindStroke.Thickness = 1
        bindStroke.Transparency = 0.5
        bindStroke.Parent = bind
        
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, -120, 1, 0)
        label.Position = UDim2.new(0, 15, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = title
        label.TextColor3 = Theme.TextColor
        label.TextSize = isMobile and 12 or 15
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.Gotham
        label.Parent = bind
        
        local keyButton = Instance.new("TextButton")
        keyButton.Size = UDim2.new(0, isMobile and 80 or 100, 0, isMobile and 30 or 35)
        keyButton.Position = UDim2.new(1, isMobile and -90 or -110, 0.5, isMobile and -15 or -17.5)
        keyButton.BackgroundColor3 = Theme.Accent
        keyButton.Text = aimGodKeyText
        keyButton.TextColor3 = Theme.TextColor
        keyButton.TextSize = isMobile and 11 or 13
        keyButton.Font = Enum.Font.GothamBold
        keyButton.Parent = bind
        
        local keyCorner = Instance.new("UICorner")
        keyCorner.CornerRadius = UDim.new(0, 6)
        keyCorner.Parent = keyButton
        
        local function getKeyName(input)
            if input.KeyCode and input.KeyCode ~= Enum.KeyCode.Unknown then
                local keyName = input.KeyCode.Name
                
                local keyNames = {
                    Zero = "0", One = "1", Two = "2", Three = "3", Four = "4",
                    Five = "5", Six = "6", Seven = "7", Eight = "8", Nine = "9",
                    
                    A = "A", B = "B", C = "C", D = "D", E = "E", F = "F", G = "G",
                    H = "H", I = "I", J = "J", K = "K", L = "L", M = "M", N = "N",
                    O = "O", P = "P", Q = "Q", R = "R", S = "S", T = "T", U = "U",
                    V = "V", W = "W", X = "X", Y = "Y", Z = "Z",
                    
                    LeftShift = "Shift", RightShift = "Shift",
                    LeftControl = "Ctrl", RightControl = "Ctrl",
                    LeftAlt = "Alt", RightAlt = "Alt",
                    CapsLock = "Caps", Tab = "Tab", Escape = "Esc",
                    Space = "Espaço", Return = "Enter", Backspace = "Backspace",
                    Delete = "Del", Insert = "Insert", Home = "Home", End = "End",
                    PageUp = "PageUp", PageDown = "PageDown",
                    
                    Up = "↑", Down = "↓", Left = "←", Right = "→",
                    
                    F1 = "F1", F2 = "F2", F3 = "F3", F4 = "F4", F5 = "F5",
                    F6 = "F6", F7 = "F7", F8 = "F8", F9 = "F9", F10 = "F10",
                    F11 = "F11", F12 = "F12",
                    
                    Semicolon = ";", Apostrophe = "'", Comma = ",", Period = ".",
                    Slash = "/", Backslash = "\\", LeftBracket = "[", RightBracket = "]",
                    Minus = "-", Equals = "=", Grave = "`"
                }
                
                return keyNames[keyName] or keyName
            end
            
            if input.UserInputType then
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    return "Mouse1"
                elseif input.UserInputType == Enum.UserInputType.MouseButton2 then
                    return "Mouse2"
                elseif input.UserInputType == Enum.UserInputType.MouseButton3 then
                    return "Mouse3"
                end
            end
            
            return "?"
        end
        
        keyButton.MouseButton1Click:Connect(function()
            keyButton.Text = "..."
            keyButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            
            local connection
            connection = UserInputService.InputBegan:Connect(function(input, gameProcessed)
                if input.KeyCode == Enum.KeyCode.Escape then
                    keyButton.Text = aimGodKeyText
                    keyButton.BackgroundColor3 = Theme.Accent
                    connection:Disconnect()
                    return
                end
                
                local keyData = nil
                local keyText = ""
                
                if input.KeyCode and input.KeyCode ~= Enum.KeyCode.Unknown then
                    keyData = {
                        Type = "Keyboard",
                        Key = input.KeyCode
                    }
                    keyText = getKeyName(input)
                    
                elseif input.UserInputType == Enum.UserInputType.MouseButton1 or
                       input.UserInputType == Enum.UserInputType.MouseButton2 or
                       input.UserInputType == Enum.UserInputType.MouseButton3 then
                    
                    keyData = {
                        Type = "Mouse",
                        Button = input.UserInputType
                    }
                    keyText = getKeyName(input)
                end
                
                if keyData then
                    aimGodKey = keyData
                    aimGodKeyText = keyText
                    
                    keyButton.Text = keyText
                    keyButton.BackgroundColor3 = Theme.Accent
                    
                    callback(keyData)
                    
                    ShowNotification("🔑 AimGod", "Tecla definida: " .. keyText, 2, MainGUI)
                    
                    connection:Disconnect()
                end
            end)
            
            task.delay(10, function()
                if keyButton.Text == "..." then
                    keyButton.Text = aimGodKeyText
                    keyButton.BackgroundColor3 = Theme.Accent
                    if connection then connection:Disconnect() end
                end
            end)
        end)
        
        return bind
    end

    -- ============================================
    -- FUNÇÕES DE CRIAÇÃO DE ELEMENTOS
    -- ============================================

    function CreateSection(title)
        local section = Instance.new("Frame")
        section.Size = UDim2.new(0, elementWidth, 0, isMobile and 25 or 30)
        section.BackgroundTransparency = 1
        section.Parent = ContentArea
        
        local line = Instance.new("Frame")
        line.Size = UDim2.new(1, -20, 0, 2)
        line.Position = UDim2.new(0, 10, 0, isMobile and 20 or 25)
        line.BackgroundColor3 = Theme.Accent
        line.BackgroundTransparency = 0.5
        line.Parent = section
        
        local lineCorner = Instance.new("UICorner")
        lineCorner.CornerRadius = UDim.new(0, 1)
        lineCorner.Parent = line
        
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(0, 250, 0, 20)
        label.Position = UDim2.new(0, 15, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = "  " .. title
        label.TextColor3 = Theme.Accent
        label.TextSize = isMobile and 14 or 18
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.GothamBold
        label.Parent = section
        
        return section
    end

    function CreateToggle(data)
        local title = data.Name
        local default = data.CurrentValue or false
        local callback = data.Callback or function() end
        
        local toggle = Instance.new("Frame")
        toggle.Size = UDim2.new(0, elementWidth, 0, isMobile and 40 or 45)
        toggle.BackgroundColor3 = Theme.ElementBackground
        toggle.BackgroundTransparency = 0.1
        toggle.Parent = ContentArea
        
        local toggleCorner = Instance.new("UICorner")
        toggleCorner.CornerRadius = UDim.new(0, 8)
        toggleCorner.Parent = toggle
        
        local toggleStroke = Instance.new("UIStroke")
        toggleStroke.Color = Theme.ElementStroke
        toggleStroke.Thickness = 1
        toggleStroke.Transparency = 0.5
        toggleStroke.Parent = toggle
        
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, -100, 1, 0)
        label.Position = UDim2.new(0, 15, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = title
        label.TextColor3 = Theme.TextColor
        label.TextSize = isMobile and 12 or 15
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.Gotham
        label.Parent = toggle
        
        local toggleBg = Instance.new("Frame")
        toggleBg.Size = UDim2.new(0, isMobile and 40 or 50, 0, isMobile and 20 or 24)
        toggleBg.Position = UDim2.new(1, isMobile and -50 or -65, 0.5, isMobile and -10 or -12)
        toggleBg.BackgroundColor3 = default and Theme.ToggleEnabled or Theme.ToggleDisabled
        toggleBg.Parent = toggle
        
        local toggleBgCorner = Instance.new("UICorner")
        toggleBgCorner.CornerRadius = UDim.new(0, isMobile and 10 or 12)
        toggleBgCorner.Parent = toggleBg
        
        local toggleCircle = Instance.new("Frame")
        toggleCircle.Size = UDim2.new(0, isMobile and 16 or 20, 0, isMobile and 16 or 20)
        toggleCircle.Position = default and UDim2.new(1, isMobile and -20 or -24, 0.5, isMobile and -8 or -10) or UDim2.new(0, isMobile and 4 or 4, 0.5, isMobile and -8 or -10)
        toggleCircle.BackgroundColor3 = Color3.new(1, 1, 1)
        toggleCircle.Parent = toggleBg
        
        local circleCorner = Instance.new("UICorner")
        circleCorner.CornerRadius = UDim.new(0, isMobile and 8 or 10)
        circleCorner.Parent = toggleCircle
        
        local state = default
        
        toggleBg.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                state = not state
                toggleBg.BackgroundColor3 = state and Theme.ToggleEnabled or Theme.ToggleDisabled
                toggleCircle.Position = state and UDim2.new(1, isMobile and -20 or -24, 0.5, isMobile and -8 or -10) or UDim2.new(0, isMobile and 4 or 4, 0.5, isMobile and -8 or -10)
                callback(state)
                ShowNotification("⚙️ " .. title, state and "Ativado!" or "Desativado!", 1, MainGUI)
            end
        end)
        
        return toggle
    end

    function CreateSlider(data)
        local title = data.Name
        local min = data.Range[1]
        local max = data.Range[2]
        local default = data.CurrentValue or min
        local suffix = data.Suffix or ""
        local callback = data.Callback or function() end
        
        local slider = Instance.new("Frame")
        slider.Size = UDim2.new(0, elementWidth, 0, isMobile and 65 or 70)
        slider.BackgroundColor3 = Theme.ElementBackground
        slider.BackgroundTransparency = 0.1
        slider.Parent = ContentArea
        
        local sliderCorner = Instance.new("UICorner")
        sliderCorner.CornerRadius = UDim.new(0, 8)
        sliderCorner.Parent = slider
        
        local sliderStroke = Instance.new("UIStroke")
        sliderStroke.Color = Theme.ElementStroke
        sliderStroke.Thickness = 1
        sliderStroke.Transparency = 0.5
        sliderStroke.Parent = slider
        
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, -20, 0, 20)
        label.Position = UDim2.new(0, 15, 0, 10)
        label.BackgroundTransparency = 1
        label.Text = title
        label.TextColor3 = Theme.TextColor
        label.TextSize = isMobile and 12 or 15
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.Gotham
        label.Parent = slider
        
        local valueLabel = Instance.new("TextLabel")
        valueLabel.Size = UDim2.new(0, 70, 0, 20)
        valueLabel.Position = UDim2.new(1, isMobile and -75 or -85, 0, 10)
        valueLabel.BackgroundTransparency = 1
        valueLabel.Text = default .. suffix
        valueLabel.TextColor3 = Theme.Accent
        valueLabel.TextSize = isMobile and 14 or 16
        valueLabel.Font = Enum.Font.GothamBold
        valueLabel.Parent = slider
        
        local sliderBg = Instance.new("Frame")
        sliderBg.Size = UDim2.new(1, -40, 0, 6)
        sliderBg.Position = UDim2.new(0, 20, 0, isMobile and 40 or 45)
        sliderBg.BackgroundColor3 = Theme.SliderBackground
        sliderBg.Parent = slider
        
        local sliderBgCorner = Instance.new("UICorner")
        sliderBgCorner.CornerRadius = UDim.new(0, 3)
        sliderBgCorner.Parent = sliderBg
        
        local sliderProgress = Instance.new("Frame")
        sliderProgress.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
        sliderProgress.BackgroundColor3 = Theme.SliderProgress
        sliderProgress.Parent = sliderBg
        
        local progressCorner = Instance.new("UICorner")
        progressCorner.CornerRadius = UDim.new(0, 3)
        progressCorner.Parent = sliderProgress
        
        local currentValue = default
        local dragging = false
        
        local function updateValue(input)
            local pos = input.Position.X - sliderBg.AbsolutePosition.X
            local percent = math.clamp(pos / sliderBg.AbsoluteSize.X, 0, 1)
            currentValue = math.floor(min + (max - min) * percent)
            sliderProgress.Size = UDim2.new(percent, 0, 1, 0)
            valueLabel.Text = currentValue .. suffix
            callback(currentValue)
        end
        
        sliderBg.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                updateValue(input)
                dragging = true
            end
        end)
        
        sliderBg.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                updateValue(input)
            end
        end)
        
        sliderBg.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = false
                ShowNotification("⚙️ " .. title, "Valor: " .. currentValue .. suffix, 1, MainGUI)
            end
        end)
        
        return slider
    end

    function CreateColorPicker(data)
        local title = data.Name
        local default = data.Color or Theme.Accent
        local callback = data.Callback or function() end
        
        local picker = Instance.new("Frame")
        picker.Size = UDim2.new(0, elementWidth, 0, isMobile and 50 or 55)
        picker.BackgroundColor3 = Theme.ElementBackground
        picker.BackgroundTransparency = 0.1
        picker.Parent = ContentArea
        
        local pickerCorner = Instance.new("UICorner")
        pickerCorner.CornerRadius = UDim.new(0, 8)
        pickerCorner.Parent = picker
        
        local pickerStroke = Instance.new("UIStroke")
        pickerStroke.Color = Theme.ElementStroke
        pickerStroke.Thickness = 1
        pickerStroke.Transparency = 0.5
        pickerStroke.Parent = picker
        
        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, -100, 1, 0)
        label.Position = UDim2.new(0, 15, 0, 0)
        label.BackgroundTransparency = 1
        label.Text = title
        label.TextColor3 = Theme.TextColor
        label.TextSize = isMobile and 12 or 15
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.Gotham
        label.Parent = picker
        
        local colorDisplay = Instance.new("Frame")
        colorDisplay.Size = UDim2.new(0, isMobile and 25 or 30, 0, isMobile and 25 or 30)
        colorDisplay.Position = UDim2.new(1, isMobile and -35 or -45, 0.5, isMobile and -12.5 or -15)
        colorDisplay.BackgroundColor3 = default
        colorDisplay.Parent = picker
        
        local displayCorner = Instance.new("UICorner")
        displayCorner.CornerRadius = UDim.new(0, 6)
        displayCorner.Parent = colorDisplay
        
        local displayStroke = Instance.new("UIStroke")
        displayStroke.Color = Theme.TextColor
        displayStroke.Thickness = 1
        displayStroke.Parent = colorDisplay
        
        local currentColor = default
        local colorIndex = 1
        local colors = {
            Color3.fromRGB(255, 0, 0),
            Color3.fromRGB(255, 165, 0),
            Color3.fromRGB(255, 255, 0),
            Color3.fromRGB(0, 255, 0),
            Color3.fromRGB(0, 0, 255),
            Color3.fromRGB(75, 20, 140),
            Color3.fromRGB(255, 255, 255)
        }
        
        colorDisplay.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                colorIndex = colorIndex + 1
                if colorIndex > #colors then colorIndex = 1 end
                currentColor = colors[colorIndex]
                colorDisplay.BackgroundColor3 = currentColor
                callback(currentColor)
                ShowNotification("🎨 " .. title, "Cor alterada!", 1, MainGUI)
            end
        end)
        
        return picker
    end

    function CreateButton(data)
        local title = data.Name
        local callback = data.Callback or function() end
        
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(0, buttonWidth, 0, isMobile and 35 or 40)
        button.BackgroundColor3 = Theme.ElementBackground
        button.Text = title
        button.TextColor3 = Theme.TextColor
        button.TextSize = isMobile and 12 or 16
        button.Font = Enum.Font.GothamBold
        button.Parent = ContentArea
        
        local buttonCorner = Instance.new("UICorner")
        buttonCorner.CornerRadius = UDim.new(0, 8)
        buttonCorner.Parent = button
        
        local buttonStroke = Instance.new("UIStroke")
        buttonStroke.Color = Theme.Accent
        buttonStroke.Thickness = 1
        buttonStroke.Parent = button
        
        button.MouseButton1Click:Connect(function()
            callback()
            ShowNotification("✨ Ação", title .. " executado!", 1.5, MainGUI)
        end)
        
        button.MouseEnter:Connect(function()
            button.BackgroundColor3 = Theme.ElementBackgroundHover
        end)
        
        button.MouseLeave:Connect(function()
            button.BackgroundColor3 = Theme.ElementBackground
        end)
        
        return button
    end

    function CreateParagraph(data)
        local title = data.Title
        local content = data.Content
        
        local para = Instance.new("Frame")
        para.Size = UDim2.new(0, elementWidth, 0, isMobile and 100 or 90)
        para.BackgroundColor3 = Theme.ElementBackground
        para.BackgroundTransparency = 0.1
        para.Parent = ContentArea
        
        local paraCorner = Instance.new("UICorner")
        paraCorner.CornerRadius = UDim.new(0, 8)
        paraCorner.Parent = para
        
        local paraStroke = Instance.new("UIStroke")
        paraStroke.Color = Theme.ElementStroke
        paraStroke.Thickness = 1
        paraStroke.Transparency = 0.5
        paraStroke.Parent = para
        
        local titleLabel = Instance.new("TextLabel")
        titleLabel.Size = UDim2.new(1, -30, 0, 25)
        titleLabel.Position = UDim2.new(0, 15, 0, 10)
        titleLabel.BackgroundTransparency = 1
        titleLabel.Text = title
        titleLabel.TextColor3 = Theme.Accent
        titleLabel.TextSize = isMobile and 14 or 16
        titleLabel.TextXAlignment = Enum.TextXAlignment.Left
        titleLabel.Font = Enum.Font.GothamBold
        titleLabel.Parent = para
        
        local contentLabel = Instance.new("TextLabel")
        contentLabel.Size = UDim2.new(1, -30, 0, isMobile and 60 or 45)
        contentLabel.Position = UDim2.new(0, 15, 0, 35)
        contentLabel.BackgroundTransparency = 1
        contentLabel.Text = content
        contentLabel.TextColor3 = Theme.TextColor
        contentLabel.TextSize = isMobile and 10 or 13
        contentLabel.TextXAlignment = Enum.TextXAlignment.Left
        contentLabel.TextYAlignment = Enum.TextYAlignment.Top
        contentLabel.TextWrapped = true
        contentLabel.Font = Enum.Font.Gotham
        contentLabel.Parent = para
        
        return para
    end

    function CreateDivider()
        local divider = Instance.new("Frame")
        divider.Size = UDim2.new(0, elementWidth - 20, 0, 1)
        divider.Position = UDim2.new(0, 10, 0, 0)
        divider.BackgroundColor3 = Theme.ElementStroke
        divider.BackgroundTransparency = 0.7
        divider.Parent = ContentArea
    end

    -- ============================================
    -- CONTEÚDO DAS ABAS
    -- ============================================

    function UpdateTabContent(tabId)
        for _, child in pairs(ContentArea:GetChildren()) do
            if child:IsA("Frame") and child.Name ~= "UIListLayout" then
                child:Destroy()
            end
        end
        
        if tabId == "Player" then
            CreateSection("🛡️ PLAYER SETTINGS")
            
            CreateToggle({
                Name = "🚫 Noclip",
                CurrentValue = noclipEnabled,
                Callback = function(v)
                    noclipEnabled = v
                    if v then startNoclip() else stopNoclip() end
                end
            })
            
            CreateToggle({
                Name = "⚡ Speed",
                CurrentValue = speedEnabled,
                Callback = function(v)
                    speedEnabled = v
                    if v then startSpeed() else stopSpeed() end
                end
            })
            
            CreateSlider({
                Name = "📊 Velocidade Extra",
                Range = {0, 15},
                CurrentValue = speedValue,
                Suffix = " studs",
                Callback = function(v)
                    speedValue = v
                    if speedEnabled and LocalPlayer.Character then
                        local humanoid = LocalPlayer.Character:FindFirstChild("Humanoid")
                        if humanoid then humanoid.WalkSpeed = originalWalkSpeed + v end
                    end
                end
            })
            
        elseif tabId == "Combat" then
            CreateSection("🎯 AIMBOT SUPREMO")
            
            CreateToggle({
                Name = "🎯 Aimbot Supremo",
                CurrentValue = aimbotEnabled,
                Callback = function(v)
                    if v and aimbot4DEnabled then
                        aimbot4DEnabled = false
                        if aimbot4DConnection then aimbot4DConnection:Disconnect() end
                        if aimbot4DFovCircle then aimbot4DFovCircle:Destroy() end
                    end
                    aimbotEnabled = v
                    if v then
                        startAimbotWorking()
                        if fovVisible then createFovCircle() end
                    else
                        if aimbotConnection then aimbotConnection:Disconnect() end
                        if fovCircle then fovCircle:Destroy() end
                    end
                end
            })
            
            CreateToggle({
                Name = "👁️ Mostrar FOV",
                CurrentValue = fovVisible,
                Callback = function(v)
                    fovVisible = v
                    if aimbotEnabled then
                        if v then 
                            createFovCircle() 
                        elseif fovCircle then 
                            fovCircle:Destroy()
                            fovCircle = nil
                        end
                    end
                end
            })
            
            CreateColorPicker({
                Name = "🎨 Cor do FOV",
                Color = fovColor,
                Callback = function(v)
                    fovColor = v
                    if fovCircle then fovCircle.Color = v end
                end
            })
            
            CreateSlider({
                Name = "📏 Tamanho do FOV",
                Range = {10, 500},
                CurrentValue = aimbotFov,
                Suffix = "px",
                Callback = function(v)
                    aimbotFov = v
                    if fovCircle then fovCircle.Radius = v end
                end
            })
            
            CreateSlider({
                Name = "💪 Força do Aimbot",
                Range = {1, 10},
                CurrentValue = aimbotStrength,
                Suffix = "/10",
                Callback = function(v) aimbotStrength = v end
            })
            
            CreateDivider()
            
            CreateSection("🌀 AIMBOT 4D")
            
            CreateToggle({
                Name = "🌀 Aimbot 4D",
                CurrentValue = aimbot4DEnabled,
                Callback = function(v)
                    if v and aimbotEnabled then
                        aimbotEnabled = false
                        if aimbotConnection then aimbotConnection:Disconnect() end
                        if fovCircle then 
                            fovCircle:Destroy()
                            fovCircle = nil
                        end
                    end
                    aimbot4DEnabled = v
                    if v then
                        startAimbot4D()
                        if aimbot4DFovVisible then createAimbot4DFovCircle() end
                    else
                        if aimbot4DConnection then aimbot4DConnection:Disconnect() end
                        if aimbot4DFovCircle then 
                            aimbot4DFovCircle:Destroy()
                            aimbot4DFovCircle = nil
                        end
                    end
                end
            })
            
            CreateToggle({
                Name = "👁️ Mostrar FOV 4D",
                CurrentValue = aimbot4DFovVisible,
                Callback = function(v)
                    aimbot4DFovVisible = v
                    if aimbot4DEnabled then
                        if v then 
                            createAimbot4DFovCircle() 
                        elseif aimbot4DFovCircle then 
                            aimbot4DFovCircle:Destroy()
                            aimbot4DFovCircle = nil
                        end
                    end
                end
            })
            
            CreateColorPicker({
                Name = "🎨 Cor do FOV 4D",
                Color = aimbot4DColor,
                Callback = function(v)
                    aimbot4DColor = v
                    if aimbot4DFovCircle then aimbot4DFovCircle.Color = v end
                end
            })
            
            CreateSlider({
                Name = "📏 Tamanho do FOV 4D",
                Range = {10, 500},
                CurrentValue = aimbot4DFov,
                Suffix = "px",
                Callback = function(v)
                    aimbot4DFov = v
                    if aimbot4DFovCircle then aimbot4DFovCircle.Radius = v end
                end
            })
            
            CreateDivider()
            
            CreateSection("🛡️ ANTI-TEAM")
            
            CreateToggle({
                Name = "🛡️ Anti-Team",
                CurrentValue = antiTeamEnabled,
                Callback = function(v)
                    antiTeamEnabled = v
                    if v then myTeam = getPlayerTeam(LocalPlayer) end
                end
            })
            
        elseif tabId == "Visual" then
            CreateSection("👁️ ESP")
            
            CreateToggle({
                Name = "👁️ ESP Linhas",
                CurrentValue = espEnabled,
                Callback = function(v)
                    espEnabled = v
                    if v then startESP() else stopESP() end
                end
            })
            
            CreateColorPicker({
                Name = "🎨 Cor das Linhas",
                Color = espLineColor,
                Callback = function(v)
                    espLineColor = v
                    for _, line in pairs(espLines) do
                        if line then line.Color = v end
                    end
                end
            })
            
            CreateDivider()
            
            CreateSection("👮 ESP STAFF")
            
            CreateToggle({
                Name = "👮 ESP Staff",
                CurrentValue = staffEspEnabled,
                Callback = function(v)
                    staffEspEnabled = v
                    if v then startStaffESP() else stopStaffESP() end
                end
            })
            
            CreateDivider()
            
            CreateSection("📦 CAIXINHA 2D")
            
            CreateToggle({
                Name = "📦 Caixinha 2D",
                CurrentValue = box2DEnabled,
                Callback = function(v)
                    box2DEnabled = v
                    
                    if v then
                        for player, objects in pairs(box2DObjects) do
                            for _, obj in ipairs(objects) do
                                if obj then obj:Destroy() end
                            end
                        end
                        box2DObjects = {}
                        
                        for _, player in ipairs(Players:GetPlayers()) do
                            if player ~= LocalPlayer then
                                createBox2D(player)
                            end
                        end
                        
                        table.insert(box2DConnections, {
                            type = "playerAdded",
                            connection = Players.PlayerAdded:Connect(function(newPlayer)
                                task.wait(0.1)
                                if box2DEnabled then createBox2D(newPlayer) end
                            end)
                        })
                        
                        table.insert(box2DConnections, {
                            type = "playerRemoving",
                            connection = Players.PlayerRemoving:Connect(function(leavingPlayer)
                                if box2DObjects[leavingPlayer] then
                                    for _, obj in ipairs(box2DObjects[leavingPlayer]) do
                                        if obj then obj:Destroy() end
                                    end
                                    box2DObjects[leavingPlayer] = nil
                                end
                            end)
                        })
                    else
                        for player, objects in pairs(box2DObjects) do
                            for _, obj in ipairs(objects) do
                                if obj then obj:Destroy() end
                            end
                        end
                        box2DObjects = {}
                        
                        for _, data in ipairs(box2DConnections) do
                            if data.connection then data.connection:Disconnect() end
                        end
                        box2DConnections = {}
                    end
                end
            })
            
            CreateColorPicker({
                Name = "🎨 Cor da Caixinha",
                Color = box2DColor,
                Callback = function(v)
                    box2DColor = v
                    if box2DEnabled then
                        for player, objects in pairs(box2DObjects) do
                            if not isStaff(player) then
                                for i = 1, 4 do
                                    if objects[i] then objects[i].Color = v end
                                end
                                if objects[5] then objects[5].Color = v end
                            end
                        end
                    end
                end
            })
            
            CreateToggle({
                Name = "📝 Mostrar Nome",
                CurrentValue = box2DShowName,
                Callback = function(v)
                    box2DShowName = v
                    for player, objects in pairs(box2DObjects) do
                        if objects[5] then objects[5].Visible = v end
                    end
                end
            })
            
            CreateToggle({
                Name = "📏 Mostrar Distância",
                CurrentValue = box2DShowDistance,
                Callback = function(v)
                    box2DShowDistance = v
                    for player, objects in pairs(box2DObjects) do
                        if objects[6] then objects[6].Visible = v end
                    end
                end
            })
            
        elseif tabId == "Config" then
            CreateSection("+++ CONFIGURAÇÕES AVANÇADAS")
            
            CreateToggle({
                Name = "🎯 AimGod (Ativar tecla para mirar)",
                CurrentValue = aimGodEnabled,
                Callback = function(v)
                    aimGodEnabled = v
                    if not v then
                        aimGodKey = nil
                        aimGodKeyText = "Nenhuma"
                    end
                end
            })
            
            CreateKeybind({
                Name = "🔑 Tecla do AimGod",
                Callback = function(key) end
            })
            
            CreateDivider()
            
            CreateToggle({
                Name = "🔥 Cabeça Quente",
                CurrentValue = cabecaQuenteEnabled,
                Callback = function(v)
                    cabecaQuenteEnabled = v
                    if v then 
                        startCabecaQuente()
                        ShowNotification("🔥 Cabeça Quente", "Ativado! Tamanho: " .. cabecaQuenteSize, 2, MainGUI)
                    else 
                        stopCabecaQuente()
                    end
                end
            })
            
            CreateSlider({
                Name = "📏 Tamanho da Cabeça",
                Range = {1, 3},
                CurrentValue = cabecaQuenteSize,
                Suffix = "x",
                Callback = function(v)
                    cabecaQuenteSize = v
                    if cabecaQuenteEnabled then
                        startCabecaQuente()
                    end
                end
            })
            
            CreateDivider()
            
            CreateToggle({
                Name = "👻 Anti-Invisível",
                CurrentValue = antiInvisivelEnabled,
                Callback = function(v)
                    antiInvisivelEnabled = v
                    ShowNotification("👻 Anti-Invisível", v and "Ativado! Não mira em players invisíveis" or "Desativado!", 2, MainGUI)
                end
            })
            
            CreateParagraph({
                Title = "ℹ️ Sobre o Anti-Invisível",
                Content = "Quando ativado, o aimbot não irá mirar em jogadores que estão invisíveis ou com transparência alta."
            })
            
        elseif tabId == "Credits" then
            CreateSection("✨ LUNARIS HUB")
            
            CreateParagraph({
                Title = "Lunaris Hub v" .. Lunaris.Version .. (isPremium and " [PREMIUM]" or ""),
                Content = "Desenvolvido por " .. Lunaris.Developer .. "\n\n" ..
                          "✓ Speed: 0-15 studs\n" ..
                          "✓ Aimbot Supremo + AimGod\n" ..
                          "✓ Aimbot 4D\n" ..
                          "✓ Cabeça Quente (Anti-ban)\n" ..
                          "✓ Anti-Invisível\n" ..
                          "✓ Caixinha 2D + RGB Staff\n" ..
                          "✓ ESP + ESP Staff RGB\n" ..
                          "✓ Anti-Team\n" ..
                          "✓ FOV Ajustável\n" ..
                          "✓ Color Pickers\n" ..
                          "✓ Noclip"
            })
            
            CreateParagraph({
                Title = "💜 Discord",
                Content = Lunaris.Discord
            })
            
            if isPremium then
                CreateParagraph({
                    Title = "🌟 KEY PREMIUM ATIVA",
                    Content = "Sua key premium está salva no DataStore! Você não precisará digitar novamente neste jogo."
                })
            end
        end
        
        ContentArea.CanvasSize = UDim2.new(0, 0, 0, ContentList.AbsoluteContentSize.Y + 20)
    end

    -- ============================================
    -- BOTÃO DE MINIMIZAR
    -- ============================================

    MinimizeBtn.MouseButton1Click:Connect(function()
        MainContainer.Visible = false
        
        local moonBtn = Instance.new("TextButton")
        moonBtn.Name = "MoonButton"
        moonBtn.Size = UDim2.new(0, isMobile and 50 or 60, 0, isMobile and 50 or 60)
        moonBtn.Position = UDim2.new(0, isMobile and 10 or 20, 0, isMobile and 10 or 20)
        moonBtn.BackgroundTransparency = 1
        moonBtn.Text = "⦿"
        moonBtn.TextColor3 = Theme.Accent
        moonBtn.TextSize = isMobile and 40 or 50
        moonBtn.Font = Enum.Font.GothamBold
        moonBtn.BorderSizePixel = 0
        moonBtn.Parent = MainGUI
        
        local moonDragging = false
        local moonDragStart, moonStartPos
        
        moonBtn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                moonDragging = true
                moonDragStart = input.Position
                moonStartPos = moonBtn.Position
            end
        end)
        
        moonBtn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                moonDragging = false
            end
        end)
        
        UserInputService.InputChanged:Connect(function(input)
            if moonDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                local delta = input.Position - moonDragStart
                moonBtn.Position = UDim2.new(
                    moonStartPos.X.Scale,
                    moonStartPos.X.Offset + delta.X,
                    moonStartPos.Y.Scale,
                    moonStartPos.Y.Offset + delta.Y
                )
            end
        end)
        
        moonBtn.MouseButton1Click:Connect(function()
            MainContainer.Visible = true
            moonBtn:Destroy()
        end)
    end)

    CloseBtn.MouseButton1Click:Connect(function()
        MainGUI:Destroy()
        if noclipConnection then noclipConnection:Disconnect() end
        if speedConnection then speedConnection:Disconnect() end
        if aimbotConnection then aimbotConnection:Disconnect() end
        if aimbot4DConnection then aimbot4DConnection:Disconnect() end
        if fovCircle then fovCircle:Destroy() end
        if aimbot4DFovCircle then aimbot4DFovCircle:Destroy() end
        stopESP()
        stopStaffESP()
        stopCabecaQuente()
        
        for player, objects in pairs(box2DObjects) do
            for _, obj in ipairs(objects) do
                if obj then obj:Destroy() end
            end
        end
    end)

    UserInputService.InputBegan:Connect(function(input, gp)
        if not gp and input.KeyCode == Lunaris.Keybind then
            MainContainer.Visible = not MainContainer.Visible
        end
    end)

    -- INICIALIZAÇÃO
    UpdateTabContent("Player")

    spawn(function()
        while true do
            task.wait(5)
            if antiTeamEnabled then
                myTeam = getPlayerTeam(LocalPlayer)
            end
        end
    end)

    task.wait(0.5)
    ShowNotification("✨ Lunaris Hub", "Menu carregado! Pressione K", 3, MainGUI)

    print("✨ Lunaris Hub v" .. Lunaris.Version .. " carregado com sucesso!")
end
