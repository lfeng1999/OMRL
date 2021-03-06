# Offline Meta Reinforcement Learning

Code for the paper: ["Offline Meta Learning of Exploration"](https://arxiv.org/abs/2008.02598) - Ron Dorfman and Aviv Tamar.  

```
@article{dorfman2020offline,
  title={Offline Meta Reinforcement Learning},
  author={Dorfman, Ron and Tamar, Aviv},
  journal={arXiv preprint arXiv:2008.02598},
  year={2020}
}
```

### Requirements ### 
All requirements are specified in ``requirements.txt``. \
We also provide a yaml file: ``omrl.yml``. Run: ``conda env create -f omrl.yml`` and activate the env with ``conda activate omrl``.  
For the MuJoCo-based experiments (``Half-Cheetah-Vel`` and ``Ant-Semi-circle`` you will need a MuJoCo license).  

## Offline Setting ##
### Data collection ###
The main script for data collecion is ``train_single_agent.py``.  
Configuration files are in ``data_collection_config``. All training parameters can be set from within the files, or by passing command line arguments.  

Run:  
``python train_single_agent.py --env-type X --seed Y``  

where X is a domain (e.g., ``gridworld``, ``point_robot_sparse``, ``cheetah_vel``, ``ant_semicircle_sparse``) and Y is some integer (e.g. ``73``).  
This will train a standard RL agent (implemented in ``learner.py``) to solve a single task. Different seeds correspond to different tasks (from the same task distribution).  


Training data will be provided by email request: ``rondorfman2@gmail.com``.  
You can also create the data on you own (seeds 73-152 for the point robot and for Ant, and seeds 73-172 for Cheetah).   


### VAE training ###
The main script for the VAE training is ``train_vae_offline.py``.  
Configuration files are in ``vae_config``. All training parameters can be set from within the files, or by passing command line arguments.  

Run (for example):  
``python train_vae_offline.py --env-type ant_semicircle_sparse``  

This will train the VAE (implemented in ``models\vae.py``).  

Reward Relabelling (RR) is used when the argument ``--hindsight-relabelling`` is set to ``True``. In this case, before the VAE training, the   
Notice the --hindsight-relabelling argument. When it is set to True, we apply reward relabelling to the collected data
by mixing trajectories from different MDPs.  


### Offline Meta-RL Training ###
The main script for the offline meta-RL training is ``train_agent_offline.py``.  
Configuration files are in ``offline_config``. All training parameters can be set from within the files, or by passing command line arguments.  

Run (for example):  
``python train_offline_agent.py --env-type ant_semicircle_sparse``  

Note the ``--transform-data-bamdp`` argument. When you train you meta-RL agent for the first time, this argument should be set to ``True`` in order to perform State Relabelling. 
That is, after loading the datasets and the trained vae, the datasets will be passed through the encoder to produce the approximate belief. This belief is then concatenated 
to the states in our data to form the hyper-states on which our meta-RL agent is trained. This new dataset (with hyper-states) is also saved locally. If this dataset is available
(e.g., after already running the script), you can change the argument to ``False`` in order to save time.  


## Online Setting ##
For the online training, run: ``python online_training.py --env-type X`` where X is a domain (see Data Collection part above).
Configuration files are in ``online_config``. All training parameters can be set from within the files, or by passing command line arguments.  


## Communication ##
For any questions, please contact Ron Dorfman: ``rondorfman2@gmail.com``





