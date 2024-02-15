---
description: >-
  How to add additional overlays to peds, these are things like decals and
  tattoos
---

# 🖼 Addon Overlays

1. Re-size image to be used so that the dimensions are powers of 2 (e.g. 512x512, 256x1024, etc.) Note: This isn't a requirement but it will take advantage of mipmaps which improve image quality when applied to the characters.
2. Rename the image to the overlay name you'll use, e.g. example\_60.png Note: Anything proceeding the underscore must be the same as the tattoo collection name.
3. Export a tattoo from OpenIV (e.g. mp\_bus\_tat\_m\_006.ytd)
4. Rename tattoo filename to overlay name mp\_bus\_tat\_m\_006.ytd -> example\_60.ytd Note: Anything proceeding the underscore must be the same as the tattoo collection name.
5. Open the tattoo .ytd in OpenIV texture editor and use the rename option to match the image filename, e.g. example\_60
6. While still in texture editor, use the replace option and select the image file with the same name (e.g. example\_60.png).
7. You should have now have 2 textures in the tattoo, delete the older one and save the .ytd
8. Move the .ytd to a stream folder
9. Open/Create an .xml similar to example\_overalys.xml. This file dictates the tattoo gender, placement, and scale. Male tattoos will only work on males and vice versa. If you hear of someone saying the tattoo shows for them but not others, it is because they are using a tattoo opposite of their gender.

Follow previous examples located in example\_overlays.xml Note: Anything proceeding \_overlays must be the same as the tattoo collection name. The key components you need to edit:

```xml
<Item>
-->      <uvPos x="0.720000" y="0.670000" />
-->      <scale x="0.280000" y="0.350000" />
      <rotation value="0.000000" />
-->      <nameHash>example_05_M</nameHash>
-->      <txdHash>example_05</txdHash>
-->      <txtHash>example_05</txtHash>
-->      <zone>ZONE_RIGHT_ARM</zone>
-->      <type>TYPE_TATTOO</type>
      <faction>FM</faction>
      <garment>All</garment>
-->      <gender>GENDER_MALE</gender>
      <award />
      <awardLevel />
</Item>
```

Copy

If it is a female tattoo, it should be named example\_05\_F instead. If it is a badge or hairstyle, you need to change property to reflect that. Potential zones are: ZONE\_HEAD ZONE\_LEFT\_ARM ZONE\_LEFT\_LEG ZONE\_RIGHT\_ARM ZONE\_RIGHT\_LEG ZONE\_TORSO

1. Open/Create shoptattoos.meta file and add an entry there for the overlay. These don't require much work.

The key components you need to edit:

```xml
<Item>
        <id value="92610"/>
        <cost value="50"/>
-->      <textLabel>example_TAT_004</textLabel>
-->      <collection>example_overlays</collection>
-->      <preset>example_04_M</preset>
        <zone>PDZ_HEAD</zone>
        <eFacing>TATTOO_RIGHT</eFacing>
        <updateGroup>ARM_RIGHT_LOWER_SLEEVE</updateGroup>
        <eFaction>TATTOO_MP_FM</eFaction>
        <lockHash>CUSTOM_TATTOOS</lockHash>
</Item>
```

Copy

Collection needs to be the same name as your XML file. None of the other information matters as far as I've been able to tell in tattoos.meta.

I find it easier to just load the tattoos in the game and edit their position/scale while live. You can use icon or the console window to issue "restart new\_overlays" and the tattoo will update.
