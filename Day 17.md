# CTF
Santa cargo plane is running into issues can mechanical problems. Can you figure out his plane model. Please include the variant in the answer.

Ex. csd{a10}

![pic1](https://bucket.ics.red/pic1.webp)

![pic2](https://bucket.ics.red/pic2.webp)
# Solution

This was an easy one. I searched up Cargo aircraft since the CTF mentioned a cargo and got to this [page](https://en.wikipedia.org/wiki/Cargo_aircraft)

While reading it I found:
```
But it was the introduction of the turboprop that allowed the class to mature, and even one of its earliest examples, the C-130 Hercules, in the 21st century as the Lockheed Martin C-130J, is still the yardstick against which newer military transport aircraft designs are measured. Although larger, smaller and faster designs have been proposed for many years, the C-130 continues to improve at a rate that keeps it in production.
```

This paragraph mentions Lockheed Martin C-130J, So I searched up it on Youtube to see what it looks like from inside.

This is what I found:
![pic3]([https://github.com/jester-var/CsdWriteUps/blob/main/d17.png](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day17/d17.png))

This picture of cockpit matches the second picture, so the aircraft We're looking for is Lockheed Martin C-130J (C130J)

flag: `csd{c130j}`
