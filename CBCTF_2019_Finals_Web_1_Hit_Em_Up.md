Find the flag: ```http://hitemup.cb.ctf:8181/```.

The website shows a page with seemingly random characters. Upon closer inspection, I noticed the texts are reversed (duh). Reading them backwards, it says I'm the __nth visitor__ and that __every 1500 is a lucky number__. I tested the counter by refreshing the page and observed that the visitor count increments by a random number (either because the other teams are visiting or there's a script increasing the counter). 

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

I initially tried solving the flag through the console. The following script freezed my browser:

``` JavaScript
while (parseInt(document.getElementsByTagName('h1')[1].innerHTML.split(' ')[5].split('').reverse().join("").slice(0,-2))%1500 != 0) { location.href = "http://hitemup.cb.ctf:8181/"; }
```

Worth the try but time to learn Python! From the hint, I knew I needed to be the 1500th or any nth divisible by 1500th visitor. Basically, I need to load the page multiple times (hundreds of time) until I get the lucky number. I came up with the following script:

``` Python
import requests;

found = False;

def visit():
  # Load the website and split the HTML lines into a list.
  r = requests.get('http://hitemup.cb.ctf:8181/')
  lines = r.content.split("\n")
    
  # loop through each line in the list.
  for line in lines:
    # remove whitespace and reverse the text.
    reverse = line.strip()[::-1]
    
    # Check if the word visitor is in the line, that's where the visitor count is located.
    if "visitor" in reverse:
      # split the line into a list, take index 3 (the count), strip the "th" and change to int.
      visit_no = int(reverse.split()[3][0:-2])
      print visit_no
            
      # check if visitor count is equal to 1500 or if divisible by 1500 using modulus.
      # if True and only if True, print the whole content of the website.
      if visit_no % 1500 == 0:
        print r.content
        found = True;

# run the visit() function defined earlier untli the flag is found.
while (not found):
    visit()
```

Since I'm a Python noob. The script didn't actually stop when it found the flag. So I simply stared at the numbers printed on the screen until the flag is found (a bunch of HTML will suddenly appear amongst the numbers) and manually stop the script (CTRL + z).

I didn't manage to capture what I found but basically, that's how you get the flag.
