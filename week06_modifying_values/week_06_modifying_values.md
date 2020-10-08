# Modifying values

## Retrieving and modifying values by position (index)

### in vectors

First, a little review.


```R
# let's put values into a vector
my_vec <- c(1, 2, 3, 4, 5)
```


```R
# retrieve a value by index
my_vec[1]
```


1



```R
# retrieve multiple values by index
my_vec[2:5]
my_vec[-1]
```


<ol class=list-inline>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>




<ol class=list-inline>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>



It is, maybe, unsurprising that we can overwrite values in a vector, using their indices:


```R
my_vec # printing for reference

my_vec[2] <- 3
my_vec
```


<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>




```R
my_vec # printing for reference

my_vec[c(3, 4, 5)] <- c(5, 7, 9)
my_vec
```


<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ol>




<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>5</li>
	<li>7</li>
	<li>9</li>
</ol>




```R
my_vec # printing for reference

my_vec[1:5] <- c(5, 4, 3, 2, 1)
my_vec
```


<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>5</li>
	<li>7</li>
	<li>9</li>
</ol>




<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
</ol>



This is all very reasonable. We're happy with this. It works like you'd expect, probably. 

Thing is, you can also just ... add stuff to a vector, without using any special syntax or anything. (This is _wildly_ uncomfortable for people coming from most other programming languages. I'm still mad.)


```R
#print it for reference
my_vec

#print its length
length(my_vec)

# this works in R, but not most other places
my_vec[6] <- 42
my_vec
```


<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
</ol>




5



<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
</ol>



Real talk? If you can bring yourself to do that, without ruining how you approach any other language, I say go for it! I am a more cautious programmer than that, spending more of my time in other languages that don't allow this type of chicanery, so here's how I accomplish the same thing:


```R
my_vec
my_vec <- c(my_vec, 43)
my_vec
```


<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
</ol>




<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
	<li>43</li>
</ol>




```R
# don't miscount
my_vec

my_vec[13] <- 13
my_vec
```


<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
	<li>43</li>
</ol>




<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
	<li>43</li>
	<li>&lt;NA&gt;</li>
	<li>&lt;NA&gt;</li>
	<li>&lt;NA&gt;</li>
	<li>&lt;NA&gt;</li>
	<li>&lt;NA&gt;</li>
	<li>13</li>
</ol>



#### (and erasing)


```R
# REMOVING values from a vector
my_vec  <- my_vec[-(7:12)]
#same
#my_vec <- my_vec[c(1:6,13)]
my_vec
```


<ol class=list-inline>
	<li>5</li>
	<li>4</li>
	<li>3</li>
	<li>2</li>
	<li>1</li>
	<li>42</li>
	<li>13</li>
</ol>



It's also worth pointing out that R repeats values where needed, even when we're just dealing with a subset of a vector.


```R
# putting two values into 4 spots, with repetition!
my_vec[1:4] <- c(1, 42)
my_vec

# adding 1 to each of 3 different values
my_vec[c(1, 3, 5)] <- my_vec[c(1, 3, 5)] + 41
my_vec
```


<ol class=list-inline>
	<li>1</li>
	<li>42</li>
	<li>1</li>
	<li>42</li>
	<li>1</li>
	<li>42</li>
	<li>13</li>
</ol>




<ol class=list-inline>
	<li>42</li>
	<li>42</li>
	<li>42</li>
	<li>42</li>
	<li>42</li>
	<li>42</li>
	<li>13</li>
</ol>



### Retrieving and modifying values by index/position in a dataframe

This is, admittedly, not much of a thing. You're not usually going to work on columns, at least, by position, because usually columns have names, which is very convenient. Rows are somewhat less often named, so those you deal with positionally, maybe slightly(?) more often. Still, let's look at it, yeah?


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




```R
# get the first row
clp_wifi[1, ]
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
</tbody>
</table>




```R
# get the first column
head(clp_wifi[ , 1])
```


<ol class=list-inline>
	<li>'CLP01'</li>
	<li>'CLP01'</li>
	<li>'CLP01'</li>
	<li>'CLP01'</li>
	<li>'CLP01'</li>
	<li>'CLP01'</li>
</ol>




```R
clp_wifi_copy <- clp_wifi

# we can replace the IDs with the row numbers if we want
clp_wifi_copy[ , 1] <- 1:length(clp_wifi[ ,1])

head(clp_wifi_copy)
tail(clp_wifi_copy)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>1                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>2                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>3                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>129484           </td></tr>
	<tr><td>4                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>5                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>6                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>527</th><td>527              </td><td>WOODS RUN LIBRARY</td><td>2017             </td><td>11               </td><td>813              </td><td>104126           </td></tr>
	<tr><th scope=row>528</th><td>528              </td><td>WOODS RUN LIBRARY</td><td>2017             </td><td>12               </td><td>630              </td><td> 79279           </td></tr>
	<tr><th scope=row>529</th><td>529              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 1               </td><td>716              </td><td> 99671           </td></tr>
	<tr><th scope=row>530</th><td>530              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 2               </td><td>778              </td><td>108100           </td></tr>
	<tr><th scope=row>531</th><td>531              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 3               </td><td>816              </td><td>104073           </td></tr>
	<tr><th scope=row>532</th><td>532              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 4               </td><td>903              </td><td>107865           </td></tr>
</tbody>
</table>




```R
# say we found an error in the wifi minutes for Allegheny Library, in March 2016
# March 2016 is row 3; wifiMinutes is column 6
clp_wifi[3, 6] # shows initial value

clp_wifi[3, 6] <- 424242
head(clp_wifi)
```


129484



<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>424242           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>



### Using column names for retrieval and value modification

Named columns make it a lot easier to do things with our data, honestly.


```R
# get a column by name, two different ways
vector_of_names <- clp_wifi$Name
vector_of_names2 <- clp_wifi[ , "Name"]

head(vector_of_names)
head(vector_of_names2)
```


<ol class=list-inline>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
</ol>




<ol class=list-inline>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'ALLEGHENY LIBRARY'</li>
</ol>




```R
# just making a copy to mess with
clp_wifi_copy <- clp_wifi

# getting a vector the same length as our column is high
column_length_vector <- 1:length(clp_wifi$CLPID)

# and then we can just place it in there
clp_wifi_copy$CLPID <- column_length_vector

# could have done this with a for loop
# for (i in 1:length(clp_wifi$CLPID)) {
#     clp_wifi_copy$CLPID[i] = i
# }

head(clp_wifi_copy)
tail(clp_wifi_copy)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>1                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>2                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>3                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>424242           </td></tr>
	<tr><td>4                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>5                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>6                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>527</th><td>527              </td><td>WOODS RUN LIBRARY</td><td>2017             </td><td>11               </td><td>813              </td><td>104126           </td></tr>
	<tr><th scope=row>528</th><td>528              </td><td>WOODS RUN LIBRARY</td><td>2017             </td><td>12               </td><td>630              </td><td> 79279           </td></tr>
	<tr><th scope=row>529</th><td>529              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 1               </td><td>716              </td><td> 99671           </td></tr>
	<tr><th scope=row>530</th><td>530              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 2               </td><td>778              </td><td>108100           </td></tr>
	<tr><th scope=row>531</th><td>531              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 3               </td><td>816              </td><td>104073           </td></tr>
	<tr><th scope=row>532</th><td>532              </td><td>WOODS RUN LIBRARY</td><td>2018             </td><td> 4               </td><td>903              </td><td>107865           </td></tr>
</tbody>
</table>



Sometimes, you'll mix name and number to get to things you want to change, and that's fine


```R
# modify some rows in a named column
clp_wifi_copy$Name[c(1, 3, 5)] <- "VERY GOOD LIBRARY"
head(clp_wifi_copy)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>1                </td><td>VERY GOOD LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>2                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>3                </td><td>VERY GOOD LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>424242           </td></tr>
	<tr><td>4                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>5                </td><td>VERY GOOD LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>6                </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>



#### (and erasing a column)


```R
# want to erase a column? EASY!
clp_wifi_copy$CLPID <- NULL
head(clp_wifi_copy)
```


<table>
<thead><tr><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>424242           </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>



#### (and adding a column)

(Yes, I'm mad about this, too, but it seems a little less likely to ruin a programmer's day than the thing where you can just keep writing past the end of a vector.)


```R
# let's make a copy of the original clp_wifi CLPID column
our_col <- clp_wifi$CLPID

# now we can just add it to the frame we removed it from
# (I changed its name just to keep this all straight)
clp_wifi_copy$CLPID2 <- our_col
head(clp_wifi_copy)
```


<table>
<thead><tr><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th><th scope=col>CLPID2</th></tr></thead>
<tbody>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td><td>CLP01            </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td><td>CLP01            </td></tr>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>424242           </td><td>CLP01            </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td><td>CLP01            </td></tr>
	<tr><td>VERY GOOD LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td><td>CLP01            </td></tr>
	<tr><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td><td>CLP01            </td></tr>
</tbody>
</table>



## Logic!

Our comparison operators:
* `<`
* `<=`
* `>`
* `>=`
* `==`
* `!=`
* `%in%`

All but the last one do exactly what you'd expect. Mostly. One note: **types get coerced for comparison,** so if you check to see if `1 == "1"` you might get a bit of a surprise. (Gross. I know.)


```R
one <- 1
also_one <- "1"

one == also_one
```


TRUE


Anyway, as we were saying. Not a ton of surprises in the first set of logical comparison operators:


```R
sma <- 1 #create a small number
med <- 3 #create a medium number
big <- 5 #create a big number

sma < med #is sma smaller than med?

sma < big #is sma smaller than big?
```


TRUE



TRUE



```R
# just copying these down so we don't have to remember
sma <- 1 
med <- 3 
big <- 5 

big < med #is big smaller than med? - of course not

big < sma #is big smaller than sma? 
```


FALSE



FALSE



```R
# just copying these down so we don't have to remember
sma <- 1 
med <- 3 
big <- 5 

big >= med #is big greater than or equal to medium?

big >= 5 #is big greater than or equal to 5?
```


TRUE



TRUE



```R
# just copying these down so we don't have to remember
sma <- 1 
med <- 3 
big <- 5 

med == 3 #is med equal to 3?

med != 3 #is medium not equal to 3
```


TRUE



FALSE


_Possibly_ unsurprising (you may have already guessed this) fact about the logical operators above (everything except `%in%`): they work element-wise, so if you do `a_vector == "value"` you'll get a vector of TRUE/FALSE values of the same length as `a_vector`:


```R
my_vec <- 1:6
my_vec
my_vec == 3
my_vec < 5
my_vec >= 4
```


<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>




<ol class=list-inline>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>




<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>




<ol class=list-inline>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>



This is super useful, and we will come back to it in a moment.


```R
# ok and also
my_vec == 1:6

# plus
my_vec == 1:3
my_vec == 4:6
```


<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>




<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>




<ol class=list-inline>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>



Now let's talk about `%in%`.

It works a bit like Python's `in`, if that helps you. Both of these patterns are valid:

```R
one_thing %in% vector_of_many_things   # TRUE if the thing is anywhere in the vector
multiple_things %in% vector_of_many_things  # returns length(multiple_things) vector of TRUEs & FALSEs
```


```R
"CLP01" %in% clp_wifi$CLPID
"SQUIRREL HILL LIBRARY" %in% clp_wifi$Name
"DATA, WOO" %in% clp_wifi$Name

#OK but...
"SQUIRREL" %in% clp_wifi$Name
```


TRUE



TRUE



FALSE



FALSE



```R
c(1, 11, 15) %in% clp_wifi$Month
```


<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
</ol>



### Boolean operators

We can combine logical statements. It's fun!

* `&` - and
* `|` - or
* `xor` - exclusive or - one or the other, not both
* `!` - not 
* `any` - one or more of
* `all` - every single one of

If you've never seen `xor` before, it's the "or" that parents of small children mean when they ask, "would you like a piece of chocolate or a cookie?" 


```R
# unsurprising?

# and
TRUE & TRUE  # true
TRUE & FALSE # false
FALSE & FALSE # false

# or
TRUE | TRUE # true
TRUE | FALSE # true
FALSE | FALSE # false

# not
!TRUE  #false
!FALSE #true
```


TRUE



FALSE



FALSE



TRUE



TRUE



FALSE



FALSE



TRUE



```R
# xor
xor(TRUE, FALSE) # true
xor(FALSE, FALSE) #false

xor(TRUE, TRUE) #FALSE! OMG!
```


TRUE



FALSE



FALSE



```R
# any vs. all
any(FALSE, FALSE, FALSE, TRUE) # true
any(FALSE, FALSE, FALSE, FALSE) #false

all(TRUE, TRUE, TRUE, FALSE) #false
all(TRUE, TRUE, TRUE, TRUE) #true

# "TRUE" and "FALSE" no longer look like real words, sorry, I know
```


TRUE



FALSE



FALSE



TRUE



```R
# let's use some numerical examples, yes?
# just copying these down so we don't have to remember
sma <- 1 
med <- 3 
big <- 5 

!(med == 3)
!!med == 3

big > sma & med > sma
big == 5 & med == 4
big == 5 | med == 4

xor(big == 5, med == 4)
```


FALSE



TRUE



TRUE



FALSE



TRUE



TRUE



```R
# how we actually use any/all:
days_of_the_week <- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")

some_days <- c("Monday", "Wednesday", "Thursday")

some_days %in% days_of_the_week

all(some_days %in% days_of_the_week)
```


<ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>




TRUE



```R
# we also have isTRUE()

isTRUE(med == 3)
isTRUE(TRUE)
isTRUE(FALSE)
isTRUE(3)
```


TRUE



TRUE



FALSE



FALSE


## Logical subsetting

Perhaps you remember, from last week, when I said "we're going to skip over this until it gets useful," about that whole "you can index a vector with logical values" issue?

It's useful now.

You can pull items out of a vector (or, therefore, the column of a dataframe) with a vector of the same length, full of `TRUE`s and `FALSE`s:


```R
tf_vec <- c(TRUE, FALSE, TRUE, FALSE, TRUE, FALSE)
num_vec <- 1:6

num_vec[tf_vec]
```


<ol class=list-inline>
	<li>1</li>
	<li>3</li>
	<li>5</li>
</ol>



You have, in fact, already done this, even if you weren't really thinking of it that way.


```R
# let's get all of the January 2017 data for the library system

# a t/f vector - only true if month == 1 and year == 2017
jan_2017 <- clp_wifi$Year == 2017 & clp_wifi$Month == 1

# all the data
clp_wifi[jan_2017, ]

# or just the minutes of wifi
clp_wifi$WifiMinutes[jan_2017]
```


<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>13</th><td>CLP01                                         </td><td>ALLEGHENY LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td>1070                                          </td><td> 167159                                       </td></tr>
	<tr><th scope=row>41</th><td>CLP02                                         </td><td>BEECHVIEW LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td> 591                                          </td><td>  84279                                       </td></tr>
	<tr><th scope=row>69</th><td>CLP03                                         </td><td>BROOKLINE LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td> 599                                          </td><td> 101389                                       </td></tr>
	<tr><th scope=row>97</th><td>CLP04                                         </td><td>CARRICK LIBRARY                               </td><td>2017                                          </td><td>1                                             </td><td> 381                                          </td><td>  57311                                       </td></tr>
	<tr><th scope=row>125</th><td>CLP05                                                                                     </td><td><span style=white-space:pre-wrap>DOWNTOWN &amp; BUSINESS LIBRARY                   </span></td><td>2017                                                                                      </td><td>1                                                                                         </td><td>3395                                                                                      </td><td> 438874                                                                                   </td></tr>
	<tr><th scope=row>153</th><td>CLP06                                         </td><td>EAST LIBERTY LIBRARY                          </td><td>2017                                          </td><td>1                                             </td><td>2093                                          </td><td> 329518                                       </td></tr>
	<tr><th scope=row>181</th><td>CLP07                                         </td><td>HAZELWOOD LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td> 550                                          </td><td>  74418                                       </td></tr>
	<tr><th scope=row>209</th><td>CLP08                                         </td><td>HILL DISTRICT LIBRARY                         </td><td>2017                                          </td><td>1                                             </td><td> 913                                          </td><td> 132680                                       </td></tr>
	<tr><th scope=row>237</th><td>CLP09                                         </td><td>HOMEWOOD LIBRARY                              </td><td>2017                                          </td><td>1                                             </td><td> 999                                          </td><td> 159949                                       </td></tr>
	<tr><th scope=row>265</th><td>CLP10                                         </td><td>KNOXVILLE LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td> 970                                          </td><td> 144553                                       </td></tr>
	<tr><th scope=row>293</th><td>CLP11                                         </td><td>LAWRENCEVILLE LIBRARY                         </td><td>2017                                          </td><td>1                                             </td><td> 441                                          </td><td>  70790                                       </td></tr>
	<tr><th scope=row>321</th><td>CLP12                                             </td><td>LIBRARY FOR THE BLIND &amp; PHYSICALLY HANDICAPPED</td><td>2017                                              </td><td>1                                                 </td><td> 295                                              </td><td><span style=white-space:pre-wrap>  69804</span>   </td></tr>
	<tr><th scope=row>349</th><td>CLP13                                         </td><td>MAIN (OAKLAND) LIBRARY                        </td><td>2017                                          </td><td>1                                             </td><td>8854                                          </td><td>1397171                                       </td></tr>
	<tr><th scope=row>377</th><td>CLP14                                         </td><td>MOUNT WASHINGTON LIBRARY                      </td><td>2017                                          </td><td>1                                             </td><td> 167                                          </td><td>  30460                                       </td></tr>
	<tr><th scope=row>405</th><td>CLP15                                         </td><td>SHERADEN LIBRARY                              </td><td>2017                                          </td><td>1                                             </td><td>1002                                          </td><td> 146451                                       </td></tr>
	<tr><th scope=row>433</th><td>CLP16                                         </td><td>SOUTH SIDE LIBRARY                            </td><td>2017                                          </td><td>1                                             </td><td> 500                                          </td><td>  90041                                       </td></tr>
	<tr><th scope=row>461</th><td>CLP17                                         </td><td>SQUIRREL HILL LIBRARY                         </td><td>2017                                          </td><td>1                                             </td><td>3749                                          </td><td> 576312                                       </td></tr>
	<tr><th scope=row>489</th><td>CLP18                                         </td><td>WEST END LIBRARY                              </td><td>2017                                          </td><td>1                                             </td><td> 251                                          </td><td>  45518                                       </td></tr>
	<tr><th scope=row>517</th><td>CLP19                                         </td><td>WOODS RUN LIBRARY                             </td><td>2017                                          </td><td>1                                             </td><td> 876                                          </td><td> 120842                                       </td></tr>
</tbody>
</table>




<ol class=list-inline>
	<li>167159</li>
	<li>84279</li>
	<li>101389</li>
	<li>57311</li>
	<li>438874</li>
	<li>329518</li>
	<li>74418</li>
	<li>132680</li>
	<li>159949</li>
	<li>144553</li>
	<li>70790</li>
	<li>69804</li>
	<li>1397171</li>
	<li>30460</li>
	<li>146451</li>
	<li>90041</li>
	<li>576312</li>
	<li>45518</li>
	<li>120842</li>
</ol>




```R
# let's sum up the minutes of wifi used each year at CLP
years <- unique(clp_wifi$Year) # 2016, 2017, 2018

# empty vector for now
wifi_by_year <- vector()

# length(years) is just 3, in this case
for (i in 1:length(years)) {
    # make a vector of trues and falses corresponding to each year
    tf_year_i <- clp_wifi$Year == years[i]
    # pass that true/false vector in to pull out the rows we want from the minutes column
    wifi_i <- sum(clp_wifi$WifiMinutes[tf_year_i])
    wifi_by_year <- c(wifi_by_year, wifi_i)
}
# put the year name with the corresponding sum
names(wifi_by_year) <- years
wifi_by_year
```


<dl class=dl-horizontal>
	<dt>2016</dt>
		<dd>47775041</dd>
	<dt>2017</dt>
		<dd>49493126</dd>
	<dt>2018</dt>
		<dd>17540693</dd>
</dl>



OK, honestly, I'm feeling uninspired at this late hour, and I'd really rather answer the questions you have at this point (even if they're "ok, now show us some of this with the avocado data set") than contrive more examples ahead of time. Let's chat.


```R

```


```R

```
