-- Full working GUI script with 3 toggleable frames and 1 master button
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StyleAndFlowSelectorGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui

-- Function to create selector frames (style or flow)
local function createSelector(title, itemList, statPath, position)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 250, 0, 320)
    frame.Position = position
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    frame.BorderSizePixel = 0
    frame.Visible = false
    frame.Parent = screenGui

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 20)
    titleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 14
    titleLabel.Text = title
    titleLabel.Parent = frame

    local scroll = Instance.new("ScrollingFrame")
    scroll.Size = UDim2.new(1, 0, 1, -20)
    scroll.Position = UDim2.new(0, 0, 0, 20)
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.ScrollBarThickness = 6
    scroll.BackgroundTransparency = 1
    scroll.Parent = frame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 5)
    layout.Parent = scroll

    for _, name in ipairs(itemList) do
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(1, -10, 0, 30)
        button.Position = UDim2.new(0, 5, 0, 0)
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        button.TextColor3 = Color3.new(1, 1, 1)
        button.Font = Enum.Font.Gotham
        button.TextSize = 14
        button.Text = name
        button.Parent = scroll

        button.MouseButton1Click:Connect(function()
            local stats = player:FindFirstChild("PlayerStats")
            if stats and stats:FindFirstChild(statPath) then
                stats[statPath].Value = name
            end
        end)
    end

    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    end)

    return frame
end

-- Function to create addon panel with custom buttons
local function createAddonFrame(position)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 250, 0, 150)
    frame.Position = position
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    frame.BorderSizePixel = 0
    frame.Visible = false
    frame.Parent = screenGui

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 20)
    titleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    titleLabel.TextColor3 = Color3.new(1, 1, 1)
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 14
    titleLabel.Text = "NEL Reo Power"
    titleLabel.Parent = frame

    local function createButton(text, onClick)
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, -10, 0, 30)
        btn.Position = UDim2.new(0, 5, 0, 0)
        btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.Font = Enum.Font.Gotham
        btn.TextSize = 14
        btn.Text = text
        btn.Parent = frame
        btn.MouseButton1Click:Connect(onClick)
        return btn
    end

    local yOffset = 25

    local b1 = createButton("No Cooldown", function()
        local C = require(game:GetService("ReplicatedStorage").Controllers.AbilityController)
        local o = C.AbilityCooldown
        C.AbilityCooldown = function(s, n, ...) return o(s, n, 0, ...) end
    end)
    b1.Position = UDim2.new(0, 5, 0, yOffset)

    local flowToggle = false
    local b2 = createButton("Inf Flow", function()
        flowToggle = not flowToggle
        player.PlayerStats.InFlow.Value = flowToggle
    end)
    b2.Position = UDim2.new(0, 5, 0, yOffset + 35)

    local awakenToggle = false
    local b3 = createButton("Inf Awakening", function()
        local style = player.PlayerStats.Style.Value
        if style == "Kaiser" or style == "Kurona" or style == "King" then
            awakenToggle = not awakenToggle
            player.PlayerStats.InAwakening.Value = awakenToggle
        end
    end)
    b3.Position = UDim2.new(0, 5, 0, yOffset + 70)

    return frame
end

-- Data
local styleNames = {
    "Isagi", "Chigiri", "Kurona", "Igaguri", "Hiori", "Gagamuru", "Otoya",
    "Bachira", "Nagi", "Reo", "Karasu", "Shidou", "Yukimiya", "NEL Bachira",
    "King", "Kunigami", "Aiku", "Don Lorenzo", "Sae", "Kaiser", "NEL Rin"
}

local flowNames = {
    "Puzzle", "Ice", "Lightning", "Gale Burst", "Genius", "Buddha's Blessing", "Monster",
    "King's Instinct", "Crow", "Chameleon", "Demon Wings", "Wild Card", "Trap", "Snake",
    "Dribbler", "Bee Freestyle", "Awakened Genius", "Emperor", "Soul Harvester", "Destructive Impulses"
}

-- Frames
local styleFrame = createSelector("Style Changer", styleNames, "Style", UDim2.new(0, 10, 0.2, 0))
local flowFrame = createSelector("Flow Changer", flowNames, "Flow", UDim2.new(0, 270, 0.2, 0))
local addonFrame = createAddonFrame(UDim2.new(0, 530, 0.2, 0))

-- Main Toggle Button
local mainToggle = Instance.new("TextButton")
mainToggle.Size = UDim2.new(0, 120, 0, 30)
mainToggle.Position = UDim2.new(0, 10, 0.1, 0)
mainToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
mainToggle.TextColor3 = Color3.new(1, 1, 1)
mainToggle.Font = Enum.Font.GothamBold
mainToggle.TextSize = 14
mainToggle.Text = "Open Menu"
mainToggle.Parent = screenGui
mainToggle.Active = true
mainToggle.Draggable = true

local menuOpen = false
mainToggle.MouseButton1Click:Connect(function()
    menuOpen = not menuOpen
    styleFrame.Visible = menuOpen
    flowFrame.Visible = menuOpen
    addonFrame.Visible = menuOpen
    mainToggle.Text = menuOpen and "Close Menu" or "Open Menu"
end)

