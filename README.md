print("ez")

local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/Lucas-Lua/ui/main/m"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/ZAZA.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/InterfaceManager.lua.txt"))()

local ScreenGui1 = Instance.new("ScreenGui")
local ImageButton1 = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

ScreenGui1.Name = "ImageButton"
ScreenGui1.Parent = game.CoreGui
ScreenGui1.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton1.Parent = ScreenGui1
ImageButton1.BackgroundTransparency = 1
ImageButton1.BorderSizePixel = 0
ImageButton1.Position = UDim2.new(0.120833337, 0, 0.0952890813, 0)
ImageButton1.Size = UDim2.new(0, 50, 0, 50)
ImageButton1.Draggable = true
ImageButton1.Image = "rbxassetid://10723396107"
ImageButton1.MouseButton1Down:Connect(function()
	game:GetService("VirtualInputManager"):SendKeyEvent(true,305,false,game)
	game:GetService("VirtualInputManager"):SendKeyEvent(false,305,false,game)
end)
UICorner.Parent = ImageButton1

local Window = Fluent:CreateWindow({
	Title = "Rock Fruit",
	SubTitle = "Savage Hub",
	TabWidth = 130,
	Size = UDim2.fromOffset(480, 400),
	Acrylic = false, 
	Theme = "Dark",
	MinimizeKey = Enum.KeyCode.RightControl
})

local Tabs = {
	Setting = Window:AddTab({ Title = "Setting", Icon = "settings" }),
	General = Window:AddTab({ Title = "General", Icon = "cookie" })
}

local Set = Tabs.Setting:AddSection("|Setting") 

local Slider = Set:AddSlider("Distance Farm", {
	Title = "Distance Farm",
	Description = "Monster Farming Distance Let you choose to adjust the distance.",
	Default = 0,
	Min = 0,
	Max = 100,
	Rounding = 1,
	Callback = function(Value)
		_G.DistanceMob = Value
	end
})

Set:AddDropdown("Select Method", {
	Title = "Select Method",
	Values = {"Upper","Behind","Below"},
	Multi = false,
	Default = 1,
	Callback = function(v)
		_G.Method = v
	end
})

local MethodFarm = nil

spawn(function()
	while wait() do 
		pcall(function()
			if _G.Method == "Behind" then
				MethodFarm = CFrame.new(0,0,_G.DistanceMob)
			elseif _G.Method == "Below" then
				MethodFarm = CFrame.new(0,-_G.DistanceMob,0) * CFrame.Angles(math.rad(90),0,0)
			elseif _G.Method == "Upper" then
				MethodFarm = CFrame.new(0,_G.DistanceMob,0)  * CFrame.Angles(math.rad(-90),0,0)
			else
				MethodFarm = CFrame.new(0,_G.DistanceMob,0)  * CFrame.Angles(math.rad(-90),0,0)
			end
		end)
	end
end)

Set:AddDropdown("Select Weapon", {
	Title = "Select Weapon",
	Values = {"Melee","Sword","DevilFruit","Special"},
	Multi = true,
	Default = false,
	Callback = function(vd)
		_G.Weapon = vd
	end
})


spawn(function()
	while task.wait(.5) do
		if true then
			for i, v in ipairs(_G.Weapon) do
				for x,d in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
					if v.Name == d:GetAttribute("Type") then
						repeat task.wait(.5)
							game.Players.LocalPlayer.Character.Humanoid:EquipTool(d)
						until game.Players.LocalPlayer.Character:FindFirstChild(d.Name)
					end
				end
			end
		end
	end
end)

local Skill = Tabs.Setting:AddSection("|Automatic Skill") 


--skill

--end

local main = Tabs.General:AddSection("|Main") 

local interact = function(v2)
	local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up","MouseButton1Down"}
	for i,v in pairs(events) do
		for i,v in pairs(getconnections(v2[v])) do
			v.Function()
		end
	end
end




main:AddToggle("Automatic Dungeon", {
	Title = "Automatic Dungeon", 
	Default = false, 
	Callback = function(Dungeon) 
		_G.Dungeon = Dungeon
	end})


spawn(function()
	while task.wait() do
		if _G.Dungeon then
			xpcall(function()
				if workspace.Portal.Mid:GetChildren()[6].Enabled == false then
					if game:GetService("Players").LocalPlayer.PlayerGui.HUD.DungeonReward.Visible == false then
						if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Orb Dungeon") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Orb Dungeon") then
							game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-122.17112731933594, 34.3508415222168, -1161.41015625)
						else
							wait(1)
							game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Inventory"):FireServer("Orb Dungeon")
							wait(3.5)
							print("out orb")
						end
					else
						game.Players.LocalPlayer.CharacterRemoving:Wait()
						game.Players.LocalPlayer.CharacterAdded:Wait()
						interact(game:GetService("Players").LocalPlayer.PlayerGui.HUD.DungeonReward.CloseFrame.CloseButton)
						game:GetService("Players").LocalPlayer.PlayerGui.HUD.DungeonReward.Visible = false
					end
				else
					if not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("WaveUI") then
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-125.230751, 34.3508415, -1133.29834, -0.796580017, -9.39225586e-10, 0.604533076, -1.5428554e-09, 1, -4.79348838e-10, -0.604533076, -1.3145468e-09, -0.796580017)
					else
						for i, v in pairs(workspace.DunMob:GetChildren()) do
							if v and v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v:FindFirstChild("Humanoid").Health > 0 then
								repeat task.wait()
									game.Player.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,-29,0) * CFrame.Angles(math.rad(90),0,0)
								until not v or v.Humanoid.Health <= 0
							else
								repeat task.wait() until v and v:IsA("Model")
							end
						end
					end
				end
			end)
		end
	end
end)






local poin = Set:AddSlider("Poin", {
	Title = "Poin",
	Description = "The points used to upgrade can be used as much as you want.",
	Default = 0,
	Min = 0,
	Max = 2000,
	Rounding = 1,
	Callback = function(Value)
		_G.Poin = Value
	end
})


Tabs.General:AddDropdown("Select Stats", {
	Title = "Select Stats",
	Values = {"Melee","Sword","DevilFruit","Special"},
	Multi = true,
	Default = 1,
	Callback = function(bool)
		_G.Stats = bool
	end
})

Tabs.General:AddToggle("Automatic Up Stats", {
	Title = "Automatic Up Stats", 
	Default = false, 
	Callback = function(Autoup) 
		_G.Autostats = Autoup
	end})


spawn(function()
	while task.wait() do
		if _G.Autostats then
			local args = {
				[1] = "UpStats",
				[2] = _G.Stats,
				[3] = _G.Poin
			}

			game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("System"):FireServer(unpack(args))
		end
	end
end)





