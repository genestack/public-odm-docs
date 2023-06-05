# How to get ontologies from ODM
1. From the Dashboard go to Template Editor
    ![1](1.png)
1. You will see all the templates
   ![2](2.png)
2. To review ontologies used in a particular template, open the template by clicking on the title.
   ![3](3.png)
3. You will see all the attributes, including which are associated with Dictionaries and you can square the list of Dictionaries used.
5. To get source-file of ontology, you will need accession (e.g. GSF000041)
6. Then you will need to go to the File Manager by a direct link:\
```[your_host]/frontend/endpoint/application/run/genestack/filebrowser```
7. Put ontology Accession in the search field and click on the search button. You will see templates which use this dictionary and the dictionary itself. <br/>
   ![7](7.png)
There are templates files in the search results as well as dictionary files, to easily find a dictionary, look at the column "Kind" and find "Dictionary":
   ![8](8.png)
8. Click on the file title, you will see the context menu, click on "View metainfo"
   ![9](9.png)
9. To download the source file click on the link from the Data URL attribute:
   ![10](10.png)
    Or you can go to the source in the description:
![11](11.png)