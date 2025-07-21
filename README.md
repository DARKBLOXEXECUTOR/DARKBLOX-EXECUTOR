# DARKBLOX-EXECUTOR
A free executor keyless, very new!
here it is!



local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Main GUI container
local darkbloxGui = Instance.new("ScreenGui")
darkbloxGui.Name = "DarkBloxExecutor"
darkbloxGui.ResetOnSpawn = false
darkbloxGui.Parent = playerGui

-- Splash Screen
local splash = Instance.new("TextLabel")
splash.Size = UDim2.new(1, 0, 1, 0)
splash.BackgroundColor3 = Color3.new(0, 0, 0)
splash.Text = "DARKBLOX SCRIPT LOADED!"
splash.Font = Enum.Font.GothamBlack
splash.TextColor3 = Color3.fromRGB(255, 215, 0)
splash.TextScaled = true
splash.Parent = darkbloxGui

task.wait(2)
for i = 1, 20 do
	splash.TextTransparency = i * 0.05
	splash.BackgroundTransparency = i * 0.05
	task.wait(0.05)
end
splash:Destroy()

-- Main Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 420, 0, 320)
frame.Position = UDim2.new(0.5, -210, 0.5, -160)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 1
frame.Parent = darkbloxGui
frame.Active = true
frame.Draggable = true

-- Fade-in effect for frame
for i = 20, 0, -1 do
	frame.BackgroundTransparency = i * 0.05
	task.wait(0.03)
end

-- Title Bar
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.BackgroundColor3 = Color3.fromRGB(160, 130, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = frame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -60, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "✨ DarkBlox Executor"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextColor3 = Color3.fromRGB(255, 215, 0)
titleLabel.TextSize = 22
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 35, 0, 30)
closeButton.Position = UDim2.new(1, -45, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(212, 175, 55)
closeButton.BorderSizePixel = 0
closeButton.Text = "✕"
closeButton.Font = Enum.Font.GothamBold
closeButton.TextColor3 = Color3.fromRGB(60, 45, 0)
closeButton.TextSize = 24
closeButton.Parent = titleBar

-- Script TextBox
local scriptBox = Instance.new("TextBox")
scriptBox.Size = UDim2.new(0.95, 0, 0.65, 0)
scriptBox.Position = UDim2.new(0, 10, 0, 50)
scriptBox.BackgroundColor3 = Color3.fromRGB(40, 35, 10)
scriptBox.TextColor3 = Color3.fromRGB(255, 215, 0)
scriptBox.ClearTextOnFocus = false
scriptBox.TextWrapped = true
scriptBox.MultiLine = true
scriptBox.PlaceholderText = "-- Paste or write your Lua script here"
scriptBox.Font = Enum.Font.Code
scriptBox.TextSize = 18
scriptBox.Parent = frame

-- Buttons container
local buttonsFrame = Instance.new("Frame")
buttonsFrame.Size = UDim2.new(0.95, 0, 0, 45)
buttonsFrame.Position = UDim2.new(0, 10, 1, -55)
buttonsFrame.BackgroundTransparency = 1
buttonsFrame.Parent = frame

local function createButton(name, posX, text)
	local btn = Instance.new("TextButton")
	btn.Name = name
	btn.Size = UDim2.new(0, 130, 1, 0)
	btn.Position = UDim2.new(0, posX, 0, 0)
	btn.BackgroundColor3 = Color3.fromRGB(212, 175, 55)
	btn.BorderSizePixel = 0
	btn.Font = Enum.Font.GothamBold
	btn.TextColor3 = Color3.fromRGB(60, 45, 0)
	btn.TextSize = 20
	btn.Text = text
	btn.Parent = buttonsFrame
	return btn
end

local attachBtn = createButton("AttachBtn", 0, "Attach")
local executeBtn = createButton("ExecuteBtn", 140, "Execute")
local clearBtn = createButton("ClearBtn", 280, "Clear")

-- Message Label
local message = Instance.new("TextLabel")
message.Size = UDim2.new(1, 0, 0, 30)
message.Position = UDim2.new(0, 0, 1, -90)
message.BackgroundTransparency = 1
message.TextColor3 = Color3.fromRGB(255, 215, 0)
message.Font = Enum.Font.GothamBold
message.TextSize = 18
message.Text = ""
message.Parent = frame

local attached = false

-- Attach Button
attachBtn.MouseButton1Click:Connect(function()
	attached = not attached
	if attached then
		attachBtn.Text = "Attached ✅"
		message.Text = "Executor attached!"
	else
		attachBtn.Text = "Attach"
		message.Text = "Executor detached."
	end
	task.delay(2, function()
		message.Text = ""
	end)
end)

-- Execute Button
executeBtn.MouseButton1Click:Connect(function()
	if not attached then
		message.Text = "⚠️ Attach first!"
		task.delay(2, function()
			message.Text = ""
		end)
		return
	end
	local code = scriptBox.Text
	local success, err = pcall(function()
		local func = loadstring(code)
		if func then func() end
	end)
	if success then
		message.Text = "✅ Script executed!"
	else
		message.Text = "❌ Error: "..tostring(err)
	end
	task.delay(3, function()
		message.Text = ""
	end)
end)

-- Clear Button
clearBtn.MouseButton1Click:Connect(function()
	scriptBox.Text = ""
	message.Text = "Cleared!"
	task.delay(1.5, function()
		message.Text = ""
	end)
end)

-- Floating toggle icon to hide/show executor
local toggleIcon = Instance.new("TextButton")
toggleIcon.Size = UDim2.new(0, 50, 0, 40)
toggleIcon.Position = UDim2.new(0, 20, 0, 20)
toggleIcon.BackgroundColor3 = Color3.fromRGB(160, 130, 30)
toggleIcon.BorderSizePixel = 0
toggleIcon.Text = ""
toggleIcon.Parent = darkbloxGui
toggleIcon.ZIndex = 10
toggleIcon.Active = true
toggleIcon.Draggable = true

-- Add computer screen icon (gold lines)
local screenIcon = Instance.new("Frame")
screenIcon.Size = UDim2.new(0, 44, 0, 30)
screenIcon.Position = UDim2.new(0, 3, 0, 5)
screenIcon.BackgroundColor3 = Color3.fromRGB(212, 175, 55)
screenIcon.BorderSizePixel = 0
screenIcon.Parent = toggleIcon

for i = 1, 4 do
	local line = Instance.new("Frame")
	line.Size = UDim2.new(1, -14, 0, 3)
	line.Position = UDim2.new(0, 7, 0, 5 + (i - 1) * 6)
	line.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
	line.BorderSizePixel = 0
	line.Parent = screenIcon
end

local executorVisible = true
toggleIcon.MouseButton1Click:Connect(function()
	executorVisible = not executorVisible
	frame.Visible = executorVisible
	toggleIcon.Visible = not executorVisible
end)

closeButton.MouseButton1Click:Connect(function()
	frame.Visible = false
	toggleIcon.Visible = true
end)
