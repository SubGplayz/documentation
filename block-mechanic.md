---
description: How to add your own blocks to the game
---

# Block mechanic

## How does it work?

Unlike items, blocks cannot be added to the game using predicates in a model, in fact it is not really possible to add new blocks ...at least the game wasn't made for that. But there are still some hacked-up techniques to do it anyway. Oraxen allows you to do this in two different ways. Blockstates \(the json file which contains all the informations needed by Minecraft in order to render the block\) can be used to attribute block models according to the characteristics of particular blocks. The oldest method  relies on the blockfacing of the base block. Basically it uses the variations of a vanilla blocks which can have different blockfacing: the mushroom stem block. Only one variation is used in vanilla minecraft maps for this block, there are 6 faces on a block so we have 2^6 - 1 = 63 possible variations.

The most recent \(and recommended\) method is to use noteblocks and assign them different models depending on their instrument, their note and whether they are activated or not. This makes 800 possibilities \(but 25 are reserved so that the vanilla noteblocks still work\).

{% hint style="info" %}
The block and noteblock mechanics have the same configuration. To switch from one to the other you just have to rename the `block` section to `noteblock` and vice versa.
{% endhint %}

## Global configuration

This global configuration has to be used in order to define the hierarchy of between your multiple tool\_types. You can put the normal types + new types you just invented.

```yaml
noteblock:
  tool_types:
    - WOODEN
    - STONE
    - IRON
    - GOLDEN
    - DIAMOND
    - NETHERITE
  enabled: true
```

## How to create a simple block?

### Oraxen item and Pack configuration

The oraxen item root configuration is the same as for any item \(you can use any material like a diamond for example\) and set a displayname, etc. For the pack section you can use your own model or generate one. To generate a block model juste tell to oraxen that the parent model  is "block/cube\_all", then add your block texture \(one face\) inside your oraxen/pack/textures folder.

```yaml
my_block:
  displayname: "&My block"
  material: DIAMOND
  Pack:
    generate_model: true
    parent_model: "block/cube_all"
    textures:
      - my_block_texture.png
```

### Block Mechanic configuration

To use this mechanic you need to tell to oraxen which model to use \(to use the generated one, just put the name id of your item\). You then need to use custom\_variation which is not already used by another block \(since by default 1 is used by caveblock, you can for example use 2\). This example drop configurations allows you to get the drop when you mine it with a stone pickaxe.

```yaml
  Mechanics:
    block:
      custom_variation: 2
      model: my_block
      drop:
        silktouch: false 
        minimal_type: STONE
        loots:
          - {oraxen_item: caveblock, probability: 1.0}
```

### Customize the breaking speed

You can customize the breaking speed and the most suitable tools with the hardness subsection.

```yaml
  Mechanics:
    block:
      custom_variation: 2
      model: my_block
      drop:
        silktouch: false 
        minimal_type: STONE
        best_tools:
          - PICKAXE # and it's faster using a pickaxe
        loots:
          - {oraxen_item: caveblock, probability: 1.0}
      hardness: 20 # this makes it really hard to mine
```

### Produce Light

You can use the option **light** so that your block emits light.

```yaml
  Mechanics:
    block:
    custom_variation: 2
    model: my_block
    light: 5
    drop:
      silktouch: false 
      loots:
        - {oraxen_item: my_custom_item, probability: 1.0}
```

### Ores

This example configuration shows you how to create ores that support fortune and silktouch with a normal hardness.

```yaml
amethyst_ore:
  displayname: "&dAmethyst Ore"
  material: DIAMOND
  Pack:
    generate_model: true
    parent_model: "block/cube_all"
    textures:
      - amethyst_ore
  Mechanics:
    noteblock:
      break_sound: BLOCK_STONE_BREAK
      custom_variation: 1
      model: amethyst_ore
      hardness: 6
      drop:
        silktouch: true
        fortune: true
        minimal_type: IRON
        best_tools:
          - PICKAXE
        loots:
          - oraxen_item: amethyst
            probability: 1.0
```

