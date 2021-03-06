---
title: "Navigation options for SharePoint Online"
ms.author: krowley
author: kccross
manager: laurawi
ms.date: 6/27/2018
ms.audience: Admin
ms.topic: overview
ms.service: o365-administration
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Adm_O365
search.appverid: SPO160
ms.assetid: adb92b80-b342-4ecb-99a1-da2a2b4782eb
description: "This article describes how to improve page load times for SharePoint Online by using managed navigation or search-driven navigation in Classic publishing."
---

# Navigation options for SharePoint Online

This article describes how to improve page load times for SharePoint Online by using managed navigation or search-driven navigation in Classic publishing.
  
SharePoint Online with classic publishing enabled has two navigation areas; Global Navigation and Current Navigation.
  
Global navigation is the top navigation menu while Current Navigation is the side or in-context left/right navigation dependent on the language configuration and master page utilized.
  
Navigation can negatively impact performance for the entire Portal as it is often configured for portal-wide use and ﻿as such is an important element of any SharePoint site.
  
Structural navigation is not the recommended navigation option in SharePoint Online as it was designed for an On-premise topology with security trimming and this design causes excessive server calls and impacts performance when it is used.
  
That design has changed with the introduction of Modern ﻿SharePoint sites where Modern has a simplified flattened site hierarchy. This has simplified navigation with a simplified hierarchy that has eliminated performance issues related to navigation.﻿
  
There are two main out-of-the-box navigation options in SharePoint as well as a third, custom, search-driven approach. Alternatively, a fourth and fairly popular option is to build a Custom Navigation Provider. Please review [Navigation solutions for SharePoint Online portals](https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/portal-navigation) for guidance on a Custom navigation provider. 
  
Each option has pros and cons as outlined in the following table.
  
|**Structural navigation**|**Managed navigation**|**Search-driven navigation**||**Custom Navigation Provider**|
|:-----|:-----|:-----|:-----|:-----|
| Pros:  <br/>  Easy to configure  <br/>  Security-trimmed  <br/>  Automatically updates as sites are added  <br/> | Pros:  <br/>  Easy to maintain  <br/>  Recommended option  <br/> | Pros:  <br/>  Security-trimmed  <br/>  Automatically updates as sites are added  <br/>  Fast loading time and locally cached navigation structure  <br/> || Pros:  <br/>  ﻿  <br/>  Wider choice / options available  <br/>  Fast loading when caching is used correctly﻿  <br/> |
| Cons:  <br/> **Not recommended** <br/> **Impacts performance** <br/> | Cons:  <br/>  Not automatically updated to reflect site structure  <br/>  Impacts performance if security trimming is enabled  <br/> | Cons:  <br/>  No ability to easily order sites  <br/>  Requires customization of the master page (technical skills required)  <br/> || Cons:  <br/>  Custom development is required  <br/>  External data source / cache stored is needed e.g. Azure﻿  <br/> |
   
The most appropriate option for your site will depend on your site requirements and on your technical capability. If you are comfortable using a custom master page and have some capability in the organization to maintain the changes that may occur in the default master page for SharePoint Online, then the search-driven option will produce the best user experience. If you want a simple middle ground between the out-of-the-box structural navigation and search, then the managed navigation is a very good option. The managed navigation option can be maintained through configuration, does not involve code customization files, and it is significantly faster than the out-of-the-box structural navigation.
  
Simplifying the overall structure of your Classic Portal much like Modern, helps with overall performance and scale as well. What this means is that instead of having a single Site Collection with hundreds / thousands of sites (subwebs), a better approach is to have many site collections with very few subsites (subwebs).
  
This provides additional scaling options in the service, avoids putting all content into one big database ﻿and ultimately allows greater flexibility for navigation and more importantly security.
  
## Using structural navigation in SharePoint Online

This is the out-of-the-box navigation used by default and is the most straightforward solution but as such has an expensive performance trade-off. It does not require any customization and a non-technical user can also easily add items, hide items, and manage the navigation from the settings page. This is however also true for Managed Navigation so it is recommended to use Managed Navigation as that can also be easily managed and controlled as well.
  
### Turning on structural navigation in SharePoint Online

To illustrate how the performance in a standard SharePoint Online solution with structural navigation and the show subsites option turned on. Below is a screen shot settings found on the page **Site Settings** \> **Navigation**.
  
![Screenshot showing subsites](media/5b6a8841-34ed-4f61-b6d3-9d3e78d393e7.png)
  
### Analyzing structural navigation performance in SharePoint Online

To analyze the performance of a SharePoint page use the **Network** tab of the F12 developer tools in Internet Explorer. 
  
![Screenshot showing F12 dev tools Network tab](media/e2aed8af-0843-487a-8b68-4f5260a2685e.png)
  
On the **Network** tab, click on the .aspx page that is being loaded and then click on the **Details** tab. 
  
![Screenshot showing the details tab](media/ad85cefb-7bc5-4932-b29c-25f61b4ceeb2.png)
  
Click **Response headers**.
  
![Screenshot of Details tab](media/c47770ac-5b2b-4941-9830-c57565dec4cc.png)
  
SharePoint returns some useful diagnostic information in its response headers. One of the most useful is **SPRequestDuration** which is the value, in milliseconds, of how long a request took to process on the server. 
  
In the following screen shot **show subsites** is unchecked for the structural navigation. This means that there is only the site collection link in the global navigation: 
  
![Screenshot showing load times as request duration](media/3422b2e8-15ec-4bb9-ba86-0965b6b49b01.png)
  
The **SPRequestDuration** key has a value of 245 milliseconds. This represents the time it took to return the request. Since there is only one navigation item on the site, this is a good benchmark for how SharePoint Online performs without heavy navigation. The next screen shot shows how adding in the subsites affects this key. 
  
![Screenshot showing a request duration of 2502 ms](media/618ee4e9-2ffa-4a22-b638-fa77b72292b8.png)
  
Adding the subsites has significantly increased the time it takes to return the page request.
  
The advantages of using the regular structured navigation is that you can easily organize the order, hide sites, add pages, the results are security-trimmed, and you are not deviating from the supported master pages used in SharePoint Online.
  
## Using managed navigation and managed metadata in SharePoint Online

Managed navigation is another out-of-the-box option that you can use to recreate the same sort of functionality as structural navigation.
  
The advantage of using managed metadata is that it is much faster to retrieve the data than using content by query to build the site navigation. Although it is much faster there is no way to security trim the results so if a user doesn't have access to a given site, the link will still show but will lead to an error message.
  
 **How to implement managed navigation and the results**
  
There are several articles on TechNet about the details of managed navigation, for example, see [Overview of managed navigation in SharePoint Server 2013](https://go.microsoft.com/fwlink/?LinkId=708689).
  
In order to implement managed navigation, you need to have term store administrator permissions. By setting up terms with URLs that match the structure of a site collection, managed navigation can be used to replace structural navigation. For example:
  
![Screenshot of Subsite1 example](media/4155b4da-ccff-4e2a-92de-7803ac62982b.png)
  
The following example shows the performance of the complex navigation using managed navigation.
  
![Screenshot of SPRequestDuration example](media/72257528-0ec6-48b0-85a5-efeb7432db5b.png)
  
Using managed navigation consistently improves performance compared to the content by query structural navigation approach.
  
## Using Search-driven client-side scripting

Using search you can leverage the indexes that are built up in the background using continuous crawl. This means there are no heavy content queries. The search results are pulled from the search index and the results are security-trimmed. This is faster than using regular content queries. Using search for structural navigation, especially if you have a complex site structure, will speed up page loading time considerably. The main advantage of this over managed navigation is that you benefit from security trimming.
  
This approach involves creating a custom master page and replacing the out-of-the-box navigation code with custom HTML. Follow this procedure to replace the navigation code in the file seattle.html.
  
In this example, you will open the seattle.html file and replace the whole element **id="DeltaTopNavigation"** with the custom HTML code. 
  
 **Example: To replace the out-of-the-box navigation code in a master page**
  
1. Navigate to the **Site Settings** page. 
    
2. Open the master page gallery by clicking **Master Pages**.
    
3. From here you can navigate through the library and download the file **seattle.master**.
    
4. Edit the code using a text editor and delete the code block in the following screen shot.
    
    ![Screenshot of DeltaTopNavigation code to delete](media/70db2cd2-660d-4e0d-9d5c-a5df331c73b4.png)
  
5. Remove the code between the \<SharePoint:AjaxDelta id="DeltaTopNavigation"\> and \<\SharePoint:AjaxDelta\> tags and replace it with the following snippet:
    
  ```
  <div id="loading">
    <!--Replace with path to loading image.-->
    <div style="background-image: url(''); height: 22px; width: 22px; ">
    </div>
  </div>
  <!-- Main Content-->
  <div id="navContainer" style="display:none">
      <div data-bind="foreach: hierarchy" class="noindex ms-core-listMenu-horizontalBox">
          <a class="dynamic menu-item ms-core-listMenu-item ms-displayInline ms-navedit-linkNode" data-bind="attr: { href: item.Url, title: item.Title }">
              <span class="menu-item-text" data-bind="text: item.Title">
              </span>
          </a>
          <ul id="menu" data-bind="foreach: $data.children" style="padding-left:20px">
              <li class="static dynamic-children level1">
                  <a class="static dynamic-children menu-item ms-core-listMenu-item ms-displayInline ms-navedit-linkNode" data-bind="attr: { href: item.Url, title: item.Title }">
                 
                   <!-- ko if: children.length > 0-->
                      <span aria-haspopup="true" class="additional-background ms-navedit-flyoutArrow dynamic-children">
                          <span class="menu-item-text" data-bind="text: item.Title">
                          </span>
                      </span>
                  <!-- /ko -->
                  <!-- ko if: children.length == 0-->   
                      <span aria-haspopup="true" class="ms-navedit-flyoutArrow dynamic-children">
                          <span class="menu-item-text" data-bind="text: item.Title">
                          </span>
                      </span>
                  <!-- /ko -->   
                  </a>
                 
                  <!-- ko if: children.length > 0-->                                                       
                  <ul id="menu"  data-bind="foreach: children;" class="dynamic  level2" >
                      <li class="dynamic level2">
                          <a class="dynamic menu-item ms-core-listMenu-item ms-displayInline  ms-navedit-linkNode" data-bind="attr: { href: item.Url, title: item.Title }">
           
            <!-- ko if: children.length > 0-->
            <span aria-haspopup="true" class="additional-background ms-navedit-flyoutArrow dynamic-children">
             <span class="menu-item-text" data-bind="text: item.Title">
             </span>
            </span>
             <!-- /ko -->
            <!-- ko if: children.length == 0-->
            <span aria-haspopup="true" class="ms-navedit-flyoutArrow dynamic-children">
             <span class="menu-item-text" data-bind="text: item.Title">
             </span>
            </span>                 
            <!-- /ko -->   
                          </a>
            <!-- ko if: children.length > 0-->
           <ul id="menu" data-bind="foreach: children;" class="dynamic level3" >
            <li class="dynamic level3">
             <a class="dynamic menu-item ms-core-listMenu-item ms-displayInline ms-navedit-linkNode" data-bind="attr: { href: item.Url, title: item.Title }">
              <span class="menu-item-text" data-bind="text: item.Title">
              </span>
             </a>
            </li>
           </ul>
             <!-- /ko -->
                      </li>
                  </ul>
                  <!-- /ko -->
              </li>
          </ul>
      </div>
  </div>
  ```

6. Replace the URL in the loading image anchor tag at the beginning, with a link to a loading image in your site collection. After you have made the changes, rename the file and then upload it to the master page gallery. This generates a new .master file.
    
7. This HTML is the basic markup that will be populated by the search results returned from JavaScript code. You will need to edit the following code to change the value for the  `var root = "site collection URL"` as demonstrated in the following snippet: 
    
  ```
  var root = "https://spperformance.sharepoint.com/sites/NavigationBySearch";
  ```

    The entire JavaScript file is as follows:
    
  ```
  //Models and Namespaces
  var SPOCustom = SPOCustom || {};
  SPOCustom.Models = SPOCustom.Models || {}
  SPOCustom.Models.NavigationNode = function () {
      this.Url = ko.observable("");
      this.Title = ko.observable("");
      this.Parent = ko.observable("");
  };
  var root = "https://spperformance.sharepoint.com/sites/NavigationBySearch";
  var baseUrl = root + "/_api/search/query?querytext=";
  var query = baseUrl + "'contentClass=\"STS_Web\"+path:" + root + "'&amp;trimduplicates=false&amp;rowlimit=300";
  var baseRequest = {
      url: "",
      type: ""
  };
  //Parses a local object from JSON search result.
  function getNavigationFromDto(dto) {
      var item = new SPOCustom.Models.NavigationNode();
      if (dto != undefined) {
          var webTemplate = getSearchResultsValue(dto.Cells.results, 'WebTemplate');
          if (webTemplate != "APP") {
              item.Title(getSearchResultsValue(dto.Cells.results, 'Title')); //Key = Title
              item.Url(getSearchResultsValue(dto.Cells.results, 'Path')); //Key = Path
              item.Parent(getSearchResultsValue(dto.Cells.results, 'ParentLink')); //Key = ParentLink
          }
      }
      return item;
  }
  function getSearchResultsValue(results, key) {
      for (i = 0; i < results.length; i++) {
          if (results[i].Key == key) {
              return results[i].Value;
          }
      }
      return null;
  }
  //Parse a local object from the serialized cache.
  function getNavigationFromCache(dto) {
      var item = new SPOCustom.Models.NavigationNode();
      if (dto != undefined) {
          item.Title(dto.Title);
          item.Url(dto.Url);
          item.Parent(dto.Parent);
      }
      return item;
  }
  /* create a new OData request for JSON response */
  function getRequest(endpoint) {
      var request = baseRequest;
      request.type = "GET";
      request.url = endpoint;
      request.headers = { ACCEPT: "application/json;odata=verbose" };
      return request;
  };
  /* Navigation Module*/
  function NavigationViewModel() {
      "use strict";
      var self = this;
      self.nodes = ko.observableArray([]);
      self.hierarchy = ko.observableArray([]);;
      self.loadNavigatioNodes = function () {
          //Check local storage for cached navigation datasource.
          var fromStorage = localStorage["nodesCache"];
          if (false) {
              var cachedNodes = JSON.parse(localStorage["nodesCache"]);
              if (cachedNodes &amp;&amp; timeStamp) {
                  //Check for cache expiration. Currently set to 3 hrs.
                  var now = new Date();
                  var diff = now.getTime() - timeStamp;
                  if (Math.round(diff / (1000 * 60 * 60)) < 3) {
                      //return from cache.
                      var cacheResults = [];
                      $.each(cachedNodes, function (i, item) {
                          var nodeitem = getNavigationFromCache(item, true);
                          cacheResults.push(nodeitem);
                      });
                      self.buildHierarchy(cacheResults);
                      self.toggleView();
                      addEventsToElements();
                      return;
                  }
              }
          }
          //No cache hit, REST call required.
          self.queryRemoteInterface();
      };
      //Executes a REST call and builds the navigation hierarchy.
      self.queryRemoteInterface = function () {
          var oDataRequest = getRequest(query);
          $.ajax(oDataRequest).done(function (data) {
              var results = [];
              $.each(data.d.query.PrimaryQueryResult.RelevantResults.Table.Rows.results, function (i, item) {
                  if (i == 0) {
                      //Add root element.
                      var rootItem = new SPOCustom.Models.NavigationNode();
                      rootItem.Title("Root");
                      rootItem.Url(root);
                      rootItem.Parent(null);
                      results.push(rootItem);
                  }
                  var navItem = getNavigationFromDto(item);
                  results.push(navItem);
              });
              //Add to local cache
              localStorage["nodesCache"] = ko.toJSON(results);
              localStorage["nodesCachedAt"] = new Date().getTime();
              self.nodes(results);
              if (self.nodes().length > 0) {
                  var unsortedArray = self.nodes();
                  var sortedArray = unsortedArray.sort(self.sortObjectsInArray);
                  self.buildHierarchy(sortedArray);
                  self.toggleView();
                  addEventsToElements();
              }
          }).fail(function () {
              //Handle error here!!
              $("#loading").hide();
              $("#error").show();
          });
      };
      self.toggleView = function () {
          var navContainer = document.getElementById("navContainer");
          ko.applyBindings(self, navContainer);
          $("#loading").hide();
          $("#navContainer").show();
      };
      //Uses linq.js to build the navigation tree.
      self.buildHierarchy = function (enumerable) {
          self.hierarchy(Enumerable.From(enumerable).ByHierarchy(function (d) {
              return d.Parent() == null;
          }, function (parent, child) {
              if (parent.Url() == null || child.Parent() == null)
                  return false;
              return parent.Url().toUpperCase() == child.Parent().toUpperCase();
          }).ToArray());
          self.sortChildren(self.hierarchy()[0]);
      };
      self.sortChildren = function (parent) {
          // sjip processing if no children
          if (!parent || !parent.children || parent.children.length === 0) {
              return;
          }
          parent.children = parent.children.sort(self.sortObjectsInArray2);
          for (var i = 0; i < parent.children.length; i++) {
              var elem = parent.children[i];
              if (elem.children &amp;&amp; elem.children.length > 0) {
                  self.sortChildren(elem);
              }
          }
      };
      // ByHierarchy method breaks the sorting in chrome and firefix 
      // we need to resort  as ascending
      self.sortObjectsInArray2 = function (a, b) {
          if (a.item.Title() > b.item.Title())
              return 1;
          if (a.item.Title() < b.item.Title())
              return -1;
          return 0;
      };
      self.sortObjectsInArray = function (a, b) {
          if (a.Title() > b.Title())
              return -1;
          if (a.Title() < b.Title())
              return 1;
          return 0;
      }
  }
  //Loads the navigation on load and binds the event handlers for mouse interaction.
  function InitCustomNav() {
      var viewModel = new NavigationViewModel();
      viewModel.loadNavigatioNodes();
  }
  function addEventsToElements() {
      //events.
  
  ```

     $("li.level1").mouseover(function () { 
  
var position = $(this).position();
  
$(this).find("ul.level2").css({ width: 100, left: position.left + 10, top: 50 });
    
     }) 
  
.mouseout(function () {
  
$(this).find("ul.level2").css({ left: -99999, top: 0 });
  
});
  
$("li.level2").mouseover(function () {
  
var position = $(this).position();
  
console.log(JSON.stringify(position));
  
$(this).find("ul.level3").css({ width: 100, left: position.left + 95, top: position.top});
    
     }) 
  
.mouseout(function () {
  
$(this).find("ul.level3").css({ left: -99999, top: 0 });
  
});
    
    } _spBodyOnLoadFunctionNames.push("InitCustomNav");
    
    To summarize the code shown above in the jQuery **[$(document).ready]** function there is a **[viewModel]** object created and then the **[loadNavigationNodes()]** function on that object is called. This function either loads the previously built navigation hierarchy stored in the HTML5 local storage of the client browser or it calls the function **[queryRemoteInterface()]**. 
    
    **[QueryRemoteInterface()]** builds a request using the **[getRequest()]** function with the query parameter defined earlier in the script and then returns data from the server. This data is essentially an array of all the sites in the site collection represented as data transfer objects with various properties. This data is then parsed into the previously defined **[SPO.Models.NavigationNode]** objects which use Knockout.js to create observable properties for use by data binding the values into the HTML that we defined earlier. The objects are then put into a results array. This array is parsed into JSON using Knockout and stored in the local browser storage for improved performance on future page loads. 
    
8. Next, the results are assigned to the **[self.nodes]** array and a hierarchy is built out of the objects using linq.js assigning the output to an array **[self.heirarchy]**. This array is the object that is bound to the HTML. This is done in the **[toggleView()]** function by passing the self object to the **[ko.applyBinding()]** function. This then causes the hierarchy array to be bound to the following HTML: 
    
  ```
  <div data-bind="foreach: hierarchy" class="noindex ms-core-listMenu-horizontalBox">
  ```

    Finally, the event handlers for **[mouseenter]** and **[mouseexit]** are added to the top-level navigation to handle the subsite drop-down menus which is done in the **[addEventsToElements()]** function. 
    
    The results of the navigation can be seen in the screen shot below:
    
    ![Screenshot of navigation results](media/020d34d2-942a-431b-9b26-6d2c8a6fde30.png)
  
    In our complex navigation example a fresh page load without the local caching shows the time spent on the server has been cut down from the benchmark structural navigation to get a similar result as the managed navigation approach.
    
    ![Screenshot of SPRequestDuration 301](media/307d7caf-e83a-40d9-a551-91d842521d03.png)
  
    One major benefit of this approach is that by using HTML5 local storage, the navigation is stored locally for the user the next time they load the page.
    
We get major performance improvements from using the search API for structural navigation; however, it takes some technical capability to execute and customize this functionality. In the example implementation, the sites are ordered in the same way as the out-of-the-box structural navigation; alphabetical order. If you wanted to deviate from this order, it would be more complicated to develop and maintain. Also, this approach requires you to deviate from the supported master pages. If the custom master page is not maintained, your site will miss out on updates and improvements that Microsoft makes to the master pages.
  
The above code has the following dependencies:
  
- jQuery - [http://jquery.com/](http://jquery.com/)
    
- ﻿KnockoutJS - [http://knockoutjs.com/](http://knockoutjs.com/)
    
- ﻿Linq.js - [http://linqjs.codeplex.com/](http://linqjs.codeplex.com/), or [github.com/neuecc/linq.js](https://github.com/neuecc/linq.js/)
    
The current version of LinqJS does not contain the ByHierarchy method used in the above code and will break the navigation code. To fix this, add the following method to the Linq.js file before the line "Flatten: function ()".
  
```
ByHierarchy: function(firstLevel, connectBy, orderBy, ascending, parent) {
     ascending = ascending == undefined ? true : ascending;
     var orderMethod = ascending == true ? 'OrderBy' : 'OrderByDescending';
     var source = this;
     firstLevel = Utils.CreateLambda(firstLevel);
     connectBy = Utils.CreateLambda(connectBy);
     orderBy = Utils.CreateLambda(orderBy);
    
     //Initiate or increase level
     var level = parent === undefined ? 1 : parent.level + 1;
    return new Enumerable(function() {
         var enumerator;
         var index = 0;
        var createLevel = function() {
                 var obj = {
                     item: enumerator.Current(),
                     level : level
                 };
                 obj.children = Enumerable.From(source).ByHierarchy(firstLevel, connectBy, orderBy, ascending, obj);
                 if (orderBy !== undefined) {
                     obj.children = obj.children[orderMethod](function(d) {
                         return orderBy(d.item); //unwrap the actual item for sort to work
                     });
                 }
                 obj.children = obj.children.ToArray();
                 Enumerable.From(obj.children).ForEach(function(child) {
                     child.getParent = function() {
                         return obj;
                     };
                 });
                 return obj;
             };
        return new IEnumerator(
        function() {
             enumerator = source.GetEnumerator();
         }, function() {
             while (enumerator.MoveNext()) {
                 var returnArr;
                 if (!parent) {
                     if (firstLevel(enumerator.Current(), index++)) {
                         return this.Yield(createLevel());
                     }
                } else {
                     if (connectBy(parent.item, enumerator.Current(), index++)) {
                         return this.Yield(createLevel());
                     }
                 }
             }
             return false;
         }, function() {
             Utils.Dispose(enumerator);
         })
     });
 },

```


