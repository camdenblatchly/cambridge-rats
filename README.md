## Mapping Cambridge's rat hotspots

Code for [my story](https://camdenblatchly.github.io/pages/rats/) on 
311 rat reports in Cambridge, MA.

### Background

For this story, I wanted to used open data from Cambridge or Somerville, MA. 
After digging around on their open data portals, I decided to focus on 
rat-centric 311 reports. My initial story questions were:

- Which areas have the most rat reports?
- Are rat reports increasing or decreasing over time?
- Are the new SMART box rat traps working?

### Analysis and methods

#### Scope

Initially, I considered doing a unified analysis of Somerville and 
Cambridge 311 data, but, because they implement different 311 systems, I felt 
that the categorization was too different, such that displaying the 
data together could be misleading. In addition, I thought thought that it
would be too lengthy to show data for both cities separately, so I decided to 
focus only on Cambridge because their 311 data was richer than Somerville's.

#### Data Sources

- [Cambridge 311 data](https://data.cambridgema.gov/Public-Works/Commonwealth-Connect-Service-Requests/2z9k-mv9g/about_data)
- [Smart Rat Traps data](https://data.cambridgema.gov/Public-Works/Smart-Rat-Boxes/78bs-b5ig/about_data)

#### Methods

**Creating a dataset of rat-related 311 requests**

Cambridge 311 data includes an `issue_category` field that loosely encodes 
what category the request falls into (e.g., Bike Lane Obstruction,  
Traffic Sign Complaint, etc.). Most relevant to my 
analysis was the `Rodent Sighting` category, but other categories, such 
`Roadkill / Dead Animal` and `Overflowing Public Trash/Recycling Receptacle`, 
were often rat-related too. 

To create a comprehensive rat dataset, I first filtered to reports that 
mentioned “rat” or "rats" in the description using a regular expression. 
Then, based on finding that around 80% of Rodent Sighting reports 
mentioned rats, I also included those without descriptions or 
with general descriptions that didn’t mention other animals 
(like mice or raccoons). My assumption was that these ambiguous reports were 
most likely about rats, even if they did not mention rats explicitly.

**Creating a normalized metric for mapping**

To clarify spatial patterns, I linked each 311 report to its 
Census block and counted the number of rat reports per block. To 
mitigate the effects of outlier submissions, I calculated the average 
number of rat reports per year between 2016 (the first year available) 
and 2024 (the most recent complete year).

To address discrepancies in Census block size, I then found the average 
number of rat reports per year per square kilometer. This step helped 
prevent large Census blocks (such as parks) from receiving undue 
visual weight.

I also considered using a population-weighted estimate, but felt that a 
population-weighted measure would overemphasize reports in business districts, 
such as Kendall Square.

#### New Skills

**AI for open response classification**

I used `pydantic` and `openai` to loop through 311 report descriptions and 
determine if a report was rat-related or not. It worked pretty well! When I 
tested a sample of reports, it got them almost 99% right. Ultimately, though, 
this approach was very slow to run and didn't perform any better at determining 
rat-relatedness than using regular expressions. In the interest of time and 
money, I used regular expressions to construct my complete dataset.

**QGIS, Adobe Illustrator, and image-based scrollytelling**

I used this project to practice creating custom maps using QGIS and Adobe 
Illustrator. While I think an interactive map would have worked too (because 
people want to look up their block!), having worked on a Mapbox story in 
my last project, I thought the custom maps felt more unique and gave me a chance 
to try something new in an image-based scrollytelling format.


#### If I had more time

I would:  

- Create an interactive map of rat hotspots using React/Mapbox
- Consider incorporating Somerville and/or Boston into my analysis too
- Add more fun annotations to my custom maps!
