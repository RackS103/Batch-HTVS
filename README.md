# Batch High Throughput Virtual Screening Script, by Rac Mukkamala

## Description
A Bash script to submit batch jobs to ORCA, OpenBabel, MGLTools, and Autodock Vina. Automatically checks for syntax errors in ORCA input files, and can also identify ORCA runtime crashes. Provides a list of failed ORCA jobs at the end for easy troubleshooting. Organizes log files into individual directories, and creates an easy to read results summary text file. Created by Rac Mukkamala

## Prerequisite Installations
In order for the script to work, the following softwares must be installed:
- ORCA (https://orcaforum.kofo.mpg.de/app.php/portal)
- OpenBabel (`sudo apt-get install openbabel`)
- MolKit Python Package (https://packages.ubuntu.com/source/xenial/mgltools-molkit)
- Autodock Vina (http://vina.scripps.edu/)
- Python 2.7 (https://www.python.org/download/releases/2.7/)

*Note: All five of these softwares are already installed in the ASDRP server*

## Params
The `htvs_params.txt` file specifies the full path to ORCA, the preferred ORCA input header for input file syntax checking, the path to MGLTools' `prepare-ligand.py` script, the path to the Autodock Vina params script, and the path to the receptor protein. The params file can be renamed to whatever you prefer, however its contents must follow the format shown below:
```
PATH: /bin/the/path/to/orca
HEADER: ! B3LYP OPT def2-SVP NormalPrint Grid4 *NormalSCF PAL4
MGL: prepare_ligand.py
VINA: vina_params.txt
RECEPTOR: receptor_name.pdbqt
```
- If your ORCA is added to the `$PATH` environment variable and if you are not using OpenMPI, then for the `PATH: ` row in `batch_orca_params` you can just write `orca`. Otherwise, make sure you write out the full path to ORCA. OpenMPI will only run if you write out the full ORCA path! 
- Enter the entire desired ORCA header into the `HEADER: ` row. If you are running a geometry optimization with DFT, B3LYP functional, and OpenMPI parallel processing, you can just copypaste the example header into the `batch_orca_params.txt` file.
- Enter the path to the MGLTools `prepare-ligand.py` script into the `MGL: ` row.
- Enter the filename and/or path for the Autodock Vina config/params file into the `VINA: ` row.
- Enter the filename and/or path for the receptor protein PDBQT file into the `RECEPTOR: ` row.

## Recommended File Structure
```
[root directory]
      htvs.sh
      htvs_params.txt
      prepare_ligand.py
      [receptor].pdbqt
      vina_params.txt
      [input file directory]
          file1.inp
          file2.inp
          file3.inp
          [...].inp
```

## Running the script
1. Set up the file structure outlined above
2. Provide execution privileges to the script by typing the command `chmod +x orcabatch.sh`
3. **Recommended: If running using nohup:** It is recommended that you run the script using nohup as follows:
  `nohup bash htvs.sh [directory with input files] htvs_params.txt > [log.out] &`
4. **For conventional use**: The script can also be run without nohup as follows. The only caveat is that all the script's messages will be sent to stdout and may be lost:
  `./orcabatch.sh [directory with input files] htvs_params.txt &`

## Citation
**Please acknowledge use of this script in any paper/report as follows:**

"Mukkamala, R (2020) Batch High Throughput Virtual Screening Script (Version 1.0) [Source code].https://github.com/RackS103/Batch-HTVS"
