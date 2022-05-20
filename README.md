This repo is for the Senior Project done by Dareen Hussein, Israa Fahmy, Marwah Sulaiman, Mohammed Barakat, Mohammed El-Naggar, and Zahraa Shehabeldin from the American University in Cairo, Spring 22. All needed code and documentation can be found in this repo.

## Dependencies: 
Ubuntu 20.04.3 LTS
Anaconda Environment with PyTorch 1.4, Python 3.6
Python packages: numpy, matplotlib, opencv-python, pyyaml, lmdb

## Hardware setup:
64GB of DDR4 RAM, 2.80GHz
Intel Core i9-10900F CPU
NIVIDIA GetForce RTX 3090 (1 x 24 GB) GPU

## Training:

Download the official training dataset, rename to VimeoTecoGAN/Raw, and place under ./data.

```
# Install youtube-dl for online video downloading
pip install --user --upgrade youtube-dl

# take a look of the parameters first:
python3 dataPrepare.py --help

# To be on the safe side, if you just want to see what will happen, the following line won't download anything,
# and will only save information into log file.
# TrainingDataPath is still important, it the directory where logs are saved: TrainingDataPath/log/logfile_mmddHHMM.txt
python3 dataPrepare.py --start_id 2000 --duration 120 --disk_path TrainingDataPath --TEST

# This will create 308 subfolders under TrainingDataPath, each with 120 frames, from 28 online videos.
# It takes a long time.
python3 dataPrepare.py --start_id 2000 --duration 120 --REMOVE --disk_path TrainingDataPath

```

Generate LMDB for GT data to accelerate IO. The LR counterpart will then be generated on the fly during training.

python ./scripts/create_lmdb.py --dataset VimeoTecoGAN --raw_dir ./data/VimeoTecoGAN/Raw --lmdb_dir ./data/VimeoTecoGAN/GT.lmdb

Train a RBPGAN model. You can specify which gpu to be used in train.sh. By default, the training is conducted in the background and the output info will be logged in ./experiments_BD/RBPGAN/RBPGAN_VimeoTecoGAN_4xSR_2GPU/train/train.log.

bash ./train.sh BD RBPGAN/RBPGAN_VimeoTecoGAN_4xSR_2GPU
