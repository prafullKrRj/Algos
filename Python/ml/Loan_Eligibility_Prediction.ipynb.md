```ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 135,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "\n",
    "import warnings\n",
    "warnings.filterwarnings('ignore')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 136,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender               13\n",
       "Married               3\n",
       "Dependents           15\n",
       "Education             0\n",
       "Self_Employed        32\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount           22\n",
       "Loan_Amount_Term     14\n",
       "Credit_History       50\n",
       "Property_Area         0\n",
       "Loan_Status           0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 136,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "train=pd.read_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\train.csv')\n",
    "train.Loan_Status=train.Loan_Status.map({'Y':1,'N':0})\n",
    "train.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 137,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Gender</th>\n",
       "      <th>Married</th>\n",
       "      <th>Dependents</th>\n",
       "      <th>Education</th>\n",
       "      <th>Self_Employed</th>\n",
       "      <th>ApplicantIncome</th>\n",
       "      <th>CoapplicantIncome</th>\n",
       "      <th>LoanAmount</th>\n",
       "      <th>Loan_Amount_Term</th>\n",
       "      <th>Credit_History</th>\n",
       "      <th>Property_Area</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001002</td>\n",
       "      <td>Male</td>\n",
       "      <td>No</td>\n",
       "      <td>0</td>\n",
       "      <td>Graduate</td>\n",
       "      <td>No</td>\n",
       "      <td>5849</td>\n",
       "      <td>0.0</td>\n",
       "      <td>NaN</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001003</td>\n",
       "      <td>Male</td>\n",
       "      <td>Yes</td>\n",
       "      <td>1</td>\n",
       "      <td>Graduate</td>\n",
       "      <td>No</td>\n",
       "      <td>4583</td>\n",
       "      <td>1508.0</td>\n",
       "      <td>128.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Rural</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001005</td>\n",
       "      <td>Male</td>\n",
       "      <td>Yes</td>\n",
       "      <td>0</td>\n",
       "      <td>Graduate</td>\n",
       "      <td>Yes</td>\n",
       "      <td>3000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>66.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001006</td>\n",
       "      <td>Male</td>\n",
       "      <td>Yes</td>\n",
       "      <td>0</td>\n",
       "      <td>Not Graduate</td>\n",
       "      <td>No</td>\n",
       "      <td>2583</td>\n",
       "      <td>2358.0</td>\n",
       "      <td>120.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001008</td>\n",
       "      <td>Male</td>\n",
       "      <td>No</td>\n",
       "      <td>0</td>\n",
       "      <td>Graduate</td>\n",
       "      <td>No</td>\n",
       "      <td>6000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>141.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>Urban</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID Gender Married Dependents     Education Self_Employed  \\\n",
       "0  LP001002   Male      No          0      Graduate            No   \n",
       "1  LP001003   Male     Yes          1      Graduate            No   \n",
       "2  LP001005   Male     Yes          0      Graduate           Yes   \n",
       "3  LP001006   Male     Yes          0  Not Graduate            No   \n",
       "4  LP001008   Male      No          0      Graduate            No   \n",
       "\n",
       "   ApplicantIncome  CoapplicantIncome  LoanAmount  Loan_Amount_Term  \\\n",
       "0             5849                0.0         NaN             360.0   \n",
       "1             4583             1508.0       128.0             360.0   \n",
       "2             3000                0.0        66.0             360.0   \n",
       "3             2583             2358.0       120.0             360.0   \n",
       "4             6000                0.0       141.0             360.0   \n",
       "\n",
       "   Credit_History Property_Area  \n",
       "0             1.0         Urban  \n",
       "1             1.0         Rural  \n",
       "2             1.0         Urban  \n",
       "3             1.0         Urban  \n",
       "4             1.0         Urban  "
      ]
     },
     "execution_count": 137,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "Loan_status=train.Loan_Status\n",
    "train.drop('Loan_Status',axis=1,inplace=True)\n",
    "test=pd.read_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\test.csv')\n",
    "Loan_ID=test.Loan_ID\n",
    "data=train.append(test)\n",
    "data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 138,
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
       "      <th>ApplicantIncome</th>\n",
       "      <th>CoapplicantIncome</th>\n",
       "      <th>LoanAmount</th>\n",
       "      <th>Loan_Amount_Term</th>\n",
       "      <th>Credit_History</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>981.000000</td>\n",
       "      <td>981.000000</td>\n",
       "      <td>954.000000</td>\n",
       "      <td>961.000000</td>\n",
       "      <td>902.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>5179.795107</td>\n",
       "      <td>1601.916330</td>\n",
       "      <td>142.511530</td>\n",
       "      <td>342.201873</td>\n",
       "      <td>0.835920</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>5695.104533</td>\n",
       "      <td>2718.772806</td>\n",
       "      <td>77.421743</td>\n",
       "      <td>65.100602</td>\n",
       "      <td>0.370553</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>9.000000</td>\n",
       "      <td>6.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>2875.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>100.000000</td>\n",
       "      <td>360.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>3800.000000</td>\n",
       "      <td>1110.000000</td>\n",
       "      <td>126.000000</td>\n",
       "      <td>360.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>5516.000000</td>\n",
       "      <td>2365.000000</td>\n",
       "      <td>162.000000</td>\n",
       "      <td>360.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>81000.000000</td>\n",
       "      <td>41667.000000</td>\n",
       "      <td>700.000000</td>\n",
       "      <td>480.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       ApplicantIncome  CoapplicantIncome  LoanAmount  Loan_Amount_Term  \\\n",
       "count       981.000000         981.000000  954.000000        961.000000   \n",
       "mean       5179.795107        1601.916330  142.511530        342.201873   \n",
       "std        5695.104533        2718.772806   77.421743         65.100602   \n",
       "min           0.000000           0.000000    9.000000          6.000000   \n",
       "25%        2875.000000           0.000000  100.000000        360.000000   \n",
       "50%        3800.000000        1110.000000  126.000000        360.000000   \n",
       "75%        5516.000000        2365.000000  162.000000        360.000000   \n",
       "max       81000.000000       41667.000000  700.000000        480.000000   \n",
       "\n",
       "       Credit_History  \n",
       "count      902.000000  \n",
       "mean         0.835920  \n",
       "std          0.370553  \n",
       "min          0.000000  \n",
       "25%          1.000000  \n",
       "50%          1.000000  \n",
       "75%          1.000000  \n",
       "max          1.000000  "
      ]
     },
     "execution_count": 138,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 139,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender               24\n",
       "Married               3\n",
       "Dependents           25\n",
       "Education             0\n",
       "Self_Employed        55\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount           27\n",
       "Loan_Amount_Term     20\n",
       "Credit_History       79\n",
       "Property_Area         0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 139,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 140,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "dtype('O')"
      ]
     },
     "execution_count": 140,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Dependents.dtypes"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 141,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 141,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAlAAAAI3CAYAAABUE2WnAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8vihELAAAACXBIWXMAAAsTAAALEwEAmpwYAAA5wklEQVR4nO3deZxsVXnv/8+XGQRBEYkgAiKQi4wCzhNqEnN/iYqigATFGAm5DlGTe0WM0eg1khCjwYl74kVRiVMCBL1EVGRwwDDJIJMioIBExYFJ5n5+f+zdUrTdp6vO6d27q/rz9lWvU3vtXbueKjynVz/rWWulqpAkSdLw1ug7AEmSpHFjB0qSJGlEdqAkSZJGZAdKkiRpRHagJEmSRmQHSpIkaUR2oCRJ0kRL8twkVya5Ksnhs5zfOMnnk1yU5NIkr5j3nq4DJUmSJlWSNYHvAr8DXA+cCxxYVZcNXHMEsHFVvSnJZsCVwG9V1d1z3dcMlCRJmmSPB66qqqvbDtGngefPuKaAjZIE2BD4OXDvym66VheRSpIkAdxz09WdD3Wts9l2fwocOtC0oqpWtM+3BK4bOHc98IQZt/gAcDLwI2AjYP+qmlrZe9qBkiRJY63tLK2Y43Rme8mM498DLgSeBWwHfDnJ16rqlrne0yE8SZI0ya4Htho4fiRNpmnQK4ATqnEVcA3w2yu7qRkoSZLUnan7+o7gXGD7JNsCNwAHAC+dcc0PgWcDX0uyObAjcPXKbmoHSpIkTayqujfJa4BTgTWBY6vq0iSHteePAd4JfCzJJTRDfm+qqptWdl+XMZAkSZ2558dXdt7RWHvzHWerc+qUNVCSJEkjcghPkiR1Z2qlqwGMLTtQkiSpM/MspzS2HMKTJEkakRkoSZLUnQkdwjMDJUmSNCIzUJIkqTvWQEmSJAnMQEmSpC71v5VLJ8xASZIkjcgMlCRJ6o41UJIkSQIzUJIkqUuuAyVJkiQwAyVJkjrkXniSJEkCzEBJkqQuWQMlSZIkMAMlSZK6ZA2UJEmSwAyUJEnqknvhSZIkCcxASZKkLk1oDZQdKEmS1B2XMZAkSRKYgZIkSV2a0CE8M1CSJEkjMgMlSZK6Yw2UJEmSwAyUJEnqUJULaUqSJAkzUJIkqUvOwpMkSRKYgZIkSV1yFp4kSZLADJQkSeqSNVCSJEkCM1CSJKlLU5O5DpQdqDF2z01XV98xjLP1t3ha3yGMtZsP9/tbXXt8+Kq+Qxhrm66zUd8hjL2zbzg9fccwruxASZKk7lgDJUmSJDADJUmSuuQ6UJIkSQIzUJIkqUsTWgNlB0qSJHXHITxJkiSBGShJktQlM1CSJEkCM1CSJKlDVZO5lYsZKEmSpBGZgZIkSd2xBkqSJElgBkqSJHVpQhfSNAMlSZI0IjNQkiSpO9ZASZIkCcxASZKkLlkDJUmSJDADJUmSumQNlCRJksAMlCRJ6pI1UJIkSQIzUJIkqUvWQEmSJAnMQEmSpC6ZgZIkSRKYgZIkSV2a0Fl4dqAkSVJ3HMKTJEkSmIGSJEldmtAhPDNQkiRJIzIDJUmSumMNlCRJksAMlCRJ6pI1UJIkSYKOO1BJ9k1SSX57Ne7xsST7tc8/kmSnhYsQkhwx4/i2hby/JEnL2tRU948edJ2BOhD4OnDAQtysqv6kqi5biHsNOGL+SyRJku7XWQcqyYbAU4BX0nagkjwzyVlJTkxyWZJjkqzRnrstyXuSXJDktCSbzXLPM5Ls1T5/bnvtRUlOa9sen+SbSb7d/rlj235IkhOSfDHJ95L8fdt+JLB+kguTHD/jvZ7Zvt+/JrkiyfFJ0p7bu73/RUnOSbJRkvWSfDTJJe377zPw3icl+XySa5K8Jskb22u+leSh7XXbtfGdn+Rrq5O1kyRpyTADNbIXAF+squ8CP0/yuLb98cBfALsA2wEvbNsfBFxQVY8DzgTeNteN287VPwMvqqrdgBe3p64Anl5VewB/DfztwMt2B/Zv33f/JFtV1eHAHVW1e1UdNMtb7QG8HtgJeDTwlCTrAJ8B/rx97+cAdwCvBqiqXWgyb8clWa+9z87AS9vP/i7gV22MZwMva69ZAby2qvYE/hL40Byf/dAk5yU57yMf/9RcX5EkSepQl7PwDgTe1z7/dHv8/4BzqupqgCSfAp4K/CswRdMxAfgkcMJK7v1E4Kyqugagqn7etm9M03HZHihg7YHXnFZVN7fvexmwNXDdPJ/hnKq6vn3NhcA2wM3AjVV1bvvet7Tnnwq8v227IskPgB3a+5xeVbcCtya5Gfh8234JsGubrXsy8Lk2yQWw7mwBVdUKms4W99x0dc0TvyRJ/arJ/FHVSQcqyabAs4CdkxSwJk2H5pT2z0FzfbMr+8Yzx/l30nRW9k2yDXDGwLm7Bp7fx3CffbbXzPXemaVttvtMDRxPtfdcA/hlVe0+REySJKlnXQ3h7Qd8vKq2rqptqmor4BqabNPjk2zb1j7tT1NkPh3Lfu3zlw60z+Zs4BlJtgWYriOiyUDd0D4/ZMhY70my9vyX/doVwBZJ9m7fe6MkawFnAQe1bTsAjwKuHOaGbRbrmiQvbl+fJLuNEJMkSUuTNVAjORA4cUbbv9F0jM4GjgS+Q9Opmr7uduCxSc6nyV69Y66bV9VPgUOBE5JcxP1Df38PvDvJN2iyXsNYAVw8s4h8Je99N03H7/3te38ZWI+mZmnNJJe08RxSVXfNfaffcBDwyvaelwLPH+G1kiRpEaUWcWwyyTOBv6yqP5jl3G1VteGiBTMBrIFaPetv8bS+QxhrNx/u97e69vjwVX2HMNY2XWejvkMYe2ffcPrKyk8WxB3Hv7Xzn1XrH/TOzj/HTK5ELkmSNKJF3Quvqs7ggYXdg+fMPkmSNGncC0+SJEmwyBkoSZK0zPQ0S65rdqAkSVJ3JnQhTYfwJEmSRmQGSpIkdWdCh/DMQEmSJI3IDJQkSeqOGShJkiSBGShJktQlF9KUJEkSmIGSJEkdqinXgZIkSRJ2oCRJUpemprp/zCPJc5NcmeSqJIfPcc0zk1yY5NIkZ853T4fwJEnSxEqyJvBB4HeA64Fzk5xcVZcNXLMJ8CHguVX1wyQPn+++dqAkSVJ3+p+F93jgqqq6GiDJp4HnA5cNXPNS4ISq+iFAVf1kvps6hCdJksZakkOTnDfwOHTg9JbAdQPH17dtg3YAHpLkjCTnJ3nZfO9pBkqSJHVnEWbhVdUKYMUcpzPbS2YcrwXsCTwbWB84O8m3quq7c72nHShJkjTJrge2Gjh+JPCjWa65qapuB25PchawGzBnB8ohPEmS1J3+Z+GdC2yfZNsk6wAHACfPuObfgaclWSvJBsATgMtXdlMzUJIkaWJV1b1JXgOcCqwJHFtVlyY5rD1/TFVdnuSLwMXAFPCRqvrOyu5rB0qSJHVniHWaulZVpwCnzGg7ZsbxUcBRw97TITxJkqQRmYGSJEndqcncC88OlCRJ6s4SGMLrgkN4kiRJIzIDJUmSurMIC2n2wQyUJEnSiMxASZKk7vS/mXAnzEBJkiSNyAyUJEnqjjVQkiRJAjNQkiSpQzWh60DZgRpj62/xtL5DGGt3/OhrfYcw1t6151v7DmHs3XnfXX2HMNZuunsyh4Y0HuxASZKk7lgDJUmSJDADJUmSuuQ6UJIkSQIzUJIkqUvWQEmSJAnMQEmSpC5N6DpQZqAkSZJGZAZKkiR1xxooSZIkgRkoSZLUpQldB8oOlCRJ6o5DeJIkSQIzUJIkqUPlMgaSJEkCM1CSJKlL1kBJkiQJzEBJkqQumYGSJEkSmIGSJEldmtCFNM1ASZIkjcgMlCRJ6o41UJIkSQIzUJIkqUNlBkqSJElgBkqSJHXJDJQkSZLADJQkSerSlOtASZIkCTNQkiSpS9ZASZIkCcxASZKkLpmBkiRJEpiBkiRJHaqazAyUHShJktQdh/AkSZIEI3SgkvxWkk8n+X6Sy5KckmSHLoNr3/ftSf6yff6OJM9Z4Pu/PskGA8fXJnnYQr6HJEnL1lR1/+jBUB2oJAFOBM6oqu2qaifgCGDzLoObqar+uqq+ssC3fT2wwXwXSZIkTRs2A7UPcE9VHTPdUFUXAl9PclSS7yS5JMn+AEk2THJakgva9ue37dskuSLJcUkuTvKv09mfNvPzd0nOaR+PmRlEko8l2a99vneSbya5qL1+o/b+X2vf94IkT26vfWaSM9r3uyLJ8Wm8DtgCOD3J6TPea5sklyf55ySXJvlSkvXbc49J8pX2vS9Isl17v9m+i2cmOTPJZ5N8N8mRSQ5qY74kyXbtdZsl+bck57aPpwz/n1GSpKWppqrzRx+G7UDtDJw/S/sLgd2B3YDnAEcleQRwJ7BvVT2OpvP1njaLBbAjsKKqdgVuAf7HwP1uqarHAx8A3jdXMEnWAT4D/HlVTb/3HcBPgN9p33d/4OiBl+1Bk23aCXg08JSqOhr4EbBPVe0zy1ttD3ywqh4L/BJ4Udt+fNu+G/Bk4MaVfBe0bX8O7AIcDOzQfs6PAK9tr/kn4L1VtXf7Ph+Z6/NLkqR+rW4R+VOBT1XVfVX1Y+BMYG8gwN8muRj4CrAl9w/3XVdV32iff7K9x7RPDfz5pJW8747AjVV1LkBV3VJV9wJrA/+c5BLgczSdpWnnVNX1VTUFXAhsM8Tnu6bNtEHTgdwmyUbAllV1Yvved1bVr1byXQCcW1U3VtVdwPeBL7XtlwzE8RzgA0kuBE4GHty+1wMkOTTJeUnOm5q6fYiPIElSjya0BmrYZQwuBfabpT2ztAEcBGwG7FlV9yS5FlivPTfzk9YQz2d739nOvwH4MU3GZw2aTNi0uwae38dwn33ma9Zn7s88V/vM+0wNHE8NxLEG8KSqumNlAVXVCmAFwFrrbDmZc0MlSVrihs1AfRVYN8mrphuS7A38Atg/yZpJNgOeDpwDbAz8pO087QNsPXCvRyWZzi4dCHx94Nz+A3+evZJ4rgC2aGOgrX9aq33fG9ss08HAmkN8tluB38j0zKWqbgGuT/KC9r3Xbeu4zmL272JYXwJeM32QZPcRXitJ0tI0tQiPHgzVgapmGdF9gd9plzG4FHg78C/AxcBFNJ2s/1VV/0VTI7RXkvNoslFXDNzucuDl7fDeQ4EPD5xbN8l/0tQLvWEl8dxN08l6f5KLgC/TZLg+1N77W8AOwDBjXCuA/5hZRD6Pg4HXtZ/hm8Bv0cxSnO27GNbraL6zi5NcBhw2wmslSdIiymIusZ5kG+ALVbXzLOeuBfaqqpsWLaAx5xDe6rnjR1/rO4Sx9q4939p3CGPv2Fsv7juEsbbumuv0HcLY+95Pz19Z+cmC+OVBz+r8Z9Umx3+1888xkyuRS5IkjWhR98KrqmtplkSY7dw2ixmLJElaBO6FJ0mSJFjkDJQkSVpmepol1zUzUJIkSSMyAyVJkjrT1151XTMDJUmSNCIzUJIkqTvWQEmSJAnMQEmSpA5Nag2UHShJktQdh/AkSZIEZqAkSVKHygyUJEmSwAyUJEnqkhkoSZIkgRkoSZLUIWugJEmSBJiBkiRJXTIDJUmSJDADJUmSOmQNlCRJkgAzUJIkqUNmoCRJkgSYgZIkSR0yAyVJkiTADJQkSepSpe8IOmEGSpIkaURmoCRJUmesgZIkSRJgBkqSJHWopiazBsoOlCRJ6oxDeJIkSQLMQEmSpA7VhC5jYAdqjN18+NP6DmGsvWvPt/Ydwlh7y/nv7DuEsXf2Hq/uO4SxdsPdv+g7BC1jdqAkSVJnrIGSJEkSYAZKkiR1aFKXMTADJUmSNCIzUJIkqTNVfUfQDTNQkiRJI7IDJUmSOlNT6fwxnyTPTXJlkquSHL6S6/ZOcl+S/ea7px0oSZI0sZKsCXwQ+H1gJ+DAJDvNcd3fAacOc19roCRJUmeWwCy8xwNXVdXVAEk+DTwfuGzGda8F/g3Ye5ibmoGSJEljLcmhSc4beBw6cHpL4LqB4+vbtsHXbwnsCxwz7HuagZIkSZ1ZjFl4VbUCWDHH6dlSYDOjeh/wpqq6LxkuY2YHSpIkTbLrga0Gjh8J/GjGNXsBn247Tw8D/nuSe6vqpLluagdKkiR1ZgnUQJ0LbJ9kW+AG4ADgpYMXVNW208+TfAz4wso6T2AHSpIkTbCqujfJa2hm160JHFtVlyY5rD0/dN3TIDtQkiSpM1W9Z6CoqlOAU2a0zdpxqqpDhrmns/AkSZJGZAZKkiR1pqb6jqAbdqAkSVJnppbAEF4XHMKTJEkakRkoSZLUmaVQRN4FM1CSJEkjMgMlSZI6swQW0uyEGShJkqQRmYGSJEmdWYzNhPtgBkqSJGlEZqAkSVJnrIGSJEkSYAZKkiR1yJXIJUmSBJiBkiRJHXIlckmSJAFmoCRJUodcB0qSJEmAGShJktQhZ+FJkiQJMAMlSZI65Cw8SZIkAWPUgUpy2yK8xxuS3Jlk467fa544jujz/SVJWihV3T/6MDYdqEVyIHAusG/PcdiBkiRpCRvrDlSS3ZN8K8nFSU5M8pC2/VVJzk1yUZJ/S7JB2/6xJEcn+WaSq5PsN3Cv7YANgb+i6UhNtx+S5KQkn09yTZLXJHljkm+37/3QeWI5I8le7fOHJbl24L4nJPliku8l+fu2/Uhg/SQXJjl+Eb5GSZI6M1Xp/NGHse5AAR8H3lRVuwKXAG9r20+oqr2rajfgcuCVA695BPBU4A+AIwfaDwQ+BXwN2DHJwwfO7Qy8FHg88C7gV1W1B3A28LJ5YlmZ3YH9gV2A/ZNsVVWHA3dU1e5VddDMFyQ5NMl5Sc479oLvD/EWkiT1pyqdP/owth2otk5pk6o6s206Dnh6+3znJF9LcglwEPDYgZeeVFVTVXUZsPlA+wHAp6tqCjgBePHAudOr6taq+ilwM/D5tv0SYJt5YlmZ06rq5qq6E7gM2Hq+F1TViqraq6r2+uPHbTfEW0iSpIU2qcsYfAx4QVVdlOQQ4JkD5+4aeB6AJLsC2wNfTgKwDnA18MFZXjM1cDzF/N/hvdzfUV1vxrnB+943xL0kSRorLqS5xFTVzcAvkjytbToYmM4AbQTcmGRtmgzUfA4E3l5V27SPLYAtk8ybERoilmuBPdvn+zGce9rYJUnSEjROGY8Nklw/cPyPwMuBY9oi8auBV7Tn3gr8J/ADmmG2jea59wHA789oO7Ft//GQ8c0Vyz8An01yMPDVIe+1Arg4yQWz1UFJkjQuJnQvYVKTuk3yMnD7Xx/gf7zVcNRxa/Ydwlh7y/nv7DuEsfcHe7y67xDG2g13/6LvEMbed378rc7H1761xQs7/1n1xB+dsOjjhOOUgZIkSWPGGihJkiQBZqAkSVKH3ExYkiRJgBkoSZLUoam+A+iIGShJkqQRmYGSJEmdKayBkiRJEmagJElSh6YmdMlnM1CSJEkjMgMlSZI6M2UNlCRJksAMlCRJ6pCz8CRJkgSYgZIkSR2a1JXI7UBJkqTOOIQnSZIkwAyUJEnq0KQO4ZmBkiRJGpEZKEmS1BkzUJIkSQLMQEmSpA45C0+SJEmAGShJktShqclMQJmBkiRJGpUZKEmS1Jkpa6AkSZIEZqAkSVKHqu8AOmIGSpIkaURmoCRJUmdciVySJEmAGShJktShqTgLT5IkSZiBkiRJHZrUWXh2oMbYHh++qu8Qxtqd993Vdwhj7ew9Xt13CGPvC9/+YN8hjLVddtq/7xC0jNmBkiRJnZnUWXh2oCRJUmfcTFiSJEmAGShJktQhNxOWJEkSYAZKkiR1aFKXMTADJUmSNCIzUJIkqTPOwpMkSRJgBkqSJHVoUhfSNAMlSZI0IjNQkiSpM87CkyRJEmAGSpIkdchZeJIkSQLMQEmSpA45C0+SJEmAGShJktQhM1CSJEkCzEBJkqQOlbPwJEmSBGagJElShya1BsoOlCRJ6sykdqAcwpMkSRqRGShJktQZNxOWJEkSYAZKkiR1yM2EJUmSBJiBkiRJHXIWniRJkgAzUJIkqUNmoCRJkgTYgZIkSR2qRXjMJ8lzk1yZ5Kokh89y/qAkF7ePbybZbb572oGSJEkTK8mawAeB3wd2Ag5MstOMy64BnlFVuwLvBFbMd19roCRJUmeWwDpQjweuqqqrAZJ8Gng+cNn0BVX1zYHrvwU8cr6bmoGSJEljLcmhSc4beBw6cHpL4LqB4+vbtrm8EviP+d7TDJQkSerMYszCq6oVzD3sNlsObNbSqST70HSgnjrfe86bgUpy23zXdCHJG5LcmWTjPt5/II4jVnJu0yQXto//SnLDwPE6ixmnJEma1fXAVgPHjwR+NPOiJLsCHwGeX1U/m++mS3kI70DgXGDfnuOYswNVVT+rqt2ranfgGOC908dVdffKbprE7J8kaeItgVl45wLbJ9m2TW4cAJw8eEGSRwEnAAdX1XeH+Vyr1IFKsnuSb7XT/U5M8pC2/VVJzk1yUZJ/S7JB2/6xJEe3UwOvTrLfPPffDtgQ+CuajtR0+yFJTkry+STXJHlNkjcm+XYbz0Pnie+MJHu1zx+W5NqB+56Q5ItJvpfk79v2I4H124zS8SN8P3smOTPJ+UlOTfKIgff/2yRnAn/eHr83yVlJLk+ydxvH95L87znu/etx3pvv/OmwIUmStCxV1b3Aa4BTgcuBz1bVpUkOS3JYe9lfA5sCH2p/5p83331XNQP1ceBN7XS/S4C3te0nVNXeVbVbG+QrB17zCJoxxT8Ajpzn/gcCnwK+BuyY5OED53YGXkpTVf8u4FdVtQdwNvCyeeJbmd2B/YFdgP2TbFVVhwN3tBmlg4a4B0nWBt4P7FdVewLHtnFO26SqnlFV72mP766qp9NksP4deHX7GQ9JsunM+1fViqraq6r22ni9zYYJSZKk3kxRnT/mU1WnVNUOVbVdVb2rbTumqo5pn/9JVT1kYBRpr/nuOfIwUluTtElVndk2HQd8rn2+c5s52YQmg3TqwEtPqqop4LIkm8/zNgcA+1bVVJITgBfTrOEAcHpV3QrcmuRm4PNt+yXArvPEtzKnVdXN7We8DNiaB1btD2tHmg7Ql5MArAncOHD+MzOun04jXgJcWlU3tjFcTTNmO+84rCRJWlwLXYfzMeAFVXVRkkOAZw6cu2vg+ZyrQrRFXNtzfwdkHeBq7u9ADd5nauB4ivk/z73cn3Vbb8a5wfveN8S95hKajtCT5jh/+xzvO/hZpo+tk5IkjTX3wmu1WZpfJHla23QwMJ3t2Qi4sR3GGmrIaxYHAm+vqm3axxbAlkm2XoD4rgX2bJ+vtA5rwD3t5xnWlcBmSZ4EzZBekseO8HpJkrTEDZPh2CDJ9QPH/wi8HDimLRK/GnhFe+6twH8CP6AZktpoFWI6gGa59UEntu0/HvIec8X3D8BnkxwMfHXIe60ALk5ywTB1UFV1d1skf3Q7nLgW8D7g0iHfT5KkiTHMXnXjKFWT+tEm3w6b7eV/vNVw5313zX+R5vTfHjTvTgeaxxe+/cH5L9Kcdtlp/75DGHtX/OTczjdaefvWB3X+s+rtPzh+0TeMWcrrQEmSJC1JvRUpJ9kF+MSM5ruq6gl9xDOfdkmB02Y59exhViyVJGk5WgKbCXeitw5UVV1Cs/bSWGg7Sbv3HYckSeqf0+QlSVJnhlnochxZAyVJkjQiM1CSJKkzk5l/MgMlSZI0MjNQkiSpM27lIkmSJMAMlCRJ6pCz8CRJkgSYgZIkSR2azPyTGShJkqSRmYGSJEmdcRaeJEmSADNQkiSpQ87CkyRJEmAGSpIkdWgy809moCRJkkZmBkqSJHVmUmfh2YGSJEmdqQkdxHMIT5IkaURmoCRJUmcmdQjPDJQkSdKIzEBJkqTOuJCmJEmSADNQkiSpQ5OZfzIDJUmSNDIzUJIkqTPWQEmSJAkwAyVJkjrkOlCSJEkCzEBJkqQOTepeeHagxtim62zUdwhj7aa7J/Mv9WK54e5f9B3C2Ntlp/37DmGsXXLZZ/oOQcuYHShJktQZa6AkSZIEmIGSJEkdmtQaKDNQkiRJIzIDJUmSOmMNlCRJkgAzUJIkqUNTNZk1UHagJElSZyaz++QQniRJ0sjMQEmSpM5MTWgOygyUJEnSiMxASZKkzriQpiRJkgAzUJIkqUMupClJkiTADJQkSeqQs/AkSZIEmIGSJEkdchaeJEmSADNQkiSpQ87CkyRJEmAGSpIkdajKGihJkiRhBkqSJHXIdaAkSZIEmIGSJEkdchaeJEmSADNQkiSpQ5O6ErkdKEmS1BmLyCVJkgSYgZIkSR1yIU1JkiQBZqAkSVKHXMZAkiRJgBkoSZLUoUldxsAMlCRJ0ojMQEmSpM64DtQiSPJbST6d5PtJLktySpIdVvFeH0uyX/v8I0l2ap8fMcRrb5txfEiSD7TPD0vyspW89plJnrwqMUuSpPGwZDJQSQKcCBxXVQe0bbsDmwPfbY/XrKr7Rr13Vf3JwOERwN+uapxVdcw8lzwTuA345rD3TLJWVd27qjFJkrRUuQ5U9/YB7hnsoFTVhcCaSU5P8i/AJUnWTHJUknOTXJzkT6HpgCX5QJu5+n/Aw6fvk+SMJHslORJYP8mFSY5flSCTvD3JX7bPX9e+38Vt5mwb4DDgDe17PC3J1klOa685Lcmj2td+LMk/JjkdOCrJ95Js1p5bI8lVSR62KjFKkqRuLZkMFLAzcP4c5x4P7FxV1yQ5FLi5qvZOsi7wjSRfAvYAdgR2oclaXQYcO3iTqjo8yWuqavd5Ylk/yYUDxw8FTp7lusOBbavqriSbVNUvkxwD3FZV/wCQ5PPAx6vquCR/DBwNvKB9/Q7Ac6rqviS/BA4C3gc8B7ioqm6a+Ybt5z8UYNuNd2DzB20xz0eRJKk/1kD165yquqZ9/rvAy9oOzn8CmwLbA08HPlVV91XVj4Cvrsb73VFVu08/gL+e47qLgeOT/BEw1xDck4B/aZ9/AnjqwLnPDQxJHgtM11b9MfDR2W5WVSuqaq+q2svOkyRJ/VhKHahLgT3nOHf7wPMArx3o4GxbVV9qzy12N/f/Az5IE/f5SYbJ6A3G+OvPVVXXAT9O8izgCcB/LGSgkiT1oRbhf31YSh2orwLrJnnVdEOSvYFnzLjuVODPkqzdXrNDkgcBZwEHtDVSj6CpqZrNPdOvXR1J1gC2qqrTgf8FbAJsCNwKbDRw6TeBA9rnBwFfX8ltPwJ8EvjsqhTLS5KkxbFkaqCqqpLsC7wvyeHAncC1wEkzLv0IsA1wQTtz76c0NUUnAs8CLqGZtXfmHG+1Arg4yQVVddBqhLwm8MkkG9Nkxd7b1kB9HvjXJM8HXgu8Djg2yf9sY33FSu55Ms3Q3azDd5IkjZupCZ2Fl0mdXjiOkuxF0xF72jDXP2nLffyPtxpuuvuWvkMYa+uusdqJ3GXvXhPNq+WSyz7Tdwhjb+2HPTpdv8fTt3x25z+rzrrhtM4/x0xLJgO13LVZtz+jGeaTJGkiTOpv+su2A5VkU+C0WU49u6p+ttjxVNWRwJGL/b6SJGl0y7YD1XaSdu87DkmSJpnrQEmSJAlYxhkoSZLUvUnNQNmBkiRJnZnU2f4O4UmSJI3IDJQkSerMpA7hmYGSJEkakRkoSZLUmb42++2aGShJkqQRmYGSJEmdcRaeJEmSADtQkiSpQ1NU54/5JHlukiuTXJXk8FnOJ8nR7fmLkzxuvnvagZIkSRMryZrAB4HfB3YCDkyy04zLfh/Yvn0cCnx4vvtaAyVJkjqzBGqgHg9cVVVXAyT5NPB84LKBa54PfLyaYL+VZJMkj6iqG+e6qRkoSZI01pIcmuS8gcehA6e3BK4bOL6+bWPEax7ADJQkSerMYqxEXlUrgBVznM5sL1mFax7ADJQkSZpk1wNbDRw/EvjRKlzzAHagJElSZ2oR/jePc4Htk2ybZB3gAODkGdecDLysnY33RODmldU/gUN4kiRpglXVvUleA5wKrAkcW1WXJjmsPX8McArw34GrgF8Br5jvvnagJElSZ6b6n4VHVZ1C00kabDtm4HkBrx7lng7hSZIkjcgMlCRJ6swQNUpjyQyUJEnSiMxASZKkziyFGqgu2IGSJEmdcQhPkiRJgBkoSZLUoUkdwjMDJUmSNCIzUJIkqTPWQEmSJAkwAyVJkjpkDZQkSZIAM1Bj7ewbTk/fMaxMkkOrakXfcYwzv8PV4/e3evz+Vp/foTVQ0qo4tO8AJoDf4erx+1s9fn+rz+9wQpmBkiRJnama6juETpiBkiRJGpEZKHVpWY/7LxC/w9Xj97d6/P5W37L/DqcmtAYqNaHTCyVJUv+23nTXzjsaP/jZxYs+qcoMlCRJ6sykJmqsgZIkSRqRGShJktSZSa2BMgMlLUFJHtR3DJJWTZKH9h2DumcHSgsmyQ5JTkvynfZ41yR/1Xdc4yTJk5NcBlzeHu+W5EM9hzVWknximDatXJKHtH+HHzf96DumMfKfST6X5L8nWdI7RiyGqur80Qc7UFpI/wy8GbgHoKouBg7oNaLx817g94CfAVTVRcDTe41o/Dx28CDJmsCePcUylpK8E7gYOBp4T/v4h16DGi870CxfcDBwVZK/TbJDzzFpgVkDpYW0QVWdM+MXrnv7CmZcVdV1M77D+/qKZZwkeTNwBLB+klumm4G7cS2eUb0E2K6q7u47kHFUTUrky8CXk+wDfBL4H0kuAg6vqrN7DXCRTU3oLDw7UFpINyXZDpqKwST7ATf2G9LYuS7Jk4FKsg7wOtrhPK1cVb0beHeSd1fVm/uOZ8x9B9gE+EnPcYylJJsCf0STgfox8FrgZGB34HPAtr0F14NJ3UzYDpQW0qtpftP/7SQ3ANfQ/COi4R0G/BOwJXA98CWa71VDqqo3J9kS2JqBf+Oq6qz+oho77wa+3dYz3jXdWFXP6y+ksXI28AngBVV1/UD7eUmO6SkmLTBXIteCa2eQrVFVt/Ydi5afJEfS1N5dxv3Dn+UP/+EluRT4P8AlwK93gq2qM3sLaky0NXdHVdUb+45lqdh849/uvKPx45uvcCVyja8kmwAvA7YB1pqu46mq1/UX1XhJsi1Nun8bHpg98Yf/8PYFdqyqu+a9UnO5qaqO7juIcVRV9yXZre841D07UFpIpwDfYsZvrRrJScD/BT6P3+GquhpYm4GhJ43s/CTvpqnbGRzCu6C/kMbKhUlOpql3un26sapO6C+k/kzqQpp2oLSQ1jNtvdru9Df/1fYrmh9gp/HAH/5mQoe3R/vnEwfaCnhWD7GMo4fSLEUy+H0VsCw7UJPKGigtmCRvAG4DvsADf3D9vLegxkySlwLb0xSP+5v/Kkjy8tnaq+q4xY5lHLU1PK+rqvf2HYsmw8MevEPnHY2bbvmuNVAaa3cDRwFvgV/nbAt4dG8RjZ9daKY+P4v7h/D8zX8EdpRWT1vD8zyaRV21CpI8Eng/8BSav79fB/58xow8jTk7UFpIbwQeU1U39R3IGNsXeLQLGK66JNfAbxZdVJUd+eF9M8kHgM/wwBoeM6HD+SjwL8CL2+M/att+p7eIeuRCmtL8LqWpP9GquwgXMFxdew08X4/mh5ibu47mye2f7xhoMxM6vM2q6qMDxx9L8vq+glE37EBpId1HU7x7OhbvrqrNgSuSnIsLGK6SqvrZjKb3Jfk68Nd9xDOOqmqfvmMYczcl+SPgU+3xgbT7Wy5Hk1prbQdKC+mk9qFV97a+Axh3SR43cLgGTUZqo57CGUtJNgf+Ftiiqn4/yU7Ak6rq//Yc2rj4Y+ADNHVkBXwTeEWvEWnBOQtPC6rdv2161/Erq+qePuMZR+0Pr73bw3OqyuG8EbQZ0Gn3AtcC/1BVV/YT0fhJ8h80NTtvqardkqwFfLuqduk5tLGQ5ClV9Y352paLjTfcrvOOxs23fX/RZ+HZgdKCSfJM4DiaH1gBtgJe7h5kw0vyEpqZjGfQfIdPA/5nVf1rn3FpeUlyblXtneTbVbVH23ZhVe3ec2hjIckFVfW4+dqWi0ntQDmEp4X0HuB3p3/TT7IDTQ3Anr1GNV7eAuw9nXVKshnwFcAO1JCSbEwzFPr0tulM4B1VdXN/UY2HJGtV1b3A7Uk2pZ3NmOSJgN/fPJI8iaYAf7Mkg4sKPxhYs5+o+jepiZo1+g5AE2XtwWGSqvouzZYaGt4aM4bsfoZ/T0d1LHAr8JL2cQvNcJTmd07751/QbOOyXZJvAB+n2aNRK7cOsCFNcmKjgcctwH49xqUOOISnBZPkWJrfWD/RNh0ErFVVFk8OKclRwK7cP3tnf+CSqvpf/UU1XmYbanL4aTgzhuzWAnakGUq2nnEESbauqh+0z9cANqyqW3oOqzcbbrBt5x2N2351jTVQGl9J1gVeDTyV5h/ds4APVZWbuo4gyQsZ+A6r6sSeQxorSc6mqRv7env8FJoi8if1G9nSl+R64B/nOl9Vc57T/ZL8C3AYzdIu5wMbA/9YVUf1GlhP7EBJ80jyIJrNcO9rj9cE1q0qF9ccUpJtgRur6s72eH1g86q6ttfAxkiS3WkmM2xM0wn9OXBIVV3UZ1zjIMmNwIdpvrffUFV/s7gRjafpjGeSg2hqQN8EnF9Vu/YcWi8etME2nXc0bv/VtRaRa6ydBjyHZkNhgPVpNsV98pyv0Eyf44Hf131t296zX66ZqupCYLckD26Pl+3QySq4sareMf9lmsfaSdYGXgB8oKruSWK2YsLYgdJCWq+qpjtPVNVtSTboM6AxtNbgPnhVdXe7tpaGlGQT4GXANsBaSfOLqSviD2Wo3+KTPKSqftF1MGPs/9As53IRcFaSrWkKyZcl98KT5nd7ksdNbziaZE/gjp5jGjc/TfK8qjoZIMnzATdnHs0pwLeAS4CpnmMZN88e8rrTgGW5ptEwqupo4OiBph8kWbbb40xqqZAdKC2k1wOfS/Kj9vgRNLPINLzDgOOTfIAmG3AdTTZFw1uvqt44/2Waqap+PuSli15vMg6S/FFVfXLGGlCDLMKfIHagtGCq6twkv839U5+vcOrzaKrq+8ATk2xIM8nj1r5jGkOfSPIq4As8cEPmYTsHmt9kphRW34PaP917cUBN6P9dnIWnBZXkybS1J9NtVfXx3gIaM+1SEC/iN79DC3uHlOTVwLuAX3L/D/qqqkf3FtSEWc7bkmh06663VecdjbvuvM5ZeBpfST4BbAdcSDN7DJofYHaghvfvNFtmnM9A9kQjeSPwmKqydqw7DuHNIsnRKzu/XCcyTGqixg6UFtJewE41qX9bFscjq+q5fQcx5i4FXHtsNST5RFUdvJK2YYvNl5vzB57/Dc2ejJpQdqC0kL4D/BZwY9+BjLFvJtmlqi7pO5Axdh9wYZLTeWAN1LL87X8VPXbwoF0U99ebgltPNruqOm76eZLXDx4vZ5P6O7UdKC2khwGXJTmHB/7gel5/IY2dpwKHJLmG5jsMTf3OslzBeBWd1D4GTea/4AssyZuBI4D1k0yvWxTgbmBFb4GNJ/8/N+EsIteCSfKM2dqr6szFjmVctQvu/YbpjUk1uiRbAQcs133IVkWSd1fVm/uOY5xZaH+/tdbZsvOOxr1332ARucaXHaVVl+Sh7VOXLVgASR4GvBg4ENgScEPmEVTVm5NsCWzNA2eDntVfVEtfklu5P/O0wYwsXlXVg/uJTF2wA6XVNvCPRnhg2tp/NIZ3Pvd/hzMV4BT8eSTZCNgXeCmwA02n6dFV9cheAxtDSY4EDgAu44Ezau1ArURVDbX+03LbCqeP7NBicAhP0kRIcgdwDvBXwNerqpJc7fpPo0tyJbBrVbmURgcc3psMa/QdgCZHuw7UvG2aW5LThmnTrI4A1gM+DLw5yXY9xzPOrgbW7juICTaRGZnlxiE8LaSZU5/XYmDqs+aWZD2abSAeluQh3P8P7IOBLXoLbIxU1XuB9yZ5NE3t00nAFkneBJxYVd/tM74x8yuapSBOw6UguuDQzwSwA6XV5tTnBfGnNJsxb0FTDzXdgboF+GBPMY2lqrqaZiuXdyXZhaYm6j9oVsnXcE5uH5LmYA2UFoxTn1dfktdW1fv7jkNSd5J8u6r26DsOrR47UFpQTn1efW7IvHqSvBD4O+DhNJk8Z4OOqF3I9Td+OFiQP5z5tsJJ8lBXcx9/DuFpwTj1efW5IfOC+HvgD6vq8r4DGWN7DTxfj2ZNrYfOca1+k1vhLANmoLRgnPq8+pJcjhsyr5Yk36iqp/Qdx6RJ8vWqemrfcSxlg/Wg3L+h9a/rQS1xmCxmoLSQpqc+24FadW7IvPrOS/IZmll4gzPITugtojGTZHCNojVoMlJDLRK5nFXVu4F3Ww+6PNiB0kJy6vPqc0Pm1fdgmv8v/u5AWwF2oIb3noHn9wLXAi/pJ5TxkeS3q+oK4HMzOqEAVNUFPYSljjiEpwWT5OWztVfVcYsdy7hyQ2ZpfCX556p6VZLTZzldVfWsRQ9KnbEDJWmitIuSvpKmkHe96faq+uPeghozSTYG3gY8vW06E3hHVd3cX1TS0uIQnhZMku2BdwM78cAfXE59HlKSJwLvB/4bsA6wJnC7U/BH8gngCuD3gHcABwHOyBvNsTT1eNPDdgcDHwVe2FtEY6BdQmNO1uFNFjNQWjBJvk7zW+t7gT8EXkHz/7G39RrYGElyHs1SEJ+jKdx9GbB9VR3Ra2BjZHqRwiQXV9WuSdYGTnX4ZHhJLqyq3edr0wMl+Wj79OHAk4Gvtsf7AGdUlR3QCeJmwlpI61fVaTSdph9U1dsBf2iNqKquAtasqvuq6qPAM3sOadzc0/75yyQ7AxvTLEyq4d2R5NdLFiR5CnBHj/GMhap6RVW9gmbSwk5V9aKqehEz1oXSZHAITwvpziRrAN9L8hrgBprfxDS8XyVZh2Y249/TLGfwoJ5jGjcr2g2Z30qzn9uG7XMN78+A49paqAA/Bw7pNaLxsk1VDS5F8mNgh76CUTccwtOCSbI3Ta3JJsA7aaaTH1VV3+ozrnGSZGuaf2zXAd5Akz35UJuVkhZVkgcDVNUt812r+yX5ALA98CmabNQBwFVV9dpeA9OCsgMlLSFJHgTcUVVT7fGawLpV9auVv1LT2qzJ24GntU1nAO90BtnwkmxCU3+3DQ/ck9E13YaUZF/un8V4VlWd2Gc8WnjWQGnBJPly+w/v9PFDkpzaY0jj6DRgg4Hj9YGv9BTLuDoWuIVmBtlLgFtpZpBpeKfQdJ4uAc4feGh4FwD/r6reAJyaxJXcJ4w1UFpID6uqX04fVNUvklgDNZr1quq26YOqui3JBit7gX7Ddm3h7rS/SXJhX8GMqfWq6o19BzGukrwKOJRmA+btgC2BY4Bn9xmXFpYZKC2kqSSPmj5o63kcIx7N7YNbQCTZE2c/jcoZZKvvE0leleQRSR46/eg7qDHyauApNJlQqup7OKFm4piB0kJ6C/D1JNPbjjyd5rcwDe/1NPto/ag9fgSwf3/hjKXDgI+3tVAAvwBm3WZIc7obOIrm7/T0L0EFuCjucO6qqruTAJBkLfxlcuJYRK4FleRhwBNppj6fXVU39RzS2GkXftyR5ju8oqrumeclmsXgDLIkr6+q9/Uc0thI8n3gCf79XTXtEiS/pCnEfy3wP4DLquotfcalhWUHSqttegfy2XYfB3cgH0aSZ1XVV+faCsItIFZPkh9W1aPmv1IASU4GDnD256pJk3r6E+B3aX4ROhX4SPkDd6I4hKeF8BfAq4D3zHKucDXyYTyDZtuHP5zlXAF2oFZP+g5gzNxHs5jr6cBd040uYzC/djHhi6tqZ+Cf+45H3TEDJWnimYEaTZLZasaqqj6+6MGMoSTHA2+uqh/2HYu6YwZKq80dyFdfkpVOGa+qf1ysWMZVkluZvVA3NOtpaUhVddzgcZKtaFbT1nAeAVya5Bzg9unGqnpefyFpodmB0kKYbdhpmsNPw3GRvdVUVX6HC6idEPJi4ECadYxcSXseSR4DbA78zYxTz6DZG1QTxCE8SRIA7WrZ+wIvpdn89kRg/6p6ZK+BjYkkXwCOqKqLZ7TvBbytqlb2y6bGjAtpasEk2TTJ0UkuSHJ+kn9KsmnfcY2TJI9O8vkkP03ykyT/nsS1d7RYfgK8EngXzYruf0GzJpSGs83MzhNAVZ1HszWOJogdKC2kTwM/BV4E7Nc+/0yvEY2ffwE+S1NDsQXwOZod3aXFcASwHvBh4M1Jtus5nnGz3krOWYc3YexAaSE9tKreWVXXtI//DWzSd1BjJlX1iaq6t318Elcw1iKpqvdW1ROA59EU358EbJHkTUl26DW48XBuuw/eAyR5JW7GPHGsgdKCSfIPwHk0GRRoslCPraq39RfVeElyJM0Kxp+m6TjtD6wLfBCgqn7eW3BalpLsQlNIvn9VmZFaiSSb09SN3c39Haa9gHWAfavqv/qKTQvPDpQWTDuN/EHAVNu0BvdP4a2qenAvgY2RJNes5HRVlfVQ6l2Ss6vqSX3HsVQl2QfYuT28tKq+2mc86oYdKEnSSJJ8u6r26DsOqU+uA6UF1S6q+VSa4aevVdVJ/UY0XpKsR7Px6K+/Q+CYqrqz18CkB/I3by17ZqC0YJJ8CHgM988a2x/4flW9ur+oxkuSzwK3Ap9smw4EHlJVL+4vKumBklxQVbNuHi4tF2agtJCeAew8veN4kuOAS/oNaezsWFW7DRyfnuSi3qKRZufmzFr2XMZAC+lKYHDD1q2A31hUTiv17SRPnD5I8gTgGz3GI83m4L4DkPrmEJ4WTJIzgb2Bc9qmvYGzgV+BG2kOI8nlwI7A9C7ujwIup5nZWFW1a1+xafloaxn/Dng4TbYpOJNWegA7UFowSZ4xeEhTCH0gTVE0VXVmH3GNkyRbr+x8Vf1gsWLR8pXkKuAPq+ryvmORlio7UFpQSXan2Yj0JcA1wAlV9f5egxpDSR7OwLYQVfXDlVwuLagk36iqp/Qdh7SUWUSu1dZu8XAATbbpZzT736Wq9uk1sDGU5HnAe2j2wfsJsDXNEN5j+4xLy855ST5Ds5XLXdONVXVCbxFJS4wdKC2EK2jWK/rDqroKIMkb+g1pbL0TeCLwlarao13R+MCeY9Ly82Ca2sXfHWgrwA6U1LIDpYXwIpoM1OlJvkizj5vTnFfNPVX1syRrJFmjqk5P8nd9B6Xlpape0XcM0lJnB0qrrapOBE5M8iDgBcAbgM2TfBg4saq+1Gd8Y+aXSTakyegdn+QnwL09x6Rlpl0R/5U0Q8eDtXh/3FtQ0hLjOlBaMFV1e1UdX1V/ADwSuBA4vN+oxs7zgTuA1wNfBL4P/GGfAWlZ+gTwW8DvAWfS/H2+tdeIpCXGWXjSEpNkc5o1tADOqaqf9BmPlp/pzYKTXFxVuyZZGzi1qp7Vd2zSUmEGSlpCkryEZiHSF9MsBfGfSfbrNyotQ/e0f/4yyc7AxsA2/YUjLT3WQElLy1uAvaezTkk2A74C/GuvUWm5WZHkIcBbgZOBDdvnkloO4UlLSJJLqmqXgeM1gIsG2yRJ/TMDJS0tX0xyKvCp9nh/4JQe49EylGRj4O3A09qmM4B3VtXNfcUkLTVmoKQlIMljgM2r6hvtRq5PpVlL6xfA8VX1/V4D1LKS5N+A7wDHtU0HA7tV1Qv7i0paWuxASUtAki8AR1TVxTPa9wLeVlUuZaBFk+TCqtp9vjZpOXMWnrQ0bDOz8wRQVefh7CctvjuSPHX6IMlTaNYnk9SyBkpaGtZbybn1Fy0KqXEY8PG2FgqaoeSX9xiPtOSYgZKWhnOTvGpmY5JXAuf3EI+Wsaq6qKp2A3YFdq2qPQAX0ZQGWAMlLQHt6uMnAndzf4dpL2AdYN+q+q++YpMAkvywqh7VdxzSUmEHSlpCkuwD7NweXlpVX+0zHmlakuuqaqu+45CWCjtQkqR5mYGSHsgickkSAEluBWb7rTo4mUF6ADNQkiRJI3IWniRJ0ojsQEmSJI3IDpQkSdKI7EBJkiSN6P8HovENPA+TJ3QAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<Figure size 648x648 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "%matplotlib inline \n",
    "corrmat=data.corr()\n",
    "f,ax=plt.subplots(figsize=(9,9))\n",
    "sns.heatmap(corrmat,vmax=.8,square=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 142,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1.0    775\n",
       "0.0    182\n",
       "Name: Gender, dtype: int64"
      ]
     },
     "execution_count": 142,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Gender=data.Gender.map({'Male':1,'Female':0})\n",
    "data.Gender.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 143,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 143,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 648x648 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "corrmat=data.corr()\n",
    "f,ax=plt.subplots(figsize=(9,9))\n",
    "sns.heatmap(corrmat,vmax=.8,square=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 144,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Married=data.Married.map({'Yes':1,'No':0})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 145,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1.0    631\n",
       "0.0    347\n",
       "Name: Married, dtype: int64"
      ]
     },
     "execution_count": 145,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Married.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 146,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Dependents=data.Dependents.map({'0':0,'1':1,'2':2,'3+':3})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 147,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.0    545\n",
       "2.0    160\n",
       "1.0    160\n",
       "3.0     91\n",
       "Name: Dependents, dtype: int64"
      ]
     },
     "execution_count": 147,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Dependents.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 148,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 148,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 648x648 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "corrmat=data.corr()\n",
    "f,ax=plt.subplots(figsize=(9,9))\n",
    "sns.heatmap(corrmat,vmax=.8,square=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 149,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Education=data.Education.map({'Graduate':1,'Not Graduate':0})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 150,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1    763\n",
       "0    218\n",
       "Name: Education, dtype: int64"
      ]
     },
     "execution_count": 150,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Education.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 151,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Self_Employed=data.Self_Employed.map({'Yes':1,'No':0})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 152,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.0    807\n",
       "1.0    119\n",
       "Name: Self_Employed, dtype: int64"
      ]
     },
     "execution_count": 152,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Self_Employed.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 153,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Semiurban    349\n",
       "Urban        342\n",
       "Rural        290\n",
       "Name: Property_Area, dtype: int64"
      ]
     },
     "execution_count": 153,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Property_Area.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 154,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Property_Area=data.Property_Area.map({'Urban':2,'Rural':0,'Semiurban':1})"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 155,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1    349\n",
       "2    342\n",
       "0    290\n",
       "Name: Property_Area, dtype: int64"
      ]
     },
     "execution_count": 155,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Property_Area.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 156,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 156,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 648x648 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "corrmat=data.corr()\n",
    "f,ax=plt.subplots(figsize=(9,9))\n",
    "sns.heatmap(corrmat,vmax=.8,square=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 157,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Gender</th>\n",
       "      <th>Married</th>\n",
       "      <th>Dependents</th>\n",
       "      <th>Education</th>\n",
       "      <th>Self_Employed</th>\n",
       "      <th>ApplicantIncome</th>\n",
       "      <th>CoapplicantIncome</th>\n",
       "      <th>LoanAmount</th>\n",
       "      <th>Loan_Amount_Term</th>\n",
       "      <th>Credit_History</th>\n",
       "      <th>Property_Area</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001002</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>5849</td>\n",
       "      <td>0.0</td>\n",
       "      <td>NaN</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001003</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>4583</td>\n",
       "      <td>1508.0</td>\n",
       "      <td>128.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001005</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>1.0</td>\n",
       "      <td>3000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>66.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001006</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>2583</td>\n",
       "      <td>2358.0</td>\n",
       "      <td>120.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001008</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>6000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>141.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Gender  Married  Dependents  Education  Self_Employed  \\\n",
       "0  LP001002     1.0      0.0         0.0          1            0.0   \n",
       "1  LP001003     1.0      1.0         1.0          1            0.0   \n",
       "2  LP001005     1.0      1.0         0.0          1            1.0   \n",
       "3  LP001006     1.0      1.0         0.0          0            0.0   \n",
       "4  LP001008     1.0      0.0         0.0          1            0.0   \n",
       "\n",
       "   ApplicantIncome  CoapplicantIncome  LoanAmount  Loan_Amount_Term  \\\n",
       "0             5849                0.0         NaN             360.0   \n",
       "1             4583             1508.0       128.0             360.0   \n",
       "2             3000                0.0        66.0             360.0   \n",
       "3             2583             2358.0       120.0             360.0   \n",
       "4             6000                0.0       141.0             360.0   \n",
       "\n",
       "   Credit_History  Property_Area  \n",
       "0             1.0              2  \n",
       "1             1.0              0  \n",
       "2             1.0              2  \n",
       "3             1.0              2  \n",
       "4             1.0              2  "
      ]
     },
     "execution_count": 157,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 158,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "981"
      ]
     },
     "execution_count": 158,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Credit_History.size"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 159,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Credit_History.fillna(np.random.randint(0,2),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 160,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender               24\n",
       "Married               3\n",
       "Dependents           25\n",
       "Education             0\n",
       "Self_Employed        55\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount           27\n",
       "Loan_Amount_Term     20\n",
       "Credit_History        0\n",
       "Property_Area         0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 160,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 161,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Married.fillna(np.random.randint(0,2),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 162,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender               24\n",
       "Married               0\n",
       "Dependents           25\n",
       "Education             0\n",
       "Self_Employed        55\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount           27\n",
       "Loan_Amount_Term     20\n",
       "Credit_History        0\n",
       "Property_Area         0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 162,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 163,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.LoanAmount.fillna(data.LoanAmount.median(),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 164,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Loan_Amount_Term.fillna(data.Loan_Amount_Term.mean(),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 165,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender               24\n",
       "Married               0\n",
       "Dependents           25\n",
       "Education             0\n",
       "Self_Employed        55\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount            0\n",
       "Loan_Amount_Term      0\n",
       "Credit_History        0\n",
       "Property_Area         0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 165,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 166,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1.0    775\n",
       "0.0    182\n",
       "Name: Gender, dtype: int64"
      ]
     },
     "execution_count": 166,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Gender.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 167,
   "metadata": {},
   "outputs": [],
   "source": [
    "from random import randint \n",
    "data.Gender.fillna(np.random.randint(0,2),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 168,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1.0    799\n",
       "0.0    182\n",
       "Name: Gender, dtype: int64"
      ]
     },
     "execution_count": 168,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.Gender.value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 169,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Dependents.fillna(data.Dependents.median(),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 170,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID               0\n",
       "Gender                0\n",
       "Married               0\n",
       "Dependents            0\n",
       "Education             0\n",
       "Self_Employed        55\n",
       "ApplicantIncome       0\n",
       "CoapplicantIncome     0\n",
       "LoanAmount            0\n",
       "Loan_Amount_Term      0\n",
       "Credit_History        0\n",
       "Property_Area         0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 170,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 171,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 171,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 648x648 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "corrmat=data.corr()\n",
    "f,ax=plt.subplots(figsize=(9,9))\n",
    "sns.heatmap(corrmat,vmax=.8,square=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 172,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.Self_Employed.fillna(np.random.randint(0,2),inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 173,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Loan_ID              0\n",
       "Gender               0\n",
       "Married              0\n",
       "Dependents           0\n",
       "Education            0\n",
       "Self_Employed        0\n",
       "ApplicantIncome      0\n",
       "CoapplicantIncome    0\n",
       "LoanAmount           0\n",
       "Loan_Amount_Term     0\n",
       "Credit_History       0\n",
       "Property_Area        0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 173,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 174,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Gender</th>\n",
       "      <th>Married</th>\n",
       "      <th>Dependents</th>\n",
       "      <th>Education</th>\n",
       "      <th>Self_Employed</th>\n",
       "      <th>ApplicantIncome</th>\n",
       "      <th>CoapplicantIncome</th>\n",
       "      <th>LoanAmount</th>\n",
       "      <th>Loan_Amount_Term</th>\n",
       "      <th>Credit_History</th>\n",
       "      <th>Property_Area</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001002</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>5849</td>\n",
       "      <td>0.0</td>\n",
       "      <td>126.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001003</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>4583</td>\n",
       "      <td>1508.0</td>\n",
       "      <td>128.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001005</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>1.0</td>\n",
       "      <td>3000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>66.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001006</td>\n",
       "      <td>1.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>2583</td>\n",
       "      <td>2358.0</td>\n",
       "      <td>120.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001008</td>\n",
       "      <td>1.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>0.0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.0</td>\n",
       "      <td>6000</td>\n",
       "      <td>0.0</td>\n",
       "      <td>141.0</td>\n",
       "      <td>360.0</td>\n",
       "      <td>1.0</td>\n",
       "      <td>2</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Gender  Married  Dependents  Education  Self_Employed  \\\n",
       "0  LP001002     1.0      0.0         0.0          1            0.0   \n",
       "1  LP001003     1.0      1.0         1.0          1            0.0   \n",
       "2  LP001005     1.0      1.0         0.0          1            1.0   \n",
       "3  LP001006     1.0      1.0         0.0          0            0.0   \n",
       "4  LP001008     1.0      0.0         0.0          1            0.0   \n",
       "\n",
       "   ApplicantIncome  CoapplicantIncome  LoanAmount  Loan_Amount_Term  \\\n",
       "0             5849                0.0       126.0             360.0   \n",
       "1             4583             1508.0       128.0             360.0   \n",
       "2             3000                0.0        66.0             360.0   \n",
       "3             2583             2358.0       120.0             360.0   \n",
       "4             6000                0.0       141.0             360.0   \n",
       "\n",
       "   Credit_History  Property_Area  \n",
       "0             1.0              2  \n",
       "1             1.0              0  \n",
       "2             1.0              2  \n",
       "3             1.0              2  \n",
       "4             1.0              2  "
      ]
     },
     "execution_count": 174,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 175,
   "metadata": {},
   "outputs": [],
   "source": [
    "data.drop('Loan_ID',inplace=True,axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 176,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Gender               0\n",
       "Married              0\n",
       "Dependents           0\n",
       "Education            0\n",
       "Self_Employed        0\n",
       "ApplicantIncome      0\n",
       "CoapplicantIncome    0\n",
       "LoanAmount           0\n",
       "Loan_Amount_Term     0\n",
       "Credit_History       0\n",
       "Property_Area        0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 176,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 177,
   "metadata": {},
   "outputs": [],
   "source": [
    "train_X=data.iloc[:614,]\n",
    "train_y=Loan_status\n",
    "X_test=data.iloc[614:,]\n",
    "seed=7"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 178,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.model_selection import train_test_split\n",
    "train_X,test_X,train_y,test_y=train_test_split(train_X,train_y,random_state=seed)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 179,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.discriminant_analysis import LinearDiscriminantAnalysis\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn.tree import DecisionTreeClassifier\n",
    "from sklearn.svm import SVC\n",
    "from sklearn.neighbors import KNeighborsClassifier\n",
    "from sklearn.naive_bayes import GaussianNB"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 180,
   "metadata": {},
   "outputs": [],
   "source": [
    "models=[]\n",
    "models.append((\"logreg\",LogisticRegression()))\n",
    "models.append((\"tree\",DecisionTreeClassifier()))\n",
    "models.append((\"lda\",LinearDiscriminantAnalysis()))\n",
    "models.append((\"svc\",SVC()))\n",
    "models.append((\"knn\",KNeighborsClassifier()))\n",
    "models.append((\"nb\",GaussianNB()))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 181,
   "metadata": {},
   "outputs": [],
   "source": [
    "seed=7\n",
    "scoring='accuracy'"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 182,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.model_selection import KFold \n",
    "from sklearn.model_selection import cross_val_score\n",
    "result=[]\n",
    "names=[]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 203,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "logreg 0.747826 0.042600\n",
      "tree 0.678261 0.068608\n",
      "lda 0.763043 0.040612\n",
      "svc 0.684783 0.060908\n",
      "knn 0.636957 0.068089\n",
      "nb 0.719565 0.032897\n"
     ]
    }
   ],
   "source": [
    "for name,model in models:\n",
    "    #print(model)\n",
    "    kfold=KFold(n_splits=10,random_state=seed)\n",
    "    cv_result=cross_val_score(model,train_X,train_y,cv=kfold,scoring=scoring)\n",
    "    result.append(cv_result)\n",
    "    names.append(name)\n",
    "    print(\"%s %f %f\" % (name,cv_result.mean(),cv_result.std()))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 204,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.7987012987012987\n",
      "[[25 23]\n",
      " [ 8 98]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.76      0.52      0.62        48\n",
      "           1       0.81      0.92      0.86       106\n",
      "\n",
      "    accuracy                           0.80       154\n",
      "   macro avg       0.78      0.72      0.74       154\n",
      "weighted avg       0.79      0.80      0.79       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=LogisticRegression()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 205,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 206,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1,\n",
       "       0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1,\n",
       "       1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1,\n",
       "       1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1,\n",
       "       0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0,\n",
       "       1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1,\n",
       "       1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1,\n",
       "       0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0,\n",
       "       1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1])"
      ]
     },
     "execution_count": 206,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 207,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 208,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            1\n",
       "3  LP001035            0\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 208,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 209,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputlr.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 210,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.6298701298701299\n",
      "[[25 23]\n",
      " [34 72]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.42      0.52      0.47        48\n",
      "           1       0.76      0.68      0.72       106\n",
      "\n",
      "    accuracy                           0.63       154\n",
      "   macro avg       0.59      0.60      0.59       154\n",
      "weighted avg       0.65      0.63      0.64       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=DecisionTreeClassifier()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 211,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 212,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0,\n",
       "       0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 1,\n",
       "       1, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0,\n",
       "       0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1,\n",
       "       1, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1,\n",
       "       1, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1,\n",
       "       0, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 1, 0,\n",
       "       1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0,\n",
       "       1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1,\n",
       "       0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0])"
      ]
     },
     "execution_count": 212,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 213,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 214,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            1\n",
       "3  LP001035            0\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 214,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 215,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputdt.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 216,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.7922077922077922\n",
      "[[26 22]\n",
      " [10 96]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.72      0.54      0.62        48\n",
      "           1       0.81      0.91      0.86       106\n",
      "\n",
      "    accuracy                           0.79       154\n",
      "   macro avg       0.77      0.72      0.74       154\n",
      "weighted avg       0.79      0.79      0.78       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=LinearDiscriminantAnalysis()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 217,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 218,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1,\n",
       "       0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1,\n",
       "       1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1,\n",
       "       1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1,\n",
       "       0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0,\n",
       "       1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1,\n",
       "       1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1,\n",
       "       0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0,\n",
       "       1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1])"
      ]
     },
     "execution_count": 218,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 219,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 220,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            1\n",
       "3  LP001035            0\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 220,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 221,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputld.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 222,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.6883116883116883\n",
      "[[  0  48]\n",
      " [  0 106]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.00      0.00      0.00        48\n",
      "           1       0.69      1.00      0.82       106\n",
      "\n",
      "    accuracy                           0.69       154\n",
      "   macro avg       0.34      0.50      0.41       154\n",
      "weighted avg       0.47      0.69      0.56       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=SVC()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 223,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 224,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1])"
      ]
     },
     "execution_count": 224,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 225,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 226,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            1\n",
       "3  LP001035            1\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 226,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 227,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputSVC.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 228,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.6493506493506493\n",
      "[[ 8 40]\n",
      " [14 92]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.36      0.17      0.23        48\n",
      "           1       0.70      0.87      0.77       106\n",
      "\n",
      "    accuracy                           0.65       154\n",
      "   macro avg       0.53      0.52      0.50       154\n",
      "weighted avg       0.59      0.65      0.60       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=KNeighborsClassifier()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 229,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 230,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1,\n",
       "       1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 1,\n",
       "       1, 1, 0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0,\n",
       "       1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1,\n",
       "       0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1,\n",
       "       1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1,\n",
       "       1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1])"
      ]
     },
     "execution_count": 230,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 231,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 232,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            0\n",
       "3  LP001035            1\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 232,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 233,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputknn.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 234,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.7337662337662337\n",
      "[[21 27]\n",
      " [14 92]]\n",
      "              precision    recall  f1-score   support\n",
      "\n",
      "           0       0.60      0.44      0.51        48\n",
      "           1       0.77      0.87      0.82       106\n",
      "\n",
      "    accuracy                           0.73       154\n",
      "   macro avg       0.69      0.65      0.66       154\n",
      "weighted avg       0.72      0.73      0.72       154\n",
      "\n"
     ]
    }
   ],
   "source": [
    "from sklearn.metrics import accuracy_score\n",
    "from sklearn.metrics import confusion_matrix\n",
    "from sklearn.metrics import classification_report\n",
    "svc=GaussianNB()\n",
    "svc.fit(train_X,train_y)\n",
    "pred=svc.predict(test_X)\n",
    "print(accuracy_score(test_y,pred))\n",
    "print(confusion_matrix(test_y,pred))\n",
    "print(classification_report(test_y,pred))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 236,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output=pd.DataFrame()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 237,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1,\n",
       "       0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1,\n",
       "       1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1,\n",
       "       1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1,\n",
       "       0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1,\n",
       "       1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1,\n",
       "       1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,\n",
       "       1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1])"
      ]
     },
     "execution_count": 237,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "outp=svc.predict(X_test).astype(int)\n",
    "outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 238,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output['Loan_ID']=Loan_ID\n",
    "df_output['Loan_Status']=outp"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 239,
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
       "      <th>Loan_ID</th>\n",
       "      <th>Loan_Status</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>LP001015</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>LP001022</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>LP001031</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LP001035</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>LP001051</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Loan_ID  Loan_Status\n",
       "0  LP001015            1\n",
       "1  LP001022            1\n",
       "2  LP001031            1\n",
       "3  LP001035            1\n",
       "4  LP001051            1"
      ]
     },
     "execution_count": 239,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_output.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 240,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_output[['Loan_ID','Loan_Status']].to_csv(r'C:\\Users\\prath\\LoanEligibilityPrediction\\Dataset\\outputgnb.csv',index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
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
 "nbformat_minor": 4
}

```