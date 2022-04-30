---
title: "OCR with GoLang"
date:   2022-04-30 13:00:00 +0900
category: golang
tags: golang
---
# Introduction
We are going to build an OCR service that digitalizes medicine history handbooks that is all so common in Japan.
* TOC
{:toc}

# Problem Statement
Everytime you get medical service in Japan, you will usually be given a list of medicine to take in support of your medical needs.

Once you go and get your medicine from the pharmacist, you are required to provide a "medicine history handbook". If you do not have your medicine handbook with you, the pharmacist will provision one for you. In other words, the medicine history handbook is mandated by the pharmacist, but not for the customer. 

This results in people forgetting their handbooks, and getting issued a new one each time. Raising the problem of:
- Wastage of paper
- No actual traceability of medicine history

# Root cause
The root cause was vaguely mentioned, whereby the effort required to store these handbooks are less than the benefits it can bring. 

# Goal
Reduce the amount of effort required to keep a medicine handbook.

# WHY
If we breakdown as to why something it is difficult to store these handbooks, we can see a few reasonings
- There is no easy way to visualize the medicine usage pattern
- There is no actionable feedback that we can derive from the medicine usage history
- There is no easy way to consolidate handbooks that are renewed

# Solutions
Normally, you would derive a few potential solutions to this. But given I'm a solo developer working on this, I'm limited in terms of available human resources. 

Hence the solution that is visibly available to me is developing a full stack application that automates the below things.

|Problem|Automation|
|-|-|
|Ease of visualization|Graph of consumed medicine and amount|
|Ease of feedback|Statistics in reduction/ increase in consumption of various medicine|
|Ease of consolidation|Provide OCR to scan paper into digital information and append to overall database|

# Implementation
So, how are we going to implement this? 

## User Story
We shall list a few user stories here to help us concretely understand what needs to be developed.
- As a user, I want to scan my medicine book into the application
- As a user, I want to see all my medicine data
- As a user, I do not want others to see my medicine data

## Diagrams
First of all, let us draw up the UML to understand the type of data we are dealing with.

![UML](/assets/uml.png)

Below is the initial architecture diagram.

![arhictecture](/assets/architecture.png)

## API endpoints
Based on our understanding above, these are our API Endpoints.

### FrontEnd
|Endpoint|Method|Description|
|-|-|-|
|/|GET|Show all medical records|
|/user/:id|GET|Show user info|
|/login|GET|Get login page|
|/register|GET|Get registration page|

### Brain
|Endpoint|Method|Description|
|-|-|-|
|/|GET|Get version of API|
|/login|POST|Login a user|
|/register|POST|Register a user|
|/user|POST|Create user|
|/user|PUT|Update user|
|/user/:id|GET|Get user information|
|/user/:id/medicine|GET|Get array of medicine for a week|
|/scan|POST|Send data to tesseract for scanning|

### Tesseract
|Endpoint|Method|Description|
|-|-|-|
|/|GET|Get version of API|
|/scan|POST|Analyze the image received|

# Implementation Problem
There are a few visible problems that are yet to be solved with this solution.

## Lack of standard
Not all handbooks are the same as it varies from pharmacy to pharmacy.

## Metadata split across pages
Some of the data are split into other pages. For example, a date in the left page is propogated across other pages.

The solution to this is to allow modifications at the frontend for ambiguous data.

# Tech debts
We will record some of the technical debt that arises from this.

## No verification of user registration
This system does not validate the email address that is used to create the account

## No strategy in event of network failure between microservices
At this moment in time, if one of the components were to fail mid analysis, ie tesseract loses connection to backend in midst of OCR analysis, the image sent will be lost and a 500 will be returned.