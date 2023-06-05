# How to load custom templates

This article describes how to load templates from a local computer into ODM.

Loaded templates are available to all users on the instance.

## Requirements

* Python 3
* pip
* The user should have the "Set up templates" permission and API token. See [Getting a Genestack API token](https://odm-user-guide.readthedocs.io/en/latest/doc-odm-user-guide/getting-a-genestack-api-token.html#token-label)
* Genestack Python client installed. See [how to setup Genestack python client](../../00.%20Packages%20to%20install/1.%20Genestack%20python%20client)
* Auxiliary scripts installed. See [how to install Genestack auxiliary scripts](../../00.%20Packages%20to%20install/2.%20Genestack%20auxiliary%20scripts)
* A template settings json file, e.g.: [default_ODM_template_settings.json](default_ODM_template_settings.json)
* A template json file, e.g.: [Default_ODM_Template.json](templates/Default_ODM_Template.json)

## Instructions

1. Download or create a template json file (see Requirements). Templates are collections of fields with rules determining whether the field is required, which dictionary to use (if any) etc.
    ```
    [
        {
         "dataType":"genestack:sampleObject",
         "name":"Organism",
         "metainfoType":"com.genestack.api.metainfo.StringValue",
         "dictionaryName":"NCBI Taxonomy",
         "isRequired":true,
         "isReadOnly":false,
         "description": "Key description, text of 500 characters"
        }
    ]
    ```
2. Download the template settings json file (see Requirements)
3. Use a text editor to edit the template settings file and change the path to match where you have put your template json file
   
    ```"template_path": "/PATHTOTEMPLATEFILE/Default_ODM_Template.json",```
4. Run the `odm-update-template` script:
    ```
    odm-update-template -H GENESTACK_ENDPOINT_ADDR /PATHTOSETTINGSFILE/default_ODM_template_settings.json
    ```
   Where `GENESTACK_ENDPOINT_ADDR` is the URL of the Genestack platform.

## Template_settings.json

This file controls certain parameters of the template file:

```
{
    "template_path": "Default_ODM_Template.json", //template path/file name
    "template_name": "Default Template", //name of the template
    "replace": true,  //if true, will replace previous template that has the same name
    "mark_default": false //mark this template as the new default template for the organisation
}
```

## How to create a new template using template_schema.json

The template json file is validated in ODM against an internal schema: [template_schema.json](schemas/template_schema.json)

1. A single template file contains the properties (keys) for the metadata of study, sample and expression objects together.
2. For each object type (study, samples, libraries and so on) a specific dataType should be used:

    Study: `dataType = "study"`

    Samples: `dataType = "genestack:sampleObject"`

    Expression (both Transcriptomics and Proteomics): `dataType = "genestack:transcriptomicsParent"` 

3. The `Accession` property is mandatory for each type of object. The following values should be set for each dataType section:

    ```
    name = "genestack:accession",
    metainfoType = "com.genestack.api.metainfo.StringValue",
    isRequired = true,
    isReadOnly = true
    ```

4. The values accepted for each metadata attribute are given by the `metainfoType` key, and must be one of the following values:

    ```
   "com.genestack.api.metainfo.IntegerValue",
    "com.genestack.api.metainfo.DecimalValue",
    "com.genestack.api.metainfo.StringValue",
    "com.genestack.api.metainfo.BooleanValue",
    "com.genestack.api.metainfo.DateTimeValue",
    "com.genestack.api.metainfo.ExternalLink"
   ```

    * for values like FLOAT use `"com.genestack.api.metainfo.DecimalValue"`; 
    * for values of type INT use `"com.genestack.api.metainfo.IntegerValue"`; 
    * for values of type DATE or TIME use `"com.genestack.api.metainfo.DateTimeValue"`.
     
   Please note that no value bounds can be set up currently.
5. Dictionaries (controlled vocabularies which validate against lists of terms) can be specified for a metadata attribute using the `dictionaryName` key.
   See [loading new ontology](../../03.%20Dictionaries%20and%20ontologies/Loading%20new%20ontology).
6. For full details refer to the [template_schema.json](schemas/template_schema.json) file.
7. The order of the attributes in the template json file is preserved in the template in ODM.
