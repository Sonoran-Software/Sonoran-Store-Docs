---
description: Various topics regarding GTA V and CitizenFX development
---

# 5⃣ GTA V / CFX Development

## Tools of the Trade <a href="#tools-of-the-trade" id="tools-of-the-trade"></a>

All development starts with your basic tools. Along with having multiple different ways to acomplish a similar task there comes lots of different tools to work along that development path.

### 3D Modeling Tooling <a href="#h-3d-modeling-tooling" id="h-3d-modeling-tooling"></a>

To start you need a way to create geometric objects in a 3D space, this is usually done with a variety of tools, specifically for GTA V most community developers use the following...

* [Autodesk 3DS Max](https://www.autodesk.com/education/edu-software/overview?sorting=featured\&filters=individual)\
  Free Educational Licenses available otherwise it is paid (updates only for 1 year but able to use forever)
* [Blender](https://www.blender.org/download/)\
  Free and Open Source
* [ZModeler 3](https://www.zmodeler3.com/) for GTA V Models (primarily used for Vehicle development, other 3D asset creation possible)
* [Zmodeler 2](https://www.zmodeler2.com/index.php) for GTA IV models

Most developers in my experience (Max), use 3DS Max for the majority of 3D asset development in GTA V/GTA IV/CFX. Vehicle development is largely done in ZModeler 3 (for GTA V).

### GTA V/CitizenFX 3D Asset Tooling <a href="#gta-vcitizenfx-3d-asset-tooling" id="gta-vcitizenfx-3d-asset-tooling"></a>

#### GTA Asset Conversion/Viewing Applications <a href="#gta-asset-conversionviewing-applications" id="gta-asset-conversionviewing-applications"></a>

* [OpenIV](https://openiv.com/) Supports viewing most game asset files and editing meta files for RAGE (Rockstar Advanced Game Engine) based games. Used for converting in 3DS workloads and can be used for texture modding.
* [CodeWalker](https://github.com/dexyfex/CodeWalker) Similar to OpenIV but adds a lot of editing capabilities over OpenIV. Recent versions can edit YTYP and YMAP meta files which manage placement of 3D assets in GTA. This is the main tool for finalizing mapping projects and use in many points in development for import/export proceedures.

CodeWalker on GTA5 Mods is out of date, do not use it. Join the [CodeWalker Discord](https://discord.com/invite/BxfKHkk) for [up to date releases](https://discord.com/channels/329138547833700352/351357358460370944). The GitHub is considered to be updated with the up to date source code.

#### 3DS Max Plugins needed for CFX development <a href="#h-3ds-max-plugins-needed-for-cfx-development" id="h-3ds-max-plugins-needed-for-cfx-development"></a>

Generally Gims EVO is easy to find but most versions break on 3DS Max 2017 and later as the original is made for 3DS Max 2016. Some developers still have and use only 2016 as they feel it "works better". Que the unending debate of which is better. You should be fine to work on newer versions of 3DS Max\
[Gims EVO OG](https://www.gta5-mods.com/tools/gims-evo-with-gta-v-support) 3DS Max 2016 and prior?\
[Gims EVO Updated for 3DS Max 2017+](https://gtaforums.com/topic/929447-gta-v-gims-for-3dsmax-2017-2020/)

* [March 2021 - Unconfirmed if this update in the comments is better/working](https://gtaforums.com/topic/929447-gta-v-gims-for-3dsmax-2017-2020/?do=findComment\&comment=1071508240)
* [I think this is the same as March 2021 update above](https://drive.google.com/file/d/1vAxyqztYyvodMAbQGiZPU5JpNqwd1tml/view)
* Seems to use the same March 2021 Update linked above

#### Blender Plugins needed for CFX development <a href="#blender-plugins-needed-for-cfx-development" id="blender-plugins-needed-for-cfx-development"></a>

[Sollumz Blener Plugin](https://github.com/Skylumz/Sollumz)\
Sollumz is created by Skylumz and is used in conjunction with CodeWalker in order to import/export GTA V 3D assets

#### Additional Miscellaneous Tools <a href="#additional-miscellaneous-tools" id="additional-miscellaneous-tools"></a>

[Texture Toolkit](https://www.gta5-mods.com/tools/texture-toolkit) is a tool for GTAV that allows you to edit textures in:

* texture dictionary files (\*.ytd)
* drawable dictionary files (\*.ydd)
* drawable files (\*.ydr)
* fragment files (\*.yft)
* particle files (\*.ypt)

This can be used in place of OpenIV but I believe only supports the DDS image format while OpenIV support both PNG and DDS.

[Pleb Masters Forge](https://forge.plebmasters.de/) General game asset lookup and preview, this can be useful if looking for native game props to use in a mapping project or a base asset for a ped project.

### 3D Creation Assets <a href="#h-3d-creation-assets" id="h-3d-creation-assets"></a>

#### Free Texture Libraries <a href="#free-texture-libraries" id="free-texture-libraries"></a>

[https://quixel.com/megascans/home/](https://quixel.com/megascans/home/) (requires you to sign in with Epic Games and accepting UE license)\
[https://texturebox.com/](https://texturebox.com/)\
[https://3dtextures.me/](https://3dtextures.me/)\
[https://www.cgbookcase.com/textures](https://www.cgbookcase.com/textures)\
[https://texturehaven.com/textures/](https://texturehaven.com/textures/)

## Sub-Categories <a href="#sub-categories" id="sub-categories"></a>

#### [3D Asset/Prop Development](https://docs.sonoran.store/development/gta/props) <a href="#h-3d-assetprop-development" id="h-3d-assetprop-development"></a>

#### [Interior/Map Development (MLO,YMAP)](https://docs.sonoran.store/development/gta/mapping) <a href="#interiormap-development-mloymap" id="interiormap-development-mloymap"></a>

#### [Clothing/Ped Development (EUP)](https://docs.sonoran.store/development/gta/peds) <a href="#clothingped-development-eup" id="clothingped-development-eup"></a>

#### [Vehicle Development](https://docs.sonoran.store/development/gta/vehicles) <a href="#vehicle-development" id="vehicle-development"></a>

#### [Script Development](https://docs.sonoran.store/development/gta/scripts) <a href="#script-development" id="script-development"></a>
