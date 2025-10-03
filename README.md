# Delafossite Modification Workflow CLI
#### Author: Dorothea Fennell (dfennell1@bnl.gov, dfennell37@gmail.com)
**Version**: 1.0.0

A command line interface tool designed to simplify running VASP calculations for delafossite materials. Given a base structure, the workflow can:
- Modify composition
- Create vacancies
- Submit calculations
- Calculate vacancy energy
- Set up PDOS calculations
- Parse, integrate and plot PDOS
- Check calculations for errors, timeouts, and cancellations, fixes minor errors, and resubmits calculations

The workflow uses Atomic Simulation Environment (ASE) and Pymatgen to create/modify structures and generate VASP input files. As of right now, the workflow only supports SLURM for job submission. Any commands that submit calculations ***will not work*** with other workload managers. 

\
To use the workflow, you will need the following files:
- A POSCAR file of your base/parent structure that is ***inversion symmetric***. It's important the structure is inversion symmetric, as the workflow modifies the composition and creates vacancies in pairs. 
- A file of the structure's spin pairs, labeled SpinPairs.txt
- A SLURM runscript for VASP calculations

Examples for the above files can be found in the ~/wf-user-files/example-files directory following execution of `wf init`. 
## Workflow Commands (`wf`):

**Usage**:
```console
$ wf [OPTIONS] COMMAND [ARGS]...
```

**Options**:

* `-v, --version`
* `--install-completion`: Install completion for the current shell.
* `--show-completion`: Show completion for the current shell, to copy it or customize the installation.
* `-h, --help`: Show this message and exit.

**Commands**:

* `init`: Initializes workflow settings and sets up user directory ~/wf-user-files.
* `modify`: Modifies delafossite structure based on Mods.txt file, creating Modification_# directories for structural optimization. 
* `removepairs`: Removes atom pairs from structure, creating vacancy directories for structural optimization. 
* `gete`: Gets pristine energy and calculates vacancy energy (in eV). Returns E_pristine.csv and E_vac.csv.
* `pdos`: Sets up PDOS calculations using structural optimization CONTCAR and PDOS_INCAR.txt. 
* `parse`: Parses PDOS data into individual files and integrates to find number of valence electrons, oxidation state, and net spin. Returns integrated-pdos.csv in each PDOS directory. 
* `integrate`: Integrates the PDOS files.
* `plot`: Plots PDOS
* `submit`: Submits vasp calculations.
* `check`: Checks vasp.out for errors and fixes and...

### `wf init`

Initializes workflow settings and sets up user directory ~/wf-user-files. 

**Usage**:

```console
$ wf init [OPTIONS]
```

**Options**:

* `-h, --help`: Show this message and exit.

### `wf modify`

Modifies delafossite structure based on Mods.txt file, creating Modification_# directories. 

**Usage**:

```console
$ wf modify [OPTIONS]
```

**Options**:

* `-h, --help`: Show this message and exit.

### `wf removepairs`

Removes atom pairs from structure, creating vacancy directories. 

**Usage**:

```console
$ wf removepairs [OPTIONS]
```

**Options**:

* `-h, --help`: Show this message and exit.

### `wf gete`

Gets pristine energy and calculates vacancy energy (in eV). Returns E_pristine.csv and E_vac.csv. 

**Usage**:

```console
$ wf gete [OPTIONS]
```

**Options**:

* `-h, --help`: Show this message and exit.

### `wf pdos`

Sets up PDOS calculations using PDOS_INCAR.txt in ~/wf-user-files

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
* `-c, --custom-sigma`: Use custom value for Gaussian smearing. Default is 1.5
* `--help`: Show this message and exit.

## `wf submit`

Submits vasp calculations.

**Usage**:

```console
$ wf submit [OPTIONS] [CALC]
```

**Arguments**:

* `[CALC]`: The type of calculation to submit. Options: struc: Pristine or vacancy surface calculations. pdos: PDOS calculations  [default: struc]

**Options**:

* `-v, --vac`: Run only vacancy calculations. Does not work with calc = pdos
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
