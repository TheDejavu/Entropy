# Starforge
1.20.4 Cannoning/Factions Jar based on [Pufferfish](https://github.com/pufferfish-gg/Pufferfish).

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
- Cache Moving Block Shapes

### Packet Limiter
- TNT Flow
- TNT Move
- TNT Velocity
- TNT Data
- Falling Block Move
- Falling Block Velocity
- Falling Block Data
- Piston Updates

### Chunks
- TNT Chunkloading
- Falling Block Chunkloading
- Ender Pearl Chunkloading

### Limits
- Cannon Range Limit
- Ender Pearl Range Limit

### Unbreakable Breaker
- Highly Optimised
- Configurable Durability
- Configurable Check Item and Message

### Items
- Explosion Proof Items

### Factions
- Spawner Upwards Velocity
- Spawner Override Health
- Golem Fall Damage
- Disable Mob Death Animation

### Security
- IP Hashing (Hide Player IPs)
- Disable Chat Message Verification

### Player
- Player Ignore Explosion Damage
- Modify Knockback

### FPS Menu
- Configurable
- Hide Entities
- Hide Explosions
- No Flashing
- Hide Piston Movements

## Permissions
| Command  | Node                    |
|----------|-------------------------|
| /starforge | starforge.command.starforge |
| /fps     | starforge.command.fps     |

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

