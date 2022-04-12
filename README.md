![Narcissus-Caravaggio](https://user-images.githubusercontent.com/77789132/162662050-11494b6e-a4fd-486b-80ef-d895654e4a8d.jpg)

# Narcissus Clean-label Backdoor Attack

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

Narcissus clean-label backdoor attack provides an affirmative answer to whether backdoor attacks can present real threats: as they normally require label manipulations or strong accessibility to non-target class samples. This work demonstrates a simple yet powerful attack with access to only the target class with minimum assumptions on the attacker's knowledge and capability.

In our paper, we show inserting maliciously-crafted Narcissus poisoned examples totaling less than 0.5\% of the target-class data size or 0.05\% of the training set size, we can manipulate a model trained on the poisoned dataset to classify test examples from arbitrary classes into the target class when the examples are patched with a backdoor trigger; at the same time, the trained model still maintains good accuracy on typical test examples without the trigger as if it were trained on a clean dataset. 

Narcissus backdoor attack is highly effective across datasets and models, even when the trigger is injected into the physical world. Most surprisingly, our attack can evade the latest state-of-the-art defenses in their vanilla form, or after a simple twist, we can adapt to the downstream defenses. We study the cause of the intriguing effectiveness and find that because the trigger synthesized by our attack contains features as persistent as the original semantic features of the target class, any attempt to remove such triggers would inevitably hurt the model accuracy first.

# Features
- Clean label backdoor attack
- Low poison rate (can be less than 0.05\%)
- All-to-one attack
- Only require target class data
- Pyhsical world attack
- Work with the case that models are trained from scratch

# Requirements
Python >= 3.6

PyTorch >= 1.10.1

TorchVisison >= 0.11.2

OpenCV >= 4.5.3

# Usage

Use the Narcissus.ipynb notebook for a quick start of our NARCISSUS backdoor attack. The default attack and defense state both use Resnet-18 as the model, CIFAR-10 as the dataset, and the default attack poisoning rate is 0.5% In-class/0.05% overall.

There are a several of optional arguments in the ```Narcissus.ipynb```:

- ```lab```: The number of the target label
- ```l_inf_r``` : Radius of the L-inf ball which constraint the attack stealthiness.
- ```surrogate_model```, ```generating_model``` : Define the model used to generate the trigger. In the usual case should be set to the same model.
- ```surrogate_epochs``` : The number of epochs for surrogate model training.
- ```warmup_round``` : The number of epochs for poison warmup trainging.
- ```gen_round``` : The number of epoches for poison generation.
- ```patch_mode``` : Users can change this parameter to ```change```, entering the patch trigger mode. 

# Function block
By importing the ```narcissus_func.py``` file, users can quickly deploy a Narcissus backdoor attack into their own test environment by ```narcissus_gen()``` fucntion. There are 2 parameters in this function:
- ```dataset_path``` : The dataset folder for CIFAR10 and TinyImageNet
- ```lab```: The number of the target label

This function will return a (1x3x32x32) NumPy array, which contains a Narcissus backdoor trigger.

# Overall Workfolw:
![Narcissus](https://user-images.githubusercontent.com/64983135/162639447-05d02a49-9668-49a0-8d91-c82b952a801e.png)
The workflow of the Narcissus attack consists of four functional parts (<a href="https://www.cs.columbia.edu/CAVE/databases/pubfig/">PubFig</a> as an example):

- Step 1: Poi-warm-up: acquiring a surrogate model from a POOD-data-pre-trained model with only access to the target class samples. 
- Step 2: Trigger-Generation: deploying the surrogate model after the poi-warm-up as a feature extractor to synthesize the inward-pointing noise based on the target class samples; 
- Step 3: Trigger Insertion: utilizing the Narcissus trigger and poisoning a small amount of the target class sample; 
- Step 4: Test Query Manipulation: magnifying the Narcissus trigger and manipulating the test results.

