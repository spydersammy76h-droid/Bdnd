--[[
    STEAL AN EGG - MENU STYLE
    Para Roblox Studio
    Coloque em:
    StarterPlayer > StarterPlayerScripts
]]

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

--------------------------------------------------
-- CONFIGURAÇÕES
--------------------------------------------------

local UI = {
    Background = Color3.fromRGB(8, 14, 28),
    Panel = Color3.fromRGB(13, 23, 42),
    Panel2 = Color3.fromRGB(18, 31, 55),
    Blue = Color3.fromRGB(0, 190, 255),
    Text = Color3.fromRGB(240, 245, 255),
    SubText = Color3.fromRGB(145, 160, 185),
    Off = Color3.fromRGB(80, 90, 105),
}

--------------------------------------------------
-- GUI
--------------------------------------------------

local gui = Instance.new("ScreenGui")
gui.Name = "StealAnEggMenu"
gui.ResetOnSpawn = false
gui.Parent = playerGui

--------------------------------------------------
-- FUNÇÕES AUXILIARES
--------------------------------------------------

local function corner(object, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius)
    c.Parent = object
end

local function stroke(object)
    local s = Instance.new("UIStroke")
    s.Color = Color3.fromRGB(40, 70, 110)
    s.Thickness = 1
    s.Transparency = 0.35
    s.Parent = object
end

local function tween(object, time, properties)
    TweenService:Create(
        object,
        TweenInfo.new(time, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
        properties
    ):Play()
end

--------------------------------------------------
-- JANELA PRINCIPAL
--------------------------------------------------

local main = Instance.new("Frame")
main.Name = "MainWindow"
main.Size = UDim2.new(0, 720, 0, 460)
main.Position = UDim2.new(0.5, -360, 0.5, -230)
main.BackgroundColor3 = UI.Background
main.Parent = gui

corner(main, 14)
stroke(main)

--------------------------------------------------
-- TOPO
--------------------------------------------------

local top = Instance.new("Frame")
top.Size = UDim2.new(1, 0, 0, 58)
top.BackgroundColor3 = UI.Panel
top.Parent = main

corner(top, 14)

local logo = Instance.new("TextLabel")
logo.Size = UDim2.new(0, 90, 1, 0)
logo.Position = UDim2.new(0, 18, 0, 0)
logo.BackgroundTransparency = 1
logo.Text = "ZN"
logo.TextColor3 = UI.Blue
logo.Font = Enum.Font.GothamBold
logo.TextSize = 25
logo.TextXAlignment = Enum.TextXAlignment.Left
logo.Parent = top

local fps = Instance.new("TextLabel")
fps.Size = UDim2.new(0, 90, 0, 30)
fps.Position = UDim2.new(1, -180, 0, 14)
fps.BackgroundTransparency = 1
fps.Text = "● 60 FPS"
fps.TextColor3 = UI.SubText
fps.TextSize = 13
fps.Font = Enum.Font.Gotham
fps.Parent = top

local delta = Instance.new("TextLabel")
delta.Size = UDim2.new(0, 65, 0, 30)
delta.Position = UDim2.new(1, -95, 0, 14)
delta.BackgroundColor3 = Color3.fromRGB(25, 35, 60)
delta.Text = "Studio"
delta.TextColor3 = UI.Text
delta.TextSize = 12
delta.Font = Enum.Font.GothamBold
delta.Parent = top
corner(delta, 7)

--------------------------------------------------
-- BOTÃO FECHAR
--------------------------------------------------

local close = Instance.new("TextButton")
close.Size = UDim2.new(0, 40, 0, 40)
close.Position = UDim2.new(1, -45, 0, 9)
close.BackgroundTransparency = 1
close.Text = "×"
close.TextColor3 = UI.Text
close.TextSize = 28
close.Font = Enum.Font.Gotham
close.Parent = top

close.MouseButton1Click:Connect(function()
    tween(main, 0.3, {
        Size = UDim2.new(0, 0, 0, 0)
    })

    task.wait(0.35)
    gui:Destroy()
end)

--------------------------------------------------
-- SIDEBAR
--------------------------------------------------

local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0, 190, 1, -58)
sidebar.Position = UDim2.new(0, 0, 0, 58)
sidebar.BackgroundColor3 = Color3.fromRGB(10, 18, 33)
sidebar.Parent = main

local search = Instance.new("TextBox")
search.Size = UDim2.new(1, -24, 0, 38)
search.Position = UDim2.new(0, 12, 0, 15)
search.BackgroundColor3 = UI.Panel2
search.PlaceholderText = "⌕  Search..."
search.PlaceholderColor3 = UI.SubText
search.Text = ""
search.TextColor3 = UI.Text
search.TextSize = 13
search.Font = Enum.Font.Gotham
search.Parent = sidebar

corner(search, 8)

--------------------------------------------------
-- ÁREA DAS PÁGINAS
--------------------------------------------------

local pages = {}

local content = Instance.new("Frame")
content.Size = UDim2.new(1, -205, 1, -75)
content.Position = UDim2.new(0, 200, 0, 68)
content.BackgroundTransparency = 1
content.Parent = main

local function createPage(name)
    local page = Instance.new("ScrollingFrame")
    page.Name = name
    page.Size = UDim2.fromScale(1, 1)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = 3
    page.CanvasSize = UDim2.new(0, 0, 0, 600)
    page.Visible = false
    page.Parent = content

    pages[name] = page

    return page
end

local infoPage = createPage("Info")
local mainPage = createPage("Main")
local eventPage = createPage("Event")
local autoPage = createPage("Auto")
local webhookPage = createPage("Webhook")
local playerPage = createPage("Player")
local discordPage = createPage("Discord")

--------------------------------------------------
-- TÍTULO DA PÁGINA
--------------------------------------------------

local function pageTitle(page, title, subtitle)

    local t = Instance.new("TextLabel")
    t.Size = UDim2.new(1, -10, 0, 40)
    t.BackgroundTransparency = 1
    t.Text = title
    t.TextColor3 = UI.Text
    t.TextSize = 23
    t.Font = Enum.Font.GothamBold
    t.TextXAlignment = Enum.TextXAlignment.Left
    t.Parent = page

    local st = Instance.new("TextLabel")
    st.Size = UDim2.new(1, -10, 0, 25)
    st.Position = UDim2.new(0, 0, 0, 40)
    st.BackgroundTransparency = 1
    st.Text = subtitle or ""
    st.TextColor3 = UI.SubText
    st.TextSize = 12
    st.Font = Enum.Font.Gotham
    st.TextXAlignment = Enum.TextXAlignment.Left
    st.Parent = page
end

pageTitle(infoPage, "Info", "Informações do menu")
pageTitle(mainPage, "Auto Farm", "Automação disponível no seu próprio jogo")
pageTitle(eventPage, "Event", "Eventos")
pageTitle(autoPage, "Auto", "Opções automáticas")
pageTitle(webhookPage, "Webhook", "Configurações de webhook")
pageTitle(playerPage, "Player", "Configurações do jogador")
pageTitle(discordPage, "Discord", "Comunidade")

--------------------------------------------------
-- SWITCH
--------------------------------------------------

local function createToggle(page, text, y, callback)

    local row = Instance.new("Frame")
    row.Size = UDim2.new(1, -15, 0, 48)
    row.Position = UDim2.new(0, 0, 0, y)
    row.BackgroundTransparency = 1
    row.Parent = page

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -75, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = UI.Text
    label.TextSize = 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = row

    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.new(0, 42, 0, 23)
    toggle.Position = UDim2.new(1, -48, 0.5, -11)
    toggle.BackgroundColor3 = UI.Off
    toggle.Text = ""
    toggle.Parent = row

    corner(toggle, 15)

    local circle = Instance.new("Frame")
    circle.Size = UDim2.new(0, 17, 0, 17)
    circle.Position = UDim2.new(0, 3, 0.5, -8)
    circle.BackgroundColor3 = Color3.new(1,1,1)
    circle.Parent = toggle

    corner(circle, 20)

    local enabled = false

    toggle.MouseButton1Click:Connect(function()

        enabled = not enabled

        if enabled then
            tween(toggle, .2, {
                BackgroundColor3 = UI.Blue
            })

            tween(circle, .2, {
                Position = UDim2.new(1, -20, 0.5, -8)
            })
        else
            tween(toggle, .2, {
                BackgroundColor3 = UI.Off
            })

            tween(circle, .2, {
                Position = UDim2.new(0, 3, 0.5, -8)
            })
        end

        if callback then
            callback(enabled)
        end
    end)
end

--------------------------------------------------
-- INPUT
--------------------------------------------------

local function createInput(page, text, y, placeholder)

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 25)
    label.Position = UDim2.new(0, 0, 0, y)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = UI.SubText
    label.TextSize = 12
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = page

    local input = Instance.new("TextBox")
    input.Size = UDim2.new(1, -15, 0, 43)
    input.Position = UDim2.new(0, 0, 0, y + 27)
    input.BackgroundColor3 = UI.Panel2
    input.Text = ""
    input.PlaceholderText = placeholder
    input.PlaceholderColor3 = UI.SubText
    input.TextColor3 = UI.Text
    input.TextSize = 14
    input.Font = Enum.Font.Gotham
    input.Parent = page

    corner(input, 8)

    return input
end

--------------------------------------------------
-- PÁGINA MAIN
--------------------------------------------------

createToggle(mainPage, "Auto Steal", 80, function(state)
    print("Auto Steal:", state)
end)

createToggle(mainPage, "Auto Treadmill", 135, function(state)
    print("Auto Treadmill:", state)
end)

createToggle(mainPage, "Anti Knock Back", 190, function(state)
    print("Anti Knock Back:", state)
end)

createToggle(mainPage, "Anti Treadmill", 245, function(state)
    print("Anti Treadmill:", state)
end)

local speed = createInput(
    mainPage,
    "Speed level 1 - 100",
    310,
    "40"
)

speed.FocusLost:Connect(function()
    local value = tonumber(speed.Text)

    if value then
        value = math.clamp(value, 1, 100)
        print("Velocidade:", value)
    end
end)

--------------------------------------------------
-- PÁGINA EVENT
--------------------------------------------------

createToggle(eventPage, "Evento ativo", 80, function(state)
    print("Evento:", state)
end)

createToggle(eventPage, "Mostrar eventos", 135, function(state)
    print("Mostrar eventos:", state)
end)

--------------------------------------------------
-- PÁGINA AUTO
--------------------------------------------------

createToggle(autoPage, "Auto coletar", 80, function(state)
    print("Auto coletar:", state)
end)

createToggle(autoPage, "Auto abrir ovos", 135, function(state)
    print("Auto abrir ovos:", state)
end)

createToggle(autoPage, "Auto vender", 190, function(state)
    print("Auto vender:", state)
end)

--------------------------------------------------
-- PÁGINA WEBHOOK
--------------------------------------------------

local webhook = createInput(
    webhookPage,
    "Webhook URL",
    80,
    "Cole sua URL aqui"
)

createToggle(webhookPage, "Ativar webhook", 155, function(state)
    print("Webhook:", state)
end)

--------------------------------------------------
-- PÁGINA PLAYER
--------------------------------------------------

createToggle(playerPage, "Mostrar informações", 80, function(state)
    print("Informações:", state)
end)

createToggle(playerPage, "Modo rápido", 135, function(state)
    print("Modo rápido:", state)
end)

--------------------------------------------------
-- PÁGINA DISCORD
--------------------------------------------------

pageTitle(discordPage, "Discord", "Comunidade do jogo")

local discordText = Instance.new("TextLabel")
discordText.Size = UDim2.new(1, -20, 0, 100)
discordText.Position = UDim2.new(0, 0, 0, 80)
discordText.BackgroundTransparency = 1
discordText.Text = "Entre na comunidade para receber novidades e eventos."
discordText.TextColor3 = UI.SubText
discordText.TextSize = 14
discordText.Font = Enum.Font.Gotham
discordText.TextWrapped = true
discordText.TextXAlignment = Enum.TextXAlignment.Left
discordText.Parent = discordPage

--------------------------------------------------
-- BOTÕES DA SIDEBAR
--------------------------------------------------

local tabs = {
    {"ⓘ", "Info", infoPage},
    {"⌂", "Main", mainPage},
    {"✦", "Event", eventPage},
    {"↻", "Auto", autoPage},
    {"♧", "Webhook", webhookPage},
    {"●", "Player", playerPage},
    {"●", "Discord", discordPage}
}

local selectedButton

for i, data in ipairs(tabs) do

    local icon = data[1]
    local name = data[2]
    local page = data[3]

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -18, 0, 42)
    button.Position = UDim2.new(0, 9, 0, 65 + ((i-1) * 46))
    button.BackgroundColor3 = Color3.fromRGB(17, 28, 48)
    button.Text = "  " .. icon .. "   " .. name
    button.TextColor3 = UI.SubText
    button.TextSize = 14
    button.Font = Enum.Font.Gotham
    button.TextXAlignment = Enum.TextXAlignment.Left
    button.Parent = sidebar

    corner(button, 7)

    button.MouseButton1Click:Connect(function()

        for _, p in pairs(pages) do
            p.Visible = false
        end

        page.Visible = true

        if selectedButton then
            selectedButton.BackgroundColor3 =
                Color3.fromRGB(17, 28, 48)

            selectedButton.TextColor3 = UI.SubText
        end

        selectedButton = button

        button.BackgroundColor3 =
            Color3.fromRGB(0, 130, 190)

        button.TextColor3 = Color3.new(1,1,1)

        page.Position = UDim2.new(0, 220, 0, 0)

        tween(page, .2, {
            Position = UDim2.new(0, 0, 0, 0)
        })
    end)
end

--------------------------------------------------
-- PÁGINA INICIAL
--------------------------------------------------

infoPage.Visible = true

selectedButton = nil

--------------------------------------------------
-- ANIMAÇÃO INICIAL
--------------------------------------------------

main.Size = UDim2.new(0, 0, 0, 0)

tween(main, .45, {
    Size = UDim2.new(0, 720, 0, 460)
})

print("Steal An Egg Menu carregado!")# Bdnd
Hrhr
