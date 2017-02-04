# harvesting-tools
A collection of code snippets mostly designed to be dropped into the data harvesting process directly after generating the zip starter kit.

## Usage

- Familiarize yourself with the [harvesting instructions](https://github.com/datarefugephilly/workflow/tree/master/harvesting-toolkit) in the Data Rescue workflow repo.  Within the [pipeline app](http://harvest-pipeline.herokuapp.com/), click `Download Zip Starter` from the page related to a URL that you have checked out. 
- unzip the zipfile
- Choose a tool that seems likely to be helpful in capturing this particular resource, and copy the contents of its directory in this repo to the `tools` directory, e.g. with:
  ```
  cp -r harvesting-tools/TOOLNAME/* RESOURCEUUID/tools/
  ```
- Adjust the base URL for the resource along with any other relevant variables, and tweak the content of the tool as necessary
- After the resource has been harvested, proceed with the further steps in the [workflow](https://github.com/datarefugephilly/workflow/). 

## Matching Tools and Datasets

Each tool in this repo has a fairly specific use case. Your choice will depend on the shape and size of the data you're dealing with. Some datasetswill require more creativity/more elaborate tools. If you write a new tool, please [add it to the repo](#contributing). 

### [wget-loop](./wget-loop) for largely static resources
If you encounter a page that links to lots of data (for example a "downloads" page), this approach may well work. It's important to only use this approach when you encounter *data*, for example pdf's, .zip archives, .csv datasets, etc. 

The tricky part of this approach is generating a list of urls to download from the page. 
- If you're skilled with using scripts in combination with html-parsers (for example python's wonderful [beautiful-soup package](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#quick-start)), go for it. 
- if the URL's you're trying to access are dynamically generated by JavaScript in the browser environment, we've also included the [jquery-url-extraction guide](tools/jquery-url-extraction)], which will guide you through the process of exracting URL's directly from the browser console.

###  [download_ftp_tree.py](./ftp) for FTP datasets
Government datasets are often stored on FTP; this script will capture FTP directories and subdirectories.

**PLEASE NOTE** that the Internet Archive has captured over 100Tb of government FTP resoures since December 2016. Be sure to check the URL using [Mike Hucka's check-ia script](./check-ia), the [Wayback Machine Extension](https://chrome.google.com/webstore/detail/wayback-machine/fpnmgdkabkmnadcjpehmlllkndpkmiak), or your own tool that uses [the Wayback Machine's API](https://github.com/internetarchive/wayback/blob/master/wayback-cdx-server/README.md) ([example 1](https://web.archive.org/cdx/search/cdx?url=ftp://aftp.cmdl.noaa.gov/user/vasel/posters/Screen%20Shot%202016-06-30%20at%2011.15.40%20AM.png), [example 2 w/ wildcard](https://web.archive.org/cdx/search/cdx?url=ftp://aftp.cmdl.noaa.gov/user/vasel/*). If the FTP directory you're looking at has not been saved to the Internet Archive, be sure that it has also been nominated as a web crawl seed. 

Whether it has been saved or not, you may decide  to download it for chain-of-custody preservation reasons. If so, this script should do what you need.

### [Ruby/Watir](./ruby-watir-collection) for full browser automation
The last resort of harvesting should be to drive it with a full web browser. It is slower than other approaches such as `wget`, `curl`, or a headless browser. Additionally, this implementation is prone to issues where the resulting page is saved before it's done loading. There is a ruby example in [tools/example-hacks/watir.rb](tools/example-hacks/watir.rb).

### Identify Data Links & acquire them via WARCFactory

For search results from large document sets, you may need to do more sophisticated "scraping" and "crawling" -- check out tools built at previous events such as the [EIS WARC archiver](https://github.com/edgi-govdata-archiving/eis-WARC-archiver) or the [EPA Search Utils](https://github.com/edgi-govdata-archiving/epa-search-utils) for ideas on how to proceed.

### [Explore Other Tools](./utils)

the `utils` directory is for scripts that have been useful in the past but may not have very general application. But you still might find something youu like!


### API scrape / Custom Solution

If you encounter an API, chances are you'll have to build some sort of custom solution, or investigate a social angle. For example: asking someone with greater access for a database dump. Be sure to include your ocode in the `tools` directory of your zipfile, and if there is any likelihood of general application, please add to this repo. 

## Contributing
We welcome tools written in any language! Especially if they cover use cases we haven't described well here!

To add a new tool to this repository, clone or branch the repo and create a new directory for the tool. This directory should include:
- a `UNIX man`-style README with usage instructions and also a succinct list of any dependencies (shell environment, ruby/python packages, etc.) The use-case for ht tool should also be described as well as possible. 
- a main executable file
- any other files required in order to run the tool

Please also add a brief descrition of the tool **and use-case** to this page.  

### Tool Specifications

Each tool should:
* set relevant global variables `path` and `???` in an immediately obvious way at the top of the script, or should provide an obvious way to set those variable through CLI arguments.
* write all harvested data to a `../data` directory


