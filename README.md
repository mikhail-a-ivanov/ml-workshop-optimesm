# ML-downscaling workshop for the OptimESM General Assembly

During this workshop we are going to train a deep learning climate downscaling model based on a UNet architecture using SSP3.7-0 data for the 2015-2100 period centered around Denmark. We are going to use `EC-Earth3-Veg` global climate model data, interpolated to 1° resolution, as the low resolution input data (**predictors**), and `HCLIM43-ALADIN` regional climate model data, interpolated to 0.1° resolution, as the high resolution output data (**target**). 

The easiest way to train the model, run the predictions and the analysis is through the Google Colab service (a Google account is needed, but the service itself is free), as it provides the Python environment with a GPU access for a limited time. However, it is possible to run the workshop locally, albeit with more steps  to setup the environment.

Irrespective of the selected approach, the first step is to download the `ml_workshop_optimesm.ipynb` jupyter notebook.

## Google Colab setup

Go to https://colab.research.google.com/ and upload `ml_workshop_optimesm.ipynb` jupyter notebook there. Press the `Upload` button, then `Browse`, and select the notebook. After uploading the notebook, you can continue following the instructions in the notebook.

## Local setup

If you don't have a package/environment manager for Python (e.g. `conda`, `mamba`) it is advised to get one and create a separate Python environment for this project.

Follow the installation instructions for `mamba` here: https://github.com/conda-forge/miniforge

After the installation is done, set up the environment:

`mamba create -n ml-workshop xarray matplotlib jupyter pandas numpy cartopy palettable`

Activate the environment and start the jupyter lab:

`mamba activate ml-workshop`

`jupyter-lab`
