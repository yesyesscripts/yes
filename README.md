-- Roblox GUI Script: Based on the provided image
local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")

-- GUI Setup
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "CustomGUI"

-- Helper function for tabs
local function createTab(name, position)
    local tab = Instance.new("Frame")
    tab.Name = name
    tab.Size = UDim2.new(0, 300, 0, 400)
    tab.Position = position
    tab.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    tab.BorderSizePixel = 0
    tab.Visible = true
    tab.Parent = screenGui

    local title = Instance.new("TextLabel")
    title.Text = name
    title.Size = UDim2.new(1, 0, 0, 25)
    title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    title.TextColor3 = Color3.new(1, 1, 1)
    title.Font = Enum.Font.SourceSans
    title.TextSize = 18
    title.Parent = tab

    return tab
end

-- Main Tabs
local mainTab = createTab("Main", UDim2.new(0, 20, 0, 100))
local playerTab = createTab("Player", UDim2.new(0, 340, 0, 100))
local visualsTab = createTab("Visuals", UDim2.new(0, 660, 0, 100))

-- Feature Elements
-- Camlock
local camlock = Instance.new("TextButton", mainTab)
camlock.Text = "Camlock [Key: C]"
camlock.Position = UDim2.new(0, 10, 0, 40)
camlock.Size = UDim2.new(0, 120, 0, 30)
camlock.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
camlock.TextColor3 = Color3.new(1, 1, 1)
camlock.MouseButton1Click:Connect(function()
    print("Camlock toggled")
end)

-- Triggerbot
local triggerbot = Instance.new("TextButton", mainTab)
triggerbot.Text = "Triggerbot [Key: T]"
triggerbot.Position = UDim2.new(0, 10, 0, 80)
triggerbot.Size = UDim2.new(0, 120, 0, 30)
triggerbot.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
triggerbot.TextColor3 = Color3.new(1, 1, 1)
triggerbot.MouseButton1Click:Connect(function()
    print("Triggerbot toggled")
end)

-- Silent Aim
local silentAim = Instance.new("TextButton", mainTab)
silentAim.Text = "Silent Aim [Key: Z]"
silentAim.Position = UDim2.new(0, 10, 0, 120)
silentAim.Size = UDim2.new(0, 120, 0, 30)
silentAim.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
silentAim.TextColor3 = Color3.new(1, 1, 1)
silentAim.MouseButton1Click:Connect(function()
    print("Silent Aim toggled")
end)

-- Hitbox Expander
local hitbox = Instance.new("TextButton", playerTab)
hitbox.Text = "Expand Hitbox [Key: H]"
hitbox.Position = UDim2.new(0, 10, 0, 40)
hitbox.Size = UDim2.new(0, 120, 0, 30)
hitbox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
hitbox.TextColor3 = Color3.new(1, 1, 1)
hitbox.MouseButton1Click:Connect(function()
    print("Hitbox Expander toggled")
end)

-- Kill Aura
local killAura = Instance.new("TextButton", playerTab)
killAura.Text = "Kill Aura [Key: K]"
killAura.Position = UDim2.new(0, 10, 0, 80)
killAura.Size = UDim2.new(0, 120, 0, 30)
killAura.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
killAura.TextColor3 = Color3.new(1, 1, 1)
killAura.MouseButton1Click:Connect(function()
    print("Kill Aura toggled")
end)

-- Keybinds
UIS.InputBegan:Connect(function(input, processed)
    if processed then return end
    local key = input.KeyCode
    if key == Enum.KeyCode.C then
        camlock:Activate()
    elseif key == Enum.KeyCode.T then
        triggerbot:Activate()
    elseif key == Enum.KeyCode.Z then
        silentAim:Activate()
    elseif key == Enum.KeyCode.H then
        hitbox:Activate()
    elseif key == Enum.KeyCode.K then
        killAura:Activate()
    end
end)
