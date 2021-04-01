---
title: Guide
template: default.html
name: guide
---

<h1 class="hero center-text">Traindown: How To</h1>
<h2 class="center-text">An introduction to using Traindown to record your training.</h2>

---

Traindown was created to help you easily capture all the things you do in a training session. It aims to provide you with a flexible, easy to remember language for expressing your training for record keeping purposes. As you will learn, writing out your training in **Traindown** is as simple as using a hashtag.

*If you are a developer interesting in working with **Traindown**, please refer to the [Spec](/spec) for more low level details on the language.*

## The Basics

At its core, **Traindown** allows us to express simply a training event which we call a **performance**.
```
Squat: 500
```
This is saying "I performed **one** rep of **500** in the **Squat**." 500 of what, you may ask? Well, that brings us to the next section!

## Metadata

**Traindown** provides tools for recording units (and any other data you can imagine) with **metadata**. Metadata are just **key value pairs** that begin with a **#**. You can have as much or as little metadata as you prefer, but the more the better in my opinion!

Let's add some metadata to the session from above to help us understand more about what happened that day.
```
# Unit: lbs
Squat: 500
```
Ah, that's better. So this session used pounds for the unit of weight. That squat was 500 _pounds_.

Metadata allows you to express facts about the context of your training. Some common keys are duration, day/week of training cycle, and if you have any injuries you are working through. Some folks capture things like their weight that day or even what they eat leading into it. Let's go ahead and some more data to our example session.
```
# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

Squat: 500
```
All the metadata we just added to the top of the file is called **session metadata**. This information applies to the entire session. **Traindown** also supports the concepts of **performance metadata** and **movement metadata**.

Like we learned in the beginning of this guide, each combination of a load for sets and reps is considered a **performance** of a given movement. So in our example, lets assume this trainee likes to capture their "reps in reserve" for their top sets.
```
# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

Squat: 500
  # RIR: 2
```
This is saying "For the single squat at 500 pounds, I had 2 reps in reserve." Unlike the burrito count, the reps in reserve are applied to only the performance. This is because it follows immediately the target performance.

You may also track **movement metadata** that applies to all performances of the movement. To do so, just place your key value pair after the movement name and before any performances.

Like with the session or performance metadata, the target movement *immediately preceeds* the metadata to which it should apply.

By adding more details in the metadata, you can make more sense of the _why_ in your training at both the session and perofmrance level. Metadata is also very searchable, so if you choose to use a **Traindown** enabled analysis tool, you can really start to make sense of the big picture.

But wait...one key pieces is missing. _When_ did this happen?

## Date and Time

The date and time when your training occurs is similar to metadata but is special enough to warrant it's own symbol. To specify the date and/or time that you trained, just use **@**, typically as the first thing in your **Traindown** file.

In files where you may include more than a single session, the **@** will mark individual sessions.

**NOTE:** You need to make sure and include the **@** symbol if you wish to use **Traindown**. Without it, the software is not able to correctly parse your training data because it will have no signal about when your training occurred.

To build on our example, it would look like
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

Squat: 500
  # RIR: 2
```
The format for the date and time is entirely up to you. Keep in mind if you are using a **Traindown** analytics tool, there may be a preferred format or formats, so be sure to read up on your particular tool.

## Sets and Reps

Now, our example is pretty unrealistic. Cold squatting 500 pounds, three burritos or none, is not common. Let's assume this is a more mortal trainee and fill in some of the details of the day.
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

Squat: 135 10r 2s 225 5r 315 5r 405 2r 455 500 3s
```
That's more realistic. So what was added here? It should read intuitively but to be sure, let's dive into the details.

Looking at
```
135 10r 2s
```
we can read this as "I performed two **sets** of ten **reps** at 135 (pounds if we look at the session metadata)." As you can see, we denote sets with an **s** and reps with an **r**.

Further on down the day, we can see
```
225 5r
```
which can be read as "I performed one set of five reps at 225 (pounds on the Squat given the other information)." If we omit either an r or and s, the default is 1. So circling back to the original 500, it _could_ be expressed as the following.

---

**DON'T DO THIS**
```
500 1r 1s
```
**DON'T DO THIS**

---

**Note:** Seriously, don't do this. Save yourself the 1r / 1s shenanigans.

The whole point of **Traindown** is to make recording your training easy, not annoying and pedantic. The above example should look like

---

**DO THIS INSTEAD**
```
500
```
**DO THIS INSTEAD**

---

The order of the load, reps, and sets does not matter—we'll leave that debate to be settled on another date. However, folks _typically_ lead a performance with the load.

There is one piece missing with our story so far. It's a dark thing. Not to be spoken of. That's right, I am talking about missed lifts.

If you are training and need to capture failed attempts, **Traindown** allows you to do that with the "failed rep" symbol **f**.

To keep our burrito powered example free of the scourage known as missed lifts, let's use a simple example to illustrate
```
Snatch:
  60 3s
  80 2s
  100
  110
  120 1f
  120
```
Here we can see that the lifter was successful up until the 120 at which point they happened to miss the first attempt there. They appear to have been able to correct and rally to successfully hoist that 120.

The **failed rep** allows you to capture the deficit in what you originally planned on hitting when used in conjunction with the rep symbol. This can be used to capture multiple failed reps, as well. For example
```
Glute bench raise:
  1000 10r
  1000 10r 6f
```
In the literal "butt blaster" example above, the trainee would be credited with 4 completed reps and 6 missed reps. The reason we use **10r** is to indicate the _reps attempted_. In the first example, the implied **1r** is considered and the trainee is credited with no completed lifts. 1 - 1 = 0.

Another tool in your quest to capture all the sets and reps you've done is the **+** operator. This symbol allows you to link together movements to record your supersets or rounds.

Blasting your bis and tris? Well, sun's outs, guns out! Let's try it out
```
Bi Blaster 9000: 100 20r 3s
+ Tri Destroyer Supreme: 90 10r 100 10r 110 10r
```
You can chain as many movements together as you feel like doing. The number of sets _should_ match but if you happen to want another set on the Tri Destroyer Supreme, go for it.

For recording rounds, an example could be 20.4 from the Crossfit Open
```
Deadlift: 255 21r 255 15r 255 9r
+ Handstand Push-Up: 21r 15r 9r

Deadlift: 315 21r 315 15r 315 9r
+ Handstand Walk: 50 50 50
```
No matter how metconned you get, **Traindown** can support your recording of it!

## Formatting for Easy Reading

Back to the burrito example...I personally like how succinct it looks but it is difficult to read, especially after three burritos. The same training could be expressed in a more reader-friendly format like
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

Squat:
  135 10r 2s
  225 5r
  315 5r
  405 2r
  455
  500 3s
```
I dunnno about you, but that is _much_ easier on my eyes. Since **Traindown** doesn't specify rules around whitespace, you are free to really do whatever in terms of layout. Just be careful not to assert the superiority of spaces over tabs...

## Notes

So now we have a good idea of what happened on that day, but I'd imagine after eating three burritos, they may have been some...issues along the way. Lucky for us, **Traindown** provides tools for capturing notes. Let's add some!
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

* Definitely should have stopped at two burritos.

Squat:
  135 10r 2s
  225 5r
  315 5r
  405 2r
  455
  500 3s
```
What we just added was a **session note**. This is a note that applies to the entire training session and we know that because the rules for applying notes are the same as applying metadata. A note will apply to either the session, movement, or performance that immediately preceeds it. These notes are helpful for painting a landscape of your training. Let's add some fine details!
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

* Definitely should have stopped at two burritos.

Squat:
  135 10r 2s
  225 5r
  315 5r
  405 2r
    * Had to stop at two reps to run to the bathroom.
  455
  500 3s
    * Surprisingly easy singles given that episode at 405.
    * Consider going up in weight next week.
```
Remember that each combination of a load for sets and reps is considered a **performance** of a given movement. Notes that appear _after_ a performance are called...you guessed it, **performance notes**. You can apply to whatever granularity of work you like to capture. For example, the three singles at 500 could look something like
```
  500
    * Surprisingly easy single given that episode at 405.
  500
    * Whoops. Spoke too soon. Clenching!
    # pucker factor: 9000
  500
    * Another easy rep.
```
Similar to metadata, there exists a third kind of note available to you called the **movement note**. This note is useful for capturing information about the entire movement such as stance, grip placement, etc. A movement note is declared after the movement name and before any performances. For example
```
Squat:

  * Worked on trying to increase interabdominal pressure, but not too much considering the burritos.
  * Grip was pinky on the rings.

  135 10r 2s
  225 5r
  315 5r
  405 2r
    * Had to stop at two reps to run to the bathroom.
  455
  500 3s
    * Surprisingly easy singles given that episode at 405.
    * Consider going up in weight next week.
```
The notes about pressure and hand placement apply to _all the performances_ of the movement.

## Recap

You now know most of **Traindown**. Congrats!

Each session has many performances as well as a date and time on which it occurred. All three types of information-sessions, movements, and performances-can have notes and metadata. The layout of your file doesn't really matter in so far as you can read it years from now or the tool you use to analyze your training can read it.

It's entirely up to you how you want to record your training and I recommend trying out different strategies to see what feels best for you. If you happen to have any questions, please feel free to email me at <a href="mailto:tyler@greaterscott.com">tyler at greaterscott dot com</a>. Kick ass!

## FAQs

### What if part of my training is done in a different unit?

Ah, a true connoisseur of units, I see. This is well supported using performance metadata and the **Unit** key. For example, lets say we needed to add some kettlebell swings to our session from above.
```
@ Dec 25 2019 8:03am

# Week: 2
# Day: 4
# Burritos: 3
# Unit: lbs

* Definitely should have stopped at two burritos.

Squat:
  135 10r 2s
  225 5r
  315 5r
  405 2r
    * Had to stop at two reps to run to the bathroom.
  455
  500 3s
    * Surprisingly easy singles given that episode at 405.
    * Consider going up in weight next week.

Kettlebell swings: 24 10r 4s
  # Unit: kg
```
If typing so many characters is not your thing, you may also use **u**, **U**, or **unit** (saves on a shift key) as well.

### Does Traindown support non-weight based training?

Sadly, we can't lift weights all the time. Sometimes we wind up doing things like cardio or maybe even compete in sports that don't involve weights. **Traindown** is flexible enough to help you record all kinds of training. Let's see an example—this one is for a sprinter.
```
@ 12/25/19

Warmup drills: 3s

'60m sprints: 7.12 7.03 7.01
  # Unit: seconds

Jog: 1600
  # Unit: meters
```
Units in **Traindown** can really be _anything_, so whatever it is you are doing, you likely can record it with **Traindown**.

### Does Traindown support bodyweight based training?

Totally. Bodyweight movements are a cornerstone of the well rounded athlete. In order to record a bodyweight movement, you just subsitute what would normally be the load of a performance with **bw**. This should be used in conjunction with your weight noted in a metadata variable, e.g. `# bw: 160`.
```
@ 2021-03-25

# bw: 160
# unit: lbs

Dips: bw 10r 5s
```
If you are using additional weight via a belt or perhaps biting down on a dumbell, you can note that using **bw+**.
```
Dips:
  bw 10r
  bw+25 10r
  bw+50 10r
  bw 20r
```
Variations accepted are **BW**, **Bw**, and **bW** (for the true oddballs out there).

### What is the preferred file extension for Traindown? What about encoding? Or language?

This will likely depend on your analysis tool of choice, but the standard file extension is **.traindown**—e.g., _20190101.traindown_. You could just as easily save your files as plain text files, if you prefer.

All **Traindown** files should be encoded in UTF-8.

**Traindown** supports unicode so any language is allowed for use. The caveat being certain keywords like the ones used for bodyweight, unit, sets, reps, and failures will be universal across languages unless the particular software you are using specifies those as configurable.


### My Traindown file isn't being read correctly. Is there anything I can do?

One thing not discussed above is the importance of line breaks for many of the operators in **Traindown**.

For example, if we are writing a note like this
```
* My cool note
```
we know the end of the note to be the end of the line on which the note appears. If we do something like this
```
* My cool note * Here is what I would hope is another note
```
we get back only a single note with the entire content being everything on the life _after_ the _first_ star. This is because **Traindown** (or rather the software implementing [the spec](/spec)) does not know you intended to have two notes because a note is defined simply as "a line beginning with * and ending with a terminating codepoint".

The last two words are crucial. Lucky for us, there is the **;** operator that is a terminating codepoint. You can use the ; in any place you would otherwise use a carriage return (or whatever your line endings are on your system).

Looping back to the example, we could correct the two notes, one line issue like so
```
* My cool note; * Here is what I would hope is another note
```
This tool can be applied to anything in the **Traindown** file that would otherwise need a new line to parse. You could conceivably have a **Traindown** file be one long line delimited by ;. Why? I have no idea but it is possible!
