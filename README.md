# Spacial-Score
<b>A Comprehensive Topological Indicator for Small Molecule Complexity</b>
<br>(Created by the Waldmann Lab at the Max Planck Institute of Molecular Physiology, Dortmund)

The score is intended for assessing molecular topology of organic molecules and to improve upon the idea of the fraction of stereo and sp<sup>3</sup> carbons. The score is described in our J. Med. Chem. paper: [Spacial Score – A Comprehensive Topological Indicator for Small Molecule Complexity](https://doi.org/10.1021/acs.jmedchem.3c00689).

> [!IMPORTANT]
> **The SPS indicator is now also a part of the RDKit package and available through the [rdkit.Chem.SpacialScore](https://www.rdkit.org/docs/source/rdkit.Chem.SpacialScore.html#module-rdkit.Chem.SpacialScore) module. SPS script is actively mantained as part of the RDKit package, and thus, it is recommended to calculate the SPS through RDKit.**

<img src="https://repository-images.githubusercontent.com/609714000/d6a66509-cce9-4318-8023-17c674cd8cf7" alt="logo" width="500" align="centre"/>

***
### Required Python Packages
The script requires [RDKit package](https://www.rdkit.org/) and [NumPy](https://numpy.org/).
#### Installation via `uv`
1. Install [uv](https://docs.astral.sh/uv/getting-started/installation/) 
2. Create a Python environment.
```
uv venv --seed .venv --python 3.9
source activate .venv/bin/activate
```
You can replace `--python 3.9` with any other Python latest versions.

3. Install all the required packages:
```
uv sync
```
4. Get all the available options from `spacial_score.py` to calculate the scores:
``` 
python spacial_score.py -h
```

#### Installation via `conda`

1. Install [conda](https://www.anaconda.com/docs/getting-started/miniconda/install)

2. To install the required packages through [Conda](https://docs.conda.io/en/latest/miniconda.html), simply use the `environment.yml` file:
```
conda env create -f environment.yml
```
3. The script can be used after activation of the just created conda environment:
```
conda activate my_sps_env
```
4. To display the options of `spacial_score.py`, type:
```
python spacial_score.py -h
```
(Please remember that `spacial_score.py` needs to be in your current directory)
***
### Using the Script Through Command Line
The script can be used directly from a command line, reading either a directly provided SMILES string `(-s)` or a .csv/.tsv file `(-i)`:
```
usage: spacial_score.py [-h] [-s SMILES string] [-i filename.ext] [-o filename.ext] [-t] [-v] [-p]

Script for calculating Spacial Score (SPS) or normalised SPS (nSPS) for small molecules.
The script can calculate the scores for a direct SMILES input or for a .csv or .tsv file containing a list of SMILES.
nSPS is calculated by deafult.

optional arguments:
  -h, --help        show this help message and exit
  -s SMILES string  Your input SMILES string for which to calculate the score
  -i filename.ext   Your .csv or .tsv file containing column called "Smiles" which contains SMILES strings. Resutls will be
                    saved in a new .csv file
  -o filename.csv   You can specify name of the output .csv file. Not required.
  -t                Option to calculate total SPS (no normalisation).
  -v                Option to print verbose results, with information for each atom index.
  -p                Option to print confirmation after processing of each SMILES string in a file.
```

To calculate nSPS directly from a SMILES string you can just type:
```
python spacial_score.py -s CC(C)CBr
```
Where CC(C)CBr is just an example of a SMILES string (on Linux you may need to use quotation marks "CC(C)CBr").
This returns:
```
Normalisation Applied: True
SMILES: CC(C)CBr
Calculated nSPS: 9.6
```

To calculate the un-normalised (total) SPS you need to add option `-t`:
```
python spacial_score.py -s CC(C)CBr -t
```
This returns:
```
Normalisation Applied: False
SMILES: CC(C)CBr
Calculated SPS: 48
```

A more verbose output can be achieved with the option `-v`: 
```
python spacial_score.py -s CC(C)CBr -v
```
The output:
```
SMILES: CC(C)CBr
Atom Idx  Element   Hybrid    Stereo    Ring      Neighbs
------------------------------------------------------------
0         C         3         1         1         1
1         C         3         1         1         9
2         C         3         1         1         1
3         C         3         1         1         4
4         Br        3         1         1         1
------------------------------------------------------------
Total Spacial Score: 48
Per-Atom Score: 9.6
```

To read in a .csv or .tsv file, please type:
```
python spacial_score.py -i your_input_file_name.csv -o your_output_file_name.csv
```
nSPS is calculated by default, and option `-t` can be used to calculate un-normalised SPS. 
Please, remember that your input file needs to contain a column named `Smiles` containing SMILES which will be used for the calculation of the scores.
Examples of input and output files can be found in the folder named `example_input_output_files`.

***
### Calculate the Score with a Python Function
The scores can also be calculated by using function:
```
def calculate_score_from_smiles(smiles: str, per_atom=False, verbose=False) -> float:
    """ Calculates the spacial score as a total SPS or size-normalised, per-atom nSPS for a molecule.

    Parameters:
    ===========
    smiles: valid SMILES string
    per_atom: flag to denote if the normalised per-atom result (nSPS) should be returned
    verbose: flag to denote if the detailed scores for each atom should be printed

    Returns:
    ========
    Total or per-atom numeric spacial score for the provided molecule.
    """
```
***
### Testing the Script
The script in `spacial_score.py` can be tested by running [pytest](https://docs.pytest.org/en/7.2.x/contents.html). In the active `my_sps_env` conda environment type:
```
pytest
```
Correct output will look like this:
```
collected 7 items

test_spacial_score.py .......                                                                                    [100%]

================================================== 7 passed in 0.48s ==================================================

```
