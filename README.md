# Classify Camera Trap Images

## Introduction

The purpose of this repository is to provide code to classify camera trap images using models developed in the "Human-Computer Optimization" project on Zooniverse.

## Pre-Requisites

1. A trained Keras model (.hdf5 file)
2. A model configuration file (.json)
3. Images in one or multiple sub-directory(ies)
```
directory/
          -/sub-directory1/img1.jpeg, img2.jpeg, ....
          -/sub-directory2/img1.jpeg, img3.jpeg, ...
          -/...
```
4. Installation of Tensorflow and Keras
* https://keras.io/#installation
* https://www.tensorflow.org/install/

## Run Classifier

1. Import CamTrapClassifier
2. Specify parameter of CamTrapClassifier
```
  path_to_model:
      - path to model file
      - string

  model_cfg_json:
      - path to json with model config
      - Json-string with keys: "class_mapper" & "pre_processing"
      - Optional if keras_datagen provided

  keras_datagen:
      - DataGenerator which will be fit on data using a batch
        of 2'000 randomly selected images, will override
        parameters in model_cfg_json
      - Object of keras.preprocessing.image.ImageDataGenerator
      - Optional if model_cfg_json provided

  class_list:
      - list of classes in order of model output layer
      - list
      - Optional, only needs to be specified if not in model_cfg_json

  refit_on_data:
      - Whether to re-fit the DataGenerator on a batch of
        randomly selected images of the provided data
      - Boolean
      - Default: False (recommended)
```

3. Function predict_path to run classifier, produced a csv with predictions
```
  Parameters
  ------------
  path:
      - path to directory that contains 1:N sub-directories
        with images
      - string

  output_path:
      - path to directory to which prediction csv will be written
      - string

  output_file_name:
      - file name of the output csv written to output_path
      - string

  batch_size:
      - number of images to process in one batch, if too large it
        might not fit into memory
      - integer
```

## Example

```python
from classifier import CamTrapClassifier

def predict_images():
    # models
    model_files = [/models/model1.hdf5',
                   /models/model2.hdf5']

    # output path / filenames
    output_path = /data/predictions
    out_file_names = ['predictions_model1.csv', 'predictions_model2.csv']

    # model cfg files
    model_cfg_jsons = [/models/model1_cfg.json',
                       /models/model2_cfg.json']


    for model_file, model_cfg_json, output_file_name in \
     zip(model_files, model_cfg_jsons, out_file_names):

        # classifier
        classifier = CamTrapClassifier(
            path_to_model=model_file,
            model_cfg_json=model_cfg_json)

        # predict images in path
        classifier.predict_path(path=pred_path, output_path=output_path,
                                output_file_name=output_file_name)

if __name__ == "__main__":
    predict_images()
```
