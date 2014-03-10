# What is RogueSharp? #
RogueSharp is a free library written in C# to help [roguelike](http://en.wikipedia.org/wiki/Roguelike "R]roguelike") developers get a head start on their game. RogueSharp provides many utility functions for dealing with map generation, field-of-view calculations, path finding, random number generation and more.

It is loosely based on the popular [libtcod](http://doryen.eptalys.net/libtcod/ "libtcod") or "Doryen Library" though not all features overlap.

# Getting Started #
Most interactions with RogueSharp is based around the concept of a `Map` which is a rectangular grid of `Cells`. 

Each `Cell` in a `Map` has the following properties:

- `IsTransparent`: true if visibility extends through the `Cell`.
- `IsWalkable`: true if a the `Cell` may be traversed by the player
- `IsExplored`: true if the player has ever had line-of-sight to the `Cell`
- `IsInFov`: true if the `Cell` is currently in the player's field-of-view

To instantiate a new `Map` you can use its constructor which takes a width and height and will create a new map of those dimensions with the properties of all `Cells` set to false.
    
**Usage**

	IMap boringMapOfSolidStone = new Map( 5, 3 );
	Console.WriteLine( boringMapOfSolidStone.ToString() );

**Output**

	#####
	#####
	#####

Notice that the `ToString()` operator is overridden on the `Map` class to provide a simple visual representation of the map. An optional bool parameter can be provided to `ToString()` to indicate if you want to use the field-of-view or not. If the parameter is not given it defaults to false. 

The symbols used are as follows:

- `%`: `Cell` is not in field-of-view
- `.`: `Cell` is transparent, walkable, and in field-of-view
- `s`: `Cell` is walkable and in field-of-view (but not transparent)
- `o`: `Cell` is transparent and in field-of-view (but not walkable)
- `#`: `Cell` is in field-of-view (but not transparent or walkable)

A more interesting way to create a map is to use the `Map` class's static method `Create` which takes an `IMapCreationStrategy`. Some simple classes implementing `IMapCreationStrategy` are provided with RogueSharp but this is easily extended by creating your own class that implements the strategy.

**Usage**

	IMapCreationStrategy<Map> mapCreationStrategy = new RandomRoomsMapCreationStrategy<Map>( 17, 10, 30, 5, 3 )
	IMap somewhatInterestingMap = Map.Create( mapCreationStrategy );
	Console.WriteLine( somewhatInterestingMap.ToString() );

**Output**

	#################
	#################
	##...#######...##
	##.............##
	###.###....#...##
	###...##.#####.##
	###...##...###..#
	####............#
	##############..#
	#################
