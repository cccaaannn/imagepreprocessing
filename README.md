## imagepreprocessing

- **Creates train ready data for image classification tasks for keras in a single line**
- **Makes multiple image prediction process easier with using keras model from both array and directory**

- **Creates required files for darknet-yolo including cfg file with default parameters and class count calculations in a single line**
- **Predicts and saves multiple images on a directory with using darknet**
- **Annotation tool for yolo**
- **Auto annotation by given random points for yolo**
- **Draw bounding boxes of the images from annotation files that formatted for yolo**

- **Plots confusion matrix**


## Install
```sh
pip install imagepreprocessing
```

## Create training data for keras
```python
from  imagepreprocessing.keras_functions import create_training_data_keras
source_path = "datasets/deep_learning/food-101/only3"
save_path = "food10class1000sampleeach"
create_training_data_keras(source_path, save_path, image_size = 299, validation_split=0.1, percent_to_use=0.1, grayscale = True)
```

## Make prediction from directory with a keras model and plot confusion matrix
```python
from  imagepreprocessing.keras_functions import make_prediction_from_directory_keras
from  imagepreprocessing.utilities import create_confusion_matrix

images_path = "deep_learning/test_images/food2"
model_path = "deep_learning/saved_models/alexnet.h5"

predictions = make_prediction_from_directory_keras(images_path, model_path)

class_names = ["apple", "melon", "orange"]
labels = [0,0,0,1,1,1,2,2,2]
create_confusion_matrix(predictions, labels, class_names=class_names)
```


## Make prediction and create the confusion matrix
```python
from  imagepreprocessing.keras_functions import create_training_data_keras, make_prediction_from_array_keras
from  imagepreprocessing.utilities import create_confusion_matrix, train_test_split

images_path = "deep_learning/test_images/food2"
save_path = "food"

# Create training data split the data
x, y, x_val, y_val = create_training_data_keras(images_path, save_path = save_path, validation_split=0.2, percent_to_use=0.5)

# split training data
x, y, test_x, test_y =  train_test_split(x,y,save_path = save_path)

# ...
# training
# ...

class_names = ["apple", "melon", "orange"]

# make prediction
predictions = make_prediction_from_array_keras(test_x, model, print_output=False)

# create confusion matrix
create_confusion_matrix(predictions, test_y, class_names=class_names, one_hot=True)
```


## Make multi input model prediction and create the confusion matrix
```python
from imagepreprocessing.keras_functions import create_training_data_kera
from  imagepreprocessing.utilities import create_confusion_matrix, train_test_split
import numpy as np

# Create training data split the data and split the data
source_path = "/content/trainingSet"
x, y = create_training_data_keras(source_path, image_size=(28,28), validation_split=0, percent_to_use=1, grayscale=True, convert_array_and_reshape=False)
x, y, test_x, test_y = train_test_split(x,y)

# prepare the data for multi input training and testing
x1 = np.array(x).reshape(-1,28,28,1)
x2 = np.array(x).reshape(-1,28,28)
y = np.array(y)
x = [x1, x2]

test_x1 = np.array(test_x).reshape(-1,28,28,1)
test_x2 = np.array(test_x).reshape(-1,28,28)
test_y = np.array(test_y)
test_x = [test_x1, test_x2]

# ...
# training
# ...

# make prediction
predictions = make_prediction_from_array_keras(test_x, "models/model.h5",print_output=False, model_summary=False, show_images=False)

# create confusion matrix
create_confusion_matrix(predictions, test_y, class_names=["0","1","2","3","4","5","6","7","8","9"], one_hot=True)

```


## Create required files for training on darknet-yolo and auto annotate images by center
```python
from imagepreprocessing.darknet_functions import create_training_data_yolo, auto_annotation_by_random_points
import os

main_dir = "30_class/train_30_class"

# auto annotating all images by their center points (x,y,w,h)
# (it is only possible to make classification this way for detection you have to annotate images by hand)
folders = sorted(os.listdir(main_dir))
for index, folder in enumerate(folders):
    auto_annotation_by_random_points(os.path.join(main_dir, folder), index, annotation_points=((0.5,0.5), (0.5,0.5), (1.0,1.0), (1.0,1.0)))

# creating required files
create_training_data_yolo(main_dir)
```

## Annotation tool
```python
from imagepreprocessing.darknet_functions import yolo_annotation_tool
yolo_annotation_tool("test_stuff/images", "test_stuff/obj.names")
```



