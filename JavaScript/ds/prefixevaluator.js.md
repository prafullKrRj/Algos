```js
/*
Prefix expression:- An expression is called the prefix expression if the operator appears in the expression before the operands
Example: operator operand1 operand2
This program is for evaluating prefix expression.
In this we evaluate the given prefix expression using stack
*/

// creating stackarr 
var stackarr=[];

// Initialize topp variable with -1
var topp=-1;

// creating evalstack
var evalstack=[];

// function for evaluating prefix expression
function prefixevaluator(evalst)
{
    eval=evalst.split(" ");
   for(var i=eval.length-1;i>=0;i--)
   {
       if(!operator(eval[i]))
       {
           push(eval[i]);
       }
       else
       {
           var op1=parseInt(pop());
           var op2=parseInt(pop());
           
           if(eval[i]=="+")
           {
               push(op1+op2);
           }
           else if(eval[i]=="-")
           {
               push(op1-op2);
           }
           else if(eval[i]=="*")
           {
               push(op1*op2);
           }
           else
           {
               push(op1/op2);
           }
       }
   }
   console.log(pop());
}

// function for push 
function push(e)
{
   topp++;
   stackarr[topp]=e;
}

// function for pop out
function pop()
{
    if(topp==-1)
    return 0;
    else
    {
        var popped_ele=stackarr[topp];
        topp--;
        return popped_ele;
    }
}

// function for checking whether operator or not 
function operator(op)
{
    if(op=='+' || op=='-' || op=='^' || op=='*' || op=='/' || op=='(' || op==')')
    {
        return true;
    }
    else
    return false;
}

// function for checking precedency
function precedency(pre)
{
    if(pre=='@' || pre=='(' || pre==')')
    {
        return 1;
    }
    else if(pre=='+' || pre=='-')
    {
        return 2;
    }
    else if (pre=='/' || pre=='*')
    {
        return 3;
    }
    else if(pre=='^')
    {
        return 4;
    }
    else
    return 0;
}

 //In-Built readline module
const readline = require("readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const getLine = (function () {
  const getLineGen = (async function* () {
    for await (const line of rl) {
      yield line;
    }
  })();
  return async () => (await getLineGen.next()).value;
})();

const main = async () => {

    //Taking prefix expression as input
    console.log("Enter the prefix expression(Please provide space after each operator or operand)");
    var number1 = String(await getLine());

    // Caling Function prefixevaluator
    prefixevaluator(number1);

    //To close the program
    process.exit(0);
  };

  main();

 /*

 Input: 
 Enter the prefix expression(Please provide space after each operator or operand): + * 2 3 4
 
 Output:
 10

 Input:
 Enter the prefix expression(Please provide space after each operator or operand): + - 2 3 7

 Output:
 6

 Time Complexity: O(n)
 Space Complexity: O(n)

 */

```