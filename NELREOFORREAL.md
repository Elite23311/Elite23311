-- Full GUI Script with 3 Frames + One Master Toggle
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StyleAndFlowSelectorGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui

-- Function to create a Selector Frame (Style or Flow)
local function createSelector(toggleText, frameTitle, stylesList, statPath, togglePos)
    local toggleButton = Instance.new("TextButton")
    toggleButton.Size = UDim2.new(0, 120, 0, 30)
    toggleButton.Position = togglePos
    toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    toggleButton.TextColor3 = Color3.new(1, 1, 1)
    toggleButton.Font = Enum.Font.GothamBold
    toggleButton.TextSize = 14
    toggleButton.Text = "Open " .. toggleText
    toggleButton.Draggable = true
    toggleButton.Active = true
    toggleButton.Parent = screenGui

    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, 250, 0, 320)
    mainFrame.Position = UDim2.new(0, togglePos.X.Offset, 0, togglePos.Y.Offset + 40)
    mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    mainFrame.BorderSizePixel = 0
    mainFrame.Visible = false
    mainFrame.Parent = screenGui

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 20)
    titleLabel.Position = UDim2.new(0, 0, 0, 0)
    titleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 14
    titleLabel.Text = frameTitle
    titleLabel.Parent = mainFrame

    local scroll = Instance.new("ScrollingFrame")
    scroll.Size = UDim2.new(1, 0, 1, -20)
    scroll.Position = UDim2.new(0, 0, 0, 20)
    scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    scroll.ScrollBarThickness = 6
    scroll.BackgroundTransparency = 1
    scroll.Parent = mainFrame

    local layout = Instance.new("UIListLayout")
    layout.Parent = scroll
    layout.Padding = UDim.new(0, 5)

    for _, name in ipairs(stylesList) do
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(1, -10, 0, 30)
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        button.TextColor3 = Color3.new(1, 1, 1)
        button.Font = Enum.Font.Gotham
        button.TextSize = 14
        button.Text = name
        button.Parent = scroll

        button.MouseButton1Click:Connect(function()
            local statObj = player:FindFirstChild("PlayerStats")
            if statObj and statObj:FindFirstChild(statPath) then
                statObj[statPath].Value = name
            end
        end)
    end

    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    end)

    toggleButton.MouseButton1Click:Connect(function()
        mainFrame.Visible = not mainFrame.Visible
        toggleButton.Text = mainFrame.Visible and ("Close " .. toggleText) or ("Open " .. toggleText)
    end)

    return mainFrame
end

-- Function to create Addons Frame
local function createAddonsFrame()
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 250, 0, 140)
    frame.Position = UDim2.new(0.42, 0, 0.2, 0)
    frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    frame.BorderSizePixel = 0
    frame.Visible = false
    frame.Parent = screenGui

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 20)
    title.Position = UDim2.new(0, 0, 0, 0)
    title.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 14
    title.Text = "Addons"
    title.Parent = frame

    local layout = Instance.new("UIListLayout")
    layout.Parent = frame
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 5)

    local buttons = {
        {Text = "No Cooldown", Callback = function()
            local C = require(game:GetService("ReplicatedStorage").Controllers.AbilityController)
            local o = C.AbilityCooldown
            C.AbilityCooldown = function(s, n, ...) return o(s, n, 0, ...) end
        end},

        {Text = "Inf Flow", Callback = function()
            local flow = player.PlayerStats:FindFirstChild("InFlow")
            if flow then flow.Value = not flow.Value end
        end},

        {Text = "Inf Awakening", Callback = function()
            local style = player.PlayerStats and player.PlayerStats.Style and player.PlayerStats.Style.Value
            if style == "Kaiser" or style == "Kurona" or style == "King" then
                local awakening = player.PlayerStats:FindFirstChild("InAwakening")
                if awakening then awakening.Value = not awakening.Value end
            end
        end},
    }

    for _, data in ipairs(buttons) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, -10, 0, 30)
        btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.Font = Enum.Font.Gotham
        btn.TextSize = 14
        btn.Text = data.Text
        btn.Parent = frame

        btn.MouseButton1Click:Connect(data.Callback)
    end

    return frame
end

-- Style and Flow Lists
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

-- Create the UI Sections
local styleFrame = createSelector("Styles", "Style Changer", styleNames, "Style", UDim2.new(0.02, 0, 0.15, 0))
local flowFrame = createSelector("Flows", "Flow Changer", flowNames, "Flow", UDim2.new(0.22, 0, 0.15, 0))
local addonsFrame = createAddonsFrame()

-- Master Toggle Button
local masterToggle = Instance.new("TextButton")
masterToggle.Size = UDim2.new(0, 140, 0, 30)
masterToggle.Position = UDim2.new(0.7, 0, 0.15, 0)
masterToggle.BackgroundColor3 = Color3.fromRGB(80, 60, 120)
masterToggle.TextColor3 = Color3.new(1, 1, 1)
masterToggle.Font = Enum.Font.GothamBold
masterToggle.TextSize = 14
masterToggle.Text = "Toggle All Panels"
masterToggle.Parent = screenGui

local allFrames = {styleFrame, flowFrame, addonsFrame}
local masterState = false
masterToggle.MouseButton1Click:Connect(function()
    masterState = not masterState
    for _, frame in ipairs(allFrames) do
        frame.Visible = masterState
    end
end)

end

