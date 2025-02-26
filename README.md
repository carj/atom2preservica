# atom2preservica
Python Module To Synchronize Metadata from the Access To Memory (AtoM) to Preservica.

AtoM it is a web-based, open source application for standards-based archival 
description and access in a multilingual, multi-repository environment.
https://accesstomemory.org/en/

Preservica is a widely used web based digital preservation platform which stores and protects digital objects.
https://preservica.com/

## How does atom2preservica work?

The atom2preservica module is designed to search Preservica for digital objects which exist in AtoM.
When it finds them it:

1) Sets the title and description of the asset based on the AtoM metadata. It also creates
a Dublin Core XML document describing the item and adds that to Preservica asset.

2) Creates the archival hierarchy in Preservica which matches the same levels of description in AtoM, e.g. the Fonds and series etc.
It moves the Preservica asset into the correct level of the newly created hierarchy.

3) Marks the Preservica assets as synchronised, so that they are ignored the next time the module is run again.


## Select Assets for Synchronisation

atom2preservica searches a Preservica collection or the entire Preservica repository for assets which have been marked as ready
for synchronisation. The mechanism used is that objects should have an external identifier containing the AtoM slug.
The key for the identifier should be "AToM-Slug". The value should be the slug of the AtoM object.


## Limitations

The atom2preservica module does not support the creation of new assets in Preservica. It only updates existing assets.
atom2preservica does not update AtoM, after the synchronisation process is complete. Preservica assets are linked to Atom,
but AtoM does not hold a link back to Preservica. The synchronisation is not yet bidirectional.

If descriptive metadata is updated in AtoM, the module will not update the asset metadata in Preservica.

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

