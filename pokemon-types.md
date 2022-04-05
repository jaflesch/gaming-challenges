# POKÉMON TYPES CHALLENGE #
![Banner](https://i.imgur.com/9oupLmq.png)

## Description ##
Pokémon (ポケモン Pokemon?) is a media franchise owned by The Pokémon Company, having been created by Satoshi Tajiri in 1995. The franchise began with a pair of games released for the original Game Boy, developed by Game Freak and published by Nintendo. . Currently, the franchise spans across games, trading cards, television series, as well as movies, manga, and toys. It centers on fictional creatures called *Pokémon*, which humans capture and train them to fight each other with their various types of attacks.
 
Initially, there were 15 different skill types. Subsequently, the range of options was extended to 18 types. Each type of move has an advantage, disadvantage, immunity or neutrality against another type. This creates 1296 (18&times;18&times;4) possible combinations between Pokémon skill types.

The chart below displays the possible combinations via damage multipliers.

![Pokémon Types Chart](https://upload.wikimedia.org/wikipedia/commons/9/97/Pokemon_Type_Chart.svg)

- In **red (1/2)**, it means that the blow will have its power reduced by half
- In **green (2)**, it means that the blow will have its power doubled
- In **black(0)**, it means the hit will be nullified, since the target type has immunity against the hit
- In **white**, it means that the hit will have its normal damage (ie the multiplier will be 1).

These different combinations enrich the strategies between Pokémon battles, since during an attack turn, every move has a type and, on the defense turn, the target Pokémon has 1 to 2 types. For example, imagine a battle between the Pokemons [Charizard](https://www.pokemon.com/br/pokedex/charizard) and [Diglett](https://www.pokemon.com/br/pokedex/diglett) . Charizard is of the **FIRE** and **FLYING** types. Diglett is only of type **EARTH**.

Given the scenario above, this means that if Diglett uses an EARTH-type move on Charizard, despite the FIRE weakness, he will not take damage, as FLYING is immune to EARTH-type attacks. On the other hand, if Charizard uses a FIRE move on Diglett, he takes damage normally. This shows that just because one type has an advantage over another doesn't necessarily mean the other has a disadvantage over it, considering the attacker and the defender. Now, if Diglett uses a STONE-type move on Charizard, Charizard will take quadruple damage: FIRE and FLYING take double STONE damage. Similarly, if Diglett used a BUG-type move on Charizard, he would have a 1/4 multiplier to the move, as FIRE and FLYING take half damage from BUG.

## Objective ##
Develop an API that provides a template of advantages, disadvantages, immunity, and neutrality over Pokémon move type combinations. The API must present the following routes:

> Pokémon types will be treated as integers. So the first type, NORMAL, will be 0; FIRE, 1, and so on until the last, FAIRY, which will be 17.

#### (GET) /types

**Parameters**
- `att` (query /int/optional): Attacker type
- `def` (query /int/optional): Defender type

**Return**
- If `att` and `def` are empty, returns an array with the list of types shown in the graph previously
- If `att` with value and `def` empty, returns an array with the offensive multipliers of type (array row)
- If `att` empty and `def` with value, returns an array with defensive multipliers of type (array column)
- If `att` and `def` with values, returns an integer referring to the resulting multiplier of the combination

#### (GET) /battle/{type1}/{type2}

**Parameters**
- `type1` (path / int / required): Attacking Pokemon's move type
- `type2` (path / string / required): A list of types of the target Pokemon, in the format: `<number>,<number>`

**Return**
- Returns an integer referring to the resulting multiplier of the combination

#### (GET) /check/{multiplier}/{type1}/{type2}

**Parameters**
- `multiplier` (path / string / required): Multiplier resulting from the combination. Accepted values ​​are `(Z, Q, H, N, D, F)` (0, 1/4, 1/2, 1, 2, and 4 respectively)
- `att` (query / int / required): Attacking Pokemon's move type
- `def` (query / string / required): A list of types of the target Pokemon, in the format: `<number>,<number>`

**Return**
- Returns a boolean referring to the validity informed as input and the resulting multiplier of the combination

#### (GET) /advantages/{type}

**Parameters**
- `type` (path / int / required): Pokémon type
- `alt` (query / boolean / optional): Whether the return will be

**Return**
- List of Pokémon types where the type informed as input has an advantage. If `alt` is entered as a parameter in the query, the list should present the types with the name in strings, making it easier for humans to read. Otherwise, the list will be integers.
 
## Examples ##
 
Using the previous scenario, in the battle between Charizard and Diglett, possible API calls would be:

`(GET) /types`
```javascript
// Return:
[
  [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0.5, 0, 1, 1, 0.5, 1],
  [1, 0.5, 0.5, 2, 1, 2, 1, 1, 1, 1, 1, 2, 0.5, 1, 0.5, 1, 2, 1],
  [1, 2, 0.5, 0.5, 1, 1, 1, 1, 2, 1, 1, 1, 2, 1, 0.5, 1, 1, 1],
  [1, 0.5, 2, 0.5, 1, 1, 1, 0.5, 2, 0.5, 1, 0.5, 2, 1, 0.5, 1, 0.5, 1],
  [1, 2, 0.5, 0.5, 1, 1, 1, 1, 0, 2, 1, 1, 1, 1, 0.5, 1, 1, 1],
  [1, 0.5, 0.5, 2, 1, 0.5, 1, 1, 2, 2, 1, 1, 1, 1, 2, 1, 0.5, 1],
  [2, 1, 1, 1, 1, 2, 1, 0.5, 1, 0.5, 0.5, 0.5, 2, 0, 1, 2, 2, 0.5],
  [1, 1, 1, 2, 1, 1, 1, 0.5, 0.5, 1, 1, 1, 0.5, 0.5, 1, 1, 0, 2],
  [1, 2, 1, 0.5, 2, 1, 1, 2, 1, 0, 1, 0.5, 2, 1, 1, 1, 2, 1],
  [1, 1, 1, 2, 0.5, 1, 2, 1, 1, 1, 1, 2, 0.5, 1, 1, 1, 0.5, 1],
  [1, 1, 1, 1, 1, 1, 2, 2, 1, 1, 0.5, 1, 1, 1, 1, 0, 0.5, 1],
  [1, 0.5, 1, 2, 1, 1, 0.5, 0.5, 1, 0.5, 2, 1, 1, 0.5, 1, 2, 0.5, 0.5],
  [1, 2, 1, 1, 1, 2, 0.5, 1, 0.5, 2, 1, 2, 1, 1, 1, 1, 0.5, 1],
  [0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 2, 1, 0.5, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 0.5, 0],
  [1, 1, 1, 1, 1, 1, 0.5, 1, 1, 1, 2, 1, 1, 2, 1, 0.5, 1, 0.5],
  [1, 0.5, 0.5, 1, 0.5, 2, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 0.5, 2],
  [1, 0.5, 1, 1, 1, 1, 2, 0.5, 1, 1, 1, 1, 1, 1, 2, 2, 0.5, 1],
]
```

`(GET) /types?att=1`
```javascript
// Return:
[1, 0.5, 0.5, 2, 1, 2, 1, 1, 1, 1, 1, 2, 0.5, 1, 0.5, 1, 2, 1]
```

`(GET) /types?att=8`
```javascript
// Return:
[1, 2, 1, 0.5, 2, 1, 1, 2, 1, 0, 1, 0.5, 2, 1, 1, 1, 2, 1]
```

`(GET) /types?att=1&def=8`
```javascript
// Return:
1
```

`(GET) /types?att=8&def=1`
```javascript
// Return:
two
```

`(GET) /types?att=8&def=9`
```javascript
// Return:
0
```

`(GET) /battle/8/1.9`
```javascript
// Return:
0
```

`(GET) /battle/11/1.9`
```javascript
// Return:
0.25
```

`(GET) /battle/12/1.9`
```javascript
// Return:
4
```

`(GET) /check/D/8/1.9`
```javascript
// Return:
false
```

`(GET) /check/N/1/8`
```javascript
// Return:
true
```

`(GET) /advantages/6`
```javascript
// Return:
[0, 5, 12, 15, 16]
```

`(GET) /advantages/6?alt=1`
```javascript
// Return:
['NORMAL', 'ICE', 'ROCK', 'DARK', 'STEEL']
```


Good luck!
