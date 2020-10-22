# Loops

## for loops

We'll start by building up a really simple for loop. Then we'll make it more complicated.

The basic syntax:
```R
for temp_var in some_vector:
    do something, probably with temp_var
```

One by one, the value of each item in `some_vector` gets written into `temp_var` until you have looked at every item in `some_vector`

You can use `temp_var` after the `for` loop has completed; it'll hold whatever its value was during the final time through the loop.

If you want output from your `for` loop to the console, you need to explicitly print it. This isn't a huge deal; we're usually using for loops to write into vectors, not to print to console, so you're going to forget that's even a thing. (I do.) But this, for instance, won't output anything:


```R
for (x in 1:50) {
    x
    #print(x)
}
```


```R
raven <- c("Once", "upon", "a", "midnight", "dreary", "while", "I", "pondered")

# unpacks, a lot like Python's for loops
for (word in raven) { 
    cat(word)
    cat(" ")
    # coming back to this in a minute
    #word <- "coffee"
}

raven
```

    Once upon a midnight dreary while I pondered 


<ol class=list-inline>
	<li>'Once'</li>
	<li>'upon'</li>
	<li>'a'</li>
	<li>'midnight'</li>
	<li>'dreary'</li>
	<li>'while'</li>
	<li>'I'</li>
	<li>'pondered'</li>
</ol>




```R
# cat is like print, but it doesn't print the ugly quotation marks
print(raven[1])
cat(raven[1])
# or any spaces
cat(raven[2])
```

    [1] "Once"
    Onceupon


```R
# the book pointed out, you can unpack a vector of numbers, too
# and you can use it to index another vector!

raven <- c("Once", "upon", "a", "midnight", "dreary", "while", "I", "pondered")

for (i in 1:length(raven)) {
    # and address items by their indices
    cat(raven[i])
    cat(" ")
    # coming back to this in a moment
    #raven[i] <- "coffee"
}
raven
```

    Once upon a midnight dreary while I pondered 


<ol class=list-inline>
	<li>'Once'</li>
	<li>'upon'</li>
	<li>'a'</li>
	<li>'midnight'</li>
	<li>'dreary'</li>
	<li>'while'</li>
	<li>'I'</li>
	<li>'pondered'</li>
</ol>



### Let's practice a for loop problem together

Let's do a classic for loop problem, in R. It's fizzbuzz time!

Print the numbers 1-100, _except_ if a number is divisible by 3, print "fizz"; if it's divisible by 5, print "buzz"; and if it's divisible by both 3 _and_ 5, print "fizzbuzz"


```R

```

OK, we can loop through a vector, albeit a simple one. We can use a for loop to do some math, and that's good, too. Let's use a for loop to go through a data frame, yeah? (We've done this before. Maybe we should do some examples on the fly, in addition to the one I've prepared?)


```R
# first, let's get our dataframe
clp_wifi <- read.csv("clp_wifi.csv", stringsAsFactors=FALSE)
head(clp_wifi)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>129484           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>



Let's say we wanted to calculate **the number of minutes patrons spent on wifi in each library branch in 2016.** We can combine looping and logical subsetting to do that in very few lines of code:


```R
libraries <- unique(clp_wifi$Name)
minutes_in_2016 <- vector()

for (library in libraries) {
    temp <- sum(clp_wifi$WifiMinutes[clp_wifi$Name == library & clp_wifi$Year == 2016])
    minutes_in_2016 <- c(minutes_in_2016, temp)
}

names(minutes_in_2016) <- libraries

minutes_in_2016
```


<dl class=dl-horizontal>
	<dt>ALLEGHENY LIBRARY</dt>
		<dd>1916766</dd>
	<dt>BEECHVIEW LIBRARY</dt>
		<dd>846955</dd>
	<dt>BROOKLINE LIBRARY</dt>
		<dd>1139819</dd>
	<dt>CARRICK LIBRARY</dt>
		<dd>725997</dd>
	<dt>DOWNTOWN &amp; BUSINESS LIBRARY</dt>
		<dd>5087258</dd>
	<dt>EAST LIBERTY LIBRARY</dt>
		<dd>3329665</dd>
	<dt>HAZELWOOD LIBRARY</dt>
		<dd>1145404</dd>
	<dt>HILL DISTRICT LIBRARY</dt>
		<dd>1067189</dd>
	<dt>HOMEWOOD LIBRARY</dt>
		<dd>1671576</dd>
	<dt>KNOXVILLE LIBRARY</dt>
		<dd>732340</dd>
	<dt>LAWRENCEVILLE LIBRARY</dt>
		<dd>583610</dd>
	<dt>LIBRARY FOR THE BLIND &amp; PHYSICALLY HANDICAPPED</dt>
		<dd>602737</dd>
	<dt>MAIN (OAKLAND) LIBRARY</dt>
		<dd>16855952</dd>
	<dt>MOUNT WASHINGTON LIBRARY</dt>
		<dd>535389</dd>
	<dt>SHERADEN LIBRARY</dt>
		<dd>1223950</dd>
	<dt>SOUTH SIDE LIBRARY</dt>
		<dd>1298292</dd>
	<dt>SQUIRREL HILL LIBRARY</dt>
		<dd>6898729</dd>
	<dt>WEST END LIBRARY</dt>
		<dd>655676</dd>
	<dt>WOODS RUN LIBRARY</dt>
		<dd>1162979</dd>
</dl>




```R
# by giving our vector names, we've also pretty much made a lookup table:
minutes_in_2016["SQUIRREL HILL LIBRARY"]
```


<strong>SQUIRREL HILL LIBRARY:</strong> 6898729


### Getting fancy

Probably I'm just being a Python programmer about this, but ... eh. Let's go on this fun little digression, anyway, shall we? I think this is useful.

Remember when we were looking at logical operators, and we couldn't find "Squirrel Hill Library" if we just used "Squirrel"? I hated that, so I wanted to teach you a trick. What if we want to search through our list of libraries and see if there's one with the word "Squirrel" in it?

We can use `stringr::str_detect()` for this. It takes two arguments: the string you're looking in and the substring you're looking for.


```R
# the function of interest comes from stringr
library(stringr)

# in case we aren't sure there are any squirrel libraries
exact_match <- 0

# let's go through our list of libraries
for (library in libraries) {
    # if we find "squirrel" in the name of the library (when both are lowercase)
    if (str_detect(tolower(library), "squirrel")){
        # print it for us to see now
        print(library)
        # and save it
        exact_match <- library
    }
}

# and now we can use exact_match to pull items out of our column if needed
```

    [1] "SQUIRREL HILL LIBRARY"
    


```R
# ok now we're really going down a rabbit hole

getLib <- function(search_name) {
    # let's go through our list of libraries
    # in case we aren't sure there are any squirrel libraries
    exact_match <- 0
    for (library in libraries) {
        # if we find "squirrel" in the name of the library (when both are lowercase)
        if (str_detect(tolower(library), search_name)){
            # save it
            exact_match <- library
        }
    }
    return(exact_match)
}

# we can get user input! (not a thing you do often in R)
lib <- readline(prompt = "Enter library name: ")

cat("The exact name you need to use in the data frame is ")
cat(getLib(lib))
```

    Enter library name: alleg
    The exact name you need to use in the data frame is ALLEGHENY LIBRARY

Realistically, you aren't going to be matching user input to your data frames in R all that often.

But.

The part where you can get out the exact value you need for a query, even with an imperfect memory: well, look, if you're anything like me, it's going to help you. (Imagine, also, that our data entry was in some way imperfect. I know. Farfetched. But it could happen. :))

## while loops

Not used a whole ton in R, but worth seeing at least once.

The syntax:

```R
while (condition) {
    do something
    and CHANGE THE VALUE YOU ARE TESTING ON
}
```


```R
# a classic example: create the fibonacci sequence up to n
# (n is 100, but could be anything)
fib_current <- 1
fib_last <- 0

while (fib_current < 100) {
  cat(fib_current)
  cat(" ")
  fib_current <- fib_current + fib_last
  fib_last <- fib_current - fib_last
}
```

    1 1 2 3 5 8 13 21 34 55 89 


```R
user_value <- "00000"

# whew, look at that input validation loop
while (getLib(user_value) == 0) {
    user_value <- readline(prompt = "Enter library name: ")
}
getLib(user_value)
```

    Enter library name: potato
    Enter library name: topographical
    Enter library name: North
    Enter library name: squir
    


'SQUIRREL HILL LIBRARY'


### OK, now let's do a while loop problem together!

Let us, as a group, poke at the Collatz conjecture, to practice some ifs and whiles. Briefly stated:

Consider the following operation on an arbitrary positive integer:

    * If the number is even, divide it by two.
    * If the number is odd, triple it and add one.

For instance, starting with n = 12, one gets the sequence 12, 6, 3, 10, 5, 16, 8, 4, 2, 1. The sequence is sometimes referred to as the "hailstone sequence" for a number. The number of steps it takes to get to 1 is the "total stopping time" of a number. For 12, the stopping time is 9.

The Collatz conjecture is: This process will eventually reach the number 1, regardless of which positive integer is chosen initially. We aren't setting out to disprove that; it's been tried for every number up to INT_MAX in C, anyway.

But we're going to let a user specify a number for us, and we will tell them the stopping time of the hailstone sequence.


```R

```


```R

```

### And finally, repeat

If you are one of those folks who love to do a `while (TRUE) { ... break;)` you're going to love `repeat`. 'Cause that's all it is.

The syntax:

```R
repeat {
    do some stuff
    if (something) {
        break
    }
}
```

I think we can probably refactor our Collatz code to work in a repeat. Let's do that.


```R

```
