[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fexamples%2Ftree%2Fmain%2Fpython%2Fflask3&demo-title=Flask%203%20%2B%20Vercel&demo-description=Use%20Flask%203%20on%20Vercel%20with%20Serverless%20Functions%20using%20the%20Python%20Runtime.&demo-url=https%3A%2F%2Fflask3-python-template.vercel.app%2F&demo-image=https://assets.vercel.com/image/upload/v1669994156/random/flask.png)

# Flask + Vercel

This example shows how to use Flask 3 on Vercel with Serverless Functions using the [Python Runtime](https://vercel.com/docs/concepts/functions/serverless-functions/runtimes/python).

## Demo

https://flask-python-template.vercel.app/

## How it Works

This example uses the Web Server Gateway Interface (WSGI) with Flask to enable handling requests on Vercel with Serverless Functions.

## Running Locally

```bash
npm i -g vercel
vercel dev
```

Your Flask application is now available at `http://localhost:3000`.

## One-Click Deploy

Deploy the example using [Vercel](https://vercel.com?utm_source=github&utm_medium=readme&utm_campaign=vercel-examples):

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fexamples%2Ftree%2Fmain%2Fpython%2Fflask3&demo-title=Flask%203%20%2B%20Vercel&demo-description=Use%20Flask%203%20on%20Vercel%20with%20Serverless%20Functions%20using%20the%20Python%20Runtime.&demo-url=https%3A%2F%2Fflask3-python-template.vercel.app%2F&demo-image=https://assets.vercel.com/image/upload/v1669994156/random/flask.png)

# Video Dataset Preprocess

Implementations for preprocessing video datasets including UCF-101 and HMDB-51

## Original Data preprocess

### UCF-101

* Download videos and train/test splits [here](http://crcv.ucf.edu/data/UCF101.php).
  Make sure to put the video files as the following structure:

```
  UCF-101
  ├── ApplyEyeMakeup
  │   ├── v_ApplyEyeMakeup_g01_c01.avi
  │   └── ...
  ├── ApplyLipstick
  │   ├── v_ApplyLipstick_g01_c01.avi
  │   └── ...
  ├── Archery
  │   ├── v_Archery_g01_c01.avi
  │   └── ...
```

Also, the label file's structure is as follows:

```
  ucfTrainTestlist
  ├── classind.txt
  ├── testlist01.txt
  ├── testlist02.txt
  ├── testlist03.txt
  ├── trainlist01.txt
  ├── trainlist02.txt 
  └── trainlist03.txt 
```

* Convert from avi to jpg files using ``utils/video2jpg_ucf101_hmdb51.py``

```bash
python utils/video2jpg_ucf101_hmdb51.py avi_video_directory jpg_video_directory
```

* Generate n_frames files using ``utils/n_frames_ucf101_hmdb51.py``

```bash
python utils/n_frames_ucf101_hmdb51.py jpg_video_directory
```

After pre-processing, the image output dir's structure is as follows:

```
  UCF101_n_frames
  ├── ApplyEyeMakeup
  │   ├── v_ApplyEyeMakeup_g01_c01
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
  ├── ApplyLipstick
  │   ├── v_ApplyLipstick_g01_c01
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
  ├── Archery
  │   ├── v_Archery_g01_c01
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
```

### HMDB-51

* Download videos and train/test splits [here](http://serre-lab.clps.brown.edu/resource/hmdb-a-large-human-motion-database/).
  Make sure to put the video files as the following structure:

```
  HMDB51
  ├── brush_hair
  │   ├── April_09_brush_hair_u_nm_np1_ba_goo_0.avi
  │   └── ...
  ├── cartwheel
  │   ├── (Rad)Schlag_die_Bank!_cartwheel_f_cm_np1_le_med_0.avi
  │   └── ...
  ├── catch
  │   ├── 96-_Torwarttraining_1_catch_f_cm_np1_le_bad_0.avi
  │   └── ...
```

* Convert from avi to jpg files using ``utils/video_jpg_ucf101_hmdb51.py``

```bash
python utils/video2jpg_ucf101_hmdb51.py avi_video_directory jpg_video_directory
```

* Generate n_frames files using ``utils/n_frames_ucf101_hmdb51.py``

```bash
python utils/n_frames_ucf101_hmdb51.py jpg_video_directory
```

* Generate annotation file in txt format using ``utils/hmdb_gen_txt.py``
  * ``annotation_dir_path`` includes brush_hair_test_split1.txt, ...

```bash
python utils/hmdb_gen_txt.py annotation_dir_path jpg_video_directory outdir
```

After pre-processing, the image output dir's structure is as follows:

```
  hmdb51_n_frames
  ├── brush_hair
  │   ├── April_09_brush_hair_u_nm_np1_ba_goo_0
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
  ├── cartwheel
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
  ├── catch
  │   ├── 96-_Torwarttraining_1_catch_f_cm_np1_le_bad_0
  │   │   ├── image_00001.jpg
  │   │   ├── ...
  │   │   └── n_frames
  │   └── ...
```

The Train_Test split file contains following structure:

```
  hmdb51_TrainTestlist
  ├── hmdb51_train.txt
  ├── hmdb51_test.txt
  └── hmdb51_val.txt
```

## load data with PyTorch

Usage of dataloader

```
from dataloaders.hmdb_dataset import HMDBDataset

image_dir = '/home/../hmdb51_n_frames/'
label_file = '/home/../hmdb51_TrainTestlist/hmdb51_train.txt'
hmdb_trainset = HMDBDataset(image_dir, label_file, split='train', clip_len=16)
```

## Citation

The processing codes refer to this repo [3D-ResNets-PyTorch](https://github.com/kenshohara/3D-ResNets-PyTorch).

```bibtex
@inproceedings{hara3dcnns,
  author={Kensho Hara and Hirokatsu Kataoka and Yutaka Satoh},
  title={Can Spatiotemporal 3D CNNs Retrace the History of 2D CNNs and ImageNet?},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  pages={6546--6555},
  year={2018},
}
```
