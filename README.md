--==================================================
-- JAMARII.CHAR
-- LADYBUG EDITION
-- MOBILE DRAG BUTTON + PC RIGHT SHIFT
--==================================================

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer 
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local UIS = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

--==================================================
-- SETTINGS
--==================================================

local IsMobile = UIS.TouchEnabled and not UIS.KeyboardEnabled

local targetParent =
    (gethui and gethui())
    or CoreGui
    or PlayerGui

if targetParent:FindFirstChild("JAMARIICharInterface") then
    targetParent.JAMARIICharInterface:Destroy()
end

-- Put your Roblox uploaded image IDs here
local LADYBUG_LOGO = "rbxassetid://105378864530063"
local LADYBUG_DRAG_ICON = "rbxassetid://113369600301454"

_G.UIVisible = true
_G.UIToggleKey = Enum.KeyCode.RightShift

local isAvatarEnabled = false
local isHeadlessEnabled = false
local targetUserInput = ""

--==================================================
-- LADYBUG THEME
--==================================================

local Theme = {
    Red = Color3.fromRGB(235, 25, 45),
    DarkRed = Color3.fromRGB(150, 10, 25),
    LightRed = Color3.fromRGB(255, 55, 70),

    Black = Color3.fromRGB(8, 8, 10),
    DarkBlack = Color3.fromRGB(15, 15, 18),

    White = Color3.fromRGB(255, 255, 255),
    Gray = Color3.fromRGB(155, 155, 160),
    DarkGray = Color3.fromRGB(35, 35, 40),

    Border = Color3.fromRGB(65, 10, 18),

    Button = Color3.fromRGB(25, 25, 28),
    ButtonHover = Color3.fromRGB(45, 15, 20)
}

--==================================================
-- SCREEN GUI
--==================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JAMARIICharInterface"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = targetParent

--==================================================
-- MOBILE DRAG BUTTON
--==================================================

local MobileToggleBtn

if IsMobile then

    MobileToggleBtn = Instance.new("ImageButton")
    MobileToggleBtn.Name = "MobileToggleBtn"

    MobileToggleBtn.Size = UDim2.new(0, 65, 0, 65)
    MobileToggleBtn.Position = UDim2.new(0.05, 0, 0.25, 0)

    MobileToggleBtn.BackgroundColor3 = Theme.Red
    MobileToggleBtn.BorderColor3 = Theme.Black
    MobileToggleBtn.BorderSizePixel = 3

    MobileToggleBtn.Image = LADYBUG_DRAG_ICON
    MobileToggleBtn.ScaleType = Enum.ScaleType.Crop

    MobileToggleBtn.ZIndex = 100
    MobileToggleBtn.Parent = ScreenGui

    local Circle = Instance.new("UICorner")
    Circle.CornerRadius = UDim.new(1, 0)
    Circle.Parent = MobileToggleBtn

    -- Dragging
    local dragging = false
    local dragStart
    local startPos
    local moved = false

    MobileToggleBtn.InputBegan:Connect(function(input)

        if input.UserInputType == Enum.UserInputType.Touch
            or input.UserInputType == Enum.UserInputType.MouseButton1 then

            dragging = true
            moved = false

            dragStart = input.Position
            startPos = MobileToggleBtn.Position

            input.Changed:Connect(function()

                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end

            end)
        end
    end)

    UIS.InputChanged:Connect(function(input)

        if not dragging then
            return
        end

        if input.UserInputType == Enum.UserInputType.Touch
            or input.UserInputType == Enum.UserInputType.MouseMovement then

            local delta = input.Position - dragStart

            if math.abs(delta.X) > 5 or math.abs(delta.Y) > 5 then
                moved = true
            end

            MobileToggleBtn.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,

                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)

end

--==================================================
-- MAIN WINDOW
--==================================================

local WindowOuterBorder = Instance.new("Frame")
WindowOuterBorder.Name = "WindowOuterBorder"

WindowOuterBorder.Size = UDim2.new(0, 520, 0, 360)
WindowOuterBorder.Position = UDim2.new(0.5, -260, 0.5, -180)

WindowOuterBorder.BackgroundColor3 = Theme.Red
WindowOuterBorder.BorderSizePixel = 0

WindowOuterBorder.Active = true
WindowOuterBorder.ClipsDescendants = true
WindowOuterBorder.Parent = ScreenGui

local OuterCorner = Instance.new("UICorner")
OuterCorner.CornerRadius = UDim.new(0, 12)
OuterCorner.Parent = WindowOuterBorder

--==================================================
-- INNER BACKGROUND
--==================================================

local MainBody = Instance.new("Frame")
MainBody.Name = "MainBody"

MainBody.Size = UDim2.new(1, -4, 1, -4)
MainBody.Position = UDim2.new(0, 2, 0, 2)

MainBody.BackgroundColor3 = Theme.Black
MainBody.BorderSizePixel = 0

MainBody.ClipsDescendants = true
MainBody.Parent = WindowOuterBorder

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainBody

--==================================================
-- HEADER
--==================================================

local HeaderBar = Instance.new("Frame")
HeaderBar.Name = "HeaderBar"

HeaderBar.Size = UDim2.new(1, 0, 0, 42)

HeaderBar.BackgroundColor3 = Theme.Black
HeaderBar.BorderSizePixel = 0

HeaderBar.ZIndex = 5
HeaderBar.Parent = MainBody

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 10)
HeaderCorner.Parent = HeaderBar

-- Ladybug logo

local LadybugLogo = Instance.new("ImageLabel")

LadybugLogo.Name = "LadybugLogo"
LadybugLogo.Size = UDim2.new(0, 30, 0, 30)
LadybugLogo.Position = UDim2.new(0, 8, 0.5, -15)

LadybugLogo.BackgroundTransparency = 1
LadybugLogo.Image = LADYBUG_LOGO
LadybugLogo.ScaleType = Enum.ScaleType.Fit

LadybugLogo.ZIndex = 10
LadybugLogo.Parent = HeaderBar

-- Title

local TitleText = Instance.new("TextLabel")

TitleText.Name = "TitleText"
TitleText.Size = UDim2.new(0.5, 0, 1, 0)
TitleText.Position = UDim2.new(0, 46, 0, 0)

TitleText.BackgroundTransparency = 1

-- CHANGED NAME
TitleText.Text = "JAMARII.CHAR"

TitleText.TextColor3 = Theme.Red
TitleText.TextSize = 14
TitleText.Font = Enum.Font.RobotoMono

TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.ZIndex = 6
TitleText.Parent = HeaderBar

-- Date

local DateText = Instance.new("TextLabel")

DateText.Name = "DateText"
DateText.Size = UDim2.new(0.5, -15, 1, 0)
DateText.Position = UDim2.new(0.5, 0, 0, 0)

DateText.BackgroundTransparency = 1
DateText.TextColor3 = Theme.Gray

DateText.TextSize = 11
DateText.Font = Enum.Font.RobotoMono
DateText.TextXAlignment = Enum.TextXAlignment.Right

DateText.ZIndex = 6
DateText.Parent = HeaderBar

local function updateClock()
    DateText.Text = os.date("%A, %d %B, %y")
end

updateClock()

task.spawn(function()
    while task.wait(60) do
        updateClock()
    end
end)

--==================================================
-- RED LINE
--==================================================

local RedLine = Instance.new("Frame")

RedLine.Name = "RedLine"

RedLine.Size = UDim2.new(1, 0, 0, 3)
RedLine.Position = UDim2.new(0, 0, 1, 0)

RedLine.BackgroundColor3 = Theme.Red
RedLine.BorderSizePixel = 0

RedLine.ZIndex = 10
RedLine.Parent = HeaderBar

--==================================================
-- POLKA DOT BACKGROUND
--==================================================

local DotFolder = Instance.new("Folder")
DotFolder.Name = "LadybugDots"
DotFolder.Parent = MainBody

local dotPositions = {
    {0.91, 0.10},
    {0.75, 0.17},
    {0.91, 0.32},
    {0.70, 0.43},
    {0.90, 0.55},
    {0.72, 0.72},
    {0.90, 0.84},
    {0.52, 0.88},
    {0.35, 0.92},
    {0.18, 0.85},
    {0.08, 0.68},
    {0.17, 0.48},
    {0.08, 0.28},
    {0.20, 0.14},
    {0.40, 0.08},
    {0.58, 0.14}
}

for _, pos in ipairs(dotPositions) do

    local Dot = Instance.new("Frame")

    Dot.Size = UDim2.new(0, 18, 0, 18)

    Dot.Position = UDim2.new(
        pos[1],
        -9,
        pos[2],
        -9
    )

    Dot.BackgroundColor3 = Theme.Black
    Dot.BorderSizePixel = 0

    Dot.BackgroundTransparency = 0.15

    Dot.ZIndex = 1
    Dot.Parent = DotFolder

    local DotCorner = Instance.new("UICorner")
    DotCorner.CornerRadius = UDim.new(1, 0)
    DotCorner.Parent = Dot
end

--==================================================
-- DRAG MAIN WINDOW
--==================================================

local dragging = false
local dragStart
local startPos

local function updateDrag(input)

    local delta = input.Position - dragStart

    WindowOuterBorder.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,

        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

HeaderBar.InputBegan:Connect(function(input)

    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true

        dragStart = input.Position
        startPos = WindowOuterBorder.Position

        input.Changed:Connect(function()

            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end

        end)
    end
end)

UIS.InputChanged:Connect(function(input)

    if dragging then

        if input.UserInputType == Enum.UserInputType.MouseMovement
            or input.UserInputType == Enum.UserInputType.Touch then

            updateDrag(input)
        end
    end
end)

--==================================================
-- CONTENT
--==================================================

local LayoutContent = Instance.new("Frame")

LayoutContent.Name = "LayoutContent"

LayoutContent.Size = UDim2.new(1, 0, 1, -42)
LayoutContent.Position = UDim2.new(0, 0, 0, 42)

LayoutContent.BackgroundTransparency = 1
LayoutContent.ClipsDescendants = true

LayoutContent.ZIndex = 2
LayoutContent.Parent = MainBody

--==================================================
-- SIDEBAR
--==================================================

local Sidebar = Instance.new("Frame")

Sidebar.Name = "Sidebar"

Sidebar.Size = UDim2.new(0, 120, 1, 0)

Sidebar.BackgroundColor3 = Theme.DarkBlack

Sidebar.BorderColor3 = Theme.Border
Sidebar.BorderSizePixel = 1

Sidebar.ZIndex = 3
Sidebar.Parent = LayoutContent

-- Avatar tab

local SidebarTab = Instance.new("TextButton")

SidebarTab.Size = UDim2.new(1, -16, 0, 30)
SidebarTab.Position = UDim2.new(0, 12, 0, 15)

SidebarTab.BackgroundTransparency = 1

SidebarTab.Text = "●  Avatar"

SidebarTab.TextColor3 = Theme.Red

SidebarTab.Font = Enum.Font.RobotoMono
SidebarTab.TextSize = 12

SidebarTab.TextXAlignment = Enum.TextXAlignment.Left

SidebarTab.Parent = Sidebar

--==================================================
-- WORKSPACE
--==================================================

local WorkspaceFrame = Instance.new("Frame")

WorkspaceFrame.Name = "WorkspaceFrame"

WorkspaceFrame.Size = UDim2.new(1, -120, 1, 0)
WorkspaceFrame.Position = UDim2.new(0, 120, 0, 0)

WorkspaceFrame.BackgroundTransparency = 1

WorkspaceFrame.ZIndex = 3
WorkspaceFrame.Parent = LayoutContent

local Padding = Instance.new("UIPadding")

Padding.PaddingTop = UDim.new(0, 20)
Padding.PaddingLeft = UDim.new(0, 20)
Padding.PaddingRight = UDim.new(0, 20)
Padding.PaddingBottom = UDim.new(0, 20)

Padding.Parent = WorkspaceFrame

local UIList = Instance.new("UIListLayout")

UIList.SortOrder = Enum.SortOrder.LayoutOrder
UIList.Padding = UDim.new(0, 14)

UIList.Parent = WorkspaceFrame

--==================================================
-- TOGGLE
--==================================================

local function CreateToggle(parent, text, defaultState, callback)

    local Row = Instance.new("Frame")

    Row.Size = UDim2.new(1, 0, 0, 20)
    Row.BackgroundTransparency = 1

    Row.Parent = parent

    local Box = Instance.new("TextButton")

    Box.Size = UDim2.new(0, 14, 0, 14)
    Box.Position = UDim2.new(0, 0, 0.5, -7)

    Box.BackgroundColor3 =
        defaultState and Theme.Red or Theme.Button

    Box.BorderColor3 =
        defaultState and Theme.Red or Theme.DarkGray

    Box.BorderSizePixel = 1
    Box.Text = ""

    Box.Parent = Row

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 3)
    Corner.Parent = Box

    local Label = Instance.new("TextLabel")

    Label.Size = UDim2.new(1, -24, 1, 0)
    Label.Position = UDim2.new(0, 24, 0, 0)

    Label.BackgroundTransparency = 1

    Label.Text = text

    Label.TextColor3 =
        defaultState and Theme.Red or Theme.Gray

    Label.TextSize = 11
    Label.Font = Enum.Font.RobotoMono

    Label.TextXAlignment = Enum.TextXAlignment.Left

    Label.Parent = Row

    local state = defaultState

    local function updateState(newState)

        state = newState

        Box.BackgroundColor3 =
            state and Theme.Red or Theme.Button

        Box.BorderColor3 =
            state and Theme.Red or Theme.DarkGray

        Label.TextColor3 =
            state and Theme.Red or Theme.Gray
    end

    Box.MouseButton1Click:Connect(function()

        updateState(not state)

        callback(state)
    end)

    return updateState
end

--==================================================
-- INPUT
--==================================================

local function CreateInputGroup(parent, labelText, defaultVal, callback)

    local Group = Instance.new("Frame")

    Group.Size = UDim2.new(1, 0, 0, 38)
    Group.BackgroundTransparency = 1

    Group.Parent = parent

    local Label = Instance.new("TextLabel")

    Label.Size = UDim2.new(1, 0, 0, 14)

    Label.BackgroundTransparency = 1

    Label.Text = labelText

    Label.TextColor3 = Theme.Gray
    Label.TextSize = 11
    Label.Font = Enum.Font.RobotoMono

    Label.TextXAlignment = Enum.TextXAlignment.Left

    Label.Parent = Group

    local TextBox = Instance.new("TextBox")

    TextBox.Size = UDim2.new(1, 0, 0, 22)

    TextBox.Position = UDim2.new(0, 0, 0, 16)

    TextBox.BackgroundColor3 = Theme.DarkBlack

    TextBox.BorderColor3 = Theme.Border
    TextBox.BorderSizePixel = 1

    TextBox.Text = defaultVal or ""

    TextBox.PlaceholderText = "Enter User ID or Username"

    TextBox.TextColor3 = Theme.Red
    TextBox.PlaceholderColor3 = Theme.Gray

    TextBox.Font = Enum.Font.RobotoMono
    TextBox.TextSize = 11

    TextBox.TextXAlignment = Enum.TextXAlignment.Left

    TextBox.Parent = Group

    local TextPadding = Instance.new("UIPadding")
    TextPadding.PaddingLeft = UDim.new(0, 7)
    TextPadding.Parent = TextBox

    TextBox:GetPropertyChangedSignal("Text"):Connect(function()
        callback(TextBox.Text)
    end)
end

--==================================================
-- CONTROLS
--==================================================

CreateToggle(
    WorkspaceFrame,
    "Enable Avatar",
    false,
    function(v)
        isAvatarEnabled = v
    end
)

CreateInputGroup(
    WorkspaceFrame,
    "Enter User ID or Username to Char",
    "",
    function(v)
        targetUserInput = v
    end
)

CreateToggle(
    WorkspaceFrame,
    "Visual Headless",
    false,
    function(v)
        isHeadlessEnabled = v
    end
)

--==================================================
-- SHOW UI
--==================================================

local updateShowUIToggle

updateShowUIToggle = CreateToggle(
    WorkspaceFrame,
    "Show UI",
    true,
    function(v)

        _G.UIVisible = v
        WindowOuterBorder.Visible = v

    end
)

--==================================================
-- TOGGLE UI
--==================================================

local function toggleUIState()

    _G.UIVisible = not _G.UIVisible

    WindowOuterBorder.Visible = _G.UIVisible

    if updateShowUIToggle then
        updateShowUIToggle(_G.UIVisible)
    end
end

--==================================================
-- MOBILE BUTTON
--==================================================

if IsMobile then

    local lastPosition

    MobileToggleBtn.MouseButton1Click:Connect(function()

        -- Only toggle when the button wasn't dragged
        local currentPosition = MobileToggleBtn.Position

        if lastPosition == nil then
            lastPosition = currentPosition
            toggleUIState()
            return
        end

        if math.abs(
            currentPosition.X.Offset - lastPosition.X.Offset
        ) < 5
        and math.abs(
            currentPosition.Y.Offset - lastPosition.Y.Offset
        ) < 5 then

            toggleUIState()
        end

        lastPosition = currentPosition
    end)

end

--==================================================
-- PC RIGHT SHIFT
--==================================================

if not IsMobile then

    UIS.InputBegan:Connect(function(input, gameProcessed)

        if gameProcessed then
            return
        end

        if input.KeyCode == Enum.KeyCode.RightShift then
            toggleUIState()
        end

    end)

end

--==================================================
-- APPLY BUTTON
--==================================================

local ApplyBtn = Instance.new("TextButton")

ApplyBtn.Size = UDim2.new(1, 0, 0, 30)

ApplyBtn.BackgroundColor3 = Theme.Button

ApplyBtn.BorderColor3 = Theme.Red
ApplyBtn.BorderSizePixel = 1

ApplyBtn.Text = "Apply & Reset Character"

ApplyBtn.TextColor3 = Theme.Red

ApplyBtn.Font = Enum.Font.RobotoMono
ApplyBtn.TextSize = 12

ApplyBtn.Parent = WorkspaceFrame

local ApplyCorner = Instance.new("UICorner")
ApplyCorner.CornerRadius = UDim.new(0, 4)
ApplyCorner.Parent = ApplyBtn

ApplyBtn.MouseButton1Down:Connect(function()

    ApplyBtn.BackgroundColor3 = Theme.Red
    ApplyBtn.TextColor3 = Theme.White

end)

ApplyBtn.MouseButton1Up:Connect(function()

    ApplyBtn.BackgroundColor3 = Theme.Button
    ApplyBtn.TextColor3 = Theme.Red

end)

--==================================================
-- USER ID
--==================================================

local function getUserIdFromInput(input)

    input = tostring(input)
        :gsub("%s+", "")
        :gsub("^@", "")

    if input == "" then
        return nil
    end

    local id = tonumber(input)

    if id then
        return id
    end

    local success, result = pcall(function()

        return Players:GetUserIdFromNameAsync(input)

    end)

    if success and result then
        return result
    end

    return nil
end

--==================================================
-- ANIMATIONS
--==================================================

local function applyAnimationsFromDummy(char, dummyModel)

    local myAnimate = char:FindFirstChild("Animate")
    local dummyAnimate = dummyModel:FindFirstChild("Animate")
    local humanoid = char:FindFirstChildOfClass("Humanoid")

    if not myAnimate or not dummyAnimate or not humanoid then
        return
    end

    local animator = humanoid:FindFirstChildOfClass("Animator")

    if animator then

        for _, track in ipairs(
            animator:GetPlayingAnimationTracks()
        ) do
            track:Stop(0)
        end

    end

    for _, dummyChild in ipairs(
        dummyAnimate:GetChildren()
    ) do

        local myChild =
            myAnimate:FindFirstChild(dummyChild.Name)

        if myChild then

            for _, animObj in ipairs(
                dummyChild:GetChildren()
            ) do

                if animObj:IsA("Animation") then

                    local target =
                        myChild:FindFirstChild(animObj.Name)

                    if target and target:IsA("Animation") then

                        target.AnimationId =
                            animObj.AnimationId

                    else

                        animObj:Clone().Parent = myChild

                    end
                end
            end
        end
    end

    myAnimate.Disabled = true

    task.wait(0.05)

    myAnimate.Disabled = false
end

--==================================================
-- APPLY AVATAR
--==================================================

local function applyFullOutfit(char)

    if not isAvatarEnabled
        or targetUserInput == "" then
        return
    end

    local id = getUserIdFromInput(targetUserInput)

    if not id then
        return
    end

    task.spawn(function()

        local humanoid =
            char:WaitForChild("Humanoid", 5)

        if not humanoid then
            return
        end

        local success, dummyModel = pcall(function()

            return Players:CreateHumanoidModelFromUserId(id)

        end)

        if not success or not dummyModel then
            return
        end

        -- Remove old clothing/accessories

        for _, obj in ipairs(char:GetChildren()) do

            if obj:IsA("Accessory")
                or obj:IsA("Shirt")
                or obj:IsA("Pants")
                or obj:IsA("ShirtGraphic")
                or obj:IsA("BodyColors")
                or obj:IsA("CharacterMesh") then

                obj:Destroy()
            end
        end

        -- Apply description

        pcall(function()

            local dummyHum =
                dummyModel:FindFirstChildOfClass("Humanoid")

            if dummyHum then

                local description =
                    dummyHum:GetAppliedDescription()

                if description then
                    humanoid:ApplyDescription(description)
                end
            end
        end)

        -- Body colors

        local targetColors =
            dummyModel:FindFirstChildOfClass("BodyColors")

        if targetColors then

            local oldColors =
                char:FindFirstChildOfClass("BodyColors")

            if oldColors then
                oldColors:Destroy()
            end

            targetColors:Clone().Parent = char
        end

        -- Clothing

        for _, item in ipairs(
            dummyModel:GetChildren()
        ) do

            if item:IsA("Shirt")
                or item:IsA("Pants")
                or item:IsA("ShirtGraphic")
                or item:IsA("CharacterMesh") then

                item:Clone().Parent = char
            end
        end

        -- Mesh parts

        for _, dPart in ipairs(
            dummyModel:GetChildren()
        ) do

            if dPart:IsA("MeshPart")
                and dPart.Name ~= "Head" then

                local myPart =
                    char:FindFirstChild(dPart.Name)

                if myPart and myPart:IsA("MeshPart") then

                    pcall(function()

                        myPart.MeshId = dPart.MeshId
                        myPart.TextureID = dPart.TextureID

                    end)
                end
            end
        end

        -- Head

        local targetHead =
            dummyModel:FindFirstChild("Head")

        local myHead =
            char:FindFirstChild("Head")

        if targetHead and myHead then

            for _, child in ipairs(
                myHead:GetChildren()
            ) do

                if child:IsA("SpecialMesh")
                    or child:IsA("CharacterMesh")
                    or child:IsA("Decal")
                    or child:IsA("Texture")
                    or child:IsA("SurfaceAppearance")
                    or child:IsA("WrapTarget") then

                    child:Destroy()
                end
            end

            myHead.Color = targetHead.Color

            local meshSuccess = false

            if targetHead:IsA("MeshPart")
                and myHead:IsA("MeshPart") then

                meshSuccess = pcall(function()

                    myHead.MeshId =
                        targetHead.MeshId

                    myHead.TextureID =
                        targetHead.TextureID

                end)
            end

            for _, child in ipairs(
                targetHead:GetChildren()
            ) do

                if child:IsA("SpecialMesh")
                    or child:IsA("Decal")
                    or child:IsA("SurfaceAppearance") then

                    child:Clone().Parent = myHead
                end
            end

            if targetHead:IsA("MeshPart")
                and not meshSuccess then

                myHead.Transparency = 1

                local fakeHead =
                    targetHead:Clone()

                fakeHead.Name = "FakeHead"
                fakeHead.CanCollide = false
                fakeHead.Parent = char

                local weld = Instance.new("Weld")

                weld.Name = "FakeHeadWeld"
                weld.Part0 = myHead
                weld.Part1 = fakeHead

                weld.C0 = CFrame.new()
                weld.C1 = CFrame.new()

                weld.Parent = fakeHead
            else

                myHead.Transparency =
                    targetHead.Transparency
            end
        end

        -- Accessories

        for _, item in ipairs(
            dummyModel:GetChildren()
        ) do

            if item:IsA("Accessory") then

                local accessory =
                    item:Clone()

                local handle =
                    accessory:FindFirstChild("Handle")

                if handle then

                    handle.CanCollide = false

                    accessory.Parent = char

                    local attachment =
                        handle:FindFirstChildOfClass(
                            "Attachment"
                        )

                    if attachment then

                        local targetAttachment

                        for _, part in ipairs(
                            char:GetChildren()
                        ) do

                            if part:IsA("BasePart") then

                                local att =
                                    part:FindFirstChild(
                                        attachment.Name
                                    )

                                if att and
                                    att:IsA("Attachment") then

                                    targetAttachment = att
                                    break
                                end
                            end
                        end

                        if targetAttachment then

                            local weld =
                                Instance.new("Weld")

                            weld.Name =
                                "AccessoryWeld"

                            weld.Part0 = handle
                            weld.Part1 =
                                targetAttachment.Parent

                            weld.C0 =
                                attachment.CFrame

                            weld.C1 =
                                targetAttachment.CFrame

                            weld.Parent = handle
                        end

                    else

                        local head =
                            char:FindFirstChild("Head")

                        if head then

                            local weld =
                                Instance.new("Weld")

                            weld.Name =
                                "AccessoryWeld"

                            weld.Part0 = handle
                            weld.Part1 = head

                            weld.C0 = CFrame.new()
                            weld.C1 = CFrame.new()

                            weld.Parent = handle
                        end
                    end
                end
            end
        end

        applyAnimationsFromDummy(
            char,
            dummyModel
        )

        -- Headless

        if isHeadlessEnabled then

            task.wait(0.1)

            local head =
                char:FindFirstChild("Head")

            if head then

                head.Transparency = 1

                local face =
                    head:FindFirstChildOfClass("Decal")

                if face then
                    face.Transparency = 1
                end

                local fakeHead =
                    char:FindFirstChild("FakeHead")

                if fakeHead then
                    fakeHead.Transparency = 1
                end
            end
        end

        dummyModel:Destroy()

    end)
end

--==================================================
-- RESET
--==================================================

local function resetCharacter()

    local char =
        LocalPlayer.Character

    if not char then
        return
    end

    local humanoid =
        char:FindFirstChildOfClass("Humanoid")

    if humanoid then

        humanoid.Health = 0

    else

        char:BreakJoints()

    end
end

--==================================================
-- RESPAWN
--==================================================

LocalPlayer.CharacterAdded:Connect(function(newChar)

    if isAvatarEnabled
        and targetUserInput ~= "" then

        task.wait(0.5)

        applyFullOutfit(newChar)
    end

end)

--==================================================
-- APPLY
--==================================================

ApplyBtn.MouseButton1Click:Connect(function()

    resetCharacter()

end)
