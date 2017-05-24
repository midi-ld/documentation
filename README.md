# The MIDI Linked Data Project

**Contributors:**	[Albert Meroño](https://github.com/albertmeronyo), [Rinke Hoekstra](https://github.com/RinkeHoekstra), Aldo Gangemi, Peter Bloem, Reinier de Valk, Bas Stringer, Berit Janssen, Victor de Boer, Alo Allik, Stefan Schlobach, Kevin Page

For the `midi2rdf` converter, see [the project page](https://github.com/midi-ld/midi2rdf) or the [cloud service](http://midi2rdf.amp.ops.labs.vu.nl).

## Table of Contents

1. [Introduction](#introduction)
2. [The MIDI Linked Data Cloud](#tools)
3. [Using the data](#usage)
4. [Tutorials](#tutorials)
5. [Changelog](#changelog)
6. [FAQ](#faq)

### Introduction
<a name="introduction"></a>

MIDI Linked Data proposes to use the data publishing method of [Linked Data](https://en.wikipedia.org/wiki/Linked_data) to interconnect symbolic music descriptions (to a great extent very similar to music scores) contained in MIDI files. By doing this, we believe there is a great opportunity to overcome traditional problems in music databases, such as interoperability, entity linking, and semantic relations. By doing this we can, for example, enrich the [DBpedia page of Hey Jude by the Beatles](http://dbpedia.org/page/Hey_Jude) with [all the notes and instruments of Hey Jude by the Beatles](purl.org/midi-ld/pattern/53f034801899d84fd061d73bd4716912).

### The MIDI Linked Data Cloud
<a name="tools"></a>

To achieve the goal of representing MIDI files as Linked Data, we need two things: a collection of MIDI files, and a "MIDI to RDF" conversion algorithm. The collections of MIDI files we currently represent as MIDI Linked Data can be found in [this list](https://github.com/midi-ld/sources). As for the algorithm, we use [**midi2rdf**](https://github.com/midi-ld/midi2rdf), a Python-based MIDI to RDF converter that uses a simple MIDI ontology to represent MIDI events. The product of applying this algorithm to the source MIDI files is what we call the *MIDI Linked Data Cloud*, which currently contains 10,215,557,355 RDF statements about 308,443 MIDI files.

MIDI files are then represented as RDF graphs which get connected by two different kinds of resources: *notes* and *programs*. Notes are simply the pitches that the MIDI format can represent, all listed in the small [MIDI notes](purl.org/midi-ld/notes/) dataset. Programs are all the instruments (grand piano, electric guitar, etc.) that can play in a MIDI file; they are also listed in a similar [MIDI programs](purl.org/midi-ld/programs/) dataset. These instruments contain links to their equivalent DBpedia resources, so all songs in the MIDI Linked Data Cloud are connected to the rest of the Web via these links.

### Using the data
<a name="usage"></a>

The data in the MIDI Linked Data Cloud can be accessed in different ways:

- Downloading a data [dump](http://midi-ld.amp.ops.labs.vu.nl/) (beware these are large files!)
- Sending queries in the [SPARQL query language](https://en.wikipedia.org/wiki/SPARQL) to our [SPARQL endpoint](http://virtuoso-midi.amp.ops.labs.vu.nl/sparql)
- Using the MIDI-LD [RESTful API](http://grlc.io/api/midi-ld/queries/)

#### Dumps

Dumps are just big compressed files with the whole contents of the MIDI Linked Data Cloud. All releases are available at the [archive](http://midi-ld.amp.ops.labs.vu.nl/). The symbolic link [latest/](http://midi-ld.amp.ops.labs.vu.nl/latest) always points to the most recent version. The documentation (tutorials, etc.) always refers to this version. All releases contain a change log (the latest is also pasted [below](#changelog))

#### SPARQL

SPARQL is the query language of Linked Data. If you're comfortable with it, you'll be able to combine bits from different MIDI sources and do real awesomeness. If you can't speak SPARQL, you can get started [here](https://www.w3.org/TR/rdf-sparql-query/) or, even better, reading Bob DuCharme's excellent [Learning SPARQL](http://www.learningsparql.com/).

Useful queries for exploring and getting data out of the dataset are available in the [query repository](https://github.com/midi-ld/queries). You can just copy paste them and try them out in our [SPARQL endpoint](http://virtuoso-midi.amp.ops.labs.vu.nl/sparql).

When querying the SPARQL endpiont, you'll find this diagram useful:

<img src='img/midi-ld.png' style='vertical-align: center;'>

A complete walkthrough, with lots of example SPARQL queries, is available in [this tutorial](tutorials/sparql.md)

#### API

We use the convenient system [grlc](http://grlc.io/) to generate a [RESTful API](http://grlc.io/api/midi-ld/queries) on top of the SPARQL queries in the [query repository](https://github.com/midi-ld/queries). You'll find this useful if you have a consuming application that needs to access data in the MIDI Linked Data Cloud, but you don't want to couple with SPARQL libraries (take a look at [grlc's repo](https://github.com/clariah/grlc/) for more info).

### Tutorials
<a name="tutorials"></a>

These tutorials (and more to come) will help you get your way around MIDI Linked Data.

- [Tutorial](tutorials/search.md) on how to search and browse the MIDI Linked Data Cloud
- Tutorial on how to use [midi2rdf](https://github.com/midi-ld/midi2rdf) to convert your MIDI files to RDF (and back to MIDI)
- Tutorial on streaming your analog instrument output as MIDI Linked Data
- [Tutorial](tutorials/sparql.md) on querying the MIDI Linked Data Cloud using SPARQL

### Changelog

**Version 1.2**

- Added the MySongBook and Lakh datasets (see https://github.com/midi-ld/sources), increasing the piece count to 308,442
- Changed mid:Pattern into midi:Piece
- Bug fixes, documentation update

**Version 1.1**

- Added processing of CopyrightMetaEvent, MarkerEvent
- Joined together LyricsEvent strings into one single mid:lyrics label (for MIDI patterns that have them: 8,391 out of 113 565)
- Updated separate file for all sorts of labels/strings in the dataset (TrackNameEvent, TextMetaEvent, LyricsEvent, CopyrightMetaEvent, MarkerEvent)
- Bug fixes
- Updated documentation

**Version 1.0**

- Added processing of LyricsEvent
- Added links to vocabulary concepts Note and Program (see https://raw.githubusercontent.com/MIDIpedia/midi2rdf/master/midi-ld.ttl)
- Minor fixes

### FAQ
<a name="faq"></a>

- **Q.** Do you support other formats than MIDI?
- **A.** This work is specifically tailored for MIDI. If you're looking for similar projects on other formats, like MusicXML, look [here](http://www.ontologydesignpatterns.org/ont/musicml/musicml.owl) and [here](http://www.ontologydesignpatterns.org/ont/musicml/confirmation.ttl).
- **Q.** Help! My queries have stopped working.
- **A.** Although we're working with a reasonably stable data model, we're constantly fixing bugs and generating new and improved versions of the MIDI Linked Data Cloud dataset. This has (and will) marginally affect this data model, eventually changing some URI naming schemes (e.g. "patterns" will soon be renamed into "pieces"; blame the musicologists!). We'll keep the documentation consistent with the most up-to-date version of the dataset, so please refer to it if something doesn't work (and, of course, your [bug reports](https://github.com/midi-ld/documentation/issues) will be very welcome!)
- **Q.** Help! URIs that use to work don't anymore.
- **A.** See previous answer.
- **Q.** I want to communicate something that doesn't fit your issue trackers.
- **A.** Drop us an [email](mailto:albert.meronyo@gmail.com)!
