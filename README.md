# OTTR - The Oral ToxiciTy Reporter

OTTR is a mechanistic QSAR model to predict mammalian acute oral toxicity. Details on the development and evaluation of the model are available in the accompanying QMRF and associated publication: Wijeyesakere, S.J., Auernhammer, T., Parks, A., and Wilson, D. (2023). Profiling mechanisms that drive acute oral toxicity in mammals and its prediction via machine learning. Published in Toxicological Sciences.

## OTTR 2.0

OTTR 2.0 is a Python implementation of OTTR, with a desktop application, a command line and a Python
package. **No KNIME installation is needed.** It has two functions, in two tabs of one window:

| Tab | What it does |
|---|---|
| **LD50 prediction** | Predicts the rat acute oral LD50 (mg/kg), the GHS and US EPA acute oral categories, the assigned mechanism of action and an applicability-domain assessment, for typed, loaded or drawn structures. |
| **Analogue search** | New in 2.0. Builds each structure's *OTTR fingerprint* (the 207-bit vector the LD50 models use) and lists the training-set compounds most similar to it (Dice similarity; five by default), with their measured LD50, GHS category and mechanism, and a **mechanistic profile matrix** comparing which profiling flags are set. Hover over any heading for an explanation of the term (for example, TPSA is the *topological polar surface area*). Reports can be saved as Excel or PDF; both carry a glossary of the acronyms. |

The model itself is unchanged from the KNIME version: a compound is assigned a primary mechanism of action,
and that mechanism selects the regression model used to predict its dose.

## Download

Ready-to-run builds are attached to the [latest release](https://github.com/Dow-Cheminformatics/OTTR/releases/latest). Nothing else needs to be installed.

| Platform | File | How to use it |
|---|---|---|
| Windows 10/11 (64-bit) | `OTTR-2.0-windows-x64.exe` | One file; double-click. The first run unpacks it (a minute or so). |
| Windows | `OTTR-2.0-windows-x64.zip` | The same as a folder; double-click `OTTR.exe`. `ottr-cli.exe` is the command line. |
| Linux (x86-64) | `OTTR-2.0-x86_64.AppImage` | `chmod +x` it and run it. Nothing is installed. |
| Debian / Ubuntu | `ottr2_2.0_amd64.deb` | `sudo apt install ./ottr2_2.0_amd64.deb`, then run `ottr2` or choose *OTTR 2* from the menu. |
| Fedora / RHEL / openSUSE | `ottr2-2.0-1.x86_64.rpm` | `sudo dnf install ./ottr2-2.0-1.x86_64.rpm` (or `sudo zypper install`), then run `ottr2`. |
| macOS (Apple silicon) | `OTTR-2.0-macos-arm64.dmg` | Open the disk image and drag `OTTR2` onto *Applications*. The first time, right-click it and choose *Open*. |
| macOS (build it yourself) | `OTTR2-macos-build-kit.zip` | Unzip on a Mac, run `macos/build_app.sh` (or `macos/make_dmg.sh` to make just the disk image). See `docs/BUILD-ON-MAC.txt`. |
| Any | `OTTR2-source.zip` | The complete source (also in this repository). |

The Linux packages are attached to the release by hand and can also be rebuilt and tested by GitHub Actions
(`.github/workflows/linux-packages.yml`). `SHA256SUMS.txt` lists checksums: check the download against it (`sha256sum -c SHA256SUMS.txt`, or `certutil -hashfile <file> SHA256` on
Windows). The Windows and macOS builds are not code-signed, so Windows SmartScreen and macOS Gatekeeper will ask for confirmation the
first time (see the user guide).

> **Status.** The Windows and macOS packages were assembled on Linux and have not yet been run on those systems. The Linux
> packages were run on Linux but not installed through `apt`. Please report anything that misbehaves.

## Using it

**The window.** Start it from the menu or by double-clicking, or from source with `python3 bin/ottr2_gui.py`
(`--theme midnight` for the dark appearance). On either tab, put structures in by typing SMILES (one per line, with an
optional name), loading a CSV/Excel/`.smi` file, or drawing them. Ctrl+Enter (Command+Return) runs the tab that is showing.

**The command line.**

```
ottr2 --check                                  # verify the installation
ottr2 "CCO"                                    # LD50 prediction for one SMILES
ottr2 -f compounds.csv -o results.xlsx         # a file of compounds (Excel, CSV, TSV, JSON or PDF)
ottr2 --analogues "CCOC(=O)CC(C(=O)OCC)SP(=S)(OC)OC" -n 5 -o analogues.pdf
```

(From source, use `python3 bin/ottr.py` in place of `ottr2`; on Windows the command line is `ottr-cli`.) Only the SMILES are
required in an input file; names, CAS numbers and other columns are optional and carried through.

For example, the analogue search for ethanol:

```
$ ottr2 --analogues "CCO" -n 2
1. CCO  [Denaturing alcohol]
   1. 1.000  Ethanol  (64-17-5)  LD50 6200 mg/kg  GHS 5  Denaturing alcohol
   2. 0.900  2-Ethyl-1-butanol  (97-95-0)  LD50 1850 mg/kg  GHS 4  Denaturing alcohol
```

**From Python.**

```python
from ottr import pipeline
context = pipeline.Context.build()
result = pipeline.run_one(1, "56-38-2", "parathion", "CCOP(=S)(OCC)OC1=CC=C(C=C1)N(=O)=O", context)
print(result.columns["MECHANISM"], result.columns["Predicted_LD50_mg_kg"])
```

The **user guide** (`manual/OTTR2-User-Guide.pdf`) covers installation, both tabs, every option, how to read the output and the
matrix, validation status and troubleshooting. A manual page is in `man/ottr.1`.

## Running from source

Python 3.9 or newer with RDKit, NumPy, openpyxl, pycairo and Tk (pandas only to rebuild the models). With conda:

```
conda create -n ottr -c conda-forge python=3.12 rdkit numpy pandas openpyxl pycairo "tk=8.6.*=xft_*"
conda activate ottr
cd bin
python ottr.py --check
python ottr2_gui.py
```

`bin/` is self-contained (models, training set and the structure editor are inside it) and needs no internet connection.
Tests are in `bin/tests` (`validate_analogues.py` needs no display).

## Building the packages

```
python3 windows/build_portable.py --out release          # Windows exe and zip (builds on Linux/macOS/Windows; needs ziglang)
python3 linux/build_linux.py                             # AppImage, .deb and .rpm (needs dpkg-deb, mksquashfs, rpmbuild)
./macos/build_app.sh                                     # on a Mac: OTTR2.app and the disk image
python3 make_source_archive.py                           # release/OTTR2-source.zip
```

## The original KNIME version

The KNIME workflow, its machine-learning models and install instructions are in [`knime/`](knime/README.md).
The QMRF is in [`./KNIME_OTTR/OTTR_QMRF.pdf`](OTTR_QMRF.pdf).

## Licence

OTTR is free software, licensed under the **GNU General Public License, version 3** (see [`LICENSE.txt`](LICENSE.txt)). This
covers OTTR's code and its models. It comes with no warranty. The training set is publicly available data from the US National
Toxicology Program's Integrated Chemical Environment (ICE) and is not covered by that licence; the libraries and other
components included in the packaged builds keep their own licences. See [`THIRD-PARTY.txt`](THIRD-PARTY.txt),
[`COPYRIGHT.txt`](COPYRIGHT.txt) and [`SOURCE.txt`](SOURCE.txt).


## Citation

Wijeyesakere, S.J., Auernhammer, T., Parks, A., and Wilson, D. (2023). Profiling mechanisms that drive acute oral toxicity in
mammals and its prediction via machine learning. *Toxicological Sciences* 193(1), 18-30.
<https://doi.org/10.1093/toxsci/kfad025>

Questions? Please contact us via e-mail at: ottr@dow.com

