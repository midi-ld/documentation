# Searching and browsing the MIDI Linked Data cloud

Hello! I'm a tutorial for guiding you through searching and browsing data in the MIDI Linked Data Cloud. You'll just need the browser you're probably using to read this. Let's start!

## Search interface

We've used the fancyness out of [brwsr](https://github.com/data2semantics/brwsr), [grlc](https://github.com/clariah/grlc) and the might of Linked Data and HTML to let you query a whole lot of MIDI data in a simple way.

1. Go to [main website](https://midi-ld.github.io)
2. Scroll down to the "MIDI Linked Data lookup" input text
3. Type something to search for, like an artist/band name, or a song title (case insensitive). Try *war pigs*, *led zeppelin*, or *Queen*. Take into account the following: this simple interface uses the provenance over the original MIDI file names to provide you results (yes, we're working on it!).
4. You'll see a results page with a table made of two columns: *MIDIpiece*, and *wasDerivedFrom*. The latter is the full original filename from which we derived the MIDI Linked Data. The former is a URI representing the piece that matches your query as an RDF graph.
5. Click on the URI that you want to browse.
6. You'll see a descrpition of the properties for that MIDI URI. Typically this includes some metadata (for example, the original filename, or the song's lyrics), provenance information, MIDI data (for example, the resolution and format of the MIDI file); and links to **tracks**, which are the building blocks of MIDI.
7. Click on the URI of the track you want to browse.
8. This will basically list the **events** contained in that specific track. Events are musical activitis, like "start playing a note" (in MIDI jargon, a *NoteOn* event).
8. Click on the URI of the event you want to browse.
9. You'll see a list of all the relevant properties and links for that specific event. Note that different kinds of events contain different kinds of properties.

## SPARQL interface

If you have a text query and prefer to experiment with SPARQL, you can do the following:

1. Go to the [SPARQL endpoint](http://virtuoso-midi.amp.ops.labs.vu.nl/sparql) page
2. Replace the text in the second parameter of the `regex` function with some specific keyword, for instance *war pigs* or *samba* (genre information is sometimes part of filenames!). Query 1 below shows an example.
3. You can follow a similar strategy to edit Query 2 and search within *lyrics*. Query 2, for instance, will return all MIDI files with songs containing the word "stairway".
4. You can go on and filter text in any of the textual properties of MIDI files as Linked Data. Feel free to experiment and [let us know](https://github.com/midi-ld/documentation/issues)!


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
