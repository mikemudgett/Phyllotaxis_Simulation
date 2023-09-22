# Simplified Phyllotaxis Simulation

&emsp;Many flowering plants, including _Arabidopsis thaliana_, generate new organs such as leaves or flowers in a specified pattern known as phyllotaxis. In many cases, subsequent organs are initiated in a "Fibonacci spiral" wherein if you measure the angle between the newest organ and the one that formed just before it, you will find that it is always nearly the "golden angle" of 137.5 degrees. See more [here](https://www.science.smith.edu/phyllo/About/index.html) and [here](https://goldenratiomyth.weebly.com/phyllotaxis-the-fibonacci-sequence-in-nature.html#:~:text=This%20effect%20is%20the%20result,maximal%20packing%20in%20horizontal%20space.).  
&emsp;Wilhelm Hofmeister proposed that new organs form in the "least crowded" space on the meristem, or the farthest point from all existing organs. This became known as ["Hofmeister's Rule"](https://www.science.smith.edu/phyllo/About/math.html).  
&emsp;Many people have generated models to describe phyllotaxis and explain how plants are able to coordinate just an intricate system. Based on years of research, we have found that the plant hormone auxin plays a large role in flower initiation. Auxin is transported around the cells in the meristem (region of dividing stem cells that give rise to new organs) and new organs form at auxin maxima, or areas where the cellular auxin levels get really high. Some examples of different modeling approaches: [1](https://www.pnas.org/doi/10.1073/pnas.0510457103) [2](https://www.sciencedirect.com/science/article/pii/S0022519322002521?dgcid=rss_sd_all) [3](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007044) [4](https://www.pnas.org/doi/full/10.1073/pnas.0509839103) 

&emsp;I've spent a lot of time thinking about how auxin transport and gradients could lead to such a distint pattern formation so I wanted to try modeling myself, although I didn't have the time or skills to do something as complicated as what has been published. My thinking was that if the organized phyllotactic pattern is so ubiquitous and robust, the underlying principles should be relatively basic.  
&emsp;In general, the _Arabidopsis thaliana_ inflorescence meristem (the place new flowers form) can be thought of as a dome. However, new flowers only form in the "peripheral zone" around the edges, not the central zone. Also, it seems like the auxin transport mainly takes place in the topmost layer of cells of the peripheral zone (also called the L1 layer). This reduces our area of interest to a single-layer band of cells circling the meristem. For this model, I simplified it even more to just be a circle. So flowers are forming out of this circle and each cell has a certain amount of auxin in it.  
&emsp;We know that new flowers produce auxin. It is thought that the auxin from the new flowers gets pushed around the circle and it sort of "pools up" in the locations farthest from the flowers and thus generates maxima. One issue with this, however, is that when auxin is being transported away from a flower, the auxin concentration right next to the flower is going to get very high. This has lead to the development of theories about "inhibitory zones" or the idea that new flower formation is highly discouraged in areas directly adjacent to new flowers. But if new flowers somehow inhibit initiation of other flowers right near them, shouldn't that be enough to trigger flower formation at the farthest point from all relevant flowers? Why do we need the added step of funneling auxin long distances to pool up at the next organ primordium? Are there two molecules here, the fast-moving and flower-promoting auxin and the slow-moving but flower inhibiting "something else"? Some researchers may have a better idea or explanation about what is really going on here but I just wanted to see if inhibitory fields alone could generate the angles we observe in nature, in my simple system.

### The Simulation

&emsp;For this simulation, we model the meristem peripheral zone as a circle. The circle is discretized into equally spaced points along its perimeter, usually something like 1000 points. Locations along the perimeter are denoted in radians (0-2pi). A location for an initial flower is given, it doesn't really matter where. Because this is a flower, it is given an inhibitory field around it. Thus each point around the circle perimeter is assumed to have some "inhibitory potential" of sorts. I just call this the "value" of each point. The location of the new flower has the highest value, and values decrease exponentially away from that point. From this point, the system continues, following a set of three rules:

1. After one time step (plastochron) elapses, a new flower is initiated at the global minimum of the circle (the point on the perimeter with the lowest "value")

2. New flowers immediately increase the value at their specific location by 1. They also increase the values of all other points on the circle by means of an exponentially decaying function based on distance and time.

3. To simulate the older flowers being pushed away and contributing less auxin, the contribution from older flowers decays over time.

Let's see it in action:
One flower is pre-loaded at position "0". The black circle is a reference and the red represents the inhibitory potential of each point along the perimeter. There is a spike where the new flower formed.  
t=0:  
<img src="/Pictures/t0.png" width="30%">  
The next flower pops up on the exact opposite side of the circle because that is where the inhibition potential is the lowest.  
t=1:  
<img src="/Pictures/t1.png" width="30%">  
At the next time step, the new flower will not be directly up or down, but shifted a little since the first flower's impact is a little weaker. This continues for as many cycles as is set.
t=2: &emsp; &emsp; &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; t=3:  
<img src="/Pictures/t2.png" width="30%">  <img src="/Pictures/t3.png" width="30%">  




















