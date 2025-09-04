# **Neural Network Learns Multiplication: An Exploratory Project 🧠**

This project is an experiment to explore the capabilities and limitations of neural networks in learning the mathematical operation of multiplication. The goal is to understand how different problem representations and feature engineering techniques impact a model's ability to learn what is fundamentally a precise, algorithmic task.

The project evolves from attempting to teach a network the step-by-step *algorithm* of multiplication to simplifying the problem into a linear relationship that a simple model can easily solve.

## **Approaches Explored**

We are comparing two primary methods for this task.

### **Approach 1: Digit-wise Approximation with Place Value Features**

This approach attempts to make the network learn the multi-step algorithm of long multiplication from the digits of the numbers.

#### **The Initial Task**

The original goal was to create a model that takes the individual digits of two numbers as input and outputs the individual digits of their product.

* **Input:** A 5-element array, e.g., \[1, 2, 3, 4, 5\] representing 123 \* 45\.  
* **Output:** A 5-element array, e.g., \[0, 5, 5, 3, 5\] representing the product 5535\.

This proved to be extremely difficult for a standard Feedforward Neural Network (FFN), as the model struggled to learn the concepts of place value and "carrying" digits.

#### **Our Current Method (Place Value Engineering)**

To help the model, we engineered the input features to give the network a sense of each digit's magnitude.

The workflow is as follows:

1. **Input Raw Digits:** Start with the raw digit array (e.g., \[1, 2, 3, 4, 5\]).  
2. **Apply Place Values:** Multiply each digit by its corresponding place value (100, 10, 1, etc.).  
   * \[1, 2, 3, 4, 5\] becomes \[100, 20, 3, 40, 5\].  
3. **Global Normalization:** Normalize the entire transformed vector by dividing by the maximum possible value (900.0). This scales the inputs while preserving their relative magnitude.  
4. **Model:** A deep Feedforward Neural Network then takes this processed input to predict the output digits.

**Outcome:** While this feature engineering provided a significant performance boost over using raw digits, the model's accuracy remains low. This highlights the fundamental challenge for an FFN to perfectly learn a precise algorithm.

### **Approach 2: Logarithmic Transformation (Problem Simplification)**

This approach completely reframes the problem by using a clever mathematical trick to avoid teaching the network multiplication altogether.

#### **The Core Principle**

The strategy is based on the logarithmic identity:  
log(a \* b) \= log(a) \+ log(b)  
This identity allows us to convert a difficult, non-linear multiplication problem into a simple, linear addition problem.

The workflow is as follows:

1. **Input Two Numbers:** Start with the two numbers to be multiplied (e.g., \[123, 45\]).  
2. **Logarithmic Transformation:** Take the natural logarithm of each number.  
   * The model's input becomes \[log(123), log(45)\].  
3. **Model Task:** The neural network's new, much simpler job is to learn to **add** these two transformed numbers.  
4. **Inverse Transformation:** The model outputs the sum of the logs. We then apply the exponential function (exp()) to this output to get the final product.  
   * exp(log(123) \+ log(45)) equals 123 \* 45\.

**Outcome:** This method is expected to yield extremely high accuracy with a much simpler model (potentially a single neuron). It serves as a powerful demonstration of how effective feature engineering can be more important than a complex model architecture.

## **Next Steps**

Our next step is to modify our current code to implement **Approach 2**. We will then train both models and directly compare their performance metrics to quantify the impact of these different strategies.
