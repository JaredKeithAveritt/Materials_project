from mp_api.client import MPRester
import pymatgen

# Your Materials Project API key
API_KEY = "3CjJOOpDHdcVIVXS4aC2LgQsrHzWNkpS"

# The ID of the material 
material_id = "mp-1040425"


# Create an instance of MPRester
with MPRester(API_KEY) as mpr:
    material_data = mpr.materials.summary.search(material_ids=[str(material_id)])
    structure = mpr.get_structure_by_material_id(str(material_id))
    
    structure.make_supercell((5,5,1))
    from pymatgen.io.ase import AseAtomsAdaptor

# Convert pymatgen structure to ASE Atoms
adaptor = AseAtomsAdaptor()
atoms = adaptor.get_atoms(structure)
from ase import io
import ase
import numpy as np
from ase.visualize import view
import matplotlib.pyplot as plt
from ase.visualize.plot import plot_atoms



fig, ax = plt.subplots()
plot_atoms(atoms, ax, radii=0.9,rotation=('0x,0y,0z'))
fig, ax = plt.subplots()
plot_atoms(atoms, ax, radii=0.9,rotation=('0x,90y,90z'))
fig, ax = plt.subplots()
plot_atoms(atoms, ax, radii=0.9,rotation=('90x,90y,0z'))
print('total number of atoms origionally = ',len(atoms))

# Delete atoms 267 to 270
#atoms_removed = atoms.copy()
#del atoms_removed[len(atoms)-4:len(atoms)]


atoms.write('POSCAR', format='vasp', direct=True)
len(atoms)
