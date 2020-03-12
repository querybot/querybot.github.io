---
layout: post
title: Create Dynamic Nested Folders with Blue Prism
date: 2018-01-15 16:33
author: Query Bot
comments: true
categories: [BluePrism, Code Stage]
---
C# Code
<pre>
var levelsList = inCollection.AsEnumerable().Select(dItem =&amp;amp;amp;amp;amp;gt; new { Level = Convert.ToInt32(dItem[&quot;Level&quot;]) }).ToList();

var maxDepth = levelsList.Max(depth =&amp;amp;amp;amp;amp;gt; depth.Level);

var maxDepthItems = inCollection.AsEnumerable().Where(dItem =&amp;amp;amp;amp;amp;gt; Convert.ToInt32(dItem[&quot;Level&quot;]) == maxDepth).ToList();

foreach(DataRow item in maxDepthItems)
{
   var minDepthLevel = Convert.ToInt32(item[&quot;Level&quot;]);
   var parentId = Convert.ToInt32(item[&quot;ParentId&quot;]);
   string folderPath = item.Field&amp;amp;amp;amp;amp;lt;string&amp;amp;amp;amp;amp;gt;(&quot;Name&quot;);

  do
  {
    var depthEachItem = inCollection.AsEnumerable().Where(dItem =&amp;amp;amp;amp;amp;amp;gt; Convert.ToInt32(dItem[&quot;ID&quot;]) == parentId).ToList().FirstOrDefault();
    if(depthEachItem != null)
    {
      minDepthLevel = Convert.ToInt32(depthEachItem[&quot;Level&quot;]);
      parentId = Convert.ToInt32(depthEachItem[&quot;ParentId&quot;]);
      folderPath = depthEachItem.Field&amp;amp;amp;amp;amp;lt;string&amp;amp;amp;amp;amp;gt;(&quot;Name&quot;) + &quot;\\&quot; + folderPath;
    }
  }while(minDepthLevel > 0);

  string totalPath = inputBasePath + &quot;\\&quot; + folderPath;

  if(!Directory.Exists(totalPath)){
     Directory.CreateDirectory(totalPath);
   }
}
</pre>
Code for Dynamic Nested Folder Creation with Blue Prism

VB.net Code
<pre>
Dim levelsList = inCollection.AsEnumerable().Select(
Function(Item, Index) New With
{
.Level = Convert.ToInt32(Item(&quot;Level&quot;))
}).ToList()
Dim maxDepth As Int32 = 0

For Each mxItem As Object In levelsList
    If maxDepth < Convert.ToInt32(mxItem.Level) Then
          maxDepth = Convert.ToInt32(mxItem.Level)
    End If
Next

Dim maxDepthItems = inCollection.AsEnumerable().Where(Function(dItem) Convert.ToInt32(dItem(&quot;Level&quot;)) = maxDepth).ToList()

For Each item As DataRow In maxDepthItems
Dim minDepthLevel As Int32 = Convert.ToInt32(item(&quot;Level&quot;))
Dim parentId As Int32 = Convert.ToInt32(item(&quot;ParentId&quot;))
Dim folderPath As String = item(&quot;Name&quot;)

Do While minDepthLevel &amp;amp;amp;amp;amp;gt; 0
    Dim depthEachItem = inCollection.AsEnumerable().Where(Function(dItem) Convert.ToInt32(dItem(&quot;ID&quot;)) = parentId).ToList().FirstOrDefault()
    If depthEachItem IsNot Nothing Then
        minDepthLevel = Convert.ToInt32(depthEachItem(&quot;Level&quot;))
        parentId = Convert.ToInt32(depthEachItem(&quot;ParentId&quot;))
        folderPath = depthEachItem(&quot;Name&quot;) + &quot;\\&quot; + folderPath
    End If
Loop

Dim totalPath As String = baseDirPath + &quot;\\&quot; + folderPath

If Directory.Exists(totalPath) = False Then
     Directory.CreateDirectory(totalPath)
End If

Next
</pre>
Input Collection Collection Image