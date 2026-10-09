local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

local mainGui = Instance.new("ScreenGui")
mainGui.Name = "HubGui"
mainGui.ResetOnSpawn = false
mainGui.DisplayOrder = 50
pcall(function()
    mainGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end)

local selectedLanguage = "ar"

local function addButtonHover(button)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = button

    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = Color3.fromRGB(50, 50, 70)
        }):Play()
    end)

    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {
            BackgroundColor3 = Color3.fromRGB(30, 30, 40)
        }):Play()
    end)
end

local function showToast(message)
    local toast = Instance.new("TextLabel")
    toast.Size = UDim2.new(0, 250, 0, 50)
    toast.Position = UDim2.new(1, -265, 1, -70)
    toast.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    toast.BackgroundTransparency = 0.1
    toast.Text = "✨ " .. message
    toast.TextColor3 = Color3.fromRGB(255, 255, 255)
    toast.TextSize = 13
    toast.Font = Enum.Font.GothamBold
    toast.Parent = mainGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = toast

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(80, 80, 120)
    stroke.Thickness = 1
    stroke.Parent = toast

    task.delay(3, function()
        if toast and toast.Parent then
            toast:Destroy()
        end
    end)
end

local openMainMenu

-- دالة لإنشاء زر دائري عائم برمز مخصص
local function createFloatingCircle(iconText, onOpenCallback)
    local floatBtn = Instance.new("TextButton")
    floatBtn.Size = UDim2.new(0, 45, 0, 45)
    floatBtn.Position = UDim2.new(0.05, 0, 0.4, 0)
    floatBtn.BackgroundColor3 = Color3.fromRGB(155, 89, 182)
    floatBtn.Text = iconText
    floatBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    floatBtn.TextSize = 20
    floatBtn.Font = Enum.Font.GothamBold
    floatBtn.Active = true
    floatBtn.Draggable = true
    floatBtn.Parent = mainGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = floatBtn

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(255, 255, 255)
    stroke.Thickness = 2
    stroke.Parent = floatBtn

    floatBtn.MouseButton1Click:Connect(function()
        floatBtn:Destroy()
        onOpenCallback()
    end)

    return floatBtn
end

-- 1. واجهة اختيار اللغة
local function createLanguageGUI()
    local languageFrame = Instance.new("Frame")
    languageFrame.Size = UDim2.new(0, 340, 0, 210)
    languageFrame.Position = UDim2.new(0.5, -170, 0.4, -105)
    languageFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    languageFrame.BorderSizePixel = 0
    languageFrame.Active = true
    languageFrame.Draggable = true
    languageFrame.Parent = mainGui
    languageFrame.BackgroundTransparency = 0.1

    local frameCorner = Instance.new("UICorner")
    frameCorner.CornerRadius = UDim.new(0, 16)
    frameCorner.Parent = languageFrame

    local frameStroke = Instance.new("UIStroke")
    frameStroke.Color = Color3.fromRGB(70, 70, 95)
    frameStroke.Thickness = 2
    frameStroke.Parent = languageFrame

    local langMiniBtn = Instance.new("TextButton")
    langMiniBtn.Size = UDim2.new(0, 26, 0, 26)
    langMiniBtn.Position = UDim2.new(1, -32, 0, 10)
    langMiniBtn.BackgroundColor3 = Color3.fromRGB(155, 89, 182)
    langMiniBtn.Text = "-"
    langMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    langMiniBtn.TextSize = 16
    langMiniBtn.Font = Enum.Font.GothamBold
    langMiniBtn.Parent = languageFrame

    local langCorner = Instance.new("UICorner")
    langCorner.CornerRadius = UDim.new(1, 0)
    langCorner.Parent = langMiniBtn

    langMiniBtn.MouseButton1Click:Connect(function()
        languageFrame:Destroy()
        createFloatingCircle("🌐", function()
            createLanguageGUI()
        end)
    end)

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 50)
    titleLabel.Position = UDim2.new(0, 0, 0, 10)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "🌐 Select Language / اختر اللغة"
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.TextSize = 15
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.Parent = languageFrame

    local subtitleLabel = Instance.new("TextLabel")
    subtitleLabel.Size = UDim2.new(1, 0, 0, 40)
    subtitleLabel.Position = UDim2.new(0, 0, 0, 60)
    subtitleLabel.BackgroundTransparency = 1
    subtitleLabel.Text = "Choose your preferred language:\nاختر لغتك المفضلة:"
    subtitleLabel.TextColor3 = Color3.fromRGB(170, 170, 190)
    subtitleLabel.TextSize = 12
    subtitleLabel.Font = Enum.Font.Gotham
    subtitleLabel.Parent = languageFrame

    local englishBtn = Instance.new("TextButton")
    englishBtn.Size = UDim2.new(0.85, 0, 0, 42)
    englishBtn.Position = UDim2.new(0.075, 0, 0, 110)
    englishBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    englishBtn.Text = "🏴󠁧󠁢󠁥󠁮󠁧󠁿 English"
    englishBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    englishBtn.TextSize = 15
    englishBtn.Font = Enum.Font.GothamBold
    englishBtn.Parent = languageFrame
    addButtonHover(englishBtn)

    local arabicBtn = Instance.new("TextButton")
    arabicBtn.Size = UDim2.new(0.85, 0, 0, 42)
    arabicBtn.Position = UDim2.new(0.075, 0, 0, 158)
    arabicBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    arabicBtn.Text = "🇸🇦 العربية"
    arabicBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    arabicBtn.TextSize = 15
    arabicBtn.Font = Enum.Font.GothamBold
    arabicBtn.Parent = languageFrame
    addButtonHover(arabicBtn)

    englishBtn.MouseButton1Click:Connect(function()
        selectedLanguage = "en"
        languageFrame:Destroy()
        showToast("English language selected")
        openMainMenu()
    end)

    arabicBtn.MouseButton1Click:Connect(function()
        selectedLanguage = "ar"
        languageFrame:Destroy()
        showToast("تم اختيار اللغة العربية بنجاح")
        openMainMenu()
    end)
end

createLanguageGUI()

local function setupAutoGUI(autoMode, stompEnabled)
    local autoFrame = Instance.new("Frame")
    autoFrame.Size = UDim2.new(0, 250, 0, stompEnabled and 185 or 135)
    autoFrame.Position = UDim2.new(0.03, 0, 0.5, 0)
    autoFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    autoFrame.BorderSizePixel = 0
    autoFrame.Active = true
    autoFrame.Draggable = true
    autoFrame.Parent = mainGui
    autoFrame.BackgroundTransparency = 0.1

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 14)
    corner.Parent = autoFrame

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(70, 70, 95)
    stroke.Thickness = 1.5
    stroke.Parent = autoFrame

    local autoMiniBtn = Instance.new("TextButton")
    autoMiniBtn.Size = UDim2.new(0, 24, 0, 24)
    autoMiniBtn.Position = UDim2.new(1, -28, 0, 4)
    autoMiniBtn.BackgroundColor3 = Color3.fromRGB(155, 89, 182)
    autoMiniBtn.Text = "-"
    autoMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    autoMiniBtn.TextSize = 15
    autoMiniBtn.Font = Enum.Font.GothamBold
    autoMiniBtn.Parent = autoFrame

    local autoMiniCorner = Instance.new("UICorner")
    autoMiniCorner.CornerRadius = UDim.new(1, 0)
    autoMiniCorner.Parent = autoMiniBtn

    autoMiniBtn.MouseButton1Click:Connect(function()
        autoFrame:Destroy()
        createFloatingCircle("😂", function()
            setupAutoGUI(autoMode, stompEnabled)
        end)
    end)

    local rainbowLabel = Instance.new("TextLabel")
    rainbowLabel.Size = UDim2.new(1, 0, 0, 30)
    rainbowLabel.BackgroundTransparency = 1
    rainbowLabel.Text = (selectedLanguage == "en") and "Auto 💚" or "التحكم الآلي 💚"
    rainbowLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    rainbowLabel.TextSize = 14
    rainbowLabel.Font = Enum.Font.GothamBold
    rainbowLabel.Parent = autoFrame

    task.spawn(function()
        while rainbowLabel and rainbowLabel.Parent do
            for hue = 0, 1, 0.005 do
                if not rainbowLabel or not rainbowLabel.Parent then break end
                rainbowLabel.TextColor3 = Color3.fromHSV(hue, 1, 1)
                task.wait(0.04)
            end
        end
    end)

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(0.92, 0, 0, 42)
    toggleBtn.Position = UDim2.new(0.04, 0, 0, 35)
    toggleBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
    toggleBtn.Text = (selectedLanguage == "en") and "Status: OFF" or "الحالة: مطفي"
    toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleBtn.TextSize = 14
    toggleBtn.Font = Enum.Font.GothamBold
    toggleBtn.Parent = autoFrame

    local toggleCorner = Instance.new("UICorner")
    toggleCorner.CornerRadius = UDim.new(0, 10)
    toggleCorner.Parent = toggleBtn

    local modeLabel = Instance.new("TextLabel")
    modeLabel.Size = UDim2.new(0.92, 0, 0, 25)
    modeLabel.Position = UDim2.new(0.04, 0, 0, stompEnabled and 138 or 88)
    modeLabel.BackgroundTransparency = 1
    modeLabel.Text = (selectedLanguage == "en") and ("Mode: " .. autoMode) or ("الوضع: " .. autoMode)
    modeLabel.TextColor3 = Color3.fromRGB(170, 170, 200)
    modeLabel.TextSize = 12
    modeLabel.Font = Enum.Font.GothamMedium
    modeLabel.Parent = autoFrame

    local autoEnabled = false
    local stompRunning = false
    local stompButton, stompStroke, stompTask

    if stompEnabled then
        stompButton = Instance.new("TextButton")
        stompButton.Size = UDim2.new(0.92, 0, 0, 45)
        stompButton.Position = UDim2.new(0.04, 0, 0, 84)
        stompButton.BackgroundColor3 = Color3.fromRGB(32, 32, 45)
        stompButton.Text = (selectedLanguage == "en") and "Auto Stomp: Press to enable" or "اوتو دعس اضغط للتفعيل"
        stompButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        stompButton.TextSize = 13
        stompButton.Font = Enum.Font.GothamBold
        stompButton.Parent = autoFrame

        local stompCorner = Instance.new("UICorner")
        stompCorner.CornerRadius = UDim.new(0, 10)
        stompCorner.Parent = stompButton

        stompStroke = Instance.new("UIStroke")
        stompStroke.Color = Color3.fromRGB(60, 60, 85)
        stompStroke.Thickness = 1.5
        stompStroke.Parent = stompButton

        stompButton.MouseButton1Click:Connect(function()
            if stompRunning then return end
            stompRunning = true

            TweenService:Create(stompButton, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(0, 180, 80)
            }):Play()
            TweenService:Create(stompStroke, TweenInfo.new(0.2), {
                Color = Color3.fromRGB(0, 255, 120)
            }):Play()
            stompButton.Text = (selectedLanguage == "en") and "Running W S" or "جاري W S"

            if stompTask then task.cancel(stompTask) end
            stompTask = task.delay(3, function()
                stompRunning = false
                if stompButton and stompButton.Parent then
                    TweenService:Create(stompButton, TweenInfo.new(0.2), {
                        BackgroundColor3 = Color3.fromRGB(32, 32, 45)
                    }):Play()
                    TweenService:Create(stompStroke, TweenInfo.new(0.2), {
                        Color = Color3.fromRGB(60, 60, 85)
                    }):Play()
                    stompButton.Text = (selectedLanguage == "en") and "Auto Stomp: Press to enable" or "اوتو دعس اضغط للتفعيل"
                end
            end)
        end)
    end

    toggleBtn.MouseButton1Click:Connect(function()
        autoEnabled = not autoEnabled
        if autoEnabled then
            TweenService:Create(toggleBtn, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(0, 180, 80)
            }):Play()
            toggleBtn.Text = (selectedLanguage == "en") and "Status: ON" or "الحالة: مشغل"
        else
            TweenService:Create(toggleBtn, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(180, 40, 40)
            }):Play()
            toggleBtn.Text = (selectedLanguage == "en") and "Status: OFF" or "الحالة: مطفي"
        end
    end)

    local rotationTotal = 0
    local savedPosition = nil
    local currentTarget = nil

    RunService.Heartbeat:Connect(function()
        pcall(function()
            local character = LocalPlayer.Character
            if not character then return end

            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            if not humanoid or not rootPart then return end

            local currentTime = tick()

            if stompRunning then
                if not savedPosition then
                    savedPosition = rootPart.Position
                end
                local offset = math.sin(currentTime * 16)
                humanoid:MoveTo(savedPosition + rootPart.CFrame.LookVector * (offset * 1.8))
                return
            end

            savedPosition = nil

            if not autoEnabled then
                currentTarget = nil
                return
            end

            local hasTool = false
            for _, item in ipairs(character:GetChildren()) do
                if item:IsA("Tool") then
                    hasTool = true
                    break
                end
            end
            if not hasTool then return end

            if currentTarget and currentTarget.Parent and currentTarget.Parent:FindFirstChild("Humanoid") then
                if currentTarget.Parent.Humanoid.Health <= 0
                    or (rootPart.Position - currentTarget.Position).Magnitude > 30 then
                    currentTarget = nil
                end
            else
                currentTarget = nil
            end

            if not currentTarget then
                local closestDist = math.huge
                for _, otherPlayer in ipairs(Players:GetPlayers()) do
                    if otherPlayer ~= LocalPlayer
                        and otherPlayer.Character
                        and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        local otherHumanoid = otherPlayer.Character:FindFirstChild("Humanoid")
                        if otherHumanoid and otherHumanoid.Health > 0 then
                            local dist = (rootPart.Position - otherPlayer.Character.HumanoidRootPart.Position).Magnitude
                            if dist < closestDist then
                                closestDist = dist
                                currentTarget = otherPlayer.Character.HumanoidRootPart
                            end
                        end
                    end
                end
            end

            if currentTarget then
                humanoid:MoveTo(currentTarget.Position)
                local lookAt = Vector3.new(
                    currentTarget.Position.X,
                    rootPart.Position.Y,
                    currentTarget.Position.Z
                )

                if autoMode == "fast" then
                    rotationTotal = rotationTotal + 40
                    rootPart.CFrame = CFrame.new(rootPart.Position, lookAt)
                        * CFrame.Angles(0, math.rad(rotationTotal), 0)
                elseif autoMode == "flick" then
                    local flickAngle = math.sin(currentTime * 25) * 1.1
                    rootPart.CFrame = CFrame.new(rootPart.Position, lookAt)
                        * CFrame.Angles(0, flickAngle, 0)
                end
            end
        end)
    end)
end

function openMainMenu()
    local menuFrame = Instance.new("Frame")
    menuFrame.Size = UDim2.new(0, 540, 0, 290)
    menuFrame.Position = UDim2.new(0.5, -270, 0.4, -145)
    menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    menuFrame.BorderSizePixel = 0
    menuFrame.Active = true
    menuFrame.Draggable = true
    menuFrame.Parent = mainGui
    menuFrame.BackgroundTransparency = 0.1

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 18)
    corner.Parent = menuFrame

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(80, 80, 110)
    stroke.Thickness = 2
    stroke.Parent = menuFrame

    local menuMiniBtn = Instance.new("TextButton")
    menuMiniBtn.Size = UDim2.new(0, 26, 0, 26)
    menuMiniBtn.Position = UDim2.new(1, -34, 0, 8)
    menuMiniBtn.BackgroundColor3 = Color3.fromRGB(155, 89, 182)
    menuMiniBtn.Text = "-"
    menuMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    menuMiniBtn.TextSize = 16
    menuMiniBtn.Font = Enum.Font.GothamBold
    menuMiniBtn.Parent = menuFrame

    local menuMiniCorner = Instance.new("UICorner")
    menuMiniCorner.CornerRadius = UDim.new(1, 0)
    menuMiniCorner.Parent = menuMiniBtn

    menuMiniBtn.MouseButton1Click:Connect(function()
        menuFrame:Destroy()
        createFloatingCircle("😂", function()
            openMainMenu()
        end)
    end)

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 45)
    title.Position = UDim2.new(0, 0, 0, 5)
    title.BackgroundTransparency = 1
    title.Text = (selectedLanguage == "en") and "Choose an Auto mode" or "اختر الوضع المناسب"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.TextSize = 15
    title.Font = Enum.Font.GothamBold
    title.Parent = menuFrame

    local selectedMode = "normal"
    local stompEnabled = false

    local function createModeCard(xPos, badgeText, badgeColor, buttonText, modeValue)
        local card = Instance.new("Frame")
        card.Size = UDim2.new(0, 160, 0, 135)
        card.Position = UDim2.new(0, xPos, 0, 50)
        card.BackgroundColor3 = Color3.fromRGB(22, 22, 30)
        card.BorderSizePixel = 0
        card.Parent = menuFrame

        local cardCorner = Instance.new("UICorner")
        cardCorner.CornerRadius = UDim.new(0, 12)
        cardCorner.Parent = card

        local cardStroke = Instance.new("UIStroke")
        cardStroke.Color = Color3.fromRGB(45, 45, 65)
        cardStroke.Thickness = 1.5
        cardStroke.Parent = card

        local badge = Instance.new("TextLabel")
        badge.Size = UDim2.new(1, 0, 0, 25)
        badge.Position = UDim2.new(0, 0, 0, 8)
        badge.BackgroundTransparency = 1
        badge.Text = badgeText
        badge.TextColor3 = badgeColor
        badge.TextSize = 13
        badge.Font = Enum.Font.GothamBold
        badge.Parent = card

        local selectBtn = Instance.new("TextButton")
        selectBtn.Size = UDim2.new(0.85, 0, 0, 70)
        selectBtn.Position = UDim2.new(0.075, 0, 0, 38)
        selectBtn.BackgroundColor3 = Color3.fromRGB(32, 32, 45)
        selectBtn.Text = buttonText
        selectBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        selectBtn.TextSize = 15
        selectBtn.Font = Enum.Font.GothamBold
        selectBtn.Parent = card

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 10)
        btnCorner.Parent = selectBtn

        card.MouseEnter:Connect(function()
            TweenService:Create(card, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(28, 28, 40)
            }):Play()
            TweenService:Create(cardStroke, TweenInfo.new(0.2), {
                Color = badgeColor
            }):Play()
        end)

        card.MouseLeave:Connect(function()
            TweenService:Create(card, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(22, 22, 30)
            }):Play()
            TweenService:Create(cardStroke, TweenInfo.new(0.2), {
                Color = Color3.fromRGB(45, 45, 65)
            }):Play()
        end)

        selectBtn.MouseButton1Click:Connect(function()
            selectedMode = modeValue
            showToast("تم اختيار: " .. buttonText)
        end)
    end

    createModeCard(18, (selectedLanguage == "en") and "Reliable 100%" or "مضمون 100%", Color3.fromRGB(0, 255, 120), (selectedLanguage == "en") and "Normal Auto" or "الاوتو العادي", "normal")
    createModeCard(190, (selectedLanguage == "en") and "Reliable 90%" or "مضمون 90%", Color3.fromRGB(255, 200, 0), (selectedLanguage == "en") and "Fast Auto" or "الاوتو السريع", "fast")
    createModeCard(362, (selectedLanguage == "en") and "Best option" or "100% افضل شي", Color3.fromRGB(0, 200, 255), (selectedLanguage == "en") and "Flick Auto" or "الاوتو فلك", "flick")

    local stompBtn = Instance.new("TextButton")
    stompBtn.Size = UDim2.new(0.93, 0, 0, 32)
    stompBtn.Position = UDim2.new(0.035, 0, 0, 192)
    stompBtn.BackgroundColor3 = Color3.fromRGB(32, 32, 45)
    stompBtn.Text = (selectedLanguage == "en") and "Auto Stomp: OFF" or "اوتو دعس الحالة: موقف"
    stompBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    stompBtn.TextSize = 13
    stompBtn.Font = Enum.Font.GothamBold
    stompBtn.Parent = menuFrame

    local stompCorner = Instance.new("UICorner")
    stompCorner.CornerRadius = UDim.new(0, 8)
    stompCorner.Parent = stompBtn

    stompBtn.MouseButton1Click:Connect(function()
        stompEnabled = not stompEnabled
        if stompEnabled then
            TweenService:Create(stompBtn, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(0, 180, 80)
            }):Play()
            stompBtn.Text = (selectedLanguage == "en") and "Auto Stomp: ON" or "اوتو دعس الحالة: مفعل"
        else
            TweenService:Create(stompBtn, TweenInfo.new(0.2), {
                BackgroundColor3 = Color3.fromRGB(32, 32, 45)
            }):Play()
            stompBtn.Text = (selectedLanguage == "en") and "Auto Stomp: OFF" or "اوتو دعس الحالة: موقف"
        end
    end)

    local startBtn = Instance.new("TextButton")
    startBtn.Size = UDim2.new(0.93, 0, 0, 34)
    startBtn.Position = UDim2.new(0.035, 0, 0, 234)
    startBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
    startBtn.Text = (selectedLanguage == "en") and "Start ✈️😍" or "تشغيل ✈️😍"
    startBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    startBtn.TextSize = 13
    startBtn.Font = Enum.Font.GothamBold
    startBtn.Parent = menuFrame

    task.spawn(function()
        while startBtn and startBtn.Parent do
            for hue = 0, 1, 0.005 do
                if not startBtn or not startBtn.Parent then break end
                startBtn.TextColor3 = Color3.fromHSV(hue, 1, 1)
                task.wait(0.03)
            end
        end
    end)

    local startCorner = Instance.new("UICorner")
    startCorner.CornerRadius = UDim.new(0, 8)
    startCorner.Parent = startBtn

    local startStroke = Instance.new("UIStroke")
    startStroke.Color = Color3.fromRGB(80, 80, 110)
    startStroke.Thickness = 1.5
    startStroke.Parent = startBtn

    startBtn.MouseButton1Click:Connect(function()
        menuFrame:Destroy()
        setupAutoGUI(selectedMode, stompEnabled)
    end)
end

-- ============================================================
-- ASSIST MODULE
-- ============================================================
local CoreGui = game:GetService("CoreGui")

if CoreGui:FindFirstChild("Assist GUI") then
    CoreGui["Assist GUI"]:Destroy()
end

local ancHubGui = Instance.new("ScreenGui")
ancHubGui.Name = "Assist GUI"
ancHubGui.ResetOnSpawn = false
ancHubGui.Parent = CoreGui

local function createAssistGUI()
    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, 240, 0, 215)
    mainFrame.Position = UDim2.new(0.5, -120, 0.4, -107)
    mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    mainFrame.BackgroundTransparency = 0.1
    mainFrame.BorderSizePixel = 0
    mainFrame.Active = true
    mainFrame.Draggable = true
    mainFrame.Parent = ancHubGui

    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 16)
    mainCorner.Parent = mainFrame

    local mainStroke = Instance.new("UIStroke")
    mainStroke.Color = Color3.fromRGB(155, 89, 182)
    mainStroke.Transparency = 0.4
    mainStroke.Thickness = 2
    mainStroke.Parent = mainFrame

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 30)
    titleLabel.Position = UDim2.new(0, 0, 0, 8)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "<font color=\"#2ecc71\">ASSIST</font> <font color=\"#2f8cff\">HUB</font>"
    titleLabel.RichText = true
    titleLabel.TextSize = 20
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.Parent = mainFrame

    local miniCircleBtn = Instance.new("TextButton")
    miniCircleBtn.Size = UDim2.new(0, 26, 0, 26)
    miniCircleBtn.Position = UDim2.new(1, -32, 0, 6)
    miniCircleBtn.BackgroundColor3 = Color3.fromRGB(155, 89, 182)
    miniCircleBtn.Text = "-"
    miniCircleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    miniCircleBtn.TextSize = 16
    miniCircleBtn.Font = Enum.Font.GothamBold
    miniCircleBtn.Parent = mainFrame

    local circleCorner = Instance.new("UICorner")
    circleCorner.CornerRadius = UDim.new(1, 0)
    circleCorner.Parent = miniCircleBtn

    miniCircleBtn.MouseButton1Click:Connect(function()
        mainFrame:Destroy()
        createFloatingCircle("⚡", function()
            createAssistGUI()
        end)
    end)

    local phantomLabel = Instance.new("TextLabel")
    phantomLabel.Size = UDim2.new(1, 0, 0, 20)
    phantomLabel.Position = UDim2.new(0, 0, 0, 34)
    phantomLabel.BackgroundTransparency = 1
    phantomLabel.Text = (_G.HubLanguage == "en") and "⚡ Assist Module ⚡" or "⚡ وحدة المساعد ⚡"
    phantomLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    phantomLabel.TextSize = 14
    phantomLabel.Font = Enum.Font.GothamBold
    phantomLabel.Parent = mainFrame

    task.spawn(function()
        while phantomLabel and phantomLabel.Parent do
            for hue = 0, 1, 0.008 do
                if not phantomLabel or not phantomLabel.Parent then break end
                phantomLabel.TextColor3 = Color3.fromHSV(hue, 1, 1)
                task.wait(0.05)
            end
        end
    end)

    local versionLabel = Instance.new("TextLabel")
    versionLabel.Size = UDim2.new(1, 0, 0, 18)
    versionLabel.Position = UDim2.new(0, 0, 0, 56)
    versionLabel.BackgroundTransparency = 1
    versionLabel.Text = (_G.HubLanguage == "en") and "Mobile Assist" or "مساعد الجوال"
    versionLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    versionLabel.TextTransparency = 0.45
    versionLabel.TextSize = 12
    versionLabel.Font = Enum.Font.GothamMedium
    versionLabel.Parent = mainFrame

    local assistEnabled = false
    local assistButton = Instance.new("TextButton")
    assistButton.Size = UDim2.new(0.85, 0, 0, 42)
    assistButton.Position = UDim2.new(0.075, 0, 0, 82)
    assistButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    assistButton.Text = (_G.HubLanguage == "en") and "Assist: OFF" or "اسيست: غير مفعل"
    assistButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    assistButton.TextSize = 15
    assistButton.Font = Enum.Font.GothamSemibold
    assistButton.Parent = mainFrame

    local assistCorner = Instance.new("UICorner")
    assistCorner.CornerRadius = UDim.new(0, 10)
    assistCorner.Parent = assistButton

    local assistStroke = Instance.new("UIStroke")
    assistStroke.Color = Color3.fromRGB(60, 60, 70)
    assistStroke.Thickness = 1
    assistStroke.Parent = assistButton

    local statusLabel = Instance.new("TextLabel")
    statusLabel.Size = UDim2.new(1, 0, 0, 30)
    statusLabel.Position = UDim2.new(0, 0, 0, 155)
    statusLabel.BackgroundTransparency = 1
    statusLabel.Text = (_G.HubLanguage == "en") and "Status: OFF" or "الحالة: متوقف"
    statusLabel.TextColor3 = Color3.fromRGB(170, 170, 170)
    statusLabel.TextSize = 13
    statusLabel.Font = Enum.Font.GothamMedium
    statusLabel.Parent = mainFrame

    local notifyFrame = Instance.new("Frame")
    notifyFrame.Size = UDim2.new(0, 240, 0, 70)
    notifyFrame.Position = UDim2.new(0.5, -120, 0.72, 0)
    notifyFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    notifyFrame.BackgroundTransparency = 0.25
    notifyFrame.BorderSizePixel = 0
    notifyFrame.Visible = false
    notifyFrame.Parent = ancHubGui

    local notifyCorner = Instance.new("UICorner")
    notifyCorner.CornerRadius = UDim.new(0, 14)
    notifyCorner.Parent = notifyFrame

    local notifyStroke = Instance.new("UIStroke")
    notifyStroke.Color = Color3.fromRGB(46, 204, 113)
    notifyStroke.Transparency = 0.2
    notifyStroke.Thickness = 2
    notifyStroke.Parent = notifyFrame

    local notifyLabel = Instance.new("TextLabel")
    notifyLabel.Size = UDim2.new(1, 0, 1, 0)
    notifyLabel.BackgroundTransparency = 1
    notifyLabel.Text = "يالعيببب🙏🏻✈️"
    notifyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    notifyLabel.TextTransparency = 0.1
    notifyLabel.TextSize = 19
    notifyLabel.Font = Enum.Font.GothamBold
    notifyLabel.Parent = notifyFrame

    local notifyVisible = false
    local currentTarget = nil

    local function toggleNotification(show)
        if show and not notifyVisible then
            notifyVisible = true
            notifyFrame.Visible = true
            TweenService:Create(notifyFrame, TweenInfo.new(0.25), {
                BackgroundTransparency = 0.25
            }):Play()
            TweenService:Create(notifyLabel, TweenInfo.new(0.25), {
                TextTransparency = 0
            }):Play()
            TweenService:Create(notifyStroke, TweenInfo.new(0.25), {
                Transparency = 0.2
            }):Play()
        elseif not show and notifyVisible then
            notifyVisible = false
            TweenService:Create(notifyFrame, TweenInfo.new(0.3), {
                BackgroundTransparency = 1
            }):Play()
            TweenService:Create(notifyLabel, TweenInfo.new(0.3), {
                TextTransparency = 1
            }):Play()
            TweenService:Create(notifyStroke, TweenInfo.new(0.3), {
                Transparency = 1
            }):Play()
            task.delay(0.3, function()
                if not notifyVisible and notifyFrame then
                    notifyFrame.Visible = false
                end
            end)
        end
    end

    local function getNearestPlayer()
        local character = LocalPlayer.Character
        if not character or not character:FindFirstChild("HumanoidRootPart") then
            return nil
        end
        local rootPart = character.HumanoidRootPart
        local nearest = nil
        local closestDist = math.huge
        for _, otherPlayer in ipairs(Players:GetPlayers()) do
            if otherPlayer ~= LocalPlayer
                and otherPlayer.Character
                and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local otherHumanoid = otherPlayer.Character:FindFirstChildOfClass("Humanoid")
                if otherHumanoid and otherHumanoid.Health > 0 then
                    local dist = (rootPart.Position - otherPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if dist < closestDist then
                        closestDist = dist
                        nearest = otherPlayer
                    end
                end
            end
        end
        return nearest
    end

    local function hasToolEquipped()
        local character = LocalPlayer.Character
        local backpack = LocalPlayer:FindFirstChildOfClass("Backpack")
        if character and character:FindFirstChildOfClass("Tool") then
            return true
        end
        if backpack and backpack:FindFirstChildOfClass("Tool") then
            return true
        end
        return false
    end

    assistButton.MouseButton1Click:Connect(function()
        assistEnabled = not assistEnabled
        toggleNotification(false)
        if assistEnabled then
            assistButton.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
            assistButton.Text = (_G.HubLanguage == "en") and "Assist: ON" or "اسيست: مفعل"
            statusLabel.Text = (_G.HubLanguage == "en") and "Status: ON" or "الحالة: مفعل"
        else
            assistButton.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
            assistButton.Text = "اسيست: غير مفعل"
            statusLabel.Text = (_G.HubLanguage == "en") and "Status: OFF" or "الحالة: متوقف"
            currentTarget = nil
        end
    end)

    RunService.Heartbeat:Connect(function()
        if not assistEnabled then
            toggleNotification(false)
            return
        end

        if not hasToolEquipped() then
            statusLabel.Text = (_G.HubLanguage == "en") and "Status: Equip a tool" or "الحالة: مو معك القنبله"
            toggleNotification(false)
            return
        end

        statusLabel.Text = "الحالة: مفعل"

        local character = LocalPlayer.Character
        if not character or not character:FindFirstChild("HumanoidRootPart") then
            return
        end

        local humanoid = character:FindFirstChildOfClass("Humanoid")
        local rootPart = character.HumanoidRootPart
        if not humanoid then return end

        if currentTarget and currentTarget.Parent and currentTarget.Parent:FindFirstChild("Humanoid") then
            local targetHumanoid = currentTarget.Parent.Humanoid
            if targetHumanoid.Health <= 0 or (rootPart.Position - currentTarget.Position).Magnitude > 50 then
                currentTarget = nil
            end
        else
            currentTarget = nil
        end

        if not currentTarget then
            local nearestPlayer = getNearestPlayer()
            if nearestPlayer and nearestPlayer.Character then
                currentTarget = nearestPlayer.Character:FindFirstChild("HumanoidRootPart")
                if currentTarget then
                    toggleNotification(true)
                    task.delay(1, function()
                        toggleNotification(false)
                    end)
                end
            end
        end

        if currentTarget then
            humanoid:MoveTo(currentTarget.Position)
            local lookAt = Vector3.new(currentTarget.Position.X, rootPart.Position.Y, currentTarget.Position.Z)
            rootPart.CFrame = CFrame.new(rootPart.Position, lookAt)
        end
    end)
end

createAssistGUI()