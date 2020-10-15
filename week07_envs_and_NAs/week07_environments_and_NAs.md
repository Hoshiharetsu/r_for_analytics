# Environments and scoping in R

Scoping in R isn't really that weird, if we're coming into it from a programming background, but it really stresses out mathematicians, I guess. 

Generally, you'll be working in the Global Environment, unless you're working inside a function.

(Attribution note: a _lot_ of the code for this particular notebook came from Jordan Benis's lecture notes, which he kindly shared with me.)


```R
environment()
```


    <environment: R_GlobalEnv>



```R
# the global environment does have a parent environment
parent.env(environment())
```


    <environment: 0x00000000427d9fd0>
    attr(,"name")
    [1] "jupyter:irkernel"



```R
# ... which has a parent environment
parent.env(parent.env(environment()))
# we could keep going up, but you get the idea
```


    <environment: package:stats>
    attr(,"name")
    [1] "package:stats"
    attr(,"path")
    [1] "C:/Users/csheldon-hess/Anaconda3/envs/r-with-jupyter/Lib/R/library/stats"



```R
print(ls())
print(ls(globalenv()))
#ls(baseenv())
```

    character(0)
    character(0)
    


```R
checkEnv <- function(){
  print(environment())
  print(parent.env(environment()))
  print(ls())
}

checkEnv()
ls()
```

    <environment: 0x000000004c310d40>
    <environment: R_GlobalEnv>
    character(0)
    


'checkEnv'



```R
checkEnvWarg <- function(arg = 3){
  print(environment())
  print(parent.env(environment()))
  print(ls()) # oooh
}

checkEnvWarg()

```

    <environment: 0x000000004ca02ba8>
    <environment: R_GlobalEnv>
    [1] "arg"
    


```R
ls()
```


<ol class=list-inline>
	<li>'checkEnv'</li>
	<li>'checkEnvWarg'</li>
</ol>



To some extent, scoping in R works just like we expect from other languages. Create a variable inside a function? It goes away when the function returns. It's local to that function. Makes sense. Seems good. We're happy.


```R
a <- 7

changea0 <- function(){
    b <- 20
    a <- a + b
}

changea0()
a
b
```


7



    Error in eval(expr, envir, enclos): object 'b' not found
    Traceback:
    


Within a function, you can access global variables. Also unsurprising. But R does a nice thing, here, where it doesn't just let you go changing global variables without _really_ intending to.

It creates a new copy of the variable, within the environment of the function. It's separate from the global variable. Changes to it don't affect the global variable.


```R
a <- 7 #in global env

# use a in a function
changea1 <- function(){
  a <- a + 1 # now we are in function's environment
  print(a)
}
changea1()
a
```

    [1] 8
    


7



```R
a <- 7 #in global env

# ok, what if we make a a parameter?
# (don't do this, why would you do this?)
changea2 <- function(a = 9){
  a <- a + 1 # can still get that good global value!
  print(a)   # can change it in function scope
}
changea2(a)
a # but it remains the same, globally
```

    [1] 8
    


7


### Updating global variables inside functions

Occasionally, you might need to update a global variable inside a function. It's a thing, I _guess._ (I claim it's not a thing you will want to do often, although the book's example with "deal a card from the deck and then pull that card out of the deck" is a compelling exception.) 


```R
# updating a value
# how do we think this works?

globalVar <- 17

updateGV <- function(numForUpdate){
  globalVar <- globalVar + numForUpdate
  globalVar
}

updateGV(3)
globalVar
```


20



17


Yeah, no.

For this we need the, I kid you not, **superassignment operator** `<<-`


```R
globalVar <- 17

updateGV <- function(numForUpdate){
  globalVar <<- globalVar + numForUpdate
  globalVar
}
updateGV(3)

globalVar
```


20



20


Alternately, you can use the `assign()` function.


```R
globalVar <- 17

updateGV <- function(numForUpdate){
  assign("globalVar", globalVar + numForUpdate, envir = parent.env(environment()))
  globalVar
}

updateGV(3) # we can try this with other numbers if you want

globalVar
```


20



20


Questions? Things you want to try?

# NAs in R

As data analysts, we often have to deal with missing values in our data. R was built with this kind of thing in mind and represents missing values with the symbol `NA`. You can't do math or logic with NAs.


```R
5 + NA #it doesn't return 5

NA == "Dog" #it does return FALSE
```


&lt;NA&gt;



&lt;NA&gt;



```R
# OK, and?

NA == NA
```


&lt;NA&gt;



```R
vecWNA <- c(NA,1:10)

vecWNA

mean(vecWNA) #will return NA!
```


<ol class=list-inline>
	<li>&lt;NA&gt;</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




&lt;NA&gt;


## na.rm = TRUE

All is not lost! We can tell R, "yes, this data has some gaps, but we want to know what you can tell us using only the valid parts of the data." 


```R
# throw out NAs
mean(vecWNA,na.rm = TRUE)
```


5.5


Sometimes it's better to zero out our NAs. (Or to put some other value in, such as the mean.)

## is.na()


```R
# can do with a for loop!
vecWNA <- c(NA,1:10)
vecWNA

for(i in 1:length(vecWNA)){
  if(is.na(vecWNA[i])){
    vecWNA[i] <-0
  }
}

# why didn't we do this?
# for(i in vecWNA){
#   if(is.na(i)){
#     i <- 0
#   }
# }

vecWNA
```


<ol class=list-inline>
	<li>&lt;NA&gt;</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




<ol class=list-inline>
	<li>0</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




```R
# could also do with logical subsetting!

vecWNA <- c(NA,1:10)

vecWNA

vecWNA[is.na(vecWNA)] <- 0

vecWNA
```


<ol class=list-inline>
	<li>&lt;NA&gt;</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




<ol class=list-inline>
	<li>0</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




```R
# how do we feel about this?

vecWNA <- c(NA,1:10)

vecWNA

vecWNA[vecWNA == NA] <- 0

vecWNA
```


<ol class=list-inline>
	<li>&lt;NA&gt;</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




<ol class=list-inline>
	<li>&lt;NA&gt;</li>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
	<li>9</li>
	<li>10</li>
</ol>




```R

```
