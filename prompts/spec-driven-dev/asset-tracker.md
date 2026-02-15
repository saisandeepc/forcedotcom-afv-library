---
name: Asset Tracker Spec Build
description: Gather requirements and deliver a full Asset tracker app via a spec-driven workflow.
tags: spec-driven, lwc, custom-object, app
---

Create a simple Salesforce asset tracker application. The code should follow standard Salesforce styling and development best practices and be deployable to a scratch org or sandbox. Keep it fun, readable, and beginner-friendly. 

- Ask questions until you're 95% sure you can complete this task.
- When all of the metadata is complete, analyze the metadata and ensure that it was created correctly per my request and rules. Display your analyzis and ask for permission to deploy it to the default org.

## Custom Object Requirements

Create an `Asset__c` custom object with the following non-required fields:

- Autonumber Name field
- Asset Name (Text 80): A simple name for the asset, like "Dell Laptop" or "Server Rack 4."
- Status (Picklist): This field tracks the current state of the asset. 
  - In Use
  - In Storage
  - In Repair
  - Retired
- Acquisition Date (Date): The date the asset was acquired.
- Purchase Price (Currency): The cost of the asset.
- Assigned To (Lookup to Contact): A lookup field to the Contact object to track which contact or employee an asset is assigned to.

Add all of the fields to the page layout and create a tab for the object. Do not create or assign a compact layout.

## User Interface Requirements

Create an `assetTracker` LWC with a form that the user can enter a new record. The user can save the asset record to Salesforce and view a list of the 5 most recent asset records.

Add the LWC to a Lightning page and create a tab for the new page. 

Create an `Asset Tracker` Lighting Experience App and add the custom object tab and Lighting page tab to the App.

## Permission Set Requirements

- Create a new premission set that provides full access to the application, custom object and all of their custom fields.