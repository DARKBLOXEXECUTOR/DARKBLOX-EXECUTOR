# DARKBLOX-EXECUTOR
A free executor keyless, very new!
here it is!



local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Colors for gold theme
local goldMain = Color3.fromRGB(212, 175, 55)
local goldDark = Color3.fromRGB(160, 130, 30)
local goldLight = Color3.fromRGB(255, 223, 93)

-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DarkBloxExecutorGui"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

-- Function to make frames draggable
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos

    local function update(input)
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end

-- Main executor frame (initially visible)
local executorFrame = Instance.new("Frame")
executorFrame.Name = "ExecutorFrame"
executorFrame.Size = UDim2.new(0, 450, 0, 250)
executorFrame.Position = UDim2.new(0.5, -225, 0.5, -125)
executorFrame.BackgroundColor3 = Color3.fromRGB(30, 24, 8)
executorFrame.BorderSizePixel = 0
executorFrame.Active = true
executorFrame.Parent = screenGui
makeDraggable(executorFrame)

-- Title bar
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 35)
titleBar.BackgroundColor3 = goldDark
titleBar.BorderSizePixel = 0
titleBar.Parent = executorFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -70, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "✨ DarkBlox Executor"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextColor3 = goldLight
titleLabel.TextSize = 20
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

-- Close button on title bar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -40, 0, 2)
closeButton.BackgroundColor3 = goldMain
closeButton.BorderSizePixel = 0
closeButton.Text = "✕"
closeButton.Font = Enum.Font.GothamBold
closeButton.TextColor3 = Color3.new(0.1, 0.05, 0)
closeButton.TextSize = 22
closeButton.Parent = titleBar

-- TextBox for script input
local scriptBox = Instance.new("TextBox")
scriptBox.Size = UDim2.new(1, -40, 0.65, 0)
scriptBox.Position = UDim2.new(0, 20, 0, 45)
scriptBox.BackgroundColor3 = Color3.fromRGB(40, 35, 10)
scriptBox.TextColor3 = goldLight
scriptBox.Text = "-- Write your Lua script here"
scriptBox.ClearTextOnFocus = false
scriptBox.TextWrapped = true
scriptBox.MultiLine = true
scriptBox.Font = Enum.Font.Gotham
scriptBox.TextSize = 16
scriptBox.Parent = executorFrame

-- Buttons container
local buttonsFrame = Instance.new("Frame")
buttonsFrame.Size = UDim2.new(1, -40, 0, 50)
buttonsFrame.Position = UDim2.new(0, 20, 1, -60)
buttonsFrame.BackgroundTransparency = 1
buttonsFrame.Parent = executorFrame

-- Utility to create gold buttons
local function createGoldButton(name, pos, text)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Size = UDim2.new(0, 120, 1, 0)
    btn.Position = pos
    btn.BackgroundColor3 = goldMain
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = true
    btn.Font = Enum.Font.GothamBold
    btn.TextColor3 = Color3.new(0.1, 0.05, 0)
    btn.TextSize = 18
    btn.Text = text

    local grad = Instance.new("UIGradient")
    grad.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, goldLight),
        ColorSequenceKeypoint.new(0.5, goldMain),
        ColorSequenceKeypoint.new(1, goldLight),
    })
    grad.Rotation = 90
    grad.Parent = btn

    return btn
end

-- Attach button
local attached = false
local attachBtn = createGoldButton("AttachBtn", UDim2.new(0, 0, 0, 0), "Attach")
attachBtn.Parent = buttonsFrame
attachBtn.MouseButton1Click:Connect(function()
    if not attached then
        attached = true
        attachBtn.Text = "Attached ✅"
        attachBtn.BackgroundColor3 = goldDark
    else
        attached = false
        attachBtn.Text = "Attach"
        attachBtn.BackgroundColor3 = goldMain
    end
end)

-- Execute button
local executeBtn = createGoldButton("ExecuteBtn", UDim2.new(0, 135, 0, 0), "Execute")
executeBtn.Parent = buttonsFrame
executeBtn.MouseButton1Click:Connect(function()
    if not attached then
        warn("Please attach before executing the script!")
        return
    end

    local success, err = pcall(function()
        local func = loadstring(scriptBox.Text)
        if func then
            func()
        else
            warn("Invalid script!")
        end
    end)

    if not success then
        warn("Error executing script:", err)
    end
end)

-- Clear button
local clearBtn = createGoldButton("ClearBtn", UDim2.new(0, 270, 0, 0), "Clear")
clearBtn.Parent = buttonsFrame
clearBtn.MouseButton1Click:Connect(function()
    scriptBox.Text = ""
end)

-- Close button hides the executorFrame and shows the icon
closeButton.MouseButton1Click:Connect(function()
    executorFrame.Visible = false
    appIcon.Visible = true
end)

-- Create floating app icon (computer screen icon)
local appIcon = Instance.new("Frame")
appIcon.Size = UDim2.new(0, 50, 0, 40)
appIcon.Position = UDim2.new(0, 20, 0, 20)
appIcon.BackgroundColor3 = goldDark
appIcon.BorderSizePixel = 0
appIcon.Parent = screenGui
appIcon.Active = true
appIcon.ZIndex = 10

local screenPart = Instance.new("Frame")
screenPart.Size = UDim2.new(0, 44, 0, 30)
screenPart.Position = UDim2.new(0, 3, 0, 3)
screenPart.BackgroundColor3 = goldMain
screenPart.BorderSizePixel = 0
screenPart.Parent = appIcon

for i = 1, 4 do
    local line = Instance.new("Frame")
    line.Size = UDim2.new(1, -14, 0, 3)
    line.Position = UDim2.new(0, 7, 0, 4 + (i - 1) * 6)
    line.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    line.BorderSizePixel = 0
    line.Parent = screenPart
end

local stand = Instance.new("Frame")
stand.Size = UDim2.new(0, 22, 0, 6)
stand.Position = UDim2.new(0.5, -11, 1, -6)
stand.BackgroundColor3 = goldDark
stand.BorderSizePixel = 0
stand.Parent = appIcon

appIcon.MouseButton1Click = nil
appIcon.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        executorFrame.Visible = true
        appIcon.Visible = false
    end
end)

-- Start with appIcon hidden because executorFrame is visible
appIcon.Visible = false

