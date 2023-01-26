くそコード<br>
```
AddCSLuaFile()

ENT.Base 			= "base_gmodentity"
ENT.Spawnable		= true
ENT.PrintName       = "GadaiBag Spawn Egg"

function ENT:Initialize()
    --ここだけは絶対に触らないでください。
    print(game.GetMap())
    if(game.GetMap()=="gm_construct") then
        self:Tamago()
    end
end

function ENT:Tamago()
	self:SetModel( "models/props_junk/watermelon01.mdl" )
  self.l1_w={0,0,0}
	self.a=Vector(0,0,0)
	self.gamma=0
	self.umaretaka=0
	self:KRun()
end

function ENT:KRun()
    if(math.random(0,150)==math.random(0,150) and self.umaretaka==0) then
        self.umaretaka=1
        timer.Simple(2,function() self:SetModel( "models/Kleiner.mdl" ) end)
        timer.Simple(3,function() self:NNRun() end)
	else
        if(self.umaretaka==0) then
            timer.Simple(0.1,function() self:KRun() end)
        end
    end
end


function ENT:NNRun()
	local _ents = ents.FindInSphere( self:GetPos(), 2000 )
	for k,v in ipairs( _ents ) do
		if ( v:IsPlayer() ) then
			self.a=v:GetPos()
			self.a_player=v
			self.gamma=self.gamma-math.random(-1,1)

			l_w1_loss=self.a.x-self.l1_w[1]
			l_w2_loss=self.a.y-self.l1_w[2]
			l_w3_loss=self.a.z-self.l1_w[3]
			if(self.a.x>l_w1_loss)then
				self.l1_w[1]=self.l1_w[1]-self.gamma
			else
				self.l1_w[1]=self.l1_w[1]+self.gamma
			end
			
			if(self.a.y>l_w2_loss)then
				self.l1_w[2]=self.l1_w[2]-self.gamma
			else
				self.l1_w[2]=self.l1_w[2]+self.gamma
			end
			
			if(self.a.z>l_w3_loss)then
				self.l1_w[3]=self.l1_w[3]-self.gamma
			else
				self.l1_w[3]=self.l1_w[3]+self.gamma
			end
			k=Vector(self.a.x*self.l1_w[1],self.a.y*self.l1_w[2],self.a.z*self.l1_w[3])
			if(k.x<=-5240) then k.x=-5240 end
			if(k.x>=1850) then k.x=1850 end
			if(k.y<=-3711) then k.y=-3711 end
			if(k.y>=6320) then k.y=6320 end
			
			self:SetPos(k)
			break

		end
	end
    if(math.random(0,50)==math.random(0,50)) then
        
        local _ents = ents.FindInSphere( self:GetPos(), 2142210 )
        for k,v in ipairs( _ents ) do
            if ( v:IsPlayer() and v:Alive()) then
                self:SetPos(v:GetPos())
                self:Tamago()
            end
        end	
    else
        if(self.umaretaka==1) then
    	    timer.Simple(0.1,function() self:NNRun() end)
        end
    end
end
```
