# atom2preservica
Python Module To Synchronize Metadata from the Access To Memory (AtoM) platform to Preservica.

AtoM it is a web-based, open source application for standards-based archival 
description and access in a multilingual, multi-repository environment.
https://accesstomemory.org/en/

Preservica is a widely used web based active digital preservation platform which stores and protects digital objects.
https://preservica.com/

## How does atom2preservica work?

The atom2preservica module is designed to search Preservica for digital objects which have archival descriptions in AtoM.
When it finds them it:

1) Sets the title and description of the asset in Preservica based on the AtoM metadata. It also creates
a Dublin Core XML metadata document from AtoM metadata describing the asset and adds that to Preservica asset.
It also sets the level of description on the Preservica metadata along with any AtoM identifiers.

2) Creates the full archival hierarchy (levels of description) in Preservica which matches the same levels of description in AtoM, 
e.g. the Fonds and series etc.
It moves the Preservica asset into the correct level of the newly created Preservica hierarchy.

3) Marks the Preservica assets as synchronised, so that they are ignored the next time the module is run again.


## Select Assets for Synchronisation

atom2preservica searches a Preservica collection or the entire Preservica repository for assets which have been marked as ready
for synchronisation. The mechanism used to determine that an asset is ready for linking is that it should have an 
external identifier in Preservica containing the AtoM slug.
The key for the identifier should be "AToM-Slug". The value should be the slug of the AtoM object.

For example:

![AToM-Slug](https://raw.githubusercontent.com/carj/atom2preservica/refs/heads/main/docs/images/slug.png)

The atom2preservica tool will not add slugs to Preservica objects, this should be done either during ingest or after
ingest manually or via the API.


## Limitations

The atom2preservica module does not support the creation of new assets in Preservica. It only updates existing assets.
For an automated way to add Assets into Preservica see https://pypreservica.readthedocs.io/.

atom2preservica does not update or change AtoM, after the synchronisation process is complete. Preservica assets are linked to Atom,
but AtoM does not hold a link back to Preservica. The synchronisation is not yet bidirectional.

If descriptive metadata is updated in AtoM after the synchronisation, the module will not update the asset metadata in Preservica.

The AtoM Rest Plugin arRestApiPlugin needs to be activated for atom2preservica to work.

## Support 

atom2preservica is 3rd party open source library and is not affiliated or supported by Preservica Ltd or Artefactual.
There is no support for use of the library by Preservica Ltd or Artefactual.
Bug reports can be raised directly on GitHub.  https://github.com/carj/atom2preservica/

## License

The package is available as open source under the terms of the Apache License 2.0

## Installation

atom2preservica is available from the Python Package Index (PyPI)

https://pypi.org/project/atom2preservica/

To install atom2preservica, simply run this simple command in your terminal of choice:

    $ pip install atom2preservica



## Usage

atom2preservica can be configured via command line arguments or by using a properties file.

To use a properties file, create a file called `credentials.properties` in the current working directory with the following properties:

    [credentials]
    username=user@example.com
    password=123456
    server=eu.preservica.com
    atom-api-key=123456788
    atom-server=https://demo.accesstomemory.org
    security-tag=open
    search-collection=913a6bfd-874e-4cb0-8940-e1b0d2a583a7
    new-collections=0c733043-4816-4d74-9492-906badc1bce0

The property descriptions are:

    username - Your Preservica username
    password - Your Preservica password
    server -   Your Preservica server domain name without the https:// part

    atom-api-key - The API key for the AtoM user,  https://accesstomemory.org/en/docs/2.8/dev-manual/api/api-intro/#api-intro-auth-key
    atom-server -  The URL of the AtoM server
    
    security-tag - The security tag for any new collections added to Preservica
    search-collection - The Preservica collection to search for assets, ignore to search the entire repository
    new-collections - The location where new AtoM Fonds/Series/Sub-series should be created in Preservica. If not set, new collections will be created at the root of the repository
   
You can then run the script without any arguments as:

    $ python -m atom2preservica

You can download a sample `properties.credentials` file from the github repository.
https://raw.githubusercontent.com/carj/atom2preservica/refs/heads/main/credentials.properties.template


Alternatively, you can use command line arguments to configure the script. The following arguments are available:

    usage: atom2preservica [-h] -a ATOM_SERVER [-k ATOM_API_KEY] [-au ATOM_USER]
                       [-ap ATOM_PASSWORD] [-st SECURITY_TAG] [-c COLLECTION]
                       [-cr NEW_COLLECTIONS_ROOT]  [-u PRESERVICA_USERNAME]
                       [-p PRESERVICA_PASSWORD] [-s PRESERVICA_SERVER]

    mandatory arguments:

         -a [--atom-server]     The URL of the AtoM server

    optional arguments:

            -k [--atom-api-key]             The API key for the AtoM user
            -au [--atom-user]               The username for the AtoM user
            -ap [--atom-password]           The password for the AtoM user

            -st [--security-tag]            The security tag for any new collections added to Preservica
            -c [--search-collection]        The Preservica collection to search for assets, ignore to search the entire repository

            -cr [--new-collections]         Add location where new AtoM Fonds/series should be created in Preservica. 
                                            If not set, new collections will be created at the root of the repository

            -u [--preservica-username]      Your Preservica username if not using credentials.properties
                                
            -p [--preservica-password]      Your Preservica password if not using credentials.properties
                             
            -s [--preservica-server]        Your Preservica server domain name if not using credentials.properties
     


## Example

For example to synchronise assets from the AtoM demo server searching across the entire Preservica repository for 
linked assets:

    $ python -m atom2preservica --atom-server demo.accesstomemory.org --atom-api-key ae049f6e0924d477                       