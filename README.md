# Installing Tensorflow on Prince   
Try to follow the sequence of  commands as it is.  Do  not do module load anaconda, doesn't seems to  work for me. 

## Using Tunnel   (Outside NYU)  
ssh into gw.hpc.nyu.edu first, or use cisco vpn (allows file  transfer)
```
ssh pp1953@gw.hpc.nyu.edu 
```

## Linking up the prince storage to local
*very helpful, if  you  are not a big  vim fan*
```
sshfs -p 22 pp1953@prince.hpc.nyu.edu:/scratch/pp1953 ~/project
```

## Requesting GPUs :   
```
srun -c4 -t24:00:00 --mem=30000 --gres=gpu:p40:1 --pty /bin/bash
```

## Remove any preloaded modules  
```
module purge
```

## Delete a conda environment   
(if something goes wrong, start fresh) 
Clean the  environment :  
```
conda deactivtae 
conda clean -i -l -t -p -s   
```
Conda seems to have a bug, doesn't  delete the local files, will eventually  exhaust all the storage space
```
conda remove --name env_name --all
```

## Load  Cuda/Cudd modules  (strictly follow the order)

*(tensorflow==1.7.0)*  
```
module load cudnn/9.0v7.0.5  
module load cuda/9.0.176   
```
*(tensorflow==1.11.0)*
```
module load cudnn/9.0v7.3.0.29 
module load cuda/9.0.176
```
## Create environment  
```
conda create --name bert python=3.5  
conda activate bert
```


## Install  libraries (Use pip not conda!!)
Conda has some sorts of bug. It installs cuda and cudnn as well, which is already installed on prince and in contradictions to the requirements of tensorflow.
```
pip install h5py nltk pyhocon scipy sklearn
pip install tensorflow-gpu==1.7.0  
```
or
```
pip install tensorflow-gpu==1.11.0
```

## Installing Pytorch 
```
conda  env create -f req.yml 
```

req.yml : 
```
 name: env_name
 channels:
     - pytorch
 dependencies:
     - python=3.6
     - pytorch=1.0.0
     - torchvision=0.2.1
     - numpy=1.14.5
     - scikit-learn=0.19.1
     - h5py
     - scipy
     - pip:
         - tensorboard
```

## Common problems

* Conda install reverts the changes of loading cuda/cudnn  
use : ```conda uninstall tensorflow-gpu cudatoolkit cudnn ```
* Tensorflow was compiled with diffent version of  cudnn  and currently is a different version is loaded. Just load the correct/earlier version of cudnn by which *tensorflow-gpu* was  installed
* Tensorflow is cuda/cudnn installed in not compatiblet to use gpu.  ```pip uninstall tensorflow-gpu``` or  possibly delete the  whole  environment and follow the  above procedure.


## Summary
```
srun -c4 -t100:00:00 --mem=50000 --gres=gpu:p40:1 --pty /bin/bash
cd /scratch/model2/codes/
module load cudnn/9.0v7.3.0.29 
module load cuda/9.0.176
conda activate bert
```
