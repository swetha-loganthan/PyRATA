
Project development
****************************

Roadmap
============

Last report
-----------


TODO list (almost following a decreasing priority order)
-------------------------------

* docker 
import nltk
  >>> nltk.download('punkt')
python3 pyrata_re.py 'pos="DT"? pos~"JJ|NN"* pos~"NN.?"+' "[{}]" --pyrata_data --draw 

* dev 'import pyrata.re as pyrata_re' fails if graph-tool dependencies are not satisfied on the system. The motivation is to avoid to fail and to allow to use the engine even if graph-tool is not installed. So two alternatives: either move graph-tool references out from pyrata/compiled_pattern.py and pyrata/nfa.py, make pyrata_re.py use this reference. graphtool is well integrated so this may not be easy. Or use of the DEBUG var, like in the __main__ declaration in top of the file like in guiguan_nfa/regex_matching.py implementation. Implemented. To be test.  

* upload the last version on pupy (engine, wo comments), possibly wo pdf 

* temporarly comment spacy in do_benchmark.py because of bug ; solved ?
* ilimp implement extend the data structure with 'ilimp_hyp' = True for the annotation use case, externalize rules definition, and define rules...
* use case http://infolingu.univ-mlv.fr/ opinons et sentiments (DoXa),
* code optimization : look at https://docs.python.org/3/library/multiprocessing.html
* communication demo for sentiment analysis https://github.com/cjhutto/vaderSentiment ; http://alt.qcri.org/semeval2014/task4/index.php?id=data-and-tools ; http://alt.qcri.org/semeval2015/task12/ http://ai.stanford.edu/~amaas/data/sentiment/ ; test_movie_reviews () compare by using the same data set as streamhacker (see the sentiment-analysis.py code) ; evaluate vader on this corpus ; precompile the patterns ; build only full sentence pattern to demonstrate the forms "... but ...", "despite..., ....", "..., ... and ..." ... ; currently implementing booster extraction (should imagine how to assign them a weight by considering if negate is present and the current label), reuse them for extraction (if occurs twice)

* doc revise user guide to present the Match object 
* doc add description of the 'match' method in the user-guide
* doc in edit methods, annotation and replacement are not clear. Substitute annotation with data. Note sure
* doc give signature of matching methods in similar abstract way that edit methods
* api/engine implement operation merge_tokens_matching_a_pattern e.g. raw=":" raw=")" -> raw=":)" and by default get the features of the first token minus some overwritten. 
* pyrata_re implement 'draw_steps' as regex_matching_py3: "match string against pattern and draw the internal NFA at every step to  'NFA.pdf' (requires graph_tool). It is best to run this option and observe the result with a PDF viewer that can detect file change and reload the changed file. For example, the Skim app (http://skim-app.sourceforge.net)"

* how to launch a beta test campaign?
* quality test real use cases (search lexicon made of multi words, lowercase... e.g. [raw@"POSITIVE" & raw~"^[A-Z]"])
* communication demo for noun-phrase extraction http://slanglab.cs.umass.edu/phrasemachine/ ; see chunk/ne evaluation http://www.nltk.org/book/ch07.html ; see https://github.com/boudinfl/pke and https://github.com/boudinfl/semeval-2010-pre for benchmark ; http://www.aclweb.org/anthology/S10-1004
* communication demo for clause http://www.nltk.org/book/ch07.html, verb-phrase (cf. handel diaporama)

* ihm pyrata_re add more features such as input-file lexicon, lexicon-file, output-dfa, output-group, 
* communication organize the documentation http://www.sphinx-doc.org/en/stable/ ; publish on github pages
* communication developer write code explanation: in particular Running an NFA ...
* communication developer make diagrams to explain process and relations between files
* communication - user doc - illustrates the use of groups(), explains the resulting output Match, Matchlist, insert pdf of NFA when presenting CompiledPattern 
* quality test pyrata2conll http://www.nltk.org/book/ch07.html
* quality - do_benchmark.py - evaluate performance time `[pos="NNS" | pos="NNP"]`, `pos~"NN[SP]"` and 'pos~"(NNS|NNP)"', more fined grained comparison with alternatives
* quality revise logging information
* quality test - anchors wi each matching methods
* quality test - if lexicon argument kwargs is well handled in re compile is it necessary?
* quality code - refactor nfa.py to dissociate the pattern compilation (nfa build) from the data parsing (nfa run)
* quality code - refactor nfa.py to merge re search method with finditer/findall 
* quality test - systematize the tests (3 re methods + DFA in greedy/reluctant mode with aa .a a?a a*a a+a in caaaad (then aaaa/aabaa/caabaad) then the same with quantifier on the second char ; done in the first data configuration ; also consider ab a?b a*b a+b (and quantifier on last char) in cababd/abab ; some tests are already existing
* quality test - complex regex as value
* quality test - patterns error catching
* quality test - the chunk operator
* quality test - re methods on Compiled regular expression objects 
* api/engine - fix - explore the following behavior       
      >>> data = [{'raw': 'It', 'pos': 'PRP'}, {'raw': 'is', 'pos': 'VBZ'}, {'raw': 'fast', 'pos': 'JJ'}, {'raw': 'easy', 'pos': 'JJ'}, {'raw': 'and', 'pos': 'CC'}, {'raw': 'funny', 'pos': 'JJ'}, {'raw': 'to', 'pos': 'TO'}, {'raw': 'write', 'pos': 'VB'}, {'raw': 'regular', 'pos': 'JJ'}, {'raw': 'expressions', 'pos': 'NNS'}, {'raw': 'with', 'pos': 'IN'}, {'raw': 'PyRATA', 'pos': 'NNP'}]
      >>> pyrata_re.search('(pos="JJ" | (pos="JJ" pos="NNS") )', data)
      <pyrata.re Match object; groups=[[[{'raw': 'fast', 'pos': 'JJ'}], 2, 3], [[{'raw': 'fast', 'pos': 'JJ'}], 2, 3], [[{'raw': 'fast', 'pos': 'JJ'}], 2, 3]]>
      >>> pyrata_re.search('(pos="JJ" | (pos="JJ" pos="NNS") )', data)
      <pyrata.re Match object; groups=[[[{'raw': 'fast', 'pos': 'JJ'}], 2, 3], [[{'raw': 'fast', 'pos': 'JJ'}], 2, 3]]>
* api/engine - fix - the NFA _repr_ because it does not display all the states...
* api/engine - fix/revise code - In __step_special_state (i.e. when running a NFA), I add various fix since id(cs)={} was absent from NFA.states_dict. Should have be added during NFA build ! We store now.'.format(state.id)) ; Revise the code to find where to place the storing code during the build
* api/engine - apart from String, allow the processing of primitive types such as Boolean and Integer 
  python3 pyrata_re.py 'int="1"' "[{'int':1, 'str':'un', 'bool':True}]"
  python3 pyrata_re.py 'bool="True"' "[{'int':1, 'str':'un', 'bool':True}]"
* api/engine - revise - by default only the zero group is compared with eq and ne ; should be all the groups ?
* api/engine - implement methods to save, load and run previously saved DFA
* api/engine - implement draw option in main API to generate drawing when compiling
* api/engine - implement split, sub... in compiled_pattern_re module
* api/engine - implement insert, delete (sub with [] ; check), insert-to-the-leftmost (~ sub with reference)... 
* api/engine - implement "possessive matching" mode
* api/engine - implement re : see the pattern search module and its facilities
* quality code revise the __main__ section of each py
* api/engine negation of groups/alternatives is not possible ; a step is possible by the concept of class
* grammar - double quote in constraint value ; the parse is not effective or at least state.symbolic_step_expression is never initalized I guessed it was because the parser ends at the quote inside the value... indeed raw=""" is the parser input... ;  pb seems to come to the lexer "t_VALUE";  if a " occurs when in_constraint_value is true and when the previous char is \ then do not aso do not change the value of in_constraint_value ; switching off the reluctant mode i.e. from t_VALUE = r'\"([^\\\n]|(\\.))*?\"' to  t_VALUE = r'\"((\\\")|[^\\\n]|(\\.))*\"' makes it work... at which price ? constraint combination does not work anymore. So I commented the "double quote" tests  
* grammar think of an alternative as re implementation of the chunk operator in the grammar.
* grammar implement predefined quantifiers {n} Match exactly n times; {n,} Match at least n times; {n,m} Match at least n but not more than m times
* grammar implement backreference group reference so they can be matched later in the data with the \number special sequence
* grammar allow grammar with multiple rules (each rule should have an identifier... and its own groupindex)
* grammar move the python methods as grammar components
* grammar think about the context notion 
* api/engine performance - parallelize NFA running, implementation cython ?
* api/engine implement lex.lex(reflags=re.UNICODE)
* quality code : do a better error event handling (here the data is not in the right format)
>>> pyrata_re.search('a="A"',{'a':'A'})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python3.5/dist-packages/pyrata/re.py", line 78, in search
    r = compiled_nfa.search(data, **kwargs)  # greedy = True
  File "/usr/local/lib/python3.5/dist-packages/pyrata/nfa.py", line 579, in search
    c = s[j]
KeyError: 0
>>> pyrata_re.search('a="A"',[{'a':'A'}])
<pyrata.re Match object; groups=[[[{'a': 'A'}], 0, 1]]>



