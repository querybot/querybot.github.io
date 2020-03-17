---
layout: post
title: Create Dynamic Nested Folders with Blue Prism
date: 2018-01-15 16:33
author: Query Bot
comments: true
categories: [BluePrism, Code Stage]
youtubeId: YOKFDlIdNMs
---
Create dynamic Nested folders using Blue Prism tool. This blog post explains about creation of nested folders using c# as well as VB code.

{% include youtubePlayer.html id=page.youtubeId %} 

C# Code
```csharp
var levelsList = inCollection.AsEnumerable().Select(dItem => new { 
  Level = Convert.ToInt32(dItem["Level"]) 
}).ToList();

var maxDepth = levelsList.Max(depth => depth.Level);

var maxDepthItems = inCollection.AsEnumerable().Where(dItem => Convert.ToInt32(dItem["Level"]) == maxDepth).ToList();

foreach(DataRow item in maxDepthItems)
{
   var minDepthLevel = Convert.ToInt32(item["Level"]);
   var parentId = Convert.ToInt32(item["ParentId"]);
   string folderPath = item.Field<string>("Name");

  do
  {
    var depthEachItem = inCollection.AsEnumerable().Where(dItem => Convert.ToInt32(dItem["ID"]) == parentId).ToList().FirstOrDefault();
    if(depthEachItem != null)
    {
      minDepthLevel = Convert.ToInt32(depthEachItem["Level"]);
      parentId = Convert.ToInt32(depthEachItem["ParentId"]);
      folderPath = depthEachItem.Field<string>("Name") + "\\" + folderPath;
    }
  }while(minDepthLevel > 0);

  string totalPath = inputBasePath + "\\" + folderPath;

  if(!Directory.Exists(totalPath)){
     Directory.CreateDirectory(totalPath);
   }
}
```
Code for Dynamic Nested Folder Creation with Blue Prism

VB.net Code
```vb
Dim levelsList = inCollection.AsEnumerable().Select(
Function(Item, Index) New With
{
  .Level = Convert.ToInt32(Item("Level"))
}).ToList()
Dim maxDepth As Int32 = 0

For Each mxItem As Object In levelsList
    If maxDepth < Convert.ToInt32(mxItem.Level) Then
          maxDepth = Convert.ToInt32(mxItem.Level)
    End If
Next

Dim maxDepthItems = inCollection.AsEnumerable()
    .Where(Function(dItem) Convert.ToInt32(dItem("Level")) = maxDepth).ToList()

For Each item As DataRow In maxDepthItems
Dim minDepthLevel As Int32 = Convert.ToInt32(item("Level"))
Dim parentId As Int32 = Convert.ToInt32(item("ParentId"))
Dim folderPath As String = item("Name")

Do While minDepthLevel > 0
    Dim depthEachItem = inCollection.AsEnumerable().Where(Function(dItem) Convert.ToInt32(dItem("ID")) = parentId).ToList().FirstOrDefault()
    If depthEachItem IsNot Nothing Then
        minDepthLevel = Convert.ToInt32(depthEachItem("Level"))
        parentId = Convert.ToInt32(depthEachItem("ParentId"))
        folderPath = depthEachItem("Name") + "\\" + folderPath
    End If
Loop

Dim totalPath As String = baseDirPath + "\\" + folderPath

If Directory.Exists(totalPath) = False Then
     Directory.CreateDirectory(totalPath)
End If

Next
```