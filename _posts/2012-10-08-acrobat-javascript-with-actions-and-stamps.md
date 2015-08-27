---
title: Acrobat Javascript with Actions and Stamps
author: emijayne
layout: post
permalink: /acrobat-javascript-with-actions-and-stamps/
post_views_count:
  - 916
categories:
  - random code
---
Imagine for a moment that it is your job to process hundreds of engineering &#8216;blueprints&#8217;, by rubber-stamping each one. In this little stamped form, you need to write often the same bit of information on every drawing. Fortunately, in today&#8217;s world real blueprints have been replaced with black and white electronic files. And in today&#8217;s world we have the technology that can allow you to automate this entire stamping process&#8230; making days of work be filtered down to maybe an hour. [In this scenario the files are all in PDF format and we are using Adobe Acrobat X Pro.]

#### The Problem:

A batch process needs to be done on multiple PDF files. This batch process will need to:

  * cycle through all files (regardless of the number of files),
  * use a customized version of Acrobat&#8217;s &#8220;stamp&#8221; annotation in order to automatically place (possibly multiple versions of) text onto each file,
  * save and overwrite (flatten) that file

#### The Twist:

This was accomplished once. A programmer had written a multi-program approach in VB6. One began the process in MS Outlook, stepped through MS Word to select files, and ended up in Acrobat 8 while utilizing a PDF-write tool written for Acrobat 5. It was memory-hungry, and it (usually) worked. Then the user group was upgraded to brand-new Windows 7, 64-bit machines, running MS 2010 and Acrobat X Pro. Oy! All VB6 programming was now out the window with the introduction of MS&#8217; VB.NET&#8230; and that fun little Acrobat 5 tool could no longer function in the new Acrobat X environment.

#### The Solution:

It seemed convoluted to me to rewrite it exactly in the same manner. (Although, at first I did try rewriting the VB6 script into .NET. It really didn&#8217;t translate well.)

It also seemed illogical that there wasn&#8217;t a way to do this whole process directly inside Acrobat X. As it turns out, there is a way.. I just needed to learn the way Acrobat used JavaScript. <img src="http://www.emijayne.com/wp/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

**Step 1: Getting the Data**

The dynamic stamps that are built-into Acrobat, pull data that is stored either in the system or the document itself. So, my logic was to do the same with my bits of information that I needed to utilize. The first bit was just the file name, then there were a few other bits of text that the user would need to supply.

Since they may have to batch 200 of these files, I didn&#8217;t want them to be interrupted and asked EVERY time a file popped up. So, I created a simple form asking for those extra bits of information. At the bottom of this form, I created a &#8220;save settings&#8221; button. This button takes each of those fields and saves them as Global Variables, deleting the variable first as good practice.

<pre>delete global.usrtxt1;
delete global.usrtxt2;
global.usrtxt1 = this.getField("UserText1").value;
global.usrtxt2 = this.getField("UserText2").value;</pre>

**Step 2: Building an Action**

Once those variables are set, I have the user run the Action Wizard that I set up for them. [&#8220;Action wizards&#8221; are found under &#8220;Tools&#8221;, and replace the old batch processes before Acrobat X.]

I set up a folder for each user to dump all the files they need to process. This wizard runs through that folder, going through all the steps that I set up. It then saves each file in the same folder.

The first step executes a bit of JavaScript on the file. The intention here is to grab the global variables, and insert them into the document&#8217;s meta information. This way, it is usable to the dynamic stamp we&#8217;ll set up later. Now, I had a bit of fun with this one because even though the first variable is always the same for each document, the second variable may be different for each document. The code below is set so that if the variable does not exist, it will prompt the user for the information.

<pre>this.info.usrtxt1 = global.usrtxt1;
if (global.usrtxt2) {
  this.info.usrtxt2 = global.usrtxt2;
} else {
  var ut2 = app.response({
    cQuestion: "Enter the text.",
    cTitle: "Text",
    cLabel: "Text:"
  });
 this.info.usrtxt2 = ut2;
}</pre>

Acrobat allows for &#8220;instruction steps&#8221;. The second step is an instruction that will simply prompt the user to stamp the file. Once the stamp is set, the user will tell the process to continue.

As I mentioned before, each file is saved along the way.

**Step 3: Dynamic Stamp Creation**

I needed a stamp that will be able to utilize those bits of text that were first saved as global variables, then transferred to the document itself.

Creating a customized stamp is not complicated:

  * go to Comment,
  * Annotations,
  * select the Stamp tool,
  * select Custom Stamps,
  * then Manage Stamps.
  * A dialog box will appear and you select Create,
  * browse to the PDF or image file you would like to use as your stamp, and click OK,
  * Select the category under which you&#8217;d like to file the new stamp, write a new name, and click ok.
  * Select &#8220;ok&#8221; to get out of the &#8220;Manage Custom Stamps&#8221; dialog.

Voila! You are done. Now to use the stamp, you&#8217;d simply go back to the stamp tool and select your new one. However, this is just a flat image with no customized text.

Although it starts there, creating a Custom Dynamic Stamp is a little different. There are many great tutorials on how to create a customized dynamic stamp. The gist of using dynamic fields on a stamp is that they MUST be calculated fields. To my understanding, this calculation forces the field to update before it is used on the document.

What I needed for this stamp was the document name and the two user defined fields back from step 1.

Below is the calculation I used for grabbing the document name. You&#8217;ll see a split/shift thing going on.. and basically that allowed me to chop off the file extension. Pretty cool stuff. <img src="http://www.emijayne.com/wp/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

<pre>var myFilename = event.source.source.documentFileName;
var fileNmRoot = myFilename.split(".").shift();
event.value = fileNmRoot;</pre>

For the two user defined fields, I simply told it to look for the field that was saved into the document&#8217;s meta information. The code for these two fields are the same:

<pre>var ut1 = event.source.source.info.usrtxt1;
event.value = ut1;</pre>

<pre>var ut2 = event.source.source.info.usrtxt2;
event.value = ut2;</pre>

.. and that&#8217;s the end of the story. <img src="http://www.emijayne.com/wp/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />