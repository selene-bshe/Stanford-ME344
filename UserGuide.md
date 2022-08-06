### 1. Log on 
Log on to cluster `23` with user account `bshe` using password: `stanfordme344`:
```
ssh bshe@hpcc-cluster-23
```

### 2. Use slurm to submit a Gromacs simulation job with command in the run directory:
```
cd $ gxm_root_dir/run
sbatch submit_gmx.slurm
```
This will take a few minutes to finish the job. You can use the command `squeue` to check job status. To cancel jobs, use `scancel +[JOBID]`.

### 3. To visulize gromacs simulation results, submit another job with command:
```
sbatch jupyter_gmx.slurm
```
With this job running in background, open a new terminal in the local computer, and run the following command (there is no output for this command):
```
ssh -L 8888:localhost:8888 bshe@hpcc-cluster-23 -t ssh -N -L 8888:localhost:8888 compute-1-1
```
### Visualization with Jupyter notebook
Now open a browser and enter the following URL:
```
http://localhost:8888/?token=12f69a8a5b815de0f3aa04965b64cefe793a384381551e89
```
The browser now opens a Jupternotebook. Open a new Python 3 notebook, add and execute the following code:
```python
import nglview as nv
view = nv.show_structure_file("confout.gro")
view.clear()
view.add_cartoon()
view
```
After some time, you would be able to see a picture of a macromolecule (a polymer blobs). We will ask you to save the visualization results.

### Save figure
You could save the figure of the macromolecule using screenshot (for example, `Shift+Command+4`), or save the page as pdf:
1. Right click the current page, then choose `print page...`;
2. In the drop-down box, choose `Save as PDF`;
3. Choose the name and location of the pdf file, and press `Save`.






