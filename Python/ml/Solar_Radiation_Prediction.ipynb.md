```ipynb
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# SOLAR RADIATION PREDICTION"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Link to the dataset: https://www.kaggle.com/dronio/SolarEnergy"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Importing Libraries"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "sns.set()\n",
    "from sklearn.model_selection import train_test_split\n",
    "from sklearn.linear_model import LinearRegression\n",
    "from sklearn.metrics import r2_score\n",
    "from sklearn import preprocessing\n",
    "import tkinter as tk \n",
    "from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Getting our Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>UNIXTime</th>\n",
       "      <th>Data</th>\n",
       "      <th>Time</th>\n",
       "      <th>Radiation</th>\n",
       "      <th>Temperature</th>\n",
       "      <th>Pressure</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>WindDirection(Degrees)</th>\n",
       "      <th>Speed</th>\n",
       "      <th>TimeSunRise</th>\n",
       "      <th>TimeSunSet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1475229326</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:55:26</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>30.46</td>\n",
       "      <td>59</td>\n",
       "      <td>177.39</td>\n",
       "      <td>5.62</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1475229023</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:50:23</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>30.46</td>\n",
       "      <td>58</td>\n",
       "      <td>176.78</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1475228726</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:45:26</td>\n",
       "      <td>1.23</td>\n",
       "      <td>48</td>\n",
       "      <td>30.46</td>\n",
       "      <td>57</td>\n",
       "      <td>158.75</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1475228421</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:40:21</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>30.46</td>\n",
       "      <td>60</td>\n",
       "      <td>137.71</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1475228124</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:35:24</td>\n",
       "      <td>1.17</td>\n",
       "      <td>48</td>\n",
       "      <td>30.46</td>\n",
       "      <td>62</td>\n",
       "      <td>104.95</td>\n",
       "      <td>5.62</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32681</th>\n",
       "      <td>1480587604</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:20:04</td>\n",
       "      <td>1.22</td>\n",
       "      <td>44</td>\n",
       "      <td>30.43</td>\n",
       "      <td>102</td>\n",
       "      <td>145.42</td>\n",
       "      <td>6.75</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32682</th>\n",
       "      <td>1480587301</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:15:01</td>\n",
       "      <td>1.17</td>\n",
       "      <td>44</td>\n",
       "      <td>30.42</td>\n",
       "      <td>102</td>\n",
       "      <td>117.78</td>\n",
       "      <td>6.75</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32683</th>\n",
       "      <td>1480587001</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:10:01</td>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>30.42</td>\n",
       "      <td>102</td>\n",
       "      <td>145.19</td>\n",
       "      <td>9.00</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32684</th>\n",
       "      <td>1480586702</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:05:02</td>\n",
       "      <td>1.23</td>\n",
       "      <td>44</td>\n",
       "      <td>30.42</td>\n",
       "      <td>101</td>\n",
       "      <td>164.19</td>\n",
       "      <td>7.87</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32685</th>\n",
       "      <td>1480586402</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:00:02</td>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>30.43</td>\n",
       "      <td>101</td>\n",
       "      <td>83.59</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>32686 rows × 11 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "         UNIXTime                   Data      Time  Radiation  Temperature  \\\n",
       "0      1475229326  9/29/2016 12:00:00 AM  23:55:26       1.21           48   \n",
       "1      1475229023  9/29/2016 12:00:00 AM  23:50:23       1.21           48   \n",
       "2      1475228726  9/29/2016 12:00:00 AM  23:45:26       1.23           48   \n",
       "3      1475228421  9/29/2016 12:00:00 AM  23:40:21       1.21           48   \n",
       "4      1475228124  9/29/2016 12:00:00 AM  23:35:24       1.17           48   \n",
       "...           ...                    ...       ...        ...          ...   \n",
       "32681  1480587604  12/1/2016 12:00:00 AM  00:20:04       1.22           44   \n",
       "32682  1480587301  12/1/2016 12:00:00 AM  00:15:01       1.17           44   \n",
       "32683  1480587001  12/1/2016 12:00:00 AM  00:10:01       1.20           44   \n",
       "32684  1480586702  12/1/2016 12:00:00 AM  00:05:02       1.23           44   \n",
       "32685  1480586402  12/1/2016 12:00:00 AM  00:00:02       1.20           44   \n",
       "\n",
       "       Pressure  Humidity  WindDirection(Degrees)  Speed TimeSunRise  \\\n",
       "0         30.46        59                  177.39   5.62    06:13:00   \n",
       "1         30.46        58                  176.78   3.37    06:13:00   \n",
       "2         30.46        57                  158.75   3.37    06:13:00   \n",
       "3         30.46        60                  137.71   3.37    06:13:00   \n",
       "4         30.46        62                  104.95   5.62    06:13:00   \n",
       "...         ...       ...                     ...    ...         ...   \n",
       "32681     30.43       102                  145.42   6.75    06:41:00   \n",
       "32682     30.42       102                  117.78   6.75    06:41:00   \n",
       "32683     30.42       102                  145.19   9.00    06:41:00   \n",
       "32684     30.42       101                  164.19   7.87    06:41:00   \n",
       "32685     30.43       101                   83.59   3.37    06:41:00   \n",
       "\n",
       "      TimeSunSet  \n",
       "0       18:13:00  \n",
       "1       18:13:00  \n",
       "2       18:13:00  \n",
       "3       18:13:00  \n",
       "4       18:13:00  \n",
       "...          ...  \n",
       "32681   17:42:00  \n",
       "32682   17:42:00  \n",
       "32683   17:42:00  \n",
       "32684   17:42:00  \n",
       "32685   17:42:00  \n",
       "\n",
       "[32686 rows x 11 columns]"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df = pd.read_csv(r'SolarPrediction.csv')\n",
    "df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "UNIXTime                  False\n",
       "Data                      False\n",
       "Time                      False\n",
       "Radiation                 False\n",
       "Temperature               False\n",
       "Pressure                  False\n",
       "Humidity                  False\n",
       "WindDirection(Degrees)    False\n",
       "Speed                     False\n",
       "TimeSunRise               False\n",
       "TimeSunSet                False\n",
       "dtype: bool"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.isnull().any() # to see number of missing elements, use df.isnull().sum()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Calculating VIF"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "variables = df[['Temperature','Pressure','Humidity','Speed','WindDirection(Degrees)']]\n",
    "vif = pd.DataFrame()\n",
    "vif['VIF'] = [variance_inflation_factor(variables.values, i) for i in range(variables.shape[1])]\n",
    "vif['Features'] = variables.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>VIF</th>\n",
       "      <th>Features</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>82.077639</td>\n",
       "      <td>Temperature</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>129.064887</td>\n",
       "      <td>Pressure</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10.731528</td>\n",
       "      <td>Humidity</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4.445383</td>\n",
       "      <td>Speed</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>4.292385</td>\n",
       "      <td>WindDirection(Degrees)</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "          VIF                Features\n",
       "0   82.077639             Temperature\n",
       "1  129.064887                Pressure\n",
       "2   10.731528                Humidity\n",
       "3    4.445383                   Speed\n",
       "4    4.292385  WindDirection(Degrees)"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "vif\n",
    "# The VIF for Pressure is so high, that it influences Temperature's variance to swell, hence pressure is dropped and VIF\n",
    "# calculated again"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>UNIXTime</th>\n",
       "      <th>Data</th>\n",
       "      <th>Time</th>\n",
       "      <th>Radiation</th>\n",
       "      <th>Temperature</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>WindDirection(Degrees)</th>\n",
       "      <th>Speed</th>\n",
       "      <th>TimeSunRise</th>\n",
       "      <th>TimeSunSet</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1475229326</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:55:26</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>59</td>\n",
       "      <td>177.39</td>\n",
       "      <td>5.62</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1475229023</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:50:23</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>58</td>\n",
       "      <td>176.78</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1475228726</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:45:26</td>\n",
       "      <td>1.23</td>\n",
       "      <td>48</td>\n",
       "      <td>57</td>\n",
       "      <td>158.75</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1475228421</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:40:21</td>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>60</td>\n",
       "      <td>137.71</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1475228124</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:35:24</td>\n",
       "      <td>1.17</td>\n",
       "      <td>48</td>\n",
       "      <td>62</td>\n",
       "      <td>104.95</td>\n",
       "      <td>5.62</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32681</th>\n",
       "      <td>1480587604</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:20:04</td>\n",
       "      <td>1.22</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.42</td>\n",
       "      <td>6.75</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32682</th>\n",
       "      <td>1480587301</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:15:01</td>\n",
       "      <td>1.17</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>117.78</td>\n",
       "      <td>6.75</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32683</th>\n",
       "      <td>1480587001</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:10:01</td>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.19</td>\n",
       "      <td>9.00</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32684</th>\n",
       "      <td>1480586702</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:05:02</td>\n",
       "      <td>1.23</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>164.19</td>\n",
       "      <td>7.87</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32685</th>\n",
       "      <td>1480586402</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:00:02</td>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>83.59</td>\n",
       "      <td>3.37</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>32686 rows × 10 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "         UNIXTime                   Data      Time  Radiation  Temperature  \\\n",
       "0      1475229326  9/29/2016 12:00:00 AM  23:55:26       1.21           48   \n",
       "1      1475229023  9/29/2016 12:00:00 AM  23:50:23       1.21           48   \n",
       "2      1475228726  9/29/2016 12:00:00 AM  23:45:26       1.23           48   \n",
       "3      1475228421  9/29/2016 12:00:00 AM  23:40:21       1.21           48   \n",
       "4      1475228124  9/29/2016 12:00:00 AM  23:35:24       1.17           48   \n",
       "...           ...                    ...       ...        ...          ...   \n",
       "32681  1480587604  12/1/2016 12:00:00 AM  00:20:04       1.22           44   \n",
       "32682  1480587301  12/1/2016 12:00:00 AM  00:15:01       1.17           44   \n",
       "32683  1480587001  12/1/2016 12:00:00 AM  00:10:01       1.20           44   \n",
       "32684  1480586702  12/1/2016 12:00:00 AM  00:05:02       1.23           44   \n",
       "32685  1480586402  12/1/2016 12:00:00 AM  00:00:02       1.20           44   \n",
       "\n",
       "       Humidity  WindDirection(Degrees)  Speed TimeSunRise TimeSunSet  \n",
       "0            59                  177.39   5.62    06:13:00   18:13:00  \n",
       "1            58                  176.78   3.37    06:13:00   18:13:00  \n",
       "2            57                  158.75   3.37    06:13:00   18:13:00  \n",
       "3            60                  137.71   3.37    06:13:00   18:13:00  \n",
       "4            62                  104.95   5.62    06:13:00   18:13:00  \n",
       "...         ...                     ...    ...         ...        ...  \n",
       "32681       102                  145.42   6.75    06:41:00   17:42:00  \n",
       "32682       102                  117.78   6.75    06:41:00   17:42:00  \n",
       "32683       102                  145.19   9.00    06:41:00   17:42:00  \n",
       "32684       101                  164.19   7.87    06:41:00   17:42:00  \n",
       "32685       101                   83.59   3.37    06:41:00   17:42:00  \n",
       "\n",
       "[32686 rows x 10 columns]"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.drop('Pressure', axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "variables_1 = df[['Temperature','Humidity','Speed','WindDirection(Degrees)']]\n",
    "vif_1 = pd.DataFrame()\n",
    "vif_1['VIF'] = [variance_inflation_factor(variables_1.values, i) for i in range(variables_1.shape[1])]\n",
    "vif_1['Features'] = variables_1.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>VIF</th>\n",
       "      <th>Features</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>11.246320</td>\n",
       "      <td>Temperature</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>7.372081</td>\n",
       "      <td>Humidity</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>4.101280</td>\n",
       "      <td>Speed</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>3.685892</td>\n",
       "      <td>WindDirection(Degrees)</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         VIF                Features\n",
       "0  11.246320             Temperature\n",
       "1   7.372081                Humidity\n",
       "2   4.101280                   Speed\n",
       "3   3.685892  WindDirection(Degrees)"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "vif_1\n",
    "\n",
    "# Though the VIF of Temp is still high, this is mainly because of inclusion of humidity since both have atleast some amount\n",
    "# of correlation amongst each other. Also, dropping temp would lead to a deficit of a key factor used to check the radiation.\n",
    "# Hence, we proceed with these four features itself."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Data Preprocessing"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['UNIXTime', 'Data', 'Time', 'Radiation', 'Temperature', 'Pressure',\n",
       "       'Humidity', 'WindDirection(Degrees)', 'Speed', 'TimeSunRise',\n",
       "       'TimeSunSet'], dtype=object)"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.columns.values"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "cols = ['Radiation', 'Temperature', \n",
    "       'Humidity', 'WindDirection(Degrees)', 'Speed', 'Pressure','TimeSunRise',\n",
    "       'TimeSunSet', 'UNIXTime', 'Data', 'Time']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Radiation</th>\n",
       "      <th>Temperature</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>WindDirection(Degrees)</th>\n",
       "      <th>Speed</th>\n",
       "      <th>Pressure</th>\n",
       "      <th>TimeSunRise</th>\n",
       "      <th>TimeSunSet</th>\n",
       "      <th>UNIXTime</th>\n",
       "      <th>Data</th>\n",
       "      <th>Time</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>59</td>\n",
       "      <td>177.39</td>\n",
       "      <td>5.62</td>\n",
       "      <td>30.46</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "      <td>1475229326</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:55:26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>58</td>\n",
       "      <td>176.78</td>\n",
       "      <td>3.37</td>\n",
       "      <td>30.46</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "      <td>1475229023</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:50:23</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1.23</td>\n",
       "      <td>48</td>\n",
       "      <td>57</td>\n",
       "      <td>158.75</td>\n",
       "      <td>3.37</td>\n",
       "      <td>30.46</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "      <td>1475228726</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:45:26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>60</td>\n",
       "      <td>137.71</td>\n",
       "      <td>3.37</td>\n",
       "      <td>30.46</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "      <td>1475228421</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:40:21</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1.17</td>\n",
       "      <td>48</td>\n",
       "      <td>62</td>\n",
       "      <td>104.95</td>\n",
       "      <td>5.62</td>\n",
       "      <td>30.46</td>\n",
       "      <td>06:13:00</td>\n",
       "      <td>18:13:00</td>\n",
       "      <td>1475228124</td>\n",
       "      <td>9/29/2016 12:00:00 AM</td>\n",
       "      <td>23:35:24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32681</th>\n",
       "      <td>1.22</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.42</td>\n",
       "      <td>6.75</td>\n",
       "      <td>30.43</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "      <td>1480587604</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:20:04</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32682</th>\n",
       "      <td>1.17</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>117.78</td>\n",
       "      <td>6.75</td>\n",
       "      <td>30.42</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "      <td>1480587301</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:15:01</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32683</th>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.19</td>\n",
       "      <td>9.00</td>\n",
       "      <td>30.42</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "      <td>1480587001</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:10:01</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32684</th>\n",
       "      <td>1.23</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>164.19</td>\n",
       "      <td>7.87</td>\n",
       "      <td>30.42</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "      <td>1480586702</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:05:02</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32685</th>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>83.59</td>\n",
       "      <td>3.37</td>\n",
       "      <td>30.43</td>\n",
       "      <td>06:41:00</td>\n",
       "      <td>17:42:00</td>\n",
       "      <td>1480586402</td>\n",
       "      <td>12/1/2016 12:00:00 AM</td>\n",
       "      <td>00:00:02</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>32686 rows × 11 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       Radiation  Temperature  Humidity  WindDirection(Degrees)  Speed  \\\n",
       "0           1.21           48        59                  177.39   5.62   \n",
       "1           1.21           48        58                  176.78   3.37   \n",
       "2           1.23           48        57                  158.75   3.37   \n",
       "3           1.21           48        60                  137.71   3.37   \n",
       "4           1.17           48        62                  104.95   5.62   \n",
       "...          ...          ...       ...                     ...    ...   \n",
       "32681       1.22           44       102                  145.42   6.75   \n",
       "32682       1.17           44       102                  117.78   6.75   \n",
       "32683       1.20           44       102                  145.19   9.00   \n",
       "32684       1.23           44       101                  164.19   7.87   \n",
       "32685       1.20           44       101                   83.59   3.37   \n",
       "\n",
       "       Pressure TimeSunRise TimeSunSet    UNIXTime                   Data  \\\n",
       "0         30.46    06:13:00   18:13:00  1475229326  9/29/2016 12:00:00 AM   \n",
       "1         30.46    06:13:00   18:13:00  1475229023  9/29/2016 12:00:00 AM   \n",
       "2         30.46    06:13:00   18:13:00  1475228726  9/29/2016 12:00:00 AM   \n",
       "3         30.46    06:13:00   18:13:00  1475228421  9/29/2016 12:00:00 AM   \n",
       "4         30.46    06:13:00   18:13:00  1475228124  9/29/2016 12:00:00 AM   \n",
       "...         ...         ...        ...         ...                    ...   \n",
       "32681     30.43    06:41:00   17:42:00  1480587604  12/1/2016 12:00:00 AM   \n",
       "32682     30.42    06:41:00   17:42:00  1480587301  12/1/2016 12:00:00 AM   \n",
       "32683     30.42    06:41:00   17:42:00  1480587001  12/1/2016 12:00:00 AM   \n",
       "32684     30.42    06:41:00   17:42:00  1480586702  12/1/2016 12:00:00 AM   \n",
       "32685     30.43    06:41:00   17:42:00  1480586402  12/1/2016 12:00:00 AM   \n",
       "\n",
       "           Time  \n",
       "0      23:55:26  \n",
       "1      23:50:23  \n",
       "2      23:45:26  \n",
       "3      23:40:21  \n",
       "4      23:35:24  \n",
       "...         ...  \n",
       "32681  00:20:04  \n",
       "32682  00:15:01  \n",
       "32683  00:10:01  \n",
       "32684  00:05:02  \n",
       "32685  00:00:02  \n",
       "\n",
       "[32686 rows x 11 columns]"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data_ready = df[cols] # rearranging columns\n",
    "data_ready"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Radiation</th>\n",
       "      <th>Temperature</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>WindDirection(Degrees)</th>\n",
       "      <th>Speed</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>59</td>\n",
       "      <td>177.39</td>\n",
       "      <td>5.62</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>58</td>\n",
       "      <td>176.78</td>\n",
       "      <td>3.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1.23</td>\n",
       "      <td>48</td>\n",
       "      <td>57</td>\n",
       "      <td>158.75</td>\n",
       "      <td>3.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1.21</td>\n",
       "      <td>48</td>\n",
       "      <td>60</td>\n",
       "      <td>137.71</td>\n",
       "      <td>3.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1.17</td>\n",
       "      <td>48</td>\n",
       "      <td>62</td>\n",
       "      <td>104.95</td>\n",
       "      <td>5.62</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32681</th>\n",
       "      <td>1.22</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.42</td>\n",
       "      <td>6.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32682</th>\n",
       "      <td>1.17</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>117.78</td>\n",
       "      <td>6.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32683</th>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>102</td>\n",
       "      <td>145.19</td>\n",
       "      <td>9.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32684</th>\n",
       "      <td>1.23</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>164.19</td>\n",
       "      <td>7.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32685</th>\n",
       "      <td>1.20</td>\n",
       "      <td>44</td>\n",
       "      <td>101</td>\n",
       "      <td>83.59</td>\n",
       "      <td>3.37</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>32686 rows × 5 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       Radiation  Temperature  Humidity  WindDirection(Degrees)  Speed\n",
       "0           1.21           48        59                  177.39   5.62\n",
       "1           1.21           48        58                  176.78   3.37\n",
       "2           1.23           48        57                  158.75   3.37\n",
       "3           1.21           48        60                  137.71   3.37\n",
       "4           1.17           48        62                  104.95   5.62\n",
       "...          ...          ...       ...                     ...    ...\n",
       "32681       1.22           44       102                  145.42   6.75\n",
       "32682       1.17           44       102                  117.78   6.75\n",
       "32683       1.20           44       102                  145.19   9.00\n",
       "32684       1.23           44       101                  164.19   7.87\n",
       "32685       1.20           44       101                   83.59   3.37\n",
       "\n",
       "[32686 rows x 5 columns]"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# dropping columns that are not required according to our analysis\n",
    "data_ready = data_ready.drop(['Pressure','TimeSunRise','TimeSunSet','UNIXTime','Data','Time'], axis=1)\n",
    "data_ready"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Splitting for Training and Testing"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([[-0.50043927, -0.61625306,  0.40761954, -0.17873758],\n",
       "       [-0.50043927, -0.65472966,  0.40028483, -0.82335911],\n",
       "       [-0.50043927, -0.69320626,  0.1834901 , -0.82335911],\n",
       "       [-0.50043927, -0.57777646, -0.06949721, -0.82335911],\n",
       "       [-0.50043927, -0.50082325, -0.46340711, -0.17873758]])"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data = data_ready.values\n",
    "X, y = data[:,1:], data[:,0]  # splitting and getting the independent attributes, X and dependent attribute y\n",
    "X = preprocessing.StandardScaler().fit_transform(X)\n",
    "X[0:5]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.30, random_state=0)  # splitting in the ratio 70:30"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Linear Regression Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = LinearRegression()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "LinearRegression()"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "model.fit(X_train, y_train)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Making Predictions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [],
   "source": [
    "y_hat = model.predict(X_train)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "C:\\Users\\DELL\\anaconda3\\lib\\site-packages\\seaborn\\distributions.py:2551: FutureWarning: `distplot` is a deprecated function and will be removed in a future version. Please adapt your code to use either `displot` (a figure-level function with similar flexibility) or `histplot` (an axes-level function for histograms).\n",
      "  warnings.warn(msg, FutureWarning)\n"
     ]
    },
    {
     "data": {
      "image/png": "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VyvW215ee+019uzZA9Q+urBs2TKefvppDh48SEBAbWX1X/3qV0yaNKnJAsKe9MADD1BcXOx+neeeew6r1cr69eux2WxMnTqVJUuWAE0XOPaUDz74gPfee8+9nJWVxaxZs6iqqvL6+2exWLj//vt54403iI2NJTk5uUXvWVNFoT0V37Zt29iyZQsqlYphw4axZs0a/Pz8eO211/joo48IDq6tPvLjH/+Y+fPnty4+RTSb0+lUfvrTnyp///vfFUVRlLfeekt54403Gmz35z//WXn22WcVRVGUAwcOKHPnzvVoXM8995yya9euBu1r1qxR3nzzTUVRFGXHjh3K4sWLvRLf7bffrqSnpyuKoigffPCB8vOf/1xRFN95/y6Vl5en3H777UpJSYlitVqVhIQE5fTp0+32+nW++eYb5b777lNsNptSU1OjLFiwQNm3b58yY8YMxWw2N9h++vTpyvfff68oiqI8/fTTytatWz0an8vlUsaPH6/Y7XZ3W1VVlTJx4kTl4sWLit1uVx566CHliy++8Ep8lzp16pQyadIkpaioyOvv36FDh5QZM2YoQ4cOVTIzM1v1nj366KPKJ598oiiKorz22mvKhg0bPBZfRkaGMmnSJKWiokJxuVzKsmXLlL/85S+KoijKY489pnz33XcNjtGa+KRrrgU++ugjAgICSEhIACAtLY2vv/6ahIQEfv7zn5ObmwvAF198wcyZM4H6RVg9JS0tjR07dpCQkMBTTz1FWVmZO466WGfMmMGXX37pLhLbXvHV1NSwePFiBg8eDMCgQYPc75OvvH+XurRYr8FgcBfrbW8mk4nly5fj5+eHTqejf//+5OTkkJOTw4oVK0hISODVV1/F5XI1WkDY0zFnZGQA8NBDDzFz5kzee+89jhw5Qu/evenZsydarZaEhASSkpK8Et+lfvvb37JkyRICAgK8/v5t376d1atXuyu/tPQ9s9vtpKSkEB8f75FYL4/Pz8+P1atXYzQaUalUDBw40P27ePToUd58800SEhJ47rnnsNlsrY5PElEzOZ1O3njjDX7961+724KCgnjggQfYtWsXEydOdF9SN1WE1VNMJhO/+MUv+Pvf/050dDTPPfdcgziaUyTWE/z8/Jg1axZQW1/wtdde46677gJ85/27VGPFeuuK7ranAQMGuP8InT9/nj179nDbbbcxduxY1q1bx/bt20lNTeXDDz9ssoCwJ5WXl3PLLbfwpz/9iXfeeYe//e1v5OTkNPreeSO+OsnJyVRXVzN16lQKCwu9/v6tXbuWuLg493JTP2+tKQrtifhiYmK49dZbASguLmbr1q3ceeedWK1WhgwZwtKlS9mxYwfl5eVs2rSp1fHJPaJL7Nmzh/Xr19dr69evH++88w5fffUVffr0YdCgQe51dX/wAX7yk5/w4osvUlFR4bEirFeKr84jjzzCpEmTGt1f8XCR2CvFV1NTw/Lly3E4HDz22GNA+79/zXG1Yr3t7fTp0zz22GMsW7aMfv36uadHgdp7NDt37qR///7tHvMNN9zADTfc4F6eM2cOr776KjfeeGODOLz5nv7tb3/jf/7nfwDo2bOnz7x/dZp6b9qiKHRbMpvNPPLII9x7773cfPPNAGzevNm9/qGHHmLFihXMmzevVfFJIrrE1KlTmTp1aqPr/vnPfzJt2jT3ssvl4s033+TRRx9Fo9G42zUajbsIa69evYC2K8LaWHwVFRW88847/OxnPwNqf2Dr4qkrEhsVFdVokdj2iA/AarXy+OOPExISwuuvv45Op/PK+9ccUVFRpKamupcvL9bbng4ePMiiRYtYsWIF06dP5+TJk5w/f97d7aEoClqttskCwp6UmpqK3W7nlltucccSExPTaKFjb8QHtd3CKSkp/O53vwPwqfevTlPFoZtTFFqj0bTLz+fZs2d55JFHeOCBB9wDt3JyckhOTmbOnDnAf9/L1sYnXXPNdOjQoXqXrGq1mk8//ZS9e/cCtcVZR44cicFgaNcirAaDgT//+c8cPnwYgPfee899ReQrRWKXLl1K7969efnll/Hz8wN85/273NWK9baX3NxcfvnLX7Jx40amT58O1P6yr1u3jrKyMux2O9u2bWPSpElNFhD2pIqKCjZs2IDNZsNisbBjxw7+93//l3PnznHhwgWcTieffPIJEyZM8Ep8UJt4+vTpg8FgAHzr/aszcuTIFr1nVyoK7QkWi4WHH36YxYsX1xs97O/vz+9//3syMzNRFIWtW7cyadKkVscnteaaaeTIkRw4cAC9Xu9uO336NM8++ywVFRWEhYWxYcMGoqOj270Ia2pqKmvXrqW6upo+ffqwYcMGgoKCfKJI7PHjx7nnnnu47rrr3P3GERERbN682Wfev8s1Vqy3vT3//PN89NFH7qtCgPvvvx+Xy8XWrVtxOBxMnjyZp556CqDJAsKe9PLLL7N3715cLhfz5s3jwQcfZP/+/e6hyBMnTuTpp59GpVJ5Jb7du3fz6aef8tJLL7nbtm7d6hPv3x133MG7775LbGxsi9+zpopCeyK+f/7znw0en7jjjjtYvHgxe/fu5Y9//CN2u53Ro0e7h3W3Jj5JREIIIbxKuuaEEEJ4lSQiIYQQXiWJSAghhFdJIhJCCOFVkoiEEEJ4lSQiIYQQXiWJSAghhFdJIhJCCOFV/x8VeaJR2RA8nAAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.distplot(y_train-y_hat)\n",
    "plt.title('Residuals PDF')\n",
    "plt.show()\n",
    "# further on, when we create the training dataset's summary, we'll use y_train as target and y_hat(i.e., predicted X_train) \n",
    "# would be the Predictions\n",
    "\n",
    "# The residuals are normally distributed."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "206.87895083047556"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "model.intercept_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([ 2.30634392e+02,  6.22531457e-02, -1.46045072e+01,  3.18182571e+01])"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "model.coef_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [],
   "source": [
    "y_pred = model.predict(X_test) # making predictions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "numpy.ndarray"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(X)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_new = pd.DataFrame(X)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "pandas.core.frame.DataFrame"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(X_new)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_new_renamed = X_new.rename(columns={0:'Temparature',1:'Humidity',2:'WindDirection(Degrees)',3:'Speed'})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Temparature</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>WindDirection(Degrees)</th>\n",
       "      <th>Speed</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>-0.500439</td>\n",
       "      <td>-0.616253</td>\n",
       "      <td>0.407620</td>\n",
       "      <td>-0.178738</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>-0.500439</td>\n",
       "      <td>-0.654730</td>\n",
       "      <td>0.400285</td>\n",
       "      <td>-0.823359</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>-0.500439</td>\n",
       "      <td>-0.693206</td>\n",
       "      <td>0.183490</td>\n",
       "      <td>-0.823359</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>-0.500439</td>\n",
       "      <td>-0.577776</td>\n",
       "      <td>-0.069497</td>\n",
       "      <td>-0.823359</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>-0.500439</td>\n",
       "      <td>-0.500823</td>\n",
       "      <td>-0.463407</td>\n",
       "      <td>-0.178738</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32681</th>\n",
       "      <td>-1.145490</td>\n",
       "      <td>1.038241</td>\n",
       "      <td>0.023209</td>\n",
       "      <td>0.145006</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32682</th>\n",
       "      <td>-1.145490</td>\n",
       "      <td>1.038241</td>\n",
       "      <td>-0.309138</td>\n",
       "      <td>0.145006</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32683</th>\n",
       "      <td>-1.145490</td>\n",
       "      <td>1.038241</td>\n",
       "      <td>0.020443</td>\n",
       "      <td>0.789627</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32684</th>\n",
       "      <td>-1.145490</td>\n",
       "      <td>0.999764</td>\n",
       "      <td>0.248901</td>\n",
       "      <td>0.465884</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>32685</th>\n",
       "      <td>-1.145490</td>\n",
       "      <td>0.999764</td>\n",
       "      <td>-0.720242</td>\n",
       "      <td>-0.823359</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>32686 rows × 4 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "       Temparature  Humidity  WindDirection(Degrees)     Speed\n",
       "0        -0.500439 -0.616253                0.407620 -0.178738\n",
       "1        -0.500439 -0.654730                0.400285 -0.823359\n",
       "2        -0.500439 -0.693206                0.183490 -0.823359\n",
       "3        -0.500439 -0.577776               -0.069497 -0.823359\n",
       "4        -0.500439 -0.500823               -0.463407 -0.178738\n",
       "...            ...       ...                     ...       ...\n",
       "32681    -1.145490  1.038241                0.023209  0.145006\n",
       "32682    -1.145490  1.038241               -0.309138  0.145006\n",
       "32683    -1.145490  1.038241                0.020443  0.789627\n",
       "32684    -1.145490  0.999764                0.248901  0.465884\n",
       "32685    -1.145490  0.999764               -0.720242 -0.823359\n",
       "\n",
       "[32686 rows x 4 columns]"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X_new_renamed"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Model Summary"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "reg_summary = pd.DataFrame(X_new_renamed.columns.values, columns=['Features'])\n",
    "reg_summary['Weights/Coeffs'] = model.coef_"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Features</th>\n",
       "      <th>Weights/Coeffs</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Temparature</td>\n",
       "      <td>230.634392</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Humidity</td>\n",
       "      <td>0.062253</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>WindDirection(Degrees)</td>\n",
       "      <td>-14.604507</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Speed</td>\n",
       "      <td>31.818257</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                 Features  Weights/Coeffs\n",
       "0             Temparature      230.634392\n",
       "1                Humidity        0.062253\n",
       "2  WindDirection(Degrees)      -14.604507\n",
       "3                   Speed       31.818257"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "reg_summary"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [],
   "source": [
    "new_row = {'Features':'Slope Intercept', 'Weights/Coeffs':model.intercept_}"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Features</th>\n",
       "      <th>Weights/Coeffs</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Temparature</td>\n",
       "      <td>230.634392</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Humidity</td>\n",
       "      <td>0.062253</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>WindDirection(Degrees)</td>\n",
       "      <td>-14.604507</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Speed</td>\n",
       "      <td>31.818257</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>Slope Intercept</td>\n",
       "      <td>206.878951</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                 Features  Weights/Coeffs\n",
       "0             Temparature      230.634392\n",
       "1                Humidity        0.062253\n",
       "2  WindDirection(Degrees)      -14.604507\n",
       "3                   Speed       31.818257\n",
       "4         Slope Intercept      206.878951"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "reg_summary.append(new_row, ignore_index=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### The Table showing coefficients of the regression equation."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Features</th>\n",
       "      <th>Weights/Coeffs</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Temparature</td>\n",
       "      <td>230.634392</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Humidity</td>\n",
       "      <td>0.062253</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>WindDirection(Degrees)</td>\n",
       "      <td>-14.604507</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Speed</td>\n",
       "      <td>31.818257</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                 Features  Weights/Coeffs\n",
       "0             Temparature      230.634392\n",
       "1                Humidity        0.062253\n",
       "2  WindDirection(Degrees)      -14.604507\n",
       "3                   Speed       31.818257"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "reg_summary"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### R Squared"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.5453853232892336"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "r2_score(y_test, y_pred)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [],
   "source": [
    "y_hat_test = model.predict(X_test)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Visualizing Accuracy"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAZEAAAEiCAYAAAA4f++MAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8vihELAAAACXBIWXMAAAsTAAALEwEAmpwYAAEAAElEQVR4nOy9edRlV1nn/9nDme7wTjUkIeYXoKFiy5CAYLDBIBiBQCIS1DYgadvl6sZIbBFRG2gGGxSVNi3YKCg2CxDbIDJDAA2yRGRcLJHJASFkoqZ3uveeaU+/P/a5t96qeiupqlRSqXC/a7FInffce/c599z97P08z/f7FSGEwBxzzDHHHHOcBOTpHsAcc8wxxxxnLuZBZI455phjjpPGPIjMMcccc8xx0pgHkTnmmGOOOU4a8yAyxxxzzDHHSWMeROaYY4455jhpzIPIHMfE6173Oi644ILj+t8Tn/jEu2UM3/jGN/jQhz502LELLriApz/96XfL5x0vPvGJT/DFL35x9u9Pf/rTXHDBBbzqVa+6x8fyspe9jAsuuICnPe1p9/hnzzGHPt0DmOPei+/7vu/jec973mHH3vWud3Hrrbdy9dVXs7CwMDs+HA5P+ed/7Wtf48d+7Me46qqruOyyy2bHn/e857Fz585T/nnHi7e//e284hWv4P/8n/9z2sYwRdu23HDDDRRFwb/+67/yhS98gUc84hGne1hzfAdhHkTmOCYuvvhiLr744sOOfeYzn+HWW2/lP/2n/8R3fdd33a2fv7GxgTHmqOPXXnvt3fq5d4aDBw+e1s/fio997GOsr69z7bXX8rrXvY53vOMd8yAyxz2KeTprjjnOYLz73e9GSsmzn/1sHvjAB/KhD32I8Xh8uoc1x3cQ5kFkjlOG8XjMa17zGi699FIe+tCH8gM/8AO87GUv23bl/ta3vpUrr7ySRzziETzykY/kWc961mG1j9e97nVcffXVALzlLW/hggsu4NOf/jRwdE1kWrv5+te/zu/+7u/ygz/4gzz0oQ/laU97Gn/2Z3921GdPJhN+53d+hyc+8Yk8/OEP58orr+TGG2/kxS9+MRdccMEdXuNznvMcfv/3fx+An//5n9/2/He96138yI/8CA972MP4gR/4AX7zN3+TqqqOOu/LX/4y11xzDRdffDEPf/jDefrTn86f/dmfcbxKRKurq/zt3/4tD3nIQ1heXuapT30qZVnywQ9+8Jiv+au/+iue85zn8KhHPYqLL76Yn/7pn+azn/3sCZ93RzWgX/u1X+OCCy7gq1/9KgC33HILF1xwAb/3e7/HK1/5Si666CIuvvji2fe9urrKb/3Wb3HZZZdx4YUXcuGFF/K0pz2NP/zDP8Rae9T7v/Od7+THf/zHecQjHsFjH/tYrrnmGr72ta8BcOutt/Ld3/3dXHXVVdte/9VXX82FF144D7SnEPMgMscpwWg04qqrruKP/uiP+K7v+i6uvvpqHvGIR3D99dfz4z/+4+zbt2927hvf+EZe+cpXAvCTP/mTXHnllXzrW9/iF3/xF3n3u98NxHrMM57xDAAuvPBCnve853Huuefe4Rhe+MIXcv3113PJJZfwEz/xE+zdu5eXv/zlvOc975md07Yt//k//2f++I//mN27d/PsZz+bwWDANddcw9///d/f6XU+4xnP4Pu+7/sAeOpTn3pUzegDH/gA/+N//A8e/OAH8+xnP5t+v8+b3/xmXvjCFx523sc//nF+8id/kk996lM84QlP4Kd+6qfw3vPyl7+cl770pXc6julnGWN46lOfCjArrL/jHe/Y9vw3vOEN/PzP/zxf//rXefKTn8zTnvY0vvKVr/DTP/3TfOITnzjh804U119/PR/60Ie46qqruOiii7jooosYjUb8xE/8BG95y1t40IMexNVXX83ll1/O/v37ue666/hf/+t/HfYeL33pS3nRi17EwYMHefrTn84P/uAP8nd/93dcddVVfO1rX+Pcc8/l0Y9+NF/4whe49dZbD3vt3r17+exnP8sP/dAPMRgMTvo65jgCYY45TgA/9VM/Ffbs2RNuvvnmw46//OUvD3v27Alve9vbDjv+V3/1V2HPnj3hF37hF2bHvu/7vi9ceumlwRgzO3b77beHhz70oeHKK6+cHfvUpz4V9uzZE175ylce9p579uwJP/IjPzL792tf+9qwZ8+e8IQnPCEcPHhwdvzzn/982LNnT3jWs541O/amN70p7NmzJ/z6r/968N7Pjr/61a8Oe/bsCXv27LnTezD9vI9+9KNHjfXf//t/Hz772c/OjpdlGR73uMeFCy64IKyurs6OPeYxjwmPecxjDruPzrlw7bXXhj179oS/+Zu/udNxXHnlleGCCy4It99+++zYj/7oj4Y9e/aEr33ta4ed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pJSkacov/MIv8MpXvpLLL7+cH/7hHybPcz772c/yj//4jzz96U/ngQ98IBBz4wAf+tCH6PV6POMZz+DBD37wXRrnT//0T3PDDTfwxje+kc9//vM8/OEP5ytf+Qqf+9znWFhYOC4S2nRcf/AHf8BXv/rVo7gid4aFhQVe+cpX8su//Ms84xnP4NJLL2X37t185jOf4R//8R952MMexs/8zM9s+9qPfOQjTCYTLr/88ll6bjtceeWVvOMd7+Av//Iv+YVf+AX+3b/7dzzvec/jta99LU9/+tN5whOeQAiBD37wgzRNw2/+5m8CHPd5KysrXHrppXz4wx/mx3/8x3n84x/PzTffzI033sj3fu/38vnPf/647sUVV1zBn/zJn/DMZz6TSy+9FGstn/jEJ/jGN77BysoKq6urrK+vs3v3bh73uMfxzGc+k3e+8508/elP5wd+4AeYTCZ84AMfoN/vH8WvedCDHsRDHvIQvvzlL3PxxRdzzjnnHNeY5jgxzHcic5wSrKyscP311/MzP/Mz7N27l7e+9a187nOf44lPfCLXX3/9YRpcz3nOc7juuuv4ru/6Lj74wQ/yp3/6p7Rty3//7/+d3/iN35idd+65585acf/0T/902xTLiSLLMt785jfzrGc9i29961u87W1vYzwe88Y3vpH73//+5Hl+p+/x1Kc+lcsuu4ybb76Zt7/97UeR2o4Hl112GW9729t4zGMew9/+7d/ytre9jclkwjXXXMOb3/zmYwaIaYF4uio/Fh75yEfygAc8gH379vHxj38ciAz76667jnPOOYf3vOc9vO997+PhD384b3vb27jwwgtnrz3e837jN36D5zznOayvr/PWt76VW2+9lde+9rU86UlPOu778PznP59rr70WKSVvf/vb+au/+ivOPfdc3vSmN/Hc5z4XYDZ+gFe96lW87GUvI89z/vzP/5yPfvSjXHLJJfzZn/0Z55133lHvPxXunKey7j6IsF0Se4457qO45ZZbWFlZmRWQt+IJT3gCRVHcoWzIHGcWXvCCF/DXf/3XfOITn5iz1O8mzHcic3xH4X/+z//J937v9x5V/P3gBz/IbbfddpRq8RxnLv7pn/6Jj3zkI1x22WXzAHI3Yr4TmeM7CjfeeCPXXHMNi4uLPOlJT2JpaYmvf/3r/M3f/A27du3iL//yL9mxY8fpHuYcdwF/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uTazZuVx0ARhTBDFZByTjCOHDoUQZ6+mSiLNSBm1Qou1viGmUri2gec012K6EEJMBXkLPQrHNgmimFIlYKAcUqoEBFGMY5tTHZoQQkw5SSJjYAAaA7RGYyDL6EIIkZLprFEEYYxtmbiOQSHn4FcCkiRdF5G1ECHE2U7ugqNItB7WR10pg0RLW1whhJAkMor6wvpgcjZECCFSkkRG4dgmidaNRFI/GyIL60IIIUlkVJapyLgWhgFRrDEM6WgohBB1srA+BpapsExFS87BL8vZECGEqJO300IIIcZNkogQQohxkyQihBBi3CSJCCGEGLezcmH9+MODk/XYySRxnj7TIUaQOE83iXNsz29oLUevhRBCjI9MZwkhhBg3SSJCCCHGTZKIEEKIcZMkIoQQYtwkiQghhBg3SSJCCCHGTZKIEEKIcZMkIoQQYtwkiQghhBg3SSJj8POf/5zrrruOz33uc2zZsmWqw+Hhhx+mu7ub7u5uHnzwQQB27txJT08Pn/vc53jooYcaX/u///u/LF++nGuuuYZvfetbRFE06fE+8MADrFmzpmnj/MUvfsHy5cvp6upiw4YNTRvn1q1bG7/3Bx54oKniLBaLLFmyhP/7v/8bV1x79+7llltu4dprr+UrX/kKpVJpUuJ84oknWLJkCT09Paxdu5YgCJoyzrp/+Zd/YcWKFY2PpzpOALQ4qQ8++EB/5jOf0YcOHdKlUkn39PTo119/fcri+fWvf63/4i/+Qvu+r4Mg0CtXrtQ///nP9eLFi/W7776rwzDUt99+u96xY4fWWuvu7m79yiuvaK21Xrt2rd6yZcukxrtz50592WWX6b/+67/WlUql6eJ899139ac+9Sm9b98+HQSB/vznP6937NjRdHGWy2W9aNEiffDgQR2Gob7xxhv1c8891xRx/vd//7desmSJvuSSS/R77703rt/zHXfcobdt26a11vrhhx/WDz744BmP86233tJ//ud/rgcGBnSSJPqb3/ymfuyxx5ouzrrXX39dX3nllfrWW29tXJvKOOtkJDKKnTt38md/9me0tbWRzWa55ppreOaZZ6Ysno6ODtasWYPjONi2zR/8wR/wzjvvMH/+fM477zwsy6Knp4dnnnmG999/n2q1yic+8QkAli9fPqmxHz58mIceeog777wTgFdffbXp4vyP//gPrrvuOubMmYNt2zz0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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.scatter(y_test,y_hat_test, alpha=0.05)\n",
    "plt.xlabel('y_test',fontsize=15)\n",
    "plt.ylabel('Y_hat_test', fontsize=15)   #This is X_test predicted\n",
    "plt.title('Testing the Accuracy', fontsize=20)\n",
    "plt.show()\n",
    "\n",
    "# when we create the testing dataset summary, we will use y_test as target and y_hat_test(which actually is X_test predicted)\n",
    "# as predictions, and then compute further residuals and differences.\n",
    "\n",
    "# The alpha uses the blurring concept to show where the maximum points are concentrated and we see they are evenly spread\n",
    "# after being concentrated initially, and a straight line can be fit."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Testing Dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_pf = pd.DataFrame(y_hat_test, columns=['Predictions'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_pf['Target'] = y_test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([477.77, 399.87, 910.28, ...,   1.26,   1.22,   1.21])"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "y_test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Predictions</th>\n",
       "      <th>Target</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>570.445581</td>\n",
       "      <td>477.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>189.396667</td>\n",
       "      <td>399.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>640.369699</td>\n",
       "      <td>910.28</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>-45.855011</td>\n",
       "      <td>1.22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>142.607513</td>\n",
       "      <td>1.27</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9801</th>\n",
       "      <td>665.419436</td>\n",
       "      <td>828.96</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9802</th>\n",
       "      <td>70.462927</td>\n",
       "      <td>1.26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9803</th>\n",
       "      <td>174.809098</td>\n",
       "      <td>1.26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9804</th>\n",
       "      <td>61.865196</td>\n",
       "      <td>1.22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9805</th>\n",
       "      <td>89.674284</td>\n",
       "      <td>1.21</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>9806 rows × 2 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "      Predictions  Target\n",
       "0      570.445581  477.77\n",
       "1      189.396667  399.87\n",
       "2      640.369699  910.28\n",
       "3      -45.855011    1.22\n",
       "4      142.607513    1.27\n",
       "...           ...     ...\n",
       "9801   665.419436  828.96\n",
       "9802    70.462927    1.26\n",
       "9803   174.809098    1.26\n",
       "9804    61.865196    1.22\n",
       "9805    89.674284    1.21\n",
       "\n",
       "[9806 rows x 2 columns]"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_pf"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_pf['Residuals'] = df_pf['Predictions'] - df_pf['Target']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_pf['Difference%'] = np.absolute(df_pf['Residuals']/df_pf['Target']*100)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Predictions</th>\n",
       "      <th>Target</th>\n",
       "      <th>Residuals</th>\n",
       "      <th>Difference%</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>9061</th>\n",
       "      <td>365.848053</td>\n",
       "      <td>365.83</td>\n",
       "      <td>0.018053</td>\n",
       "      <td>0.004935</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7277</th>\n",
       "      <td>513.961753</td>\n",
       "      <td>513.80</td>\n",
       "      <td>0.161753</td>\n",
       "      <td>0.031482</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>940</th>\n",
       "      <td>335.181570</td>\n",
       "      <td>334.75</td>\n",
       "      <td>0.431570</td>\n",
       "      <td>0.128923</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7608</th>\n",
       "      <td>186.764487</td>\n",
       "      <td>187.09</td>\n",
       "      <td>-0.325513</td>\n",
       "      <td>0.173988</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7222</th>\n",
       "      <td>718.165862</td>\n",
       "      <td>719.54</td>\n",
       "      <td>-1.374138</td>\n",
       "      <td>0.190975</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4843</th>\n",
       "      <td>443.280330</td>\n",
       "      <td>1.23</td>\n",
       "      <td>442.050330</td>\n",
       "      <td>35939.051236</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8175</th>\n",
       "      <td>461.325421</td>\n",
       "      <td>1.22</td>\n",
       "      <td>460.105421</td>\n",
       "      <td>37713.559114</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4957</th>\n",
       "      <td>465.914488</td>\n",
       "      <td>1.21</td>\n",
       "      <td>464.704488</td>\n",
       "      <td>38405.329567</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>433</th>\n",
       "      <td>470.159054</td>\n",
       "      <td>1.22</td>\n",
       "      <td>468.939054</td>\n",
       "      <td>38437.627338</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>992</th>\n",
       "      <td>-474.180416</td>\n",
       "      <td>1.21</td>\n",
       "      <td>-475.390416</td>\n",
       "      <td>39288.464137</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>9806 rows × 4 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "      Predictions  Target   Residuals   Difference%\n",
       "9061   365.848053  365.83    0.018053      0.004935\n",
       "7277   513.961753  513.80    0.161753      0.031482\n",
       "940    335.181570  334.75    0.431570      0.128923\n",
       "7608   186.764487  187.09   -0.325513      0.173988\n",
       "7222   718.165862  719.54   -1.374138      0.190975\n",
       "...           ...     ...         ...           ...\n",
       "4843   443.280330    1.23  442.050330  35939.051236\n",
       "8175   461.325421    1.22  460.105421  37713.559114\n",
       "4957   465.914488    1.21  464.704488  38405.329567\n",
       "433    470.159054    1.22  468.939054  38437.627338\n",
       "992   -474.180416    1.21 -475.390416  39288.464137\n",
       "\n",
       "[9806 rows x 4 columns]"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_pf.sort_values(by='Difference%')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_pf_better = df_pf[df_pf['Difference%']<100]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Final Testing Dataset... 3412 rows"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Predictions</th>\n",
       "      <th>Target</th>\n",
       "      <th>Residuals</th>\n",
       "      <th>Difference%</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>9061</th>\n",
       "      <td>365.848053</td>\n",
       "      <td>365.83</td>\n",
       "      <td>0.018053</td>\n",
       "      <td>0.004935</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7277</th>\n",
       "      <td>513.961753</td>\n",
       "      <td>513.80</td>\n",
       "      <td>0.161753</td>\n",
       "      <td>0.031482</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>940</th>\n",
       "      <td>335.181570</td>\n",
       "      <td>334.75</td>\n",
       "      <td>0.431570</td>\n",
       "      <td>0.128923</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7608</th>\n",
       "      <td>186.764487</td>\n",
       "      <td>187.09</td>\n",
       "      <td>-0.325513</td>\n",
       "      <td>0.173988</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7222</th>\n",
       "      <td>718.165862</td>\n",
       "      <td>719.54</td>\n",
       "      <td>-1.374138</td>\n",
       "      <td>0.190975</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7486</th>\n",
       "      <td>70.137404</td>\n",
       "      <td>69.99</td>\n",
       "      <td>0.147404</td>\n",
       "      <td>0.210607</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9564</th>\n",
       "      <td>426.179695</td>\n",
       "      <td>425.22</td>\n",
       "      <td>0.959695</td>\n",
       "      <td>0.225694</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9638</th>\n",
       "      <td>447.476944</td>\n",
       "      <td>448.50</td>\n",
       "      <td>-1.023056</td>\n",
       "      <td>0.228106</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8874</th>\n",
       "      <td>579.848305</td>\n",
       "      <td>578.24</td>\n",
       "      <td>1.608305</td>\n",
       "      <td>0.278138</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2482</th>\n",
       "      <td>438.248345</td>\n",
       "      <td>436.90</td>\n",
       "      <td>1.348345</td>\n",
       "      <td>0.308616</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      Predictions  Target  Residuals  Difference%\n",
       "9061   365.848053  365.83   0.018053     0.004935\n",
       "7277   513.961753  513.80   0.161753     0.031482\n",
       "940    335.181570  334.75   0.431570     0.128923\n",
       "7608   186.764487  187.09  -0.325513     0.173988\n",
       "7222   718.165862  719.54  -1.374138     0.190975\n",
       "7486    70.137404   69.99   0.147404     0.210607\n",
       "9564   426.179695  425.22   0.959695     0.225694\n",
       "9638   447.476944  448.50  -1.023056     0.228106\n",
       "8874   579.848305  578.24   1.608305     0.278138\n",
       "2482   438.248345  436.90   1.348345     0.308616"
      ]
     },
     "execution_count": 43,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_pf_better.sort_values(by='Difference%').head(10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Training Dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_tr = pd.DataFrame(y_hat, columns=['Predcitions'])\n",
    "# model.predict(X_train) is y_hat"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train_new = pd.DataFrame(X_train)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_tr['Target'] = y_train"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_tr['Residuals'] = df_tr['Predcitions'] - df_tr['Target']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_tr['Difference%'] = np.absolute(df_tr['Residuals']/df_tr['Target']*100)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_tr_better = df_tr[df_tr['Difference%']<100]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Final Training Dataset...7905 rows"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Predcitions</th>\n",
       "      <th>Target</th>\n",
       "      <th>Residuals</th>\n",
       "      <th>Difference%</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>8237</th>\n",
       "      <td>599.559743</td>\n",
       "      <td>599.61</td>\n",
       "      <td>-0.050257</td>\n",
       "      <td>0.008382</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16986</th>\n",
       "      <td>565.239959</td>\n",
       "      <td>565.15</td>\n",
       "      <td>0.089959</td>\n",
       "      <td>0.015918</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1743</th>\n",
       "      <td>30.939268</td>\n",
       "      <td>30.95</td>\n",
       "      <td>-0.010732</td>\n",
       "      <td>0.034676</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7029</th>\n",
       "      <td>390.607127</td>\n",
       "      <td>390.82</td>\n",
       "      <td>-0.212873</td>\n",
       "      <td>0.054468</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8546</th>\n",
       "      <td>213.585282</td>\n",
       "      <td>213.46</td>\n",
       "      <td>0.125282</td>\n",
       "      <td>0.058691</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12713</th>\n",
       "      <td>452.090420</td>\n",
       "      <td>452.40</td>\n",
       "      <td>-0.309580</td>\n",
       "      <td>0.068431</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8726</th>\n",
       "      <td>704.955553</td>\n",
       "      <td>704.41</td>\n",
       "      <td>0.545553</td>\n",
       "      <td>0.077448</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16457</th>\n",
       "      <td>709.831874</td>\n",
       "      <td>710.51</td>\n",
       "      <td>-0.678126</td>\n",
       "      <td>0.095442</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18114</th>\n",
       "      <td>630.976531</td>\n",
       "      <td>631.67</td>\n",
       "      <td>-0.693469</td>\n",
       "      <td>0.109783</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18699</th>\n",
       "      <td>302.004368</td>\n",
       "      <td>301.66</td>\n",
       "      <td>0.344368</td>\n",
       "      <td>0.114158</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       Predcitions  Target  Residuals  Difference%\n",
       "8237    599.559743  599.61  -0.050257     0.008382\n",
       "16986   565.239959  565.15   0.089959     0.015918\n",
       "1743     30.939268   30.95  -0.010732     0.034676\n",
       "7029    390.607127  390.82  -0.212873     0.054468\n",
       "8546    213.585282  213.46   0.125282     0.058691\n",
       "12713   452.090420  452.40  -0.309580     0.068431\n",
       "8726    704.955553  704.41   0.545553     0.077448\n",
       "16457   709.831874  710.51  -0.678126     0.095442\n",
       "18114   630.976531  631.67  -0.693469     0.109783\n",
       "18699   302.004368  301.66   0.344368     0.114158"
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_tr_better.sort_values(by='Difference%').head(10)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}

```