# Discussion document moving to NAF version 4

This document contains proposals to be discussed to update NAF version 3 to version 4.

### Tokens and terms
NAF currently does not have subtoken representation.
Having this would facilitate representating compounds.

While working on compounds and multi-word expressions,
this became an active discussion point.

An important task to aid in deciding how to deal with this is to inspect which modules make use of which layer.

|  System | Input layer(s)  | Output layer(s)  | Actively used in  |
|---|---|---|---|
|  ixa-pipe-tok | raw text  | text   |   | 
| ixa-pipe-pos  | text  | terms   |   |
| ixa-pipe-parse  | terms   | constituency   |   |
| ixa-pipe-srl | terms | deps | |
| fbk-timepro | terms - entities - constituency | timeExpressions | | 
| ixa-pipe-nerc | terms | entities | |
| it_makes_sense_WSD | terms | terms | |
| wsd-ukb | terms | terms | |
| ixa-pipe-ned | entities | entities | |
| POCUS | entities | entities | |
| ixa-pipe-wikify | text - terms | markables | |
| corefgraph | term - entities  constituency | coreferences | |
| ixa-pipe-srl | terms - deps | srl | |
| vua-eventcoreference | srl | coreferences | |
| fbk-temprel | text(?) - terms - entities  constituency - deps - srl - coreference - timeExpressions | temporalRelations | |
| fbk-causalrel | terms - entities - srl - coreferences - timeExpressions - temporalRelations | causalRelations | |
| VUA-perspective-factuality | text - terms - dependency - srl  coreferences | factualitylayer | |
| opinion-miner | text - terms - entities - dependencies - constituency | opinions |
| ixa-pipe-topic | text | topics | |

### Multi-words

NAF makes a distinction between components and terms.
A proposal is to represent terms that contain multiple tokens, e.g, phrasal verbs and idioms,
in the following way:

```xml
    <term id="t2" lemma="give" pos="VERB" phrase_type="component">
      <span>
        <target id="w2"/>
      </span>
    </term>
    <term id="t3" lemma="up" pos="PART" phrase_type="component">
      <span>
        <target id="w3"/>
      </span>
    </term>
    <term id="t4" lemma="give_up" pos="V" phrase_type="multi_word">
        <component>
            <target id="t2"/>
            <target id="t3"/>
        </component>
    </term>
```

### Compound terms
Section 8.4 of the [NAF specifications version 3](https://github.com/newsreader/NAF/blob/master/naf.pdf)
describe how to represent compound terms.
* In the example, would *w7* and *w8* refer to components of a compound?
* How to deal with [tussen-s](https://nl.wiktionary.org/wiki/tussen-s) or other vowels or consonants used to bind the components?

### Annotation process identifiers in header
* begin and end time
* name
* version
* use identifiers for annotations with time stamp

### part of speech
The current part of speech tagset is:
* N common noun
* R proper noun
* G adjective
* V verb
* P preposition
* A adverb
* C conjunction
* D determiner
* O other

A proposal is to move to [Universal Dependencies](https://universaldependencies.org/) since it is
more widely accepted and hopefully would limit mapping to this tagset.

Even though the DTD will validate if you use another tagset, I propose that we update the DTD regardless.

### IRIs and name spaces for identifiers
The goal is to make sure we use IRIs as much as possible.

### Multiple layers of the same layer
NAF currently does not allow to contain multiple layers of the same information layer,
e.g., multiple text, terms, entities, srl, etc. layers.

### Representation discourse units
There is a desire to be able to represent **headers**, **paragraphs**, etc.
One proposal is to represent ranges of term ids that represent a particular discourse unit, e.g.,
{t1, t2, t3, t4} represent the title.

### Hidden characters
Text can contains hidden characters such as newlines, tabs, etc.
It is possible to use ![CDATA[TOKEN]] surrounding the token but other modules might break when we allow hidden characters.
Is there a good convention how to deal with hidden characters?
