--// Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

--// Interface
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "AimbotUI"

local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 120, 0, 40)
toggleBtn.Position = UDim2.new(0, 20, 0, 20)
toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Text = "Aimbot: OFF"
toggleBtn.TextSize = 18

local info = Instance.new("TextLabel", gui)
info.Size = UDim2.new(0, 200, 0, 30)
info.Position = UDim2.new(0, 20, 0, 70)
info.BackgroundTransparency = 1
info.TextColor3 = Color3.new(1, 1, 1)
info.Text = "Alvo: Nenhum"
info.TextSize = 18

--// Configurações
local AimbotEnabled = false
local AimRadius = 1000
local TargetPart = "Head"

--// Função para encontrar o inimigo mais próximo
local function getClosestEnemy()
	local closest, shortest = nil, AimRadius

	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team then
			local char = player.Character
			local part = char and char:FindFirstChild(TargetPart)
			local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

			if part and root then
				local dist = (part.Position - Camera.CFrame.Position).Magnitude
				if dist < shortest then
					closest = part
					shortest = dist
				end
			end
		end
	end

	return closest
end

--// Atualizar botão
toggleBtn.MouseButton1Click:Connect(function()
	AimbotEnabled = not AimbotEnabled
	toggleBtn.Text = AimbotEnabled and "Aimbot: ON" or "Aimbot: OFF"
end)

--// Loop principal
RunService.RenderStepped:Connect(function()
	if AimbotEnabled then
		local target = getClosestEnemy()
		if target then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
			info.Text = "Alvo: " .. target.Parent.Name
		else
			info.Text = "Alvo: Nenhum"
		end
	else
		info.Text = "Aimbot desativad
