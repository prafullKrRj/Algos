```ipynb
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Building a decision tree from scracth "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 88,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Okay using only pandas and numpy \n",
    "import pandas as pd\n",
    "import numpy as np"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Lets take a toy dataset to get the idea of a decision tree "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 89,
   "metadata": {},
   "outputs": [],
   "source": [
    "training_data = [\n",
    "    ['Green', 3, 'Apple'],\n",
    "    ['Yellow', 3, 'Apple'],\n",
    "    ['Red', 1, 'Grape'],\n",
    "    ['Yellow', 3, 'Lemon'],\n",
    "]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 90,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[['Green', 3, 'Apple'],\n",
       " ['Yellow', 3, 'Apple'],\n",
       " ['Red', 1, 'Grape'],\n",
       " ['Yellow', 3, 'Lemon']]"
      ]
     },
     "execution_count": 90,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "training_data"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "    Lets calculate gini index first so that it tells us how impure the current distribution is. \n",
    "    A value close to 0 indicates that the data is relatively pure , \n",
    "    while data closer to >=0.5 indicates that the decision is impure (i.e:there is a mix of 2 or more classes).   "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 91,
   "metadata": {},
   "outputs": [],
   "source": [
    "def Gini_Index(data):\n",
    "    \"\"\"\n",
    "    Given some input data we calculate the percentage representation of each output class wrt the data \n",
    "    We always assume to last column is the class labels\n",
    "    \"\"\"\n",
    "    df=pd.DataFrame(data)\n",
    "    #p contains the percentage probabilites of each category of output label,\n",
    "    p=df[len(df.columns)-1].value_counts(normalize=True).values\n",
    "    #We return the values as computed by the gini index formula\n",
    "    return 1-sum(list(map(lambda n:n**2,p)))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 92,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.625"
      ]
     },
     "execution_count": 92,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# The value closer to 0.5 indicates a good mixture in the distribution , i.e : it is impure\n",
    "Gini_Index(training_data)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now we need to find a way to split the data into child nodes of a root to eventually build a tree.The way we derive child nodes is based on a rule which we use to split the data at the node into 2 child such that we minimize impurity on each split.   "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 93,
   "metadata": {},
   "outputs": [],
   "source": [
    "#To split the data we need rules\n",
    "class Rule:\n",
    "    #We construct a rule object based on the column and the attribute on which rule needs to be applied for a split. \n",
    "    def __init__(self,feature_col,feature_value):\n",
    "        self.feature_col=feature_col\n",
    "        self.feature_value=feature_value\n",
    "    \n",
    "    #Simple function to check rule for any input value\n",
    "    def check_rule(self,sample):\n",
    "        #print(sample,\"  \",self.feature_col)\n",
    "        if sample[self.feature_col]==self.feature_value:\n",
    "            return True\n",
    "        else:\n",
    "            return False\n",
    "    #A representation function to return a string description when we try to print the object\n",
    "    def __repr__(self):\n",
    "        return f\"Check for rule on {self.feature_col} if value is equal to {self.feature_value}\""
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 94,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Lets make a rule where we want to check if a sample matchs the color red in column number 0\n",
    "r1=Rule(0,'Red')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 95,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 95,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# call the check_rule() off the r1 object while passing the sample parameter for which the rule must be checked\n",
    "r1.check_rule(['Red',3,'Apple'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 96,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "False"
      ]
     },
     "execution_count": 96,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Just to check if its working lets try a sample that must return a false\n",
    "r1.check_rule(['Green',3,'Apple'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 97,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Lets try another rule on column 1 to check if diameter equals 3\n",
    "r2=Rule(1,3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 98,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "False"
      ]
     },
     "execution_count": 98,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "r2.check_rule(['Yellow',1,\"Lemon\"])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 99,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 99,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "r2.check_rule(['Green',3,\"Apple\"])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now that we have the rule setup we need to be able to split a set of rows based on a rule."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 100,
   "metadata": {},
   "outputs": [],
   "source": [
    "def find_split(data,rule):\n",
    "    \"\"\"\n",
    "    Input:\n",
    "        data : the set of rows that need to split\n",
    "        rule : type:Rule object \n",
    "               rule which needs to be used to split the set of rows passed as data.\n",
    "    Returns:\n",
    "        Two lists\n",
    "        true_child and false_child \n",
    "        All the rows that match the rule are put into true_child and everything not matching the rule is in false_child\n",
    "    \"\"\"\n",
    "    true_child=[]\n",
    "    false_child=[]\n",
    "    for sample in data:\n",
    "        if rule.check_rule(sample) == True:\n",
    "            true_child.append(sample)\n",
    "        else:\n",
    "            false_child.append(sample)\n",
    "            \n",
    "    return true_child,false_child"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 101,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Lets use the rule we made previous,r1, to split the training data\n",
    "left,right=find_split(training_data,r1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 102,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[['Red', 1, 'Grape']]"
      ]
     },
     "execution_count": 102,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "left #All the rows that match the rule"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 103,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[['Green', 3, 'Apple'], ['Yellow', 3, 'Apple'], ['Yellow', 3, 'Lemon']]"
      ]
     },
     "execution_count": 103,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "right #All the rows that did not match the rule"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 104,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Left(Matched the rule)\n",
      " [['Green', 3, 'Apple'], ['Yellow', 3, 'Apple'], ['Yellow', 3, 'Lemon']]\n",
      "Right(Did not match the rule)\n",
      " [['Red', 1, 'Grape']]\n"
     ]
    }
   ],
   "source": [
    "# Similarly we can split on rule r2 also\n",
    "left,right=find_split(training_data,r2)\n",
    "print(\"Left(Matched the rule)\\n\",left)\n",
    "print(\"Right(Did not match the rule)\\n\",right)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## How do we know which rule to split on?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 105,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.625"
      ]
     },
     "execution_count": 105,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#TO being with lets calculate the Ginni_Index AKA the level of impurity before we split\n",
    "Gini_Index(training_data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 106,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Lets try any rule and see after split \n",
    "left,right=find_split(training_data,Rule(0,\"Green\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 107,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[['Green', 3, 'Apple']]\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "0.0"
      ]
     },
     "execution_count": 107,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#The left side (Rule-True side) is completely pure , hence gini_index=0\n",
    "print(left)\n",
    "Gini_Index(left)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 108,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[['Yellow', 3, 'Apple'], ['Red', 1, 'Grape'], ['Yellow', 3, 'Lemon']]\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "0.6666666666666667"
      ]
     },
     "execution_count": 108,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#All values that dont match the rule are sent to right side and even so it contains impurities\n",
    "print(right)\n",
    "Gini_Index(right)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 109,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[['Red', 1, 'Grape']]\n",
      "Gini Index Of left child node : 0.0\n",
      "[['Green', 3, 'Apple'], ['Yellow', 3, 'Apple'], ['Yellow', 3, 'Lemon']]\n",
      "Gini Index Of left right node : 0.4444444444444444\n"
     ]
    }
   ],
   "source": [
    "#Same thing we repeat for other classes of column 0\n",
    "left,right=find_split(training_data,Rule(0,\"Red\"))\n",
    "print(left)\n",
    "print(\"Gini Index Of left child node :\",Gini_Index(left))\n",
    "print(right)\n",
    "print(\"Gini Index Of left right node :\",Gini_Index(right))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 110,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[['Yellow', 3, 'Apple'], ['Yellow', 3, 'Lemon']]\n",
      "Gini Index Of left child node : 0.5\n",
      "[['Green', 3, 'Apple'], ['Red', 1, 'Grape']]\n",
      "Gini Index Of left right node : 0.5\n"
     ]
    }
   ],
   "source": [
    "#Same thing we repeat for other classes of column 0\n",
    "left,right=find_split(training_data,Rule(0,\"Yellow\"))\n",
    "print(left)\n",
    "print(\"Gini Index Of left child node :\",Gini_Index(left))\n",
    "print(right)\n",
    "print(\"Gini Index Of left right node :\",Gini_Index(right))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Gini index tells us how impure the data after split results in , we would like to have splits that produce \n",
    " the purest possible partitions on the data and also we would like to have larger less impure splits instead of\n",
    " single values/very small partitions of completely pure data.\n",
    " Hence we use a weighted sum of the splits to determine the best rule for splitting,A.K.A Information Gain"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 111,
   "metadata": {},
   "outputs": [],
   "source": [
    "def Info_Gain(left,right,original_gini_index):\n",
    "    #Calculating the weights based on the size of the partition\n",
    "    wt=[ len(left)/(len(left)+len(right)) ,  len(right)/(len(left)+len(right))]\n",
    "    #Now we use the weighted sum to account for the above description\n",
    "    \n",
    "    #subtracting the weighed gini_indexs from current uncertainity(a.k.a gini_index(parent_node))\n",
    "    return original_gini_index - (wt[0]*Gini_Index(left) + wt[1]*Gini_Index(right))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 112,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.125"
      ]
     },
     "execution_count": 112,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Now lets see how much infomation we gain by splitting on the class in column 0 (color)\n",
    "left,right=find_split(training_data,Rule(0,\"Yellow\"))\n",
    "Info_Gain(left,right,Gini_Index(training_data))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 113,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.2916666666666667"
      ]
     },
     "execution_count": 113,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "left,right=find_split(training_data,Rule(0,\"Red\"))\n",
    "Info_Gain(left,right,Gini_Index(training_data))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 114,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.125"
      ]
     },
     "execution_count": 114,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "left,right=find_split(training_data,Rule(0,\"Green\"))\n",
    "Info_Gain(left,right,Gini_Index(training_data))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 115,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Of all the rules we see that the info gain is maximum if we partiton the data with the rule as red for col 0"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 116,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Lets write a function such that it recieves the data and reutns the best rule to split the data on"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 117,
   "metadata": {},
   "outputs": [],
   "source": [
    "def find_best_rule(data):\n",
    "    \"\"\"\n",
    "    Input: \n",
    "        A list of rows\n",
    "    Returns:\n",
    "        best_rule -> A rule object generating the max info gain\n",
    "        max_info_gain -> the info gain generated by splitting the data on the best_rule\n",
    "    \"\"\"\n",
    "    max_info_gain=-1\n",
    "    best_rule=None\n",
    "    #For each column\n",
    "    for col in range(len(data[0])-1):\n",
    "        #For each unique category of the column\n",
    "        for val in np.unique(np.array(data)[:,col]):\n",
    "            feature_col=col\n",
    "            feature_value=val\n",
    "            #Try every rule ==> test_rule\n",
    "            test_rule=Rule(feature_col,feature_value)\n",
    "            #Split the data based on the test_rule\n",
    "            left,right=find_split(data,test_rule)\n",
    "            if len(left)==0 or len(right)==0:\n",
    "                continue\n",
    "            #calculate the information gain for split based on current test_rule\n",
    "            info_gain=Info_Gain(left,right,Gini_Index(data))\n",
    "            #print(f\"Infogain of {col} and {val} is : {info_gain}\")\n",
    "            #If info_gain is the more than known max_info_gain, update\n",
    "            if info_gain>max_info_gain:\n",
    "                max_info_gain=info_gain\n",
    "                best_rule=test_rule\n",
    "    return best_rule,max_info_gain \n",
    "            "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 118,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Testing the best rule function\n",
    "best_rule,max_gain= find_best_rule(training_data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 119,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Check for rule on 0 if value is equal to Red"
      ]
     },
     "execution_count": 119,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "best_rule"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 120,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.2916666666666667"
      ]
     },
     "execution_count": 120,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "max_gain"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 121,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Now that we have the best rule we can split the training data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 122,
   "metadata": {},
   "outputs": [],
   "source": [
    "left_child,right_child=find_split(training_data,best_rule)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 123,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(None, -1)"
      ]
     },
     "execution_count": 123,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Now we need to see if we can split the child nodes furthur to possibly improve the purity\n",
    "find_best_rule(left_child)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If the find_best_rule() function returns (None,-1) it means the node already produces a pure group and splitting isnt requried furthur."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 124,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(Check for rule on 0 if value is equal to Green, 0.1111111111111111)"
      ]
     },
     "execution_count": 124,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "find_best_rule(right_child)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## How we have all the tools we need to start building the tree"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 125,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Lets define a class Node for the tree"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 164,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Node():\n",
    "    def __init__(self,data,rule=None):\n",
    "        self.data=data\n",
    "        self.rule=rule\n",
    "        self.left=None #True branch\n",
    "        self.right=None #Flase branch\n",
    "        "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 128,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Construct Tree"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 165,
   "metadata": {},
   "outputs": [],
   "source": [
    "ctr=0\n",
    "def make_tree(training_data,curr_node,prev_score):\n",
    "    \"\"\"\n",
    "    Input : \n",
    "            training_data : a list of rows for building the tree\n",
    "            curr_node : Initially while calling this function create a root node of type <Node> and pass it here.\n",
    "            prev_score : Initally defaults to 0. Through recursion values are pass in the process of bulding the tree.\n",
    "    \"\"\"\n",
    "    \n",
    "    global ctr\n",
    "    \n",
    "    # If we dont recieve any data then we just return \n",
    "    if len(training_data)==0 :\n",
    "        return\n",
    "    \n",
    "    #If we do recieve data we store it in the node's data attribute\n",
    "    curr_node.data=training_data\n",
    "    \n",
    "    #Then we run the best_rule function over this data \n",
    "    best_rule=find_best_rule(curr_node.data)\n",
    "    \n",
    "    #If the purest split is alreadt achieved then we wont have a best rule to split hence it will return None so we return\n",
    "    if best_rule[0]==None:\n",
    "        print(curr_node.data)\n",
    "        return\n",
    "    \n",
    "    #Bunch of prints to understand whats happeneing\n",
    "    print(\"datastarts\",\"=====\"*ctr)\n",
    "    print(\"This DATA :\")\n",
    "    [print(row) for row in curr_node.data]\n",
    "    print(\"is SPLIT On the rule \",best_rule,\"\\n\")\n",
    "    print(\"dataends\",\"=====\"*ctr)\n",
    "    \n",
    "    # If the parent node has better information gain comparent to splitting by a rule dont split \n",
    "    if prev_score<best_rule[1]:\n",
    "        #If we get a rule which improves the info-gain we place it into the node\n",
    "        curr_node.rule=best_rule\n",
    "        print(curr_node.rule,\"\\n\\n\")\n",
    "        #And split the data based on the best rule to constrcut and assign left and right child nodes\n",
    "        left_child,right_child=find_split(training_data,curr_node.rule[0])\n",
    "        curr_node.left=Node(left_child)\n",
    "        curr_node.right=Node(right_child)\n",
    "        print(\"Start Left sub tree\")\n",
    "        ctr+=1\n",
    "        #Recursive function call to construct the left sub tree\n",
    "        make_tree(curr_node.left.data,curr_node.left,curr_node.rule[1])\n",
    "        print(\"Left sub tree done\")\n",
    "        ctr=1\n",
    "        #Recursive function call to construct the right sub tree\n",
    "        print(\"Start Right sub tree\")\n",
    "        make_tree(curr_node.right.data,curr_node.right,curr_node.rule[1])\n",
    "        print(\"Right sub tree done\")\n",
    "        \n",
    "    return"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 166,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "datastarts \n",
      "On the rule  (Check for rule on 0 if value is equal to Red, 0.2916666666666667) we split the Current data:\n",
      "\n",
      "['Green', 3, 'Apple']\n",
      "['Yellow', 3, 'Apple']\n",
      "['Red', 1, 'Grape']\n",
      "['Yellow', 3, 'Lemon']\n",
      "dataends \n",
      "(Check for rule on 0 if value is equal to Red, 0.2916666666666667) \n",
      "\n",
      "\n",
      "Start Left sub tree\n",
      "[['Red', 1, 'Grape']]\n",
      "Left sub tree done\n",
      "Start Right sub tree\n",
      "datastarts =====\n",
      "On the rule  (Check for rule on 0 if value is equal to Green, 0.1111111111111111) we split the Current data:\n",
      "\n",
      "['Green', 3, 'Apple']\n",
      "['Yellow', 3, 'Apple']\n",
      "['Yellow', 3, 'Lemon']\n",
      "dataends =====\n",
      "Right sub tree done\n"
     ]
    }
   ],
   "source": [
    "#First create a empty root node\n",
    "root=Node([])\n",
    "#Pass the training data along with the root and initial score to build the tree\n",
    "make_tree(training_data,root,0)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now that we have constructed the tree all we need to do is traverse the tree for a test sample, the rule at each node is matched and the appropriate child is visited until we reach a leaf node which outputs the majority labels contained at its node.data as the output class for that leaf."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 168,
   "metadata": {},
   "outputs": [],
   "source": [
    "def traverse_tree(root,sample):\n",
    "    #If we dont get a root node return\n",
    "    if root==None:\n",
    "        return\n",
    "    #If we are at the leaf node , print out all the labels and their percentage representation at the leaf\n",
    "    if root.left==None and root.right==None:#root.rule==None:\n",
    "        res_classes=np.array(root.data)[:,-1]\n",
    "        class_label,count=np.unique(res_classes,return_counts=True)\n",
    "        [print(class_label[x],\" : \",count[x]/sum(count)) for x in range(len(class_label)) ]\n",
    "        return\n",
    "    \n",
    "    #If it is not a leaf node match the values against the rule at the node and take the appropriate path\n",
    "    elif root.rule[0]!=None:\n",
    "        print(\"Checked for :\",\"\\t\",root.rule)\n",
    "        if root.rule[0].check_rule(sample):\n",
    "            root=root.left\n",
    "        else:\n",
    "            root=root.right\n",
    "    \n",
    "    #print(root)\n",
    "    #print(root.left)\n",
    "    #print(root.right)\n",
    "    \n",
    "    #Finally make a recursive call to keep traversing until a leaf node is reached\n",
    "    traverse_tree(root,sample)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 175,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Same as above , we construct the tree \n",
    "root=Node([])\n",
    "make_tree(training_data,root,0)"
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
       "'Apple'"
      ]
     },
     "execution_count": 176,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Traverse the tree \n",
    "####\n",
    "# NOTE\n",
    "# Though we have only 2 input features we need to pass the sample having an element in place of the class label\n",
    "# Here ive left it as a ? , as it is ignored but the len() of the list matters during index slicing \n",
    "####\n",
    "traverse_tree(root,[\"Black\",1,\"?\"])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 177,
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
       "      <th>Outlook</th>\n",
       "      <th>Temperature</th>\n",
       "      <th>Humidity</th>\n",
       "      <th>Wind</th>\n",
       "      <th>Answer</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>sunny</td>\n",
       "      <td>hot</td>\n",
       "      <td>high</td>\n",
       "      <td>weak</td>\n",
       "      <td>no</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>sunny</td>\n",
       "      <td>hot</td>\n",
       "      <td>high</td>\n",
       "      <td>strong</td>\n",
       "      <td>no</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>overcast</td>\n",
       "      <td>hot</td>\n",
       "      <td>high</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>rain</td>\n",
       "      <td>mild</td>\n",
       "      <td>high</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>rain</td>\n",
       "      <td>cool</td>\n",
       "      <td>normal</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>rain</td>\n",
       "      <td>cool</td>\n",
       "      <td>normal</td>\n",
       "      <td>strong</td>\n",
       "      <td>no</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>overcast</td>\n",
       "      <td>cool</td>\n",
       "      <td>normal</td>\n",
       "      <td>strong</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>sunny</td>\n",
       "      <td>mild</td>\n",
       "      <td>high</td>\n",
       "      <td>weak</td>\n",
       "      <td>no</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>sunny</td>\n",
       "      <td>cool</td>\n",
       "      <td>normal</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>rain</td>\n",
       "      <td>mild</td>\n",
       "      <td>normal</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>sunny</td>\n",
       "      <td>mild</td>\n",
       "      <td>normal</td>\n",
       "      <td>strong</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>overcast</td>\n",
       "      <td>mild</td>\n",
       "      <td>high</td>\n",
       "      <td>strong</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>overcast</td>\n",
       "      <td>hot</td>\n",
       "      <td>normal</td>\n",
       "      <td>weak</td>\n",
       "      <td>yes</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>rain</td>\n",
       "      <td>mild</td>\n",
       "      <td>high</td>\n",
       "      <td>strong</td>\n",
       "      <td>no</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "     Outlook Temperature Humidity    Wind Answer\n",
       "0      sunny         hot     high    weak     no\n",
       "1      sunny         hot     high  strong     no\n",
       "2   overcast         hot     high    weak    yes\n",
       "3       rain        mild     high    weak    yes\n",
       "4       rain        cool   normal    weak    yes\n",
       "5       rain        cool   normal  strong     no\n",
       "6   overcast        cool   normal  strong    yes\n",
       "7      sunny        mild     high    weak     no\n",
       "8      sunny        cool   normal    weak    yes\n",
       "9       rain        mild   normal    weak    yes\n",
       "10     sunny        mild   normal  strong    yes\n",
       "11  overcast        mild     high  strong    yes\n",
       "12  overcast         hot   normal    weak    yes\n",
       "13      rain        mild     high  strong     no"
      ]
     },
     "execution_count": 177,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Now lets try it with a little bigger dataset which looks like this\n",
    "df=pd.read_csv('Datasets/id3.csv')\n",
    "df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 178,
   "metadata": {},
   "outputs": [],
   "source": [
    "#First lets convert this dataframe into a list of rows\n",
    "data=df.values.tolist()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 179,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Create a root node and construct the tree\n",
    "root=Node([])\n",
    "make_tree(data,root,0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 180,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'yes'"
      ]
     },
     "execution_count": 180,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#traverse the tree for a test sample\n",
    "traverse_tree(root,[\"sunny\",\"hot\",\"normal\",\"strong\",\"\"])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Here all the logging and prints are removed for a cleaner output and now in the traverse tree function we only return the label with the most confidence/representation in the leaf node."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 181,
   "metadata": {},
   "outputs": [],
   "source": [
    "ctr=0\n",
    "def make_tree(training_data,curr_node,prev_score):\n",
    "    global ctr\n",
    "    if len(training_data)==0 :\n",
    "        return\n",
    "    \n",
    "    curr_node.data=training_data\n",
    "    best_rule=find_best_rule(curr_node.data)\n",
    "    if best_rule[0]==None:\n",
    "        #print(curr_node.data)\n",
    "        return\n",
    "    #print(\"datastarts=====\"*ctr)\n",
    "    #print(\"On the rule \",best_rule,\"we split the Current data:\\n\")\n",
    "    #[print(row) for row in curr_node.data]\n",
    "    #print(\"dataends=====\"*ctr)\n",
    "    \n",
    "    if prev_score<best_rule[1]:\n",
    "        curr_node.rule=best_rule\n",
    "        #print(curr_node.rule,\"\\n\\n\")\n",
    "        left_child,right_child=find_split(training_data,curr_node.rule[0])\n",
    "        curr_node.left=Node(left_child)\n",
    "        curr_node.right=Node(right_child)\n",
    "        #print(\"Start Left sub tree\")\n",
    "        ctr+=1\n",
    "        make_tree(curr_node.left.data,curr_node.left,curr_node.rule[1])\n",
    "        #print(\"Left sub tree done\")\n",
    "        ctr=1\n",
    "        #print(\"Start Right sub tree\")\n",
    "        make_tree(curr_node.right.data,curr_node.right,curr_node.rule[1])\n",
    "        #print(\"Right sub tree done\")\n",
    "    return"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 182,
   "metadata": {},
   "outputs": [],
   "source": [
    "def traverse_tree(root,sample):\n",
    "    \n",
    "    if root.left==None and root.right==None:#root.rule==None:\n",
    "        res_classes=np.array(root.data)[:,-1]\n",
    "        class_label,count=np.unique(res_classes,return_counts=True)\n",
    "        #[print(class_label[x],\" : \",count[x]/sum(count)) for x in range(len(class_label)) ]\n",
    "        ind=np.argmax(count)\n",
    "        #print(class_label[ind])  # prints the most frequent element\n",
    "        return class_label[ind]\n",
    "    \n",
    "    elif root.rule[0]!=None:\n",
    "        #print(\"Checked for :\",\"\\t\",root.rule)\n",
    "        if root.rule[0].check_rule(sample):\n",
    "            root=root.left\n",
    "        else:\n",
    "            root=root.right\n",
    "    \n",
    "    #print(root)\n",
    "    #print(root.left)\n",
    "    #print(root.right)\n",
    "    return traverse_tree(root,sample)\n",
    "     "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 184,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'yes'"
      ]
     },
     "execution_count": 184,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "res=traverse_tree(root,[\"sunny\",\"hot\",\"normal\",\"strong\",\"\"])\n",
    "res"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Now time to try this on a big dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 185,
   "metadata": {},
   "outputs": [],
   "source": [
    "adf=pd.read_csv('Datasets/adult.data.csv',header=None).sample(frac=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 187,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(32561, 5)"
      ]
     },
     "execution_count": 187,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data=adf[[1,5,8,9,14]].values\n",
    "data.shape #around 32k rows"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 188,
   "metadata": {},
   "outputs": [],
   "source": [
    "train=data[:-3000]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 189,
   "metadata": {},
   "outputs": [],
   "source": [
    "test=data[-3000:]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 190,
   "metadata": {},
   "outputs": [],
   "source": [
    "#we follow the same procedure, create a root and then construct the tree \n",
    "root=Node([])\n",
    "make_tree(train,root,0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 157,
   "metadata": {},
   "outputs": [],
   "source": [
    "#Let us use a for loop to go over a set of test sample ,3000 rows,and we'll try to gauge the models accuracy on this set. \n",
    "\n",
    "#initialize the score to be 0\n",
    "score=0\n",
    "#Loop over each test sample and obtain the prediction according to the tree and then compare it with the true value \n",
    "for row in test:\n",
    "    if row[-1]==traverse_tree(root,row):\n",
    "        #if prediction is true then increment score\n",
    "        score+=1"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 151,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.754"
      ]
     },
     "execution_count": 151,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Accuracy\n",
    "score/3000"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# End"
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
   "version": "3.7.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}

```