---
description: Specifics on developing new or modified game world mapping
---

# 🏙 Mapping (MLO/YMAP)

## Types of GTA Mapping <a href="#types-of-gta-mapping" id="types-of-gta-mapping"></a>

Over time there have been varying levels of mapping development in GTA. Many low-level developers know how to use CodeWalker or some other map editor to create what is referred to as a YMAP. YMAPs are a type of GTA meta file that defines props that are loaded into the world in the exterior world (outside of an interior). Early "interiors" were created as YMAPs and generally were very poor in performance and looks as the building construction was limited to overlapping props to emulate actual building interiors and sometimes exteriors. This is at least how YMAPs can be used when loaded into the global world using absolute coordinate

As the GTA modding community reverse-engineered the game files further it became known that MLOs or MiLOs as some refer to them still. An excerpt from this [loading interiors guide](https://forums.gta5-mods.com/topic/19039/milo-starter-guide) explains what a MLO is...

> For starters, what is MILO? I don't even know for sure, as this game's innards appear to be notoriously undocumented (at least officially); but it's reasonable to assume it stands for Moveable Interior LOader. The idea behind a MILO is rather brilliant, really. Instead of hard-coding all your build's objects coordinates, you create a ymap file with just relative coordinates (relative to a, preferably nearby, virtual 'root' prim). That ymap is typically embedded in a CMloArchetypeDef container, inside a ytyp file.

These days MLO developers are popping up left and right and many are turning out to be pretty competent in their 3D modeling and design skills. It still takes a lot of skill and knowledge to put together 3D modeling, meta understanding and game optimization to make high-quality performant MLOs.

### MLOs <a href="#mlos" id="mlos"></a>

The most preferred way of editing the map is as Rockstar does, this will lead to keeping high performance in mind and gives you the most possibilities in your projects to make a truly amazing element in the GTA world. MLOs although mainly relating to interiors also typically are used to describe custom exteriors placed into the GTA world.

### YMAPs <a href="#ymaps" id="ymaps"></a>

As mentioned earlier, YMAPs were used and probably a better term is abused to make interiors and exterior elements using combinations of props before a lot of information was publicized about how to create MLOs as information and tools have been released YMAPs have been becoming less popular.

YMAPs are generally not a good idea, especially when placing a lot of props across the map. Unless modifying YMAPs that are native to the game, which comes with its own set of issues, addon YMAPS generally load props with no distance culling. Meaning that if you are across the map and an addon YMAP is loading props at the opposite end, your game is tracking those objects and causing performance to suffer as a result. Lots of YMAPs will lead to degraded performance.

## File Types <a href="#file-types" id="file-types"></a>

WIP

## Tutorials <a href="#tutorials" id="tutorials"></a>

WIP
