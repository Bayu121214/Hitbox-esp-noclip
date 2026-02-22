local g = gethui and gethui() or game:GetService("CoreGui")
for _,v in ipairs(g:GetChildren()) do if v.Name=="HBX" then v:Destroy() end end
local sg = Instance.new("ScreenGui")
sg.Name = "HBX"
sg.ResetOnSpawn = false
sg.DisplayOrder = 999
sg.IgnoreGuiInset = true
sg.Parent = g

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TweenService")
local LP = Players.LocalPlayer

local HB_SIZE = 30
local FLY_MIN,FLY_MAX,WS_MIN,WS_MAX = 10,200,1,200
local hbOn,espOn,ncOn,flyOn = false,false,false,false
local flySpd,walkSpd = 60,16
local origSz,hbBoxes,hls,tags = {},{},{},{}
local flyC,flyBV = nil,nil

local function cr(p,r)
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0,r or 6)
	c.Parent = p
end

local PW,PH = 240,310

local side = Instance.new("TextButton")
side.Size = UDim2.new(0,34,0,100)
side.Position = UDim2.new(0,4,0.5,-50)
side.BackgroundColor3 = Color3.fromRGB(22,10,55)
side.BorderSizePixel = 0
side.Text = "H\nI\nT\nB\nO\nX"
side.TextColor3 = Color3.fromRGB(185,145,255)
side.TextSize = 10
side.Font = Enum.Font.GothamBold
side.ZIndex = 20
side.Parent = sg
cr(side,6)

local panel = Instance.new("Frame")
panel.Size = UDim2.new(0,PW,0,PH)
panel.Position = UDim2.new(0,42,0.5,-(PH/2))
panel.BackgroundColor3 = Color3.fromRGB(10,8,18)
panel.BorderSizePixel = 0
panel.ZIndex = 10
panel.Parent = sg
cr(panel,10)

local tbar = Instance.new("Frame")
tbar.Size = UDim2.new(1,0,0,36)
tbar.BackgroundColor3 = Color3.fromRGB(18,10,48)
tbar.BorderSizePixel = 0
tbar.ZIndex = 11
tbar.Parent = panel
cr(tbar,10)

local patch = Instance.new("Frame")
patch.Size = UDim2.new(1,0,0,12)
patch.Position = UDim2.new(0,0,1,-12)
patch.BackgroundColor3 = Color3.fromRGB(18,10,48)
patch.BorderSizePixel = 0
patch.ZIndex = 11
patch.Parent = tbar

local tlbl = Instance.new("TextLabel")
tlbl.Size = UDim2.new(1,-70,1,0)
tlbl.Position = UDim2.new(0,8,0,0)
tlbl.BackgroundTransparency = 1
tlbl.Text = "HITBOX EXPANDER"
tlbl.TextColor3 = Color3.fromRGB(178,142,255)
tlbl.TextSize = 12
tlbl.Font = Enum.Font.GothamBold
tlbl.TextXAlignment = Enum.TextXAlignment.Left
tlbl.ZIndex = 12
tlbl.Parent = tbar

local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0,26,0,26)
minBtn.Position = UDim2.new(1,-56,0.5,-13)
minBtn.BackgroundColor3 = Color3.fromRGB(38,24,85)
minBtn.BorderSizePixel = 0
minBtn.Text = "-"
minBtn.TextColor3 = Color3.fromRGB(198,172,255)
minBtn.TextSize = 18
minBtn.Font = Enum.Font.GothamBold
minBtn.ZIndex = 13
minBtn.Parent = tbar
cr(minBtn,5)

local clsBtn = Instance.new("TextButton")
clsBtn.Size = UDim2.new(0,26,0,26)
clsBtn.Position = UDim2.new(1,-28,0.5,-13)
clsBtn.BackgroundColor3 = Color3.fromRGB(138,22,42)
clsBtn.BorderSizePixel = 0
clsBtn.Text = "X"
clsBtn.TextColor3 = Color3.fromRGB(255,182,182)
clsBtn.TextSize = 12
clsBtn.Font = Enum.Font.GothamBold
clsBtn.ZIndex = 13
clsBtn.Parent = tbar
cr(clsBtn,5)

local tabRow = Instance.new("Frame")
tabRow.Size = UDim2.new(1,-10,0,30)
tabRow.Position = UDim2.new(0,5,0,38)
tabRow.BackgroundColor3 = Color3.fromRGB(14,10,28)
tabRow.BorderSizePixel = 0
tabRow.ZIndex = 11
tabRow.Parent = panel
cr(tabRow,6)

local tM = Instance.new("TextButton")
tM.Size = UDim2.new(0.5,-3,1,-6)
tM.Position = UDim2.new(0,3,0,3)
tM.BackgroundColor3 = Color3.fromRGB(68,34,175)
tM.BorderSizePixel = 0
tM.Text = "MAIN"
tM.TextColor3 = Color3.fromRGB(255,255,255)
tM.TextSize = 11
tM.Font = Enum.Font.GothamBold
tM.ZIndex = 12
tM.Parent = tabRow
cr(tM,5)

local tC = Instance.new("TextButton")
tC.Size = UDim2.new(0.5,-3,1,-6)
tC.Position = UDim2.new(0.5,0,0,3)
tC.BackgroundColor3 = Color3.fromRGB(24,16,52)
tC.BorderSizePixel = 0
tC.Text = "CHARACTER"
tC.TextColor3 = Color3.fromRGB(138,108,198)
tC.TextSize = 11
tC.Font = Enum.Font.GothamBold
tC.ZIndex = 12
tC.Parent = tabRow
cr(tC,5)

local cM = Instance.new("Frame")
cM.Size = UDim2.new(1,0,0,PH-72)
cM.Position = UDim2.new(0,0,0,72)
cM.BackgroundTransparency = 1
cM.ZIndex = 11
cM.Visible = true
cM.Parent = panel

local cCh = Instance.new("Frame")
cCh.Size = UDim2.new(1,0,0,PH-72)
cCh.Position = UDim2.new(0,0,0,72)
cCh.BackgroundTransparency = 1
cCh.ZIndex = 11
cCh.Visible = false
cCh.Parent = panel

local function mkB(par,txt,y,col)
	local b = Instance.new("TextButton")
	b.Size = UDim2.new(1,-10,0,34)
	b.Position = UDim2.new(0,5,0,y)
	b.BackgroundColor3 = col or Color3.fromRGB(68,34,175)
	b.BorderSizePixel = 0
	b.Text = txt
	b.TextColor3 = Color3.fromRGB(255,255,255)
	b.TextSize = 12
	b.Font = Enum.Font.GothamBold
	b.ZIndex = 12
	b.Parent = par
	cr(b,6)
	return b
end

local function mkSep(par,y)
	local f = Instance.new("Frame")
	f.Size = UDim2.new(1,-10,0,1)
	f.Position = UDim2.new(0,5,0,y)
	f.BackgroundColor3 = Color3.fromRGB(34,22,68)
	f.BorderSizePixel = 0
	f.ZIndex = 12
	f.Parent = par
end

local function mkLbl(par,txt,y,col)
	local l = Instance.new("TextLabel")
	l.Size = UDim2.new(1,-10,0,16)
	l.Position = UDim2.new(0,5,0,y)
	l.BackgroundTransparency = 1
	l.Text = txt
	l.TextColor3 = col or Color3.fromRGB(152,122,212)
	l.TextSize = 11
	l.Font = Enum.Font.Gotham
	l.TextXAlignment = Enum.TextXAlignment.Left
	l.ZIndex = 12
	l.Parent = par
	return l
end

local function mkSlider(par,y,fillCol,knobCol)
	local tr = Instance.new("Frame")
	tr.Size = UDim2.new(1,-10,0,16)
	tr.Position = UDim2.new(0,5,0,y)
	tr.BackgroundColor3 = Color3.fromRGB(24,16,50)
	tr.BorderSizePixel = 0
	tr.ZIndex = 12
	tr.Parent = par
	cr(tr,8)
	local fi = Instance.new("Frame")
	fi.Size = UDim2.new(0,0,1,0)
	fi.BackgroundColor3 = fillCol or Color3.fromRGB(12,62,138)
	fi.BorderSizePixel = 0
	fi.ZIndex = 13
	fi.Parent = tr
	cr(fi,8)
	local kn = Instance.new("Frame")
	kn.Size = UDim2.new(0,28,0,28)
	kn.Position = UDim2.new(0,-14,0.5,-14)
	kn.BackgroundColor3 = knobCol or Color3.fromRGB(108,162,255)
	kn.BorderSizePixel = 0
	kn.ZIndex = 14
	kn.Parent = tr
	cr(kn,14)
	return tr,fi,kn
end

local function mkStat(par,y)
	local l = Instance.new("TextLabel")
	l.Size = UDim2.new(1,-10,0,20)
	l.Position = UDim2.new(0,5,0,y)
	l.BackgroundTransparency = 1
	l.Text = "All OFF"
	l.TextColor3 = Color3.fromRGB(78,58,118)
	l.TextSize = 10
	l.Font = Enum.Font.Gotham
	l.TextXAlignment = Enum.TextXAlignment.Center
	l.TextWrapped = true
	l.ZIndex = 12
	l.Parent = par
	return l
end

local bHB  = mkB(cM,"ENABLE HITBOX",4,Color3.fromRGB(68,34,175))
local bESP = mkB(cM,"ENABLE ESP + TAGS",43,Color3.fromRGB(12,62,138))
mkSep(cM,82)
local stM = mkStat(cM,86)

local bFly = mkB(cCh,"ENABLE FLY",4,Color3.fromRGB(12,62,138))
local bNC  = mkB(cCh,"ENABLE NOCLIP",43,Color3.fromRGB(15,90,50))
local spLbl = mkLbl(cCh,"Fly Speed: "..flySpd,82,Color3.fromRGB(152,122,212))
local slT,slF,slK = mkSlider(cCh,100,Color3.fromRGB(12,62,138),Color3.fromRGB(108,162,255))
local flyA = (flySpd-FLY_MIN)/(FLY_MAX-FLY_MIN)
slF.Size = UDim2.new(flyA,0,1,0)
slK.Position = UDim2.new(flyA,-14,0.5,-14)
local wsLbl = mkLbl(cCh,"Walk Speed: "..walkSpd,122,Color3.fromRGB(100,212,120))
local wsT,wsF,wsK = mkSlider(cCh,140,Color3.fromRGB(15,90,50),Color3.fromRGB(100,220,120))
local wsA = (walkSpd-WS_MIN)/(WS_MAX-WS_MIN)
wsF.Size = UDim2.new(wsA,0,1,0)
wsK.Position = UDim2.new(wsA,-14,0.5,-14)
mkSep(cCh,164)
local stC = mkStat(cCh,168)

local function setTab(t)
	if t=="MAIN" then
		cM.Visible=true cCh.Visible=false
		tM.BackgroundColor3=Color3.fromRGB(68,34,175) tM.TextColor3=Color3.fromRGB(255,255,255)
		tC.BackgroundColor3=Color3.fromRGB(24,16,52) tC.TextColor3=Color3.fromRGB(138,108,198)
	else
		cM.Visible=false cCh.Visible=true
		tC.BackgroundColor3=Color3.fromRGB(68,34,175) tC.TextColor3=Color3.fromRGB(255,255,255)
		tM.BackgroundColor3=Color3.fromRGB(24,16,52) tM.TextColor3=Color3.fromRGB(138,108,198)
	end
end
tM.MouseButton1Click:Connect(function() setTab("MAIN") end)
tC.MouseButton1Click:Connect(function() setTab("CHARACTER") end)

local pOpen,pMin = true,false
local TI = TweenInfo.new(0.15,Enum.EasingStyle.Quad,Enum.EasingDirection.Out)

local function openP()
	pOpen=true panel.Visible=true
	TS:Create(panel,TI,{Size=UDim2.new(0,PW,0,pMin and 36 or PH)}):Play()
	side.Text="H\nI\nT\nB\nO\nX" side.BackgroundColor3=Color3.fromRGB(22,10,55)
end
local function closeP()
	pOpen=false
	local tw=TS:Create(panel,TI,{Size=UDim2.new(0,0,0,PH)})
	tw.Completed:Connect(function() if not pOpen then panel.Visible=false end end)
	tw:Play()
	side.Text=">" side.BackgroundColor3=Color3.fromRGB(68,34,175)
end
local function setMin(v)
	pMin=v
	if v then
		cM.Visible=false cCh.Visible=false tabRow.Visible=false
		TS:Create(panel,TI,{Size=UDim2.new(0,PW,0,36)}):Play() minBtn.Text="+"
	else
		TS:Create(panel,TI,{Size=UDim2.new(0,PW,0,PH)}):Play()
		task.delay(0.15,function() tabRow.Visible=true setTab("MAIN") end)
		minBtn.Text="-"
	end
end
side.MouseButton1Click:Connect(function() if pOpen then closeP() else openP() end end)
clsBtn.MouseButton1Click:Connect(closeP)
minBtn.MouseButton1Click:Connect(function() setMin(not pMin) end)

local drag,dS,dO = false,nil,nil
tbar.InputBegan:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then
		if i.Target==minBtn or i.Target==clsBtn then return end
		drag=true dS=i.Position dO=panel.Position
	end
end)
tbar.InputEnded:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then drag=false end
end)
UIS.InputChanged:Connect(function(i)
	if drag and (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) then
		local d=i.Position-dS
		panel.Position=UDim2.new(dO.X.Scale,dO.X.Offset+d.X,dO.Y.Scale,dO.Y.Offset+d.Y)
		side.Position=UDim2.new(0,panel.AbsolutePosition.X-40,0,panel.AbsolutePosition.Y+PH/2-50)
	end
end)

local slDrag=false
slT.InputBegan:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then slDrag=true end
end)
UIS.InputEnded:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then slDrag=false end
end)
UIS.InputChanged:Connect(function(i)
	if slDrag and (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) then
		local a=math.clamp((i.Position.X-slT.AbsolutePosition.X)/slT.AbsoluteSize.X,0,1)
		flySpd=math.floor(FLY_MIN+a*(FLY_MAX-FLY_MIN))
		slF.Size=UDim2.new(a,0,1,0) slK.Position=UDim2.new(a,-14,0.5,-14)
		spLbl.Text="Fly Speed: "..flySpd
		if flyBV and flyBV.Parent then flyBV.MaxForce=Vector3.new(flySpd*1000,flySpd*1000,flySpd*1000) end
	end
end)

local wsDrag=false
wsT.InputBegan:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then wsDrag=true end
end)
UIS.InputEnded:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then wsDrag=false end
end)
UIS.InputChanged:Connect(function(i)
	if wsDrag and (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) then
		local a=math.clamp((i.Position.X-wsT.AbsolutePosition.X)/wsT.AbsoluteSize.X,0,1)
		walkSpd=math.floor(WS_MIN+a*(WS_MAX-WS_MIN))
		wsF.Size=UDim2.new(a,0,1,0) wsK.Position=UDim2.new(a,-14,0.5,-14)
		wsLbl.Text="Walk Speed: "..walkSpd
		local mc=LP.Character
		if mc then local h=mc:FindFirstChildOfClass("Humanoid") if h then h.WalkSpeed=walkSpd end end
	end
end)

local function updS()
	local m={}
	if hbOn then table.insert(m,"Hitbox") end
	if espOn then table.insert(m,"ESP") end
	stM.Text=#m>0 and ("ON: "..table.concat(m," | ")) or "All OFF"
	local c={}
	if flyOn then table.insert(c,"Fly "..flySpd) end
	if ncOn then table.insert(c,"Noclip") end
	table.insert(c,"WS:"..walkSpd)
	stC.Text=table.concat(c," | ")
end

local function rmBox(ch) if hbBoxes[ch] and hbBoxes[ch].Parent then hbBoxes[ch]:Destroy() end hbBoxes[ch]=nil end
local function mkBox(ch,root)
	if hbBoxes[ch] and hbBoxes[ch].Parent then hbBoxes[ch].Size=Vector3.new(HB_SIZE,HB_SIZE,HB_SIZE) hbBoxes[ch].CFrame=root.CFrame return end
	local b=Instance.new("Part") b.Anchored=true b.CanCollide=false b.CanTouch=false b.CanQuery=false b.Massless=true b.CastShadow=false
	b.BrickColor=BrickColor.new("Bright red") b.Material=Enum.Material.Neon b.Transparency=0.88
	b.Size=Vector3.new(HB_SIZE,HB_SIZE,HB_SIZE) b.CFrame=root.CFrame b.Parent=workspace.CurrentCamera hbBoxes[ch]=b
end

local function rmHL(ch) if hls[ch] and hls[ch].Parent then hls[ch]:Destroy() end hls[ch]=nil end
local function mkHL(ch)
	if hls[ch] and hls[ch].Parent then return end
	local h=Instance.new("Highlight") h.Adornee=ch h.FillColor=Color3.fromRGB(255,50,50)
	h.OutlineColor=Color3.fromRGB(255,255,255) h.FillTransparency=0.55 h.OutlineTransparency=0
	h.DepthMode=Enum.HighlightDepthMode.AlwaysOnTop h.Parent=ch hls[ch]=h
end

local function rmTag(ch) if tags[ch] and tags[ch].Parent then tags[ch]:Destroy() end tags[ch]=nil end
local function mkTag(ch,pl)
	if tags[ch] and tags[ch].Parent then return end
	local head=ch:FindFirstChild("Head") if not head then return end
	local bb=Instance.new("BillboardGui") bb.Adornee=head bb.AlwaysOnTop=true
	bb.Size=UDim2.new(0,100,0,38) bb.StudsOffset=Vector3.new(0,2.5,0) bb.ResetOnSpawn=false bb.Parent=ch
	local bg=Instance.new("Frame") bg.Size=UDim2.new(1,0,1,0) bg.BackgroundColor3=Color3.fromRGB(8,5,16)
	bg.BackgroundTransparency=0.3 bg.BorderSizePixel=0 bg.Parent=bb cr(bg,5)
	local nl=Instance.new("TextLabel") nl.Size=UDim2.new(1,-4,0.58,0) nl.Position=UDim2.new(0,2,0,2)
	nl.BackgroundTransparency=1 nl.Text=pl.DisplayName nl.TextColor3=Color3.fromRGB(255,70,70)
	nl.TextSize=8 nl.Font=Enum.Font.GothamBold nl.Parent=bg
	local hpBg=Instance.new("Frame") hpBg.Name="HPBar" hpBg.Size=UDim2.new(1,-4,0,4)
	hpBg.Position=UDim2.new(0,2,1,-7) hpBg.BackgroundColor3=Color3.fromRGB(35,6,6) hpBg.BorderSizePixel=0 hpBg.Parent=bg cr(hpBg,2)
	local fill=Instance.new("Frame") fill.Name="Fill" fill.Size=UDim2.new(1,0,1,0)
	fill.BackgroundColor3=Color3.fromRGB(80,220,80) fill.BorderSizePixel=0 fill.Parent=hpBg cr(fill,2) tags[ch]=bb
end
local function updTag(ch)
	local bb=tags[ch] if not bb or not bb.Parent then return end
	local hum=ch:FindFirstChildOfClass("Humanoid") if not hum then return end
	local pct=math.clamp(hum.Health/math.max(hum.MaxHealth,1),0,1)
	local bg=bb:FindFirstChildOfClass("Frame") if not bg then return end
	local hpBg=bg:FindFirstChild("HPBar") if not hpBg then return end
	local fill=hpBg:FindFirstChild("Fill") if not fill then return end
	fill.Size=UDim2.new(pct,0,1,0) fill.BackgroundColor3=Color3.fromRGB(math.floor((1-pct)*220),math.floor(pct*220),25)
end

local flyBG = nil
local function stopFly()
	flyOn=false
	if flyC then flyC:Disconnect() flyC=nil end
	if flyBV and flyBV.Parent then flyBV:Destroy() end flyBV=nil
	if flyBG and flyBG.Parent then flyBG:Destroy() end flyBG=nil
	local mc=LP.Character if mc then local h=mc:FindFirstChildOfClass("Humanoid") if h then h.PlatformStand=false end end
end
local function startFly()
	flyOn=true
	local mc=LP.Character if not mc then return end
	local root=mc:FindFirstChild("HumanoidRootPart") if not root then return end
	local hum=mc:FindFirstChildOfClass("Humanoid") if not hum then return end
	hum.PlatformStand=true
	if flyBV and flyBV.Parent then flyBV:Destroy() end
	if flyBG and flyBG.Parent then flyBG:Destroy() end
	local bv=Instance.new("BodyVelocity")
	bv.Velocity=Vector3.zero
	bv.MaxForce=Vector3.new(1e5,1e5,1e5)
	bv.P=1e4
	bv.Parent=root flyBV=bv
	local bg=Instance.new("BodyGyro")
	bg.MaxTorque=Vector3.new(1e5,1e5,1e5)
	bg.P=1e4
	bg.D=500
	bg.CFrame=root.CFrame
	bg.Parent=root flyBG=bg
	local cam=workspace.CurrentCamera
	flyC=RunService.Heartbeat:Connect(function()
		if not flyOn then return end
		local mc2=LP.Character if not mc2 then return end
		if not flyBV or not flyBV.Parent then return end
		local h2=mc2:FindFirstChildOfClass("Humanoid") if not h2 then return end
		local r2=mc2:FindFirstChild("HumanoidRootPart") if not r2 then return end
		local cf=cam.CFrame
		r2.CFrame=CFrame.new(r2.Position,r2.Position+cf.LookVector)
		if flyBG and flyBG.Parent then
			flyBG.CFrame=CFrame.new(r2.Position,r2.Position+cf.LookVector)
		end
		local move=h2.MoveDirection local dir=Vector3.zero
		if move.Magnitude>0 then
			local lm=cf:VectorToObjectSpace(move)
			dir=(cf.LookVector*-lm.Z+cf.RightVector*lm.X)
			if dir.Magnitude>0 then dir=dir.Unit end
		end
		flyBV.Velocity=dir*flySpd
		flyBV.MaxForce=Vector3.new(1e5,1e5,1e5)
	end)
end

RunService.Heartbeat:Connect(function()
	for _,pl in ipairs(Players:GetPlayers()) do
		if pl==LP then continue end
		local ch=pl.Character if not ch then continue end
		local root=ch:FindFirstChild("HumanoidRootPart") if not root then continue end
		if hbOn then
			if not origSz[root] then origSz[root]=root.Size end
			root.Size=Vector3.new(HB_SIZE,HB_SIZE,HB_SIZE) mkBox(ch,root)
		else
			if origSz[root] then root.Size=origSz[root] origSz[root]=nil end
			if hbBoxes[ch] then rmBox(ch) end
		end
		if espOn then mkHL(ch) mkTag(ch,pl) updTag(ch)
		else if hls[ch] then rmHL(ch) end if tags[ch] then rmTag(ch) end end
	end
	for c in pairs(hbBoxes) do if not c.Parent then rmBox(c) end end
	for c in pairs(hls) do if not c.Parent then rmHL(c) end end
	for c in pairs(tags) do if not c.Parent then rmTag(c) end end
	for r in pairs(origSz) do if not r.Parent then origSz[r]=nil end end
end)

RunService.Stepped:Connect(function()
	if not ncOn then return end
	local mc=LP.Character if not mc then return end
	for _,p in ipairs(mc:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide=false end end
end)

bHB.MouseButton1Click:Connect(function()
	hbOn=not hbOn bHB.Text=hbOn and "DISABLE HITBOX" or "ENABLE HITBOX"
	bHB.BackgroundColor3=hbOn and Color3.fromRGB(152,26,48) or Color3.fromRGB(68,34,175)
	if not hbOn then for c in pairs(hbBoxes) do rmBox(c) end end updS()
end)
bESP.MouseButton1Click:Connect(function()
	espOn=not espOn bESP.Text=espOn and "DISABLE ESP + TAGS" or "ENABLE ESP + TAGS"
	bESP.BackgroundColor3=espOn and Color3.fromRGB(152,26,48) or Color3.fromRGB(12,62,138)
	if not espOn then for c in pairs(hls) do rmHL(c) end for c in pairs(tags) do rmTag(c) end end updS()
end)
bFly.MouseButton1Click:Connect(function()
	if flyOn then stopFly() bFly.Text="ENABLE FLY" bFly.BackgroundColor3=Color3.fromRGB(12,62,138)
	else startFly() bFly.Text="DISABLE FLY" bFly.BackgroundColor3=Color3.fromRGB(152,26,48) end updS()
end)
bNC.MouseButton1Click:Connect(function()
	ncOn=not ncOn bNC.Text=ncOn and "DISABLE NOCLIP" or "ENABLE NOCLIP"
	bNC.BackgroundColor3=ncOn and Color3.fromRGB(152,26,48) or Color3.fromRGB(15,90,50)
	if not ncOn then local mc=LP.Character if mc then for _,p in ipairs(mc:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide=true end end end end
	updS()
end)

LP.CharacterAdded:Connect(function()
	task.wait(1)
	if flyOn then startFly() end
	local mc=LP.Character
	if mc then local h=mc:FindFirstChildOfClass("Humanoid") if h then h.WalkSpeed=walkSpd end end
end)

updS()
