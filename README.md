# yolov7-keypoint-customization
Revision of official yolov7-pose to support custom dataset for keypoint detection.<br>
## Statement
"yolov7-keypoint-customization" is for convinient use of keypoint detection. The revision that added custom keypoint support was based on the official yolov7-pose project.<br>
See https://github.com/WongKinYiu/yolov7 for full official project please.
## Get started
### Configuration
Before training, you need to revise the configuration in ```./data/custom.yaml``` and ```./cfg/yolov7-w6-pose-custom.yaml```.<br>
In ```./data/custom.yaml```, you need to edit your dataset path, class number and class label as follows:<br>
```
train: ../root_path/your_traget_path/yolo_labels/train.txt 
val: ../root_path/your_traget_path/yolo_labels/val.txt      
test: ../root_path/your_traget_path/yolo_labels/val.txt     

nc: 1 #num of class 
names: [ 'custom']  # label of each class 
```
And the train.txt/val.txt should be filled with corresponding image path (Assumed you have completed the preparation of dataset). <br>
In ```./cfg/yolov7-w6-pose-custom.yaml```, just need to revise the nkpt (number of keypoint in each sample) and nc (class_num).<br>
### Running
Now let's start to train your custom yolov7-keypoint model. The pre-training weight can be obtained in official page, see https://github.com/WongKinYiu/yolov7/tree/pose <br>
#### DP mode
```
python train_keypoint.py  --weights ./yolov7-w6-person.pt  --data data/custom.yaml --device 0,1,2,3  --hyp data/hyp.pose.yaml --cfg cfg/yolov7-w6-pose-custom.yaml --name yolov7-keypoint --kpt-label [--epoch] [--batch] [--linear-lr]
```
#### DDP mode
```
python -m torch.distributed.launch --nproc_per_node 4 --master_port 9527 train_keypoint.py --workers 8 --device 0,1,2,3 --sync-bn --batch-size 128 --data data/custom.yaml --img 640 640 --cfg cfg/yolov7-w6-pose-custom.yaml --weights '' --name yolov7-keypoint --hyp data/hyp.pose.yaml
```
#### Detecting
```
python detect.py  --weights ./[your_best].pt --source [your_testset_path] --kpt-label
```
## Acknowledgements
https://github.com/WongKinYiu/yolov7/tree/pose <br>
https://github.com/WongKinYiu/yolov7 <br>
## Further
Any problems about this, welcome to communicate with me. <br>
Email: chenjisen123@foxmail.com <br>
