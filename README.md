# Automated_Emails

<h2>Description</h2>
Mastery based grading schemes are becoming more common in education. There are a variety of ways to implement these, but a common thread is the idea to allow students multiple attempts or multiple ways of demonstrating their knowledge and learning. Below is an example of one these schemes I implemented at UMD in an introductory probability course. The set of specifications I developed used a mix of points and letter marks from grading rubrics. The built in gradebook in our learning management system could not handle the mix of letters and point values so I had to make a custom gradebook and wrote a script to send automated grade update emails each week to all of my students.
<br />


<h2>Languages and Software used Used</h2>

- <b>Google Sheets</b> 
- <b>Google Apps Script (JavaScript) </b>

<h2>Techniques Used </h2>

- <b>Text and Logic Spreadsheet Functions</b>
 </b>

<h2>Project walk-through or samples:</h2>

First, it helps to see the grading table that was in the syllabus. Below are the instructions for how one determines their grade. The details on resubmissions, extra attempts, modifiers, etc., are left off of this example.

 <h3 id="kl_panel_4" class="kl_panel_heading kl_panel_4">Determining Your Grade</h3>
            <div id="kl_panel_4_content" class="kl_panel_content kl_panel_4">
                <p>Your grade for the semester is not based on an overall percentage or some weighted average of points. Most items in this course are not worth point values. Your grade is instead determined by a set of <strong>specifications </strong>indicating the quantity and quality of evidence you can provide of mastery of the core content of Probability and Statistics.</p>
                <p>To determine your base grade, use the following table. To earn a grade, you must complete <strong>all</strong> the requirements in the column for that grade; your base grade is the <strong>highest grade level for which all the requirements have been met or exceeded</strong>.</p>
                <table style="border-collapse: collapse; width: 100%;" border="1">
                    <caption>Base Grade Specifications</caption>
                    <tbody>
                        <tr>
                            <th style="width: 20%; text-align: center;" scope="col"><strong>Component</strong></th>
                            <th style="width: 20%; text-align: center;" scope="col"><strong>D</strong></th>
                            <th style="width: 20%; text-align: center;" scope="col"><strong>C</strong></th>
                            <th style="width: 20%; text-align: center;" scope="col"><strong>B</strong></th>
                            <th style="width: 20%; text-align: center;" scope="col"><strong>A</strong></th>
                        </tr>
                        <tr>
                            <td style="width: 20%;">Online HW (cumulative point total)</td>
                            <td style="width: 20%;">
                                <p>60% or more</p>
                            </td>
                            <td style="width: 20%;">
                                <p>70% or more</p>
                            </td>
                            <td style="width: 20%;">
                                <p>80% or more</p>
                            </td>
                            <td style="width: 20%;">
                                <p>90% or more</p>
                            </td>
                        </tr>
                        <tr>
                            <td style="width: 20%;">Individual HW (5 total)</td>
                            <td style="width: 20%;">All but at most three earn M or higher</td>
                            <td style="width: 20%;">All but at most one earn M or higher</td>
                            <td style="width: 20%;">All at M or higher, with at least one E</td>
                            <td style="width: 20%;">All at M or higher, with at least four E</td>
                        </tr>
                        <tr>
                            <td style="width: 20%;">Group HW (14+)</td>
                            <td style="width: 20%;">50% of problems earn M or higher</td>
                            <td style="width: 20%;">80% of problems earn M or higher</td>
                            <td style="width: 20%;">All earn M or higher, with at least 20% at E</td>
                            <td style="width: 20%;">100% M or higher with 70% or more at E</td>
                        </tr>
                        <tr>
                            <td style="width: 20%;">Individual Exam Questions (15-20)</td>
                            <td style="width: 20%;">At least Partial Credit on all but 10% of questions</td>
                            <td style="width: 20%;">Partial Credit on all questions</td>
                            <td style="width: 20%;">Partial Credit on all questions with at least 50% full credit</td>
                            <td style="width: 20%;">Partial Credit on all with at least 90% full credit</td>
                        </tr>
                    </tbody>
                </table>
                <p>Again, <strong><em>all</em> of the requirements in a grade column must be met or exceeded in order to earn that grade</strong>. Otherwise your grade is the <em>highest</em> grade for which <em>all</em> the requirements are met are exceeded. For example, if you earned 88% on the Online HW, you are not eligible to earn a base A, your base grade is B at best.&nbsp; A grade of F will be given if the requirements for D are not met.</p>
                
<h3>Script for Automated Email Update </h3>

The script for the email update can be found [here](https://github.com/AaronShepanik/Automated_Emails/blob/main/Send_Emails) and is shown below. The script references fields from the gradebook below.

~~~
/**
 * Sends emails with data from the current spreadsheet.
 */
function sendEmails() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2; // First row of data to process
  var numRows = 93; // Number of rows to process -change this number based on the number students to send emails to in the sheet
  // Fetch the range of cells A2:B3
  var dataRange = sheet.getRange(startRow, 1, numRows, 4);
  // Fetch values for each row in the Range, syntax is getRange (start row cell, starting column cell, number of rows in the range, number of columns in the range
  var data = dataRange.getValues();
  for (var i in data) {
    var row = data[i];
    var name = row[0]; // first column
    var emailAddress = row[1]; // second column
    var message = row[2]; // third column
    var grade = row[3]; // fourth column
    var subject = 'Automated Grade Update from A. Shepanik';
    
    message=message.replace("<name>", name).replace("<grade>", grade);
    MailApp.sendEmail(emailAddress, subject, message, {cc: ''});
  }
}
~~~
<h3>The gradebook </h3>

<p align="center">
Here we just record the grade data for HW, etc.,  (attempts A and B if needed) in cells E on up as needed: <br/>
<img src="https://github.com/AaronShepanik/Automated_Emails/blob/main/Images/WrittenHW.png"/>
<br />
<br />

<p align="center">
Cells B, C, and D are what are used to compose the message. The formula in Cell D2 is shown that aggregates the gradebook data into a useful summary for the student: <br/>
<img src="https://github.com/AaronShepanik/Automated_Emails/blob/main/Images/EmailMessageAggregation.png"/>
