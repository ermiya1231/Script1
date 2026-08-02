local Players = game:GetService("Players")
local Debris = game:GetService("Debris")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local cooldown = 2
local debounce = false
local systemActive = false

local character, humanoid, root, animator
local walkTrack, idle1, idle2, idle3
local heartbeatConn, align, att

local function createSound(id)
	local s = Instance.new("Sound")
	s.SoundId = "rbxassetid://" .. id
	s.Volume = 1
	s.Parent = Workspace
	return s
end

local soundCharge = createSound("72906924519776")
local soundJump   = createSound("97551697833107")
local soundLand   = createSound("86836108531543")
local soundSlam   = createSound("75832950373264")

local function LoadAssets(char)
	character = char
	humanoid = char:WaitForChild("Humanoid")
	root = char:WaitForChild("HumanoidRootPart")
	animator = humanoid:WaitForChild("Animator")

	local function createAnim(id)
		local a = Instance.new("Animation")
		a.AnimationId = "rbxassetid://" .. id
		return animator:LoadAnimation(a)
	end

	walkTrack = createAnim("48885239")
	idle1 = createAnim("21633130")
	idle2 = createAnim("126753849")
	idle3 = createAnim("94853940")
end

local function TriggerImpact(pos)

	local wave = Instance.new("Part")
	wave.Size = Vector3.new(0.2, 1, 1)
	wave.CFrame = CFrame.new(pos + Vector3.new(0, 0.1, 0)) * CFrame.Angles(0, 0, math.rad(90))
	wave.Anchored = true
	wave.CanCollide = false
	wave.Shape = Enum.PartType.Cylinder
	wave.Material = Enum.Material.SmoothPlastic
	wave.Color = Color3.fromRGB(200, 200, 200)
	wave.Transparency = 0
	wave.CastShadow = false
	wave.Parent = Workspace
	TweenService:Create(wave, TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
		Size = Vector3.new(0.05, 90, 90),
		Transparency = 1,
	}):Play()
	Debris:AddItem(wave, 0.65)

	for i = 1, 14 do
		local angle = math.rad((360 / 14) * i + math.random(-12, 12))
		local dist  = math.random(3, 10)
		local spike = Instance.new("Part")
		spike.Size = Vector3.new(0.6, math.random(3, 8), 0.6)
		spike.CFrame = CFrame.new(pos + Vector3.new(math.cos(angle) * dist, spike.Size.Y / 2 - 1, math.sin(angle) * dist))
			* CFrame.Angles(math.random(-15, 15) * math.pi/180, angle, math.random(-15, 15) * math.pi/180)
		spike.Anchored = true
		spike.CanCollide = false
		spike.Material = Enum.Material.Slate
		spike.Color = Color3.fromRGB(70, 70, 70)
		spike.CastShadow = false
		spike.Parent = Workspace
		TweenService:Create(spike, TweenInfo.new(0.55, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
			Size = Vector3.new(0.6, 0.1, 0.6),
			CFrame = spike.CFrame * CFrame.new(0, -spike.Size.Y / 2, 0),
		}):Play()
		Debris:AddItem(spike, 0.65)
	end

	local debrisMaterials = {Enum.Material.Slate, Enum.Material.Rock, Enum.Material.Concrete, Enum.Material.Cobblestone}
	for i = 1, 20 do
		local p = Instance.new("Part")
		local sz = math.random(4, 10) / 10
		p.Size = Vector3.new(sz * math.random(8,15)/10, sz, sz * math.random(8,15)/10)
		p.Position = pos + Vector3.new(math.random(-5, 5), 0.5, math.random(-5, 5))
		p.CFrame = p.CFrame * CFrame.Angles(math.random(0,360), math.random(0,360), math.random(0,360))
		p.Velocity = Vector3.new(math.random(-100, 100), math.random(60, 100), math.random(-100, 100))
		p.RotVelocity = Vector3.new(math.random(-25,25), math.random(-25,25), math.random(-25,25))
		p.Material = debrisMaterials[math.random(1, #debrisMaterials)]
		p.Color = Color3.fromRGB(math.random(50,100), math.random(45,85), math.random(40,75))
		p.CastShadow = false
		p.Parent = Workspace
		task.delay(0.5, function()
			TweenService:Create(p, TweenInfo.new(0.5), {Transparency = 1}):Play()
		end)
		Debris:AddItem(p, 1.1)
	end

	soundLand:Play()

	for _, obj in pairs(Workspace:GetDescendants()) do
		if obj:IsA("Model") and obj ~= character then
			local npcHum = obj:FindFirstChildWhichIsA("Humanoid", true)
			local npcRoot = obj:FindFirstChild("HumanoidRootPart")
				or obj:FindFirstChildWhichIsA("BasePart", true)

			if npcHum and npcRoot and npcHum.Health > 0 then
				if not Players:GetPlayerFromCharacter(obj) then
					local diff = npcRoot.Position - pos
					local flatDist = Vector2.new(diff.X, diff.Z).Magnitude
					if flatDist <= 40 then
						npcHum.Health = 0

						local dir = Vector3.new(diff.X, 0, diff.Z)
						dir = (dir.Magnitude > 0) and dir.Unit or Vector3.new(1, 0, 0)
						local launchDir = (dir + Vector3.new(0, 3.5, 0)).Unit
						local forceMult = math.clamp(1 - (flatDist / 40), 0.4, 1)

						npcRoot.AssemblyLinearVelocity = launchDir * (200 * forceMult)
						npcRoot.AssemblyAngularVelocity = Vector3.new(
							math.random(-50, 50),
							math.random(-50, 50),
							math.random(-50, 50)
						)
					end
				end
			end
		end
	end
end

local lastPos = Vector3.new()
local currentYaw = 0
local tiltX, tiltY, tiltZ, slopeTilt = 0, 0, 0, 0

local function ToggleSystems(on)
	systemActive = on
	if on and humanoid and root then
		humanoid.AutoRotate = false
		currentYaw = math.rad(root.Orientation.Y)
		lastPos = root.Position

		att = Instance.new("Attachment", root)
		align = Instance.new("AlignOrientation", root)
		align.Mode = Enum.OrientationAlignmentMode.OneAttachment
		align.Attachment0 = att
		align.MaxTorque = 200000
		align.Responsiveness = 35

		heartbeatConn = RunService.Heartbeat:Connect(function(dt)
			if not humanoid or not root or humanoid.Health <= 0 then return end

			local vel = (root.Position - lastPos) / math.max(dt, 1/60)
			lastPos = root.Position
			local moveDir = humanoid.MoveDirection

			if moveDir.Magnitude > 0.1 then
				local targetYaw = math.atan2(-moveDir.X, -moveDir.Z)
				currentYaw += math.atan2(math.sin(targetYaw - currentYaw), math.cos(targetYaw - currentYaw)) * dt * 12
			end

			local ray = Workspace:Raycast(root.Position, Vector3.new(0, -8, 0))
			local targetSlope = ray and -root.CFrame.LookVector:Dot(ray.Normal) * math.rad(35) or 0
			slopeTilt += (targetSlope - slopeTilt) * dt * 10

			tiltX += (-moveDir:Dot(root.CFrame.LookVector) * math.rad(40) * 0.5 - tiltX) * dt * 10
			tiltZ += (-moveDir:Dot(root.CFrame.RightVector) * math.rad(40) * 0.5 - tiltZ) * dt * 10
			tiltY += (math.clamp(vel.Y/50, -1, 1) * math.rad(25) - tiltY) * dt * 10

			align.CFrame = CFrame.Angles(0, currentYaw, 0) * CFrame.Angles(tiltY + slopeTilt, 0, 0) * CFrame.Angles(tiltX, 0, tiltZ)

			if moveDir.Magnitude > 0 then
				if not walkTrack.IsPlaying then walkTrack:Play(0.3) end
				walkTrack:AdjustSpeed(humanoid.WalkSpeed / 16)
				idle1:Stop(0.3); idle2:Stop(0.3); idle3:Stop(0.3)
			else
				walkTrack:Stop(0.3)
				if not idle1.IsPlaying then idle1:Play(0.3) end
				if not idle2.IsPlaying then
					idle2:Play(0.3)
					idle2.TimePosition = 1
					idle2:AdjustSpeed(0)
				end
				if not idle3.IsPlaying then idle3:Play(0.3) end
			end
		end)
	else
		if heartbeatConn then heartbeatConn:Disconnect() end
		if align then align:Destroy() end
		if att then att:Destroy() end
		if walkTrack then walkTrack:Stop(); idle1:Stop(); idle2:Stop(); idle3:Stop() end
		if humanoid then humanoid.AutoRotate = true end
	end
end

local tool = player.Backpack:FindFirstChild("overhand leap") or Instance.new("Tool")
tool.Name = "overhand leap"
tool.RequiresHandle = false
tool.Parent = player.Backpack

tool.Equipped:Connect(function() ToggleSystems(true) end)
tool.Unequipped:Connect(function() ToggleSystems(false) end)

tool.Activated:Connect(function()
	if debounce or not systemActive or not root then return end
	debounce = true

	local CHARGE_DURATION = 0.8
	local SLAM_CHANCE     = 0.45

	humanoid.WalkSpeed = 0
	soundCharge:Play()

	task.wait(CHARGE_DURATION)

	soundCharge:Stop()
	humanoid.WalkSpeed = 16

	soundJump:Play()

	local leapAnim = Instance.new("Animation")
	leapAnim.AnimationId = "rbxassetid://94160738"
	local track = animator:LoadAnimation(leapAnim)
	track:Play(0.2)
	track:AdjustSpeed(0.5)

	local bv = Instance.new("BodyVelocity")
	bv.Velocity = root.CFrame.LookVector * 45 + Vector3.new(0, 60, 0)
	bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
	bv.Parent = root
	Debris:AddItem(bv, 0.2)

	task.spawn(function()
		task.wait(0.3)
		while root.AssemblyLinearVelocity.Y > 0 do task.wait() end

		if math.random() < SLAM_CHANCE then
			soundSlam:Play()
		end

		while humanoid.FloorMaterial == Enum.Material.Air do task.wait() end
		TriggerImpact(root.Position)
	end)

	task.delay(cooldown, function() debounce = false end)
end)

if player.Character then LoadAssets(player.Character) end
player.CharacterAdded:Connect(LoadAssets)
