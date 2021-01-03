---
layout: post
title: Rhythm Shmup Devlog 2 - Bullet Particle Systems
date:   2021-01-03 16:00:00
categories: unity
---

This is the second in a series of posts regarding the development process of a 
rhythm hell dodge-em-up game that I've been working on. The first post which covers some of 
the overall design details of this game can be found [**here**](https://www.joeyrosicka.com/unity/2020/10/31/rhythm-shoot-em-up-devlog-design.html).

This post will cover the idea of using Unity's [**particle systems**](https://learn.unity.com/tutorial/introduction-to-particle-systems) 
to create bullet patterns, the implementation of this system, and some of the advantages and drawbacks to using them this way. I refer to particle systems as 
"Bullet Particle Systems" when they are used in this way. 

## The idea

![interface](/static/img/BulletParticleSystem/particleSystemInterface.png)

<blockquote>
Wait, all of this is built into Unity?
</blockquote>

When looking into tools to use for configuring bullet patterns, I was drawn to Unity's particle systems which have an 
extensive amount of functionality in place for configuring particle emission and particle properties. 
Details such as emission shapes, trajectories, speeds, particle colors, sizes, and more can be adjusted by manipulating 
various inputs for a particle system attached to a GameObject. 

Notably, particle systems provide instant feedback without needing to enter playmode in the Unity editor. One can make adjustments 
to a particle system and immediately see the differences, as shown below. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/BulletParticleSystem/configuringInInspector.mp4" type="video/mp4">
</video>
<br/><br/>
So, particle systems looked to be a wonderful tool with which to design and implement bullet patterns in my 
rhythm hell dodge-em-up (or any SHMUP, really) because of their impressive functionality and the 
instantaneous visualization feedback they provide. Using particle systems in this way does come with some significant 
drawbacks though, which are covered later. 

## Implementation

There were some considerations I needed to address when implementing Bullet Particle Systems in order to use Unity 
particle systems to shoot bullet patterns.

Particle systems do not provide a way to animate particles in multiple stages. For example, consider a bullet that 
spawns in with an animation that provides a "flare effect" - the bullet briefly appears large and completely white, 
then quickly shrinks down to its normal size and fades its color from white to black. 

<video class="center-block" width="100%" height="auto" controls="controls">
  <source src="/static/img/BulletParticleSystem/bpsAnimationFlare.mp4" type="video/mp4">
</video>
<br/><br/>
The beginning part of the bullets' appearance (animating from large and white to small and black) is different from 
the rest of their appearance (the same size and solid black color). We could implement this by configuring multiple 
keyframes for the "Color over Lifetime" and "Size over Lifetime" sections of the particle system, and that wouldn't be 
a bad option. 

However, this would be a bit limiting since it means we would need to have this opening bullet animation be the same entity as the rest 
of the bullet. What if we do not want the bullet's collider to match the size of its enlarged appearance at the beginning of 
its lifetime? After all, the point of the first stage of the bullet in this example is to appear as an animation flare, not 
to be part of the actual bullet. 

The solution I used was to allow a Bullet Particle System to include multiple particle systems and multiple nested Bullet Particle Systems. The particle systems 
are configured to match each other, and then the necessary modifications are made in order to represent each stage of 
the bullet pattern. 

So, when firing or stopping a bullet pattern, we would call the appropriate methods in the BulletParticleSystem script 
rather than in a single particle system: 

{% highlight csharp %}
public void Shoot() {
    foreach (ParticleSystem pSystem in subPSystems) {
        if (pSystem)
            pSystem.Play();
    }
}

public void StopAll() {
    foreach (ParticleSystem pSystem in subPSystems) {
        if (pSystem)
            pSystem.Stop();
    }
}
{% endhighlight %}

Back to the original example - we can now add a particle system for the main part of the bullet pattern and include collision 
on that, and add another particle system for the animate-in section of the pattern and exclude collision on that. 

Additionally, this system allows us to include multiple materials for the bullets created by a single Bullet Particle 
System. For example, we could assign a material to represent the inner section of the bullets and another material for the outer section of the bullets, and then 
define different animation behaviors for them such as different sets of color changes over time. 

So, a single Bullet Particle System might look like this: 

![structure](/static/img/BulletParticleSystem/bpsStructure.png)

Here, `Fire Effect` and `Visible` are child Bullet Particle Systems of `Bullet Particle System`, and `Fire Effect Outline`, 
`Fire Effect Middle`, `Visible Outline`, and `Visible Middle` are child particle systems. 

The root Bullet Particle System needs to be able to keep track of its child Bullet Particle Systems and 
particle systems so that it can send updates. It also needs to synchronize the seeds of all of its child particle systems 
so that any randomness applied to a particle system is applied in the same way to all other particle systems - we wouldn't 
want the animate-in section of a bullet to travel in the wrong direction relative to the main section of that bullet. 

{% highlight csharp %}
private bool IsRootBulletParticleSystem() {
    return GetComponentsInParent<BulletParticleSystem>(true).Length <= 1;
}

void Awake() {
    if (IsRootBulletParticleSystem())
        SyncParticleSeeds(true);
    
    // We do not want to keep this template particle system active at runtime
    if (Application.isPlaying)
        Destroy(GetComponent<ParticleSystem>());

    subBSystems = GetComponentsInChildren<BulletParticleSystem>(false).ToList();
    subPSystems = GetComponentsInChildren<ParticleSystem>(false).ToList();
}

private void SyncParticleSeeds(bool generateNew) {
    if (generateNew)
        particleSeed = (uint) Random.Range(0, int.MaxValue);
    
    foreach (ParticleSystem pSystem in GetComponentsInChildren<ParticleSystem>(true)) {
        pSystem.randomSeed = particleSeed;
    }

    foreach (BulletParticleSystem bSystem in GetComponentsInChildren<BulletParticleSystem>(true)) {
        bSystem.SetParticleSeed(particleSeed);
    }
}

public void SetParticleSeed(uint newParticleSeed) {
    particleSeed = newParticleSeed;
}
{% endhighlight %}

With that, all particle systems included in a root BulletParticleSystem will share the same seed. 

I also added a custom editor script to allow for copying the values of a BulletParticleSystem's particle system component 
to all of its children (and synchronizing the seeds) for convenience when configuring the BulletParticleSystem's behavior. 

{% highlight csharp %}
#if UNITY_EDITOR
    public void SyncParticleSystems() {
        ParticleSystem templatePSystem = GetComponent<ParticleSystem>();
        Assert.IsNotNull(templatePSystem, "Need to attach a ParticleSystem component!");
        UnityEditorInternal.ComponentUtility.CopyComponent(templatePSystem);
        foreach (ParticleSystem pSystem in GetImmediateChildrenParticleSystems()) {
            UnityEditorInternal.ComponentUtility.PasteComponentValues(pSystem);
            pSystem.randomSeed = particleSeed;
        }
        
        foreach (BulletParticleSystem bSystem in GetImmediateChildrenBulletParticleSystems()) {
            bSystem.SyncParticleSystems();
            bSystem.SetParticleSeed(particleSeed);
        }
    }
    
    [CustomEditor(typeof(BulletParticleSystem))]
    public class PatternMeasureEditor : Editor {
        public override void OnInspectorGUI() {
            DrawDefaultInspector();
            BulletParticleSystem bSystem = target as BulletParticleSystem;
            
            EditorGUILayout.Space();
            
            if (GUILayout.Button("Sync Particle Systems")) {
                if (bSystem == null) return;
                ParticleSystem pSystem = bSystem.GetComponent<ParticleSystem>();
                Assert.IsNotNull(pSystem, "Need to attach a ParticleSystem component!");
                pSystem.Stop();
                bSystem.StopAll();
                    
                bSystem.SyncParticleSystems();
                bSystem.SyncParticleSeeds(true);
            }
        }
    }
#endif
{% endhighlight %}

## Limitations

Although I was able to get around the animation limitation with particle systems, they still present other challenges 
that I have not been able to find practical solutions to. 

### Particles are not GameObjects

It would be very handy to be able to attach script components to bullets that are created from Bullet Particle Systems. 
This would allow us to define custom behaviors for our bullet patterns and design them to act in more interesting ways 
such as spawning collections of bullets that change direction or animation at some queue, bullets that follow the player, 
or bullets that we can perform any other arbitrary operation on. 

Of course, the bullets created from Bullet Particle Systems are not GameObjects that we can attach components to - they 
are particles. Every piece of a particle's behavior must be configured within its particle system, which means that adding programmable behavior is not possible. 

### Colliders on particles can only be circle colliders

![colliders](/static/img/BulletParticleSystem/particleColliders.png)

Particles can have colliders that send messages to GameObjects that they collide with, which is important so that 
we can handle when the player collides with enemy bullets. However, only circle colliders can be used, which is 
quite limiting - it means that we can only fire projectiles shaped like circles unless we want the bullet hitboxes 
to be wildly different from what the player expects (spoiler: we do not). 

### Particle emission depends on the frame rate

Generally, having game logic depend on a game's frame rate should be avoided - it is difficult to test and account for 
all of the different expected frame rates, and things can become messy quite quickly. For bullet patterns 
in particular, we would almost always want randomness to be deliberate rather than dependent on whatever machine the player 
is running the game on. 

Unfortunately, particle emissions from particle systems are indeed dependent on the frame rate. Specifically, if particles are 
configured to be emitted at high frequencies, some of these emissions will be skipped at lower frame rates rather than being 
delayed. This is apparent both in play mode, in builds, and in the scene view out of play mode.  

Particle systems are (from what I can tell) intended to be used for visual effects rather than for gameplay-impacting content. 
Most examples of particle systems that I've found are used for animations (e.g. explosions or flares) or ambient background 
effects (e.g. rain or specks of light). As such, perhaps a reason for the lacking functionality for individual particles is 
that these particles are not meant to be used in such a gameplay-dependent manner. 

## Final words

While working on the implementation of Bullet Particle Systems, it became more and more apparent that the system would not 
be robust enough for what I wanted in my game, particularly with the above limitations. So, I will use a custom-made 
solution that does not involve particle systems. However, the built-in configuration tools and instant feedback given by 
particle systems make them a powerful tool to use for implementing bullet patterns in a project with a small scope. As long as the 
limitations I covered are paid close attention to, particle systems could serve as a great tool for use in a small-scale SHMUP.