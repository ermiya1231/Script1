local g = game
local Workspace = g:GetService("Workspace")
local ReplicatedStorage = g:GetService("ReplicatedStorage")

--// the audios will be parent in whatever instance is in this variable, its recommended you should replace this if your getting kicked cuz dev may protect TestService since it's the default
local INJECT = g:GetService("TestService")
local REMOTE = "AC6_FE_Sounds" --// replace this if the dev changed the remotes name

--// varbs
local NotificationLibrary = loadstring(g:HttpGet("https://raw.githubusercontent.com/Roblox-HttpSpy/AC6-Music-Exploit/refs/heads/main/notif.luau"))()
local Chars = "qwertyuiopasdfghjklzxcvbnm" --// dont use random symbols ill just make it more detectable
local Random = Random.new()
local loopState, isDragging = true, false
local typof, Udim2, Color, ref = typeof, UDim2.new, Color3.new, {}
local eep, make, call, font = task.wait, Instance.new, pcall, Font.fromEnum(Enum.Font.Cartoon)
local split, tonum, ismatch = task.spawn, tonumber, string.match
local gradcolor = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255,0,0)),
	ColorSequenceKeypoint.new(0.16, Color3.fromRGB(255,255,0)),
	ColorSequenceKeypoint.new(0.33, Color3.fromRGB(0,255,0)),
	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0,255,255)),
	ColorSequenceKeypoint.new(0.66, Color3.fromRGB(0,0,255)),
	ColorSequenceKeypoint.new(0.83, Color3.fromRGB(255,0,255)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255,0,0))
})

--// functions
local function RandomName()
	local length = Random:NextInteger(5, 10)
	local mode = Random:NextInteger(1, 3)
	local secondCapIndex
	if mode == 1 then
		secondCapIndex = Random:NextInteger(2, length)
	end
	local result = table.create(length)
	for i = 1, length do
		local index = Random:NextInteger(1, 26)
		local char = string.char(Chars:byte(index))
		if (mode == 1 and (i == 1 or i == secondCapIndex)) or (mode == 2 and i == 1) then
			char = string.upper(char)
		end
		result[i] = char
	end
	return table.concat(result)
end

local function formatAssetId(input)
	if ismatch(input, "^rbxassetid://")
		or ismatch(input, "^rbxasset://")
		or ismatch(input, "^rbxgameasset://")
		or ismatch(input, "^rbxhttp://") then
		return input
	end
	local id = ismatch(input, "/asset/(%d+)")
	return "rbxassetid://" .. (id or input)
end

local function toNumber(value, default)
	return tonumber(value) or default
end

local function notify(typ, msg, duration)
	NotificationLibrary:SendNotification(typ, msg, duration)
end

local function Query(name, parent)
	local results = parent:QueryDescendants(">>RemoteEvent#" .. name)
	return results[1]
end

local function getremote()
	return Query(REMOTE, Workspace)
		or Query(REMOTE, ReplicatedStorage)
end

local function firesound(remote, name, assetId, pitch, volume, looped)
	remote:FireServer("newSound", name, INJECT, assetId, pitch, volume, looped)
	remote:FireServer("playSound", name)
end

local function hiddengui()
	local ok, core = call(gethui)
	if ok and core then
		return core
	end
	return g:GetService("CoreGui")
end

local function insertgrad(par)
	local grad = make("UIGradient", par)
	grad.Rotation = 0
	grad.Color = gradcolor
	split(function()
		while eep(0.03) do
			grad.Rotation = (grad.Rotation + 1) % 360
		end
	end)
end

local function create(class, props, parent)
    local instance = make(class)
    if props then
        for key, value in props do
            if typof(key) == "number" and typof(value) == "Instance" then
                value.Parent = instance
            else
                local success, err = call(function()
                    instance[key] = value
                end)
                if not success then
                    warn(`Failed to set property "{key}" on {class}: {err}`)
                end
            end
        end
    end
    if parent then
        instance.Parent = parent
    end
    return instance
end

--// UI
local gooey = hiddengui()
local gui = create("ScreenGui", {
	Name = RandomName(),
	ResetOnSpawn = false,
}, gooey)

local folder = create("Folder", ref, gui)

local OhNo = create("Sound", {
	SoundId = "rbxassetid://9066167010",
	Volume = 1,
}, folder)

local Click = create("Sound", {
	SoundId = "rbxassetid://156785206",
	Volume = 1,
}, folder)

local main = create("Frame", {
	Size = Udim2(0, 400, 0, 250),
	Position = Udim2(0.5, -200, 0.5, -125),
	BackgroundColor3 = Color(0, 0, 0),
	create("UICorner", ref),
	create("UIDragDetector", ref),
}, gui)

local border = create("Frame", {
	Size = Udim2(1, 10, 1, 10),
	Position = Udim2(0, -5, 0, -5),
	BackgroundColor3 = Color(255, 255, 255),
	ZIndex = 0,
	create("UICorner", ref)
}, main) insertgrad(border)

local title = create("TextLabel", {
	Size = Udim2(1, 0, 0, 40),
	BackgroundTransparency = 1,
	Text = "Ac6-Vulnerablity [By: https]",
	FontFace = font,
	TextSize = 28,
	TextColor3 = Color(1, 1, 1),
}, main) insertgrad(title)

local textBox = create("TextBox", {
	Size = Udim2(0.8, 0, 0, 40),
	Position = Udim2(0.1, 0, 0.3, 0),
	BackgroundColor3 = Color(0, 0, 0),
	TextColor3 = Color(1, 1, 1),
	PlaceholderText = "put music id here! (it can be just the numbers or the enitre rbxassestid://?)",
	Text = "",
	FontFace = font,
	TextSize = 20,
	TextWrap = true,
	create("UICorner", ref)
}, main)

local function createBox(size, pos, placeholder)
	local box = create("TextBox", {
		Size = size,
		Position = pos,
		BackgroundColor3 = Color(0, 0, 0),
		TextColor3 = Color(1, 1, 1),
		PlaceholderText = placeholder,
		Text = "",
		FontFace = font,
		TextSize = 18,
		ClearTextOnFocus = false,
		TextWrapped = true,
		create("UICorner"),
	}, main)
	return box
end

local volumeBox = createBox(Udim2(0.25,0,0,35), Udim2(0.1,0,0.6,0), "Volume")
local pitchBox = createBox(Udim2(0.25,0,0,35), Udim2(0.65,0,0.6,0), "Pitch")

local loopBox = create("TextButton", {
	Size = Udim2(0.25, 0, 0, 35),
	Position = Udim2(0.375, 0, 0.6, 0),
	BackgroundColor3 = Color(0, 0, 0),
	TextColor3 = Color(1, 1, 1),
	FontFace = font,
	TextSize = 18,
	RichText = true,
	Text = "Loops? <font color='rgb(0,255,0)'>true</font>",
	create("UICorner"),
}, main)

local button = create("TextButton", {
	Size = Udim2(0.3, 0, 0, 35),
	Position = Udim2(0.35, 0, 0.8, 0),
	BackgroundColor3 = Color(0, 0, 0),
	TextColor3 = Color(1, 1, 1),
	Text = "Play!",
	FontFace = font,
	TextSize = 22,
	create("UICorner"),
}, main)

local closeButton = create("TextButton", {
	Size = Udim2(0, 30, 0, 30),
	Position = Udim2(1, -35, 0, 5),
	BackgroundColor3 = Color(0, 0, 0),
	TextColor3 = Color(1, 1, 1),
	Text = "X",
	FontFace = font,
	TextSize = 20,
	create("UICorner"),
}, main)

local bubble = create("TextButton", {
	Size = Udim2(0, 70, 0, 70),
	Position = Udim2(0.5, -35, 0.2, 0),
	BackgroundColor3 = Color(0, 0, 0),
	TextColor3 = Color(1, 1, 1),
	Text = "AC6",
	FontFace = font,
	TextSize = 22,
	Visible = false,
	create("UICorner"),
}, gui) insertgrad(bubble)

--// script
local drag = make("UIDragDetector", bubble)
drag.DragStart:Connect(function()
	isDragging = true
end)

drag.DragEnd:Connect(function()
	if isDragging then
		main.Visible = true
		bubble.Visible = false
		Click:Play()
	end
end)

drag.DragContinue:Connect(function()
	isDragging = false
end)

closeButton.Activated:Connect(function()
	main.Visible = false
	bubble.Visible = true
	Click:Play()
end)

loopBox.Activated:Connect(function()
    Click:Play()
    if loopState then
        loopState = false
        loopBox.Text = "Loops? <font color='rgb(255,0,0)'>false</font>"
    else
        loopState = true
        loopBox.Text = "Loops? <font color='rgb(0,255,0)'>true</font>"
    end
end)

--// saves name & remote so you can re-edit one audio instead of creating multiple, you can instead cut and paste it below the Activated:Connect for the opposite effect
local AudioName = RandomName()
local SavedRemote = getremote()

button.Activated:Connect(function()
	local remote = (SavedRemote and SavedRemote:IsDescendantOf(g)) and SavedRemote or getremote()
	
	if not remote then
		OhNo:Play()
		return notify("Error", "Couldn't Find AC6 Remote :(", 5)
	end
	
	local success = call(firesound,
		remote,
		AudioName,
		formatAssetId(textBox.Text),
		toNumber(pitchBox.Text, 1),
		toNumber(volumeBox.Text, 1),
		loopState
	)
	
	if not success then
		OhNo:Play()
		return notify("Error", "unknown/unexpected error? script itself may have broke", 4)
	end
	
	Click:Play()
	notify("Info", "Successfully Executed!", 3)
end)
