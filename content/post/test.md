---
title: Test blog
subtitle: Welcome 👋 We know that first impressions are important, so we've populated your new site with some initial content to help you get familiar with everything in no time.

# Summary for listings and search engines
summary: Welcome 👋 We know that first impressions are important, so we've populated your new site with some initial content to help you get familiar with everything in no time.

# Link this post with a project
projects: []

# Date published
date: "2020-12-13T00:00:00Z"

# Date updated
lastmod: "2020-12-13T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin
- Hugh Lilly, Billie Berry and Andre Geldnehuis

tags:
- Academic


categories:
- Test

---

# Collecting Sensitive Data (in Qualtrics)

In most cases when collecting personal or *sensitive data*, it's important to keep survey responses separate from any identifying data such as an email address. This helps ensure good data hygeine and mitigates risks if there happens to be an unfortunate data breache.

If there's no need to collect personally identifiable data, best practice is not to ask for it. Remember the principle, **only collect what you need**! 

However, there are cases where you may need or want to preserve the option to align deidentified survey data with contact information, or other information, from your survey respondentes. For example, you might need to compare a participant’s response from one survey to the next in a longitudinal study; or there might be a good reason why you need to follow up with them.

Two Qualtrics-based approaches for solving this common research promlem involve the creation and use of a `participant ID`, a unique, non-personally identifiable "identifier" which is meaningless on its own, but can be used to link to personally identifiable information when a researcher has a cause to do so. 

These two approaches are: 

1) a screener survey is created to automatically generate `participant ID` that is then embedded in the main research survey. 
2) An existing spreadsheet - containing contact and other identifiable information, formatted in a specific way and exported as a `.csv` file - is used to manually create a `participant ID` field and uploaded to Qualtrics. This `.csv` file can also be used to create a contact list that Qualtrics can use to directly email respondents.


## Approach 1: Using a Screener Survey

* In this scenario, you have not yet assigned `participant IDs`. 

* You have a screener survey that you are using to collect contact information and/or determine whether the participant is eligible for the study. 

* At the end of the screener survey, the participant will automatically be redirected to a second survey which will collect data for your study (referred to as the “research survey”). (If you have criteria for participation, you can [use survey logic to only redirect eligible participants](https://www.qualtrics.com/support/survey-platform/survey-module/survey-flow/standard-elements/end-of-survey-element/).)

* When they are redirected, Qualtrics will pass along a `participant ID`. The research survey will store its research data with their `participant ID`, but the data will remain separate from any sensitive personal data stored in the first (screener) survey.

### Step 1.  **Create the Research Survey**
This is the second survey the participant will take. You don’t need to add all the questions yet, we’re just setting it up now so we can get its link.
![](https://i.imgur.com/jOuv4QW.png)

1. Create a survey. *(New to Qualtrics? Here’s their basic documentation on creating a survey.) You can add questions now or do it later.*

2. Click the Survey Flow button at the top of the survey.


3. Add a new element, and choose Embedded Data as the type, and name the field ID - it’ll look like the image below. This is where the value passed from the screener survey will be stored.

 ![](https://i.imgur.com/oPoqoaK.png)
![](https://i.imgur.com/gn9vRAJ.png)


Click the `Move` link and drag this Embedded Data flow element above, but on the same hierarchical level, as the Default Question Block. We like it to be first so any future logic in our survey won’t skip over it. After you move it, the two blocks will look like this:

![](https://i.imgur.com/KEzxA7b.png)![]


Click `Apply` return to the survey builder ![](https://i.imgur.com/LTx6GnW.png)

Click the `Publish` button to activate the survey. (Don’t worry if you’re not done with your questions - as long as you don’t share this link with participants yet, it’s fine to add questions after activating).

You will now see an anonymous survey link on the page - copy this, you’ll need it in the next section.

e.g. http://vuw.qualtrics.com/jfe/form/SV_71lLtOC5SEZLwSG

~~Optional: If you want to be sure that the embedded data is coming through, you might want to put in a branch that checks for the embedded data and prevents someone from taking the research survey if it's not present. (Usually this is not a problem.)~~

![](https://i.imgur.com/yde3KeH.png)


### Step 2: **Create the Screener Survey**
Create a survey and add questions. (New to Qualtrics? Here’s their basic documentation on creating a survey.)

Click the Survey Flow button at the top of the survey.

Add a new element, and choose Embedded Data as the type. Call this field ID as you did before, but this time, click Set a Value Now, and set it equal to `${e://Field/ResponseID}`  (you can just copy and paste that value into the text box). This ResponseID refers to a randomly generated unique value that Qualtrics already creates every time someone fills out your survey. When you’re done, it will look like this:

![](https://i.imgur.com/Bo2zhPr.png)


As previously, click the Move link inside the element box, and drag this Embedded Data flow element above (but on the same hierarchical level) as the Default Question Block. We do this so that every survey participant is assigned an ID before answering the questions. After you move it, the two blocks will look like this:


Survey Termination section, select `Redirect to a URL`.

![](https://i.imgur.com/bSEjKPj.png)

![](https://i.imgur.com/Gr9xDbf.png)

Paste the anonymous survey link for the other survey (Research Survey) - the link you copied at the end of the last section of instructions - into the text field provided. At the end of this link add the following text:
`?ID=${e://Field/ID}`
(Note: if there is already a question mark in the address, use a & instead of the ?)



As usual, Publish your screener survey and share the anonymous link with participants.

http://vuw.qualtrics.com/jfe/form/SV_eWnGsmQIDlNrLZY
 
This is the link participants will click on. At the end of the survey they will automatically be redirected to the research survey

### Step 3: **View Responses**
When you download your data from the Research Survey, the value indicated in the ID column will correspond to the ResponseID value in the data for the Screener Survey.

![Parent Navigation](https://i.imgur.com/lILW7Lo.png)
![Export tab](https://i.imgur.com/BkbFeDR.png)
![Data tab](https://i.imgur.com/yN69Cpl.png)


![download as](https://i.imgur.com/YjVFj22.png)


## Approach 2: Using Pre-Generated Participant IDs




In the first appraoch the participant ID was automatically generated when the participant took a screener survey. 

#### Using Pre-Assigned Participant IDs
In this scenario, you start with an Excel spreadsheet with participant info and pre-assigned participant IDs. In Qualtrics, Panels allow you to send out surveys to specific people using their e-mail. In order to still keep data anonymous you need to have an `ID#` in data and not their personal information. When they click on the link in their email the following instructions will allow you to add variables in your data to track who is who anonymously - especially if you send them multiple surveys.

 

#### Step 1: Create a Participant Spreadsheet 

Use the format below to create your panel. It needs to be saved as a `.csv` file - you can create it in Excel. The first four headings have to be exactly the same as the example: FirstName, LastName, Email, ExternalDataReference. The rest of the headings can be whatever variables you want embedded in your data set. One of these should contain the same participant ID as is in the ExternalDataReference column. In this example, we called that column “ID” - you can call it something different as long as you use that name consistently when following other parts of these instructions which refer to ID.

You can include the name and email address as it they will be stripped out of the results as long as you use the recommended column names (FirstName, LastName, Email, ExternalDataReference). Including the name in the panel allows you to personalize the email you will be sending out for survey. By following the rest of these instructions, Qualtrics will omit any information in the first four columns, leaving only the ID (and whatever other additional columns you add) in the dataset at the end of the survey.


 

#### Second: Create the Survey
Create a survey. (New to Qualtrics? Here’s their basic documentation on creating a survey.)

Create a new panel, choose to import from a file, and upload your .csv file. If you’ve never created a panel before, Qualtrics has instructions here. When you upload the .csv file into the panel, the column with the heading "ID" (and any additional columns you added to its right) should be in blue, the rest should be black.

Now, you will add Embedded Data to the survey so the ID, as well as any additional columns you added to its right, will be included in your data set. Click the Survey Flow button.

![](https://i.imgur.com/LP0d0AB.png)

 

Click `Add New Embedded Data Field`

Create embedded data elements for each of your custom column headers. The naming needs to be exactly the same. In this case, I have `ID` and `PlaceOfBirth`.

![](https://i.imgur.com/tNaQKcQ.png)


Move this element to the top of the survey flow by clicking `Move` and dragging it up. It should look something like this when you’re done:

Click `Apply`
**Important: At the top of the survey, click `Survey Options` and then `Security`, then check the `Anonymize responses` slider at the bottom of the page.


Now your survey is set up! To send emails, follow instructions for [Using the Qualtrics Mailer](https://www.qualtrics.com/support/survey-platform/distributions-module/email-distribution/emails-overview/), indicating your panel in the `To` field.  After the initial distribution, you can track how many participants have responded and use the [Send Reminder or Thank You](https://www.qualtrics.com/support/survey-platform/distributions-module/email-distribution/reminder-thank-emails/) feature to follow up with participants.

Create contact list, then `Distributions > Compose Email > To > Select Contacts From a List > Choose List`
![](https://i.imgur.com/U0f4KzC.png)

![](https://i.imgur.com/VUggmo1.png)

 

#### Step 3: View Responses 
When you download your data, it will not include the values in the first four columns of your panel. However, it will include the ID so that you could, for example, identify participants who should take a second survey based on their responses to the first, or compensate participants for their effort, using the contact information in your original .csv.

To maintain confidentiality, take precautions if you download and store your survey responses and panel .csv; for example, you might password protect these files, password protect your computer, and/or obscure the file names so the relationship is not obvious to someone who might unexpectedly access your files.