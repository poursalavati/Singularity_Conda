# Singularity_Conda
How to clone a conda env as a Singularity image


# Clean Conda env (recomended)

Start with a clean conda env and install all desired tools:
```
Conda create -n <env> -c <channel1> -c <channel2> conda-pack <tool1> <tool2> 
```

# Pack your environment

cd to a new folder:
```
conda-pack -n <env> -o packed_env.tar.gz
```

# Make a def file

Create a env.def file:
```
From: continuumio/miniconda3

%files
    packed_env.tar.gz /packed_env.tar.gz
%post
    tar xvzf /packed_env.tar.gz -C /opt/conda
    rm /packed_env.tar.gz
```

OR, if you need java or other env variable, use %environment:
```
Bootstrap: docker
From: continuumio/miniconda3

%files
    packed_env.tar.gz /packed_env.tar.gz

%environment
    export JAVA_HOME=/opt/conda/lib/jvm
    export JAVA_LD_LIBRARY_PATH=/opt/conda/lib/jvm/lib/server

%post
    tar xvzf /packed_env.tar.gz -C /opt/conda
    rm /packed_env.tar.gz
```
# Create a Singularity image
```
sudo singularity build conda.sif env.def
```
