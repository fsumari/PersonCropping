# PedestrianCropping

* Esta es una modificacion de YOLO, para hacer recortes de los boundingBoxes y generar CROPS.
* Estos crops pueden ser utilizados en cualquier algoritmo de Person ReID
* Recorte de person de manera automatica.


Deteccion de objetos y recorte de personas en un dataset ..

* Primero se hara correr YOLO en TF
* El script PedestrianCropping.py ... ya tienen implementado los recortes.

## 🆕 Are you looking for a new YOLOv3 implemented by TF2.0 ?

>If you hate the fucking tensorflow1.x very much, no worries! I have implemented **a new YOLOv3 repo with TF2.0**, and also made a chinese blog on how to implement YOLOv3 object detector from scratch. <br>
[code](https://github.com/YunYang1994/TensorFlow2.0-Examples/tree/master/4-Object_Detection/YOLOV3) | [blog](https://yunyang1994.github.io/posts/YOLOv3/#more)  | [issue](https://github.com/YunYang1994/tensorflow-yolov3/issues/39)

## part 1. Quick start
1. Clone this file
```bashrc
$ git clone https://github.com/YunYang1994/tensorflow-yolov3.git
```
2.  You are supposed  to install some dependencies before getting out hands with these codes.
```bashrc
$ cd tensorflow-yolov3
$ pip install -r ./docs/requirements.txt
```
3. Exporting loaded COCO weights as TF checkpoint(`yolov3_coco.ckpt`)【[BaiduCloud](https://pan.baidu.com/s/11mwiUy8KotjUVQXqkGGPFQ&shfl=sharepset)】
```bashrc
$ cd checkpoint
$ wget https://github.com/YunYang1994/tensorflow-yolov3/releases/download/v1.0/yolov3_coco.tar.gz
$ tar -xvf yolov3_coco.tar.gz
$ cd ..
$ python convert_weight.py
$ python freeze_graph.py
```
4. Then you will get some `.pb` files in the root path.,  and run the demo script
```bashrc
$ python image_demo.py
$ python video_demo.py # if use camera, set video_path = 0
```
<p align="center">
    <img width="100%" src="https://user-images.githubusercontent.com/30433053/68088581-9255e700-fe9b-11e9-8672-2672ab398abe.jpg" style="max-width:100%;">
    </a>
</p>

## part 2. Train your own dataset
Two files are required as follows:

- [`dataset.txt`](https://raw.githubusercontent.com/YunYang1994/tensorflow-yolov3/master/data/dataset/voc_train.txt):

```
xxx/xxx.jpg 18.19,6.32,424.13,421.83,20 323.86,2.65,640.0,421.94,20
xxx/xxx.jpg 48,240,195,371,11 8,12,352,498,14
# image_path x_min, y_min, x_max, y_max, class_id  x_min, y_min ,..., class_id
# make sure that x_max < width and y_max < height
```

- [`class.names`](https://github.com/YunYang1994/tensorflow-yolov3/blob/master/data/classes/coco.names):

```
person
bicycle
car
...
toothbrush
```

### 2.1 Train on VOC dataset
Download VOC PASCAL trainval  and test data
```bashrc
$ wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
$ wget http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
$ wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
```
Extract all of these tars into one directory and rename them, which should have the following basic structure.

```bashrc

VOC           # path:  /home/yang/dataset/VOC
├── test
|    └──VOCdevkit
|        └──VOC2007 (from VOCtest_06-Nov-2007.tar)
└── train
     └──VOCdevkit
         └──VOC2007 (from VOCtrainval_06-Nov-2007.tar)
         └──VOC2012 (from VOCtrainval_11-May-2012.tar)

$ python scripts/voc_annotation.py --data_path /home/yang/test/VOC
```
Then edit your `./core/config.py` to make some necessary configurations

```bashrc
__C.YOLO.CLASSES                = "./data/classes/voc.names"
__C.TRAIN.ANNOT_PATH            = "./data/dataset/voc_train.txt"
__C.TEST.ANNOT_PATH             = "./data/dataset/voc_test.txt"
```
