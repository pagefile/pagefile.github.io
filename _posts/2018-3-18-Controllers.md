---
layout: post
title: "Controllers"
date: 2018-3-18 12:51:00 -0600
---

Not the joypad kind. The code kind. It’s easy to just slap some input code on a player object to get it working. That’s all well and good until you need a different player object. Or a different kind of player object. That’s where having a controller class is handy. It separates the movement and control logic from the input logic. This let’s me do things like this:

{% include video.html file="/files/videos/controllers1.mp4" %}

It’s very trivial to have the player control the enemy ships, because both ships are agnostic to what is controlling them. In fact, they are the same class, with just different meshes and parameters. The fighter class knows how to move and shoot, but nothing else. There's no AI code and no input code. That's all in the controller classes.

```c#
public class PlayerController : FighterController
{
    #region Overrides
    public override void Init(Fighter entity)
    {
        base.Init(entity);
    }

    public override void Update()
    {
        float horizontal = Input.GetAxisRaw("Horizontal");
        float vertical = Input.GetAxisRaw("Vertical");
        bool firing = Input.GetButton("primaryFire");
        fighter.FireingGuns(firing);
        fighter.MovementAxis(vertical, horizontal);
    }
    #endregion
}
```

Here’s where the input code lives. Very simple. The enemy controller (for now) is even simpler. FighterController is an abstract class that takes care of a little background work that all controllers would need. Nothing particularly special

```c#
public class BasicEnemyController : FighterController
{
    #region Overrides
    public override void Init(Fighter entity)
    {
        base.Init(entity);
    }

    public override void Update()
    {
        fighter.MovementAxis(1.0f, 0.0f);
        fighter.FireingGuns(true);
    }
    #endregion
}
```

It’s a pretty dumb enemy. But it’s also easy to make it slightly better

```c#
public class BasicEnemyWaveController : FighterController
{
    [SerializeField]
    private float Frequency = 1.0f;

    private float time = 0.0f;

    #region Overrides
    public override void Init(Fighter entity)
    {
        base.Init(entity);
    }

    public override void Update()
    {
        time += Time.deltaTime;
        fighter.MovementAxis(1.0f, Mathf.Sin(time * Frequency));
        fighter.FireingGuns(true);
    }
    #endregion
}
```

And now I have one that moves back and forth. I’ll plug it into the game.


And of course, I can have the player ship do the same thing


Even in single player game a design like this is important because of the flexibility it offers. I’m not trapped by the limitations of code, so if down the line I want something like a “Reverse Boss Rush” mode where the player plays as bosses, the core mechanics are very simple to implement. It also makes for nice and clean code that’s easy to read. No one likes a multithousand line behemoth.
