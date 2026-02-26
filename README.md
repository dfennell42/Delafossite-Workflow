# Delafossite Modification Workflow CLI
#### Author: Dorothea Fennell (dfennell1@bnl.gov, dfennell37@gmail.com)
**Version**: 1.3.4

A command line interface tool designed to simplify running VASP calculations for delafossite materials. Given a base structure, the workflow can:
- Modify composition
- Create vacancies
- Submit calculations
- Calculate vacancy energy
- Set up PDOS calculations
- Parse, integrate and plot PDOS
- Check calculations for errors, timeouts, and cancellations, fixes minor errors, and resubmits calculations

The workflow uses Atomic Simulation Environment (ASE) and Pymatgen to create/modify structures and generate VASP input files. As of right now, the workflow only supports SLURM for job submission. Any commands that submit calculations ***will not work*** with other workload managers. 

## Installing the Workflow:
The workflow can be installed in multiple ways, depending on your needs. If you want to use the workflow as-is, you can install it like any other package. This can be done by using the GitHub link and pip or by downloading the WHL file and installing it manually. 

For the LCO workflow, ***the following editable installation is recommended.*** This is due to the fact that the workflow was written to work on a specific computing cluster, and as such, some paths are hardcoded, which means they will not work if the package is installed as-is. If using the generalized Delafossite workflow, either installation is fine. 

| Note: It is *highly recommended* to install and use the workflow in a separate environment to minimize potential dependency conflicts.|
|:---|

**Editable Installation:**  
To create an editable installation, you will first need to install Poetry, which the workflow uses as a package builder and dependency manager. The Poetry docs are linked here for reference: [Poetry Docs](https://python-poetry.org/docs/). After installing Poetry, run `poetry self update`. This is the best way to make sure Poetry is up to date before setting up the installation. 

You can then install the workflow by either cloning the GitHub repository or by downloading the tar.gz file and un-tarring it in your home directory. Cloning the repository is probably the easiest way to get any updates made, but I (as of writing this) have not tried that method. It should work, but if you want to be one hundred percent certain it will work, I would recommend using the tar file. 

After installing the workflow, go into the package's head directory, which contains the *pyproject.toml* and *poetry.lock* files. Then run `poetry install` to install the workflow as a package and all necessary dependencies. If Poetry returns an error, run `poetry self update` and then try again. 

### Updating the Workflow
***Note:*** This command requires the installation of GitHub's CLI package `gh` and due to the way it's packaged, Poetry cannot add it as a dependency (believe me, I tried). The package and installation instructions are available here: [GitHub CLI](https://github.com/cli/cli). 

To update the workflow to the latest version, use command `wf update`. This command will download the latest version of the workflow from GitHub and install it. The workflow uses pip to install the new version, so if pip is not installed on your system, it will simply download the wheel file to your home directory. 

If you prefer to install the workflow as an editable package, use option `--editable` or `-e` to download the tar file instead. 

## Using the Workflow:  
### See the workflow guide here: [Workflow Guide](Workflow_Guide.md)


## Workflow Commands (`wf`):

**Usage**:

```console
$ wf [OPTIONS] COMMAND [ARGS]...
```

**Options**:

* `-v, --version`
* `--install-completion`: Install completion for the current shell.
* `--show-completion`: Show completion for the current shell, to copy it or customize the installation.
* `--help`: Show this message and exit.

**Commands**:

* `init`: Initializes workflow settings.
* `modify`: Modifies delafossite structure based on...
* `removepairs`: Removes atom pairs from structures
* `addpairs`: Adds pairs of atoms to structures.
* `gete`: Gets pristine E and E vac
* `pdos`: sets up PDOS calculations
* `parse`: Parses PDOS data into individual files and...
* `integrate`: Integrates the PDOS files.
* `plot`: Plots PDOS
* `bands`: Sets up band structure calculations using...
* `getgap`: Gets band gap, Fermi energy, VBM, and...
* `submit`: Submits vasp calculations.
* `check`: Checks vasp.out for errors and fixes and...
* `update`: Checks workflow version and updates if...

## `wf init`

Initializes workflow settings.

**Usage**:

```console
$ wf init [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf modify`

Modifies delafossite structure based on user input. Needs Mods.txt

**Usage**:

```console
$ wf modify [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf removepairs`

Removes atom pairs from structures

**Usage**:

```console
$ wf removepairs [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf addpairs`

Adds pairs of atoms to structures.

**Usage**:

```console
$ wf addpairs [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf gete`

Gets pristine E and E vac

**Usage**:

```console
$ wf gete [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf pdos`

sets up PDOS calculations

**Usage**:

```console
$ wf pdos [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf parse`

Parses PDOS data into individual files and integrates

**Usage**:

```console
$ wf parse [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf integrate`

Integrates the PDOS files. Note: Files must be parsed before integration. The parse command parses AND integrates, so this command is only if integration needs to be performed on already parsed files.

**Usage**:

```console
$ wf integrate [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf plot`

Plots PDOS

**Usage**:

```console
$ wf plot [OPTIONS]
```

**Options**:

* `-n, --no-show-image`: Do not display plot in X11 window after running command.
* `--help`: Show this message and exit.

## `wf bands`

Sets up band structure calculations using hybrid functionals. Note: Requires WAVECAR file from completed DFT structural optimization.

**Usage**:

```console
$ wf bands [OPTIONS]
```

**Options**:

* `-k, --kpoints TEXT`: The number of K points to use per line in KPOINTS_OPT file. Default is 10.  [default: 10]
* `--help`: Show this message and exit.

## `wf getgap`

Gets band gap, Fermi energy, VBM, and CBM for each modification directory and outputs data into a CSV file.

**Usage**:

```console
$ wf getgap [OPTIONS]
```

**Options**:

* `--help`: Show this message and exit.

## `wf submit`

Submits vasp calculations.

**Usage**:

```console
$ wf submit [OPTIONS] [CALC]
```

**Arguments**:

* `[CALC]`: The type of calculation to submit. Options: struc: Pristine or vacancy surface calculations. pdos: PDOS calculations. bands: Band structure calculations.  [default: struc]

**Options**:

* `-v, --vac`: Run only vacancy calculations. Does not work with calc = pdos
* `-a, --add`: Run only adsorption calculations. Does not work with calc = pdos
* `--help`: Show this message and exit.

## `wf check`

Checks vasp.out for errors and fixes and resubmits calculations if possible.

**Usage**:

```console
$ wf check [OPTIONS]
```

**Options**:

* `-n, --no-submit`: Use -n or --no-submit to run check without autosubmitting calculations  [default: True]
* `--help`: Show this message and exit.

## `wf update`

Checks workflow version and updates if necessary.

**Usage**:

```console
$ wf update [OPTIONS]
```

**Options**:

* `-e, --editable`: Install the workflow as an editable package.
* `--help`: Show this message and exit.
