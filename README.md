# Hadoop Customer Churn Analysis

A MapReduce-based system for analyzing and predicting customer churn using Hadoop.

## Project Overview

This project implements a probabilistic model for customer churn prediction using Hadoop MapReduce. By analyzing customer attributes in relation to churn status, the system identifies patterns that can help businesses understand and predict customer churn behavior.

## Features

- **Data Processing**: Efficient processing of customer data using MapReduce
- **Attribute Analysis**: Calculation of attribute probabilities in relation to churn
- **Predictive Modeling**: Implementation of a probabilistic model to predict customer churn
- **Performance Metrics**: Evaluation of prediction accuracy

## System Architecture

The system follows a MapReduce architecture with the following components:

1. **Data Input**: Customer data with attributes and churn status
2. **Map Phase**: Processes records to extract attribute-churn pairs
3. **Reduce Phase**: Aggregates data to calculate probabilities
4. **Prediction**: Uses probability calculations to predict churn likelihood
5. **Evaluation**: Measures the accuracy of predictions

## Technical Implementation

### Key Components

- `CStype.java`: Custom writable class implementing WritableComparable for composite keys
- `PAtrriDriver.java`: Main driver configuring and running the MapReduce job
- `PAtrriMapper.java`: Maps customer attributes to churn indicators
- `PAtrriReducer.java`: Calculates probabilities from aggregated data
- `ChurnProb.java`: Processes raw counts into normalized probabilities
- `TestDriver.java`: Driver for testing the prediction model
- `TestMapper.java`: Implements the prediction algorithm
- `AppendPr.java`: Combines predictions with original data for evaluation

### Workflow

1. **Attribute Probability Calculation**:
   - Input data from `trainingSet`
   - MapReduce job processes records
   - Outputs attribute-churn counts to `ATrriProbeFile1`

2. **Probability Normalization**:
   - Processes raw counts from `part-r-00000`
   - Calculates conditional probabilities
   - Outputs normalized probabilities to `Probability`

3. **Churn Prediction**:
   - Uses cached probability file
   - Calculates churn likelihood for each customer
   - Outputs predictions to `finalProb`

4. **Accuracy Evaluation**:
   - Compares predictions with actual churn status
   - Outputs evaluation metrics to `fop_tr`

## Performance

The system uses 10 reducers for parallel processing, optimizing performance on distributed clusters. The prediction model is evaluated based on its accuracy in correctly identifying customer churn.

## Usage

### Prerequisites

- Hadoop cluster or standalone setup
- Java Development Kit (JDK)
- Training data in the appropriate format

### Setup and Execution

1. **Compile the Java files**:
   ```
   javac -classpath $HADOOP_HOME/share/hadoop/common/*:$HADOOP_HOME/share/hadoop/mapreduce/* -d bin src/churn/*.java
   ```

2. **Create JAR file**:
   ```
   jar -cvf ChurnAnalysis.jar -C bin/ .
   ```

3. **Prepare input data**:
   - Place training data in HDFS at `smp_data/trainingSet`

4. **Run the MapReduce job**:
   ```
   hadoop jar ChurnAnalysis.jar churn.PAtrriDriver
   ```

5. **Process probability data**:
   ```
   java -cp ChurnAnalysis.jar churn.ChurnProb
   ```

6. **Run prediction test**:
   ```
   hadoop jar ChurnAnalysis.jar churn.TestDriver
   ```

7. **Evaluate results**:
   ```
   java -cp ChurnAnalysis.jar churn.AppendPr
   ```

### Input Data Format

The input data should be formatted with customer attributes and churn status, with fields separated by double spaces. Example:

```
yes  contract_month-to-month  tenure_0-12  charges_high  paperless_yes  multiple_lines_yes  internet_fiber  tech_support_no
no   contract_one-year        tenure_24-48 charges_low   paperless_yes  multiple_lines_no   internet_dsl    tech_support_yes
```