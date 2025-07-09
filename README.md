

# Logistics Operations Performance Analysis


## 1. Background

In the competitive logistics industry, operational efficiency and service quality are paramount. The ability to leverage data is critical for identifying process bottlenecks, optimizing resource allocation, and enhancing decision-making. This report presents a comprehensive analysis of a logistics dataset to uncover key performance logistic worker/courier and build predictive models that can support strategic operational improvements.

---

## 2. Objective

The primary objectives of this research are:
* To analyze the end-to-end logistics operational data to understand key characteristics and performance metrics.
* To identify the primary factors that influence the success or failure of delivery tasks.
* To develop predictive models to forecast task outcomes and durations.
* To segment couriers into distinct performance profiles to support targeted management and training initiatives.

---

## 3. Research Questions

This analysis seeks to answer the following key questions:
1.  What are the fundamental characteristics of the delivery operations regarding volume, timing, and outcomes?
2.  What are the most significant factors that determine whether a delivery task will succeed or fail?
3.  Can the duration of a delivery task be accurately predicted based on available data?
4.  Can couriers be grouped into meaningful performance-based segments to better understand team dynamics?

---

## 4. Analysis Method

The analysis was conducted through a multi-stage process involving data preparation, feature engineering, descriptive analysis, and predictive modeling.

### 4.1. Data Preparation
The initial phase involved cleaning the raw dataset. Records containing significant missing values across multiple columns, for which a clear reason could not be determined, were removed to ensure data integrity for subsequent analysis.

### 4.2. Feature Engineering
To enrich the dataset for analysis, several new features were engineered:
* **`taskDuration`**: Calculated as the time difference between task completion and creation to measure efficiency.
* **Time-based Features**: The day and hour were extracted from the task creation timestamp to analyze temporal patterns.
* **`is_cod`**: A binary flag (1/0) was created to clearly distinguish Cash on Delivery (COD) tasks from non-COD tasks.

### 4.3. Descriptive Analysis
A comprehensive descriptive analysis was performed using statistical summaries and data visualization to explore the underlying patterns and distributions within the data.

### 4.4. Modeling
Three distinct modeling approaches were implemented to address the research questions:
* **Classification**: A model was trained to predict task success or failure. Due to the imbalanced nature of the target variable, **Precision** was selected as the primary evaluation metric to minimize false positives.
* **Regression**: A model was developed to predict the `taskDuration`. Performance was evaluated using R-squared and Root Mean Squared Error (RMSE).
* **Clustering**: K-Means clustering was used to segment couriers into distinct groups based on aggregated performance metrics.

---

## 5. Results

### 5.1. Data Preparation & Feature Engineering
After removing incomplete records, new features (`taskDuration`, `day`, `hour`, `is_cod`) were successfully created and integrated into the dataset, forming the basis for the following analyses.

### 5.2. Descriptive Analysis

* **Task Status (Completion vs. Failure Rate)**: The analysis reveals a high-throughput process, with **90.9%** of all tasks reaching completion. However, this efficiency is undermined by a significant quality issue: **28.3%** of all completed tasks end in failure.

* **Task Duration**: The process is highly efficient for the vast majority of tasks. The distribution is heavily concentrated at the low end, indicating that a large volume of tasks completes very quickly (likely in under 50 minutes). The distribution is also **strongly right-skewed**, with a long tail, showing that while infrequent, a notable number of outlier tasks take an exceptionally long time to complete (e.g., exceeding 150-200 minutes).

* **Task Activity (Day and Hour)**: Task activity is overwhelmingly concentrated in the early morning, specifically between **7 AM and 9 AM**, with the absolute busiest time being **Wednesday at 7 AM**. Most weekdays follow a pattern of high morning activity that sharply drops off, with activity being nearly non-existent after 9 AM. Friday shows a different, lower-volume pattern, and weekend activity is minimal.

* **COD Amount**: The distribution of COD amounts is **strongly right-skewed**. The highest frequency of orders falls into the lowest value brackets, indicating that transactions with a large COD amount are very infrequent compared to the typical low-value purchase.

* **Package Weight**: The vast majority of packages are **very lightweight**, with the distribution being **extremely right-skewed**. Heavy packages are rare outliers, suggesting that logistics processes are primarily designed for small items.

### 5.3. Modeling

* **Classification (Success/Failure Prediction)**:
    * The best performing model was a fine-tuned **Random Forest**, which achieved a **Precision score of 0.9343**.
    * The most influential variables in determining the success of a task were **`taskDuration`** and **`cod.amount`**.

* **Regression (Task Duration Prediction)**:
    * The best model was **Linear Regression**, achieving a near-perfect **R-squared of 1.0** and an **RMSE of 0**.
    * The variables that most significantly influenced task duration were **`is_cod`**, **`taskCreatedHour`**, and **`task_status_label`**.

* **Clustering (Courier Segmentation)**:
    * The analysis identifies four distinct courier performance profiles:
        * **Cluster 3 (Yellow) - The Top Performers**: The ideal group, consistently achieving a very high success rate across all delivery durations.
        * **Cluster 0 (Purple) - The Fast but Inconsistent**: Quick couriers whose performance is unpredictable, with success rates ranging from 0% to 100%.
        * **Cluster 2 (Green) - The Slow & Polarized**: Couriers who take a long time per delivery and whose outcomes are extreme—either complete success or complete failure.
        * **Cluster 1 (Blue) - The Fast & Mediocre**: A unique segment of fast couriers who consistently perform at a mediocre ~50% success rate.

---

## 6. Disclaimer

The modeling results presented in this report are based on an initial analysis and are intended to demonstrate analytical possibilities. The reported performance metrics, particularly the perfect scores in the regression model, may be indicative of data leakage or overfitting and require further validation, cross-validation, and testing on a holdout dataset before being considered for production use. The findings should be treated as preliminary insights that warrant deeper investigation rather than definitive conclusions.