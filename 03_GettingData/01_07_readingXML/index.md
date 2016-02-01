---
title       : Reading XML 
subtitle    : 
author      : Jeffrey Leek 
job         : Johns Hopkins Bloomberg School of Public Health
logo        : bloomberg_shield.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow   # 
url:
  lib: ../../librariesNew
  assets: ../../assets
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---





## XML

* Extensible markup language
* Frequently used to store structured data
* Particularly widely used in internet applications
* Extracting XML is the basis for most web scraping
* Components
  * Markup - labels that give the text structure
  * Content - the actual text of the document

[http://en.wikipedia.org/wiki/XML](http://en.wikipedia.org/wiki/XML)


---

## Tags, elements and attributes

* Tags correspond to general labels
  * Start tags `<section>`
  * End tags `</section>`
  * Empty tags `<line-break />`
* Elements are specific examples of tags
  * `<Greeting> Hello, world </Greeting>`
* Attributes are components of the label
  * `<img src="jeff.jpg" alt="instructor"/>`
  * `<step number="3"> Connect A to B. </step>`
  

[http://en.wikipedia.org/wiki/XML](http://en.wikipedia.org/wiki/XML)

---

## Example XML file

<img class=center src=../../assets/img/03_ObtainingData/xmlexample.png height=450>


[http://www.w3schools.com/xml/simple.xml](http://www.w3schools.com/xml/simple.xml)

---

## Read the file into R


```r
library(XML)
fileUrl <- "http://www.w3schools.com/xml/simple.xml"
doc <- xmlTreeParse(fileUrl,useInternal=TRUE)
rootNode <- xmlRoot(doc)
xmlName(rootNode)
```

```
[1] "breakfast_menu"
```

```r
names(rootNode)
```

```
  food   food   food   food   food 
"food" "food" "food" "food" "food" 
```


---

## Directly access parts of the XML document


```r
rootNode[[1]]
```

```
<food>
  <name>Belgian Waffles</name>
  <price>$5.95</price>
  <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
  <calories>650</calories>
</food> 
```

```r
rootNode[[1]][[1]]
```

```
<name>Belgian Waffles</name> 
```



---

## Programatically extract parts of the file


```r
xmlSApply(rootNode,xmlValue)
```

```
                                                                                                                    food 
                              "Belgian Waffles$5.95Two of our famous Belgian Waffles with plenty of real maple syrup650" 
                                                                                                                    food 
                   "Strawberry Belgian Waffles$7.95Light Belgian waffles covered with strawberries and whipped cream900" 
                                                                                                                    food 
"Berry-Berry Belgian Waffles$8.95Light Belgian waffles covered with an assortment of fresh berries and whipped cream900" 
                                                                                                                    food 
                                               "French Toast$4.50Thick slices made from our homemade sourdough bread600" 
                                                                                                                    food 
                        "Homestyle Breakfast$6.95Two eggs, bacon or sausage, toast, and our ever-popular hash browns950" 
```


---

## XPath

* _/node_ Top level node
* _//node_ Node at any level
* _node[@attr-name]_ Node with an attribute name
* _node[@attr-name='bob']_ Node with attribute name attr-name='bob'

Information from: [http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf](http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf)


---

## Get the items on the menu and prices


```r
xpathSApply(rootNode,"//name",xmlValue)
```

```
[1] "Belgian Waffles"             "Strawberry Belgian Waffles"  "Berry-Berry Belgian Waffles"
[4] "French Toast"                "Homestyle Breakfast"        
```

```r
xpathSApply(rootNode,"//price",xmlValue)
```

```
[1] "$5.95" "$7.95" "$8.95" "$4.50" "$6.95"
```



---

## Another example


<img class=center src=../../assets/img/03_ObtainingData/ravens.png height=450>

[http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens](http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens)


---

## Viewing the source

<img class=center src=../../assets/img/03_ObtainingData/ravenssource.png height=450>

[http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens](http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens)


---

## Extract content by attributes


```r
fileUrl <- "http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens"
doc <- htmlTreeParse(fileUrl,useInternal=TRUE)
scores <- xpathSApply(doc,"//li[@class='score']",xmlValue)
teams <- xpathSApply(doc,"//li[@class='team-name']",xmlValue)
scores
```

```
list()
```

```r
teams
```

```
[1] "Baltimore RavensRavens" "Baltimore RavensRavens"
```

---

## Notes and further resources

* Official XML tutorials [short](https://web.archive.org/web/20140906181450/http://www.omegahat.org/RSXML/shortIntro.pdf), [long](https://web.archive.org/web/20141113153333/http://www.omegahat.org/RSXML/Tour.pdf)
* [An outstanding guide to the XML package](http://www.stat.berkeley.edu/~statcur/Workshop2/Presentations/XML.pdf)

