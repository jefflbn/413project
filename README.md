# **Introduction**


We will use **LSTM model** to analyze the previous stock prices, updating the trainable weights during each
learning processes, utilizes LSTM cells to capture the weight of dependencies, and use our pre-trained models to
predict the close price of next day.


*   **Input:** opening price and closing price of **previous one week (7 days)**
*   **Output:** next day's closing price

Input and output are both float.

# **Model Figure**

Our LSTM model consists of serveral functions: the hidden state update function $f_W$ and the output function $g_U$. 


Input gate: $i_t = \sigma(W_{i}x_t  + U_{i}h_{t-1} + b_{i})$


Output gate: $o_t = \sigma(W_{r}x_t  + W_{r}h_{t-1} + b_{r})$


Hidden state: $h_t = o_t*\tanh(c_t)$


Forget gate: $r_t = \sigma(W_{r}x_t  + U_{r}h_{t-1} + b_{r})$


Cell gate: ${c_t} = \tanh(W_{ic}x_t + b_{ic} + W_{hc}h_{t-1} + b_{hc})$


Update cell state: $c_t = f_tc_{t-1} + i_t{c_t}$





# **Model Parameters**

We have **2 input parameters** from downloaded dataset:

*   opening price
*   closing price

And **Hyperparameters**:

*   Hidden size
*   Number of layers
*   Learning rate
*   Optimizer
*   Batch size
*   Sequence length

Hidden size, number of layers effects our **model building**,

Learning rate, optimizer, batch size effects the **training processes**,

Sequence length (7 days) will decide the effects of **dependencies weight of day-streak**, we will make it **7** (one week) in our project.

All of the hyperparameters would effects the performance of models.


# **Data Source**

We have our data from website:

https://finance.yahoo.com/quote/TSLA/history/


# **Data Summary**

The dataset is about the stock price from Mar 20, 2013 to Mar 20, 2023. 

This dataset could avoiding under-fitting since the dataset is sufficient, no empty data at all make sure that the quantity of data is enough and contains the stock prices of previous 10 years. We do not have to clean the data since
the data type is not string, no empty data and dollar sign.

The data is a CSV file and the variables we intend to use are all numerical values, we will import the data and select the column of opening price and closing
price as the input data, and we will have the next day's closing data as our target.




# **Data Transformation**

We have the 10 years of tesla stock price as our dataset.

After downloading the csv data, we implement *read_csv* function to extract the data,

We implement normalization to transform the input data distribution to a standard distribution， which could reducing the problems of vanishing or exploding gradients during training, also making the model more generalizable.

Then we transform our raw data into $x$ and $y$

where $x$ has shape of (7, 2), 7 represents previous 7 days' data, 2 stands for
opening price and closing price of that day.

and $y$ has shape of (1), which represents the next day's closing price.

Since the length of training data is 1510, we have total 1503 number of $x$ and
1503 $y$ corresponding to $x$.


# **Data Split**

We have 2517 rows of data, we will take

70% data as training data (1510) ,

10% data as validation data(216), for tuning hyperparameters and preventing overfitting.

20% data as testing data (504) for ensuring the model has the ability to generalize.

# **Hyperparamter Tuning**

We will tune hyperparameters like following:

*   Number of layers：2 (to improve the model's expressive power)
*   Hidden size：64 (common value that can effectively balance model performance and training time)
*   Optimizer：Adam (performs well on many problems)

The rest hyperparameters were chosen based on the dataset and our project.

*   Learning rate：0.001
*   Batch size：25
*   Sequence length：7


# **Quantitative Measures**

For measuring the performance of models,

We chose to use MSEloss as our metric because it not only tells us the magnitude of the prediction error, but also reflects the direction of the error.
Also, MSEloss is not sensitive to the outlier datas which fit our model better.

We did not use Cross-entropy since Cross-entropy is for evaluating classification problems, while stock prediction is a regression problem that requires predicting a continuous variable.

# **Quantitative and Qualitative Results**

Based on the training curve, the top half of the data fits our prediction before Tesla became famous. However, the stock will have a big change in a short time with the impression of news, government and so on. In the latter part, because of such changes, the prediction result is not very ideal.

# **Justification of Results**

Stock prices are time series data, and the trend of rise and fall and the change of trend have obvious time correlation. LSTM model can capture long-term dependence and complex relationships in time series data.

Besides, Predicting stock prices requires taking historical data, while LSTM models can capture historical information through memory units and gating mechanisms and make predictions based on historical information.

# **Ethical Consideration**

If our LSTM models could predict market trends and provide valuable information to investors, then our model can reduce risks of investors. 

However, if this technology is used for fraudulent purposes, it can negatively effect the market.

For **stock traders** and **investment advisors**, using LSTM model requires legal standards to ensure the profits of client.

For **relevant government employees**, they need to develop regulations and laws to supervise the use of LSTM technology to prevent it from being misused.

For **Limitation of our model**
Our model predict very well based on the historical dataset. However, our model could not capture the factors of the news, politics of the government and other data not based on the historical open and close price. The large bias significantly effect our prediction


# **Authors**

We have 2 members in group, and we shared the equal workload, including the steps of finding
dataset, data exploring, construct architecture of model mathematical formulas, analyzing the
feasibility, implementing in code, analyzing the training, evaluating the model, debugging,
encouraging each other, enjoying the completion of project, etc.