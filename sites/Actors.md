
# Actors and their usage
* [Back to main page](README.md)
* [Useful Objects list](#useful-objects-list)
* [Actors name filter](#actors-name-filter)
* Usage

## Useful Objects list
Class | Description
----- | ----------------
AAthenaPlayerCharacter | Player actor
AAthenaAICharacter | Skelet NPC enemy actor
AFauna | Animal actor
APet | Pet actor
ASunkenCurseArtefact | Sunken Mermaid Statue
AStrongholdKey | Stronghold Fort Key actor
AStrongholdKeyProxy | Stronghold Fort key pproxy
ABootyProxy | Base class for any kind of treasure chests/skulls etc.
ATreasureChest | Base class for chests
AMerchantCrate, AStaticMerchantCrate | Base class for merchant stuff (boxes, cages, gunpowder etc.)
ABountyReward | Skulls base actor
AStaticSimpleBootyWieldableItem, ASimpleBootyWieldableItem | base actor for some wieldable items
AStorageCrateItemProxy | base actor for the Storage Crate
AShip | Ship's actor
AShipNetProxy | Draws instead of AShip after about 1500 meters.
ARowboat | Rowboats actor
ASharkPawn | Shark actor
AStorm | Storm actor
AGameplayEventSignal | Fort and Ship raid cloud
ACannonProjectile | Cannon balls projectiles
AMermaid | Mermaid actor
AShipwreck | Shipwreck base actor
AKraken | Kraken's base actor
AMessageInABottleItemProxy | Message in a bottle actor
AFogBank | Fog's actor
AStorageContainer | Storage barrels actor, in the actor array are only ships barrels and supplies in the water
ACookingPot | Cooking pot actor (on ships and islands)
AFishingFish | Fish actor that spawns while someone is fishing
ABP_AmmoChest_C | Ammo chest (ships and islands)
AVolcano | Volcano actor


## Actors name filter
Object | Search Strings
------ | ----------------
Sapphire Gem | {"MermaidGem", "Sapphire"}
Emerald Gem | {"MermaidGem", "Emerald"}
Ruby Gem | {"MermaidGem", "Ruby"}
StrongholdKey | {"StrongholdKey"}
Stronghold Chest | {"TreasureChest", "Fort"}
Collectors Chest | {"CollectorsChest"}
Box of Wondrous Secrets | {"Wondrous"}
Chest of Sorrow | {"TreasureChest", "Weeping"}
Chest of a Thousand Grogs | {"TreasureChest", "Drunken"}
Chest of Legends | {"TreasureChest", "PirateLegend"}
Sea Dog Chest | {"TreasureChest", "Rare", "Rome"}
Castaway's Chest | {"TreasureChest", "Common"}
Seafarer's Chest | {"TreasureChest", "Rare"}
Marauder's Chest | {"TreasureChest", "Legendary"}
Captain's Chest | {"TreasureChest", "Mythical"}
Skeleton Captain's Chest | {"TreasureChest", "AIShip"}
Stronghold Gunpowder | {"BigGunpowderBarrel"}
Gunpowder | {"GunpowderBarrel"}
Banana Crate / Fruit Crate | {"BananaCrate"}
Storage Crate | {"AnyItemCrate"}
Cannonball Crate | {"CannonballCrate"}			
Wood Crate | {"WoodCrate"}
Fine Sugar | {"SugarCrate"}
Rare Tea | {"TeaCrate"}
Exotic Silks | {"SilkCrate"}
Exquisite Spices | {"SpiceCrate"}
Ancient Bone Dust | {"MerchantCrate", "Fort"}
Plants | {"Plants"}
Cloth | {"Cloth"}
Rum Bottles | {"Rum"}
Volcanic Stone | {"VolcanicStone"}
Fine Ore | {"Ore"}
Precious Gemstones | {"Gemstones"}
Extraordinary Minerals | {"Minerals"}
Chicken Crate | {"ChickenCrate"}
Snake Basket | {"SnakeBasket"}
Pig Crate | {"PigCrate"}
Foul Skull | {"BountyRewardSkull", "Common"}
Disgraced Skull | {"BountyRewardSkull", "Rare"}
Hateful Skull | {"BountyRewardSkull", "Legendary"}
Villainous Skull | {"BountyRewardSkull", "Mythical"}
Stronghold Skull | {"BountyRewardSkull", "AIShip"}
Skeleton Captain's Skull | {"BountyRewardSkull", "Fort"}
Decorative Coffer | {"Treasure", "Artifact", "box_01"}
Bronze Secret-Keeper | {"Treasure", "Artifact", "box_02"}
Golden Reliquary | {"Treasure", "Artifact", "box_03"}
Ancient Goblet | {"Treasure", "Artifact", "goblet_01"}
Silvered Cup | {"Treasure", "Artifact", "goblet_02"}
Gilded Chalice | {"Treasure", "Artifact", "goblet_03"}
Opulent Curio | {"Treasure", "Artifact", "impressive_01"}
Adorned Receptacle| {"Treasure", "Artifact", "impressive_02"}
Elaborate Flagon | {"Treasure", "Artifact", "impressive_03"}
Mysterious Vessel | {"Treasure", "Artifact", "vase_01"}
Peculiar Relic | {"Treasure", "Artifact", "vase_02"}
Ornate Carafe | {"Treasure", "Artifact", "vase_03"}
?Shiny Base? | {"Treasure", "Artifact", "base"}
Roaring Goblet | {"Treasure", "Artifact", "DVR_Common"}
Brimstone Casket | {"Treasure", "Artifact", "DVR_Rare"}
Devil's Remnant | {"Treasure", "Artifact", "DVR_Legendary"}
Magma's Grail | {"Treasure", "Artifact", "DVR_Mythical"}

### Usage example
```cpp
bool CompareActor(std::string actorName, std::vector<std::string> searchStrings)
{
    size_t offset = 0;
    for (auto &search : searchStrings)
    {
        offset = actorName.find(search);
        if (offset == std::string::npos)
        {
            return false;
        }
        offset++;
    }
    return true;
}

std::string itemName;
const auto actorName = actor->GetName();
if(CompareActor(actorName, { "MermaidGem", "Sapphire" }))
{
    itemName = "Sapphire Gem";
} else if (CompareActor(actorName, {"Wondrous"}))
{
    itemName = "Box of Wondrous Secrets";
}
```