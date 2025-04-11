# 4D Instruct-GS2GS

Editing dynamic scenes is crucial in various real-world applications, such as virtual reality, augmented reality, and
film production, where precise control and modification of scenes are necessary. Building on the advancements of Instruct-NeRF2NeRF and Instruct-GS2GS, we built this project, **4D Instruct-GS2GS**, which extends semantic editing capabilities to dynamic scenes. This method leverages an iterative dataset update technique for consistent 3D and temporal editing of Gaussian splatting scenes via text instructions. Our approach enables realistic global text edits on large real-world scenes and individual subjects, incorporating the temporal dimension while maintaining rapid training times (approximately 30 minutes per scene) and high
rendering speeds (up to 82 FPS). This work significantly enhances the practicality and versatility of dynamic scene editing in 3D vision and computer graphics.

## Examples of Edited Dynamic Scenes

<div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">

  <figure style="text-align: center;">
    <video width="500" controls>
      <source src="edited_scenes/trex.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <figcaption><strong>Video 1:</strong> T-Rex Scene (<b>caption: Turn the skeleton into gold and the stone into
red</b>)</figcaption>
  </figure>

  <figure style="text-align: center;">
    <video width="500" controls>
      <source src="edited_scenes/standup.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <figcaption><strong>Figure 2:</strong> Stand-up Scene (<b>caption: Change the outfit color into green</b>)</figcaption>
  </figure>

</div>

## Environmental Setups

Please follow the [3D-GS](https://github.com/graphdeco-inria/gaussian-splatting) to install the relative packages.

```bash
conda create -n 4dedit python=3.8 
conda activate 4dedit

pip install -r requirements.txt
pip install -e submodules/depth-diff-gaussian-rasterization
pip install -e submodules/simple-knn
```

In our environment, we use pytorch=1.13.1+cu116.

Follow the instructions [at this link](https://docs.nerf.studio/quickstart/installation.html) to create the environment and install dependencies. Only follow the commands up to tinycudann. After the dependencies have been installed, return here.

Once you have finished installing dependencies, including those for gsplat, you can install Instruct-GS2GS using the following command:
```bash
pip install git+https://github.com/cvachha/instruct-gs2gs
```


## Data Preparation

**For synthetic scenes:**
The dataset provided in [D-NeRF](https://github.com/albertpumarola/D-NeRF) is used. You can download the dataset from [dropbox](https://www.dropbox.com/s/0bf6fl0ye2vz3vr/data.zip?dl=0).

**For real dynamic scenes:**
The dataset provided in [HyperNeRF](https://github.com/google/hypernerf) is used. You can download scenes from [Hypernerf Dataset](https://github.com/google/hypernerf/releases/tag/v0.1) and organize them as [Nerfies](https://github.com/google/nerfies#datasets). 

For training hypernerf scenes such as `virg/broom`: Pregenerated point clouds by COLMAP are provided [here](https://drive.google.com/file/d/1fUHiSgimVjVQZ2OOzTFtz02E9EqCoWr5/view). Just download them and put them in to correspond folder.

```
├── data
│   | dnerf 
│     ├── mutant
│     ├── standup 
│     ├── ...
│   | hypernerf
│     ├── interp
│     ├── virg
│   
```



## Training

For training synthetic scenes such as `trex`, run

```
python train.py -s data/dnerf/trex --port 6017 --expname "dnerf/trex" --configs arguments/dnerf/trex.py --prompt "Turn the skeleton into gold and the stone into red" --dataset_change_iter 10000
```
### **The training script "train.py" takes the variable "prompt" as argument with some default text. To give different prompts to the diffusion model please change that default text.**

## Rendering

Run the following script to render the images.

```
python render.py --model_path "output/dnerf/trex/"  --skip_train --configs arguments/dnerf/trex.py  &
```


## Acknowledgements
This project is built on top of the of the codebase provided by these super cool projects:
["InstructGS2GS"](https://instruct-gs2gs.github.io/) and 
["4D Gaussian Splatting for Real-Time Dynamic Scene Rendering"](https://guanjunwu.github.io/4dgs/)
