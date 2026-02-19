# Eolabtools

Eolabtools allows to use various tools for satellite imagery analysis.

## Documentation

Full documentation available at [eolabtools.readthedocs.io](https://eolabtools.readthedocs.io).  

## Detection Orientation Culture

### Installation procedure

To install DetectionOrientationCulture, please launch the following commands :

```bash
conda create -n orcult_env -c conda-forge python=3.11 numpy=1.26.4 gdal=3.11.5 pandas=1.5.3 geopandas=0.14.4 rasterio fiona scikit-learn opencv pyyaml
conda activate orcult_env
pip install opencv-contrib-python "eolabtools[DetecOrCult]"
```

### Usage

To obtain the crop orientation in a shapefile format, please use the following command. The method implemented uses the fld library from openCV.

```bash
detection_orientation_culture --img path/to/image_file_or_directory
                              --type extension_file_type
                              --rpg path/to/rpg_file.shp \
                              --out_shp path/to/output_file.shp \
                              --out_csv path/to/output_file.csv \
                              --nb_cores 12 \
                              --patch_size 10000 \
                              --slope path/to/slope_file.tif \
                              --aspect path/to/aspect_file.tif
```

- The code relies on the fld algorithm to detect the segments in the images from which the orientations of each of the input RPG
plots are calculated.

- To run the code in parallel, select `--nb_cores`>1.

- If the input image(s) is (are) large, it is advisable to define a --patch_size which will be used to perform patch processing
(faster thanks to parallelization).

- The `--slope` and `--aspect` files must be generated beforehand (see Calculating data used in orientation calculations) and
supplied as input.- ``--osm-config`` : Path to the OSM configuration file with tags to keep in binary raster


## Night OSM Registration

This tool performs night visible data registration based on OSM reference.

### Installation procedure

To install NightOsmRegistration, please launch the following commands :

```bash
conda create -n nightosm_env -c conda-forge python=3.12 gdal=3.11.5 pyrosm rasterio numpy pyogrio geopandas opencv matplotlib pyyaml fiona scikit-image
conda activate nightosm_env
pip install "eolabtools[NightOsmReg]"
```

### Using night_osm_image_registration

Use the command ``night_osm_image_registration`` with the following arguments :

```bash
night_osm_image_registration /path_to_input_files/input_file_1.tif [/path_to_input_files/input_file_2.tif] [...]
                             -o /path_to_output_directory/output_directory/
                             --config /path_to_config_directory/config_file_name
                             --osm-config /path_to_config_directory/osm_config_file_name
```

Arguments are the following :

- ``infile`` : Reference input image to compute shift grid

- ``auxfiles`` : Optional list of additional images to shift based on the same grid

- ``-o``, ``--outdir`` : Output files location

- ``--config`` : Path to the main configuration file

- ``--osm-config`` : Path to the OSM configuration file with tags to keep in binary raster

### Using night_osm_vector_registration

Use the command ``night_osm_vector_registration`` with the following arguments :

```bash
night_osm_vector_registration /path_to_points/points.gpkg
                              /path_to_displacement_grid/displacement_grid.tif
                              -o /path_to_output_directory/output_directory/
                              -n output_file_basename
```


Arguments are the following :

- ``invector`` : Path to the input vector file.

- ``grid`` : Path to the displacement grid (band1 : shift along X in pixels, band 2 : shift along Y in pixels).

- ``-o``, ``--outdir`` : Output directory.

- ``-n``, ``--name`` : Basename for the output file.

## Sun Map Generation

SunMapGeneration generates a map per date with the percentage of sun exposure over the time range, as well as a vector file that
gives the transition times from shade to sun (and sun to shade) for each pixel at the first date.
It takes as inputs a Digital Surface Model (DSM) of the area of interest, a range of dates and a time range per date of your choice.
Shadow masks can also be produced at each step.

*Please note that the time range is given in the local time of the area of interest.*

If the area of interest is important, the DSM should be divided into tiles beforehand (typically 1km*1km). The list of tiles is
given as input. The tool will manage the shadow impact on adjacent tiles.

### Installation procedure


To install SunMapGeneration, please launch the following commands :

```bash
conda create -n sunmap_env -c conda-forge python=3.12 numpy=1.26.4 gdal=3.9.3 rasterio timezonefinder pytz ephem pyyaml click geopandas kiwisolver matplotlib fiona scipy pyscaffold tqdm
conda activate sunmap_env
pip install georastertools
pip install "eolabtools[SunMapGen]"
```

### Usage

To launch SunMapGeneration, please use the following command :

```bash
sun_map_generation --digital_surface_model /path_to_input_files/input_files.lst (or .tif)
                   --date YYYY-MM-DD YYYY-MM-DD 3
                   --time HH:MM HH:MM 30
                   --nb_cores 32
                   --occ_changes 4
                   --output_dir /path_to_output_directory/output_directory/
                   --save_temp
                   --save_masks
```

- ``--digital_surface_model`` : Path to the `.lst` file containing the names of the `.tif` files. When only one input file is necessary for the computation, the name `.tif` file can be given.

- ``--tiles_file`` :

- ``--date`` : Date or date range (YYYY-MM-DD) and step (in days). The step value should be strictly positive and default value is 1 day.

- ``--time`` : Time or time range (HH:MM) and step (in minutes). The step value should be strictly positive and default value is 30 minutes.

- ``--occ_changes`` (should be >= 3) : Limit of sun/shade change of a pixel over one day. Default value 4.

- ``--nb_cores`` : To launch parallel processing. Number of processes to be entered.

- ``--output_dir`` : Path to the output directory.

- ``--save_temp`` : To be filled in to obtain the file describing the calculation time per step in the processing chain (`processing_time.csv`).

- ``--save_masks`` : To save shadow masks calculated at each time step


## Tests

The project comes with a suite functional tests. To run them, 
launch the command ``pytest tests``. To run a specific test, execute ``pytest tests/test_toolname.py::test_name``.
The tests perform comparisons between generated files and reference files. 


## Documentation generation

To generate the documentation, run in an environment that contains `sphinx_rtd_theme` and `sphinxcontrib.bibtex` : 

```bash
sphinx-build -b html docs/source docs/build
```

The documentation is generated using the theme "readthedocs".

