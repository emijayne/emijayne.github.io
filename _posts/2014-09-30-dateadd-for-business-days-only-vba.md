---
title: DateAdd for Business Days Only (VBA)
author: emijayne
layout: post
permalink: /dateadd-for-business-days-only-vba/
post_views_count:
  - 33519
categories:
  - random code
tags:
  - ms access
  - vba
---
When we set up projects here at work, Project Managers will let us know how soon they want Clients and Vendors to provide return documentation to us. Usually it is 10 business days.. which translates to two weeks. When we send out correspondence, our little Access database will do a calculation to figure out a due date.

However, we came across an error when one Project Manager wanted Clients to get back to us in 4 days. We kept getting an &#8216;overflow&#8217; message and it would print out &#8220;12:00:00 AM&#8221; instead of a date. So, I had to dig into the code to figure it out. It was pretty interesting. The original programmer had a conversion to weeks to figure out the proper number of days. It was only off by one day for most use, but would only ever work over 5 days (one week).

The week conversion would obviously not work for numbers less than 5, so I rewrote the functions:

<pre>Function AddDays(BegDate As Variant, dateCount As Integer) As Date

  Dim finalCount, iterate, weekend As Integer
  Dim EndDate As Date
  finalCount = dateCount
  iterate = 0
  weekend = 0

  Do Until (finalCount = iterate) And (finalCount - weekend = dateCount)
    iterate = iterate + 1
    If DatePart("w", DateAdd("d", iterate, BegDate)) = 1 Then
        finalCount = finalCount + 1
        weekend = weekend + 1
    ElseIf DatePart("w", DateAdd("d", iterate, BegDate)) = 7 Then
        finalCount = finalCount + 1
        weekend = weekend + 1
    End If
    EndDate = DateAdd("d", iterate, BegDate)
  Loop
  AddDays = EndDate

End Function</pre>

I wonder if there is be a simpler way to do this, but basically I am iterating through to check if that particular day is a weekend day, and if so, then it will add an extra day to the finalCount. This way, if the system specifies 10 business days, it will add 4 extra days to add the proper finalCount of 14 days. Also, it will still work for something as simple as 4 days.