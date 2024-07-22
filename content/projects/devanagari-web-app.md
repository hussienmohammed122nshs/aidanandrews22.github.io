This project demonstrates a comprehensive approach to developing a CNN for handwritten Devanagari character recognition. I built this model based on AlexNet and the heavily used research paper ["ImageNet Classification with Deep Convolutional Neural Networks" by Krizhevsky, Sutskever, and Hinton (2012)](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf), google (of course), and the dreaded ChatGPT. By employing advanced data augmentation, dropout layers, and rigorous testing and analysis, the model was optimized to achieve high accuracy and robustness.

## Technical Implementation

### Convolutional Neural Network (CNN) Architecture

The CNN model was designed to accurately recognize handwritten Devanagari characters. The architecture consists of the following layers:

1. **Conv2D Layers**: Extract features from the input images using convolutional filters.
2. **MaxPooling Layers**: Reduce the spatial dimensions by a factor of 2 to increase computational efficiency.
3. **Flatten Layer**: Convert the data to a vector format suitable for fully connected layers.
4. **Dropout Layer**: Set at 50% to prevent overfitting by randomly setting half of the input units to 0 at each update during training.
5. **Dense Layers**: Use softmax activation to classify the distinct characters.

![Model Architecture](/assets/model_architecture.png)

### Model Code With Detailed Comments

``` python
# Enhanced Model Architecture
model = Sequential()

# Starting with a Conv2D layer with 64 filters for initial feature extraction from the input image
model.add(Conv2D(64, (3,3), padding='same', input_shape=(32, 32, 1), activation='relu'))
# Normalizing the output of the previous layer to speed up training and improve model stability
model.add(BatchNormalization())
# Adding another Conv2D layer with 64 filters to extract more complex features
model.add(Conv2D(64, (3,3), padding='same', activation='relu'))
# Normalizing the output of the previous Conv2D layer
model.add(BatchNormalization())
# Applying MaxPooling2D to reduce spatial dimensions and condense the most important features
model.add(MaxPooling2D(pool_size=(2,2)))

# Increasing the depth with 128 filters to capture finer details in the images
model.add(Conv2D(128, (3,3), padding='same', activation='relu'))
# BatchNormalization to maintain the mean output close to 0 and the standard deviation close to 1
model.add(BatchNormalization())
# Reducing the dimensions further to ensure the model focuses on the most critical features
model.add(MaxPooling2D(pool_size=(2,2)))

# Further increasing the depth with 256 filters for high-level feature extraction
model.add(Conv2D(256, (3,3), padding='same', activation='relu'))
# Normalizing the features from the previous Conv2D layer
model.add(BatchNormalization())
# Another MaxPooling2D to reduce dimensions and prepare the data for the Dense layers
model.add(MaxPooling2D(pool_size=(2,2)))

# Adding more complexity with a high number of filters to learn the most abstract representations
model.add(Conv2D(512, (3,3), padding='same', activation='relu'))
# Double BatchNormalization potentially by mistake, usually a single one is sufficient after a Conv2D
model.add(BatchNormalization())
model.add(BatchNormalization())  # This line might be redundant and can be removed.
# Final MaxPooling2D to reduce the feature map to its smallest spatial dimensions
model.add(MaxPooling2D(pool_size=(2,2)))

# Flattening the 3D output to a 1D vector to serve as input to the Dense layer
model.add(Flatten())
# A Dense layer with 512 units to interpret the features extracted by Conv2D layers
model.add(Dense(512, activation='relu', kernel_regularizer='l2'))  # Regularization to prevent overfitting
# Dropout layer to ignore randomly selected neurons during training to prevent overfitting
model.add(Dropout(0.5))
# The final Dense layer with 46 units and softmax activation for multi-class classification
model.add(Dense(46, activation='softmax'))

# Displaying the model's architecture summary
model.summary()

# Compile the model with Adam optimizer and categorical crossentropy for multi-class classification
optimizer = Adam(lr=0.001)
model.compile(loss='categorical_crossentropy', optimizer=optimizer, metrics=['accuracy'])

# Training the model using data augmentation to improve generalization
history = model.fit(
    datagen.flow(re_x_train, y_train, batch_size=32),
    validation_data=(re_x_test, y_test),
    epochs=150,  # Extended training period for better learning
    verbose=2,
    callbacks=[lr_scheduler, lr_plateau_callback]  # Callbacks for dynamic learning rate adjustments
)
```

### Challenges and Solutions

**Problem**: Overfitting
Overfitting was a significant challenge encountered during the development of the CNN. Overfitting occurs when the model learns the training data too well, including the noise and details, which negatively impacts its performance on new, unseen data.

**Solution**:
To mitigate overfitting, several techniques were employed:

1. **Dropout Layers**: By setting the dropout rate to 50%, we ensured that during each training iteration, half of the neurons were randomly ignored, which forced the model to learn more robust features.
   
2. **Data Augmentation**: Real-time data augmentation techniques such as rotation, scaling, and flipping were used to artificially increase the size of the training dataset and improve the model's generalization ability.

### Data Analysis and Model Testing

To understand and address the overfitting issue, a detailed analysis of the model's performance was conducted:

1. **Graphing Model Performance**: Accuracy and loss graphs were plotted to visualize the training and validation performance over epochs. This helped identify when the model started to overfit.
   
2. **Manual Testing**: The model was manually tested on a subset of images to identify specific instances where it underperformed. This helped in understanding the types of images that were causing issues.
   
3. **Statistical Analysis**: A statistical analysis was performed to find discrepancies in the model's predictions. This included analyzing the distribution of errors and understanding the characteristics of misclassified images.

### Training and Evaluation

1. **Adaptive Learning Rate Scheduling**: The learning rate was dynamically adjusted during training to improve convergence and avoid getting stuck in local minima.
   
2. **Epochs**: The model was trained over 150 epochs to ensure comprehensive learning without overfitting.
   
3. **Evaluation Metrics**: Accuracy and loss were monitored throughout the training process. Step times were printed to keep track of the computational efficiency.

![Graphs and Statistical Analysis](/assets/dp_testing.png)

### Libraries Used
- Python
- Flask
- TensorFlow
- Keras
- scikit-learn
- Pandas
- NumPy
- Matplotlib
