--// SSVIRUS PANEL - Complete UI Edition

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local LOGO_ID = "rbxassetid://130910831750255"

--==================================================
-- THEME
--==================================================

local C = {
	bg = Color3.fromRGB(9, 13, 11),
	header = Color3.fromRGB(11, 24, 16),
	card = Color3.fromRGB(16, 28, 21),
	cardHover = Color3.fromRGB(22, 42, 30),
	accent = Color3.fromRGB(35, 255, 95),
	accentDark = Color3.fromRGB(20, 150, 55),
	stroke = Color3.fromRGB(45, 140, 75),
	text = Color3.fromRGB(255, 255, 255),
	muted = Color3.fromRGB(135, 165, 145),
	success = Color3.fromRGB(35, 255, 95),
	fail = Color3.fromRGB(255, 70, 90),
}

local scriptsList = {
	{
		name = "Example",
		desc = "Example local action",
		callback = function()
			print("Example executed")
		end,
	},
}

local oldGui = playerGui:FindFirstChild("SSVIRUS_PANEL")
if oldGui then oldGui:Destroy() end

local function make(className, props, parent)
	local object = Instance.new(className)
	for property, value in pairs(props or {}) do
		object[property] = value
	end
	object.Parent = parent
	return object
end

local function round(parent, radius)
	return make("UICorner", {CornerRadius = UDim.new(0, radius)}, parent)
end

local function outline(parent, color, thickness, transparency)
	return make("UIStroke", {
		Color = color,
		Thickness = thickness or 1,
		Transparency = transparency or 0.4,
		ApplyStrokeMode = Enum.ApplyStrokeMode.Border,
	}, parent)
end

local function tween(object, duration, properties, style, direction)
	local animation = TweenService:Create(object, TweenInfo.new(
		duration,
		style or Enum.EasingStyle.Quad,
		direction or Enum.EasingDirection.Out
	), properties)
	animation:Play()
	return animation
end

local gui = make("ScreenGui", {
	Name = "SSVIRUS_PANEL",
	IgnoreGuiInset = true,
	ResetOnSpawn = false,
	ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
}, playerGui)

local rgbObjects = {}
local spinners = {}
local hue = 0

local function addRGB(object, property)
	if object then
		table.insert(rgbObjects, {obj = object, prop = property or "TextColor3"})
	end
end

local function addSpinner(object, speed)
	if object then
		table.insert(spinners, {obj = object, speed = speed or 90})
	end
end

local heartbeatConnection = RunService.Heartbeat:Connect(function(deltaTime)
	hue = (hue + deltaTime * 0.25) % 1
	local color = Color3.fromHSV(hue, 0.85, 1)

	for i = #rgbObjects, 1, -1 do
		local item = rgbObjects[i]
		if item.obj and item.obj.Parent then
			pcall(function()
				item.obj[item.prop] = color
			end)
		else
			table.remove(rgbObjects, i)
		end
	end

	for i = #spinners, 1, -1 do
		local item = spinners[i]
		if item.obj and item.obj.Parent then
			item.obj.Rotation = (item.obj.Rotation + deltaTime * item.speed) % 360
		else
			table.remove(spinners, i)
		end
	end
end)

gui.Destroying:Connect(function()
	if heartbeatConnection then
		heartbeatConnection:Disconnect()
		heartbeatConnection = nil
	end
end)

--==================================================
-- LOADING SCREEN
--==================================================

local loading = make("Frame", {
	Size = UDim2.fromScale(1, 1),
	BackgroundColor3 = Color3.fromRGB(4, 6, 5),
	BorderSizePixel = 0,
	ZIndex = 50,
}, gui)

make("UIGradient", {
	Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(6, 22, 12)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(2, 3, 3)),
	}),
	Rotation = 90,
}, loading)

local glow = make("Frame", {
	Size = UDim2.fromOffset(240, 240),
	AnchorPoint = Vector2.new(0.5, 0.5),
	Position = UDim2.fromScale(0.5, 0.4),
	BackgroundColor3 = C.accent,
	BackgroundTransparency = 0.88,
	BorderSizePixel = 0,
	ZIndex = 51,
}, loading)

round(glow, 120)

task.spawn(function()
	while glow.Parent do
		tween(glow, 1.1, {
			Size = UDim2.fromOffset(270, 270),
			BackgroundTransparency = 0.94,
		}, Enum.EasingStyle.Sine)
		task.wait(1.1)
		if not glow.Parent then break end
		tween(glow, 1.1, {
			Size = UDim2.fromOffset(230, 230),
			BackgroundTransparency = 0.86,
		}, Enum.EasingStyle.Sine)
		task.wait(1.1)
	end
end)

local loadLogo = make("ImageLabel", {
	Size = UDim2.fromOffset(150, 150),
	AnchorPoint = Vector2.new(0.5, 0.5),
	Position = UDim2.fromScale(0.5, 0.4),
	BackgroundTransparency = 1,
	Image = LOGO_ID,
	ScaleType = Enum.ScaleType.Fit,
	ZIndex = 52,
}, loading)

addSpinner(loadLogo, 100)

local loadTitle = make("TextLabel", {
	Size = UDim2.new(1, 0, 0, 40),
	AnchorPoint = Vector2.new(0.5, 0),
	Position = UDim2.new(0.5, 0, 0.4, 100),
	BackgroundTransparency = 1,
	Text = "SSVIRUS",
	TextColor3 = C.accent,
	TextSize = 32,
	Font = Enum.Font.GothamBlack,
	ZIndex = 52,
}, loading)

make("UIGradient", {
	Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(35, 255, 95)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(120, 255, 200)),
	}),
}, loadTitle)

local loadStatus = make("TextLabel", {
	Size = UDim2.new(1, 0, 0, 20),
	Position = UDim2.new(0, 0, 0.4, 145),
	BackgroundTransparency = 1,
	Text = "Initializing...",
	TextColor3 = C.muted,
	TextSize = 13,
	Font = Enum.Font.Gotham,
	ZIndex = 52,
}, loading)

local barBack = make("Frame", {
	Size = UDim2.fromOffset(260, 8),
	AnchorPoint = Vector2.new(0.5, 0),
	Position = UDim2.new(0.5, 0, 0.4, 180),
	BackgroundColor3 = Color3.fromRGB(20, 30, 24),
	BorderSizePixel = 0,
	ZIndex = 52,
}, loading)

round(barBack, 8)
outline(barBack, C.stroke, 1, 0.6)

local barFill = make("Frame", {
	Size = UDim2.fromScale(0, 1),
	BackgroundColor3 = C.accent,
	BorderSizePixel = 0,
	ZIndex = 53,
}, barBack)

round(barFill, 8)

local percent = make("TextLabel", {
	Size = UDim2.new(1, 0, 0, 18),
	Position = UDim2.new(0, 0, 0.4, 196),
	BackgroundTransparency = 1,
	Text = "0%",
	TextColor3 = C.text,
	TextSize = 12,
	Font = Enum.Font.GothamBold,
	ZIndex = 52,
}, loading)

local stages = {
	{0, "Initializing..."},
	{25, "Loading modules..."},
	{55, "Preparing interface..."},
	{85, "Almost ready..."},
	{100, "Welcome!"},
}

for i = 1, 100 do
	barFill.Size = UDim2.fromScale(i / 100, 1)
	percent.Text = i .. "%"
	for _, stage in ipairs(stages) do
		if i >= stage[1] then
			loadStatus.Text = stage[2]
		end
	end
	task.wait(i < 30 and 0.012 or (i < 80 and 0.018 or 0.01))
end

task.wait(0.2)

--==================================================
-- MAIN PANEL
--==================================================

local panel = make("Frame", {
	Size = UDim2.fromOffset(400, 380),
	AnchorPoint = Vector2.new(0.5, 0.5),
	Position = UDim2.fromScale(0.5, 0.5),
	BackgroundColor3 = C.bg,
	BackgroundTransparency = 0.05,
	BorderSizePixel = 0,
	ZIndex = 10,
}, gui)

round(panel, 18)
outline(panel, C.stroke, 1.5, 0.3)

make("UIGradient", {
	Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(14, 24, 18)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(6, 9, 7)),
	}),
	Rotation = 90,
}, panel)

local panelScale = make("UIScale", {Scale = 0.85}, panel)

--==================================================
-- HEADER
--==================================================

local header = make("Frame", {
	Size = UDim2.new(1, 0, 0, 58),
	BackgroundColor3 = C.header,
	BackgroundTransparency = 0.1,
	BorderSizePixel = 0,
}, panel)

round(header, 18)
outline(header, C.stroke, 1, 0.65)

local headerLogo = make("ImageLabel", {
	Size = UDim2.fromOffset(38, 38),
	Position = UDim2.fromOffset(12, 10),
	BackgroundTransparency = 1,
	Image = LOGO_ID,
	ScaleType = Enum.ScaleType.Fit,
}, header)

addSpinner(headerLogo, 90)

make("TextLabel", {
	Size = UDim2.new(1, -140, 0, 24),
	Position = UDim2.fromOffset(60, 9),
	BackgroundTransparency = 1,
	Text = "SSVIRUS",
	TextColor3 = C.text,
	TextSize = 18,
	Font = Enum.Font.GothamBlack,
	TextXAlignment = Enum.TextXAlignment.Left,
}, header)

make("TextLabel", {
	Size = UDim2.new(1, -140, 0, 16),
	Position = UDim2.fromOffset(60, 32),
	BackgroundTransparency = 1,
	Text = "SSVIRUS TEAM",
	TextColor3 = C.muted,
	TextSize = 10,
	Font = Enum.Font.Gotham,
	TextXAlignment = Enum.TextXAlignment.Left,
}, header)

local function headerButton(text, x, textSize)
	local button = make("TextButton", {
		Size = UDim2.fromOffset(28, 28),
		Position = UDim2.new(1, x, 0, 15),
		BackgroundColor3 = C.card,
		BorderSizePixel = 0,
		Text = text,
		TextColor3 = C.text,
		TextSize = textSize,
		Font = Enum.Font.GothamBold,
		AutoButtonColor = false,
	}, header)

	round(button, 8)
	outline(button, C.stroke, 1, 0.5)

	button.MouseEnter:Connect(function()
		tween(button, 0.15, {BackgroundColor3 = C.cardHover})
	end)

	button.MouseLeave:Connect(function()
		tween(button, 0.15, {BackgroundColor3 = C.card})
	end)

	return button
end

local closeButton = headerButton("×", -38, 20)
local minimizeButton = headerButton("—", -72, 15)

--==================================================
-- TABS
--==================================================

local tabBar = make("Frame", {
	Size = UDim2.new(1, -24, 0, 38),
	Position = UDim2.fromOffset(12, 68),
	BackgroundColor3 = C.card,
	BackgroundTransparency = 0.15,
	BorderSizePixel = 0,
}, panel)

round(tabBar, 10)
outline(tabBar, C.stroke, 1, 0.6)

local indicator = make("Frame", {
	Size = UDim2.new(0.5, -6, 1, -6),
	Position = UDim2.new(0, 3, 0, 3),
	BackgroundColor3 = C.accentDark,
	BorderSizePixel = 0,
}, tabBar)

round(indicator, 8)

local function tabButton(text, xScale)
	return make("TextButton", {
		Size = UDim2.fromScale(0.5, 1),
		Position = UDim2.fromScale(xScale, 0),
		BackgroundTransparency = 1,
		Text = text,
		TextColor3 = C.text,
		TextSize = 13,
		Font = Enum.Font.GothamBold,
		AutoButtonColor = false,
		ZIndex = 2,
	}, tabBar)
end

local scriptsTab = tabButton("Scripts", 0)
local infoTab = tabButton("Info", 0.5)

--==================================================
-- PAGES
--==================================================

local pages = make("Frame", {
	Size = UDim2.new(1, -24, 1, -118),
	Position = UDim2.fromOffset(12, 112),
	BackgroundTransparency = 1,
	ClipsDescendants = true,
}, panel)

local scriptsPage = make("Frame", {
	Size = UDim2.fromScale(1, 1),
	Position = UDim2.fromScale(0, 0),
	BackgroundTransparency = 1,
}, pages)

local infoPage = make("Frame", {
	Size = UDim2.fromScale(1, 1),
	Position = UDim2.fromScale(1, 0),
	BackgroundTransparency = 1,
}, pages)

local function executeScript(scriptData)
	if not scriptData.callback then
		warn("No local callback defined for:", scriptData.name)
		return false
	end

	local success, err = pcall(function()
		scriptData.callback()
	end)

	if success then
		print("✅ " .. scriptData.name .. " executed")
		return true
	else
		warn("❌ Error in " .. scriptData.name .. ": " .. tostring(err))
		return false
	end
end

--==================================================
-- SCRIPTS PAGE
--==================================================

if #scriptsList == 0 then
	local emptyIcon = make("ImageLabel", {
		Size = UDim2.fromOffset(70, 70),
		AnchorPoint = Vector2.new(0.5, 0.5),
		Position = UDim2.new(0.5, 0, 0.5, -40),
		BackgroundTransparency = 1,
		Image = LOGO_ID,
		ImageTransparency = 0.7,
		ScaleType = Enum.ScaleType.Fit,
	}, scriptsPage)

	addSpinner(emptyIcon, 30)

	make("TextLabel", {
		Size = UDim2.new(1, 0, 0, 22),
		AnchorPoint = Vector2.new(0.5, 0.5),
		Position = UDim2.new(0.5, 0, 0.5, 20),
		BackgroundTransparency = 1,
		Text = "No scripts available",
		TextColor3 = C.muted,
		TextSize = 14,
		Font = Enum.Font.GothamBold,
	}, scriptsPage)
else
	local scroll = make("ScrollingFrame", {
		Size = UDim2.fromScale(1, 1),
		BackgroundTransparency = 1,
		BorderSizePixel = 0,
		ScrollBarThickness = 4,
		ScrollBarImageColor3 = C.accent,
		CanvasSize = UDim2.new(0, 0, 0, 0),
		AutomaticCanvasSize = Enum.AutomaticSize.Y,
	}, scriptsPage)

	make("UIListLayout", {
		Padding = UDim.new(0, 8),
		SortOrder = Enum.SortOrder.LayoutOrder,
	}, scroll)

	make("UIPadding", {
		PaddingTop = UDim.new(0, 4),
		PaddingBottom = UDim.new(0, 4),
	}, scroll)

	for _, scriptData in ipairs(scriptsList) do
		local card = make("TextButton", {
			Size = UDim2.new(1, -8, 0, 62),
			BackgroundColor3 = C.card,
			BorderSizePixel = 0,
			Text = "",
			AutoButtonColor = false,
		}, scroll)

		round(card, 12)
		outline(card, C.stroke, 1.2, 0.4)

		make("TextLabel", {
			Size = UDim2.new(1, -90, 0, 22),
			Position = UDim2.fromOffset(14, 10),
			BackgroundTransparency = 1,
			Text = scriptData.name,
			TextColor3 = C.text,
			TextSize = 15,
			Font = Enum.Font.GothamBold,
			TextXAlignment = Enum.TextXAlignment.Left,
		}, card)

		make("TextLabel", {
			Size = UDim2.new(1, -90, 0, 18),
			Position = UDim2.fromOffset(14, 32),
			BackgroundTransparency = 1,
			Text = scriptData.desc,
			TextColor3 = C.muted,
			TextSize = 11,
			Font = Enum.Font.Gotham,
			TextXAlignment = Enum.TextXAlignment.Left,
		}, card)

		local execBtn = make("TextButton", {
			Size = UDim2.fromOffset(70, 30),
			AnchorPoint = Vector2.new(1, 0.5),
			Position = UDim2.new(1, -12, 0.5, 0),
			BackgroundColor3 = C.accentDark,
			BorderSizePixel = 0,
			Text = "Execute",
			TextColor3 = C.text,
			TextSize = 12,
			Font = Enum.Font.GothamBold,
			AutoButtonColor = false,
		}, card)

		round(execBtn, 8)
		outline(execBtn, C.accent, 1, 0.3)

		card.MouseEnter:Connect(function()
			tween(card, 0.15, {BackgroundColor3 = C.cardHover})
		end)

		card.MouseLeave:Connect(function()
			tween(card, 0.15, {BackgroundColor3 = C.card})
		end)

		execBtn.MouseEnter:Connect(function()
			tween(execBtn, 0.15, {BackgroundColor3 = C.accent})
		end)

		execBtn.MouseLeave:Connect(function()
			tween(execBtn, 0.15, {BackgroundColor3 = C.accentDark})
		end)

		execBtn.MouseButton1Click:Connect(function()
			execBtn.Text = "..."
			local success = executeScript(scriptData)

			if success then
				execBtn.Text = "Done"
				task.wait(0.6)
				if execBtn.Parent then execBtn.Text = "Execute" end
			else
				execBtn.Text = "Error"
				task.wait(0.8)
				if execBtn.Parent then execBtn.Text = "Execute" end
			end
		end)
	end
end

--==================================================
-- INFO PAGE
--==================================================

local infoLogo = make("ImageLabel", {
	Size = UDim2.fromOffset(64, 64),
	AnchorPoint = Vector2.new(0.5, 0),
	Position = UDim2.new(0.5, 0, 0, 4),
	BackgroundTransparency = 1,
	Image = LOGO_ID,
	ScaleType = Enum.ScaleType.Fit,
}, infoPage)

addSpinner(infoLogo, 60)

local infoTitle = make("TextLabel", {
	Size = UDim2.new(1, 0, 0, 24),
	Position = UDim2.fromOffset(0, 72),
	BackgroundTransparency = 1,
	Text = "SSVIRUS TEAM",
	TextColor3 = C.accent,
	TextSize = 18,
	Font = Enum.Font.GothamBlack,
}, infoPage)

addRGB(infoTitle)

local function infoCard(y, label, name)
	local card = make("Frame", {
		Size = UDim2.new(1, -6, 0, 50),
		Position = UDim2.fromOffset(0, y),
		BackgroundColor3 = C.card,
		BorderSizePixel = 0,
	}, infoPage)

	round(card, 12)
	outline(card, C.stroke, 1.2, 0.35)

	make("TextLabel", {
		Size = UDim2.fromScale(1, 1),
		BackgroundTransparency = 1,
		RichText = true,
		Text = string.format(
			'<font color="rgb(135,165,145)">%s</font>  <font color="rgb(35,255,95)"><b>%s</b></font>',
			label,
			name
		),
		TextColor3 = C.text,
		TextSize = 17,
		Font = Enum.Font.GothamMedium,
	}, card)
end

infoCard(106, "Owner :", "C00LWHITE")
infoCard(164, "Owner 2 :", "SSDEVIL")
infoCard(222, "Owner 3 :", "Sskid")

--==================================================
-- LOADING FADE
--==================================================

local fadeDuration = 0.55

for _, object in ipairs(loading:GetDescendants()) do
	if object:IsA("TextLabel") or object:IsA("TextButton") then
		tween(object, fadeDuration, {TextTransparency = 1})
	elseif object:IsA("ImageLabel") or object:IsA("ImageButton") then
		tween(object, fadeDuration, {ImageTransparency = 1})
	elseif object:IsA("Frame") then
		tween(object, fadeDuration, {BackgroundTransparency = 1})
	elseif object:IsA("UIStroke") then
		tween(object, fadeDuration, {Transparency = 1})
	end
end

tween(loading, fadeDuration, {BackgroundTransparency = 1})
tween(panelScale, 0.5, {Scale = 1}, Enum.EasingStyle.Back)

task.wait(fadeDuration)

if loading and loading.Parent then loading:Destroy() end

--==================================================
-- TAB SWITCHING
--==================================================

local currentTab = "scripts"

local function switchTab(which)
	if which == currentTab then return end

	currentTab = which
	local showingInfo = which == "info"

	tween(indicator, 0.3, {
		Position = UDim2.new(showingInfo and 0.5 or 0, 3, 0, 3),
	}, Enum.EasingStyle.Quint)

	tween(scriptsPage, 0.35, {
		Position = UDim2.fromScale(showingInfo and -1 or 0, 0),
	}, Enum.EasingStyle.Quint)

	tween(infoPage, 0.35, {
		Position = UDim2.fromScale(showingInfo and 0 or 1, 0),
	}, Enum.EasingStyle.Quint)
end

scriptsTab.MouseButton1Click:Connect(function()
	switchTab("scripts")
end)

infoTab.MouseButton1Click:Connect(function()
	switchTab("info")
end)

--==================================================
-- CLOSE
--==================================================

closeButton.MouseButton1Click:Connect(function()
	tween(panelScale, 0.25, {
		Scale = 0.8,
	}, Enum.EasingStyle.Back, Enum.EasingDirection.In)

	for _, object in ipairs(panel:GetDescendants()) do
		if object:IsA("TextLabel") or object:IsA("TextButton") then
			tween(object, 0.2, {TextTransparency = 1})
		elseif object:IsA("ImageLabel") or object:IsA("ImageButton") then
			tween(object, 0.2, {ImageTransparency = 1})
		elseif object:IsA("Frame") then
			tween(object, 0.2, {BackgroundTransparency = 1})
		elseif object:IsA("UIStroke") then
			tween(object, 0.2, {Transparency = 1})
		end
	end

	task.wait(0.25)

	if gui and gui.Parent then gui:Destroy() end
end)

--==================================================
-- MINIMIZE
--==================================================

local minimized = false

minimizeButton.MouseButton1Click:Connect(function()
	minimized = not minimized

	if minimized then
		minimizeButton.Text = "+"

		tween(panelScale, 0.3, {
			Scale = 0.85,
		}, Enum.EasingStyle.Back)

		tween(panel, 0.3, {
			Size = UDim2.fromOffset(400, 78),
		}, Enum.EasingStyle.Quint)

		tabBar.Visible = false
		pages.Visible = false
	else
		minimizeButton.Text = "—"

		tabBar.Visible = true
		pages.Visible = true

		tween(panelScale, 0.3, {
			Scale = 1,
		}, Enum.EasingStyle.Back)

		tween(panel, 0.3, {
			Size = UDim2.fromOffset(400, 380),
		}, Enum.EasingStyle.Quint)
	end
end)

--==================================================
-- DRAGGING
--==================================================

local dragging = false
local dragStart
local startPosition

header.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPosition = panel.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if not dragging then return end

	if input.UserInputType ~= Enum.UserInputType.MouseMovement
		and input.UserInputType ~= Enum.UserInputType.Touch then
		return
	end

	local delta = input.Position - dragStart

	panel.Position = UDim2.new(
		startPosition.X.Scale,
		startPosition.X.Offset + delta.X,
		startPosition.Y.Scale,
		startPosition.Y.Offset + delta.Y
	)
end)

print("SSVIRUS PANEL loaded successfully.")
