# Retrieving, Processing, and Visualizing Data with Python

## Introduction

This project was developed as part of the online course [Retrieving, Processing, and Visualizing Data with Python](https://www.coursera.org/account/accomplishments/certificate/AFKXKC6FV436) which was part of the [Python for Everybody Specialization](https://www.coursera.org/account/accomplishments/specialization/certificate/5A3DL455XG3F) on [Coursera](https://www.coursera.org/specializations/python?) from Mar - Apr 2020. This specialization was offered by [University of Michigan](https://www.coursera.org/umich)

Course Instructor: [Charles Russell Severance](https://www.coursera.org/instructor/drchuck)

Course Website (MOOC): [py4e](https://www.py4e.com/)

Course Book: [Python for Everybody: Exploring Data Using Python 3](https://www.py4e.com/book.php)

## Repository Structure

Following two projects have been implemented in this repository:

1. [pagerank](./pagerank/) - Simple Python Search Spider, Page Ranker, and Visualizer
2. [my edited gmane](./my%20edited%20gmane/) - Analyzing an EMAIL Archive from [gmane](http://gmane.org/export.php) and vizualizing the data using the D3 JavaScript library.

**_Note:_** Database files (sqlite) have been stored using [Git Large File Storage](https://git-lfs.github.com/)

## Page Rank

This is a set of programs that emulate some of the functions of a search engine.  They store their data in a SQLITE3 database named [spider.sqlite](./pagerank/spider.sqlite).

[SQLite browser](http://sqlitebrowser.org/) was used to view and modify the databases.

This program crawls a web site and pulls a series of pages into the database, recording the links between pages.

Followings python scripts are included:

1. [spider.py](./pagerank/spider.py)
2. [spdump.py](./pagerank/spdump.py)
3. [sprank.py](./pagerank/sprank.py)
4. [spreset.py](./pagerank/spreset.py)
5. [spjson.py](./pagerank/spjson.py)

### spider.py 

This program crawls a web site and pulls a series of pages into the database, recording the links between pages. You can have multiple starting points in the same database - within the program these are called "webs". The spider chooses randomly amongst all non-visited links across all the webs.

![spider.py](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%201%20spider%20py.jpg)

### spdump.py 

Dumps the contents of the spider.sqlite file. It shows the number of incoming links, the old page rank, the new page rank, the id of the page, and the url of the page.  The spdump.py program only shows pages that have at least one incoming link to them.

![spdump.py](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%203%20spdump%20py.jpg)

![spdump.py](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%205%20spdump%20py%20kjsce.jpg)

### sprank.py 

Once we have a few pages in the database, we can run Page Rank on the pages using the sprank.py program.  simply specify how many Page Rank iterations to run.

Dumping the database again verifies that page rank has been updated.

Running [spider.py](./pagerank/spider.py) multiple times refines the page rank.

![sprank.py](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%202%20sprank%20py.jpg)

### spreset.py

Restarts the Page Rank calculations without re-spidering the web pages.

**_Note:_** For each iteration of the page rank algorithm it prints the average change per page of the page rank. The network initially is quite unbalanced and so the individual page ranks are changing wildly. But in a few short iterations, the page rank converges. Run [sprank.py](./pagerank/sprank.py) long enough that the page ranks converge.

### spjson.py

Visualize the current top pages in terms of page rank. Write the pages out in JSON format to be viewed in a web browser.

Open [force.html](./pagerank/force.html) in a browser to view the visualization.

This visualization is provided using the force layout from:

http://mbostock.github.com/d3/

![force.html](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%204%20force%20html.jpg)

![force.html](figures/Course%205%20Capstone%20-%202.6A%20-%20Page%20Rank%206%20force%20html%20kjsce.jpg)

**_Note:_** Refer tho the [README.txt](pagerank/README.txt) file for additional info.

## Gmane

Analyzing an EMAIL Archive from [gmane](http://gmane.org/export.php) and vizualizing the data using the D3 JavaScript library.

[SQLite browser](http://sqlitebrowser.org/) was used to view and modify the databases.

Followings python scripts are included:

1. [gmane.py](my%20edited%20gmane/gmane.py)
2. [gmodel.py](my%20edited%20gmane/gmodel.py)
3. [gbasic.py](my%20edited%20gmane/gbasic.py)
4. [gword.py](my%20edited%20gmane/gword.py)
5. [gline.py](my%20edited%20gmane/gline.py)

The first step is to spider the gmane repository.

### gmane.py

It operates as a spider in that it runs slowly and retrieves one mail message per second so as to avoid getting throttled by gmane.org. It stores all of its data in a database and can be interrupted and re-started as often as needed.

The program scans content.sqlite from 1 up to the first message number not already spidered and starts spidering at that message. It continues spidering until it has spidered the desired number of messages or it reaches a page that does not appear to be a properly formatted message.

The retireved messages are added to [content.sqlite](my%20edited%20gmane/content.sqlite) database. It's data is pretty raw, with an innefficient data model, and not compressed.

![content.sqlite](figures/Course%205%20Capstone%20-%202.12A-1-content%20db.jpg)

### gmodel.py

The second process is running the program [gmodel.py](my%20edited%20gmane/gmodel.py). It reads the rough/raw data from [content.sqlite](my%20edited%20gmane/content.sqlite) and produces a cleaned-up and well-modeled version of the data in the file [index.sqlite](my%20edited%20gmane/index.sqlite).  The file [index.sqlite](my%20edited%20gmane/index.sqlite) will be much smaller (often 10X smaller) than content.sqlite because it also compresses the header and body text.

Each time gmodel.py runs - it completely wipes out and re-builds [index.sqlite](my%20edited%20gmane/index.sqlite).

It does a number of data cleaing steps.

![gmodel.py](figures/Course%205%20Capstone%20-%202.12A-2-gmodel%20py.jpg)
![index.sqlite](figures/Course%205%20Capstone%20-%202.12A-3-index%20db.jpg)

### gbasic.py

[index.sqlite](my%20edited%20gmane/index.sqlite) is the file to use to do data analysis. Simplest data analysis is to do a "who does the most" and "which organzation does the most"? This is done using [gbasic.py](my%20edited%20gmane/gbasic.py).

![gbasic.py](figures/Course%205%20Capstone%20-%202.12A-4%20gbasic%20py.jpg)
![gbasic.py](figures/Course%205%20Capstone%20-%202.15A-1%20gbasic%20py.JPG)

### gword.py

It is a simple vizualization of the word frequency in the subject lines. This produces the file [gword.js](my%20edited%20gmane/gword.js) which you can visualize using the file [gword.htm](my%20edited%20gmane/gword.htm).

![gword.py](figures/Course%205%20Capstone%20-%202.15A-2%20gword%20htm.JPG)

### gline.py

It visualizes email participation by organizations over time. Its output is written to [gline.js](my%20edited%20gmane/gline.js) which is visualized using [gline.htm](my%20edited%20gmane/gline.htm).

![gline.py](figures/Course%205%20Capstone%20-%202.15A-3%20gline%20htm.jpg)
![gline.py](figures/Course%205%20Capstone%20-%202.15A-4%20gline%20htm%20for%20years.jpg)

**_Note:_** Refer tho the [README.txt](my%20edited%20gmane/README.txt) file for additional info.