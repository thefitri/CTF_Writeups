# Question
I love adding up previous two numbers.

def ct(f): return f if f < 2 else ct(f-2) + ct(f-1)
print "Your flag is:" + str(ct(2019))[:42]

# Approach and Answer

*Note: I tweaked my original answer on this write-up to make it easier to understand. So it's different than the one I shared earlier.*

Based on the syntax and keywords (def, print), we can assume the above is a python script. The first thing I did was to execute the script. It returns with **"RuntimeError: maximum recursion depth exceeded"**.

The error message was pretty obvious. But to be honest, I didn't understand what was wrong. So I rewrote the script so it's easier to understand (I've commented them below):

```python
# define a function called "ct" with input "f".
def ct(f):
    # return "f" if it's less than 2.
    if f < 2:
        return f
    # perform recursion on values f-2 and f-1 and perform addition on return values.
    # example if f=4 then ct(4-2) + ct(4-1) and so on.
    else:
	return ct(f-2) + ct(f-1)

# print final return value from function ct.
# convert return value to string and return only first 42 chars (0-41).
# it's easier to see what's going on if we use a different input.
print "Your flag is:" + str(ct(1))[:42]
print "Your flag is:" + str(ct(2))[:42]
print "Your flag is:" + str(ct(3))[:42]
print "Your flag is:" + str(ct(4))[:42]
print "Your flag is:" + str(ct(5))[:42]
print "Your flag is:" + str(ct(6))[:42]
print "Your flag is:" + str(ct(7))[:42]
print "Your flag is:" + str(ct(8))[:42]
print "Your flag is:" + str(ct(9))[:42]
print "Your flag is:" + str(ct(10))[:42]
print "Your flag is:" + str(ct(11))[:42]
print "Your flag is:" + str(ct(12))[:42]
print "Your flag is:" + str(ct(100))[:42]
print "Your flag is:" + str(ct(1000))[:42]
```

Output:
```
Your flag is: 1
Your flag is: 1
Your flag is: 2
Your flag is: 3
Your flag is: 5
Your flag is: 8
Your flag is: 13
Your flag is: 21
Your flag is: 34
Your flag is: 55
Your flag is: 89
Your flag is: 144
```

At input 100, the script never returned a value (too many recursions). We can assume that we have to write an alternate algorithm. My first assumption was that it performed an addition of all prime numbers between 0 and input f. That's not it and the hint is actually in the question: "I love adding the previous two number".

Looking at the output, we can see the **output is equal to the sum of the previous two output**. My original answer looks like this:

```python
# I don't need an input value as it will be hardcoded inside the script.
def countme():
    # I initialise the array with the output for input value "1" and "2".
    a = [1,1]
    # Loop for input 3-12 or the number of times to loop.
    for x in range(3,13):
	# add the previous two ouput or last two values in array a.
        output = a[-1] + a[-2]
	# append the latest output to array a.
        a.append(output)
    
    # print the last value in array a.
    print str(a[-1])[:42]

print "Your flag is:" + str(ct(1000))[:42]            
countme()
```

Here's the output

```
Your flag is:144
144
None
```

To check if my algorithm is correct, I made sure that ct(12) is equal to my output, and it is. So we can now change range(3,13) to range(3,2020) to perform addition of previous two values from 3 to 2019 and you get the value for the **flag** ...

```
394966864727721633978749819285325869377850
```

**which is WRONG!**. I'm not sure if my algorithm was wrong or if the organiser entered the wrong value (Remember, we checked the return value of input 12 and both returned 144). No fret, I'll just increase the range tp (3,2021) and we got the right flag. YAY?! *[confused face]*

```
639069811559435586651349273985287139381962
```

**CBCTF{639069811559435586651349273985287139381962}**

Expanding on this we can rewrite our code as such:

```python
def findflag(f):
    n_minus2 = 0
    n_minus1 = 0
    n = 0

    for x in range(f):
        n = n_minus2 + n_minus1

        n_minus2 = n_minus1
        n_minus1 = n
        if n == 0:
            n_minus1 = 1

        return n

print "Your flag is: CBCTF{" + str(findflag(2019))[:42] + "}"
```

Now if we go back to the original code and input any number. The output will be equal to the code above. The value of the right answer is actually for ct(2020) or findflag(2020) and not ct(2019).

*[derp face]*
