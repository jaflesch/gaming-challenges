# CYBERPUNK 2077 BREACH PROTOCOL CHALLENGE #
![Banner](https://i.imgur.com/HCgOqdy.png)

## Description ##

Cyberpunk 2077 is an action role-playing video game developed and published by CD Projekt. The story takes place in Night City, an open world set in the Cyberpunk universe. Players assume the first-person perspective of a customisable mercenary known as V, who can acquire skills in hacking and machinery with options for melee and ranged combat.

When you start a Breach Protocol, you’ll enter a kind of puzzle minigame.

## Breach Protocol Sample  ##

Among the different hacking proposals that the game offers, one of them is called Breach Protocol Invasion. In terms of gameplay, this translates into a puzzle minigame that consists of injecting Daemons (malicious programs) into the network to be invaded.

The minigame consists of 4 parts: *Breach Time Remaining (BTR)*, *Buffer (B)*, *Code Matrix (M)*, *Sequence to Upload (S)*.

- Breach Time Remaining is a counter that starts from a value and is decremented to 0
- Buffer are slots that save each position of the traversed matrix
- Code Matrix is the system code matrix, where each position of the matrix corresponds to a code snippet
- Sequence to Upload is a list of up to 3 Daemons, where each Daemon is a sequence of code snippets present in the main array (M)

The image below presents the parts that make up the minigame as well as its look.

![Breach Protocol Sample](https://cdn.vox-cdn.com/thumbor/mZkjbe9UajcNUL_B4iKCuN97BAQ=/0x0:2880x1622/920x0/filters:focal(0x0:2880x1622):format(webp):no_upscale()/cdn.vox-cdn.com/uploads/chorus_asset/file/22156955/Cyberpunk_2077_Breach_Protocol_copy.jpg)

## Minigame mechanics ##

In a very simplified way, the game starts generating, in a random way, the code snippets of the main matrix and sequences for uploading Daemons. From there, the player needs to navigate between the positions of the matrix, selecting code snippets that match the sequences for uploading Daemons.

Every move will be saved in the Buffer until it is full. When performing the first move, the timer starts and the counter is decremented until it reaches 0. Finally, the minigame ends when: (1) the Buffer is full, (2) all Daemons in the sequences are successfully injected (all sequences are combined), or (3) the counter reaches 0.

If the player completes at least one Daemon streak before the timer runs out, he wins. Otherwise, you lose.

However, this is a minimalist view of the minigame. There are several other rules to make it interesting and stay true to the puzzle mechanics. Are they:

- BRT and Buffer have variable sizes according to the character's skills in the game
- M has dimensions 6×6, that is, 36 positions
- Each position is occupied by a character, written in hexadecimal value. Possible values are `1C`, `55`, `7A`, `BD`, `E9`, `FF`
- The Sequence to Upload is a list ranging from 1 to up to 3 Daemons
- A Daemon is a string with a length ranging from 3 to 5 characters. Possible characters, expressed in hexadecimal, are the same as for M: `1C`, `55`, `7A`, `BD`, `E9`, `FF`
- To traverse the array and select characters, it is necessary to obey an alternating sequence of characters in a row and a column. Each character selected toggles the selection of the next one. That is, if the first character was selected in a row, the next one will be from the column of the previously chosen character (the next section of this guide explains the matrix movement better with examples)
- A character of a certain position can only be selected once. 
- Each selected character fills a Buffer position
- Character selection is performed until the BRT reaches zero OR the Buffer is not full

## Matrix Traverse sample ##

To facilitate the sample, imagine the following example matrix (3×3):

<table>
<tr>
<td>5A</td>
<td>1C</td>
<td>FF</td>
</tr>
<tr>
<td>BD</td>
<td>1C</td>
<td>7A</td>
</tr>
<tr>
<td>FF</td>
<td>BD</td>
<td>BD</td>
</tr>
</table>

The first move always starts at the column selection of the first row, so the options are `5A`, `1C` or `FF`.

<table>
<tr>
<td><strong><em><ins>5A</ins></em></strong></td>
<td><strong><em><ins>1C</ins></em></strong></td>
<td><strong><em><ins>FF</ins></em></strong></td>
</tr>
<tr>
<td>BD</td>
<td>1C</td>
<td>7A</td>
</tr>
<tr>
<td>FF</td>
<td>BD</td>
<td>BD</td>
</tr>
</table>

If the character `5A` is chosen, then all characters in the first column except the one selected earlier are eligible, ie `BD` and `FF`. Similarly, if the second column (`1C`) had been chosen, the options would be `1C`, `BD`. Finally, if it were the third column (`FF`), `7A` and `BD` would be eligible.

Continuing the first example (`5A`), the next options would be:

<table>
<tr>
<td><s>5A</s></td>
<td>1C</td>
<td>FF</td>
</tr>
<tr>
<td><strong><em><ins>BD</strong></em></ins></td>
<td>1C</td>
<td>7A</td>
</tr>
<tr>
<td><strong><em><ins>FF</strong></em></ins></td>
<td>BD</td>
<td>BD</td>
</tr>
</table>

In this column, we select the character of the second line `BD`. Now, we go back to choosing a character from the columns of this row again. So, the options become `1C` and `7A`:

<table>
<tr>
<td><s>5A</s></td>
<td>1C</td>
<td>FF</td>
</tr>
<tr>
<td><s>BD</s></td>
<td><strong><em><ins>1C</strong></em></ins></td>
<td><strong><em><ins>7A</strong></em></ins></td>
</tr>
<tr>
<td>FF</td>
<td>BD</td>
<td>BD</td>
</tr>
</table>

The next character selected will be the one in the last column of the current line (`7A`). The next possible options will be `FF` or `BD`:

<table>
<tr>
<td><s>5A</s></td>
<td>1C</td>
<td><strong><em><ins>FF</strong></em></ins></td>
</tr>
<tr>
<td><s>BD</s></td>
<td>1C</td>
<td><s>7A</s></td>
</tr>
<tr>
<td>FF</td>
<td>BD</td>
<td><strong><em><ins>BD</strong>/<em></ins></td>
</tr>
</table></table>
  
The next character chosen will be `FF` and consequently the only available column will have the character `1C`:
  
<table>
<tr>
<td><s>5A</s></td>
<td><strong><em><ins>1C</td>
<td><s>FF</td>
</tr>
<tr>
<td><s>BD</s></td>
<td>1C</td>
<td><s>7A</s></td>
</tr>
<tr>
<td>FF</td>
<td>BD</td>
<td>BD</td>
</tr>
</table></table></table>

The character choices will continue to alternate between rows and columns until the Buffer is full or the BRT ends. In the presented scenario, the Buffer would have stored the following values: `5A`,`BD`,`7A`,`FF`,`1C`.

If there is at least one Daemon, from the Sequence List of uploads, with a sequence (partial or total) of the contents of the Buffer, then it is injected into the network, or, in other words, the player wins the minigame.

Note that if a Daemon with the word `5A 7A 1C` would not be injected into the network with the values of the Buffer in question. That is, even if you have the correct characters, the order matters. Valid sequences would be: `5A BD 7A`, `BD 7A FF`, `7A FF 1C`, `5A BD 7A FF`, `BD 7A FF 1C`, or `5A BD 7A FF 1C`.

To complement the explanations, [this video shows a recorded snippet of Cyberpunk's gameplay where it is possible to observe the mechanics of Breach Protocol](https://www.youtube.com/watch?v=nKcOpYEUklg&ab_channel=ItsShatter).


## Objective ##

> Develop a SPA (Single Page Application) solution that implements Cyberpunk 2077's Breach Protocol minigame. 

When mapping the rules to the algorithm of this challenge, we will make some simplifications. They are:
- BRT will be fixed at 60 seconds
- Buffer has fixed size of 6 slots
- Possible hex codes are `BD`, `1C`, `55`, `7A`, `E9`

Good luck!
