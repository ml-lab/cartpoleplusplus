# cartpole ++

implementing a non trivial 3d cartpole [gym](https://gym.openai.com/)
environment using [bullet physics](http://bulletphysics.org/)
trained with a [dqn](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf)
from [keras-rl](https://github.com/matthiasplappert/keras-rl)

see [the blog post](http://matpalm.com) for loads more info...

```
# some random things i did...
sudo apt-get install libhdf5-dev
virtualenv venv --system-site-packages
. venv/bin/activate
pip install keras numpy h5py 
pip install <whatever_tensorflow_wheel_file>
export PYTHONPATH=$PYTHONPATH:$HOME/dev/keras-rl
```

## before training

(click through for video)

[![link](https://img.youtube.com/vi/buSAT-3Q8Zs/0.jpg)](https://www.youtube.com/watch?v=buSAT-3Q8Zs)

```
# some sanity checks...

# no initial push and taking no action (action=0) results in episode timeout of 200 steps
$ ./random_action_agent.py --initial-force=0 --actions="0" --num-eval=100 | ./deciles.py 
[ 200.  200.  200.  200.  200.  200.  200.  200.  200.  200.  200.]

# no initial push and random actions knocks pole over
$ ./random_action_agent.py --initial-force=0 --actions="0,1,2,3,4" --num-eval=100 | ./deciles.py
[ 16.   22.9  26.   28.   31.6  35.   37.4  42.3  48.4  56.1  79. ]

# initial push and no action knocks pole over
$ ./random_action_agent.py --initial-force=55 --actions="0" --num-eval=100 | ./deciles.py
[  6.    7.    7.    8.    8.6   9.   11.   12.3  15.   21.   39. ]

# initial push and random action knocks pole over
$ ./random_action_agent.py --initial-force=55 --actions="0,1,2,3,4" --num-eval=100 | ./deciles.py 
[  3.    5.9   7.    7.7   8.    9.   10.   11.   13.   15.   32. ]
```

## training

```
$ python ./dqn_bullet_cartpole.py \
 --num-train=2000000 --num-eval=0 \
 --save-file=ckpt.h5
```

## after training

by numbers...

```
$ python ./dqn_bullet_cartpole.py \
 --load-file=ckpt.h5 \
 --num-train=0 --num-eval=100 \
 | grep ^Episode | sed -es/.*steps:// | ./deciles.py 
[   5.    35.5   49.8   63.4   79.   104.5  122.   162.6  184.   200.   200. ]
```

visually (click through for video)

[![link](https://img.youtube.com/vi/zteyMIvhn1U/0.jpg)](https://www.youtube.com/watch?v=zteyMIvhn1U)

```
$ python ./dqn_bullet_cartpole.py \
 --gui=True --delay=0.005 \
 --load-file=run11_50.weights.2.h5 \
 --num-train=0 --num-eval=100
```
