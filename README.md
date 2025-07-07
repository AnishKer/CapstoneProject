# Capstone Project
This project addresses the problem of inefficient urban parking utilization caused by static pricing models. Traditional fixed-price systems often lead to parking lots being either overcrowded during peak hours or underused during off-peak times. The main goal of this project was to design and implement an intelligent, data-driven pricing engine for 14 urban parking spaces that dynamically adjusts prices in real time. The key outcome is a complete, end-to-end simulation that ingests streaming parking data, processes it through three custom-built pricing models of increasing complexity, and provides live visualizations of the resulting price changes, demonstrating a smooth, logical, and explainable pricing strategy based on demand and competition.

# Tech Stack
The primary technologies, frameworks, and libraries used to build this project are listed below.

Programming Language: Python
Data Manipulation: Pandas, NumPy
Real-Time Data Processing: Pathway
Data Visualization: Bokeh
Machine Learning: All models were built from scratch using only NumPy and Pandas, with no external machine learning libraries used, as per the project requirements.
Notebook Environment: Google Colab

## Architecture Diagram

The following diagram illustrates the overall architecture and data flow of the project.

graph TD;
A[Data Collection] --> B[Data Preprocessing & Cleaning];
B --> C[Exploratory Data Analysis (EDA)];
C --> D[Feature Engineering];
D --> E[Model Training & Hyperparameter Tuning];
E --> F[Model Evaluation];
F --> G[Results Visualization & Reporting];


## Project Architecture and Workflow

This section provides a detailed explanation of the project's structure and the steps involved from start to finish.

1.** Data Collection **
The data for this project was provided as a single CSV file named dataset.csv as part of the "Dynamic Pricing for Urban Parking Lots" capstone project requirements. This dataset is not publicly available and was created specifically for this project. It contains simulated data capturing the state of 14 distinct urban parking spaces over a period of 73 days. For each day, data was sampled at 18 different time points, creating a comprehensive time-series dataset ideal for analyzing demand fluctuations.

2.** Data Preprocessing & Cleaning**
The data was prepared for analysis through a multi-step preprocessing and cleaning pipeline implemented in Python using the Pandas library. The key steps were:

Handling Missing Values: The dataset was first inspected for any missing data points in its columns. The strategy adopted was to fill any numerical missing values with the mean of their respective columns to preserve the dataset's size and statistical properties.

Correcting Data Types: All columns were checked to ensure they had the appropriate data type. A critical step was converting the timestamp column from a string or object type into a proper datetime object, which is essential for time-series analysis and sorting.

Removing Duplicates: The dataset was scanned for any duplicate rows, and all such rows were removed to prevent skewed analysis and model bias.

Header Cleaning: To ensure compatibility with the Pathway real-time processing framework, the column headers in the CSV file were cleaned. This involved replacing spaces with underscores (e.g., changing 'Queue length' to Queue_length). This one-time preprocessing step created a new file, dataset_cleaned.csv, which was used for the simulation phase.

3.** Exploratory Data Analysis (EDA)**
The analysis of the dataset began with an exploratory phase focused on understanding its structure and content. Using Pandas functions like info(), head(), and isnull().sum(), an initial assessment of the data was performed. This revealed the data types of each column, the presence of any missing values, and the overall shape of the dataset. The primary goal of this EDA was to verify data quality and identify the necessary preprocessing steps. The analysis confirmed the presence of key features required for the pricing models, including Occupancy, Capacity, Queue_length, Traffic, Latitude, and Longitude, which formed the basis for subsequent feature engineering and model development.

4. **Feature Engineering**
Raw data was transformed into meaningful features to improve the performance and logic of the pricing models. This was done from scratch using only NumPy and Pandas. The new features created include:

Occupancy Rate: A crucial feature, calculated as Occupancy / Capacity. This normalized value served as a primary input for both the baseline and demand-based models.

Categorical Encoding: To use categorical data in mathematical models, the Vehicle Type column was one-hot encoded into separate binary columns (vehicle_car, vehicle_bike, etc.). The Special Day indicator was also converted into a numerical (0 or 1) format.

Time-Based Features: The hour_of_day was extracted from the timestamp column to allow the models to learn time-dependent demand patterns, such as morning rush hours.

Feature Normalization: Numerical features like Queue length and Traffic were scaled to a range of 0 to 1 using Min-Max normalization. This ensured that all features contributed fairly to the demand function without scale-related bias.

Location ID: A unique numerical ID was assigned to each of the 14 parking lots based on their latitude and longitude. This allowed for the independent application of pricing logic to each location.

5.** Model Training & Evaluation**
In line with the project's constraints, three distinct pricing models were developed from scratch using only Python, NumPy, and Pandas. Evaluation was qualitative, focusing on creating smooth, explainable, and logical price fluctuations rather than using traditional quantitative metrics.

Model 1: Baseline Linear Model: A simple iterative model was implemented using the formula: Price(t+1) = Price(t) + α * (Occupancy_Rate). This model served as a basic reference point, adjusting the price based solely on how full the lot was.

Model 2: Demand-Based Price Function: A more sophisticated model was built around a multi-factor demand function. This function calculated a "demand score" as a weighted sum of several features, including occupancy_rate, Queue_length, Traffic, is_special_day, and vehicle_type_weight. The resulting score was normalized and used to adjust a base price of $10, with the final price being constrained to a range between 0.5x and 2x the base price to prevent erratic behavior.

Model 3: Competitive Pricing Model: This optional model added geographic intelligence. It first calculated a distance matrix for all parking lots using the Haversine formula to identify the nearest competitor for each lot. It then adjusted the price from Model 2 based on the competitor's price and the lot's current occupancy, simulating a competitive market environment.

6.** Results Visualization & Reporting**
The final results were demonstrated through a real-time simulation and live visualization, fulfilling the core requirements of the project.

Real-Time Simulation: The pricing models were integrated into a real-time data pipeline using the Pathway framework. A User-Defined Function (@pw.udf) was created to apply the demand-based pricing logic to a simulated stream of data from the cleaned CSV file. This setup continuously processed incoming data and wrote the original features along with the calculated price to an output CSV file.

Visualization: Bokeh was used to create a live-updating line chart directly within the Google Colab notebook. This chart read the output file from the Pathway simulation and dynamically plotted the demand_based_price against the timestamp for a specific parking lot. The plot automatically refreshed as new data was processed, providing a clear, visual representation of the model's performance and justifying its pricing decisions in response to changing conditions.

Key Conclusions: The project successfully demonstrated an end-to-end system for dynamic pricing in an urban parking context. By integrating from-scratch models with the Pathway and Bokeh libraries, it was possible to simulate a real-world, event-driven application where prices responded intelligently and smoothly to fluctuations in demand and competition.

## How to Run the Project

Follow these steps to set up and run the project on your local machine.

### Prerequisites

Ensure you have Python 3.8+ and `pip` installed on your system.

### Installation

1.  **Clone the repository:**
    ```
    git clone https://github.com/your-username/your-repository-name.git
    cd your-repository-name
    ```

### Usage

1.  **Launch Jupyter Notebook:**
    ```
    jupyter notebook
    ```

2.  **Open and run the notebook:**
    Navigate to the project notebook (e.g., `IITGsummer.ipynb`) and load the dataset.csv file and then run the cells sequentially to reproduce the analysis and results.
