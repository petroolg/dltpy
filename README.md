## Original DLTPy README

<details closed>
<summary>HERE</summary>
<br>
# DLTPy
Deep Learning Type Inference of Python Function Signatures using their Natural Language Context

DLTPy makes type predictions based on comments, on the semantic elements of the function name and argument names,
and on the semantic elements of identifiers in the return expressions.  Using the natural language of these 
different elements, we have trained a classifier that predicts types. We use a recurrent neural network (RNN)
with a Long Short-Term Memory (LSTM) architecture.

_Read our [paper](https://arxiv.org/abs/1912.00680) for the full details._

## Components

![DLTPy flow](https://user-images.githubusercontent.com/15815208/67791371-98049480-fa77-11e9-95ed-bb94e7b06eeb.png)

### `preprocessing/` Preprocessing Pipeline (a-d)
Downloads projects, extracts comments and typesm and gives a csv file per project containing all functions.

Start using:
``` bash
$ python preprocessing/pipeline.py
```
Optional arguments:
```
  -h, --help            show this help message and exit
  --projects_file PROJECTS_FILE
                        json file containing GitHub projects
  --limit LIMIT         limit the number of projects for which the pipeline
                        should run
  --jobs JOBS           number of jobs to use for pipeline.
  --output_dir OUTPUT_DIR
                        output dir for the pipeline
  --start START         start position within projects list
```

### `input-preparation/` Input Preparation (e-f)
`input-preparation/generate_df.py` can be used to combine all the separate csv files per project into one big file
while applying filtering.

`input-preparation/df_to_vec.py` can be used to convert this generated csv to vectors.

`input-preparation/embedder.py` can be used to train word embeddings for `input-preparation/df_to_vec.py`.

### `learning/` Learning (g)
The different RNN models we evaluated can be found in `learning/learn.py`.

## Testing
``` bash
$ pytest
```

## Credits
- [Casper Boone](https://github.com/casperboone)
- [Niels de Bruin](https://github.com/nielsdebruin)
- [Arjan Langerak](https://github.com/alangerak)
- [Fabian Stelmach](https://github.com/fabianstelmach)
- [All contributors](../../contributors)

## License
The MIT License (MIT). Please see the [license file](LICENSE) for more information.
</details>

## How to actually make it work?

### I. Conda or virtual environment

There are only 2 datails you should know about: 
1) The environment must be based on Python 3.7 (weird incompatibilities with some lib...)
2) don't forget to install the project's requirements.
    ```shell
      pip install -r requirements.txt
    ```
    
### II. The execution itself

1) Make sure that you can access github.com over SSH. 
  Execute the following command:
    ```shell
      ssh -T git@github.com
    ```
    If you see the following message, you're good to go.
  
    ```
      > Hi username! You've successfully authenticated, but GitHub does not
      > provide shell access.
    ```
2) Add project's root dir to PYTHONPATH system variable. 
   From the project's root directory execute:
    ```shell
      export PYTHONPATH=$(pwd)
    ```

3) Create necessary dirs for intermediate files.
    From the project's root directory execute:
    ```shell
      mkdir output/data
      mkdir output/ml_inputs
      mkdir output/models
      mkdir output/vectors
    ```
4) After that you should be ready to execute the whole pipeline.
    From the root directory you can execute the following commands one by one:
    ```shell
      # Downloads projects, extracts comments and typesm and gives a csv file per project containing all functions.
      python preprocessing/pipeline --output_dir $PYTHONPATH/output/data --limit 100

      # can be used to combine all the separate csv files per project into one big file while applying filtering.
      python input-preparation/generate_df.py

      # can be used to train word embeddings
      # for input-preparation/df_to_vec.py [the next step]
      python input-preparation/embedder.py

      # can be used to convert this generated csv to vectors.
      python input-preparation/df_to_vec.py

      # NN training
      python learning/learn.py
    ```












