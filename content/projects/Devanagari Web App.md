# Devanagari Language Learning Web App

## Technical Implementation

### Technologies Used
- Python
- Flask
- TensorFlow
- Keras
- scikit-learn
- Pandas
- NumPy
- Matplotlib

### Convolutional Neural Network (CNN) Architecture

The CNN model was designed to accurately recognize handwritten Devanagari characters. The architecture consists of the following layers:

1. **Conv2D Layers**: Extract features from the input images using convolutional filters.
2. **MaxPooling Layers**: Reduce the spatial dimensions by a factor of 2 to increase computational efficiency.
3. **Flatten Layer**: Convert the data to a vector format suitable for fully connected layers.
4. **Dropout Layer**: Set at 50% to prevent overfitting by randomly setting half of the input units to 0 at each update during training.
5. **Dense Layers**: Use softmax activation to classify the distinct characters.

![Model Architecture](/assets/model_architecture.svg)

### Code Snippets

[Include Explained Code Snippets Here]

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

[Include Graphs and Statistical Analysis Here]

## Conclusion

This project showcases a comprehensive approach to developing a CNN for handwritten Devanagari character recognition. By employing advanced data augmentation, dropout layers, and rigorous testing and analysis, the model was optimized to achieve high accuracy and robustness. The integration into a web-based interface further demonstrates the practical application of this technology in real-time language recognition.

Through this detailed implementation, the project highlights critical aspects of deep learning, including model architecture, overfitting mitigation, and performance evaluation, providing valuable insights into developing and refining neural networks for complex tasks.
