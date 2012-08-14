---
layout: post
category: nonsense
tags: [ nlp ]
---
{% include JB/setup %}

A bit of background information on the sort of text processing gertrude does behind the scenes.

## History of gertrude's infobot capabilities

Way back when she was a custom perl script, gertrude used to collect any in-channel utterances that contained the
words 'is' and 'are' and stash them away in her database of 'factoids' which she could subsequently search when
queried.

It turned out however that there is an *awful lot* of day to day crap containing the words 'is' and 'are' that
are entirely unsuited for use as factoids, and which are largely information free.

We tried to get around this by applying an ever-growing chain of regular expressions to candidate strings, in
an attempt to weed out the worst offenders, but this is only useful if someone is continually monitoring the
output and adding new regexes to filter out newly discovered unwanted patterns. Additionally, some candidates
cannot be ruled out on purely syntactical grounds, but require some semantic understanding: for example, the
phrase "the one that is going to the party" is untuitively *not* a useful factoid, but it is difficult to craft
a regular expression to catch it, without falsely rejecting other potentially useful strings.

When we ported gertrude to infobot, we discovered the infobot guys had run into the same problem, and had come
up with the same solution, but they had a vastly better set of regex rules than we did, so we adopted those.

During her period as a mozbot and an rbot, gertrude's regex rules were essentially a direct port of those she
had in her infobot days.

## Let's see if we can do better than that

The sixth generation of gertrude, based on the ruby IRC library cinch, has a ground up rewrite of the infobot
functionality. gertrude started with a database of just over 19,000 factoids, and probably 10% of that is
useful information. The rest is chaff that has slipped through the regexes, or just random historical rubbish
that predates our regex parsing.

Our goal was to significantly prune gertrude's database of factoids using some Natural Language Processing.
Hopefully the same systems which we deploy to winnow the factoid database can also be applied in real-time
when gertrude is attempting to passively learn new factoids from her channel, and can also help in information
retrieval.

### Databases

Gertrude originally maintained separate 'is' and 'are' factoid databases in Berkeley DBM format. During the
mozbot era, these were shifted to MySQL databases, then during the RBot era they were combined into a single
'factoid' SQLite database. For her latest incarnation, gertrude is using Mongo, a fast SQL-less database.

### Weeding out the essay writers

At various points in her life, gertrude has been taught factoids with very long-winded explanations. For example,
her entry on CSS is:

> css is Cascading Style Sheets. It"s a way of defining styles for your HTML elements 
> separately from the content. or at http://www.w3.org/Style/CSS or cross-site scripting

There are a couple of problems here: the entry is very long-winded, and it contains a lot of 'or' clauses where
gertrude has been taught different parallel meanings - it is unfortunate that the original infobot code stores
alternatives in this way, but there it is.

Our approach is fairly brutal: we segment all the factoid descriptions into sentences using [Punkt][], 
and for those factoids which have more than one sentence in their description, we keep the first sentence 
that contains the words "is" or "are", and disregard the rest. 

Additionally we have to compensate for the subject of a factoid having multiple sentences, as in:

> bah. fucking nasdaq =is=> down

At the same time we can strip out low hanging fruit like descriptions that end with a '?', or subjects that
consist of only a pronoun like 'he', 'what' etc.

All together, this results in approximately 2,000 factoids being altered and another 1,800 being dropped.

## It's all a question of semantics

That's about as far as we can go using simple regexes, to weed out more of the rubbish we're going to have
to understand the *semantics* of our factoids. To do this we're going to every word with its relevant
*part of speech*, using the [Brill][] tagger.

This tells us which words are verbs, which are nouns etc. However, this is not sufficient information to
decide the fate of factoids, because the subject of the factoid is often not just a simple noun, e.g.

> the computer that i bought =is=> awesome

> running with scissors =is=> inadvisable

Here the *subject* of our *head phease verb* (is/are) is a *noun phrase*, not just a simple noun. Splitting
the sentence into its *constituents* (noun phrase, verb phrase) etc is more of an involved process; we could
use a relatively simple noun-phrase chunker, but we've decided to use a full parser, [Enju][], which is a
Head-driven Phrase Structure Grammar. What this means is it builds a tree structure from our sentences and
marks it up with syntactic and semantic information, which we can use to further exclude bogus factoids.

First off, we'll discard any sentences where the main head verb isn't *is* or *are*, as that's kinda the
whole point of the factoid database. This lets us weed out nonsense like: 

> quixy, you life is what you say, he is a guy that won"t get no love from sl.

Our next level of exclusion will exclude any subject phrases of the main head verb which are not 
*noun phrases*.


[Punkt]: http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.85.5017&rep=rep1&type=pdf
[Brill]: http://acl.ldc.upenn.edu/H/H92/H92-1022.pdf
[Enju]: http://www.nactem.ac.uk/enju/
