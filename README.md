-- Full Roblox Aimbot GUI Script with Customization, FOV, Triggerbot, Keybinds, and Team Check --

local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local camera = workspace.CurrentCamera

-- GUI Setup
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "CustomGUI"
screenGui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 300, 0, 380)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -190)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Visible = false

-- Close Button
local closeButton = Instance.new("TextButton", mainFrame)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.Text = "X"
closeButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
closeButton.TextColor3 = Color3.new(1, 1, 1)

-- Resize Handle
local resizeHandle = Instance.new("Frame", mainFrame)
resizeHandle.Size = UDim2.new(0, 20, 0, 20)
resizeHandle.Position = UDim2.new(1, -20, 1, -20)
resizeHandle.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
resizeHandle.AnchorPoint = Vector2.new(1, 1)
resizeHandle.Name = "ResizeHandle"

-- GUI Toggle Button
local toggleButton = Instance.new("TextButton", screenGui)
toggleButton.Size = UDim2.new(0, 140, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.Text = "Toggle GUI"
toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleButton.TextColor3 = Color3.new(1, 1, 1)

-- Keybind Button for main GUI
local keybindButton = Instance.new("TextButton", screenGui)
keybindButton.Size = UDim2.new(0, 140, 0, 40)
keybindButton.Position = UDim2.new(0, 10, 0, 60)
keybindButton.Text = "Set Keybind: T"
keybindButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
keybindButton.TextColor3 = Color3.new(1, 1, 1)

local toggleKey = Enum.KeyCode.T
keybindButton.MouseButton1Click:Connect(function()
	keybindButton.Text = "Press a key..."
	local conn
	conn = uis.InputBegan:Connect(function(input, processed)
		if not processed and input.UserInputType == Enum.UserInputType.Keyboard then
			toggleKey = input.KeyCode
			keybindButton.Text = "Set Keybind: " .. toggleKey.Name
			conn:Disconnect()
		end
	end)
end)

uis.InputBegan:Connect(function(input, processed)
	if not processed and input.KeyCode == toggleKey then
		mainFrame.Visible = not mainFrame.Visible
	end
end)

toggleButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
end)

closeButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
end)

-- Resize logic
local draggingResize = false
local dragStart, startSize
resizeHandle.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingResize = true
		dragStart = input.Position
		startSize = mainFrame.Size
	end
end)

uis.InputChanged:Connect(function(input)
	if draggingResize and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		mainFrame.Size = UDim2.new(0, startSize.X.Offset + delta.X, 0, startSize.Y.Offset + delta.Y)
	end
end)

uis.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		draggingResize = false
	end
end)

-- Drawing FOV
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = true
fovCircle.Thickness = 1
fovCircle.NumSides = 64
fovCircle.Radius = 100
fovCircle.Transparency = 1
fovCircle.Color = Color3.fromRGB(255, 255, 255)

-- Aimbot Vars
local aimbotEnabled = false
local aimbotKey = Enum.KeyCode.G
local smoothness = 50
local fovRadius = 100
local teamCheck = true
local fovColor = {R=255, G=255, B=255}

-- Triggerbot with Custom Keybind Setup
local triggerbotEnabled = false
local triggerMode = "Toggle" -- or "Hold"
local triggerKey = Enum.KeyCode.MouseButton2 -- Default keybind (Right Click)

local triggerbotButton = Instance.new("TextButton", mainFrame)
triggerbotButton.Size = UDim2.new(0, 270, 0, 25)
triggerbotButton.Position = UDim2.new(0, 10, 0, 365)
triggerbotButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
triggerbotButton.TextColor3 = Color3.new(1, 1, 1)
triggerbotButton.Text = "Triggerbot: OFF"

-- Mini buttons (initially hidden)
local holdButton = Instance.new("TextButton", triggerbotButton)
holdButton.Size = UDim2.new(0, 65, 0, 20)
holdButton.Position = UDim2.new(0, 0, 1, 0)
holdButton.Visible = false
holdButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
holdButton.TextColor3 = Color3.new(1, 1, 1)
holdButton.Text = "Hold"

local toggleMiniButton = Instance.new("TextButton", triggerbotButton)
toggleMiniButton.Size = UDim2.new(0, 65, 0, 20)
toggleMiniButton.Position = UDim2.new(0, 70, 1, 0)
toggleMiniButton.Visible = false
toggleMiniButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
toggleMiniButton.TextColor3 = Color3.new(1, 1, 1)
toggleMiniButton.Text = "Toggle"

-- Custom Keybind Button
local keybindButtonTrigger = Instance.new("TextButton", triggerbotButton)
keybindButtonTrigger.Size = UDim2.new(0, 100, 0, 20)
keybindButtonTrigger.Position = UDim2.new(0, 140, 1, 0)
keybindButtonTrigger.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
keybindButtonTrigger.TextColor3 = Color3.new(1, 1, 1)
keybindButtonTrigger.Text = "Keybind: Mouse2"

-- Triggerbot Logic
triggerbotButton.MouseButton1Click:Connect(function()
	triggerbotEnabled = not triggerbotEnabled
	triggerbotButton.Text = "Triggerbot: " .. (triggerbotEnabled and "ON" or "OFF")
end)

triggerbotButton.MouseButton2Click:Connect(function()
	holdButton.Visible = not holdButton.Visible
	toggleMiniButton.Visible = not toggleMiniButton.Visible
end)

holdButton.MouseButton1Click:Connect(function()
	triggerMode = "Hold"
	holdButton.BackgroundColor3 = Color3.fromRGB(30, 100, 30)
	toggleMiniButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
end)

toggleMiniButton.MouseButton1Click:Connect(function()
	triggerMode = "Toggle"
	toggleMiniButton.BackgroundColor3 = Color3.fromRGB(30, 100, 30)
	holdButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
end)

-- Keybind Setup for Triggerbot
keybindButtonTrigger.MouseButton1Click:Connect(function()
	keybindButtonTrigger.Text = "Press a key..."
	local conn
	conn = uis.InputBegan:Connect(function(input, processed)
		if not processed and input.UserInputType == Enum.UserInputType.Keyboard then
			triggerKey = input.KeyCode
			keybindButtonTrigger.Text = "Keybind: " .. triggerKey.Name
			conn:Disconnect()
		end
	end)
end)

-- Fire Triggerbot (placeholder: auto-clicks when over target)
runService.RenderStepped:Connect(function()
	if triggerbotEnabled then
		if triggerMode == "Hold" and not uis:IsMouseButtonPressed(triggerKey) then return end
		if triggerMode == "Toggle" and uis:IsKeyDown(triggerKey) then
			local target = getClosestPlayer()
			if target and target.Character and target.Character:FindFirstChild("Head") then
				mouse1click() -- placeholder for firing
			end
		end
	end
end)

-- Aimbot Logic
local function getClosestPlayer()
	local closestPlayer = nil
	local shortestDistance = math.huge
	local mouseLocation = uis:GetMouseLocation()
	for _, otherPlayer in pairs(game.Players:GetPlayers()) do
		if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("Head") then
			if teamCheck and otherPlayer.Team == player.Team then continue end
			local head = otherPlayer.Character.Head
			local screenPoint, onScreen = camera:WorldToViewportPoint(head.Position)
			if onScreen then
				local dist = (Vector2.new(screenPoint.X, screenPoint.Y) - &#8203;:contentReference[oaicite:0]{index=0}&#8203;
