Find the flag: ```http://hitemup.cb.ctf:8181/```.

The website shows a page with with obfuscated texts. Upon closer inspection, I noticed the texts are reversed. I decided to just read them backwards. It tells that I'm the __nth visitor__ and that __every 1500 is a lucky number__. I tested the counter by refreshing the page and noticed that the number increments by a random amount, either because the other teams are visiting or there's a script increasing the counter. 

``` HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>ɿoɿɿiM</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <link href="/bootstrap/css/bootstrap.css" rel="stylesheet">
    <style>
      body {
        padding-top: 300px;
      }
    </style>
    <link href="/bootstrap/css/bootstrap-responsive.css" rel="stylesheet">
  </head>

  <body>
  
    <div class="container">
      <h1></h1>
      
      <p> ?etisbew a fo enecs eht dniheb morf ekil kool ti woh rednow reve uoy evah </p>
      <p>?wonk ot ekil uoy dluow esle tahW .thgir looc ytterP</p>

      <h1> .egap siht fo rotisiv ht49793  eht era uoY</h1>
      !rebmun ykcul a si 0051 yreve dias yehT </p>
    </div> <!-- /container -->
  </body>
</html>
```

I initially tried solving the flag on the browser's console. The following script freezed my browser:

``` JavaScript
while (parseInt(document.getElementsByTagName('h1')[1].innerHTML.split(' ')[5].split('').reverse().join("").slice(0,-2))%1500 != 0) { location.href = "http://hitemup.cb.ctf:8181/"; }
```

Time to learn Python! From the hint, I knew that I needed to be the 1500th or any nth divisible by 1500. I need to somehow loop my script until I'm exactly that nth visitor. I came up with the following script:

``` Python
import requests;

found = False;

def visit():
  # basically curl a website and split the lines into a list
  r = requests.get('http://hitemup.cb.ctf:8181/')
  lines = r.content.split("\n")
    
  # loop through each line in the list, reverse the text, find the visitor count line and get the visitor count.
  for line in lines:
    reverse = line.strip()[::-1]
    
    if "visitor" in reverse:
      visit_no = int(reverse.split()[3][0:-2])
      print visit_no
            
      # check if visitor count is equal to 1500 or if dividable by 1500 using modulus
      # if True, print the whole content of the website
      if visit_no % 1500 == 0:
        print r.content
        found = True;

while (not found):
    visit()
```

Since I'm a python noob. The script didn't actually stop when it found the flag. So I simply stared at the numbers printed on the screen until the flag is found (a bunch of HTML will suddenly appear amongst the numbers) and manually stop the script (CTRL + z).

I didn't manage to capture what I found but basically, that's how you get the flag.
