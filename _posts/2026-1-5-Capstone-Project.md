# Capstone Project - AI Yearbook Digitization

## Research

## Description of Capstone

For my capstone project, I chose to help design and build an automated data extraction and analysis tool that helps collect and organize large sets of raw information from past yearbooks at Charlotte Latin and turn it into a preservable digital database. The goal of the project is to reduce the amount of manual work required when trying to access data from decades of yearbooks and creating a digital database to preserve the information throughout time.

I chose this project because it allows me to combine the theoretical coding I have been learning to real-world applications. This project enables me to use my abilties to create a useful tool for the community my school and community.

A successful project would:
- Automatically extract relevant data from past yearbook pdfs
- Clean and standardize the extracted data
- Output the results in a structured format ready for analysis
- Increase efficiency by reducding human manual labor
- Create a searchable AI database with the extracted data

Ultimately, success for this project means creating a tool that saves time, improves accuracy, and can realistically be continously used in future years to make an efficient way to continue adding data to a digital searchable database.

## INSTRUCTIONS SECTION  

## Tools Used 

### Software Tools:
- **Python** – language used to build the extraction and processing logic
- **VS Code** – editor for writing and debugging scripts
- **GitHub** – file control and  management


## Workflow of Extraction:

### Step 1: Configuration Setup  
A YAML file is created for each yearbook yearbook for metadata: page ranges for each section (e.g. clubs, sports, etc.) and expected counts of names for validation. This YAML file serves as a helpful guide for the AI throughout the entire extraction process.

### Step 2: Parallel Extractions  
The system uses Python multiprocessing to extract multiple yearbook pages simultaneously with each parallel script extracting a different secrtion and page at the same time to increase speed and efficiency. These scripts are created to extract different sections of the yearbook like lower schoolers vs sports.

### Step 3: Orchestration  
A universal orchestration script runs all extraction scripts in parallel based with the help of the pre-determined page number sections in the YAML file. It successfully extracts information and outputs into a unified database for review.

### Step 4: YAML File Review   
Extracted results are validated by comparing the number of detected records against expected counts in the YAML file. Pages with mismatches are logged while the successful outputs are created into JSON files as the final product.

### Step 5: Manual Review  
Remaining unmatched names are exported to a CSV for human review using a spreadsheet. This allows fast bulk corrections for nicknames and abbreviations without building a tedious custom script. It is hard to catch everything, so the manual review helps to catch the little amount that the AI extraction misses.

### Step 6: Apply Corrections  
Manual corrections from the CSV are validated and merged back into the database with the previously approved data. The final output is then ready for use.


## Struggles

One of the main challenges I faced was validating and correcting the YAML configuration files that control the entire extraction workflow. The initial YAML files were partially generated using AI, but when I 
manually reviewed the 1975 yearbook, I found that many page numbers, name counts, and even names were incorrect. In several  sections, the AI had generated names that did not exist in the yearbook at all, 
while sports sections were only occasionally accurate. To fix this, I went page by page through the PDF and compared it against the YAML file. Instead of tracking individual names, I instead ensured 
that page ranges and expected person counts were correct since those are critical for validation whearas specific names weren't as important.

## Daily Journal

12/10:
Today, I went through the 1975 final yearbook pdf to comapre the page numbers and name counts with the 1975 YAML file. When doing this, I first noticed that the names, name counts, and page numbers 
were incorrect majority of the time(AI attempted the first try). While it sometimes got the page numbers right(more for sports groups) and names correct, a lot of the names were made up and not within the yearbook (this happened
for the class pictures and names a lot). 

12/11:
I continued working on the 1975 YAML and finished fixing it this class. Instead of tracking names, the main focus was getting the page number and count of names right.

12/12:
Today, I started checking the 1977 YAML file. In this one, the librarians also had a name count with page numbers. After checking, majority of the name counts were correct, the only thing was that for each page, the 
librarian counted duplicate names on the same page.

12/15:
Today, I modified the 1979 yearbook to have the page numbers of the pdf align with the actual page numbers of the yearbook. Additioanlly, I checked the librarians count in the 1979 YAML file.

## Reflection (Ongoing)

This project taught me that large projects are less about technical difficulty and more about **organization, persistence, and adaptability**. I learned that mistakes are not setbacks but signals that something needs to be redesigned or clarified. Moving forward, I would like to expand this tool into a more user-friendly application with a graphical interface and broader dataset support.


