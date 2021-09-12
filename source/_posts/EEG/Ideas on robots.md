Brain controlled robot learning 

本来在跟朋友聊家政的事情。中国未来人口老龄化严重，需要大量的生产力去照顾这些人，所以需要一定数量的家政机器人。但是现在的机器人基本上只能做非常basic 的事情，最好的Boston Dynamics的狗造价很贵，而且职能也许没那么明确。如何能把人类的想法移植给机器人呢？有没有什么generic BCI interface？

能考虑到的有motion imaginary BCI，我们大脑里面想如何让这个机器人动起来，比如先给机器人3D扫描一下，然后人在一边思考这个机器人动起来的样子，然后让机器人真的按照这种方式动起来，就像sci fi里面的脑控机器人一样。这样机器人也许不需要那么多的训练

当然，要考虑到我们人脑大概率会省略很多的中间过程

好处当然是可以实现这些机器人的个性化需求，不需要大量的training

如果人和机器人用同样的神经网络，也就是cerebral cortex的话，说不定是可以直接translate和移植的，就像我示范一遍，另外一个人就可以有样学样

但是机器人领域里面很多人在研究了，包括用实际的data 去训练



How coarse it will become in order to get it work

https://engineering.cmu.edu/news-events/news/2019/06/20-he-sci-robotics.html

https://www.csail.mit.edu/research/brain-controlled-robots

Controlling of virtual avatar to actual robot, is it possible? Then map it to actual robot 

Then robot learn from it and repeat or adjust their behavior accordingly to a point they don't need assist from human anymore 

Make it a standard 

Difficulties, robots had their own way of study, can they understand human intention? 

Next steps

- Why it's so hard to get a clear EEG signal for controlling of robotic arm? How to improve it?
- Is it possible to control the moving of virtual avatar?
- Is it possible to map the moving of virtual avatar to actual robot? 
- Can this be a standard or generic interface to get it work with any type of robot? 

比如教机械手臂洗盘子，先用3D建模出通用的两个机械臂加机械手（也可以在VR里面模拟），然后让大脑想象打开水龙头，一直手抓盘子，另外一只手洗的动作，让3D中的机械臂动起来，然后把3D中的点的运动转化成关节的动作，比如一个时间序列，每个时间点上是每个关节运动的角度，接着把这一系列角度传给机器，告诉它这是一次成功的洗盘子。之后可以多次模拟训练，提供数据集，用reinforcement learning把这个洗盘子的动作教会给机械臂。

