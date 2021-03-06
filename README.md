# fMoW: Functional Map of the World

This code was developed by [JHU/APL](https://jhuapl.edu).


## Dependencies

The following libraries were used for training/testing the deep learning models:

Keras 2.0.5

Tensorflow 1.2.1

tqdm

## Dataset

The following directory structure was used for training/testing:


```

fmow_dataset/
    train/
        airport/
            airport_0/
                airport_0_0_rgb.jpg
                airport_0_0_rgb.json
                ...
                airport_0_5_rgb.jpg
                airport_0_5_rgb.json
            ...
        ...
        
        zoo/
            zoo_0/
                zoo_0_0_rgb.jpg
                zoo_0_0_rgb.json
                ...
                zoo_0_8_rgb.jpg
                zoo_0_8_rgb.json
                
                ...

    test/
        0000000/
            0000000_0_rgb.jpg
            0000000_0_rgb.json
            ...
            0000000_5_rgb.jpg
            0000000_5_rgb.json
        ...
```

## Results Format

This code will output txt files in the format required by Topcoder, where each line contains comma-separated values of the bounding box ID and a string containing the category. The txt files are stored in: 

```
data/output
```

## Running the Code

To first prepare the dataset for training and testing, change the following line in params.py to point to the RGB-only version of the dataset:

```directories['dataset'] = '../../fmow_dataset'```

Our code does not currently operate on the full (TIFF) version of the fMoW dataset, but this should be possible with simple code changes.

To process the dataset, saving the AOIs and saving metadata feature vectors, run the following code (note: this may take hours and will generate files occupying ~8.7GB):

```
python runBaseline.py -prepare
```

Once this is complete, the following arguments can be passed in to run different parts of the code:

```
-all: run everything
-cnn: train the CNN (default: with metadata -- use -nm to use only image features)
-codes: generate and save 4096-d feature vectors to be passed to an LSTM
-lstm: train the LSTM using the codes generated by the CNN and concatenate metadata features (unless -nm argument used)
-test: generate predictions files using both CNN and LSTM models
-test_cnn: generate a predictions file only using the CNN
-test_lstm: generate a predictions file only using the LSTM
-nm: do not use metadata for training or testing
```

Our best performing model is the CNN-only approach, which sums predictions over each temporal view and then takes an argmax. However, we provide code for using an LSTM, which performs slightly worse, so that modifications can be made.

## License

The license is Apache 2.0. See LICENSE.
