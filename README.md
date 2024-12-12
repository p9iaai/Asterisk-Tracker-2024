# Asterisk Tracker - JavaScript One-Liner (2024)

A modern JavaScript recreation of the classic 1984 BBC BASIC one-liner game "Asterisk Tracker", maintaining the spirit of the original single-line implementation.

## History

This is a JavaScript port of a famous BBC BASIC one-liner game from 1984, originally published in BEEBUG Magazine by N. Silver. The game later inspired:
- Stewart Russell's "Star Dodger" series (Amstrad Computer User, 1988)
- Graham French's "Star Dodger V2" (Amstrad Action, 1992)
- [A modern remake by p9iaai](https://github.com/p9iaai/StarDodgerV2-2024Remake) (2024)

## The Game

Navigate your player through a field of asterisks to reach the gap on the right side of the screen. Your dot moves diagonally down-right automatically, and holding SPACE makes it move up-right instead.

- Each level adds 5 more asterisks to dodge
- Hit an asterisk = game over
- Hit the walls = game over
- Reach the gap = next level
- Score starts at 0 and increases by 1 with each level completed

## The Code

The entire game logic is contained in a single line of JavaScript, requiring only minimal HTML/CSS setup for the canvas. This maintains the spirit of the original BBC BASIC one-liner while using modern web technologies.

## Running the Game

1. Download `index.html`
2. Open it in any modern web browser
3. Use SPACE to control upward movement
4. Try to reach the gap on the right side
5. Don't hit the asterisks or walls!

## Credits

- Original BBC BASIC game: N. Silver (BEEBUG Magazine, 1984)
- JavaScript one-liner recreation: Created in response to a discussion on [CPCWiki](https://www.cpcwiki.eu/forum/general-discussion/i-ve-recreated-an-old-cpc-type-in-game/)

## Original BBC BASIC One-liner

For historical reference, here's the original one-liner from 1984...

```basic
1L=0:REP.L=L+3:MO.4:DR.1279,0:DR.1279,452:MOVE1279,572:DR.1279,1023:DR.0,1023:F.I=1TOL:V.31,RND(32)+5,RND(31),42,30:N.:P.(L-3)/3:X=0:Y=512:REP.PL.69,X,Y:X=X+4:Y=Y-(INKEY-74+.5)8:U.PO.X,Y)=1ORX=1280:U.X<1280:V.7:REP.U.INKEY-99:RUN
```

...and a readable (and editable) version with comments because I'm a nice person:

```basic
10 L=0                              'Score initialization
20 REPEAT                           'Main game loop
30 L=L+3                            'Score increment
40 MODE 4                           'Graphics mode
50 DRAW 1279,0                      'Draw play area
60 DRAW 1279,452                    'Bottom barrier
70 MOVE 1279,572                    'Position for top
80 DRAW 1279,1023                   'Top barrier
90 DRAW 0,1023                      'Complete boundary
100 FOR I=1 TO L                    'Obstacle loop
110 VDU 31,RND(32)+5,RND(31),42,30  'Place asterisks
120 NEXT
130 PRINT (L-3)/3                   'Show score
140 X=0                             'Player X position
150 Y=512                           'Player Y position
160 REPEAT                          'Movement loop
170 PLOT 69,X,Y                     'Draw player
180 X=X+4                           'Move right
190 Y=Y-(INKEY(-74)+.5)*8           'Up/down control
200 UNTIL POINT(X,Y)=1 OR X=1280    'Collision check
210 UNTIL X<1280                    'Game over check
220 VDU 7                           'Beep!
230 REPEAT UNTIL INKEY(-99)         'Wait for SPACE
240 RUN                             'Play again
```


## What's Going on Here, Then?

It's a one-liner!

```javascript
(s=1,x=0,y=512,trail=[],stars=[],newGame=()=>{stars=Array(1+s*5).fill().map(()=>({x:Math.random()*1200+50,y:Math.random()*900+50}))},newGame(),addEventListener('keydown',e=>k=e.code=='Space'),addEventListener('keyup',e=>k=0),setInterval(()=>{with(g.getContext`2d`){clearRect(0,0,1280,1024);fillStyle='#fff';fillRect(1279,0,1,452);fillRect(1279,572,1,451);font='16px monospace';stars.forEach(s=>fillText('*',s.x,s.y));trail.push({x,y});if(trail.length>300)trail.shift();trail.forEach((p,i)=>fillRect(p.x,p.y,4,4));x+=1.5;y+=k?-3:1.5;if(stars.some(s=>Math.abs(s.x-x)<8&&Math.abs(s.y-y)<8)||y<0||y>1023||(x>=1279&&(y<=452||y>=572))){s=1;x=0;y=512;trail=[];k=0;newGame();alert('Game Over!\nPress OK to play again')}else if(x>=1279&&y>452&&y<572){alert(`Score: ${s}\nPress OK for next level`);s++;x=0;y=512;trail=[];k=0;newGame()}}}),16)
```

I have to wrap it in an HTML file to make it work in a browser, but the code is all in the `<script>` tag. That counts, right?

Want to experiment? Here are the key variables you can tweak:

### Initial setup
```
s = 1                   // Starting score/level
x = 0                   // Player starting X position
y = 512                 // Player starting Y position (middle of screen)
```

### Movement speeds
```
x += 1.5                // Horizontal movement speed
y += k ? -3 : 1.5       // Vertical speeds: -3 up (when SPACE pressed), 1.5 down
```

### Trail effects
```
trail.length > 300      // Maximum trail length (higher = longer trail)
fillRect(p.x,p.y,4,4)   // Trail dot size (4x4 pixels)
```

### Level generation
```
1+s*5                   // Number of asterisks (increases with score)
Math.random()*1200+50   // Asterisk X position range
Math.random()*900+50    // Asterisk Y position range
```

### Collision detection

```
Math.abs(s.x-x)<8       // Collision distance with asterisks
Math.abs(s.y-y)<8       // (smaller numbers = tighter collisions)
```
### Animation timing
```
setInterval(..., 16)    // Game update interval (16ms = ~60fps)
```

## License

Feel free to use and modify as you wish!
