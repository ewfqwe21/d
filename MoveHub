--[[
    ██████╗ ██╗  ██╗██╗   ██╗███████╗██╗ ██████╗███████╗    ██╗      █████╗ ██████╗
    ██╔══██╗██║  ██║╚██╗ ██╔╝██╔════╝██║██╔════╝██╔════╝    ██║     ██╔══██╗██╔══██╗
    ██████╔╝███████║ ╚████╔╝ ███████╗██║██║     ███████╗    ██║     ███████║██████╔╝
    ██╔═══╝ ██╔══██║  ╚██╔╝  ╚════██║██║██║     ╚════██║    ██║     ██╔══██║██╔══██╗
    ██║     ██║  ██║   ██║   ███████║██║╚██████╗███████║    ███████╗██║  ██║██████╔╝
    ╚═╝     ╚═╝  ╚═╝   ╚═╝   ╚══════╝╚═╝ ╚═════╝╚══════╝    ╚══════╝╚═╝  ╚═╝╚═════╝

    PhysicsLab v8 — LIQUID GLASS EDITION
    Полное слияние PhysicsLab v7 (логика) + LiquidGlass GUI (интерфейс)

    Особенности GUI:
    • Liquid Glass — BlurEffect + полупрозрачные панели + глянцевый градиент + UIStroke
    • Draggable — свободное перетаскивание за header
    • Inertia drag — после отпускания окно «доезжает» по инерции
    • Floating idle — лёгкое покачивание когда окно не трогают
    • iOS-style тогглы, слайдеры, сегментные кнопки
    • DisplayOrder 999999999 — поверх всех других GUI

    Все функции PhysicsLab v7 сохранены:
    Bhop, InfJump, DoubleJump, AirStrafe, ADStrafe, DrunkMovement,
    Fly, Spin, SmoothCam, Noclip, AntiRag, Boost, TeleportSwap,
    TeleportToCam, Trail, Fog, Blur, FX, Sky, Crosshair, SpeedMeter,
    FpsPing, ESP, JumpRings, KeyHints, ExportCFG, ImportCFG, MoveV2
]]

-- ════════════════════════════════════════════════════════════════════
-- СЕРВИСЫ
-- ════════════════════════════════════════════════════════════════════
local Players      = game:GetService("Players")
local RunService   = game:GetService("RunService")
local UIS          = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting     = game:GetService("Lighting")
local HttpService  = game:GetService("HttpService")
local Stats        = game:GetService("Stats")

local LP   = Players.LocalPlayer
local PGui = LP:WaitForChild("PlayerGui")

print("[PhysicsLab] v8 LIQUID GLASS")

-- ════════════════════════════════════════════════════════════════════
-- КОНФИГ
-- ════════════════════════════════════════════════════════════════════
local Cfg = {
    WalkSpeed   = 16,
    JumpPower   = 50,
    Gravity     = 196.2,
    FOV         = 70,

    BhopOn      = false,
    BhopAuto    = true,
    BhopGain    = 1.04,
    BhopMax     = 120,
    BhopStreak  = 0,

    InfJumpOn       = false,
    InfJumpCooldown = 5,

    DoubleJumpOn = false,

    StrafeOn    = false,
    StrafeAccel = 60,
    StrafeMax   = 100,

    ADStrafeOn    = false,
    ADStrafeSpeed = 16,

    DrunkOn       = false,
    DrunkAccel    = 8,
    DrunkMaxSpeed = 12,
    DrunkSlide    = 0.97,

    MoveMethod   = "WalkSpeed",
    MoveV2Speed  = 16,

    FpsPingOn   = false,

    FlyOn       = false,
    FlySpeed    = 40,

    SpinOn      = false,
    SpinSpeed   = 180,

    BoostOn     = false,
    BoostSpeed  = 120,
    BoostDur    = 0.4,

    NoclipOn    = false,
    AntiRagOn   = false,

    SmoothCamOn   = false,
    SmoothCamLerp = 0.15,

    MenuKey        = Enum.KeyCode.RightAlt,
    SwapBindKey    = Enum.KeyCode.Q,
    SwapUseKey     = Enum.KeyCode.E,
    BoostKey       = Enum.KeyCode.F,
    TeleportCamKey = Enum.KeyCode.T,

    SwapMarkerR = 0, SwapMarkerG = 200, SwapMarkerB = 255,
    BlurOn = false, BlurSize = 10,
    FXOn = false, FXType = 1,
    CrossOn = false, CrossSize = 10, CrossGap = 4, CrossThick = 2, CrossDot = false,
    CrossR = 255, CrossG = 255, CrossB = 255,
    FogOn = false, FogStart = 50, FogEnd = 300, FogR = 180, FogG = 200, FogB = 255,
    TrailOn = false, TrailLife = 0.5, TrailWidth = 1.5,
    TrailR1 = 255, TrailG1 = 255, TrailB1 = 255,
    TrailR2 = 80,  TrailG2 = 180, TrailB2 = 255,
    SpeedOn = false, SafeMode = false, MenuVisible = true,

    EspOn = false, EspUseTeamColor = true, EspTrackRoles = true,
    EspFriendR = 0, EspFriendG = 255, EspFriendB = 100,
    EspEnemyR  = 255, EspEnemyG = 50,  EspEnemyB = 50,

    KeyHintsOn      = true,

    JumpRingsOn          = false,
    JumpRingsMode        = "Self",
    JumpRingsColorR      = 80,
    JumpRingsColorG      = 200,
    JumpRingsColorB      = 255,
    JumpRingsRadius      = 4,
    JumpRingsThickness   = 0.4,
    JumpRingsHeight      = 0.15,
    JumpRingsTransparency= 0.2,
    JumpRingsOutline     = true,
    JumpRingsLifetime    = 3,
}

-- ════════════════════════════════════════════════════════════════════
-- LIQUID GLASS ПАЛИТРА
-- ════════════════════════════════════════════════════════════════════
local GLASS_BG    = Color3.fromRGB(18, 18, 24)
local GLASS_BG2   = Color3.fromRGB(32, 32, 42)
local GL_TEXT     = Color3.fromRGB(245, 245, 250)
local GL_SUB      = Color3.fromRGB(150, 150, 165)
local GL_ACCENT   = Color3.fromRGB(10, 132, 255)
local GL_ACCENT2  = Color3.fromRGB(80, 190, 255)
local GL_STROKE   = Color3.fromRGB(255, 255, 255)
local GL_DANGER   = Color3.fromRGB(255, 69, 58)
local GL_SUCCESS  = Color3.fromRGB(48, 209, 88)
local GL_SURF     = Color3.fromRGB(38, 38, 50)

-- ════════════════════════════════════════════════════════════════════
-- ПЕРСОНАЖ / СОСТОЯНИЕ
-- ════════════════════════════════════════════════════════════════════
local Char, Hum, HRP, Cam
local wasGround       = true
local flyVel          = Vector3.zero
local spinAngle       = 0
local trailObj        = nil
local loopConn        = nil
local noclipConn      = nil
local antiRagConn     = nil
local boostConn       = nil
local boostActive     = false
local swapPos         = nil
local swapMarker      = nil
local fxParts         = {}
local fxConn          = nil
local blurObj         = nil
local smoothYaw       = 0
local lastInfJumpTime = 0
local hasDoubleJumped = false
local moveV2Obj       = nil
local fpsPingConn     = nil
local fpsPingLabel    = nil

-- Forward-объявления (используются внутри BuildGUI до своей реализации)
local startJumpRings, stopJumpRings, restartJumpRings
local ShowKeyHint, SetMenuVisible

-- ════════════════════════════════════════════════════════════════════
-- ESP
-- ════════════════════════════════════════════════════════════════════
local espHolder = game:GetService("CoreGui"):FindFirstChild("PhysLab_ESP")
    or Instance.new("Folder", game:GetService("CoreGui"))
espHolder.Name = "PhysLab_ESP"

local RoleColors = {
    Murderer = Color3.fromRGB(255,0,0),
    Sheriff  = Color3.fromRGB(0,100,255),
    Innocent = Color3.fromRGB(0,255,100),
    Traitor  = Color3.fromRGB(255,150,0),
    Detective= Color3.fromRGB(0,200,255),
}

local function GetPlayerRole(p)
    if not Cfg.EspTrackRoles then return nil end
    local bp   = p:FindFirstChild("Backpack")
    local char = p.Character
    if bp or char then
        if (bp and bp:FindFirstChild("Knife")) or (char and char:FindFirstChild("Knife")) then return "Murderer"
        elseif (bp and bp:FindFirstChild("Gun")) or (char and char:FindFirstChild("Gun")) then return "Sheriff" end
    end
    local rv = p:FindFirstChild("Role") or p:FindFirstChild("role") or p:FindFirstChild("Status")
    if rv and rv:IsA("StringValue") and RoleColors[rv.Value] then return rv.Value end
    return nil
end

local function GetEspColor(p)
    local role = GetPlayerRole(p)
    if role and RoleColors[role] then return RoleColors[role] end
    if Cfg.EspUseTeamColor then return p.TeamColor.Color end
    if p.TeamColor == LP.TeamColor then
        return Color3.fromRGB(Cfg.EspFriendR, Cfg.EspFriendG, Cfg.EspFriendB)
    else
        return Color3.fromRGB(Cfg.EspEnemyR, Cfg.EspEnemyG, Cfg.EspEnemyB)
    end
end

local function UpdateESP()
    for _, p in ipairs(Players:GetPlayers()) do
        if p == LP then continue end
        local pf = espHolder:FindFirstChild(p.Name)
        if not Cfg.EspOn or not p.Parent or not p.Character or not p.Character:FindFirstChild("HumanoidRootPart") then
            if pf then pf:Destroy() end; continue
        end
        if not pf then
            pf = Instance.new("Highlight", espHolder); pf.Name = p.Name
        end
        pf.Adornee = p.Character
        pf.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        pf.FillColor = GetEspColor(p)
        pf.FillTransparency = 0.4
        pf.OutlineColor = Color3.new(1,1,1)
        pf.OutlineTransparency = 0.2
    end
end
RunService.Heartbeat:Connect(function() pcall(UpdateESP) end)

-- ════════════════════════════════════════════════════════════════════
-- CFG IMPORT / EXPORT
-- ════════════════════════════════════════════════════════════════════
_G.ExportCFG = function()
    local exportData = {}
    for k,v in pairs(Cfg) do
        if typeof(v)=="Color3" then exportData[k]={v.R,v.G,v.B}
        elseif typeof(v)=="EnumItem" then exportData[k]=tostring(v)
        else exportData[k]=v end
    end
    local json = HttpService:JSONEncode(exportData)
    local hex  = ""
    for i=1,#json do hex=hex..string.format("%02X",string.byte(json,i)) end
    if setclipboard then setclipboard(hex); print("[PhysLab CFG] Код скопирован!") end
    return hex
end

_G.ImportCFG = function(hex)
    if not hex or hex=="" then warn("[PhysLab CFG] Код пуст!"); return false end
    local success, json = pcall(function()
        local res=""
        for i=1,#hex,2 do res=res..string.char(tonumber(hex:sub(i,i+1),16)) end
        return res
    end)
    if not success or not json:find("{") then warn("[PhysLab CFG] Неверный код!"); return false end
    local ok, data = pcall(function() return HttpService:JSONDecode(json) end)
    if ok and type(data)=="table" then
        for k,v in pairs(data) do
            if type(v)=="table" then Cfg[k]=Color3.new(v[1],v[2],v[3]) else Cfg[k]=v end
        end
        print("[PhysLab CFG] Настройки применены!")
        return true
    end
    return false
end

-- ════════════════════════════════════════════════════════════════════
-- ПЕРСОНАЖ HELPERS
-- ════════════════════════════════════════════════════════════════════
local function GetChar()
    Char = LP.Character
    if not Char then return false end
    Hum  = Char:FindFirstChildOfClass("Humanoid")
    HRP  = Char:FindFirstChild("HumanoidRootPart")
    Cam  = workspace.CurrentCamera
    return Hum ~= nil and HRP ~= nil
end

local function IsGround()
    return Hum and Hum.FloorMaterial ~= Enum.Material.Air
end

-- ════════════════════════════════════════════════════════════════════
-- ДВИЖЕНИЕ: QAccel + WishDir
-- ════════════════════════════════════════════════════════════════════
local function QAccel(vel, dir, wishSpd, accel, dt)
    local cur = vel:Dot(dir); local add = wishSpd - cur
    if add <= 0 then return vel end
    return vel + dir * math.min(accel*dt, add)
end

local function GetWishDir()
    local d = Vector3.zero
    if UIS:IsKeyDown(Enum.KeyCode.W) then d+=Vector3.new(0,0,-1) end
    if UIS:IsKeyDown(Enum.KeyCode.S) then d+=Vector3.new(0,0, 1) end
    if UIS:IsKeyDown(Enum.KeyCode.A) then d+=Vector3.new(-1,0,0) end
    if UIS:IsKeyDown(Enum.KeyCode.D) then d+=Vector3.new( 1,0,0) end
    if d.Magnitude<0.01 then return Vector3.zero end
    if not Cam then return d.Unit end
    local lv  = Cam.CFrame.LookVector
    local yaw = CFrame.Angles(0, math.atan2(-lv.X,-lv.Z), 0)
    return (yaw*d).Unit
end

-- ════════════════════════════════════════════════════════════════════
-- ДВИЖЕНИЕ: Spin / SmoothCam / Noclip / AntiRag / Boost / TeleportSwap
-- ════════════════════════════════════════════════════════════════════
local function DoSpin(dt)
    if not Cfg.SpinOn or not HRP then return end
    spinAngle=(spinAngle+Cfg.SpinSpeed*dt)%360
    local p=HRP.CFrame.Position
    HRP.CFrame=CFrame.new(p)*CFrame.Angles(0,math.rad(spinAngle),0)
end

local function DoSmoothCam(dt)
    if not Cfg.SmoothCamOn or not HRP or not Cam then return end
    if Cfg.SpinOn then return end
    local lv=Cam.CFrame.LookVector
    local targetYaw=math.atan2(-lv.X,-lv.Z)
    smoothYaw=smoothYaw+(targetYaw-smoothYaw)*math.min(Cfg.SmoothCamLerp*60*dt,1)
    local p=HRP.CFrame.Position
    HRP.CFrame=CFrame.new(p)*CFrame.Angles(0,smoothYaw,0)
end

local function SetNoclip(on)
    if noclipConn then noclipConn:Disconnect(); noclipConn=nil end
    if on and Char then
        noclipConn=RunService.Stepped:Connect(function()
            if not Char then return end
            for _,p in ipairs(Char:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide=false end
            end
        end)
    else
        if Char then for _,p in ipairs(Char:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide=true end end end
    end
end

local function SetAntiRag(on)
    if antiRagConn then antiRagConn:Disconnect(); antiRagConn=nil end
    if on and Hum then
        antiRagConn=Hum.StateChanged:Connect(function(_,new)
            if new==Enum.HumanoidStateType.FallingDown or new==Enum.HumanoidStateType.Ragdoll then
                Hum:ChangeState(Enum.HumanoidStateType.GettingUp)
            end
        end)
    end
end

local function SetBoost(on)
    if boostConn then boostConn:Disconnect(); boostConn=nil end
    if on then
        boostConn=UIS.InputBegan:Connect(function(i,g)
            if g or i.KeyCode~=Cfg.BoostKey then return end
            if boostActive then return end
            boostActive=true
            if ShowKeyHint then ShowKeyHint("Boost",Cfg.BoostKey) end
            if Hum then Hum.WalkSpeed=Cfg.BoostSpeed end
            if HRP then
                local dir=GetWishDir()
                if dir.Magnitude>0.01 then HRP.AssemblyLinearVelocity=dir*Cfg.BoostSpeed end
            end
            task.delay(Cfg.BoostDur,function()
                boostActive=false
                if Hum and not Cfg.FlyOn then Hum.WalkSpeed=Cfg.WalkSpeed end
            end)
        end)
    end
end

local function TeleportToCam()
    if not HRP or not Cam then return end
    local cf=Cam.CFrame; local origin=cf.Position; local dir=cf.LookVector
    local params=RaycastParams.new()
    params.FilterDescendantsInstances={Char}; params.FilterType=Enum.RaycastFilterType.Exclude
    local result=workspace:Raycast(origin,dir*80,params)
    local dest=result and (result.Position+Vector3.new(0,3,0)) or (origin+dir*50)
    HRP.CFrame=CFrame.new(dest)
end

local function DestroyMarker()
    if swapMarker then swapMarker:Destroy(); swapMarker=nil end
end

local function CreateMarker(pos)
    DestroyMarker()
    local m=Instance.new("Part",workspace)
    m.Name="SwapMarker"; m.Anchored=true; m.CanCollide=false
    m.Shape=Enum.PartType.Ball; m.Size=Vector3.new(1.5,1.5,1.5)
    m.Position=pos+Vector3.new(0,1.5,0); m.Material=Enum.Material.Neon
    m.Color=Color3.fromRGB(Cfg.SwapMarkerR,Cfg.SwapMarkerG,Cfg.SwapMarkerB); m.CastShadow=false
    local inner=Instance.new("Part",m)
    inner.Anchored=true; inner.CanCollide=false; inner.Shape=Enum.PartType.Ball
    inner.Size=Vector3.new(0.7,0.7,0.7); inner.Position=m.Position
    inner.Material=Enum.Material.Neon; inner.Color=Color3.new(1,1,1); inner.CastShadow=false
    local pl=Instance.new("PointLight",m); pl.Brightness=5; pl.Range=16
    pl.Color=Color3.fromRGB(Cfg.SwapMarkerR,Cfg.SwapMarkerG,Cfg.SwapMarkerB)
    TweenService:Create(m,TweenInfo.new(0.7,Enum.EasingStyle.Sine,Enum.EasingDirection.InOut,-1,true),{Size=Vector3.new(2,2,2)}):Play()
    TweenService:Create(pl,TweenInfo.new(0.7,Enum.EasingStyle.Sine,Enum.EasingDirection.InOut,-1,true),{Brightness=2}):Play()
    local orbiters={}; local orbitAngle=0
    for i=1,4 do
        local ob=Instance.new("Part",m); ob.Anchored=true; ob.CanCollide=false
        ob.Shape=Enum.PartType.Ball; ob.Size=Vector3.new(0.25,0.25,0.25)
        ob.Material=Enum.Material.Neon; ob.Color=Color3.fromRGB(Cfg.SwapMarkerR,Cfg.SwapMarkerG,Cfg.SwapMarkerB)
        ob.CastShadow=false; table.insert(orbiters,{part=ob,offset=math.pi*2/4*i})
    end
    local orbitConn
    orbitConn=RunService.Heartbeat:Connect(function(dt)
        if not m or not m.Parent then orbitConn:Disconnect(); return end
        orbitAngle=orbitAngle+dt*2; local center=m.Position
        for _,ob in ipairs(orbiters) do
            local a=orbitAngle+ob.offset
            ob.part.Position=center+Vector3.new(math.cos(a)*2.2,math.sin(orbitAngle*0.7)*0.5,math.sin(a)*2.2)
        end
        inner.Position=center
    end)
    swapMarker=m
end

local function BindSwapPos()
    if not HRP then return end; swapPos=HRP.Position; CreateMarker(swapPos)
end
local function UseSwap()
    if not swapPos or not HRP then return end
    HRP.CFrame=CFrame.new(swapPos+Vector3.new(0,3,0))
end

-- ════════════════════════════════════════════════════════════════════
-- VISUAL: Blur / FX / Trail / Fog / Sky
-- ════════════════════════════════════════════════════════════════════
local function SetBlur(on,size)
    if blurObj then blurObj:Destroy(); blurObj=nil end
    if on then blurObj=Instance.new("BlurEffect",Lighting); blurObj.Size=size or Cfg.BlurSize end
end
local function UpdateBlur()
    if Cfg.BlurOn then SetBlur(true,Cfg.BlurSize) else SetBlur(false) end
end

local function ClearFX()
    if fxConn then fxConn:Disconnect(); fxConn=nil end
    for _,p in ipairs(fxParts) do if p and p.Parent then p:Destroy() end end; fxParts={}
end
local FX_COLORS={
    Color3.fromRGB(80,200,255), Color3.fromRGB(255,100,200),
    Color3.fromRGB(100,255,150), Color3.fromRGB(255,220,80), Color3.fromRGB(200,100,255),
}
local function MakeFX()
    ClearFX(); if not Cfg.FXOn then return end
    local RADIUS=12; local COUNT=18; local parts={}
    for i=1,COUNT do
        local col=FX_COLORS[(i%#FX_COLORS)+1]
        local p=Instance.new("Part",workspace)
        p.Anchored=true; p.CanCollide=false; p.CastShadow=false
        p.Material=Enum.Material.Neon; p.Color=col
        if Cfg.FXType==1 then p.Shape=Enum.PartType.Ball; local sz=math.random(3,8)/10; p.Size=Vector3.new(sz,sz,sz)
        elseif Cfg.FXType==2 then p.Shape=Enum.PartType.Cylinder; p.Size=Vector3.new(0.1,math.random(5,12)/10,math.random(5,12)/10)
        else p.Shape=Enum.PartType.Block; p.Size=Vector3.new(0.08,math.random(3,8)/10,0.08) end
        local pl=Instance.new("PointLight",p); pl.Brightness=2; pl.Range=6; pl.Color=col
        local angle=math.random()*math.pi*2; local height=math.random(-4,8)
        local radius=math.random(3,RADIUS); local speed=(math.random()-0.5)*1.5+0.8
        local vSpeed=(math.random()-0.5)*1.5
        table.insert(parts,{part=p,angle=angle,height=height,radius=radius,speed=speed,vSpeed=vSpeed,baseH=height,col=col})
        table.insert(fxParts,p)
    end
    local t=0
    fxConn=RunService.Heartbeat:Connect(function(dt)
        if not HRP then return end; t=t+dt
        local base=HRP.Position
        for _,d in ipairs(parts) do
            if not d.part.Parent then continue end
            d.angle=d.angle+d.speed*dt
            d.height=d.baseH+math.sin(t*d.vSpeed+d.angle)*2
            local x=base.X+math.cos(d.angle)*d.radius
            local z=base.Z+math.sin(d.angle)*d.radius
            local y=base.Y+d.height
            if Cfg.FXType==2 then d.part.CFrame=CFrame.new(x,y,z)*CFrame.Angles(0,d.angle,math.pi/2)
            else d.part.Position=Vector3.new(x,y,z) end
        end
    end)
end

local function SetTrail(on)
    if trailObj then trailObj:Destroy(); trailObj=nil end
    if on and HRP then
        local a0=Instance.new("Attachment",HRP); a0.Position=Vector3.new(0,1,0)
        local a1=Instance.new("Attachment",HRP); a1.Position=Vector3.new(0,-1,0)
        local t=Instance.new("Trail",HRP)
        t.Attachment0=a0; t.Attachment1=a1; t.Lifetime=Cfg.TrailLife
        local c1=Color3.fromRGB(Cfg.TrailR1,Cfg.TrailG1,Cfg.TrailB1)
        local c2=Color3.fromRGB(Cfg.TrailR2,Cfg.TrailG2,Cfg.TrailB2)
        t.Color=ColorSequence.new(c1,c2)
        t.WidthScale=NumberSequence.new({NumberSequenceKeypoint.new(0,Cfg.TrailWidth),NumberSequenceKeypoint.new(1,0)})
        t.Transparency=NumberSequence.new({NumberSequenceKeypoint.new(0,0.2),NumberSequenceKeypoint.new(1,1)})
        trailObj=t
    end
end

local atmObj=nil
local function ApplyFog()
    if atmObj then atmObj:Destroy(); atmObj=nil end
    if Cfg.FogOn then
        local col=Color3.fromRGB(Cfg.FogR,Cfg.FogG,Cfg.FogB)
        atmObj=Instance.new("Atmosphere",Lighting)
        atmObj.Density=0.4; atmObj.Color=col; atmObj.Haze=math.clamp(Cfg.FogEnd/200,0,2.5)
        Lighting.FogColor=col; Lighting.FogStart=Cfg.FogStart; Lighting.FogEnd=Cfg.FogEnd
    else Lighting.FogEnd=100000 end
end

local SkyPresets={
    {name="Default",  time=14, bright=2,    amb=Color3.fromRGB(128,128,128)},
    {name="Night",    time=0,  bright=0.2,  amb=Color3.fromRGB(20,20,40)},
    {name="Sunset",   time=19, bright=1.5,  amb=Color3.fromRGB(180,100,60)},
    {name="Overcast", time=12, bright=0.7,  amb=Color3.fromRGB(100,110,120)},
    {name="Midnight", time=2,  bright=0.05, amb=Color3.fromRGB(5,5,15)},
}
local function ApplySky(i)
    local p=SkyPresets[i]; if not p then return end
    TweenService:Create(Lighting,TweenInfo.new(1.5,Enum.EasingStyle.Sine),{ClockTime=p.time,Brightness=p.bright,Ambient=p.amb,OutdoorAmbient=p.amb}):Play()
end

local function Reset()
    if Hum then Hum.WalkSpeed=16; Hum.JumpPower=50 end
    workspace.Gravity=196.2; if Cam then Cam.FieldOfView=70 end
    if atmObj then atmObj:Destroy(); atmObj=nil end; Lighting.FogEnd=100000
    SetBlur(false); ClearFX(); DestroyMarker(); ApplySky(1); SetNoclip(false); SetAntiRag(false); SetBoost(false)
end

-- ════════════════════════════════════════════════════════════════════
-- MOVE V2
-- ════════════════════════════════════════════════════════════════════
local function ClearMoveV2()
    if moveV2Obj then moveV2Obj:Destroy(); moveV2Obj=nil end
    if Hum then Hum.WalkSpeed=Cfg.WalkSpeed end
end
local function SetupMoveV2()
    ClearMoveV2(); if not HRP then return end
    if Cfg.MoveMethod=="LinearVelocity" then
        local att=HRP:FindFirstChild("MoveV2Attach") or Instance.new("Attachment",HRP)
        att.Name="MoveV2Attach"
        local lv=Instance.new("LinearVelocity"); lv.Name="MoveV2LV"
        lv.Attachment0=att; lv.MaxForce=100000
        lv.VectorVelocity=Vector3.zero; lv.RelativeTo=Enum.ActuatorRelativeTo.World; lv.Parent=HRP
        moveV2Obj=lv; Hum.WalkSpeed=0
    elseif Cfg.MoveMethod=="BodyVelocity" then
        local bv=Instance.new("BodyVelocity"); bv.Name="MoveV2BV"
        bv.MaxForce=Vector3.new(1e5,0,1e5); bv.Velocity=Vector3.zero; bv.Parent=HRP
        moveV2Obj=bv; Hum.WalkSpeed=0
    else Hum.WalkSpeed=Cfg.WalkSpeed end
end
local function ApplyMoveV2(dt)
    if Cfg.MoveMethod=="WalkSpeed" or not moveV2Obj then return end
    local wd=GetWishDir(); local targetVel=wd*Cfg.MoveV2Speed
    if moveV2Obj:IsA("LinearVelocity") then
        local v=HRP.AssemblyLinearVelocity
        moveV2Obj.VectorVelocity=Vector3.new(targetVel.X,v.Y,targetVel.Z)
    elseif moveV2Obj:IsA("BodyVelocity") then
        moveV2Obj.Velocity=Vector3.new(targetVel.X,0,targetVel.Z)
    end
end

-- ════════════════════════════════════════════════════════════════════
-- MAIN LOOP
-- ════════════════════════════════════════════════════════════════════
local function DoStep(dt)
    if not GetChar() then return end
    if not boostActive then
        if Cfg.MoveMethod~="WalkSpeed" then
            -- ничего, скорость через MoveV2
        elseif Cfg.BhopOn and Cfg.BhopStreak>0 then
            local v=HRP and HRP.AssemblyLinearVelocity or Vector3.zero
            Hum.WalkSpeed=math.max(Cfg.WalkSpeed,Vector3.new(v.X,0,v.Z).Magnitude)
        else Hum.WalkSpeed=Cfg.WalkSpeed end
    end
    Hum.JumpPower=Cfg.JumpPower; workspace.Gravity=Cfg.Gravity
    if Cam then Cam.FieldOfView=Cfg.FOV end
    ApplyMoveV2(dt); DoSmoothCam(dt)
    if not Cfg.SmoothCamOn then DoSpin(dt) end

    if Cfg.FlyOn then
        local dir=Vector3.zero
        if UIS:IsKeyDown(Enum.KeyCode.W) then dir+=Cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then dir-=Cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then dir-=Cam.CFrame.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then dir+=Cam.CFrame.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then dir+=Vector3.new(0,1,0) end
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir-=Vector3.new(0,1,0) end
        if dir.Magnitude>0 then dir=dir.Unit end
        flyVel=flyVel:Lerp(dir*Cfg.FlySpeed,math.min(8*dt,1))
        HRP.AssemblyLinearVelocity=flyVel
        HRP:ApplyImpulse(Vector3.new(0,workspace.Gravity*HRP.AssemblyMass*dt,0))
        return
    end

    flyVel=Vector3.zero
    local onGround=IsGround()
    local v=HRP.AssemblyLinearVelocity
    local hor=Vector3.new(v.X,0,v.Z)

    if Cfg.BhopOn then
        local spaceHeld=UIS:IsKeyDown(Enum.KeyCode.Space)
        if Cfg.BhopAuto and onGround and spaceHeld then Hum.Jump=true end
        if onGround and not wasGround then
            if spaceHeld then
                Cfg.BhopStreak=Cfg.BhopStreak+1
                local gained=hor*Cfg.BhopGain
                if gained.Magnitude>Cfg.BhopMax then gained=gained.Unit*Cfg.BhopMax end
                task.defer(function()
                    if HRP then
                        local cur=HRP.AssemblyLinearVelocity
                        HRP.AssemblyLinearVelocity=Vector3.new(gained.X,cur.Y,gained.Z)
                    end
                end)
            else Cfg.BhopStreak=0 end
        end
    else Cfg.BhopStreak=0 end
    wasGround=onGround

    if Cfg.DoubleJumpOn and onGround then hasDoubleJumped=false end

    if Cfg.InfJumpOn then
        local now=os.clock()
        if UIS:IsKeyDown(Enum.KeyCode.Space) and (now-lastInfJumpTime)>=Cfg.InfJumpCooldown then
            Hum.Jump=true; lastInfJumpTime=now
        end
    end

    if Cfg.StrafeOn and not onGround then
        local wd=GetWishDir()
        if wd.Magnitude>0.01 then
            local newHor=QAccel(hor,wd,Cfg.StrafeMax,Cfg.StrafeAccel,dt)
            HRP.AssemblyLinearVelocity=Vector3.new(newHor.X,v.Y,newHor.Z)
        end
    end

    if Cfg.ADStrafeOn and Cam then
        local aHeld=UIS:IsKeyDown(Enum.KeyCode.A); local dHeld=UIS:IsKeyDown(Enum.KeyCode.D)
        if aHeld~=dHeld then
            local right=Cam.CFrame.RightVector
            local sideDir=(aHeld and -right or right)
            sideDir=Vector3.new(sideDir.X,0,sideDir.Z)
            if sideDir.Magnitude>0.01 then
                sideDir=sideDir.Unit
                HRP.AssemblyLinearVelocity=Vector3.new(sideDir.X*Cfg.ADStrafeSpeed,v.Y,sideDir.Z*Cfg.ADStrafeSpeed)
            end
        end
    end

    if Cfg.DrunkOn and onGround then
        local wd=GetWishDir(); local curHor=Vector3.new(v.X,0,v.Z)
        local newHor=curHor*Cfg.DrunkSlide
        if wd.Magnitude>0.01 then newHor=QAccel(newHor,wd,Cfg.DrunkMaxSpeed,Cfg.DrunkAccel,dt) end
        HRP.AssemblyLinearVelocity=Vector3.new(newHor.X,v.Y,newHor.Z)
    end
end

local function StartLoop()
    if loopConn then loopConn:Disconnect() end
    loopConn=RunService.Heartbeat:Connect(DoStep)
end

-- ════════════════════════════════════════════════════════════════════
-- FPS & PING
-- ════════════════════════════════════════════════════════════════════
local function SetupFpsPing()
    if fpsPingConn then fpsPingConn:Disconnect(); fpsPingConn=nil end
    if not Cfg.FpsPingOn then
        if fpsPingLabel then fpsPingLabel.Visible=false end; return
    end
    if not fpsPingLabel or not fpsPingLabel.Parent then return end
    fpsPingLabel.Visible=true
    local frameCount=0; local fpsTimer=0; local curFps=60
    fpsPingConn=RunService.Heartbeat:Connect(function(dt)
        frameCount=frameCount+1; fpsTimer=fpsTimer+dt
        if fpsTimer>=0.5 then
            curFps=math.floor(frameCount/fpsTimer+0.5); frameCount=0; fpsTimer=0
            local ping=0
            pcall(function() ping=math.floor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue()) end)
            if not Cfg.FpsPingOn then return end
            fpsPingLabel.Text=string.format("FPS: %d   PING: %dms",curFps,ping)
        end
    end)
end

-- ════════════════════════════════════════════════════════════════════
-- JUMP RINGS
-- ════════════════════════════════════════════════════════════════════
local jumpRingsPlayerConn={}

local function spawnJumpRingAt(position)
    if not Cfg.JumpRingsOn then return end
    local color=Color3.fromRGB(Cfg.JumpRingsColorR,Cfg.JumpRingsColorG,Cfg.JumpRingsColorB)
    local radius=math.max(0.5,Cfg.JumpRingsRadius)
    local thickness=math.clamp(Cfg.JumpRingsThickness,0.05,radius-0.1)
    local height=math.max(0.05,Cfg.JumpRingsHeight)
    local model=Instance.new("Model"); model.Name="JumpRing"
    local outer=Instance.new("Part"); outer.Shape=Enum.PartType.Cylinder
    outer.Anchored=true; outer.CanCollide=false; outer.CastShadow=false
    outer.Material=Enum.Material.Neon; outer.Color=color
    outer.Transparency=Cfg.JumpRingsTransparency
    outer.Size=Vector3.new(height,radius*2,radius*2)
    outer.CFrame=CFrame.new(position)*CFrame.Angles(0,0,math.rad(90))
    outer.Parent=model
    local inner=Instance.new("Part"); inner.Shape=Enum.PartType.Cylinder
    inner.Anchored=true; inner.CanCollide=false; inner.CastShadow=false
    inner.Material=Enum.Material.Neon; inner.Color=color; inner.Transparency=1
    local innerRadius=math.max(0.1,radius-thickness)
    inner.Size=Vector3.new(height+0.02,innerRadius*2,innerRadius*2)
    inner.CFrame=CFrame.new(position)*CFrame.Angles(0,0,math.rad(90))
    inner.Parent=model; model.PrimaryPart=outer; model.Parent=workspace
    local highlight=nil
    if Cfg.JumpRingsOutline then
        highlight=Instance.new("Highlight",model); highlight.FillTransparency=1
        highlight.OutlineColor=color; highlight.OutlineTransparency=0
        highlight.DepthMode=Enum.HighlightDepthMode.AlwaysOnTop
    end
    task.delay(Cfg.JumpRingsLifetime,function()
        if not model or not model.Parent then return end
        local fadeTime=0.5
        TweenService:Create(outer,TweenInfo.new(fadeTime),{Transparency=1}):Play()
        if highlight then TweenService:Create(highlight,TweenInfo.new(fadeTime),{OutlineTransparency=1}):Play() end
        task.delay(fadeTime+0.05,function() if model and model.Parent then model:Destroy() end end)
    end)
end

local function trackPlayerJumps(player)
    if jumpRingsPlayerConn[player] then jumpRingsPlayerConn[player]:Disconnect(); jumpRingsPlayerConn[player]=nil end
    local char=player.Character; if not char then return end
    local hum=char:FindFirstChildOfClass("Humanoid"); local root=char:FindFirstChild("HumanoidRootPart")
    if not hum or not root then return end
    local wasOnGround=true
    jumpRingsPlayerConn[player]=RunService.Heartbeat:Connect(function()
        if not Cfg.JumpRingsOn then return end
        if not ((player==LP and Cfg.JumpRingsMode=="Self") or Cfg.JumpRingsMode=="All") then return end
        if not hum or not hum.Parent or not root or not root.Parent then return end
        local onGround=hum.FloorMaterial~=Enum.Material.Air
        if wasOnGround and not onGround then
            spawnJumpRingAt(root.Position-Vector3.new(0,(hum.HipHeight or 2)+1,0))
        end
        wasOnGround=onGround
    end)
end

startJumpRings=function()
    stopJumpRings(); if not Cfg.JumpRingsOn then return end
    for _,p in ipairs(Players:GetPlayers()) do
        if (p==LP and Cfg.JumpRingsMode=="Self") or Cfg.JumpRingsMode=="All" then
            trackPlayerJumps(p)
        end
    end
end
stopJumpRings=function()
    for p,conn in pairs(jumpRingsPlayerConn) do if conn then conn:Disconnect() end end
    jumpRingsPlayerConn={}
end
restartJumpRings=function()
    if Cfg.JumpRingsOn then stopJumpRings(); startJumpRings() end
end

Players.PlayerAdded:Connect(function(p)
    p.CharacterAdded:Connect(function()
        task.wait(0.5)
        if Cfg.JumpRingsOn and ((p==LP and Cfg.JumpRingsMode=="Self") or Cfg.JumpRingsMode=="All") then
            trackPlayerJumps(p)
        end
    end)
    if Cfg.JumpRingsOn and Cfg.JumpRingsMode=="All" then trackPlayerJumps(p) end
end)
Players.PlayerRemoving:Connect(function(p)
    if jumpRingsPlayerConn[p] then jumpRingsPlayerConn[p]:Disconnect(); jumpRingsPlayerConn[p]=nil end
end)

-- ════════════════════════════════════════════════════════════════════
-- ██████╗ ██╗   ██╗██╗
-- ██╔════╝ ██║   ██║██║
-- ██║  ███╗██║   ██║██║
-- ██║   ██║██║   ██║██║
-- ╚██████╔╝╚██████╔╝██║
--  ╚═════╝  ╚═════╝ ╚═╝   LIQUID GLASS
-- ════════════════════════════════════════════════════════════════════

-- Глобальный blur мира (backdrop-filter imitation)
local function EnsureGlobalBlur(size)
    local blur=Lighting:FindFirstChild("LiquidGlassBlur")
    if not blur then
        blur=Instance.new("BlurEffect"); blur.Name="LiquidGlassBlur"; blur.Size=0; blur.Parent=Lighting
    end
    TweenService:Create(blur,TweenInfo.new(0.35,Enum.EasingStyle.Quint),{Size=size}):Play()
end
local function ClearGlobalBlur()
    local blur=Lighting:FindFirstChild("LiquidGlassBlur")
    if blur then TweenService:Create(blur,TweenInfo.new(0.3),{Size=0}):Play() end
end

-- Стеклянная панель (base + UIStroke + gradient + highlight)
local function MakeGlassPanel(parent,cornerRadius,bgTransp)
    local panel=Instance.new("Frame")
    panel.BackgroundColor3=GLASS_BG
    panel.BackgroundTransparency=bgTransp or 0.18
    panel.BorderSizePixel=0; panel.Parent=parent
    local corner=Instance.new("UICorner"); corner.CornerRadius=UDim.new(0,cornerRadius or 22); corner.Parent=panel
    local stroke=Instance.new("UIStroke"); stroke.Color=GL_STROKE; stroke.Thickness=1
    stroke.Transparency=0.72; stroke.ApplyStrokeMode=Enum.ApplyStrokeMode.Border; stroke.Parent=panel
    local gradient=Instance.new("UIGradient")
    gradient.Color=ColorSequence.new({
        ColorSequenceKeypoint.new(0,Color3.fromRGB(255,255,255)),
        ColorSequenceKeypoint.new(0.4,Color3.fromRGB(255,255,255)),
        ColorSequenceKeypoint.new(1,Color3.fromRGB(180,180,190)),
    })
    gradient.Transparency=NumberSequence.new({
        NumberSequenceKeypoint.new(0,0.84),
        NumberSequenceKeypoint.new(0.5,0.97),
        NumberSequenceKeypoint.new(1,0.91),
    })
    gradient.Rotation=90; gradient.Parent=panel
    local highlight=Instance.new("Frame"); highlight.Size=UDim2.new(0.0,0,0.0,0)
    highlight.Position=UDim2.new(0.03,0,0.03,0); highlight.BackgroundColor3=Color3.new(1,1,1)
    highlight.BackgroundTransparency=0.92; highlight.BorderSizePixel=0; highlight.ZIndex=panel.ZIndex+1
    Instance.new("UICorner",highlight).CornerRadius=UDim.new(1,0)
    highlight.Parent=panel
    return panel
end

-- Стеклянная «строка» — облегчённая версия без highlight
local function MakeGlassRow(parent,h,cr)
    local row=MakeGlassPanel(parent,cr or 14,0.42)
    row.Size=UDim2.new(1,0,0,h)
    return row
end

-- ════════════════════════════════════════════════════════════════════
-- BUILD GUI — LIQUID GLASS
-- ════════════════════════════════════════════════════════════════════
local function BuildGUI()
    local old=PGui:FindFirstChild("PhysLab")
    if old then old:Destroy() end

    local SG=Instance.new("ScreenGui")
    SG.Name="PhysLab"; SG.ResetOnSpawn=false; SG.IgnoreGuiInset=true
    SG.ZIndexBehavior=Enum.ZIndexBehavior.Sibling
    SG.DisplayOrder=999999999; SG.Parent=PGui

    EnsureGlobalBlur(0)

    -- ── Размеры окна ──
    local WIN_W, WIN_H = 360, 520

    -- ── Тень ──
    local Shadow=Instance.new("ImageLabel",SG)
    Shadow.Image="rbxasset://textures/ui/Controls/DropShadow.png"
    Shadow.ScaleType=Enum.ScaleType.Slice; Shadow.SliceCenter=Rect.new(10,10,118,118)
    Shadow.Size=UDim2.new(0,WIN_W+60,0,WIN_H+60)
    Shadow.Position=UDim2.new(0,-10,0,66)
    Shadow.BackgroundTransparency=1; Shadow.ImageColor3=Color3.new(0,0,0)
    Shadow.ImageTransparency=0.60; Shadow.ZIndex=8

    -- ── Главное окно ──
    local Win=MakeGlassPanel(SG,26)
    Win.Name="Win"
    Win.Size=UDim2.new(0,WIN_W,0,WIN_H)
    Win.Position=UDim2.new(0,16,0,70)
    Win.ZIndex=10; Win.ClipsDescendants=false

    -- ── Toggle-кнопка (✦) ──
    local ToggleBtn=MakeGlassPanel(SG,20)
    ToggleBtn.Size=UDim2.new(0,44,0,44); ToggleBtn.Position=UDim2.new(0,16,0,16); ToggleBtn.ZIndex=50
    local ToggleIcon=Instance.new("TextLabel",ToggleBtn)
    ToggleIcon.Size=UDim2.new(1,0,1,0); ToggleIcon.BackgroundTransparency=1
    ToggleIcon.Text="✦"; ToggleIcon.TextColor3=GL_ACCENT2
    ToggleIcon.Font=Enum.Font.GothamBold; ToggleIcon.TextSize=18; ToggleIcon.ZIndex=51
    local ToggleHit=Instance.new("TextButton",ToggleBtn)
    ToggleHit.Size=UDim2.new(1,0,1,0); ToggleHit.BackgroundTransparency=1; ToggleHit.Text=""; ToggleHit.ZIndex=52

    -- ── HEADER ──
    local Header=Instance.new("Frame",Win)
    Header.Size=UDim2.new(1,0,0,62); Header.BackgroundTransparency=1; Header.ZIndex=11; Header.Active=true

    local TitleL=Instance.new("TextLabel",Header)
    TitleL.Position=UDim2.new(0,20,0,10); TitleL.Size=UDim2.new(1,-70,0,22)
    TitleL.BackgroundTransparency=1; TitleL.Text="PHYSICS LAB"
    TitleL.TextColor3=GL_TEXT; TitleL.Font=Enum.Font.GothamBold; TitleL.TextSize=16
    TitleL.TextXAlignment=Enum.TextXAlignment.Left; TitleL.ZIndex=12

    local SubL=Instance.new("TextLabel",Header)
    SubL.Position=UDim2.new(0,20,0,33); SubL.Size=UDim2.new(1,-70,0,16)
    SubL.BackgroundTransparency=1; SubL.Text="liquid glass  •  v8  •  movement + fx + rings"
    SubL.TextColor3=GL_SUB; SubL.Font=Enum.Font.Gotham; SubL.TextSize=10
    SubL.TextXAlignment=Enum.TextXAlignment.Left; SubL.ZIndex=12

    -- iOS close dot
    local XBtn=Instance.new("TextButton",Header)
    XBtn.Size=UDim2.new(0,28,0,28); XBtn.Position=UDim2.new(1,-40,0,14)
    XBtn.BackgroundColor3=Color3.new(1,1,1); XBtn.BackgroundTransparency=0.88
    XBtn.BorderSizePixel=0; XBtn.Text="✕"; XBtn.TextColor3=GL_DANGER
    XBtn.Font=Enum.Font.GothamBold; XBtn.TextSize=11; XBtn.ZIndex=13
    Instance.new("UICorner",XBtn).CornerRadius=UDim.new(1,0)
    local xStroke=Instance.new("UIStroke",XBtn); xStroke.Color=GL_STROKE; xStroke.Thickness=1; xStroke.Transparency=0.82

    -- Разделитель
    local Sep=Instance.new("Frame",Win)
    Sep.Size=UDim2.new(1,-32,0,1); Sep.Position=UDim2.new(0,16,0,62)
    Sep.BackgroundColor3=Color3.new(1,1,1); Sep.BackgroundTransparency=0.88; Sep.BorderSizePixel=0; Sep.ZIndex=11

    -- ── TAB BAR (pill segmented control) ──
    local TabBarHolder=Instance.new("Frame",Win)
    TabBarHolder.Size=UDim2.new(1,-32,0,36); TabBarHolder.Position=UDim2.new(0,16,0,71)
    TabBarHolder.BackgroundColor3=Color3.new(0,0,0); TabBarHolder.BackgroundTransparency=0.72
    TabBarHolder.BorderSizePixel=0; TabBarHolder.ZIndex=11
    Instance.new("UICorner",TabBarHolder).CornerRadius=UDim.new(0,12)

    local TabBar=Instance.new("Frame",TabBarHolder)
    TabBar.Size=UDim2.new(1,-6,1,-6); TabBar.Position=UDim2.new(0,3,0,3)
    TabBar.BackgroundTransparency=1; TabBar.ZIndex=12
    local TabLL=Instance.new("UIListLayout",TabBar)
    TabLL.FillDirection=Enum.FillDirection.Horizontal
    TabLL.SortOrder=Enum.SortOrder.LayoutOrder; TabLL.Padding=UDim.new(0,2)

    -- Скользящий pill-индикатор активного таба
    local TabPill=Instance.new("Frame",TabBar)
    TabPill.BackgroundColor3=Color3.new(1,1,1); TabPill.BackgroundTransparency=0.84
    TabPill.BorderSizePixel=0; TabPill.ZIndex=11; TabPill.Size=UDim2.new(0,0,1,0)
    Instance.new("UICorner",TabPill).CornerRadius=UDim.new(0,9)

    -- ── CONTENT ──
    local Content=Instance.new("Frame",Win)
    Content.Size=UDim2.new(1,0,1,-117); Content.Position=UDim2.new(0,0,0,117)
    Content.BackgroundTransparency=1; Content.ClipsDescendants=true; Content.ZIndex=11

    -- ══════════════════════════════════════════
    -- TAB SYSTEM
    -- ══════════════════════════════════════════
    local tabMap={}
    local activeTabName=nil

    local function SwitchTab(name)
        if activeTabName==name then return end
        activeTabName=name
        for n,t in pairs(tabMap) do
            local isActive=(n==name)
            TweenService:Create(t.btn,TweenInfo.new(0.18),{TextColor3=isActive and GL_TEXT or GL_SUB}):Play()
            if isActive then
                t.page.Visible=true
                TweenService:Create(TabPill,TweenInfo.new(0.25,Enum.EasingStyle.Quint),{
                    Size=UDim2.new(0,t.btn.AbsoluteSize.X,1,0),
                    Position=UDim2.new(0,t.btn.Position.X.Offset,0,0)
                }):Play()
            else
                t.page.Visible=false
            end
        end
    end

    local function NewTab(name,label,order)
        local B=Instance.new("TextButton",TabBar)
        B.LayoutOrder=order
        -- Динамическая ширина по количеству табов
        B.Size=UDim2.new(0,math.floor((WIN_W-38)/9),1,0)
        B.BackgroundTransparency=1; B.BorderSizePixel=0
        B.Text=label; B.TextColor3=GL_SUB
        B.Font=Enum.Font.GothamBold; B.TextSize=9; B.ZIndex=13

        local P=Instance.new("ScrollingFrame",Content)
        P.Size=UDim2.new(1,0,1,0); P.BackgroundTransparency=1; P.BorderSizePixel=0
        P.ScrollBarThickness=3; P.ScrollBarImageColor3=Color3.new(1,1,1)
        P.ScrollBarImageTransparency=0.7; P.CanvasSize=UDim2.new(0,0,0,0); P.Visible=false; P.ZIndex=11
        local L=Instance.new("UIListLayout",P)
        L.SortOrder=Enum.SortOrder.LayoutOrder; L.Padding=UDim.new(0,6)
        local Pad=Instance.new("UIPadding",P)
        Pad.PaddingLeft=UDim.new(0,14); Pad.PaddingRight=UDim.new(0,14)
        Pad.PaddingTop=UDim.new(0,10); Pad.PaddingBottom=UDim.new(0,16)
        L:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            P.CanvasSize=UDim2.new(0,0,0,L.AbsoluteContentSize.Y+22)
        end)
        tabMap[name]={btn=B,page=P}
        B.MouseButton1Click:Connect(function() SwitchTab(name) end)
        return P
    end

    -- ══════════════════════════════════════════
    -- UI BUILDERS — LIQUID GLASS STYLE
    -- ══════════════════════════════════════════

    -- Секция-заголовок
    local function S(parent,text,order)
        local f=Instance.new("Frame",parent)
        f.LayoutOrder=order; f.Size=UDim2.new(1,0,0,24); f.BackgroundTransparency=1
        local l=Instance.new("TextLabel",f)
        l.Size=UDim2.new(1,0,1,0); l.BackgroundTransparency=1
        l.Text=text:upper(); l.TextColor3=GL_ACCENT2
        l.Font=Enum.Font.GothamBold; l.TextSize=10
        l.TextXAlignment=Enum.TextXAlignment.Left; l.Position=UDim2.new(0,0,0,6)
    end

    -- Toggle (iOS switch)
    local function T(parent,ltext,htext,key,order,cb)
        local Row=MakeGlassRow(parent,52)
        Row.LayoutOrder=order; Row.ZIndex=12

        local L1=Instance.new("TextLabel",Row)
        L1.Position=UDim2.new(0,14,0,8); L1.Size=UDim2.new(1,-76,0,18)
        L1.BackgroundTransparency=1; L1.Text=ltext
        L1.TextColor3=GL_TEXT; L1.Font=Enum.Font.GothamSemibold; L1.TextSize=13
        L1.TextXAlignment=Enum.TextXAlignment.Left; L1.ZIndex=14

        local L2=Instance.new("TextLabel",Row)
        L2.Position=UDim2.new(0,14,0,28); L2.Size=UDim2.new(1,-76,0,15)
        L2.BackgroundTransparency=1; L2.Text=htext
        L2.TextColor3=GL_SUB; L2.Font=Enum.Font.Gotham; L2.TextSize=10
        L2.TextXAlignment=Enum.TextXAlignment.Left; L2.ZIndex=14

        local Switch=Instance.new("Frame",Row)
        Switch.Size=UDim2.new(0,46,0,27); Switch.Position=UDim2.new(1,-58,0.5,-13.5)
        Switch.BackgroundColor3=Color3.fromRGB(55,55,65); Switch.BorderSizePixel=0; Switch.ZIndex=14
        Instance.new("UICorner",Switch).CornerRadius=UDim.new(1,0)

        local Knob=Instance.new("Frame",Switch)
        Knob.Size=UDim2.new(0,22,0,22); Knob.Position=UDim2.new(0,2,0.5,-11)
        Knob.BackgroundColor3=Color3.new(1,1,1); Knob.BorderSizePixel=0; Knob.ZIndex=15
        Instance.new("UICorner",Knob).CornerRadius=UDim.new(1,0)
        local kStroke=Instance.new("UIStroke",Knob); kStroke.Color=Color3.new(0,0,0); kStroke.Thickness=0.5; kStroke.Transparency=0.82

        local function Refresh(anim)
            local on=Cfg[key]; local sp=anim and 0.16 or 0
            TweenService:Create(Switch,TweenInfo.new(sp),{BackgroundColor3=on and GL_SUCCESS or Color3.fromRGB(55,55,65)}):Play()
            TweenService:Create(Knob,TweenInfo.new(sp,Enum.EasingStyle.Back,Enum.EasingDirection.Out),{
                Position=on and UDim2.new(1,-24,0.5,-11) or UDim2.new(0,2,0.5,-11)
            }):Play()
        end
        Refresh(false)

        local Hit=Instance.new("TextButton",Row)
        Hit.Size=UDim2.new(1,0,1,0); Hit.BackgroundTransparency=1; Hit.Text=""; Hit.BorderSizePixel=0; Hit.ZIndex=16
        Hit.MouseButton1Click:Connect(function()
            Cfg[key]=not Cfg[key]; Refresh(true); if cb then cb(Cfg[key]) end
        end)
    end

    -- Slider
    local function SL(parent,ltext,htext,key,mn,mx,stp,order,cb)
        local Row=MakeGlassRow(parent,62)
        Row.LayoutOrder=order; Row.ZIndex=12

        local L1=Instance.new("TextLabel",Row)
        L1.Position=UDim2.new(0,14,0,8); L1.Size=UDim2.new(1,-80,0,17)
        L1.BackgroundTransparency=1; L1.Text=ltext
        L1.TextColor3=GL_TEXT; L1.Font=Enum.Font.GothamSemibold; L1.TextSize=13
        L1.TextXAlignment=Enum.TextXAlignment.Left; L1.ZIndex=14

        local L2=Instance.new("TextLabel",Row)
        L2.Position=UDim2.new(0,14,0,26); L2.Size=UDim2.new(0.6,0,0,14)
        L2.BackgroundTransparency=1; L2.Text=htext
        L2.TextColor3=GL_SUB; L2.Font=Enum.Font.Gotham; L2.TextSize=10
        L2.TextXAlignment=Enum.TextXAlignment.Left; L2.ZIndex=14

        local VL=Instance.new("TextLabel",Row)
        VL.Position=UDim2.new(1,-72,0,5); VL.Size=UDim2.new(0,66,0,22)
        VL.BackgroundTransparency=1; VL.TextColor3=GL_ACCENT2
        VL.Font=Enum.Font.GothamBold; VL.TextSize=15; VL.TextXAlignment=Enum.TextXAlignment.Right; VL.ZIndex=14

        local Track=Instance.new("Frame",Row)
        Track.Size=UDim2.new(1,-28,0,5); Track.Position=UDim2.new(0,14,0,48)
        Track.BackgroundColor3=Color3.new(1,1,1); Track.BackgroundTransparency=0.84; Track.BorderSizePixel=0; Track.ZIndex=14
        Instance.new("UICorner",Track).CornerRadius=UDim.new(1,0)

        local FillF=Instance.new("Frame",Track)
        FillF.BackgroundColor3=GL_ACCENT; FillF.BorderSizePixel=0; FillF.Size=UDim2.new(0,0,1,0); FillF.ZIndex=15
        Instance.new("UICorner",FillF).CornerRadius=UDim.new(1,0)

        local Knob=Instance.new("Frame",Track)
        Knob.Size=UDim2.new(0,16,0,16); Knob.AnchorPoint=Vector2.new(0.5,0.5)
        Knob.Position=UDim2.new(0,0,0.5,0); Knob.BackgroundColor3=Color3.new(1,1,1); Knob.BorderSizePixel=0; Knob.ZIndex=16
        Instance.new("UICorner",Knob).CornerRadius=UDim.new(1,0)
        local kStr=Instance.new("UIStroke",Knob); kStr.Color=Color3.new(0,0,0); kStr.Thickness=0.5; kStr.Transparency=0.82

        local Hit=Instance.new("TextButton",Track)
        Hit.Size=UDim2.new(1,0,0,30); Hit.Position=UDim2.new(0,0,0.5,-15)
        Hit.BackgroundTransparency=1; Hit.Text=""; Hit.BorderSizePixel=0; Hit.ZIndex=17

        local function SetV(v)
            v=math.clamp(math.round(v/stp)*stp,mn,mx); Cfg[key]=v
            local t2=(v-mn)/(mx-mn)
            TweenService:Create(FillF,TweenInfo.new(0.06),{Size=UDim2.new(t2,0,1,0)}):Play()
            TweenService:Create(Knob,TweenInfo.new(0.06),{Position=UDim2.new(t2,0,0.5,0)}):Play()
            VL.Text=(stp<1) and string.format("%.2f",v) or tostring(math.floor(v))
            if cb then cb(v) end
        end
        SetV(Cfg[key])

        local dragging=false
        Hit.InputBegan:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then
                dragging=true; TweenService:Create(Knob,TweenInfo.new(0.1),{Size=UDim2.new(0,20,0,20)}):Play()
            end
        end)
        Hit.InputEnded:Connect(function(i)
            if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then
                dragging=false; TweenService:Create(Knob,TweenInfo.new(0.1),{Size=UDim2.new(0,16,0,16)}):Play()
            end
        end)
        UIS.InputChanged:Connect(function(i)
            if dragging and (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) then
                local rel=math.clamp((i.Position.X-Track.AbsolutePosition.X)/Track.AbsoluteSize.X,0,1)
                SetV(mn+rel*(mx-mn))
            end
        end)
    end

    -- Кнопка-действие
    local function BTN(parent,label,hint,order,btnLabel,col,cb)
        if type(col)=="function" then cb=col; col=nil end
        local Row=MakeGlassRow(parent,52)
        Row.LayoutOrder=order; Row.ZIndex=12

        local L1=Instance.new("TextLabel",Row)
        L1.Position=UDim2.new(0,14,0,8); L1.Size=UDim2.new(1,-92,0,18)
        L1.BackgroundTransparency=1; L1.Text=label
        L1.TextColor3=GL_TEXT; L1.Font=Enum.Font.GothamSemibold; L1.TextSize=13
        L1.TextXAlignment=Enum.TextXAlignment.Left; L1.ZIndex=14

        local L2=Instance.new("TextLabel",Row)
        L2.Position=UDim2.new(0,14,0,28); L2.Size=UDim2.new(1,-92,0,16)
        L2.BackgroundTransparency=1; L2.Text=hint
        L2.TextColor3=GL_SUB; L2.Font=Enum.Font.Gotham; L2.TextSize=10
        L2.TextXAlignment=Enum.TextXAlignment.Left; L2.ZIndex=14

        local Btn=Instance.new("TextButton",Row)
        Btn.Size=UDim2.new(0,70,0,32); Btn.Position=UDim2.new(1,-82,0.5,-16)
        Btn.BackgroundColor3=col or GL_ACCENT; Btn.BackgroundTransparency=0.12
        Btn.BorderSizePixel=0; Btn.Text=btnLabel or "USE"
        Btn.TextColor3=Color3.new(1,1,1); Btn.Font=Enum.Font.GothamBold; Btn.TextSize=11; Btn.ZIndex=14
        Instance.new("UICorner",Btn).CornerRadius=UDim.new(0,10)
        Btn.MouseButton1Click:Connect(function()
            TweenService:Create(Btn,TweenInfo.new(0.07),{Size=UDim2.new(0,62,0,28)}):Play()
            task.delay(0.08,function() TweenService:Create(Btn,TweenInfo.new(0.12),{Size=UDim2.new(0,70,0,32)}):Play() end)
            if cb then cb() end
        end)
    end

    -- Segmented control
    local function SEG(parent,label,options,key,order,cb)
        local Row=MakeGlassRow(parent,66)
        Row.LayoutOrder=order; Row.ZIndex=12

        local L1=Instance.new("TextLabel",Row)
        L1.Position=UDim2.new(0,14,0,8); L1.Size=UDim2.new(1,-22,0,16)
        L1.BackgroundTransparency=1; L1.Text=label
        L1.TextColor3=GL_TEXT; L1.Font=Enum.Font.GothamSemibold; L1.TextSize=13
        L1.TextXAlignment=Enum.TextXAlignment.Left; L1.ZIndex=14

        local BtnHolder=Instance.new("Frame",Row)
        BtnHolder.Position=UDim2.new(0,12,0,30); BtnHolder.Size=UDim2.new(1,-24,0,26)
        BtnHolder.BackgroundColor3=Color3.new(0,0,0); BtnHolder.BackgroundTransparency=0.72; BtnHolder.ZIndex=14
        Instance.new("UICorner",BtnHolder).CornerRadius=UDim.new(0,9)

        local BtnRow=Instance.new("Frame",BtnHolder)
        BtnRow.Position=UDim2.new(0,2,0,2); BtnRow.Size=UDim2.new(1,-4,1,-4)
        BtnRow.BackgroundTransparency=1; BtnRow.ZIndex=15
        local BLL=Instance.new("UIListLayout",BtnRow)
        BLL.FillDirection=Enum.FillDirection.Horizontal; BLL.Padding=UDim.new(0,2)

        local btns={}
        for i,opt in ipairs(options) do
            local b=Instance.new("TextButton",BtnRow)
            local w=math.floor((WIN_W-50-2*(#options-1))/#options)
            b.Size=UDim2.new(0,w,1,0); b.BorderSizePixel=0
            b.BackgroundColor3=(Cfg[key]==i) and GL_ACCENT or Color3.new(1,1,1)
            b.BackgroundTransparency=(Cfg[key]==i) and 0.1 or 0.94
            b.Text=opt; b.TextColor3=(Cfg[key]==i) and Color3.new(1,1,1) or GL_SUB
            b.Font=Enum.Font.GothamBold; b.TextSize=9; b.ZIndex=16
            Instance.new("UICorner",b).CornerRadius=UDim.new(0,7)
            btns[i]=b
            b.MouseButton1Click:Connect(function()
                Cfg[key]=i
                for j,btn in ipairs(btns) do
                    TweenService:Create(btn,TweenInfo.new(0.15),{
                        BackgroundColor3=(j==i) and GL_ACCENT or Color3.new(1,1,1),
                        BackgroundTransparency=(j==i) and 0.1 or 0.94,
                        TextColor3=(j==i) and Color3.new(1,1,1) or GL_SUB,
                    }):Play()
                end
                if cb then cb(i) end
            end)
        end
    end

    -- KeyBinder
    local function KeyBinder(parent,labelText,hintText,cfgKey,order)
        local Row=MakeGlassRow(parent,52); Row.LayoutOrder=order; Row.ZIndex=12
        local L1=Instance.new("TextLabel",Row)
        L1.Position=UDim2.new(0,14,0,8); L1.Size=UDim2.new(1,-92,0,18)
        L1.BackgroundTransparency=1; L1.Text=labelText
        L1.TextColor3=GL_TEXT; L1.Font=Enum.Font.GothamSemibold; L1.TextSize=13
        L1.TextXAlignment=Enum.TextXAlignment.Left; L1.ZIndex=14
        local L2=Instance.new("TextLabel",Row)
        L2.Position=UDim2.new(0,14,0,28); L2.Size=UDim2.new(1,-92,0,16)
        L2.BackgroundTransparency=1; L2.Text=hintText
        L2.TextColor3=GL_SUB; L2.Font=Enum.Font.Gotham; L2.TextSize=10
        L2.TextXAlignment=Enum.TextXAlignment.Left; L2.ZIndex=14
        local Btn=Instance.new("TextButton",Row)
        Btn.Size=UDim2.new(0,70,0,32); Btn.Position=UDim2.new(1,-82,0.5,-16)
        Btn.BackgroundColor3=GL_SURF; Btn.BackgroundTransparency=0.1; Btn.BorderSizePixel=0
        Btn.Text=tostring(Cfg[cfgKey].Name)
        Btn.TextColor3=GL_ACCENT2; Btn.Font=Enum.Font.GothamBold; Btn.TextSize=11; Btn.ZIndex=14
        Instance.new("UICorner",Btn).CornerRadius=UDim.new(0,10)
        local listening=false
        Btn.MouseButton1Click:Connect(function()
            if listening then return end
            listening=true; Btn.Text="..."; Btn.TextColor3=Color3.fromRGB(255,200,80)
            local conn1
            conn1=UIS.InputBegan:Connect(function(input,gpe)
                if gpe then return end
                if input.UserInputType==Enum.UserInputType.Keyboard then
                    Cfg[cfgKey]=input.KeyCode
                    Btn.Text=tostring(input.KeyCode.Name); Btn.TextColor3=GL_ACCENT2
                    listening=false; conn1:Disconnect()
                end
            end)
        end)
        return Btn
    end

    -- Info block (тёмный, с цветной обводкой)
    local function INFO(parent,text,order)
        local F=Instance.new("Frame",parent)
        F.LayoutOrder=order; F.Size=UDim2.new(1,0,0,58)
        F.BackgroundColor3=Color3.fromRGB(10,18,30); F.BackgroundTransparency=0.08; F.BorderSizePixel=0
        Instance.new("UICorner",F).CornerRadius=UDim.new(0,10)
        local fs=Instance.new("UIStroke",F); fs.Color=GL_ACCENT; fs.Thickness=1; fs.Transparency=0.65
        local FT=Instance.new("TextLabel",F)
        FT.Position=UDim2.new(0,12,0,6); FT.Size=UDim2.new(1,-16,1,-10)
        FT.BackgroundTransparency=1; FT.TextColor3=GL_SUB
        FT.Font=Enum.Font.Gotham; FT.TextSize=11; FT.TextWrapped=true
        FT.TextXAlignment=Enum.TextXAlignment.Left; FT.TextYAlignment=Enum.TextYAlignment.Top
        FT.Text=text
    end

    -- ══════════════════════════════════════════
    -- ТАБЫ
    -- ══════════════════════════════════════════
    local PM  = NewTab("move",  "MOV", 1)
    local PM2 = NewTab("movev2","MV2", 2)
    local PV  = NewTab("vis",   "VIS", 3)
    local PX  = NewTab("tricks","TRK", 4)
    local PF  = NewTab("fx",    "FX",  5)
    local PC  = NewTab("misc",  "MISC",6)
    local PE  = NewTab("esp",   "ESP", 7)
    local PJR = NewTab("rings", "RING",8)

    -- ══════════════════════════════════════════
    -- MOVE TAB
    -- ══════════════════════════════════════════
    S(PM,"SPEED",1)
    SL(PM,"Walk Speed","Скорость ходьбы","WalkSpeed",16,350,1,2,function(v) if Hum then Hum.WalkSpeed=v end end)
    SL(PM,"Jump Power","Сила прыжка","JumpPower",50,300,5,3,function(v) if Hum then Hum.JumpPower=v end end)
    SL(PM,"Gravity","Гравитация (стандарт 196)","Gravity",30,400,5,4,function(v) workspace.Gravity=v end)

    S(PM,"BUNNY HOP",10)
    INFO(PM,"BhopOn + AutoJump → держи ПРОБЕЛ → прыгаешь сам. Скорость ×Gain за прыжок до Max. Стрейф A/D в воздухе = доп. ускорение.",10)
    T(PM,"Bunny Hop","Система bhop","BhopOn",11)
    T(PM,"Auto Jump","Авто-прыжок при приземлении","BhopAuto",12)
    SL(PM,"BHop Gain","×1.04 = +4% за прыжок","BhopGain",1.0,1.2,0.01,13)
    SL(PM,"BHop Max","Максимальная скорость","BhopMax",40,350,5,14)

    S(PM,"INF JUMP",20)
    T(PM,"Inf Jump","Прыгать повторно без касания земли","InfJumpOn",21)
    SL(PM,"Inf Jump Cooldown","Секунд между прыжками","InfJumpCooldown",0.5,15,0.5,22)

    S(PM,"DOUBLE JUMP",30)
    T(PM,"Double Jump","Второй прыжок в воздухе","DoubleJumpOn",31)

    S(PM,"AIR STRAFE",40)
    T(PM,"Air Strafe","A/D в воздухе","StrafeOn",41)
    SL(PM,"Strafe Accel","Ускорение в воздухе","StrafeAccel",10,200,5,42)
    SL(PM,"Strafe Max","Максимум в воздухе","StrafeMax",40,300,5,43)

    S(PM,"A/D STRAFE (CS2-style)",50)
    T(PM,"A/D Strafe","Идёт строго вбок, как в CS2","ADStrafeOn",51)
    SL(PM,"AD Strafe Speed","Скорость бокового шага","ADStrafeSpeed",5,40,1,52)

    S(PM,"DRUNK MOVEMENT",60)
    T(PM,"Drunk Movement","Скользкое инерционное движение","DrunkOn",61)
    SL(PM,"Drunk Accel","Ускорение (меньше = инертнее)","DrunkAccel",1,30,1,62)
    SL(PM,"Drunk Max Speed","Максимальная скорость","DrunkMaxSpeed",4,30,1,63)
    SL(PM,"Drunk Slide","0.99=долго скользит  0.8=быстро тормозит","DrunkSlide",0.80,0.995,0.005,64)

    S(PM,"FLY",70)
    T(PM,"Fly Mode","WASD + Пробел/Ctrl","FlyOn",71,function() flyVel=Vector3.zero end)
    SL(PM,"Fly Speed","Скорость полёта","FlySpeed",10,200,5,72)

    -- ══════════════════════════════════════════
    -- MOVE V2 TAB
    -- ══════════════════════════════════════════
    S(PM2,"МЕТОД ДВИЖЕНИЯ",1)
    INFO(PM2,"WalkSpeed — стандарт Humanoid. LinearVelocity / BodyVelocity — через физику в обход контроллера. Bhop/Strafe работают только с WalkSpeed.",1)

    local methodNames={"WalkSpeed","LinearVelocity","BodyVelocity"}
    local methodRow=Instance.new("Frame",PM2)
    methodRow.LayoutOrder=2; methodRow.Size=UDim2.new(1,0,0,38); methodRow.BackgroundTransparency=1
    local methodLL=Instance.new("UIListLayout",methodRow); methodLL.FillDirection=Enum.FillDirection.Horizontal; methodLL.Padding=UDim.new(0,5)
    local methodBtns={}
    for i,name in ipairs(methodNames) do
        local mb=Instance.new("TextButton",methodRow)
        mb.Size=UDim2.new(0,math.floor((WIN_W-56)/3),1,0); mb.BorderSizePixel=0
        mb.BackgroundColor3=GL_SURF
        mb.BackgroundTransparency=(Cfg.MoveMethod==name) and 0.05 or 0.45
        mb.Text=name; mb.TextColor3=(Cfg.MoveMethod==name) and GL_TEXT or GL_SUB
        mb.Font=Enum.Font.GothamBold; mb.TextSize=10
        Instance.new("UICorner",mb).CornerRadius=UDim.new(0,9)
        methodBtns[i]=mb
        mb.MouseButton1Click:Connect(function()
            Cfg.MoveMethod=name
            for j,b in ipairs(methodBtns) do
                b.BackgroundTransparency=(j==i) and 0.05 or 0.45
                b.TextColor3=(j==i) and GL_TEXT or GL_SUB
            end
            SetupMoveV2()
        end)
    end
    SL(PM2,"Move V2 Speed","Скорость для LV/BV методов","MoveV2Speed",4,80,1,3)

    S(PM2,"FPS & PING",10)
    T(PM2,"Show FPS / Ping","Счётчик в углу экрана","FpsPingOn",11,function() SetupFpsPing() end)

    -- ══════════════════════════════════════════
    -- VIS TAB
    -- ══════════════════════════════════════════
    S(PV,"CAMERA",1)
    SL(PV,"FOV","Поле обзора (стандарт 70)","FOV",50,120,1,2,function(v) if Cam then Cam.FieldOfView=v end end)

    -- Crosshair
    local CrHolder=Instance.new("Frame",SG)
    CrHolder.Name="Crosshair"; CrHolder.AnchorPoint=Vector2.new(0.5,0.5)
    CrHolder.Position=UDim2.new(0.5,0,0.5,0); CrHolder.Size=UDim2.new(0,100,0,100)
    CrHolder.BackgroundTransparency=1; CrHolder.ZIndex=20; CrHolder.Visible=false
    local function MakeLine()
        local f=Instance.new("Frame",CrHolder); f.AnchorPoint=Vector2.new(0.5,0.5)
        f.BackgroundColor3=Color3.new(1,1,1); f.BorderSizePixel=0; f.ZIndex=20
        Instance.new("UICorner",f).CornerRadius=UDim.new(0,2); return f
    end
    local CrLeft=MakeLine(); local CrRight=MakeLine(); local CrUp=MakeLine(); local CrDown=MakeLine(); local CrDot=MakeLine()
    local function UpdateCr()
        CrHolder.Visible=Cfg.CrossOn
        local sz=Cfg.CrossSize; local gap=Cfg.CrossGap; local th=Cfg.CrossThick
        local col=Color3.fromRGB(Cfg.CrossR,Cfg.CrossG,Cfg.CrossB)
        local cx,cy=50,50
        CrLeft.BackgroundColor3=col; CrLeft.Size=UDim2.new(0,sz,0,th); CrLeft.Position=UDim2.new(0,cx-gap-sz,0,cy-th/2)
        CrRight.BackgroundColor3=col; CrRight.Size=UDim2.new(0,sz,0,th); CrRight.Position=UDim2.new(0,cx+gap,0,cy-th/2)
        CrUp.BackgroundColor3=col; CrUp.Size=UDim2.new(0,th,0,sz); CrUp.Position=UDim2.new(0,cx-th/2,0,cy-gap-sz)
        CrDown.BackgroundColor3=col; CrDown.Size=UDim2.new(0,th,0,sz); CrDown.Position=UDim2.new(0,cx-th/2,0,cy+gap)
        CrDot.Visible=Cfg.CrossOn and Cfg.CrossDot
        CrDot.BackgroundColor3=col; CrDot.Size=UDim2.new(0,th+1,0,th+1); CrDot.Position=UDim2.new(0,cx-(th+1)/2,0,cy-(th+1)/2)
    end
    S(PV,"CROSSHAIR",10)
    T(PV,"Crosshair","Прицел в центре","CrossOn",11,function() UpdateCr() end)
    T(PV,"Center Dot","Точка в центре","CrossDot",12,function() UpdateCr() end)
    SL(PV,"Size","Длина линий","CrossSize",2,40,1,13,function() UpdateCr() end)
    SL(PV,"Gap","Зазор","CrossGap",0,25,1,14,function() UpdateCr() end)
    SL(PV,"Thickness","Толщина","CrossThick",1,8,1,15,function() UpdateCr() end)
    SL(PV,"R","","CrossR",0,255,5,16,function() UpdateCr() end)
    SL(PV,"G","","CrossG",0,255,5,17,function() UpdateCr() end)
    SL(PV,"B","","CrossB",0,255,5,18,function() UpdateCr() end)

    S(PV,"TRAIL",25)
    T(PV,"Trail","След за персонажем","TrailOn",26,function(v) SetTrail(v) end)
    SL(PV,"Lifetime","Длина следа","TrailLife",0.1,3,0.1,27,function() if Cfg.TrailOn then SetTrail(true) end end)
    SL(PV,"Width","Ширина","TrailWidth",0.5,5,0.5,28,function() if Cfg.TrailOn then SetTrail(true) end end)
    S(PV,"TRAIL START COLOR",29)
    SL(PV,"R","","TrailR1",0,255,5,30,function() if Cfg.TrailOn then SetTrail(true) end end)
    SL(PV,"G","","TrailG1",0,255,5,31,function() if Cfg.TrailOn then SetTrail(true) end end)
    SL(PV,"B","","TrailB1",0,255,5,32,function() if Cfg.TrailOn then SetTrail(true) end end)
    S(PV,"TRAIL END COLOR",33)
    SL(PV,"R","","TrailR2",0,255,5,34,function() if Cfg.TrailOn then SetTrail(true) end end)
    SL(PV,"G","","TrailG2",0,255,5,35,function() if Cfg.TrailOn then SetTrail(true) end end)
    SL(PV,"B","","TrailB2",0,255,5,36,function() if Cfg.TrailOn then SetTrail(true) end end)

    S(PV,"ATMOSPHERE",40)
    T(PV,"Fog","Туман","FogOn",41,function() ApplyFog() end)
    SL(PV,"Fog Start","","FogStart",0,500,10,42,function() if Cfg.FogOn then ApplyFog() end end)
    SL(PV,"Fog End","","FogEnd",50,2000,25,43,function() if Cfg.FogOn then ApplyFog() end end)
    SL(PV,"Fog R","","FogR",0,255,5,44,function() if Cfg.FogOn then ApplyFog() end end)
    SL(PV,"Fog G","","FogG",0,255,5,45,function() if Cfg.FogOn then ApplyFog() end end)
    SL(PV,"Fog B","","FogB",0,255,5,46,function() if Cfg.FogOn then ApplyFog() end end)

    S(PV,"MOTION BLUR",50)
    T(PV,"Motion Blur","Размытие в движении","BlurOn",51,function() UpdateBlur() end)
    SL(PV,"Blur Size","Интенсивность (0–56)","BlurSize",0,56,1,52,function() if Cfg.BlurOn then UpdateBlur() end end)

    S(PV,"SKY PRESETS",60)
    local SkyRow=Instance.new("Frame",PV); SkyRow.LayoutOrder=61; SkyRow.Size=UDim2.new(1,0,0,38); SkyRow.BackgroundTransparency=1
    local SkyLL=Instance.new("UIListLayout",SkyRow); SkyLL.FillDirection=Enum.FillDirection.Horizontal; SkyLL.Padding=UDim.new(0,4)
    local skyBtns={}
    for i,p in ipairs(SkyPresets) do
        local sb=Instance.new("TextButton",SkyRow)
        sb.Size=UDim2.new(0,60,1,0); sb.BorderSizePixel=0
        sb.BackgroundColor3=GL_SURF; sb.BackgroundTransparency=0.3
        sb.Text=p.name:sub(1,4):upper(); sb.TextColor3=GL_SUB
        sb.Font=Enum.Font.GothamBold; sb.TextSize=10
        Instance.new("UICorner",sb).CornerRadius=UDim.new(0,9)
        skyBtns[i]=sb
        sb.MouseButton1Click:Connect(function()
            ApplySky(i)
            for j,b in ipairs(skyBtns) do
                b.BackgroundTransparency=(j==i) and 0.05 or 0.3
                b.TextColor3=(j==i) and GL_TEXT or GL_SUB
            end
        end)
    end

    S(PV,"HUD",70)
    local SpeedLbl=Instance.new("TextLabel",SG)
    SpeedLbl.Size=UDim2.new(0,220,0,28); SpeedLbl.Position=UDim2.new(0.5,-110,1,-74)
    SpeedLbl.BackgroundTransparency=1; SpeedLbl.Text=""
    SpeedLbl.TextColor3=Color3.new(1,1,1); SpeedLbl.Font=Enum.Font.GothamBold
    SpeedLbl.TextSize=17; SpeedLbl.TextStrokeTransparency=0.5; SpeedLbl.Visible=false; SpeedLbl.ZIndex=8
    T(PV,"Speed Meter","Скорость внизу экрана","SpeedOn",71,function(v) SpeedLbl.Visible=v end)
    RunService.Heartbeat:Connect(function()
        if Cfg.SpeedOn and HRP then
            local v=HRP.AssemblyLinearVelocity; local spd=Vector3.new(v.X,0,v.Z).Magnitude
            local streak=Cfg.BhopStreak
            if streak>1 then SpeedLbl.Text=string.format("%.0f u/s  🔥×%d",spd,streak)
            else SpeedLbl.Text=string.format("%.0f u/s",spd) end
        end
    end)

    -- FPS/Ping label
    fpsPingLabel=Instance.new("TextLabel",SG)
    fpsPingLabel.Name="FpsPing"; fpsPingLabel.AnchorPoint=Vector2.new(1,0)
    fpsPingLabel.Position=UDim2.new(1,-12,0,12); fpsPingLabel.Size=UDim2.new(0,175,0,22)
    fpsPingLabel.BackgroundColor3=Color3.fromRGB(10,10,14); fpsPingLabel.BackgroundTransparency=0.3
    fpsPingLabel.BorderSizePixel=0; fpsPingLabel.Text="FPS: --   PING: --"
    fpsPingLabel.TextColor3=GL_TEXT; fpsPingLabel.Font=Enum.Font.Code; fpsPingLabel.TextSize=12
    fpsPingLabel.ZIndex=30; fpsPingLabel.Visible=false
    Instance.new("UICorner",fpsPingLabel).CornerRadius=UDim.new(0,7)
    SetupFpsPing()

    -- ══════════════════════════════════════════
    -- TRICKS TAB
    -- ══════════════════════════════════════════
    S(PX,"SPIN 360",1)
    T(PX,"Spin 360","Вращение персонажа","SpinOn",2)
    SL(PX,"Spin Speed","Градусов в секунду","SpinSpeed",30,720,15,3)

    S(PX,"SMOOTH CAM MODE",10)
    INFO(PX,"Персонаж смотрит куда камера, как в шутере. Плавность — скорость поворота.",10)
    T(PX,"Smooth Cam","Шутер-режим","SmoothCamOn",11)
    SL(PX,"Cam Smoothness","0.05=плавно  1.0=мгновенно","SmoothCamLerp",0.05,1,0.05,12)

    S(PX,"TELEPORT SWAP",20)
    INFO(PX,"Запомни точку клавишей → телепортируйся на маркер другой клавишей.",20)
    KeyBinder(PX,"Swap Bind","Клавиша запомнить позицию","SwapBindKey",21)
    KeyBinder(PX,"Swap Use","Клавиша телепорта","SwapUseKey",22)
    BTN(PX,"Bind Position","Запомнить текущую позицию",23,"BIND",GL_ACCENT,function() BindSwapPos() end)
    BTN(PX,"Teleport to Swap","Прыгнуть на маркер",24,"GO",GL_ACCENT,function() UseSwap() end)
    BTN(PX,"Clear Marker","Удалить маркер",25,"CLR",GL_DANGER,function() DestroyMarker(); swapPos=nil end)
    S(PX,"MARKER COLOR",26)
    SL(PX,"R","","SwapMarkerR",0,255,5,27,function() if swapPos then CreateMarker(swapPos) end end)
    SL(PX,"G","","SwapMarkerG",0,255,5,28,function() if swapPos then CreateMarker(swapPos) end end)
    SL(PX,"B","","SwapMarkerB",0,255,5,29,function() if swapPos then CreateMarker(swapPos) end end)

    S(PX,"SPEED BOOST",30)
    T(PX,"Speed Boost","Рывок","BoostOn",31,function(v) SetBoost(v) end)
    KeyBinder(PX,"Boost Key","Клавиша рывка","BoostKey",32)
    SL(PX,"Boost Speed","","BoostSpeed",40,400,10,33)
    SL(PX,"Boost Duration","","BoostDur",0.1,1.5,0.05,34)

    S(PX,"OTHER",40)
    T(PX,"Noclip","Сквозь стены","NoclipOn",41,function(v) SetNoclip(v) end)
    T(PX,"Anti-Ragdoll","Не падать","AntiRagOn",42,function(v) SetAntiRag(v) end)

    S(PX,"TELEPORT TO CAMERA",50)
    KeyBinder(PX,"Teleport Key","Клавиша телепорта по взгляду","TeleportCamKey",51)
    BTN(PX,"Teleport to Camera","Смотришь → телепорт туда",52,"GO",GL_ACCENT,function() TeleportToCam() end)

    -- ══════════════════════════════════════════
    -- FX TAB
    -- ══════════════════════════════════════════
    S(PF,"WORLD FX",1)
    INFO(PF,"Светящиеся объекты летают вокруг персонажа. Выбери тип и включи.",1)
    T(PF,"World FX","Частицы вокруг персонажа","FXOn",2,function(v) if v then MakeFX() else ClearFX() end end)

    local FxTypeRow=Instance.new("Frame",PF)
    FxTypeRow.LayoutOrder=3; FxTypeRow.Size=UDim2.new(1,0,0,38); FxTypeRow.BackgroundTransparency=1
    local FxTypeLL=Instance.new("UIListLayout",FxTypeRow); FxTypeLL.FillDirection=Enum.FillDirection.Horizontal; FxTypeLL.Padding=UDim.new(0,5)
    local fxTypeBtns={}; local fxTypeNames={"● Шары","◎ Кольца","⎸ Палки"}
    for i=1,3 do
        local sb=Instance.new("TextButton",FxTypeRow)
        sb.Size=UDim2.new(0,math.floor((WIN_W-56)/3),1,0); sb.BorderSizePixel=0
        sb.BackgroundColor3=GL_SURF; sb.BackgroundTransparency=(Cfg.FXType==i) and 0.05 or 0.45
        sb.Text=fxTypeNames[i]; sb.TextColor3=(Cfg.FXType==i) and GL_TEXT or GL_SUB
        sb.Font=Enum.Font.GothamBold; sb.TextSize=10
        Instance.new("UICorner",sb).CornerRadius=UDim.new(0,9)
        fxTypeBtns[i]=sb
        sb.MouseButton1Click:Connect(function()
            Cfg.FXType=i
            for j,b in ipairs(fxTypeBtns) do b.BackgroundTransparency=(j==i) and 0.05 or 0.45; b.TextColor3=(j==i) and GL_TEXT or GL_SUB end
            if Cfg.FXOn then MakeFX() end
        end)
    end

    -- ══════════════════════════════════════════
    -- JUMP RINGS TAB (отдельная вкладка)
    -- ══════════════════════════════════════════
    S(PJR,"JUMP RINGS",1)
    INFO(PJR,"Красивые светящиеся кольца появляются на месте, откуда игрок прыгнул.",1)
    T(PJR,"Enable Jump Rings","Включить кольца при прыжке","JumpRingsOn",2,function(v)
        if v then startJumpRings() else stopJumpRings() end
    end)

    -- Self / All toggle
    local modeRow2=MakeGlassRow(PJR,52); modeRow2.LayoutOrder=3; modeRow2.ZIndex=12
    local ml1=Instance.new("TextLabel",modeRow2)
    ml1.Position=UDim2.new(0,14,0,8); ml1.Size=UDim2.new(1,-92,0,18)
    ml1.BackgroundTransparency=1; ml1.Text="Self / All"
    ml1.TextColor3=GL_TEXT; ml1.Font=Enum.Font.GothamSemibold; ml1.TextSize=13
    ml1.TextXAlignment=Enum.TextXAlignment.Left; ml1.ZIndex=14
    local ml2=Instance.new("TextLabel",modeRow2)
    ml2.Position=UDim2.new(0,14,0,28); ml2.Size=UDim2.new(1,-92,0,16)
    ml2.BackgroundTransparency=1; ml2.Text="Self = только ты, All = все игроки"
    ml2.TextColor3=GL_SUB; ml2.Font=Enum.Font.Gotham; ml2.TextSize=10
    ml2.TextXAlignment=Enum.TextXAlignment.Left; ml2.ZIndex=14
    local modeBtn2=Instance.new("TextButton",modeRow2)
    modeBtn2.Size=UDim2.new(0,70,0,32); modeBtn2.Position=UDim2.new(1,-82,0.5,-16)
    modeBtn2.BackgroundColor3=GL_SURF; modeBtn2.BackgroundTransparency=0.1; modeBtn2.BorderSizePixel=0
    modeBtn2.Text=Cfg.JumpRingsMode; modeBtn2.TextColor3=GL_ACCENT2
    modeBtn2.Font=Enum.Font.GothamBold; modeBtn2.TextSize=11; modeBtn2.ZIndex=14
    Instance.new("UICorner",modeBtn2).CornerRadius=UDim.new(0,10)
    modeBtn2.MouseButton1Click:Connect(function()
        Cfg.JumpRingsMode=(Cfg.JumpRingsMode=="Self") and "All" or "Self"
        modeBtn2.Text=Cfg.JumpRingsMode; restartJumpRings()
    end)

    T(PJR,"Outline","Контурная обводка кольца","JumpRingsOutline",4)
    SL(PJR,"Radius (studs)","Радиус кольца в студах","JumpRingsRadius",1,15,0.5,5)
    SL(PJR,"Thickness","Толщина обода","JumpRingsThickness",0.1,3,0.1,6)
    SL(PJR,"Transparency","Прозрачность (0=непрозрачный)","JumpRingsTransparency",0,1,0.05,7)
    SL(PJR,"Lifetime (sec)","Через сколько секунд исчезает","JumpRingsLifetime",0.5,15,0.5,8)
    S(PJR,"ЦВЕТ КОЛЕЦ",9)
    SL(PJR,"R","","JumpRingsColorR",0,255,5,10)
    SL(PJR,"G","","JumpRingsColorG",0,255,5,11)
    SL(PJR,"B","","JumpRingsColorB",0,255,5,12)

    -- ══════════════════════════════════════════
    -- MISC TAB
    -- ══════════════════════════════════════════
    T(PC,"Safe Mode","Сброс при закрытии","SafeMode",1)
    S(PC,"KEY HINTS",5)
    T(PC,"Show Key Hints","Подсказка при нажатии клавиш","KeyHintsOn",6)
    S(PC,"MENU KEY",7)
    KeyBinder(PC,"Menu Toggle","Клавиша скрыть/показать GUI","MenuKey",8)

    BTN(PC,"Reset to Defaults","Сбросить все настройки",10,"RESET",GL_DANGER,function() Reset() end)

    S(PC,"GUIDE",15)
    local guide={
        {"BhopAuto","Держи Пробел → перс прыгает сам при приземлении"},
        {"Bhop Streak","Каждый прыжок подряд +4% скорости до Max"},
        {"Air Strafe","A/D в воздухе нарастает скорость. Чередуй A→D→A"},
        {"Smooth Cam","Перс смотрит куда камера, как в шутере"},
        {"Double Jump","Второй прыжок в воздухе — Space в воздухе"},
        {"Inf Jump","Прыжок раз в N секунд даже без земли"},
        {"Swap Bind","Запомни точку → телепортируйся на маркер"},
        {"Boost Key","Мгновенный рывок по нажатию клавиши"},
        {"Teleport Key","Смотри куда хочешь → клавиша → туда"},
        {"Jump Rings","Вкладка RING — кольца на месте прыжка"},
        {"World FX","FX вкладка → выбери тип → включи"},
    }
    for i,g in ipairs(guide) do
        local C2=Instance.new("Frame",PC)
        C2.LayoutOrder=15+i; C2.Size=UDim2.new(1,0,0,52)
        C2.BackgroundColor3=GLASS_BG2; C2.BackgroundTransparency=0.3; C2.BorderSizePixel=0
        Instance.new("UICorner",C2).CornerRadius=UDim.new(0,10)
        local T1=Instance.new("TextLabel",C2)
        T1.Position=UDim2.new(0,12,0,7); T1.Size=UDim2.new(1,-18,0,18)
        T1.BackgroundTransparency=1; T1.Text=g[1]
        T1.TextColor3=GL_TEXT; T1.Font=Enum.Font.GothamBold; T1.TextSize=13
        T1.TextXAlignment=Enum.TextXAlignment.Left
        local T2=Instance.new("TextLabel",C2)
        T2.Position=UDim2.new(0,12,0,27); T2.Size=UDim2.new(1,-18,0,18)
        T2.BackgroundTransparency=1; T2.Text=g[2]
        T2.TextColor3=GL_SUB; T2.Font=Enum.Font.Gotham; T2.TextSize=11
        T2.TextXAlignment=Enum.TextXAlignment.Left; T2.TextWrapped=true
    end

    -- ══════════════════════════════════════════
    -- ESP / CFG TAB
    -- ══════════════════════════════════════════
    S(PE,"PLAYER ESP",1)
    T(PE,"Enable ESP","Подсветка игроков","EspOn",2)
    T(PE,"Use Team Color","Цвета команд","EspUseTeamColor",3)
    T(PE,"Track Roles","Роли (Murderer/Sheriff)","EspTrackRoles",4)
    S(PE,"ЦВЕТ ДРУГА",5)
    SL(PE,"R","","EspFriendR",0,255,5,6)
    SL(PE,"G","","EspFriendG",0,255,5,7)
    SL(PE,"B","","EspFriendB",0,255,5,8)
    S(PE,"ЦВЕТ ВРАГА",9)
    SL(PE,"R","","EspEnemyR",0,255,5,10)
    SL(PE,"G","","EspEnemyG",0,255,5,11)
    SL(PE,"B","","EspEnemyB",0,255,5,12)

    S(PE,"CONFIGURATION",20)
    BTN(PE,"Export Config","Скопировать код в буфер",21,"COPY",GL_ACCENT,function() _G.ExportCFG() end)

    -- Import box
    local ImportRow=MakeGlassRow(PE,98); ImportRow.LayoutOrder=22; ImportRow.ZIndex=12
    local ImpL=Instance.new("TextLabel",ImportRow)
    ImpL.Position=UDim2.new(0,14,0,8); ImpL.Size=UDim2.new(1,-28,0,18)
    ImpL.BackgroundTransparency=1; ImpL.Text="Import Config"
    ImpL.TextColor3=GL_TEXT; ImpL.Font=Enum.Font.GothamSemibold; ImpL.TextSize=13
    ImpL.TextXAlignment=Enum.TextXAlignment.Left; ImpL.ZIndex=14
    local ImportBox=Instance.new("TextBox",ImportRow)
    ImportBox.Position=UDim2.new(0,14,0,30); ImportBox.Size=UDim2.new(1,-28,0,34)
    ImportBox.BackgroundColor3=Color3.fromRGB(10,10,16); ImportBox.BorderSizePixel=0
    ImportBox.Text=""; ImportBox.PlaceholderText="Вставь сюда hex-код конфига..."
    ImportBox.TextColor3=GL_TEXT; ImportBox.PlaceholderColor3=GL_SUB
    ImportBox.Font=Enum.Font.Code; ImportBox.TextSize=12
    ImportBox.ClearTextOnFocus=false; ImportBox.TextXAlignment=Enum.TextXAlignment.Left; ImportBox.ClipsDescendants=true; ImportBox.ZIndex=14
    Instance.new("UICorner",ImportBox).CornerRadius=UDim.new(0,8)
    local ImpPad=Instance.new("UIPadding",ImportBox); ImpPad.PaddingLeft=UDim.new(0,8); ImpPad.PaddingRight=UDim.new(0,8)

    local ImpStatus=Instance.new("TextLabel",ImportRow)
    ImpStatus.Position=UDim2.new(0,14,0,70); ImpStatus.Size=UDim2.new(1,-100,0,20)
    ImpStatus.BackgroundTransparency=1; ImpStatus.Text=""
    ImpStatus.TextColor3=GL_SUB; ImpStatus.Font=Enum.Font.Gotham; ImpStatus.TextSize=11
    ImpStatus.TextXAlignment=Enum.TextXAlignment.Left; ImpStatus.ZIndex=14

    local ImpBtn=Instance.new("TextButton",ImportRow)
    ImpBtn.Size=UDim2.new(0,70,0,26); ImpBtn.Position=UDim2.new(1,-82,0,68)
    ImpBtn.BackgroundColor3=GL_ACCENT; ImpBtn.BackgroundTransparency=0.12; ImpBtn.BorderSizePixel=0
    ImpBtn.Text="LOAD"; ImpBtn.TextColor3=Color3.new(1,1,1)
    ImpBtn.Font=Enum.Font.GothamBold; ImpBtn.TextSize=11; ImpBtn.ZIndex=14
    Instance.new("UICorner",ImpBtn).CornerRadius=UDim.new(0,8)
    ImpBtn.MouseButton1Click:Connect(function()
        local code=ImportBox.Text
        if not code or code:gsub("%s","")=="" then
            ImpStatus.Text="Поле пустое!"; ImpStatus.TextColor3=Color3.fromRGB(255,100,100); return
        end
        local ok=_G.ImportCFG(code)
        if ok then
            ImpStatus.Text="Загружено ✓ Перестраиваю..."; ImpStatus.TextColor3=Color3.fromRGB(80,220,120)
            task.defer(function()
                if Hum then Hum.WalkSpeed=Cfg.WalkSpeed; Hum.JumpPower=Cfg.JumpPower end
                workspace.Gravity=Cfg.Gravity; if Cam then Cam.FieldOfView=Cfg.FOV end
                SetTrail(Cfg.TrailOn); ApplyFog(); UpdateBlur()
                if Cfg.FXOn then MakeFX() else ClearFX() end
                SetNoclip(Cfg.NoclipOn); SetAntiRag(Cfg.AntiRagOn); SetBoost(Cfg.BoostOn)
                restartJumpRings(); BuildGUI()
            end)
        else
            ImpStatus.Text="Ошибка! Проверь код."; ImpStatus.TextColor3=Color3.fromRGB(255,100,100)
        end
    end)

    -- ══════════════════════════════════════════
    -- KEY HINTS (всплывающие подсказки)
    -- ══════════════════════════════════════════
    local HintHolder=Instance.new("Frame",SG)
    HintHolder.Name="KeyHints"; HintHolder.AnchorPoint=Vector2.new(1,1)
    HintHolder.Position=UDim2.new(1,-16,1,-16); HintHolder.Size=UDim2.new(0,220,0,0)
    HintHolder.BackgroundTransparency=1; HintHolder.ZIndex=35
    local HintLL=Instance.new("UIListLayout",HintHolder)
    HintLL.FillDirection=Enum.FillDirection.Vertical; HintLL.VerticalAlignment=Enum.VerticalAlignment.Bottom
    HintLL.HorizontalAlignment=Enum.HorizontalAlignment.Right
    HintLL.SortOrder=Enum.SortOrder.LayoutOrder; HintLL.Padding=UDim.new(0,5)

    ShowKeyHint=function(actionName,keyCode)
        if not Cfg.KeyHintsOn then return end
        local pill=MakeGlassPanel(HintHolder,10,0.12)
        pill.Size=UDim2.new(1,0,0,34); pill.ZIndex=35
        local pStroke=pill:FindFirstChildOfClass("UIStroke")
        if pStroke then pStroke.Color=GL_ACCENT; pStroke.Transparency=0.4 end

        local keyBadge=Instance.new("Frame",pill)
        keyBadge.Position=UDim2.new(0,8,0.5,-12); keyBadge.Size=UDim2.new(0,38,0,24)
        keyBadge.BackgroundColor3=GL_SURF; keyBadge.BorderSizePixel=0; keyBadge.ZIndex=36
        Instance.new("UICorner",keyBadge).CornerRadius=UDim.new(0,6)
        local keyTxt=Instance.new("TextLabel",keyBadge)
        keyTxt.Size=UDim2.new(1,0,1,0); keyTxt.BackgroundTransparency=1
        keyTxt.Text=tostring(keyCode and keyCode.Name or "?")
        keyTxt.TextColor3=GL_ACCENT2; keyTxt.Font=Enum.Font.GothamBold; keyTxt.TextSize=10; keyTxt.ZIndex=37

        local actionLbl=Instance.new("TextLabel",pill)
        actionLbl.Position=UDim2.new(0,54,0,0); actionLbl.Size=UDim2.new(1,-62,1,0)
        actionLbl.BackgroundTransparency=1; actionLbl.Text=actionName
        actionLbl.TextColor3=GL_TEXT; actionLbl.Font=Enum.Font.GothamBold; actionLbl.TextSize=12
        actionLbl.TextXAlignment=Enum.TextXAlignment.Left; actionLbl.ZIndex=36

        task.delay(1.4,function()
            if not pill or not pill.Parent then return end
            local tw=TweenService:Create(pill,TweenInfo.new(0.25),{BackgroundTransparency=1})
            tw:Play(); TweenService:Create(actionLbl,TweenInfo.new(0.25),{TextTransparency=1}):Play()
            TweenService:Create(keyTxt,TweenInfo.new(0.25),{TextTransparency=1}):Play()
            tw.Completed:Connect(function() if pill and pill.Parent then pill:Destroy() end end)
        end)
    end

    -- ══════════════════════════════════════════
    -- DRAGGABLE + INERTIA + FLOATING IDLE
    -- ══════════════════════════════════════════
    local dragging = false
    local dragStart, startPos
    local velocity  = Vector2.new(0,0)
    local lastDragPos, lastDragTime = nil, nil
    local inertiaConn = nil
    local floatConn   = nil
    local floatTime   = 0
    local floatBasePos= Win.Position

    local function StopFloat()
        if floatConn then floatConn:Disconnect(); floatConn=nil end
    end
    local function StartFloat()
        StopFloat(); floatTime=0
        floatConn=RunService.Heartbeat:Connect(function(dt)
            floatTime=floatTime+dt
            local oy=math.sin(floatTime*1.05)*4
            local ox=math.sin(floatTime*0.65)*2
            Win.Position=UDim2.new(
                floatBasePos.X.Scale, floatBasePos.X.Offset+ox,
                floatBasePos.Y.Scale, floatBasePos.Y.Offset+oy
            )
        end)
    end
    local function StopInertia()
        if inertiaConn then inertiaConn:Disconnect(); inertiaConn=nil end
    end
    local function StartInertia()
        StopInertia(); StopFloat()
        inertiaConn=RunService.Heartbeat:Connect(function(dt)
            velocity=velocity*0.90
            Win.Position=UDim2.new(
                Win.Position.X.Scale, Win.Position.X.Offset+velocity.X*dt,
                Win.Position.Y.Scale, Win.Position.Y.Offset+velocity.Y*dt
            )
            if velocity.Magnitude<4 then
                StopInertia(); floatBasePos=Win.Position; StartFloat()
            end
        end)
    end

    Header.InputBegan:Connect(function(input)
        if input.UserInputType==Enum.UserInputType.MouseButton1 or input.UserInputType==Enum.UserInputType.Touch then
            dragging=true; StopFloat(); StopInertia()
            dragStart=input.Position; startPos=Win.Position
            lastDragPos=input.Position; lastDragTime=tick(); velocity=Vector2.new(0,0)
            TweenService:Create(Win,TweenInfo.new(0.10),{Size=UDim2.new(0,WIN_W+5,0,WIN_H+5)}):Play()
        end
    end)
    Header.InputEnded:Connect(function(input)
        if input.UserInputType==Enum.UserInputType.MouseButton1 or input.UserInputType==Enum.UserInputType.Touch then
            dragging=false
            TweenService:Create(Win,TweenInfo.new(0.15,Enum.EasingStyle.Quint),{Size=UDim2.new(0,WIN_W,0,WIN_H)}):Play()
            StartInertia()
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType==Enum.UserInputType.MouseMovement or input.UserInputType==Enum.UserInputType.Touch) then
            local delta=input.Position-dragStart
            Win.Position=UDim2.new(startPos.X.Scale,startPos.X.Offset+delta.X,startPos.Y.Scale,startPos.Y.Offset+delta.Y)
            local now=tick(); local dt2=math.max(now-(lastDragTime or now),0.001)
            local pd=input.Position-(lastDragPos or input.Position)
            velocity=Vector2.new(pd.X/dt2,pd.Y/dt2)
            lastDragPos=input.Position; lastDragTime=now
        end
    end)

    -- ══════════════════════════════════════════
    -- SHOW / HIDE (пружинная анимация)
    -- ══════════════════════════════════════════
    local visible=true
    SetMenuVisible=function(v)
        Cfg.MenuVisible=v; visible=v
        if v then
            Win.Visible=true; EnsureGlobalBlur(5)
            Win.Size=UDim2.new(0,WIN_W*0.85,0,WIN_H*0.85)
            TweenService:Create(Win,TweenInfo.new(0.32,Enum.EasingStyle.Back,Enum.EasingDirection.Out),{Size=UDim2.new(0,WIN_W,0,WIN_H)}):Play()
            StartFloat()
        else
            StopFloat(); StopInertia(); ClearGlobalBlur()
            local tw=TweenService:Create(Win,TweenInfo.new(0.18,Enum.EasingStyle.Quint),{Size=UDim2.new(0,WIN_W*0.8,0,WIN_H*0.8)})
            tw:Play(); tw.Completed:Connect(function() if not visible then Win.Visible=false end end)
        end
    end

    ToggleHit.MouseButton1Click:Connect(function() SetMenuVisible(not visible) end)

    XBtn.MouseButton1Click:Connect(function()
        if Cfg.SafeMode then Reset() end
        ClearGlobalBlur(); SG:Destroy()
    end)

    -- Активируем первый таб и запускаем floating
    SwitchTab("move")
    floatBasePos=Win.Position; StartFloat()

    -- ══════════════════════════════════════════
    -- ЗВЁЗДЫ / МЕТЕОРЫ (фоновый декор)
    -- ══════════════════════════════════════════
    local StarFrame=Instance.new("Frame",SG)
    StarFrame.Size=UDim2.new(1,0,1,0); StarFrame.BackgroundTransparency=1; StarFrame.ZIndex=0; StarFrame.Name="Stars"
    local function MakeStar()
        local f=Instance.new("Frame",StarFrame); f.AnchorPoint=Vector2.new(0.5,0.5)
        f.BackgroundColor3=Color3.new(1,1,1); f.BackgroundTransparency=math.random(3,8)/10
        f.BorderSizePixel=0; local sz=math.random(1,2); f.Size=UDim2.new(0,sz,0,sz)
        Instance.new("UICorner",f).CornerRadius=UDim.new(1,0); f.ZIndex=0; return f
    end
    local function DropStar(star,delay)
        task.delay(delay,function()
            local x=math.random(2,98)/100; local dur=math.random(55,130)/10
            star.Position=UDim2.new(x,0,-0.03,0)
            local drift=(math.random()-0.5)*0.05
            local tw=TweenService:Create(star,TweenInfo.new(dur,Enum.EasingStyle.Linear),{Position=UDim2.new(x+drift,0,1.04,0)})
            tw:Play(); tw.Completed:Connect(function() DropStar(star,math.random(0,20)/10) end)
        end)
    end
    for i=1,20 do
        local st=MakeStar(); st.Position=UDim2.new(math.random(0,100)/100,0,math.random(0,100)/100,0)
        DropStar(st,math.random(0,60)/10)
    end
    for i=1,3 do
        local m=Instance.new("Frame",StarFrame); m.AnchorPoint=Vector2.new(0.5,0.5)
        m.Size=UDim2.new(0,math.random(28,55),0,1); m.BackgroundColor3=Color3.new(1,1,1)
        m.BackgroundTransparency=0.52; m.BorderSizePixel=0; m.ZIndex=0
        Instance.new("UICorner",m).CornerRadius=UDim.new(1,0)
        local function DropMeteor(f,d)
            task.delay(d,function()
                local sx=math.random(5,80)/100; local dur=math.random(45,85)/10
                f.Position=UDim2.new(sx,0,-0.03,0)
                local tw=TweenService:Create(f,TweenInfo.new(dur,Enum.EasingStyle.Linear),{Position=UDim2.new(sx+0.15,0,1.04,0)})
                tw:Play(); tw.Completed:Connect(function() DropMeteor(f,math.random(8,25)) end)
            end)
        end
        DropMeteor(m,math.random(0,15))
    end

    print("[PhysicsLab] v8 LIQUID GLASS GUI построен ✓")
end

-- ════════════════════════════════════════════════════════════════════
-- ГЛОБАЛЬНЫЕ БИНДЫ
-- ════════════════════════════════════════════════════════════════════
local function SetupGlobalBinds()
    UIS.InputBegan:Connect(function(i,g)
        if g then return end
        if i.KeyCode==Cfg.MenuKey then
            SetMenuVisible(not Cfg.MenuVisible)
            if ShowKeyHint then ShowKeyHint("Menu",Cfg.MenuKey) end
        end
    end)
    UIS.InputBegan:Connect(function(i,g)
        if g then return end
        if i.KeyCode==Cfg.SwapBindKey then
            BindSwapPos(); if ShowKeyHint then ShowKeyHint("Swap: Bind",Cfg.SwapBindKey) end
        end
    end)
    UIS.InputBegan:Connect(function(i,g)
        if g then return end
        if i.KeyCode==Cfg.SwapUseKey then
            UseSwap(); if ShowKeyHint then ShowKeyHint("Swap: Teleport",Cfg.SwapUseKey) end
        end
    end)
    UIS.InputBegan:Connect(function(i,g)
        if g then return end
        if i.KeyCode==Cfg.TeleportCamKey then
            TeleportToCam(); if ShowKeyHint then ShowKeyHint("Teleport to Cam",Cfg.TeleportCamKey) end
        end
    end)
    -- Double Jump (edge trigger)
    UIS.InputBegan:Connect(function(i,g)
        if g then return end
        if i.KeyCode~=Enum.KeyCode.Space then return end
        if not Cfg.DoubleJumpOn then return end
        if not Hum or not HRP then return end
        local onGround=Hum.FloorMaterial~=Enum.Material.Air
        if not onGround and not hasDoubleJumped then
            hasDoubleJumped=true
            local v=HRP.AssemblyLinearVelocity
            HRP.AssemblyLinearVelocity=Vector3.new(v.X,0,v.Z)
            HRP:ApplyImpulse(Vector3.new(0,HRP.AssemblyMass*Cfg.JumpPower*1.8,0))
            if ShowKeyHint then ShowKeyHint("Double Jump",Enum.KeyCode.Space) end
        end
    end)
end

-- ════════════════════════════════════════════════════════════════════
-- ИНИЦИАЛИЗАЦИЯ
-- ════════════════════════════════════════════════════════════════════
local function Init()
    if not GetChar() then LP.CharacterAdded:Wait(); task.wait(0.5); GetChar() end
    StartLoop()
    BuildGUI()
    SetupGlobalBinds()
    SetupMoveV2()
    if Cfg.JumpRingsOn then startJumpRings() end

    LP.CharacterAdded:Connect(function()
        task.wait(0.5); GetChar()
        flyVel=Vector3.zero; wasGround=true; spinAngle=0
        smoothYaw=0; hasDoubleJumped=false
        StartLoop(); SetTrail(Cfg.TrailOn)
        if Cfg.NoclipOn  then SetNoclip(true)   end
        if Cfg.AntiRagOn then SetAntiRag(true)   end
        if Cfg.BoostOn   then SetBoost(true)     end
        if Cfg.BlurOn    then UpdateBlur()        end
        if Cfg.FXOn      then MakeFX()           end
        SetupMoveV2()
        if Cfg.JumpRingsOn then startJumpRings() end
    end)
end

local ok,err=pcall(Init)
if not ok then warn("[PhysicsLab] ОШИБКА: "..tostring(err)) end
