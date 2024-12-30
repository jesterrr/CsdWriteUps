
# CTF

Santa got lost on his way to the largest hacking conference. He was out cycling and got lost and all we have is this picture of him can you help us find him?

Round to 3 decimal places in cords. IE csd{10.111,-23,400} DO NOT TRUNCATE, leave all zeros and negative

![image](https://bucket.ics.red/foto_no_exif.jpg)

## Hint 1

What cycling event is there?

## Hint 2

What is the largest hacking conference?

# Solution

The largest hacking conference is DEF CON which is in Nevada, which gives us a big hint.

After that, we can use google lens to get close to the place the picture was taken.

![image](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day20/day20p1.png)

Red Rock Canyon is indeed in Nevada, so we know the approximate location. Now, we can go to google maps and look for the mountain in that area using street view.

Now, we can look around that area, to find anything that would help us. Google Maps has Scenic spot filter for places that could be a tourist attraction or a place for photos, so let's use that filter by just typing it in.

![image](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day20/day20p2.png)

It's logical that there's a high chance of asphalt being close to the road, so let's look at the ones really close to the roads. Since there's a lot of them, we can take a look at the ones with the camera Icons first, since the place on the picture doesn't really look like a hiking place.

First, (Red rock canyon marker (north ) )

![image](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day20/day20p3.png)

this looks nothing like the place we're looking for.

Second (Red Rock Canyon NCA Scenic Drive)

![image](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day20/day20p4.png)

well, we did find the mountains on the picture, but this isn't the location, since it's too far.

Third (Red Rock Canyon Overlook)

![image](https://github.com/jester-var/CsdWriteUps/blob/main/imgs/day20/day20p5.png)

We've found the place on the picture! now let's get the coordinates!

The cords that we get are `36.11711251168812, -115.44500088773073` but we have to round the m to 3 decimal places, which would be `36.117,-115.445`
flag: `csd{36.117,-115.445}`
