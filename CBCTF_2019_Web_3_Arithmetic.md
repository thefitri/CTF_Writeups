The website shows a math challenge and a field to submit answer. It hints that there's 500 questions to answer. I tried answering a few questions manually and noticed that the 2nd number and operator doesn't change (i.e n - 100). I thought of creating a python script but to save time, I decided to try everything on web console.

First, I needed to get the numbers inside the ```<pre>``` tags. I wasn't aware of the getElementsByTagName so I used getElementsByClass on the parent div instead and browse through the childNodes until I get to the element I wanted (```<pre>```). I then used the data and split method to get the numbers and operator(s) into an array.

```Javascript
numbers = (document.getElementsByClassName("container")[1].childNodes[5].childNodes[10].childNodes[0].data.split(" ")
```

After that I needed to figure out a way to submit the answer through the form. But I realised the URL variable of the current page is the answer to the previous question. So all I needed to do is redirect the page with the answer as the "math" variable:

```Javascript
location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + answer
```

Repeat that a 100 times and you'll get a section of the flag. The 2nd (and 3rd) numbers and operators eventually varies on part 4. Part 5 introduces a simple algebra but it was easy to convert. Here's my JavaScript codes:

```Javascript
location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (document.getElementsByClassName("container")[1].childNodes[5].childNodes[11].childNodes[0].data.split(" ")[1] - 100)
# CBCTF{500

location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (document.getElementsByClassName("container")[1].childNodes[5].childNodes[10].childNodes[0].data.split(" ")[0] * 100 - 300)
# problem

number = document.getElementsByClassName("container")[1].childNodes[5].childNodes[11].childNodes[0].data.split(" ")[1]; location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number * number + (Number(number) + Number(number)))
# But

number = document.getElementsByClassName("container")[1].childNodes[5].childNodes[10].childNodes[0].data.split(" "); location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number[1] ** number[4])
# MathA

number = document.getElementsByClassName("container")[1].childNodes[5].childNodes[10].childNodes[0].data.split(" "); location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number[6] - (number[2] * number[4]))
# intOne}
```

The flag is : CBCTF{500problemButMathAintOne}

The equation on part 5 was:
```
Z - X * X = Y
Z - XX = Y
Z = Y + XX

e.g

Z - 2 * 2 = 10
Z - 4 = 10
Z = 10 - 4
Z = 6
```

The codes could've been simplified to these:

```Javascript
location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (document.getElementsByTagNAme("pre")[0].data.split(" ")[1] - 100)

location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (document.getElementsByTagNAme("pre")[0].data.split(" ")[0] * 100 - 300)

number = document.getElementsByTagNAme("pre")[0].data.split(" ")[1]; location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number * number + (Number(number) + Number(number)))

number = document.getElementsByTagNAme("pre")[0].data.split(" "); location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number[1] ** number[4])

number = (document.getElementsByTagNAme("pre")[0].data.split(" "); location.href = "http://cbweb.cyberbattle.info:8008/index.php?name=lowkey&math=" + (number[6] - (number[2] * number[4]))
```

How did I run it a 100 times on the web console? ... I pressed up and enter 100 times for every parts. Could've been automated but ain't nobody got time for that.
