# R Notation

Pythonistas: I'm really sorry about this. No, it isn't my fault. I'm still sorry.

The most common way to pull things out of dataframes is with square brackets. For dataframe `df` we can pull out the value from a specific cell at **row a, column b** with `df[a, b]` - pulling one single value from a cell isn't something we're going to do very often, but everything we're about to talk about makes much more sense once you understand that the **spot before the comma is for rows, and the spot after the comma is for columns.**

We have several tools available for pulling items (or vectors!) out of dataframes:
* Positive integers (the row/column corresponding to the integer), using \[\] notation
* Negative integers (everything _except_ the row/column corresponding to the integer), using \[\] notation
* Blank spaces (everything, all of it, in that dimension), using \[\] notation
* Logical values (we're going to skip over this until it gets useful)
* Names, via both \$ notation and \[\] notation.

First, let's remind ourselves how to pull in a dataframe. 


```R
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



## Getting columns from a data frame

### Numeric indexing to include certain columns

If you know the position of the column(s) you want, you can get a column using numeric indexing with `df[ , numVec]`:


```R
# column 4, with a reminder that we start counting at 1 not 0
col4 <- clp_wifi[ , 4] # the space means "all rows"
head(col4)
```


<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>




```R
# columns 5 and 6
col5_6 <- clp_wifi[ , 5:6]
# same:
# col5_6 <- clp_wifi[ , c(5, 6)]
head(col5_6)
```


<table>
<thead><tr><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>1037  </td><td>148513</td></tr>
	<tr><td>1064  </td><td>150948</td></tr>
	<tr><td> 949  </td><td>129484</td></tr>
	<tr><td> 934  </td><td>136196</td></tr>
	<tr><td>1018  </td><td>135793</td></tr>
	<tr><td>1163  </td><td>169426</td></tr>
</tbody>
</table>



### Numeric indexing to _exclude_ certain columns

If you know the position of the column(s) you **don't want** you can also get that with numeric indexing, using `df[ , -numVec]`


```R
# what if we don't want the name of the library?
not_col_2 <- clp_wifi[ , -2]
head(not_col_2)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01 </td><td>2016  </td><td>1     </td><td>1037  </td><td>148513</td></tr>
	<tr><td>CLP01 </td><td>2016  </td><td>2     </td><td>1064  </td><td>150948</td></tr>
	<tr><td>CLP01 </td><td>2016  </td><td>3     </td><td> 949  </td><td>129484</td></tr>
	<tr><td>CLP01 </td><td>2016  </td><td>4     </td><td> 934  </td><td>136196</td></tr>
	<tr><td>CLP01 </td><td>2016  </td><td>5     </td><td>1018  </td><td>135793</td></tr>
	<tr><td>CLP01 </td><td>2016  </td><td>6     </td><td>1163  </td><td>169426</td></tr>
</tbody>
</table>



### Addressing columns by name

OK, but these are data frames. Often&mdash;dare I say, _usually_&mdash;they will have named columns. We can make use of that. 

First, here's how you do this with brackets: `df[ , "colName"]` - yeah, you want those quotes, and yeah, you want that comma (what I did last week shouldn't have worked, properly speaking, oops).


```R
# give us the number of wifi sessions!

sessions <- clp_wifi[ , "WifiSessions"]
head(sessions)
class(sessions)
```


<ol class=list-inline>
	<li>1037</li>
	<li>1064</li>
	<li>949</li>
	<li>934</li>
	<li>1018</li>
	<li>1163</li>
</ol>




'integer'



```R
# give us both of the last two columns

# note the vector notation!
last_two_columns <- clp_wifi[ , c("WifiSessions", "WifiMinutes")]
head(last_two_columns)

# OK, but also, note:
class(last_two_columns)
```


<table>
<thead><tr><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>1037  </td><td>148513</td></tr>
	<tr><td>1064  </td><td>150948</td></tr>
	<tr><td> 949  </td><td>129484</td></tr>
	<tr><td> 934  </td><td>136196</td></tr>
	<tr><td>1018  </td><td>135793</td></tr>
	<tr><td>1163  </td><td>169426</td></tr>
</tbody>
</table>




'data.frame'


OK, but. Let's say that you only want one column, but you WANT it to be a data frame. (It could happen!) Easy enough:

`df[ , "colName", drop = FALSE]`


```R
sessions_df <- clp_wifi[ , "WifiSessions", drop=FALSE]
head(sessions_df)
class(sessions_df)
```


<table>
<thead><tr><th scope=col>WifiSessions</th></tr></thead>
<tbody>
	<tr><td>1037</td></tr>
	<tr><td>1064</td></tr>
	<tr><td> 949</td></tr>
	<tr><td> 934</td></tr>
	<tr><td>1018</td></tr>
	<tr><td>1163</td></tr>
</tbody>
</table>




'data.frame'


OK, so, that's how we get a column with brackets. Great! As you already know (but I'm going to tell you again anyway), there's another option available for us to pull one column out of a dataframe: `df$colName` - note that there are no quotation marks there


```R
# OK, now let's pull out just the WifiMinutes:

mins <- clp_wifi$WifiMinutes
head(mins)
class(mins)  
```


<ol class=list-inline>
	<li>148513</li>
	<li>150948</li>
	<li>129484</li>
	<li>136196</li>
	<li>135793</li>
	<li>169426</li>
</ol>




'integer'


### Doing things with columns once we've retrieved them

What's the average number of minutes per wifi session at a library in Pittsburgh?

Which libraries are in this data set?

What years does it cover?

All questions we can answer with just a couple of lines of code!


```R
# What's the average number of minutes per wifi session at a library in Pittsburgh?

# we pulled out the vector of minutes above! 
# mins <- clp_wifi$WifiMinutes

# also, the vector of wifi sessions:
# sessions <- clp_wifi[ , "WifiSessions"]

# bit naive, assumes we have data in every row,
# but it's a decent approximation probably
avg <- sum(mins)/sum(sessions)
avg
```


132.611608166479



```R
# Which libraries are in this data set?
# (Bonus) How many are there?

lib_names <- unique(clp_wifi$Name)
lib_names
length(lib_names)
```


<ol class=list-inline>
	<li>'ALLEGHENY LIBRARY'</li>
	<li>'BEECHVIEW LIBRARY'</li>
	<li>'BROOKLINE LIBRARY'</li>
	<li>'CARRICK LIBRARY'</li>
	<li>'DOWNTOWN &amp; BUSINESS LIBRARY'</li>
	<li>'EAST LIBERTY LIBRARY'</li>
	<li>'HAZELWOOD LIBRARY'</li>
	<li>'HILL DISTRICT LIBRARY'</li>
	<li>'HOMEWOOD LIBRARY'</li>
	<li>'KNOXVILLE LIBRARY'</li>
	<li>'LAWRENCEVILLE LIBRARY'</li>
	<li>'LIBRARY FOR THE BLIND &amp; PHYSICALLY HANDICAPPED'</li>
	<li>'MAIN (OAKLAND) LIBRARY'</li>
	<li>'MOUNT WASHINGTON LIBRARY'</li>
	<li>'SHERADEN LIBRARY'</li>
	<li>'SOUTH SIDE LIBRARY'</li>
	<li>'SQUIRREL HILL LIBRARY'</li>
	<li>'WEST END LIBRARY'</li>
	<li>'WOODS RUN LIBRARY'</li>
</ol>




19



```R
# Which years does this data set cover?

#same trick, actually
unique(clp_wifi$Year)
```


<ol class=list-inline>
	<li>2016</li>
	<li>2017</li>
	<li>2018</li>
</ol>



## Getting rows from data frames

A row corresponds (usually, nearly always) to a specific observation. Sometimes you're going to want to be able to pull out one row (or a set of rows) to examine what's happening in them. 

### By position with numeric indexing

Unsurprisingly, it's just like above, only you put your number in the first spot instead of the second: `df[numVec, ]` - again, the space in the column spot tells R "I want all the columns in this row"


```R
row1 <- clp_wifi[1, ]
row1
class(row1)
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
</tbody>
</table>




'data.frame'


It makes a certain amount of sense that we don't have a vector, since data types will vary across columns in a data frame, and a single vector can't contain multiple data types. 

It means nothing changes when we grab multiple rows:


```R
first_10 <- clp_wifi[1:10, ]
first_10
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 1               </td><td>1037             </td><td>148513           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 2               </td><td>1064             </td><td>150948           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 3               </td><td> 949             </td><td>129484           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 4               </td><td> 934             </td><td>136196           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 5               </td><td>1018             </td><td>135793           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 6               </td><td>1163             </td><td>169426           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 7               </td><td>1352             </td><td>208802           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 8               </td><td>1436             </td><td>219842           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td> 9               </td><td>1222             </td><td>180409           </td></tr>
	<tr><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>10               </td><td>1086             </td><td>156120           </td></tr>
</tbody>
</table>



### Leaving out rows with negative indices

Again, no surprises here; it works just like with columns, except we always get a data frame.


```R
all_but_first_10 <- clp_wifi[-(1:10), ]
head(all_but_first_10)
```


<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>11</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>11               </td><td>1038             </td><td>141003           </td></tr>
	<tr><th scope=row>12</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>12               </td><td> 925             </td><td>140230           </td></tr>
	<tr><th scope=row>13</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2017             </td><td> 1               </td><td>1070             </td><td>167159           </td></tr>
	<tr><th scope=row>14</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2017             </td><td> 2               </td><td>1029             </td><td>148976           </td></tr>
	<tr><th scope=row>15</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2017             </td><td> 3               </td><td>1400             </td><td>118952           </td></tr>
	<tr><th scope=row>16</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2017             </td><td> 4               </td><td>1725             </td><td>155280           </td></tr>
</tbody>
</table>



### Getting rows by name 

This isn't something you'll necessarily do every day, but some datasets do have named rows. I'd feel remiss if I left it out.

The format is unsurprising: `df["rowName", ]` or `df[c("rowName1", "rowName2", ...), ]`

Let's give our dataframe some row names. 

#### Slight detour: for loops in R

What if I also show you how to write a for loop in R, while we're at it?

General format of a for loop in R:
```R
for (iterVar in numVec) {
  # do something
}
```


```R
# I want my names to take the form Year_Month_CLPID

# we start with an empty vector
ids <- vector()

# we're going to loop through each row of the frame
for (i in 1:length(clp_wifi$Month)) {
    # the row name will be a string, so we need our numerics as characters
    year <- as.character(clp_wifi$Year[i])
    month <- as.character(clp_wifi$Month[i])
    # I want all my names to be the same length, so I preappend zeros as needed
    if (clp_wifi$Month[i] < 10) {
        month <- paste("0", month, sep = "")
    }
    # combining the strings with underscores between them
    id <- paste(year, month, clp_wifi$CLPID[i], sep = "_")
    
    # and then we append each new id onto the vector of IDs
    ids <- c(ids, id)
}

# having my vector of row names, I now need to add it to my dataframe
row.names(clp_wifi) <- ids
head(clp_wifi)
```


<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2016_01_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><th scope=row>2016_02_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><th scope=row>2016_03_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>3                </td><td> 949             </td><td>129484           </td></tr>
	<tr><th scope=row>2016_04_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><th scope=row>2016_05_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><th scope=row>2016_06_CLP01</th><td>CLP01            </td><td>ALLEGHENY LIBRARY</td><td>2016             </td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>




```R
# now that we have given our rows names :), let's pull out some rows by name

# January 2018 in Squirrel Hill 
clp_wifi["2018_01_CLP17", ]

# the first quarter of 2018 in Squirrel Hill
clp_wifi[c("2018_01_CLP17", "2018_02_CLP17", "2018_03_CLP17"), ]
```


<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2018_01_CLP17</th><td>CLP17                </td><td>SQUIRREL HILL LIBRARY</td><td>2018                 </td><td>1                    </td><td>4574                 </td><td>628896               </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2018_01_CLP17</th><td>CLP17                </td><td>SQUIRREL HILL LIBRARY</td><td>2018                 </td><td>1                    </td><td>4574                 </td><td>628896               </td></tr>
	<tr><th scope=row>2018_02_CLP17</th><td>CLP17                </td><td>SQUIRREL HILL LIBRARY</td><td>2018                 </td><td>2                    </td><td>5022                 </td><td>696437               </td></tr>
	<tr><th scope=row>2018_03_CLP17</th><td>CLP17                </td><td>SQUIRREL HILL LIBRARY</td><td>2018                 </td><td>3                    </td><td>5427                 </td><td>752002               </td></tr>
</tbody>
</table>



Now, whether we're discussing rows or columns, there are some rules. We can't go mixing positive and negative indices. It isn't allowed.


```R
clp_wifi[c(-1, 2, 3), ]
```


    Error in xj[i]: only 0's may be mixed with negative subscripts
    Traceback:
    

    1. clp_wifi[c(-1, 2, 3), ]

    2. `[.data.frame`(clp_wifi, c(-1, 2, 3), )


Speaking of zeroes, the book included them as if they're some kind of valid thing, but then freely admitted that they're useless. Let's go ahead and try out some zeroes, yeah?


```R
clp_wifi[0, ]
```


<table>
<thead><tr><th scope=col>CLPID</th><th scope=col>Name</th><th scope=col>Year</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
</tbody>
</table>




```R
clp_wifi[, 0]
```

    Warning message in rbind(parts$upper, ellip_v, parts$lower, deparse.level = 0L):
    "number of columns of result is not a multiple of vector length (arg 2)"






```R
print(clp_wifi[0, 0])
# don't forget print() exists; sometimes we get more information that way
```

    data frame with 0 columns and 0 rows
    

## Pulling together _some_ rows and _some_ columns

Sometimes we don't want the entire huge dataframe we've been given. We can subset it down to the data we find useful, combining our methods for choosing rows with our methods for choosing columns.


```R
# what if we just want the first 25 rows, and only WifiSessions and WifiMinutes?

clp_wifi[1:25, c("WifiSessions", "WifiMinutes")]
```


<table>
<thead><tr><th></th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2016_01_CLP01</th><td>1037  </td><td>148513</td></tr>
	<tr><th scope=row>2016_02_CLP01</th><td>1064  </td><td>150948</td></tr>
	<tr><th scope=row>2016_03_CLP01</th><td> 949  </td><td>129484</td></tr>
	<tr><th scope=row>2016_04_CLP01</th><td> 934  </td><td>136196</td></tr>
	<tr><th scope=row>2016_05_CLP01</th><td>1018  </td><td>135793</td></tr>
	<tr><th scope=row>2016_06_CLP01</th><td>1163  </td><td>169426</td></tr>
	<tr><th scope=row>2016_07_CLP01</th><td>1352  </td><td>208802</td></tr>
	<tr><th scope=row>2016_08_CLP01</th><td>1436  </td><td>219842</td></tr>
	<tr><th scope=row>2016_09_CLP01</th><td>1222  </td><td>180409</td></tr>
	<tr><th scope=row>2016_10_CLP01</th><td>1086  </td><td>156120</td></tr>
	<tr><th scope=row>2016_11_CLP01</th><td>1038  </td><td>141003</td></tr>
	<tr><th scope=row>2016_12_CLP01</th><td> 925  </td><td>140230</td></tr>
	<tr><th scope=row>2017_01_CLP01</th><td>1070  </td><td>167159</td></tr>
	<tr><th scope=row>2017_02_CLP01</th><td>1029  </td><td>148976</td></tr>
	<tr><th scope=row>2017_03_CLP01</th><td>1400  </td><td>118952</td></tr>
	<tr><th scope=row>2017_04_CLP01</th><td>1725  </td><td>155280</td></tr>
	<tr><th scope=row>2017_05_CLP01</th><td>1758  </td><td>154950</td></tr>
	<tr><th scope=row>2017_06_CLP01</th><td>1943  </td><td>161154</td></tr>
	<tr><th scope=row>2017_07_CLP01</th><td>1804  </td><td>143425</td></tr>
	<tr><th scope=row>2017_08_CLP01</th><td>2274  </td><td>195316</td></tr>
	<tr><th scope=row>2017_09_CLP01</th><td>2064  </td><td>170576</td></tr>
	<tr><th scope=row>2017_10_CLP01</th><td>1896  </td><td>159538</td></tr>
	<tr><th scope=row>2017_11_CLP01</th><td>1587  </td><td>139323</td></tr>
	<tr><th scope=row>2017_12_CLP01</th><td>1364  </td><td>123693</td></tr>
	<tr><th scope=row>2018_01_CLP01</th><td>1771  </td><td>156736</td></tr>
</tbody>
</table>



(And here, Coral gets a bit ahead of themself, so that you have something interesting to do for homework.)

We can pull data from frames with conditionals. We'll do a lot of this next week, but let's do just a little of it this week, as a treat.

Say we only want library name, month, sessions, and minutes from 2016, for instance. We can do that.


```R
#minutes_in_2016 <- clp_wifi$WifiMinutes[clp_wifi$Year == 2016]
data_from_2016 <- clp_wifi[clp_wifi$Year == 2016, c("Name", "Month", "WifiSessions", "WifiMinutes")]

# confirming
head(data_from_2016)
tail(data_from_2016)
```


<table>
<thead><tr><th></th><th scope=col>Name</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2016_01_CLP01</th><td>ALLEGHENY LIBRARY</td><td>1                </td><td>1037             </td><td>148513           </td></tr>
	<tr><th scope=row>2016_02_CLP01</th><td>ALLEGHENY LIBRARY</td><td>2                </td><td>1064             </td><td>150948           </td></tr>
	<tr><th scope=row>2016_03_CLP01</th><td>ALLEGHENY LIBRARY</td><td>3                </td><td> 949             </td><td>129484           </td></tr>
	<tr><th scope=row>2016_04_CLP01</th><td>ALLEGHENY LIBRARY</td><td>4                </td><td> 934             </td><td>136196           </td></tr>
	<tr><th scope=row>2016_05_CLP01</th><td>ALLEGHENY LIBRARY</td><td>5                </td><td>1018             </td><td>135793           </td></tr>
	<tr><th scope=row>2016_06_CLP01</th><td>ALLEGHENY LIBRARY</td><td>6                </td><td>1163             </td><td>169426           </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>Name</th><th scope=col>Month</th><th scope=col>WifiSessions</th><th scope=col>WifiMinutes</th></tr></thead>
<tbody>
	<tr><th scope=row>2016_07_CLP19</th><td>WOODS RUN LIBRARY</td><td> 7               </td><td>910              </td><td>121013           </td></tr>
	<tr><th scope=row>2016_08_CLP19</th><td>WOODS RUN LIBRARY</td><td> 8               </td><td>936              </td><td>115524           </td></tr>
	<tr><th scope=row>2016_09_CLP19</th><td>WOODS RUN LIBRARY</td><td> 9               </td><td>787              </td><td>113217           </td></tr>
	<tr><th scope=row>2016_10_CLP19</th><td>WOODS RUN LIBRARY</td><td>10               </td><td>690              </td><td>101788           </td></tr>
	<tr><th scope=row>2016_11_CLP19</th><td>WOODS RUN LIBRARY</td><td>11               </td><td>766              </td><td>119559           </td></tr>
	<tr><th scope=row>2016_12_CLP19</th><td>WOODS RUN LIBRARY</td><td>12               </td><td>538              </td><td> 78865           </td></tr>
</tbody>
</table>




```R
library_name <- data_from_2016$Name
library(ggplot2)
qplot(data_from_2016$Month, data_from_2016$WifiMinutes, color=library_name, xlab="Month", ylab="Minutes of WiFi Usage")
```


![png](output_37_0.png)



```R

```
