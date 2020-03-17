---
layout: post
title: Send Email with Custom Table Data with Blue Prism
date: 2018-01-06 18:12
author: Query Bot
comments: true
categories: [BluePrism, Code Stage]
youtubeId: KRdeok5ypXE
---
Hi Folks, Welcome back.

Sending email is easy task in Blue Prism, However it is bit dificult to inject Table inside email body using Blue Prism.

With the help of Code Stage will achive this by preparing html body which includes dynamic data in it.

Assumptions :
<ul>
 	<li>Data available in Collection (source might be from Excel or Database).</li>
 	<li>User already know how to fill Collection from excel or database.</li>
</ul>
Once above assumption is done, will start writing a program which will take above Collection as input and it will prepare and store email body in html format  to given Data Item.

Required Libraries and Name Spaces :

Library : System.dll (Add this in External References section)

Namespaces : System.Data, System.Collections.Generic, System.Text (Add these items in Namespace Import section)

Once it is set, will jump to code.

It is better to have a visual. If yes have a look at below Video

{% include youtubePlayer.html id=page.youtubeId %}

```csharp
StringBuilder sb = new StringBuilder();
if(InData.Rows.Count > 0)
{
    sb.Append("Hi There,<br><br>");
    sb.Append("Please find the bellow mentioned Information. <br><br>");
    sb.Append("<table style='border:1px solid black; border-collapse: collapse;'>");
    sb.Append("<tr style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
    foreach (DataColumn dc in InData.Columns)
    {
       sb.Append("<th style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
       sb.Append(dc.ColumnName);
       sb.Append("</th>");
    }
    sb.Append("</tr>");

    foreach (DataRow dr in InData.Rows)
    {
       sb.Append("<tr style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
       foreach (DataColumn dc in InData.Columns)
       {
          sb.Append("<td style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
          sb.Append(dr[dc.ColumnName].ToString());
          sb.Append("</td>");
       }
       sb.Append("</tr>");
    }
    sb.Append("</table><br><br>");
    sb.Append("Regards,<br>");
    sb.Append("Team QueryBot");
}
outEmailHtmlTable = sb.ToString();
```
Adding color to first row with code.

Code for first column coloring with Yellow.

```csharp
StringBuilder sb = new StringBuilder();
if (InData.Rows.Count > 0)
{
    sb.Append("Hi There,<br><br>");
    sb.Append("Please find the bellow mentioned Information. <br><br>");
    sb.Append("<table style='border:1px solid black; border-collapse: collapse;'>");
    sb.Append("<tr style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
    string firstColumnName = InData.Columns[0]?.ColumnName;
    foreach (DataColumn dc in InData.Columns)
    {
         if (firstColumnName.Equals(dc.ColumnName))
         {
             sb.Append("<th style='border:1px solid black; border-collapse: collapse; padding:2px;background-color:yellow;'>");
         }
         else
         {
             sb.Append("<th style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
         }

         sb.Append(dc.ColumnName);
         sb.Append("</th>");
    }
    sb.Append("</tr>");

    foreach (DataRow dr in InData.Rows)
    {
         sb.Append("<tr style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
         foreach (DataColumn dc in InData.Columns)
         {
            if (firstColumnName.Equals(dc.ColumnName))
            {
                 sb.Append("<th style='border:1px solid black; border-collapse: collapse; padding:2px;background-color:yellow;'>");
            }
            else
            {
                 sb.Append("<th style='border:1px solid black; border-collapse: collapse; padding:2px;'>");
            }
            sb.Append(dr[dc.ColumnName].ToString());
            sb.Append("</td>");
    }
    sb.Append("</tr>");
  }
  sb.Append("</table><br><br>");
  sb.Append("Regards,<br>");
  sb.Append("Team QueryBot");
}
outEmailHtmlTable = sb.ToString();
```

