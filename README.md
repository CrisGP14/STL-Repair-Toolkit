# STL Repair Toolkit

A local command-line tool that diagnoses and repairs broken STL models before 3D printing or CAD work.

It loads an `.stl` file, reports the state of the mesh, repairs it with [trimesh](https://trimesh.org/) and [pymeshfix](https://github.com/pyvista/pymeshfix), and prints a before/after comparison. Your models never leave your machine.

## Features

- **Diagnosis** of:
  - open edges (holes) and non-watertight meshes
  - duplicate and unreferenced vertices
  - degenerate (near zero-area) faces
  - inconsistent normal orientation
  - number of separate components
- **Two-stage repair:** a basic pass with `trimesh`, then hole filling and non-manifold repair with `pymeshfix`
- **Before/after summary** with the change in each metric
- **Graceful fallback:** if `pymeshfix` fails, the `trimesh` result is kept

## Requirements

- Python 3.11 or newer
- The packages in `requirements.txt`, plus `networkx`, which `trimesh` needs to split meshes and fill holes

## Installation

```bash
git clone https://github.com/CrisGP14/STL-Repair-Toolkit.git
cd STL-Repair-Toolkit
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt networkx
```

## Usage

```bash
python3 Flix.py model.stl
```

The repaired mesh is saved next to the original as `model_fixed.stl`. The original file is not modified.

| Option | Description |
|---|---|
| `-o`, `--output PATH` | Path of the repaired file (default: `<name>_fixed.stl`) |
| `--only-trimesh` | Run only the basic repair: faster and less aggressive |

### Examples

```bash
# Choose the output file
python3 Flix.py input.stl --output repaired.stl

# Basic repair only
python3 Flix.py input.stl --only-trimesh
```

> [!NOTE]
> The program's console messages are in Spanish.

## How it works

1. **Load** the STL as a single triangle mesh.
2. **Diagnose** it and print the results. If the mesh is already watertight and has no duplicate vertices or degenerate faces, the program stops here and writes no output file.
3. **Basic repair (`trimesh`):** merge vertices, remove unreferenced vertices and degenerate faces, fix normals and fill small holes.
4. **Advanced repair (`pymeshfix`):** close the remaining holes and fix non-manifold geometry. Skipped with `--only-trimesh`.
5. **Diagnose again**, print the comparison and export the result.

## Limitations

- **Memory use grows with the square of the vertex count.** A test mesh with about 41,000 vertices tried to allocate 12.5 GiB during diagnosis, so detailed models may need to be decimated first.
- `--only-trimesh` closes only small holes; larger ones need the full repair.
- Automatic repair can change the geometry. Check the result before printing, and use Blender, Meshmixer or a CAD tool for complex damage.

## Project structure

```text
STL-Repair-Toolkit/
├── Flix.py            # command-line tool
├── requirements.txt   # Python dependencies
├── .gitignore         # also excludes *.stl files
└── README.md
```
