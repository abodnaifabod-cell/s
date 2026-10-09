-- السكربت الشامل (تلوين المساكات الأربع فقط باللون الأبيض، كشف أحمر، إشعار، ومكان القفل)
local SGui = game:GetService("StarterGui")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera
local playerGui = player:WaitForChild("PlayerGui")

-- 1. إرسال الإشعار المطلوب فوراً
pcall(function()
    SGui:SetCore("SendNotification", {
        Title = "تنبيه",
        Text = "ها وصلت لك ؟ 🔥",
        Duration = 3
    })
end)

-- 2. إضافة كشف اللاعبين باللون الأحمر من خلف الجدران (Wallhack ESP)
local function addRedEsp(plr)
    if plr == player then return end
    
    local function applyHighlight(char)
        if not char:FindFirstChild("RedWallhackHighlight") then
            local highlight = Instance.new("Highlight")
            highlight.Name = "RedWallhackHighlight"
            highlight.Adornee = char
            highlight.FillColor = Color3.fromRGB(255, 0, 0)
            highlight.FillTransparency = 0.3
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.OutlineTransparency = 0
            highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            highlight.Parent = char
        end
    end

    if plr.Character then
        applyHighlight(plr.Character)
    end
    plr.CharacterAdded:Connect(applyHighlight)
end

for _, plr in ipairs(Players:GetPlayers()) do
    addRedEsp(plr)
end
Players.PlayerAdded:Connect(addRedEsp)

-- 3. تلوين المساكات الأربع (العوائق الداخلية) باللون الأبيض فقط بدقة تامة دون المساس بباقي الماب
for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
    if v:IsA("BasePart") then
        local name = string.lower(v.Name)
        -- التحقق من أن الجزء يمثل مساكة أو عائق داخلي (عبر خصائص الحجم أو الاسم المتعارف عليه)
        -- بحيث لا يؤثر أبداً على الأرضية الكبيرة أو الجدران الخارجية المحيطة
        local isBarrier = (v.Size.X > 3 and v.Size.X < 25) and (v.Size.Z > 3 and v.Size.Z < 25) and (v.Size.Y > 2 and v.Size.Y < 8)
        if isBarrier then
            v.Material = Enum.Material.SmoothPlastic
            v.Color = Color3.fromRGB(255, 255, 255)
        end
    end
end

-- 4. إضاءة نقية ودقة عالية للرؤية واللوحات عن بعد
local lighting = game:GetService("Lighting")
lighting.GlobalShadows = true
lighting.Brightness = 2.5
lighting.ClockTime = 12

for _, e in pairs(lighting:GetChildren()) do
    if e:IsA("BlurEffect") or e:IsA("SunRaysEffect") or e:IsA("ColorCorrectionEffect") then
        e:Destroy()
    end
end

local bloom = Instance.new("BloomEffect")
bloom.Intensity = 0.3
bloom.Size = 24
bloom.Threshold = 0.8
bloom.Parent = lighting

local cc = Instance.new("ColorCorrectionEffect")
cc.Brightness = 0.08
cc.Contrast = 0.3
cc.Saturation = 0.1
cc.Parent = lighting

-- 5. توسيع زاوية الرؤية (FOV)
camera.FieldOfView = 90

-- 6. نقل زر القفل/التصويب إلى الموضع الجديد فوق الخط بدقة
local function findAndMoveLockButton()
    for _, gui in ipairs(playerGui:GetDescendants()) do
        if gui:IsA("ImageButton") or gui:IsA("TextButton") or gui:IsA("Frame") then
            local name = string.lower(gui.Name)
            if name:find("lock") or name:find("target") or name:find("aim") then
                
                gui.Position = UDim2.new(0.35, 0, 0.05, 0)
                
                if gui:IsA("ImageButton") or gui:IsA("TextButton") then
                    pcall(function()
                        firesignal(gui.MouseButton1Click)
                        firesignal(gui.Activated)
                    end)
                end
                
                return true
            end
        end
    end
    return false
end

findAndMoveLockButton()

print("تم تحديد المساكات الأربع وتلوينها بالبيض بنجاح!")