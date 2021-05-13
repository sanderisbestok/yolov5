:warning: **This is not ther original YoloV5**: This version is edited so it can be used for my Thesis. For the official YoloV5 repo please go to [this repository](https://github.com/ultralytics/yolov5).

## Description
This readme will provide, installation, training and testing instructions. Experiments for the thesis are conducted on the Lisa cluster and all installations are done in an Anaconda Environment.

Algorithms used for the thesis are:

 1. [ExtremeNet](https://github.com/sanderisbestok/ExtremeNet)
 2. [TridentNet in Detectron2](https://github.com/sanderisbestok/detectron2)
 3. [YoloV5](https://github.com/sanderisbestok/yolov5)

Evaluation tools can be found in [this repository](https://github.com/sanderisbestok/thesis_tools). Data preparation steps can also be found in this repository, it is advised to first follow the steps there.

## Installation
To install on the Lisa cluster: 

1. Load modules
    ```
    module load 2020
    module load Anaconda3/2020.02 
    ```

2. Clone the repo:
   ```
   git clone https://github.com/sanderisbestok/yolov5
   ```

3. Create the environment:
    ```
    conda create --name yolov5 python=3.8
    ```

4. Install requirements (check if you use the pip of the environment):
    ```
    conda activate yolov5
    pip install -r requirements.txt
    ```

## Training
To train we need the pre-trained YoloV5x model which can be downloaded [here](https://github.com/ultralytics/yolov5/releases). Place this model in the main folder.

The following job can be used to train the network if the network is installed in ~/networks/yolov5 with the environment yolov5.

```
#!/bin/bash
#SBATCH -t 08:00:00

#SBATCH -p gpu
#SBATCH -N 1
#SBATCH --gpus-per-node=gtx1080ti:4

module load 2020
module load Anaconda3/2020.02 

mkdir $TMPDIR/sander
cp -r $HOME/data $TMPDIR/sander/

conda activate yolov5
cd ~/networks/yolov5/
python train.py --img 640 --batch 16 --epochs 300 --data ego.yaml --weights yolov5x.pt
```

## Testing
As of this moment, testing is build into the training stage. So during the training the results will be saved. No later testing is needed.

## Extra
YoloV5 visualiser can be used with the following command:

```
python detect.py --source path_to_image --weights path_to_model --conf 0.50
```

Models are saved in runs/train