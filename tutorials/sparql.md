# MIDI Linked Data SPARQL tutorial

Hello! I'm an SPARQL tutorial for querying data (beyond basic text search) in the MIDI Linked Data Cloud. I'm assuming you have some basic experience with using W3C's [SPARQL query language](https://www.w3.org/TR/rdf-sparql-query/).

To query the data, you'll find this diagram useful:

<img src='../img/midi-ld.png' style='vertical-align: center;'>

Take into account that this diagram represents the upcoming version of the dataset. There are two important changes with respect to the version available at the endpoint: `midi:MIDIFile` instances are not yet available; and `midi:Piece` is currently named `midi:Pattern`. For now, you can safely ignore the former (it will only add one more join operation when querying for filenames, for example) and read the latter as if the diagram said `midi:Pattern` instead of `midi:Piece`.


## Querying for text

As shown in the [search and browsing tutorial](tutorial.md), the most straightforward way of querying the MIDI Linked Data Cloud is to choose some text property where to start, like things you eventually find in *file names* (band names or song titles, but also genra or collection names).

1. File name search. Query 1 below shows how to search for MIDI data using a text query over their original *filenames* (in this case, filenames containing the words "war pigs").
2. Lyrics search. Query 2 below shows how to search for MIDI data using a text query over their original *lyrics* (in this case, lyrics mentioning the word "stairway").


## Querying piece data

Pieces (patterns) have interesting information to query on.

1. All piece data (format, resolution, lyrics, etc.) grouped by the specific MIDI piece can be retrieved with Query 3.
2. We can also force the pattern to query only for pieces that have certain properties. For instance, query 4 looks for songs with lyrics only.
3. Similarly, query 5 finds MIDI pieces with songs that do *not* contain any lyrics data.
4. Filtering over these properties is easy. Query 6 shows how to query specifically for MIDI pieces of format 1 with a resolution higher than 400.
5. All tracks in a piece are linked from it. For example, query 7 shows how to get the count of tracks per piece, and query 8 shows how to get them listed.

**Important**. In general it's a good idea to use the filtering patterns in these previous examples in all the following queries, and whenever you query the SPARQL endpoint. Especially, when you query for track/event data. If you do so without constraining the search, the endpoint could take a *long* time to give the results back ot you (or fail to do so after a long waiting time). You'll also make it friendlier for others using these resources. Thanks!


## Querying track data

Tracks contain the core of MIDI: events. The following examples show how to get different data about events contained in tracks (which are contained in pieces).

1. Query 9 shows all the different types of events in the dataset.
2. Query 10 shows how to query specific properties of concrete event types, such as the note that is being played in a *NoteOn* event. These are the notes that your computer plays when you play the MIDI file!

## Queries

**Query 1.** Filtering MIDI file names.
<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT ?filename ?pattern WHERE {
  ?pattern prov:wasDerivedFrom ?filename .
  FILTER (regex(?filename, "war pigs", "i"))
}
</pre>


**Query 2.** Filtering lyrics.

<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT ?filename ?pattern WHERE {
  ?pattern prov:wasDerivedFrom ?filename .
  ?pattern midi:lyrics ?lyrics .
  FILTER (regex(?lyrics, "stairway", "i"))
}
</pre>

**Query 3.** Querying (all) piece properties (except tracks).

<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT * WHERE {
  ?pattern a midi:Pattern ;
           midi:format ?format ;
           midi:resolution ?resolution .
  OPTIONAL { ?pattern midi:lyrics ?lyrics . }
} GROUP BY ?pattern
LIMIT 1000
</pre>

**Query 4.** Querying pieces that only contain lyrics.

<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT * WHERE {
  ?pattern a midi:Pattern ;
  ?pattern midi:lyrics ?lyrics .
} GROUP BY ?pattern
LIMIT 1000
</pre>

**Query 5.** Querying pieces that do not contain lyrics.

<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT * WHERE {
  ?pattern a midi:Pattern .
  FILTER NOT EXISTS { ?pattern midi:lyrics ?lyrics . }
}
LIMIT 1000
</pre>

**Query 6.** Querying only format 1, higher than 400 resolution pieces.

<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT * WHERE {
  ?pattern a midi:Pattern .
  ?pattern midi:format 1 .
  ?pattern midi:resolution ?resolution .
  FILTER (?resolution > 400)
} GROUP BY ?pattern
LIMIT 1000
</pre>

**Query 7.** Tracks per MIDI piece.
<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT * WHERE {
  ?pattern a midi:Pattern .
  ?pattern midi:hasTrack ?track .
} GROUP BY ?pattern
LIMIT 1000
</pre>

**Query 8.** Track count per MIDI piece.
<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT ?pattern COUNT(?track) WHERE {
  ?pattern a midi:Pattern .
  ?pattern midi:hasTrack ?track .
} GROUP BY ?pattern
LIMIT 1000
</pre>

**Query 9.** MIDI event types in the dataset.
<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT DISTINCT(?type) WHERE {
  ?pattern midi:hasTrack ?track .
  ?track midi:hasEvent ?event .
  ?event a ?type .
}
</pre>

**Query 10.** MIDI Note On events and their notes.
<pre>
PREFIX prov: &lt;http://www.w3.org/ns/prov#&gt;
PREFIX midi: &lt;http://purl.org/midi-ld/midi#&gt;

SELECT ?pattern ?note WHERE {
  ?pattern midi:hasTrack ?track .
  ?track midi:hasEvent ?event .
  ?event a midi:NoteOnEvent .
  ?event midi:note ?note .
} GROUP BY ?event ?note
</pre>
