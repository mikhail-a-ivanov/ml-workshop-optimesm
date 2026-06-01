# Detex—an AI-based detector of extreme events

## When running on Alvis, make sure that the `HDF5_USE_FILE_LOCKING` variable is set to false (otherwise some files cannot be read or written!):

`export HDF5_USE_FILE_LOCKING=FALSE`

The command above can be placed in the `~/.basrhc` file or in the slurm scripts

## Running `detex` training with a container

Use the following command to run `detex` training with a container (assuming `container.sif` is in the current directory):

`apptainer exec container.sif python training.py -c training-config.toml -d cuda -n 16`

`-c` flag is used to specify the path to the TOML configuration file, `-d` flag specifies the device (`cuda` by default). If several CUDA GPUs are present, use `cuda:GPU_id`, where `GPU_id` starts with 0. `-n` flag specifies the number of CPU cores used for the data loading (it is recommended to use as many CPU cores as possible).

The container with a ready-to-use Python environment (including pytorch, jupyter, iris, xarray, ncview and cdo) can be found here:

`/mimer/NOBACKUP/groups/mlhighres/projects/downscaling/containers/mldown.sif`

When running the applications with the container, it can be convenient to link it to your home directory:

`ln -s /mimer/NOBACKUP/groups/mlhighres/projects/downscaling/containers/mldown.sif ~/mldown.sif`

Then, one can run the following without the need to include the full path:

`apptainer exec ~/mldown.sif python training.py -c training-config.toml -d cuda -n 16`

## Inference and evaluation

The training produces only the model weights, which can be later used by an `inference.py` script to produce the prediction netCDF files:

`apptainer exec ~/mldown.sif python inference.py -c inference-config.toml -d cuda -n 16`

The predictions can be assesed with the `evaluation.py` script that does not require GPU to run:

`apptainer exec ~/mldown.sif python evaluation.py -c evaluation-config.toml -n 16`

The results of the inference and evaluation can be viewed with `ncview`, which can be called from the container too:

`apptainer exec ~/mldown.sif ncview cube.nc`

## Slurm scripts

The slurm script `detex-launch.sh` is updated and ready to run on alvis with the `mldown.sif` container (make sure to link the container to your home directory or use the commented `apptainer` line instead). The launch script chains the training, inference and evaluation in a single job, so make sure that the configuration files are compatible, e.g. the model hyperparameters are consistent and the file paths are correct.

One can, of course, run training, inference and evaluation separately, by commenting out or deleting unnecessary run commands.

To run the launch script use `sbatch detex-launch.sh`. For more info see https://slurm.schedmd.com/quickstart.html.

## Learning rate finder

`lr_finder.ipynb` jupyter notebook contains a script that helps finding an optimal learning rate for the specified hyperparameters. This is done by running several training iterations with linearly increasing learning rate, and measuring the gradient of the loss function. The output of `lr_finder` can be used as a learning rate parameter in the `training-config.toml`. To run the jupyter notebook, start a jupyter server on Alvis within the following container: `/mimer/NOBACKUP/groups/mlhighres/projects/downscaling/containers/mldown-lr-finder.sif`. 
