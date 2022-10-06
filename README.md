# Tested by KiokAHn
##  Environment
- MS Windows 10 Pro    
- Python 3.10.7    
- PyTorch 1.12    
- torchvision 0.13.1    
- torchaudio 0.12.1    
- CUDA 11.6    

## Make Python Virtual Environment
```
cd tiny-faces-pytorch
python -m venv ./venv
.\venv\Script\activate.bat
```

## Install PyTorch   
```
pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
pip install -r requirements.txt
```

## WIDER Face dataset

[Download WIDER Face dataset](http://shuoyang1213.me/WIDERFACE]    


## Octave for Evaluation

[eval_tools Download](http://shuoyang1213.me/WIDERFACE/support/eval_script/eval_tools.zip)    
[Octave Download](https://octave.org/download.html#ms-windows)    

### Modified code for Octave        
- plot_pr.m    
Reference : https://docs.octave.org/v7.2.0/Figure-Properties.html    
```
%figure1 = figure('PaperSize',[20.98 29.68],'Color',[1 1 1], 'rend','painters','pos',[1 1 800 400]);
figure1 = figure(1, 'papersize',[20.98 29.68],'color',[1 1 1], 'renderer','painters', 'position',[1 1 800 400]);
```


cli   
.\Octave-7.2.0\cmdshell.bat
change drive(example (e:))    
```
cd /e/
```
```
warning: implicit conversion from numeric to char
warning: called from
    wider_plot at line 26 column 26
    wider_eval at line 35 column 1

Mesa: User error: GL_INVALID_VALUE in glTexImage2D(invalid width=51012 or height=25505 or depth=1)
Mesa: User error: GL_INVALID_VALUE in glRenderbufferStorage(invalid width 51012)
C:\DevTools\Octave-7.2.0\mingw64\bin\octave.exe: No error
```

## Training
```
python main.py ./wider_face_split/wider_face_train_bbx_gt.txt ./wider_face_split/wider_face_val_bbx_gt.txt --dataset-root ./data/WIDER/
```

## Evaluation
```
python evaluate.py ./wider_face_split/wider_face_val_bbx_gt.txt --dataset-root ./data/WIDER/ --checkpoint ./weights/checkpoint_50.pth --split val
``` 

# tiny-faces-pytorch

This is a PyTorch implementation of Peiyun Hu's [awesome tiny face detector](https://github.com/peiyunh/tiny). 

We use (and recommend) **Python 3.6+** for minimal pain when using this codebase (plus Python 3.6 has really cool features).

**NOTE** Be sure to cite Peiyun's CVPR paper and this repo if you use this code!

This code gives the following mAP results on the WIDER Face dataset:

| Setting | mAP   |
|---------|-------|
| easy    | 0.902 |
| medium  | 0.892 |
| hard    | 0.797 |

## Getting Started

- Clone this repository.
- Download the WIDER Face dataset and annotations files to `data/WIDER`.
- Install dependencies with `pip install -r requirements.txt`.

Your data directory should look like this for WIDERFace

```
- data
    - WIDER
        - README.md
        - wider_face_split
        - WIDER_train
        - WIDER_val
        - WIDER_test
```

## Pretrained Weights

You can find the pretrained weights which get the above mAP results [here](https://drive.google.com/open?id=1V8c8xkMrQaCnd3MVChvJ2Ge-DUfXPHNu).

## Training

Just type `make` at the repo root and you should be good to go!

In case you wish to change some settings (such as data location), you can modify the `Makefile` which should be super easy to work with.

## Evaluation

To run evaluation and generate the output files as per the WIDERFace specification, simply run `make evaluate`. The results will be stored in the `val_results` directory.

You can then use the dataset's `eval_tools` to generate the mAP numbers (this needs Matlab/Octave).

Similarly, to run the model on the test set, run `make test` to generate results in the `test_results` directory.
