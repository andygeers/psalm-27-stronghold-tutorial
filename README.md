# Bible Transformers 4: Psalm 27

## Introduction @showdialog

Welcome to Bible Transformers! Psalm 27 is a wonderful song of hope by King David
in the face of many enemies on every side.

> The Lord is my light and my salvation—
>    whom shall I fear?
> The Lord is the stronghold of my life—
>    of whom shall I be afraid?

He doesn't need to be afraid because the Lord God is his 'stronghold'.

Together we're going to make a game all about reaching the 'stronghold' and finding
safety.

> When the wicked advance against me
>    to devour me...
> Though an army besiege me,
>    my heart will not fear;
> though war break out against me,
>    even then I will be confident.

You will face many enemies in this game, but if you can reach safety in the Lord
then you don't need to be afraid!

## Step 1: Adding the player character

We're going to begin by populating our ``||loops:on start||`` block:

  * Create a sprite for your player character with the ``||variables(sprites):set sprite to||`` block. Set it to be of type "Player".

  * Set the player's initial position using ``||sprites:set sprite position||``.
The 'x' value means how far left - try 0 to start with. The 'y' value means how far from the top - try something like 65.

  * Now we want to be able to move the player around using the joystick controls. Open up the "Controller" menu and add:
``||controller:move sprite with buttons||``

  * Finally, let's make the camera move with the player:
``||scene:camera follow sprite||``

Try running the game and seeing what happens!

```blocks
let player = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . c c c . . . . . . . 
. . . . . . 5 3 5 . . . . . . . 
. . . . . . 3 3 3 . . . . . . . 
. . . . . 2 2 2 2 2 2 2 3 . . . 
. . . . 2 2 2 2 2 . . . . . . . 
. . . . 2 . 2 2 2 . . . . . . . 
. . . . 3 . 2 2 2 . . . . . . . 
. . . . . . b b b . . . . . . . 
. . . . . . b b b . . . . . . . 
. . . . . . b . b . . . . . . . 
. . . . . b b . b b . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Player)

player.setPosition(0, 63)
controller.moveSprite(player, 100, 100)
scene.cameraFollowSprite(player)
```

## Step 2: Creating a map

Now we're going to create a world for your character to explore!

Start by adding a ``||scene:set tilemap to||`` block to the workspace.

Now we're going to create a tilemap. Make a large grass area in the top left, perhaps
with some paths running top to bottom, and a castle wall down the right hand side
(our "stronghold").

Activate the "Draw walls" function:

Fill in your "stronghold" as a solid wall, and also the area at the bottom, below your grass area:

```blocks
tiles.setCurrentTilemap(tilemap``)
```

## Step 3: Adding some enemies

Time to add a bit of tension: let's create some enemies! Just as David was surrounded
by armies, we're going to add some soldiers marching back and forth.

* Start with a sprite for the first two soldiers, with the ``||variables(sprites):set sprite to||`` block.
You'll need to create a new variable for each one, and make sure you set them to be of type "Enemy".
* For each one, ``||sprites:set bounce on wall||`` to ON.
* Set the initial position: ``||sprites:set position||``. Try 25, 20 for the first one, and 71, 78 for the second one.
* Set an initial velocity (speed): ``||sprites:set velocity||``. Try 0, -42 for the first and 0, -72 for the second. That means that the second one will move faster than the second.
* Finally, for a bit of juice, add a sparking trail effect to each one: ``||sprites:start trail effect||``.

```blocks
let enemy1 = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . f . . . . . . . . 
. . . . . . f f f . . . . . . . 
. . . . . f f f f f . . . . . . 
. . . . . f d f d f . . . . . . 
. . . . . . d d d . . . . . . . 
. . . . . . a a a . . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . 2 2 2 f . . . . . . 
. . . . . . 2 . 2 f . . . . . . 
. . . . . . . . . f . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
let enemy2 = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . f . . . . . . . . 
. . . . . . f f f . . . . . . . 
. . . . . f f f f f . . . . . . 
. . . . . f d f d f . . . . . . 
. . . . . . d d d . . . . . . . 
. . . . . . 8 8 8 . . . . . . . 
. . . . . f 8 8 8 . . . . . . . 
. . . . f f f 8 8 . . . . . . . 
. . . . . f 8 8 8 . . . . . . . 
. . . . . f 4 4 4 . . . . . . . 
. . . . . f 4 . 4 . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
enemy1.setFlag(SpriteFlag.BounceOnWall, true)
enemy2.setFlag(SpriteFlag.BounceOnWall, true)
enemy1.setPosition(25, 20)
enemy2.setPosition(71, 78)
enemy1.setVelocity(0, -42)
enemy2.setVelocity(0, -76)
enemy1.startEffect(effects.trail)
enemy2.startEffect(effects.trail)
```

## Step 4: Losing the game

So you can walk around, and there's some baddies - but it's still not quite a ***game***
yet until you can **win** and **lose**. Let's start with the lose state - when one of
the guards catches you. We're going to start by creating an "event":

``||sprites:on sprite of kind Player overlaps `otherSprite` of kind Enemy||``

And when that happens, we will trigger a "game over" state:

``||game:game over LOSE||``

```blocks
sprites.onOverlap(SpriteKind.Enemy, SpriteKind.Player, function (sprite, otherSprite) {
    game.over(false)
})
```

## Step 5: Winning the game

For the win state, we want to detect when the player reaches the stronghold on the
right hand side. Thankfully, there's an easy way to tell when you hit the right-hand wall!

Every time the game updates we will run some code:

``||game:on game update||``

And we're going to test if the player is hitting the right hand wall:

``||logic:if||`` ``||scene:is player hitting wall right||``

If they are, then lets:

* Play a good sound effect: ``||music:play sound magic wand in background||``
* Show a relevant verse from Psalm 27: ``||game:show long text bottom||``
- let's make it show v1:
> "The Lord is the stronghold of my life- of whom shall I be afraid?"
* Trigger the WIN game state: ``||game:game over WIN||``. Why not add a special effect too?

```blocks

let player = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . c c c . . . . . . . 
. . . . . . 5 3 5 . . . . . . . 
. . . . . . 3 3 3 . . . . . . . 
. . . . . 2 2 2 2 2 2 2 3 . . . 
. . . . 2 2 2 2 2 . . . . . . . 
. . . . 2 . 2 2 2 . . . . . . . 
. . . . 3 . 2 2 2 . . . . . . . 
. . . . . . b b b . . . . . . . 
. . . . . . b b b . . . . . . . 
. . . . . . b . b . . . . . . . 
. . . . . b b . b b . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Player)

game.onUpdate(function () {
    if (player.isHittingTile(CollisionDirection.Right)) {
        music.play(music.melodyPlayable(music.magicWand), music.PlaybackMode.InBackground)
        game.showLongText("\"The Lord is the stronghold of my life- of whom shall I be afraid?\"", DialogLayout.Bottom)
        game.over(true, effects.smiles)
    }
})
```

## Step 6: More enemies

Well done, you're doing brilliantly! By now, you should have a simple game, which you
can both win and lose. But it's probably a little too easy right now, right?
David says in v6 about "the enemies who surround me" - not just one or two!

Let's add a slightly different kind of baddie, that marches past on a regular schedule:

* Add a ``||game:on game update every 800ms||`` block
* Once again, let's create a new sprite ``||variables(sprites):set sprite to||``, of type `Enemy` again
* ``||sprites:set position||`` and ``||sprites:set velocity||``.
Make one at position 119,10 and one at 215,10.
For velocity you could try 0,100 for both of them.
* For these ones, we'll make them disappear as soon as they hit the bottom wall:
``||sprites:set destroy on wall||`` to ON 
* Once again, you could ``||sprites:start trail effect||`` for extra juice.

Why not add a third one heading in the opposite direction? Try it every 1000ms this time,
position 168,120 and velocity 0,-79. This one will appear a little slower and less often,
and will be walking UP the screen not DOWN).

```blocks
let fast_1: Sprite = null
let fast_2: Sprite = null
let slow_1: Sprite = null

game.onUpdateInterval(800, function () {
    fast_1 = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . f . . . . . . . . 
. . . . . . f f f . . . . . . . 
. . . . . f f f f f . . . . . . 
. . . . . f d f d f . . . . . . 
. . . . . . d d d . . . . . . . 
. . . . . . 8 8 8 . . . . . . . 
. . . . . f 8 f 8 . . . . . . . 
. . . . . . f 8 8 f . . . . . . 
. . . . . f 8 f 8 f . . . . . . 
. . . . . . 2 2 2 f . . . . . . 
. . . . . . 2 . 2 . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
    fast_1.setFlag(SpriteFlag.DestroyOnWall, true)
    fast_1.setPosition(119, 10)
    fast_1.setVelocity(0, 100)
    fast_1.startEffect(effects.trail)
})
game.onUpdateInterval(800, function () {
    fast_2 = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . f . . . . . . . . 
. . . . . . f f f . . . . . . . 
. . . . . f f f f f . . . . . . 
. . . . . f d f d f . . . . . . 
. . . . . . d d d . . . . . . . 
. . . . . . a a a . . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . a a a f . . . . . . 
. . . . . . 2 2 2 f . . . . . . 
. . . . . . 2 . 2 f . . . . . . 
. . . . . . . . . f . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
    fast_2.setFlag(SpriteFlag.DestroyOnWall, true)
    fast_2.setPosition(215, 10)
    fast_2.setVelocity(0, 100)
    fast_2.startEffect(effects.trail)
})
game.onUpdateInterval(1000, function () {
    slow_1 = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . f . . . . . . . . . 
. . . . . f f f . . . . . . . . 
. . . . f f f f f . . . . . . . 
. . . . f d f d f . . . . . . . 
. . . . . d d d . . . . . . . . 
. . . . 2 2 2 2 2 . . . . . . . 
. . . 2 2 2 2 2 2 f . . . . . . 
. . . 2 2 2 2 2 f f f . . . . . 
. . . . 2 2 2 2 2 f . . . . . . 
. . . . 8 8 8 8 8 f . . . . . . 
. . . . 8 . . . 8 f . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
    slow_1.setFlag(SpriteFlag.DestroyOnWall, true)
    slow_1.setPosition(168, 120)
    slow_1.setVelocity(0, -79)
    slow_1.startEffect(effects.trail)
})
```
## Step 7: Scoring

For a bit of added replayability, it's a great idea to add some kind of high
scoring system, to give players an incentive to try again and see if they can do
better. Given the setting of our game, a time-based incentive would work well - 
let's reward the player with more points the quicker they beat the game.

* In our `||loops:on start||` event, let's start ``||info:start countdown 10s||``.
* Then just before our game WIN state, let's ``||info:set score||`` to ``||info:countdown||``. So if you complete it with 6 seconds left on the clock, you get 6 points, if you complete it with 4 seconds left on the clock, you get 4 points, and so on.
* Finally, every time you're about to LOSE the game, ``||info:set score||`` to 0.

Microsoft MakeCode Arcade automatically keeps track of your high score and things like
that, which is brilliant!

```blocks
info.startCountdown(10)
game.onUpdate(function () {
    if (player.isHittingTile(CollisionDirection.Right)) {
        music.play(music.melodyPlayable(music.magicWand), music.PlaybackMode.InBackground)
        info.setScore(info.countdown())
        game.showLongText("\"The Lord is the stronghold of my life- of whom shall I be afraid?\"", DialogLayout.Bottom)
        game.over(true, effects.smiles)
    }
})
sprites.onOverlap(SpriteKind.Enemy, SpriteKind.Player, function (sprite, otherSprite) {
    info.setScore(0)
    game.over(false)
})

```
## Step 8: A big boss

Finally, for a bit of added urgency to stop the player waiting too long to sneak past
one of the guards, lets add in a big boss enemy that starts to chase you once the timer runs out:

For this we'll use the ``||info:on countdown end||`` event:

* ``||music:play big crash sound in background||`` to warn the player something bad is about to happen!
* Use ``||variables(sprites):set sprite to||`` to create a boss of type `Enemy`
* ``||sprites:set position||`` to 0,50
* This time we will use ``||sprites:set boss follow player with speed 40||``
so that they chase you, wherever you are

```blocks
info.onCountdownEnd(function () {
    music.play(music.melodyPlayable(music.bigCrash), music.PlaybackMode.UntilDone)
    let boss = sprites.create(img`
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
. f . . . . . f f f f f . . . . 
. f f . . . f f f f f f f . . . 
. . . f . f f . f f f . f f . f 
. . . . f 5 . 5 5 f 5 . 5 . f f 
. . . . 5 . 8 5 5 5 5 5 8 f f . 
. . . . . 5 8 8 5 5 5 8 8 3 . . 
. . . . . 5 8 8 8 8 8 8 8 3 . . 
. . . . . 8 8 8 8 8 8 8 8 8 . . 
. . . . f 5 8 8 8 8 8 8 8 . . . 
. . f f . . 2 2 8 8 8 2 2 f f . 
. f f . . 2 2 2 2 2 2 2 2 2 f f 
. . . . . . 2 2 . . . 2 2 . . f 
. . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . 
`, SpriteKind.Enemy)
    boss.setPosition(0, 50)
    boss.setFlag(SpriteFlag.BounceOnWall, true)
    boss.follow(frog, 40)
})
```

## Conclusion

Well done, you made a game!

Let us leave you with this thought from King David:

> "One thing I ask from the Lord,
>    this only do I seek:
> that I may dwell in the house of the Lord
>     all the days of my life,
> to gaze on the beauty of the Lord
>    and to seek him in his temple.
> For in the day of trouble
>    he will keep me safe in his dwelling;" (Psalm 27:4-5)
