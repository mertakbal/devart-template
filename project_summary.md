# Avian Flight Simulator

## Mert, Akbal, https://github.com/mertakbal



## Description
As an artist I consider dreams as a medium of art. I try to transfer my dreams into my artistic works in different media. These experiments build the foundation of my artistic studies.
I had many flight experiences in my dreams. To translate flight dreams into waking reality I invented the Avian Flight Simulator in 2011. In Avian Flight Simulator the player can experience a flight similar to those in dreams, using his body. As the player flaps his arms in different positions, the first person view of the simulator gains height and accelerates forward. As the player leans his torso forward and backward or turns his arms left and right, the view rotates. The position of player‘s head, torso and hands are detected by an infrared camera (Microsoft Kinect) and processed through middleware software (Delicode NI-Mate). The processed data are implemented to the 3D simulation built in the graphic software (Blender3D). The 3D scene, where the simulation takes places, is a 3D model of one of my recent flying dreams.

## Link to Prototype
https://vimeo.com/61423093



## Example Code
NOTE: Wrap your code blocks or any code citation by using ``` like the example below.
```
import bge
import math
def main():
	cont = bge.logic.getCurrentController()
	bullet = cont.owner
	keyboard =bge.logic.keyboard

	scene=bge.logic.getCurrentScene()
    

	touch=cont.sensors["Touch"]

	hit=touch.positive



	Torso = scene.objects['Torso']
	Kopf = scene.objects['Head']    

	RArm = scene.objects['Hand.R']
	LArm = scene.objects['Hand.L']
	Vogel = scene.objects['Kus']
	compass=scene.objects['compass']
	
	movSpeed=0.1
	checkHit=False
	zfarki=0
	Kopfz=Kopf.localPosition.z
	Kopfx=Kopf.localPosition.x
	Kopfy=Kopf.localPosition.y
	RArmz=RArm.localPosition.z
	RArmx=RArm.localPosition.x
	RArmy=RArm.localPosition.y
	LArmz=LArm.localPosition.z
	LArmx=LArm.localPosition.x
	LArmy=LArm.localPosition.y
	Torsoz=Torso.localPosition.z
	Torsox=Torso.localPosition.x
	Torsoy=Torso.localPosition.y
	#CONSTANTS

	VogelRot = Vogel.orientation.to_euler()
	VogelRot.y =0
	VogelRot.x =0
	compass.orientation = VogelRot
	
	
	zfarki = (RArmz - LArmz)/1000
	yfarki = (Torsoy-Kopfy)/100
	kollarinyfarki = (RArmy - LArmy)/1000
	kolfarki=Torsoy-((LArmy+RArmy)/2)+2 
	speed  = 150
    
	#ROTATE
	#Rotate Kus left & right
	Vogel.applyRotation((0,0,zfarki),True)
	Vogel.applyRotation((0,0,kollarinyfarki),True)
	#Rotate Kus up&down
	#Vogel.applyRotation((yfarki/7,0,0),True)
	
	#print(yfarki)
	
	
    # FLAP & FLY & FLOAT
	forceforward=0
	forceup=0
	Vogelz=Vogel.localPosition.z
	if not "lastVogelz" in bge.logic.globalDict:
		bge.logic.globalDict['lastVogelz'] = 0
	#Define Flap
	if hit:
		checkHit=True
		print (yfarki)
	#GROUNDED
	#Flap-Grounded 
	if Vogelz == bge.logic.globalDict['lastVogelz']:
		if checkHit==True:
			if Kopfz > RArmz :
				if Kopfz > LArmz :
					forceforward=speed
					forceup=speed
					Vogel.applyForce((0,0,forceup),True)
    
	#Dont Float while Grounded        
		if checkHit==False:
			Vogel.applyMovement((0,0,0),True)
    
	#Reset Rotation
		if checkHit==True:
			if Torsoz < RArmz :
				if Torsoz < LArmz:
					Vogel.orientation=compass.orientation

	
	
	#FLYING
	#Flap While Flying
	if Vogelz != bge.logic.globalDict['lastVogelz']:
		if checkHit==True:
			if Kopfz > RArmz :
				if Kopfz > LArmz :
					forceforward=130*kolfarki
					forceup=speed
					Vogel.applyForce((0,forceforward,forceup),True)
	#Float While Flying
	if Vogelz < bge.logic.globalDict['lastVogelz']:	
		if checkHit==False:
			
			forceforward=5*kolfarki
			print(kolfarki)   
			Vogel.applyForce((0,forceforward,0),True)
				
	#FALL down
		if checkHit==True:
			if Kopfz < RArmz :
				if Kopfz < LArmz:
					forceforward=230*kolfarki
					forceup=-230*kolfarki
					Vogel.applyForce((0,forceforward,forceup),True)
					Vogel.orientation=compass.orientation
					
	bge.logic.globalDict['lastVogelz'] = Vogelz
	

main()    
```
## Links to External Libraries
 NOTE: You can also use this space to link to external libraries or Github repositories you used on your project.

[Example Link](http://www.google.com "Example Link")

## Images & Videos
NOTE: For additional images you can either use a relative link to an image on this repo or an absolute link to an externally hosted image.

![Example Image](project_images/cover.jpg?raw=true "Example Image")

https://vimeo.com/61423093
