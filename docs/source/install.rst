.. _install:

======================
Installation
======================

Each tools can be use in separate virtual environments.
The single tool installation is provided below and in each tool page.
Please ensure that you use the appropriate GCC version corresponding to the GDAL version required by the tool.

SunMapGeneration installation
==============================

To install SunMapGeneration, please launch the following commands :

.. code-block:: console

    conda create -n sunmap_env -c conda-forge python=3.12 numpy=1.26.4 gdal=3.9.3 rasterio timezonefinder pytz ephem pyyaml click geopandas kiwisolver matplotlib fiona scipy pyscaffold tqdm
    conda activate sunmap_env
    pip install georastertools "eolabtools[SunMapGen]"


NightOsmRegistration installation
=================================

To install NightOsmRegistration, please launch the following commands :

.. code-block:: console

    conda create -n nightosm_env -c conda-forge python=3.12 gdal=3.11.5 pyrosm rasterio numpy pyogrio geopandas opencv matplotlib pyyaml fiona scikit-image
    conda activate nightosm_env
    pip install "eolabtools[NightOsmReg]"


DetectionOrientationCulture installation
=================================

To install DetectionOrientationCulture, please launch the following commands :

.. code-block:: console

    conda create -n orcult_env -c conda-forge python=3.11 numpy=1.26.4 gdal=3.11.5 pandas=1.5.3 geopandas=0.14.4 rasterio fiona scikit-learn opencv pyyaml
    conda activate orcult_env
    pip install opencv-contrib-python "eolabtools[DetecOrCult]"
