# jetson AI for robomaster robot Mineral
Using the jetson inference docker container by dusty, this project aims to train a detection model for the yellow Robomaster Mineral that is used during the robomaster competition. The barcode on the cube is essential that it must face a certain way in order to trade for coins.

# Download python3-numpy
 $ cd<br/>
 $ sudo apt-get update<br/>
 $ sudo apt-get install git cmake libpython3-dev python3-numpy<br/>

# Download jetson-inference
 $ git clone --recursive https://github.com/dusty-nv/jetson-inference<br/>
 $ cd jetson-inference<br/>
 $ mkdir build<br/>
 $ cd build<br/>
 $ cmake ../<br/>
 $ make<br/>
 $ sudo make install<br/>
 $ sudo ldconfig<br/>

# Run docker
$ docker/run.sh <br/>
$ cd python/training/detection/ssd/data<br/>
$ mkdir cube <br/>
Open another terminal <br/>
$ cd jetson-inference/python/training/detection/ssd/data/cube<br/>
$ touch labels.txt<br/>

Open a text editor and edit the labels.txt file <br/>
add some labels <br/>
R<br/>
Barcode<br/>
then save and exit the text editor<br/>

# Collect data
$ cd jetson-inference/python/training/detection/ssd <br/>

$ camera-capture /dev/video0 <br/>

(the video0 depends on your video feed)<br/>
Change dataset type to detection , Dataset path to ssd/data/cube, Class labels to labels.txt, Then collect data for train, val, and test <br/>
Once have enough data, close the video window


# train model
$ python3 train_ssd.py -–dataset-type=voc –-data=data/cube –-model-dir=models/cube –-batch-size=2 --workers=1 –-epochs=10<br/>

$ python3 onnx_export.py –-model-dir=models/cube <br/>


# test out the model 
$ detectnet –-model=models/cube/ssd-mobilenet.onnx –-labels=models/cube/labels.txt –-input-blob=input_0 –-output-cvg=scores –-output-bbox=boxes<br/>
