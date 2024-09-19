## Problem
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed 104.

 

### Example 1:

Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0 
(2-(1-1)) = 2

### Example 2:

Input: expression = "2*3-4*5"
Output: [-34,-14,-10,-10,10]
Explanation:
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
 

### Constraints:

1 <= expression.length <= 20
expression consists of digits and the operator '+', '-', and '*'.
All the integer values in the input expression are in the range [0, 99].
The integer values in the input expression do not have a leading '-' or '+' denoting the sign.

## My Solution

``` ts
function diffWaysToCompute(expression: string): number[] {
  const operations = {
    "+": (number1: number, number2: number) => number1 + number2,
    "-": (number1: number, number2: number) => number1 - number2,
    "*": (number1: number, number2: number) => number1 * number2
  }
  const dfs = ((numbers: string): number[] => {
    //es un numero pero es texto
    if (/^\d+$/.test(numbers)) {
      return [parseInt(numbers)];
    }
    //
    const result: number[] = [];
    //Recorremos el array de numeros y operadores
    for (let i = 0; i < numbers.length; i++) {
      const operator = numbers[i];
      //Cuando encontremos un operador
      if (['-', '+', '*'].includes(operator)) {
        //obtenemos los numeros de la izquierda y la derecha y obtenemos los resultados
        const left = dfs(numbers.slice(0, i));
        const right = dfs(numbers.slice(i + 1));

        for (const numberFromLeft of left) {
          for (const numberFromRight of right) {
            result.push(operations[operator](numberFromLeft, numberFromRight));
          }
        }
      }
    }
    return result;
  });

  return dfs(expression);
}
```
