--[[Main Script]]
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local camera = workspace.CurrentCamera
local runService = game:GetService("RunService")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FOVChangerGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 150)
frame.Position = UDim2.new(0.5, -125, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "FOV Changer"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24
title.Parent = frame

local textbox = Instance.new("TextBox")
textbox.PlaceholderText = "Enter FOV"
textbox.Size = UDim2.new(0.8, 0, 0, 30)
textbox.Position = UDim2.new(0.1, 0, 0.4, 0)
textbox.Text = ""
textbox.TextColor3 = Color3.new(1, 1, 1)
textbox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
textbox.ClearTextOnFocus = false
textbox.Font = Enum.Font.SourceSans
textbox.TextSize = 18
textbox.Parent = frame

local button = Instance.new("TextButton")
button.Size = UDim2.new(0.5, 0, 0, 30)
button.Position = UDim2.new(0.25, 0, 0.7, 0)
button.Text = "Submit"
button.TextColor3 = Color3.new(1, 1, 1)
button.BackgroundColor3 = Color3.fromRGB(30, 120, 30)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 20
button.Parent = frame

local lockedFOV = nil

runService.RenderStepped:Connect(function()
	if lockedFOV then
		if camera.FieldOfView ~= lockedFOV then
			camera.FieldOfView = lockedFOV
		end
	end
end)

button.MouseButton1Click:Connect(function()
	local fov = tonumber(textbox.Text)
	if fov and fov >= 1 and fov <= 120 then
		local cam = workspace.CurrentCamera
		cam.FieldOfView = fov
	else
		button.Text = "Invalid FOV"
		wait(1)
		button.Text = "Submit"
	end
end)
