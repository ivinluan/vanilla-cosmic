# Guidelines

This is a guideline for putting values in the block properties.

## Drills
### BASE VALUE
    health/120 * no. of tiles * version no. (if any) / 2
    armor/30 * no. of tiles * version no. (if any)
    liquid/0.2 * no. of tiles * version no. (if any) * 10 / 60
    liquidCapacity/itemCapacity * 0.4 (round it)
    liquidBoostIntensity/1.5 + liquid value
    itemCapacity/normal no. of items/sec * boost (if any) (make it whole number, round it)
    power/0.25 * no. of tiles * version no. (if any) / 60

### BASE VALUE for CRAFTING
    copper/30 * no. of tiles * version no. (if any)
    lead/30 * no. of tiles * version no. (if any)
    graphite/25 * no. of tiles * version no. (if any) / 2
    silicon/15 * no. of tiles * version no. (if any) / 2
    titanium/25 * no. of tiles * version no. (if any)
    plastanium/15 * no. of tiles * version no. (if any) / 2
    thorium/25 * no. of tiles * version no. (if any)
    phase-fabric/15 * no. of tiles * version no. (if any) / 2
    beryllium/25 * no. of tiles * version no. (if any)
    surge-alloy/5 * no. of tiles * version no. (if any) / 3
    tungsten/20 * no. of tiles * version no. (if any)
    carbide/10 * no. of tiles * version no. (if any) / 2

### NOT SHOWING EFFECTS
    lightning
    blockCrash
    trailFade
    unitSpawn
    unitControl
    unitDespawn
    unitSpirit
    itemTransfer
    pointBeam
    pointHit
    coreBuildShockwave
    coreBuildBlock
    upgradeCore
    payloadDeposit
    unitWreck
    unitAssemble
    unitLandSmall
    artilleryTrail
    incendTrail
    missileTrail
    fireRemove
    forceShrink
    colorTrail
    missileTrailShort
    sapped
    muddy
    wet
    dropItem
    regenSuppressSeek
    ripple
    healBlock
    healBlockFull
    rotateBlock
    lightBlock
    overdriveBlockFull
    chainLightning
    unitShieldBreak
    chainEmp
    arcShieldBreak
    debugLine
    debugRect
    legDestroy