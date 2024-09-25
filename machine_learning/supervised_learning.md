Reinforcement learning works like dog training: “good” behavior is rewarded so that the dog does more of it.

## Learning
Need two things:
	1. Example
	2. Label

For example, to identify different dog breeds from bark sounds, we need a sample of a .wav file containing dog bark sounds, and breed labels corresponding to each file.

Labels can be either numerical or categorical:
- **Numerical labels:** These are just a number, as in the case of the temperature-to-lemonade converter.
- **Categorical labels:** These represent a category in a pre-defined set, as in the case of the dog breed detector.

## Phases of Supervised Learning:
### Phase 1: Training
Feed the labeled examples to an algorithm that’s designed to spot patterns.
This is called the **training phase**, because the algorithm is looking at the examples repeatedly and learning to recognize those patterns.

### Phase 2: Prediction
Now we show new examples to the algorithm, which classifies the examples according to the labels we fed.


