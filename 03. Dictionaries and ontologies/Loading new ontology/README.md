# How to load custom dictionaries (ontologies)
This article explains how to load custom dictionaries (ontologies) in ODM.

> Uploading a dictionary with the same name as a previous dictionary will create a new version (new accession) of the dictionary and the system will mark the old version as obsolete, which will highlight it red in the template editor. Templates will need to be updated to use the new dictionary.

## Requirements
* Python 3
* pip
* Genestack Python client installed. See [how to setup Genestack python client](../../00.%20Packages%20to%20install/1.%20Genestack%20python%20client)
* Auxiliary scripts installed. See [how to install Genestack auxiliary scripts](../../00.%20Packages%20to%20install/2.%20Genestack%20auxiliary%20scripts)
* A file describing dictionaries, e.g.: [dictionaries.json](dictionaries.json)
* One or more dictionaries in CSV, JSON, OWL, OBO or TTL formats, hosted at FTP or HTTP web addresses or contained in a local folder, see [dictionary example](http://purl.obolibrary.org/obo/go.owl)

## Setting up dictionary name/location
Open the ```dictionaries.json``` file and replace the ```name```, ```url``` and ```description``` sections with the desired dictionary name as it will appear in ODM, dictionary location, and dictionary description.

```
[
    {
        "name": "NCI Thesaurus",
        "url": "http://purl.obolibrary.org/obo/ncit.owl",
        "description": "NCI Thesaurus (NCIt) is a reference terminology that includes broad coverage of the cancer domain, including cancer related diseases, findings and abnormalities. The NCIt OBO Edition aims to increase integration of the NCIt with OBO Library ontologies"
    }
]
```
Multiple dictionaries can be supplied by repeating the section in curly brackets. \
Local files can be supplied with the full path on the machine - replace ```FULL_PATH_TO_THE_FILE``` to the path to the dictionary, in this case the ```url``` should be specified as:
``` 
"url": "/FULL_PATH_TO_THE_FILE/body_weight_unit.csv"
```

Or the dictionary file name can be used if the dictionary file is in the same directory as the script. In this case the ```url``` should be specified as:

``` "url": "body_weight_unit.csv" ```

## Running the command to import dictionaries
Run the following command from your terminal:

```shell
odm-update-dictionary -u YOUR_ALIAS_FOR_USER --file_with_dictionaries /FULL_PATH_TO_THE_DICTIONARIES_JSON_FILE/dictionaries.json
```

```-u``` [optional] parameter should contain the alias for the user you set up with the Genestack python client previously. \
```-H [hostname]``` [optional] hostname for the environment being used; you can use either -u or -H.  \
```--file_with_dictionaries``` parameter should contain full path to the ```dictionaries.json``` file - replace ```FULL_PATH_TO_THE_DICTIONARIES_JSON_FILE``` with this path. \
or run

```shell
odm-update-dictionary -u YOUR_ALIAS_FOR_USER --file_with_dictionaries dictionaries.json
``` 

if ```dictionaries.json``` is presented in the current working directory.

Once loaded the dictionary needs to be indexed. This will occur automatically in the background but can take several minutes (~25 minutes for a 600MB ontology). The indexing task can be monitored in the tasks log.
