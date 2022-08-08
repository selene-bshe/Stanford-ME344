### 1. Log on 
Log on to cluster `23` with user account `bshe` using password: `stanfordme344`:
```
ssh bshe@hpcc-cluster-23
```
You should see the CLI now shows:
```
[bshe@hpcc-cluster-23 ~]$
```
### 2. Submiting GROMACS job:
First, change to the `run` directory:
```
cd /home/bshe/project-2-b/gromacs/gromacs-2020.2/run
```
Next use slurm to submit a Gromacs simulation job with command in the run directory:
```
sbatch submit_gmx.slurm
```
This will take a few minutes to finish. You can use the command `squeue` to check job status. You will see results as below. To cancel jobs, use `scancel [JOBID]`(e.g., `$ scancel 8` in this case).
```
 JOBID PARTITION      NAME     USER ST       TIME  NODES   NODELIST(REASON)
     8    normal   Jupyter     bshe  R       1:06      1     compute-1-1 
```


### 3. Submiting another Jupyeter job: 
```
sbatch jupyter_submit.slurm
```
With this job running in background, ***open a new terminal in the local computer***, and run the following command:
```
ssh -L 8888:localhost:8888 bshe@hpcc-cluster-23 -t ssh -N -L 8888:localhost:8888 compute-1-1
```
You will be asked to enter password for `hpcc-cluster-23` and `compute-1-1` nodes. Use `stanfordme344` for both case. There should be no output, and simply proceed to next step.

### 4. Visualization with Jupyter notebook
We will now use a browser to run Jupyter notebook. Go back to the first terminal that was running `sbatch` on, and enter the command:
```
egrep -w 'compute|localhost'  slurm-*.out
```
Copy the URL in the bottom line of the output, which is in the format as:
```
slurm-[JOBID].out:        http://localhost:8888/?token=9c2ac0cca5ae89f93432c557c3c200c792f11ca2a37d714a
```
where the `[JOBID]` corresponds to the Jupyter job you just submitted. You can use `squeue` to check. Paste the URL to a broswer. The browser now opens a Jupyter page. 

If you see `Visualization.ipynb`, open it and run the cell. Otherwise open a new Python 3 notebook, add and execute the following code:
```python
import nglview as nv
view = nv.show_structure_file("confout.gro")
view.clear()
view.add_cartoon()
view
```
After some time, you would be able to see a picture of a macromolecule (a polymer blobs). We will ask you to save the visualization results.

### 5. Save figure
You could save the figure of the macromolecule using screenshot (for example, `Shift+Command+4` for Macbook user), or save the page as pdf:
1. Right click the current page, then choose `print page...`;
2. In the drop-down box, choose `Save as PDF`;
3. Choose the name and location of the pdf file, and press `Save`.



### 6. Terminating Jupyter
To terminate Jupyter notebook, close the browser, and enter `control+C` in the terminal of local computer to interupt the port-forwarding process.

Next, go back to the first terminal and use `scancel [JOBID]]`, where `[JOBID]` can be found using `squeue` as previously described. Now the Juoyter job is stopped from running and you can safely exit remote server using `exit`.

