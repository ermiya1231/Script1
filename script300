-- Script by IceMinister#9889
--// modified by Roblox-HttpSpy

--// made it for ac6 but feel free to also use it

local function HideGui()
	local success, result = pcall(function()
		return gethui()
	end)
	if success and result then
		return result
	else
		return game:GetService("CoreGui"):FindFirstChild("RobloxGui")
	end
end

local LibraryName = "i_eat_shampoo"
local NotificationLibrary = {}

local TweenService = game:GetService("TweenService")
local PlayerGui = HideGui()

local library
local templateFolder
local canvas

local function loadLibrary()
    local existing = PlayerGui:FindFirstChild(LibraryName)
    if existing then
        library = existing
    else
        local success, result = pcall(function()
            return game:GetObjects("rbxassetid://15133757123")[1]
        end)
        
        if not success or not result then
            warn("Failed to load library asset!")
            return
        end

        library = result
        library.Name = LibraryName
        library.Parent = PlayerGui
    end

    templateFolder = library:WaitForChild("Templates")
    canvas = library:WaitForChild("list")
end

function NotificationLibrary:Load()
    if not library or not library.Parent then
        loadLibrary()
    end
end

function NotificationLibrary:SendNotification(Mode, Text, Duration)
    if not library or not library.Parent then
        loadLibrary()
    end

    if not templateFolder or not canvas then
        warn("Library not initialized properly")
        return
    end

    local template = templateFolder:FindFirstChild(Mode)
    if not template then
        warn("Invalid notification type:", Mode)
        return
    end

    task.spawn(function()
        local success, err = pcall(function()
            local Notification = template:Clone()
            local filler = Notification:WaitForChild("Filler")
            local bar = Notification:WaitForChild("bar")

            Notification.Header.Text = Text
            Notification.Visible = true
            Notification.Size = UDim2.new(0, 0, 0.087, 0)
            Notification.Parent = canvas

            filler.Size = UDim2.new(1, 0, 1, 0)

            local T1 = TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
            local T2 = TweenInfo.new(Duration, Enum.EasingStyle.Linear)
            local T3 = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

            TweenService:Create(Notification, T1, { Size = UDim2.new(1, 0, 0.087, 0) }):Play()
            task.wait(0.2)
            TweenService:Create(filler, T3, { Size = UDim2.new(0.011, 0, 1, 0) }):Play()
            TweenService:Create(bar, T2, { Size = UDim2.new(1, 0, 0.05, 0) }):Play()

            task.wait(Duration)
            TweenService:Create(filler, T1, { Size = UDim2.new(1, 0, 1, 0) }):Play()
            task.wait(0.25)
            TweenService:Create(Notification, T3, { Size = UDim2.new(0, 0, 0.087, 0) }):Play()
            task.wait(0.25)
            Notification:Destroy()
        end)

        if not success then
            warn("failed to create notification:", err)
        end
    end)
end

return NotificationLibrary
