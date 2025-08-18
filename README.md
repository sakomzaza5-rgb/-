--// Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local UIS = game:GetService("UserInputService")

--// Remotes
local growRemote = ReplicatedStorage:WaitForChild("Events"):WaitForChild("Gameplay"):WaitForChild("GrowPassiveFood")
local friendsRemote = ReplicatedStorage:WaitForChild("Events"):WaitForChild("Other"):WaitForChild("ChangeFriendsOnline")

--// ScreenGui
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "AutoGUI"

--// Main Frame
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 250, 0, 200)
Frame.Position = UDim2.new(0, 20, 0, 50)
Frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Visible = false
Frame.ZIndex = 4

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 12)

local UIStroke = Instance.new("UIStroke", Frame)
UIStroke.Color = Color3.fromRGB(0, 200, 255)
UIStroke.Thickness = 2

--// Toggle Button
local ToggleBtn = Instance.new("TextButton", ScreenGui)
ToggleBtn.Size = UDim2.new(0, 40, 0, 40)
ToggleBtn.Position = UDim2.new(0, 5, 0, 5)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ToggleBtn.Text = "≡"
ToggleBtn.TextColor3 = Color3.fromRGB(255,255,255)
ToggleBtn.TextScaled = true
ToggleBtn.ZIndex = 5

local UICorner2 = Instance.new("UICorner", ToggleBtn)
UICorner2.CornerRadius = UDim.new(1,0)

ToggleBtn.MouseButton1Click:Connect(function()
	Frame.Visible = not Frame.Visible
end)

--// Title
local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, -10, 0, 30)
Title.Position = UDim2.new(0, 5, 0, 5)
Title.BackgroundTransparency = 1
Title.Text = "⚡ Auto Farm"
Title.TextColor3 = Color3.fromRGB(0, 255, 200)
Title.TextScaled = true
Title.ZIndex = 5

--// Grow Multiplier
local MultiplierBox = Instance.new("TextBox", Frame)
MultiplierBox.Size = UDim2.new(0.45, -10, 0, 30)
MultiplierBox.Position = UDim2.new(0, 5, 0, 45)
MultiplierBox.PlaceholderText = "Grow x10"
MultiplierBox.Text = ""
MultiplierBox.TextScaled = true
MultiplierBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
MultiplierBox.TextColor3 = Color3.fromRGB(255,255,255)
MultiplierBox.ZIndex = 5
MultiplierBox.ClearTextOnFocus = false
MultiplierBox.TextEditable = true

local UICorner3 = Instance.new("UICorner", MultiplierBox)
UICorner3.CornerRadius = UDim.new(0, 8)

--// Grow Speed
local SpeedBox = Instance.new("TextBox", Frame)
SpeedBox.Size = UDim2.new(0.45, -10, 0, 30)
SpeedBox.Position = UDim2.new(0.5, 5, 0, 45)
SpeedBox.PlaceholderText = "Delay 0.01"
SpeedBox.Text = ""
SpeedBox.TextScaled = true
SpeedBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
SpeedBox.TextColor3 = Color3.fromRGB(255,255,255)
SpeedBox.ZIndex = 5
SpeedBox.ClearTextOnFocus = false
SpeedBox.TextEditable = true

local UICorner4 = Instance.new("UICorner", SpeedBox)
UICorner4.CornerRadius = UDim.new(0, 8)

--// Start Grow Button
local StartGrowBtn = Instance.new("TextButton", Frame)
StartGrowBtn.Size = UDim2.new(1, -10, 0, 40)
StartGrowBtn.Position = UDim2.new(0, 5, 0, 85)
StartGrowBtn.Text = "▶ Start Auto Grow"
StartGrowBtn.BackgroundColor3 = Color3.fromRGB(0,150,0)
StartGrowBtn.TextColor3 = Color3.fromRGB(255,255,255)
StartGrowBtn.TextScaled = true
StartGrowBtn.ZIndex = 5

local UICorner5 = Instance.new("UICorner", StartGrowBtn)
UICorner5.CornerRadius = UDim.new(0, 10)

--// Start Friends Button
local StartFriendBtn = Instance.new("TextButton", Frame)
StartFriendBtn.Size = UDim2.new(1, -10, 0, 40)
StartFriendBtn.Position = UDim2.new(0, 5, 0, 135)
StartFriendBtn.Text = "▶ Start Auto Friends"
StartFriendBtn.BackgroundColor3 = Color3.fromRGB(0,150,0)
StartFriendBtn.TextColor3 = Color3.fromRGB(255,255,255)
StartFriendBtn.TextScaled = true
StartFriendBtn.ZIndex = 5

local UICorner6 = Instance.new("UICorner", StartFriendBtn)
UICorner6.CornerRadius = UDim.new(0, 10)

--// Toggle States
local runningGrow = false
local runningFriends = false

--// Auto Grow Function
StartGrowBtn.MouseButton1Click:Connect(function()
	runningGrow = not runningGrow
	if runningGrow then
		StartGrowBtn.Text = "■ Stop Auto Grow"
		StartGrowBtn.BackgroundColor3 = Color3.fromRGB(200,0,0)
		
		spawn(function()
			while runningGrow do
				local multiplier = tonumber(MultiplierBox.Text) or 10
				local delay = tonumber(SpeedBox.Text) or 0.01
				for i = 1, multiplier do
					growRemote:FireServer()
				end
				task.wait(delay)
			end
		end)
	else
		StartGrowBtn.Text = "▶ Start Auto Grow"
		StartGrowBtn.BackgroundColor3 = Color3.fromRGB(0,150,0)
	end
end)

--// Auto Friends Function
StartFriendBtn.MouseButton1Click:Connect(function()
	runningFriends = not runningFriends
	if runningFriends then
		StartFriendBtn.Text = "■ Stop Auto Friends"
		StartFriendBtn.BackgroundColor3 = Color3.fromRGB(200,0,0)
		
		spawn(function()
			local count = 0
			while runningFriends do
				count = count + 1
				friendsRemote:FireServer(count)
				task.wait(0.01)
			end
		end)
	else
		StartFriendBtn.Text = "▶ Start Auto Friends"
		StartFriendBtn.BackgroundColor3 = Color3.fromRGB(0,150,0)
	end
end)
