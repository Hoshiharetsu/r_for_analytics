# Data Types

There are six types of objects in R:
* double
* integer
* character
* logical
* complex
* raw

You can find out what type of thing you are dealing with using `typeof()`

You can (often, not always) coerce a thing from one type to another.

## Double

Most of the time when you feed R a number it's going to assume you want a double. 

A note on naming: the book sometimes says "numeric" in the place of "double." Either's fine. It's a difference between calling `typeof()` and `class()`. Also, while "numeric" doesn't mean anything outside of the context of R, the term "double" has a very formal and specific meaning that transcends specific programming languages. Let's talk about that:

### Floating point

The term "double" is just short for "double precision floating point."

Do you remember from science courses in your past how we tend to use scientific notation for very small and very large numbers?

* 6.02 * 10^23
* 1.616255 * 10^−35

Yeah, well, we pretty much also do that in binary for numbers that *could be* very large or very small, and that representation is called "floating point."
* 1.1111... * 2^-1023 - smallest double \[precision floating point number\]
* 1.1111... * 2^1023 - biggest double \[precision floating point number\]

Without digging into all the details, the parts you might actually care about go like this:
* there's a bit that says "this is a positive or negative number"
* there are 11 bits that are the exponent - up to 2^11 
* there are 52 bits that specify the "significand" or "mantissa" - up to 2^52
* there are both negative and positive zeros
* there are both negative and positive infinities
* there is more nuance that I have not explained here

And most importantly:
* there are rounding errors

If you're doing financial calculations, you either need to use a library specifically for that, or you need to do all of your math in integers, dealing with dollars and cents separately. 

For most data science applications we'll use floats even though that can sometimes get us in trouble. ¯\\_(ツ)_/¯

We can [play around with this if you want](https://evanw.github.io/float-toy/)


```R
n <- 5
n
typeof(n)
class(n)
```


5



'double'



'numeric'


## Integer

While there are some rounding errors inherent in dealing with floats/doubles/numerics, integers come with none of that. (We can talk about how integers are represented in binary if you want, but we don't have to.) 

Down side: they're just whole numbers, no fun decimal points or anything. 


```R
n <- 5L
n
typeof(n)
class(n)
```


5



'integer'



'integer'



```R
# ok but what if...?
n <- 5.5L
n
typeof(n)
class(n)
```


5.5



'double'



'numeric'



```R
# and how about ...?
n <- as.integer(5.5)
n
typeof(n)
class(n)
```


5



'integer'



'integer'


## Character

Objects of type "character" (which in other languages we might refer to as "strings") contain one or more letters, numbers, or symbols. You can tell you're dealing with a character type because it has quotes around it, e.g.
```R
n <- 5    # double/numeric
m <- "5"  # character
```


```R
n <- "hello!"
n
typeof(n)
```


'hello!'



'character'



```R
n <- 5 # that's a double, yes, intentionally
m <- as.character(n)
m
typeof(m)
```


'5'



'character'


## Logicals

In other languages we might call these "Booleans" or "bools"

The two logical values:
* `TRUE`
* `FALSE`

If you're dealing with a function that expects a logical as an argument, you can shorten to `T` or `F`, and it'll work fine.


```R
n <- TRUE
n
typeof(n)
```


TRUE



'logical'



```R
# OK, but ...
n <- TRUE

# can we make a logical into an int?
m <- as.integer(n)
m
typeof(m)

# can we make a logical into a character?
k <- as.character(n)
k
typeof(k)
```


1



'integer'



'TRUE'



'character'



```R
# can we make other things into logicals?

# numeric to logical?
x <- 1
y <- as.logical(x)
y
typeof(y)

# character to logical?
q <- "TRUE"
r <- as.logical(q)
q
typeof(q)
```


TRUE



'logical'



'TRUE'



'character'


## Complex and raw - we keep glossing over these

But we really don't use them outside of some very specific kinds of engineering.


```R
cpl <- 4 + 5i
cpl
typeof(cpl)
```


4+5i



'complex'



```R
x <- raw(8)
x
typeof(x)
```


    [1] 00 00 00 00 00 00 00 00



'raw'


# Data ... Collection? Types?

Yes, I'm going to call them "Data Collection Types," by which I mean
* Atomic vectors
* Lists
* Matrices
* Arrays (which I'm going to skip)
* Data frames

## Vectors

Vectors are the most basic type. They are single-dimensional and will only accept a single type of object. (No vectors with combinations of characters, numerics, and logicals. It's not allowed. R will just coerce everything into one type. I'll show you in a moment, after this big reveal.)

Secretly, **everything that isn't some more complicated type is actually an atomic vector.** (Whoa.) 


```R
m <- 5
is.vector(m)
length(m)
```


TRUE



1


It's just a single-element vector. That's fine.

And we already know how to build multi-element vectors.


```R
my_vector <- c(1, 2, 3, 4, 5, 6, 7, 8, 9)
is.vector(my_vector)
typeof(my_vector)
length(my_vector)
```


TRUE



'double'



9


So, type coersion. Let's see how that works, yeah?


```R
v0 <- c(1.1, 2L, 3L)
v0

v1 <- c(1, 2, TRUE)
v1

v2 <- c("a", "b", TRUE)
v2

v3 <- c("a", "b", 1, 2)
v3

v4 <- c(1, 2, "a", "b", TRUE)
v4
```


<ol class=list-inline>
	<li>1.1</li>
	<li>2</li>
	<li>3</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>1</li>
</ol>




<ol class=list-inline>
	<li>'a'</li>
	<li>'b'</li>
	<li>'TRUE'</li>
</ol>




<ol class=list-inline>
	<li>'a'</li>
	<li>'b'</li>
	<li>'1'</li>
	<li>'2'</li>
</ol>




<ol class=list-inline>
	<li>'1'</li>
	<li>'2'</li>
	<li>'a'</li>
	<li>'b'</li>
	<li>'TRUE'</li>
</ol>



How R auto-coerces types:
* ints -> doubles 
* logicals -> doubles
* logicals -> characters
* doubles -> characters

But sometimes this auto-coersion is useful. For instance, if we try to count how many answers are "TRUE"


```R
tf <- c(TRUE, FALSE, TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, TRUE)
sum(tf) # they got coerced to 1's and 0's
```


4


Anyway, I said earlier that atomic vectors are "basic," and that's ... true? But they can hold a lot of information, actually. (Metadata! I assume all data analysts love metadata, not just those of us who got here via librarianship?)

You can give each item in the vector a **name**, and you can specify **dimensions** for the vector, making it sort of multi-dimensional. 

### Attributes

There are a couple of ways to name your vector elements:
* 
```
vec <- c(name1 = element1, name2 = element2, name3 = element3, ...)
```
* 
```
vec <- c(element1, element2, element3, ...)
names(vec) <- c(name1, name2, name3, ...)
```
Similarly, you can apply dimensions to your vector like so:
* 
```
dim(vec) <- c(a, b) # a rows, b columns, and it fills DOWN THE COLUMNS
```  
  
And then when you want to know what names your elements have, you can call `attributes(vec)`


```R
die <- c(one=1, two=2, three=3, four=4, five=5, six=6)
# die <- 1:6
# names(die) <- c("one, "two", "three", "four", "five", "six")
die
attributes(die)
names(die)
class(die)
```


<dl class=dl-horizontal>
	<dt>one</dt>
		<dd>1</dd>
	<dt>two</dt>
		<dd>2</dd>
	<dt>three</dt>
		<dd>3</dd>
	<dt>four</dt>
		<dd>4</dd>
	<dt>five</dt>
		<dd>5</dd>
	<dt>six</dt>
		<dd>6</dd>
</dl>




<strong>$names</strong> = <ol class=list-inline>
	<li>'one'</li>
	<li>'two'</li>
	<li>'three'</li>
	<li>'four'</li>
	<li>'five'</li>
	<li>'six'</li>
</ol>




<ol class=list-inline>
	<li>'one'</li>
	<li>'two'</li>
	<li>'three'</li>
	<li>'four'</li>
	<li>'five'</li>
	<li>'six'</li>
</ol>




'numeric'



```R
dim(die) <- c(3, 2)
die # notice the order in which it fills
attributes(die)
names(die) # oops
class(die) # hmm
```


<table>
<tbody>
	<tr><td>1</td><td>4</td></tr>
	<tr><td>2</td><td>5</td></tr>
	<tr><td>3</td><td>6</td></tr>
</tbody>
</table>




<strong>$dim</strong> = <ol class=list-inline>
	<li>3</li>
	<li>2</li>
</ol>




    NULL



'matrix'


### Factors

This is how R tries to make it easy for us to deal with categorical data. Each string gets mapped to an integer.


```R
# here's the book's example, with an update
gender <- factor(c("male", "female", "other", "other", "female"))
gender
typeof(gender)
attributes(gender)
```


<ol class=list-inline>
	<li>male</li>
	<li>female</li>
	<li>other</li>
	<li>other</li>
	<li>female</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'female'</li>
		<li>'male'</li>
		<li>'other'</li>
	</ol>
</details>



'integer'



<dl>
	<dt>$levels</dt>
		<dd><ol class=list-inline>
	<li>'female'</li>
	<li>'male'</li>
	<li>'other'</li>
</ol>
</dd>
	<dt>$class</dt>
		<dd>'factor'</dd>
</dl>




```R
# and we can kind of figure out how maybe the levels were assigned
sort(gender) # looks alphabetical to me

# and we can also look at the integers in place:
unclass(gender)
```


<ol class=list-inline>
	<li>female</li>
	<li>female</li>
	<li>male</li>
	<li>other</li>
	<li>other</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'female'</li>
		<li>'male'</li>
		<li>'other'</li>
	</ol>
</details>



<ol class=list-inline>
	<li>2</li>
	<li>1</li>
	<li>3</li>
	<li>3</li>
	<li>1</li>
</ol>



That, above? That's an OK usage of factors, and you'll sometimes decide to let that happen to some of your data. Often that's fine. But sometimes it's a little more useful to use factors more deliberately:


```R
# pre-decide your levels and put them in the order you want
month_levels <- c(
  "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
  "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
)
# put your observations into a vector
obvs <- c("Oct", "May", "Jan", "Apr")

# and then factor them deliberately!
obvs_factored <- factor(obvs, levels=month_levels)

# what our factored observations look like (um, nothing new)
obvs_factored

# but now they can be sorted more usefully
sort(obvs_factored)
```


<ol class=list-inline>
	<li>Oct</li>
	<li>May</li>
	<li>Jan</li>
	<li>Apr</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'Jan'</li>
		<li>'Feb'</li>
		<li>'Mar'</li>
		<li>'Apr'</li>
		<li>'May'</li>
		<li>'Jun'</li>
		<li>'Jul'</li>
		<li>'Aug'</li>
		<li>'Sep'</li>
		<li>'Oct'</li>
		<li>'Nov'</li>
		<li>'Dec'</li>
	</ol>
</details>



<ol class=list-inline>
	<li>Jan</li>
	<li>Apr</li>
	<li>May</li>
	<li>Oct</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'Jan'</li>
		<li>'Feb'</li>
		<li>'Mar'</li>
		<li>'Apr'</li>
		<li>'May'</li>
		<li>'Jun'</li>
		<li>'Jul'</li>
		<li>'Aug'</li>
		<li>'Sep'</li>
		<li>'Oct'</li>
		<li>'Nov'</li>
		<li>'Dec'</li>
	</ol>
</details>



```R
# now watch this

obvs2 <- c("Oct", "Max", "Jan", "Apr")
obvs2_fact <- factor(obvs2, levels=month_levels)
obvs2_fact
```


<ol class=list-inline>
	<li>Oct</li>
	<li>&lt;NA&gt;</li>
	<li>Jan</li>
	<li>Apr</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'Jan'</li>
		<li>'Feb'</li>
		<li>'Mar'</li>
		<li>'Apr'</li>
		<li>'May'</li>
		<li>'Jun'</li>
		<li>'Jul'</li>
		<li>'Aug'</li>
		<li>'Sep'</li>
		<li>'Oct'</li>
		<li>'Nov'</li>
		<li>'Dec'</li>
	</ol>
</details>


Setting your levels explicitly allows you to throw out errors in data entry. (Was "Max" supposed to be "Mar" or "May"? Dunno, so manybe an NA is better.)

## Matrices

OK, let's say we like the idea of a multi-dimensional(ish) vector. Our default way to handle that was

```R
dim(vec) <- c(a, b)
```

But what if we don't want it filling by column? No problem; we have a thing for that.

```R
m <- matrix(vector, nrow=a, ncol=b, byrow=TRUE)
```


```R
die <- 1:6
mdie <- matrix(die, nrow=3, ncol=2, byrow=TRUE)
mdie
attributes(mdie)
class(mdie)
```


<table>
<tbody>
	<tr><td>1</td><td>2</td></tr>
	<tr><td>3</td><td>4</td></tr>
	<tr><td>5</td><td>6</td></tr>
</tbody>
</table>




<strong>$dim</strong> = <ol class=list-inline>
	<li>3</li>
	<li>2</li>
</ol>




'matrix'


## Lists

Sometimes you want a thing that's like a vector but with lots of types in it. OK. That's why we have lists. They are collections of vectors. Each vector can only have one type in it, but you can have multiple vectors of different types.


```R
my_list <- list(die, "potato", "orange", c(TRUE, FALSE, TRUE, TRUE))
my_list
typeof(my_list)
class(my_list)
```


<ol>
	<li><ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>
</li>
	<li>'potato'</li>
	<li>'orange'</li>
	<li><ol class=list-inline>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>
</li>
</ol>




'list'



'list'


## Data frames

It's all been leading up to this. Here we are. 

Data frames are where R pays for itself. (OK, R is free, but so is Python, and most of us know that better, right now. :)) They're a built-in type. You don't have to import any modules or anything to have them; they're just part of R! 

Data frames are two-dimensional; they're tables. Each column is a vector (which means each column must have the same type all the way down), but your columns don't all have to be the same type. As with most tabular data, we think of rows as individual measurements/observations and columns as individual variables, or attributes of each observation. Anything you can put into a spreadsheet you can put into a data frame. 

You can build a data frame by hand if you want to. That looks like this:
```R
a <- c(val1, val2, val3, ...)
b <- c(x1, x2, x3, ...)
c <- c(m1, m2, m3, ...)
...
df <- data.frame(col_name_1 = a, col_name_2 = b, col_name_3 = c, ...)
```

And then you'll have a data frame where the columns are named "col_name_1", "col_name_2," ... and each column contains the vector associated with it, in a table:

```
     col_name_1     col_name_2    col_name_3   ...
     val1           x1            m1
     val2           x2            m2
     val3           x3            m3
     ...
```


```R
# let's build a data frame with people's pets
# we should decide on our columns (species, name, ...?)
# and people can volunteer individual observations for our data frame :)

pets_df <- data.frame()

```

## Saving our data frame

First set your working directory. (In a Jupyter notebook, it's going to be wherever you saved the notebook.)

In RStudio, you go to Session -> Set Working Directory -> Choose Directory, and decide where you want files to go.


```R
write.csv(pets_df, file="pets.csv", row.names=FALSE) # we don't usually want row names, which are just numbers by default
```

## Loading a data frame

Most of the data we want is somewhere out there, maybe in a .csv or something. We need to be able to pull that into R.

In RStudio, you find the "Import Dataset" button under the "Environment" tab in the Environment Pane. A wizard will pop up.

Or you can use a text-based command (which is all we've got in Jupyter notebooks, anyway).


```R
bike_df = read.csv("avsurvey2019data.csv")
head(bike_df)
```


<table>
<thead><tr><th scope=col>RespondentID</th><th scope=col>StartDate</th><th scope=col>EndDate</th><th scope=col>FamiliarityNews</th><th scope=col>FamiliarityTech</th><th scope=col>SharedCyclist</th><th scope=col>SharedPedestrian</th><th scope=col>SafeAv</th><th scope=col>SafeHuman</th><th scope=col>AvImpact</th><th scope=col>...</th><th scope=col>SchoolZoneManual</th><th scope=col>ShareTripData</th><th scope=col>SharePerformanceData</th><th scope=col>ReportSafetyIncident</th><th scope=col>ArizonaCrash</th><th scope=col>ZipCode</th><th scope=col>BikePghMember</th><th scope=col>AutoOwner</th><th scope=col>SmartphoneOwner</th><th scope=col>Age</th></tr></thead>
<tbody>
	<tr><td>10505419886                        </td><td>2/2/2019                           </td><td>2/2/2019                           </td><td>To a moderate extent               </td><td>Somewhat familiar                  </td><td>Yes                                </td><td>Yes                                </td><td>4                                  </td><td>2                                  </td><td>Significantly Better               </td><td>...                                </td><td>No                                 </td><td>Not sure                           </td><td>Yes                                </td><td>Yes                                </td><td>No change                          </td><td>15212                              </td><td>No                                 </td><td>Yes                                </td><td>Yes                                </td><td>25-34                              </td></tr>
	<tr><td>10505138734                        </td><td>2/2/2019                           </td><td>2/2/2019                           </td><td>To a moderate extent               </td><td>Somewhat familiar                  </td><td>Yes                                </td><td>No                                 </td><td>5                                  </td><td>4                                  </td><td>Significantly Better               </td><td>...                                </td><td>No                                 </td><td>No                                 </td><td>Yes                                </td><td>Not sure                           </td><td>No change                          </td><td>15232                              </td><td>Not sure                           </td><td>Yes                                </td><td>Yes                                </td><td>25-34                              </td></tr>
	<tr><td>10504803283                        </td><td>2/1/2019                           </td><td>2/1/2019                           </td><td>To a moderate extent               </td><td>Somewhat familiar                  </td><td>Yes                                </td><td>Yes                                </td><td>1                                  </td><td>4                                  </td><td>Significantly Worse                </td><td>...                                </td><td>No                                 </td><td>No                                 </td><td>No                                 </td><td>Not sure                           </td><td>Significantly more negative opinion</td><td>null                               </td><td>No                                 </td><td>No                                 </td><td>No                                 </td><td>null                               </td></tr>
	<tr><td>10504337177                        </td><td>2/1/2019                           </td><td>2/1/2019                           </td><td>To a moderate extent               </td><td>Extremely familiar                 </td><td>Yes                                </td><td>Yes                                </td><td>2                                  </td><td>3                                  </td><td>Slightly Worse                     </td><td>...                                </td><td>Yes                                </td><td>No                                 </td><td>Yes                                </td><td>Yes                                </td><td>No change                          </td><td>15136                              </td><td>No                                 </td><td>No                                 </td><td>Yes                                </td><td>55-64                              </td></tr>
	<tr><td>10504261546                        </td><td>2/1/2019                           </td><td>2/1/2019                           </td><td>To a moderate extent               </td><td>Mostly familiar                    </td><td>Yes                                </td><td>No                                 </td><td>5                                  </td><td>3                                  </td><td>Slightly Better                    </td><td>...                                </td><td>Not sure                           </td><td>Yes                                </td><td>Yes                                </td><td>No                                 </td><td>No change                          </td><td>15201                              </td><td>Yes                                </td><td>No                                 </td><td>Yes                                </td><td>35-44                              </td></tr>
	<tr><td>10504232487                        </td><td>2/1/2019                           </td><td>2/1/2019                           </td><td>To a large extent                  </td><td>Mostly familiar                    </td><td>Yes                                </td><td>Yes                                </td><td>1                                  </td><td>3                                  </td><td>Slightly Worse                     </td><td>...                                </td><td>Yes                                </td><td>Yes                                </td><td>Yes                                </td><td>Yes                                </td><td>Somewhat more negative opinion     </td><td>15222                              </td><td>Yes                                </td><td>Yes                                </td><td>Yes                                </td><td>35-44                              </td></tr>
</tbody>
</table>




```R
# pulling out one column to mess with
shared <- bike_df["SharedCyclist"]
# and just printing a little of it
head(shared)
```


<table>
<thead><tr><th scope=col>SharedCyclist</th></tr></thead>
<tbody>
	<tr><td>Yes</td></tr>
	<tr><td>Yes</td></tr>
	<tr><td>Yes</td></tr>
	<tr><td>Yes</td></tr>
	<tr><td>Yes</td></tr>
	<tr><td>Yes</td></tr>
</tbody>
</table>




```R
# counts every value in the SharedCyclist column that has a value
has_val <- sum(!is.na(shared))
has_val

# counts yesses
yes <- sum(shared == "Yes")
yes

# percentage that were yesses
perc <- yes / has_val * 100
perc
```


795



422



53.0817610062893

