local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StyleAndFlowSelectorGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = PlayerGui

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
            if statObj and statObj:FindFirstChild(statPath) and statObj[statPath]:IsA("StringValue") then
                statObj[statPath].Value = name
                print(statPath .. " set to:", name)
            else
                warn("PlayerStats." .. statPath .. " not found or invalid.")
            end
        end)
    end

    task.wait()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y)
    end)
    
    toggleButton.MouseButton1Click:Connect(function()
        mainFrame.Visible = not mainFrame.Visible
        toggleButton.Text = mainFrame.Visible and ("Close " .. toggleText) or ("Open " .. toggleText)
    end)
end

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

createSelector("Styles", "Style Changer", styleNames, "Style", UDim2.new(0.02, 0, 0.15, 0))
createSelector("Flows", "Flow Changer", flowNames, "Flow", UDim2.new(0.22, 0, 0.15, 0))

local function createAddonsFrame()
    local toggleButton = Instance.new("TextButton")
    toggleButton.Size = UDim2.new(0, 120, 0, 30)
    toggleButton.Position = UDim2.new(0.42, 0, 0.15, 0)
    toggleButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    toggleButton.TextColor3 = Color3.new(1, 1, 1)
    toggleButton.Font = Enum.Font.GothamBold
    toggleButton.TextSize = 14
    toggleButton.Text = "Open Addons"
    toggleButton.Draggable = true
    toggleButton.Active = true
    toggleButton.Parent = screenGui

    local addonsFrame = Instance.new("Frame")
    addonsFrame.Size = UDim2.new(0, 250, 0, 140)
    addonsFrame.Position = UDim2.new(0, 470, 0, 200)
    addonsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    addonsFrame.BorderSizePixel = 0
    addonsFrame.Visible = false
    addonsFrame.Parent = screenGui

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 20)
    titleLabel.Position = UDim2.new(0, 0, 0, 0)
    titleLabel.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 14
    titleLabel.Text = "Addons"
    titleLabel.Parent = addonsFrame

    local function createButton(text, yPos, callback)
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(0, 230, 0, 30)
        button.Position = UDim2.new(0, 10, 0, yPos)
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        button.TextColor3 = Color3.new(1, 1, 1)
        button.Font = Enum.Font.Gotham
        button.TextSize = 14
        button.Text = text
        button.Parent = addonsFrame
        button.MouseButton1Click:Connect(callback)
    end

    createButton("No Cooldown", 25, function()
        local success, err = pcall(function()
            local C = require(game:GetService("ReplicatedStorage"):WaitForChild("Controllers"):WaitForChild("AbilityController"))
            local o = C.AbilityCooldown
            C.AbilityCooldown = function(s, n, ...)
                return o(s, n, 0, ...)
            end
            print("No Cooldown enabled.")
        end)
        if not success then warn("Cooldown Error:", err) end
    end)

    createButton("Toggle Inf Flow", 60, function()
        local stat = player:FindFirstChild("PlayerStats") and player.PlayerStats:FindFirstChild("InFlow")
        if stat and stat:IsA("BoolValue") then
            stat.Value = not stat.Value
            print("InFlow:", stat.Value)
        else
            warn("InFlow not found or invalid.")
        end
    end)

    createButton("Toggle Inf Awakening", 95, function()
        local stat = player:FindFirstChild("PlayerStats") and player.PlayerStats:FindFirstChild("InAwakening")
        if stat and stat:IsA("BoolValue") then
            stat.Value = not stat.Value
            print("InAwakening:", stat.Value)
        else
            warn("InAwakening not found or invalid.")
        end
    end)

    toggleButton.MouseButton1Click:Connect(function()
        addonsFrame.Visible = not addonsFrame.Visible
        toggleButton.Text = addonsFrame.Visible and "Close Addons" or "Open Addons"
    end)
end

createAddonsFrame()

