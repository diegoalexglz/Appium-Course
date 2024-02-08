# INTRO

### Why design techniques

- Wider coverage
- More defect detections

### Types

- Error guessing
- Equivalence partitioning
- Boundary value analysys
- Decision table
- State transition



# ERROR GUESSING

Consists in trying to guess the error. It doesn't follow rules.

> EXAMPLE

if the app says it can accepts only numeric digits, we would try to enter (-) and not (+) data, decimal digits, alphanumeric digits, special characters, spaces between numbers, etc.



# EQUIVALENCE PARTITIONING

It states that if we have a text field and our requirement is that the field's must accept input within a specific range, we can divide the range into classes and test one element per class, instead of the whole class.

> EXAMPLE

If our field supposedly accepts input in the 1-500 range, we can create these classes:

- -100 - 0
- 1 - 100
- ...
- 501 - 600

And then, out of each class, we'll only enter one digit and check if it is accepted or not.

- If it is, then we assume that all the numbers contained in that class will be accepted, too.

  This is known as: '**the whole class passed**' ✅

- If it isn't, we assume the opposite.

  This is known as '**the whole class failed**' ❎



# BOUNDARY VALUE ANALYSYS

It consists in testing at the boundaries between partitions.

So, if we have a range of values between A and B, we have to design test cases for:

- A
- A±1
- B
- B±1

> EXAMPLE

If our value range is 1-10, our test cases must be for:

- -1, 1, 1
- 9, 10, 11



# DECISION TABLE

In this technique we check for multiple conditions/combinations and criteria.

The **conditions** are taken **as an input**, and **actions** are taken **as an output**.

The **number of test cases** to be designed are **equal to the number of rules**, which, in turn, will be equal to 
$$
2^C
$$
where **C is the number of conditions**.



> EXAMPLE

Company states that:

- New customers get       ->   15% discount
- Regular customers get  ->   10% discount
- Coupon holders get       ->   30% discount



In this example, we have:
$$
No. Of Test Cases = 2^3 = 8
$$


And we create our table with the combinations (quite similar to binary digital logic for boolean expressions):

<table style="width: 100%; text-align: center; vertical-align: middle; border-collapse: collapse;">
    <thead>
        <tr>
            <th width="11%">Conditions/Rule</th>
            <th width="11%">R1</th>
            <th width="11%">R2</th>
            <th width="11%">R3</th>
            <th width="11%">R4</th>
            <th width="11%">R5</th>
            <th width="11%">R6</th>
            <th width="11%">R7</th>
            <th width="11%">R8</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>New C.</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
        </tr>
        <tr>
            <td>Reg C.</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
        </tr>
        <tr>
            <td>Coupon</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>0</td>
            <td>1</td>
            <td>0</td>
        </tr>
        <tr>
            <td>Result</td>
            <td>X</td>
            <td>X</td>
            <td>✔</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
    </tbody>
</table>

(Notice that in this pattern we used 1 and 0 to represent True and False)

**Each rule (R1-R8) forms a test case**.

Let's analyze some cases:

- <u>R3</u>: Person is a new customer and has a coupon, so he gets 45% discount, and **the case is valid**.

- <u>R1 & R2</u>: Person may or may not have a coupon, but he surely can't be a new customer and a regular customer at the same time, so we say **the case is invalid**.
- <u>R8</u>: Same happens here, either you are a new or a regular customer, you must be one of them.

Finally, we go and check what's the actual app output for both valid and invalid cases and see what it returns.
