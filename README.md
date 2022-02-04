# Gradientless, integrity-validating molecular optimization framework
A transformer-based molecular optimization framework. Optimize any molecule using an arbitrary scoring function while maintaining its integrity and purpose.


## Framework overview

The optimization framework consists of three elements - attention loss, latent space distance, and a user-defined scoring function.

 - Attention loss helps us in maintaining integrity of the molecule. Chemformer's attention heads penalize modifications which lead to an invalid molecule.
 - Latent space distance helps us in sticking to the purpose of the original molecule - it measures the semantic difference between our modified molecule and the sourceo ne.
 - User-defined objective can by any function which accepts SMILES and returns a scalar. 

## Architecture
![Framework overview](https://i.imgur.com/uYNyr13.png)
 - Input: SMILES embedded using a Chemformer (`Chemformer embedding.ipynb`)
 - Candidate generation: handles reranking of the candidates, chooses the best of them, and runs them through the benchmark. (`Optimize molecule.Sampler` - currently only a greedy sampler is supported, feel free to add more!)
 - Attention loss: prediction of the most fitting modifications (`Optimize molecule.MolecularOptimizer.get_transformer_ll`)
 - Latent space distance: cosine distance between embeddings of the source molecule and the candidate. Currently it's intertwined with scoring function, sorry :/ 
 - Scoring: whatever you want it to be, get crazy. `Optimize molecule` notebook shows two examples.
    - A chance that a molecule will be a Mu-receptor antagonist (`score_fn`), based on a neural network predictions (thanks to dr Sabina Podlewska for providing data)
    - Possibility of DILI-related injury, based on an XGBoost algorithm (gradient-less! Data from CAMDA challenge 2020)
  
## Results
Sample optimizations of a molecule to be a better Mu-receptor antagonist (prediction format: `[binding probability, 1-antagonist probability]`)
![Sample optimization](https://i.imgur.com/QYoxbhJ.png)

DILI chance optimization (don't pay too much attention to this one, we had a poor classifier)
![Sample optimization](https://i.imgur.com/IOQiAuG.png)

## Scoring functions AUC
![AUC](https://i.imgur.com/YWTwKwp.png)
