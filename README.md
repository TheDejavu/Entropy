# Entropy
1.20.1 Cannoning/Factions Jar based on [Pufferfish](https://github.com/pufferfish-gg/Pufferfish).

 - [FragmentMC Discord](http://fragmentmc.xyz)

## Features

### Patches
- TNT Flow
- TNT Spread
- Parity
- Waterlogged Blocks
- Lava
- Boats
- Concrete
- Bubble Columns
- Honey Blocks
- Non Full Blocks

### Optimizations
- Skip Ray Origin
- Merge Entities
- Skip Submerged
- Cache Explosion Damage
- Combined Explosions
- Static Ray Strength
- Cache Block Density
- Reduce Rays
- Cache Block States

### Chunks
- TNT Chunkloading
- Falling Block Chunkloading
- Ender Pearl Chunkloading

### Limits
- Cannon Range Limit
- Ender Pearl Range Limit

### Unbreakable Breaker
- Configurable Durability
- Configurable Check Item and Message

### Items
- Explosion Proof Items

### Factions
- Player Ignore Explosion Damage
- Spawner Upwards Velocity
- Spawner Override Health
- Golem Fall Damage
- Disable Death Animation

### MISC
- Disable Chat Message Verification

## Building

```bash
./gradlew build
```

Or building a Paperclip JAR for distribution:

```bash
./gradlew paperclip
```

## License
Patches are licensed under GPL-3.0.
All other files are licensed under MIT.
