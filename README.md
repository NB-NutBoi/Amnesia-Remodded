# Amnesia : Remodded
A modified game executable using BUzzer's x64 fork that adds a bunch of new functions and options for modders to play around with.

Made by NutBoi & Nate with the help of the Frictional games discord community.

Current version: 1.1

Also linux and mac are currently not supported to my knowledge, if you want to make a port for them, feel free to, the more people who can use this, the better.

## Key changes:
- Infinite lanterns.
- Multiple arms (not fully working as expected yet).
- A way to remove notes from the journal.
- AngelScript Update().
- Fixed timer loop crash. (by zenmumbler)
- Setting The rotation of entities.
- Attaching the camera to entities on command.
- Doubled shadow resolutions for spotlights.
- Some other miscellaneous functions:
	- Getting rotation of player camera/entities
	- Getting position of specific bones (rotation is broken, don't know fully why)
	- A lerp function for floats (don't know if it works as expected yet)

## Modding Documentation:

---------------------------------------

### Infinite lanterns:


There has to be a lantern.ho file for every lantern you need, with a very specific naming rule.<br />
Each lantern has to be lantern(number).ho<br />
Example: lantern31.ho

Now that you've set up the files you can dynamically switch between them with the function: 
```as
void SetLantern(int alLanternNum);
```
1. *alLanternNum* - The number the lantern should be switched to.

You can also get the lantern number it's on currently if you ever needed to, with the function:
```as
int GetLantern();
```
deafult lantern is 0 on startup, lantern saves when saving the game.

#### ----LANTERN.HOs-----

have a few new customisable options:

LanternOffSound: The sound that will play when the lantern is turned off. (default ui_lantern_off)<br />
LanternOnSound: The sound that will play when the lantern is turned on. (default ui_lantern_on)<br />
LanternNoOilSound: The sound that will play when the lantern is out of oil and is tried to turn on. (default ui_lantern_off)<br />

LanternOilDrainSpeed: The speed the oil will drain from that specific lantern (default 0.23)<br />

---------------------------------------

### Removing Notes:<br />
this is pretty straightforward, only 2 functions for now.<br />
(note: this does not work on diaries, that will have its own function in the future)<br />
(note 2: this is not reversible, if you remove a note, it's gone forever unless there's an exact copy somewhere else or you add it via AddNote();)<br />

to remove a specific note: 
```as
void RemoveNote(string &in entry); 
```
1. *entry* - The name of the entry the note has. this is the same as in the lang file.<br />

to remove all notes: 
```as
void RemoveAllNotes();
```
as simple as that, all notes will be gone.<br />

---------------------------------------

### Update:
A function that gets called every frame.
```as
void OnUpdate(float afTimeStep){}
```
1. *afTimeStep* - time in between calls (so to account for missed frames)

---------------------------------------

### Multiple arms:
#### (WARNING: DO NOT CHANGE ARM BEFORE LANTERN IS TURNED OFF, THIS BREAKS IT BECOUSE HPL.)<br />
#### (putting SetLanternActive right before you change it will also not work, best approach is to make sure the lantern is turned off and disbling the ability to turn it back on until after change, up to user how to do this.)

Default arm is 0 on startup, arm saves when saving the game.

Similar implementation to the lantern, have hands(number).ent and they will correspond to the integer you put in. this will not change even when i properly implement infinite arms.

Functions:
```as
void SetArm(int arm);
```
1. *arm* - the arm that should be equipped
```as
void GetArm();
```

---------------------------------------

### Getting rotation from entities/camera:

It will return the rotation from the axis the function describes.

Functions:

Camera:
```as
float GetCameraRotationX();
float GetCameraRotationY();
float GetCameraRotationZ();
```

Entity:
```as
GetEntityRotationX(string &in asName, int body);
GetEntityRotationY(string &in asName, int body);
GetEntityRotationZ(string &in asName, int body);
```
1. *asName* - Name of the entity to get the rotation from.<br />
2. *body* - The body of the entity to get the rotation from (if you're not sure, go with 0)

---------------------------------------

### Setting rotation for entities:

It will set the world rotation for the entity body selected.

```as
void SetEntityRotation(string &in asName, float afrX, float afrY, float afrZ, int body);
```
1. *asName* - name of the entity<br />
2. *afrX* - rotation in the X axis that should be set<br />
3. *afrY* - rotation in the Y axis that should be set<br />
4. *afrZ* - rotation in the Z axis that should be set<br />
5. *body* - body of the entity that the rotation should be applied to (if you're not sure, go with 0)<br />

#### You can also combine rotation and position on one function. Why? not sure.

(Untested)<br />
```as
void SetEntityRotationAndPosition(string &in asName, float afrX, float afrY, float afrZ, float afpX, float afpY, float afpZ, int body); 
```
1. *asName* - name of the entity<br />
2. *afrX* - rotation in the X axis that should be set<br />
3. *afrY* - rotation in the Y axis that should be set<br />
4. *afrZ* - rotation in the Z axis that should be set<br />
5. *afpX* - position in the X axis that should be set<br />
6. *afpY* - position in the Y axis that should be set<br />
7. *afpZ* - position in the Z axis that should be set<br />
8. *body* - body of the entity that the rotation should be applied to (if you're not sure, go with 0)

---------------------------------------

### Getting position of bones in a rigged entity:

You can get the specific location of bones with:
```as
float GetBonePosX(string &in asEntity, string &in asBone);
float GetBonePosY(string &in asEntity, string &in asBone);
float GetBonePosZ(string &in asEntity, string &in asBone);
```
1. *asEntity* - name of the entity (must have a skeleton)<br />
2. *asBone* - name of the bone in the entity (get with the ModelEditor)

#### You can also get the rotation<br />
But this is experimental and i'm not even sure if it works, i haven't had luck myself.

```as
float GetBoneRotX(string &in asEntity, string &in asBone);
float GetBoneRotY(string &in asEntity, string &in asBone);
float GetBoneRotZ(string &in asEntity, string &in asBone);
```
1. *asEntity* - name of the entity (must have a skeleton)<br />
2. *asBone* - name of the bone in the entity (get with the ModelEditor)

---------------------------------------

### Attaching the camera to an entity:

You can attach the camera to any entity. (BEWARE: THIS DOES NOT PREVENT THE PLAYER FROM MOVING, YOU NEED TO DO THIS ON YOUR OWN, ALSO, ONCE THE FUNCTION IS OVER, THE CAMERA WONT RETURN TO THE POSITION IT WAS IN BEFORE: IT WILL CARRY THE ROTATION OF THE ENTITY BACK TO THE PLAYER.)
```as
void AttachPlayerCameraToEntity(string &in asProp);
```
1. *asProp* - Entity the camera will be attached to.

```as
void DetachPlayerCamera();
```

---------------------------------------

### Some other miscellaneous functions:

Some other lesser functions added to remodded for various purposes.
```as
bool CheckEntityLineOfSight(string &in asEntity1, string &in asEntity1);
```
1. *asEntity1* - entity 1<br />
2. *asEntity2* - entity 2

```as
CheckIfEnemyDetectsPlayer(string &in asEnemy);
```
1. *asEnemy* - enemy checked.

This will linearly interpolate a and b by t (Alan, please test.)
```as
float fLerp(float a, float b, float t);
```
1. *a* - float.<br />
2. *b* - float.<br />
3. *t* - float.

---------------------------------------

## Compiling:
Should be as simple as BUzzer's x64 fork.
