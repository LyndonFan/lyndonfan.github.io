---
layout: post
title:  "Bus Stops on an Infinite Grid"
date:   2025-03-26 22:25:00 +0000
tags: Math
---

While having dinner with my flatmates, one of them dropped this problem from work: what's the catchment area of an average bus stop?

It completely [nerd-sniped](https://xkcd.com/356/) me so I immediately asked for some pencil and paper.

# Setup

Let's formalise the problem and give it a more complete setup.

Consider a city over an infinite grid, with grid lines set 800 meters apart. Each grid line is a bus route, with a stop at each intersection and every 200 meters along the axes. Each bus stop can serve residents up to 400 meters away.

![Grid lines 800 meters apart with points on the lines at 200m intervals](/assets/img/20250326_01_setup.png)

The residents in the grid always pick the closest bus stop to go to catch the bus. Let the catchment area of the bus stop be the area of residents it serves.

![3 sample points pointing their their nearest stops](/assets/img/20250326_02_nearest_stop.png)

What is the catchment area of an average[^1] bus stop?

[^1]: I know this is tricky over an infinite grid (in practical and mathematical terms), but we'll resolve this.

## Initial Attempt

Initially, I tried to draw what it would look like just for a single bus line, but it was messy with the larger grid in the way. I used Geogebra to draw how the areas overlapped for a single grid square, which didn't help.

![Circles per each point on a particular square](/assets/img/20250326_03_circles.png)
*gee, I wonder why...*

It took me a while, but I realised these two properties:

1. We don't care that the bus stop's service area is a circle. It was probably a red herring.
2. Bus stops in the same relative position within the square have the same catchment area. For example, the catchment areas of bus stops at intersections all have the same shape.

We also know

3. The catchment areas of all the bus stops cover the entire infinite grid.

I feel like I was onto something. Maybe the grid being infinite can help...

Wait! What if I can assign the catchment areas of each point instead of just having it defined as above?

For example, I can assign all the people in each square to go to the bus stop at the top left (or northwest, if the grid is aligned to the cardinal directions). Then the catchment area of the 7 bus stops on the left side or top side -- but not the bottom left or top right corners -- would be the square itself, therefore the average catchment area of these stops is just the area divided by 7. Since the squares tessellate, this is also the size of the average catchment area.

![The same 3 sample points from earlier, but each going to the closest bus stop to the top left](/assets/img/20250326_04_top_left.png)

But my argument didn't use my arbitrary assignment of the bus stop, so I could have used the original catchment areas. From points 2. and 3. above, their catchment areas must also tessellate the grid. But the catchment areas have the same total area as the original square, so we arrive at the same answer: one-seventh of the square, i.e. `800^2 / 7 = 640000/7`, which is about 91428 m<sup>2</sup> or 9.14 hectares.

## ChatGPT's Attempt

Out of curiosity, I tried to get ChatGPT to solve this with reasoning. The below 3 subsections are presented as is.

---

### My Prompt

consider an infinite grid of 800m x 800m squares. Each line in the grid is a bus stop, with the bus stops at each intersection and at each 200m interval along the grid lines. We define the catchment area of bus stop as the locus of points it is closest to that bus stop than any other bus stop. What is the average catchment area of a bus stop?

### Inner Reasoning

We’re asked about the average catchment area of a bus stop on an infinite grid of 800m x 800m squares. The grid lines themselves are bus stops, and they appear both at the intersections of these grid lines and at 200m intervals along them. This means bus stops are spaced every 200m in both horizontal and vertical directions across the grid. Let me carefully think through how to calculate the average catchment area for these locations.

Okay, the grid is composed of 800m x 800m squares, and the bus stops are placed on the grid lines. There are stops at every 200m along the grid lines, both on the horizontal and vertical lines. So, bus stops aren’t just at the intersections (which are 800m apart); they also occur at 200m intervals between intersections along each grid line. But the key is understanding that the stops are only on the lines themselves, not inside the squares. So, it’s a 200m spaced system along both directions.

The bus stops are arranged along two sets of grid lines spaced 800m apart, with additional stops every 200m along these lines. The stops at intersections appear in both sets, but they’re the same physical stop. We want to calculate the density of stops within a representative 800x800m square. Careful: the stops are not uniformly distributed within the square—they only appear along the boundaries. I’ll need to consider the Voronoi region to compute the average catchment area for each bus stop.

### Final Response

The key is that the average area per bus stop equals the reciprocal of the stop density. Bus stops lie along vertical lines (every 800 m) with stops every 200 m, giving a density of 1⁄(200·800) = 1⁄160,000 per m². The same applies for horizontal lines. However, intersections (which occur every 800 m by 800 m, or 1⁄640,000 per m²) are counted twice if we simply add. So, the unique density is

  1/160,000 + 1/160,000 − 1/640,000 = 7/640,000 stops per m².

Thus, the average catchment area is the reciprocal:

  640,000/7 ≈ 91,429 m².

So, on average, each bus stop serves about 91,429 square meters.

---

### Analysis

So it _did_ get the correct answer, but with a different approach of first calculating the density (points per area) and then inverting it to get the average catchment area (area per point). It briefly mentioned the Voronoi region, which was nice but necessary. The final response goes with the reciprocal of the density approach as well, but the method it calculated density is a bit weird. It is _much_ easier to arrive at the same conclusion by noting there are 7 full bus stops within 800 by 800 square.

Also it decided to use weird slashes for some of the fractions for no reason :shrugs:

## Takeaways

Right off the bat, my friend told me this number would serve as a good reference. As an urban planner[^2], he was investigating how many people are needed to maintain a bus route. With the size of the average catchment area, he can multiply that with the number of stops the bus route needed for the population, and work out the size of the area needed to fit all the people in. Turns out, it was a bit more spacious than he thought.

We also noticed that the project is over a finite area, and there will only be a single bus route. So most assumptions no longer applied (a lot of the trickery I used assumes an infinite grid), but this was still a good starting point.

He also mentioned that his boss's daughter, who studies Maths at university, said this problem might involve trigonometric functions. Similar to what ChatGPT initially thought, this is overkill. Yes, maths sometimes requires brute-forcing an answer, but often it is about changing a perspective or reshaping the question so it becomes easy. Or if I may, trivial.

This also reminded me of [a math question in the anime Assassination Classroom](https://aminoapps.com/c/assassination-classroom/page/blog/atomic-crystalline-lattice-final-examination-showdown-breakdown/r0da_04gIeuxvnEkwDbb40wZQ0KWa0xMrD7). The post is quite light on spoilers (only the answer to the question), so I do recommend watching the real thing afterwards.

[^2]: Corrected from "architect".

## P.S.

As I was writing this, I thought about it more. Drawing the actual Voronoi diagram didn't need any complicated maths either, just some guessing and working with loci. I've put the resulting Geogebra file [here](/code/2025-03-26/bus-stop-catchment-area.ggb).

## P.P.S.

This blog took me over a few weeks to finish, so I got to hear how the math was used.

Or rather, _not_ used since bus routes were thrown away in their approach.

If you'd excuse me, I've got something to throw away as well.

*glass breaks, electronics shatter a few floors down*

---
