# System Performance Measures (SPMs)

Code to help ServicePoint users run and manage their SPM reports

This repository contains all the code you'll need to check that you ran your SPMs correctly, to compare this year to last year, and gives you data frames you can build on to do whatever other data manipulations or visualizations you would like.

## Step 0: To Change the 0701 or Not To Change the 0701?

The "get_SPM_data.R" script expects the Returns to Homelessness report (the 0701) to have one more tab than it does if you download the 0701 as-is from ServicePoint. You can either modify the code to not expect this -OR- you can follow the instructions at the bottom of this page to modify your 0701 ART report. We add an extra tab to break out the PSH from the RRH recurrence rates. If you decide to modify your 0701, please use the instructions at the bottom of the page to modify your 0701. If you decide not to, you should modify the code to look at Sheet 1 instead of Sheet 2 (lines 789 and 883 in the get_SPM_data.R script). 

Once this has been sorted out, you can proceed to the next steps.

## Steps

1. Upon cloning this repository and creating a project in R Studio, you should create a folder directly in the project called "SPM_data", then two folders under that called "Current" and "Prior".

2. Next, in ART, schedule all of your 0700 reports on this year's date range, export to Excel and save them all as-is to the "Current" directory, then do it all again, but on last year's date range, and save those to the "Prior" directory. 

3. Open each Excel file and click "Enable Editing", then close the file. This is very important, as if you don't do this (at least on my machine) the script will not work when it tries to read the file. If anyone knows how to get around this, please let me know.

3. Open "get_SPM_data.R" in R Studio and on line 21, you will see there is a variable holding the name of our CoC. You should change this to whatever your CoC's name is in the CoC prompt in ART. It should look very similar to how ours is.

4. Either modify the 0701 in ART or modify the script. See the next section for details.

## How to Break Out PSH from RRH in the Recurrence Report (0701)

1. Start with a fresh 0701 report straight from the ART Gallery.

2. Run the report on any date range in Edit mode.

### Creating the variable you'll need:

3. In the Data tab in the left sidebar, find the variable named “First Proj Type”. Right click it and select “Duplicate”.

4. Double-click the variable named "First Proj Type (1)" and change its name to "First Proj Type PH", then replace the formula with the following:

```
=If([BISData].[Entry Exit Provider Program Type Code]="Emergency Shelter (HUD)";"2ES"
;If([BISData].[Entry Exit Provider Program Type Code]="Safe Haven (HUD)";"4SH"
;If([BISData].[Entry Exit Provider Program Type Code]="Street Outreach (HUD)";"1SO"
;If([BISData].[Entry Exit Provider Program Type Code]="Transitional housing (HUD)";"3TH"
;If([BISData].[Entry Exit Provider Program Type Code] inlist( "PH - Permanent Supportive Housing (disability required for entry) (HUD)";"PH - Housing only (HUD)");"6PSH Only"
;If([BISData].[Entry Exit Provider Program Type Code] = "PH - Rapid Re-Housing (HUD)";"7RRH"
;[BISData].[Entry Exit Provider Program Type Code])))))) Where([Valid First Exit]="X") In([Merge Unique Id];[Merge EE])
```

5. Save the variable. Save the report too in case ART crashes.

### Creating the tab where PH will be broken out:

6. Right click on "Tab A – Summary" and click “Duplicate Report”. Rename the tab "PSH, RRH broken out" or something similar.

7. Double-click the first column where the rows read like "Exits from...".

8. Press Ctrl-Enter to get the formula editor.

9. Replace the variable that is there to the one created above called "First Proj Type PH". Click OK.

10. Save the report.

### Fix the Alerter so it looks nice (optional):

11. Click the icon with the red triangle and an ! in the middle. Select the alerter called "Project Type Null" and click Edit.

12. There are six sub-alerters inside this and they are all filtering on "First Proj Type". Change each of those to "First Proj Type PH". (Click the three dots button, then "Select an object or variable", then select "First Proj Type PH".)

13. The last sub-alerter has "5All PH" for its Operand. Change that to "6PSH Only", then click "Format...". Change the Display text to "Exits from PSH". Click OK.

14. Click "Add Sub-Alerter". Check that your Filtered object is "First Proj Type PH" and if not, change it to that. In the Operands field, type "7RRH" and click "Format...". Change the color to Default. For the display text, type "Exits from RRH". Click OK, OK, OK.

15. Save the report.


