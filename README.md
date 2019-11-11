# volumetric segmentation pipeline #

# dependencies
- Red Hat Enterprise Linux 3.10.0
- Python 3.5.2 and IPython 5.3.0
- Apache Spark 1.6.2
- Advanced Normalization Tools (ANTS) 2.1.0

# general instructions
- Download an example dataset folder: 

	https://www.dropbox.com/sh/psrj9lusohj7epu/AAAbj8Jbb3o__pyKTTDxPvIKa?dl=0

- Launch IPython with Spark.

- Import package and load default parameters.

- Set and save parameters (see `voluseg.parameter_dictionary??` for details).

- Execute code sequentially to perform cell detection.

- The final output is in the file `cells0_clean.hdf5` in the output directory.

# example code

'''
# set up
import os
import sys
import pprint

# add package to path
dir_code = 'path/to/package'
sys.path.append(dir_code)

# check for updates
from voluseg import update
update()

# import package
import voluseg

# set and save parameters
parameters0 = voluseg.parameter_dictionary()
parameters0['dir_ants'] = '/path/to/ants/bin/'
parameters0['dir_input'] = '/path/to/input/images/'
parameters0['dir_output'] = '/path/to/output/directory/'
parameters0['registration'] = 'translation'
parameters0['diam_cell'] = 5.0
parameters0['f_volume'] = 2.0
voluseg.step0_process_parameters(parameters0)

# load parameters
filename_parameters = os.path.join(parameters0['dir_output'], 'parameters.pickle')
parameters = voluseg.load_parameters(filename_parameters)
pprint.pprint(parameters)

# save parameters
print("process images.")
voluseg.step1_process_images(parameters)

print("align images.")
voluseg.step2_align_images(parameters)

print("mask images.")
voluseg.step3_mask_images(parameters)

print("detect cells.")
voluseg.step4_detect_cells(parameters)

print("clean cells.")
voluseg.step5_clean_cells(parameters)
'''
