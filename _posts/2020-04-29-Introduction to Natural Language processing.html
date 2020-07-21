---
title: "Introduction to Natural Language processing"
date: 2020-04-29T15:34:30-04:00
categories:
  - blog
tags:
  - NLP
  - AI
#image: .jpg
---



<p>This post documents my learning of lesson NLP, as part of nanodegree: AI in Algorithmic trading.</p>

<!-- wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4><strong>Backus-Naur Form</strong></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As part of learning a programming language, we learn the its grammar. For example, consider the following line of code in python:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">print("Hello World")</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It has grammar/ rules. The rules are that strings should be within quotes and if we want to print a statement, we should enclose them in parenthesis (Python 3). </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Backus-Naur invented the form of writing the language rules precisely, and takes the following form:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>&lt;non-terminal&gt; -&gt; replacement</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>&lt;non-terminal&gt; represents something that's unfinished, which can be replaced by a set of non-terminals or terminals. Using the rule, we keep replacing the non-terminals with replacements until we are only left with only terminals.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The languages we use have similar grammatical rules. While humans can understand each other even if the language strays from the correct grammatical rules, computers cannot. It can be circumvented by: ?</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Process words and phrases to identify keywords, Parts of Speech, named entities and quantities, among others</li><li> Extract relevant parts based on the above process</li><li>Analyze overall document</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4>Counting number of words in a text</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Consider the following text:</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>As I was waiting, a man came out of a side room, and at a glance I was sure he must be Long John. His left leg was cut off close by the hip, and under the left shoulder he carried a crutch, which he managed with wonderful dexterity, hopping about upon it like a bird. He was very tall and strong, with a face as big as a ham—plain and pale, but intelligent and smiling. Indeed, he seemed in the most cheerful spirits, whistling as he moved about among the tables, with a merry word or a slap on the shoulder for the more favoured of his guests.</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>To count the number of words, we first split the words.</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">text.split(' ')</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">['as',  'i',  'was',  'waiting,',  'a',  'man',  'came',  'out',  'of',  'a',  'side',  'room,',  'and',  'at',  'a',  'glance',  'i',  'was',  'sure',  'he',  'must',  'be',  'long',  'john.',  'his',  'left',  'leg',  'was',  'cut',  'off',  'close',  'by',  'the',  'hip,',  'and',  'under',  'the',  'left',  'shoulder',  'he',  'carried',  'a',  'crutch,',  'which',  'he',  'managed',  'with',  'wonderful',  'dexterity,',  'hopping',  'about',  'upon',  'it',  'like',  'a',  'bird.',  'he',  'was',  'very',  'tall',  'and',  'strong,',  'with',  'a',  'face',  'as',  'big',  'as',  'a', <strong> 'ham—plain'</strong>,  'and',  'pale,',  'but',  'intelligent',  'and',  'smiling.',  'indeed,',  'he',  'seemed',  'in',  'the',  'most',  'cheerful',  'spirits,',  'whistling',  'as',  'he',  'moved',  'about',  'among',  'the',  'tables,',  'with',  'a',  'merry',  'word',  'or',  'a',  'slap',  'on',  'the',  'shoulder',  'for',  'the',  'more',  'favoured',  'of',  'his',  'guests.']</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As we can see from the given text, simply splitting the words by white space would not be effective; ham and plain in line 4 are to be considered as separate words, whereas, splitting them by white space would not separate them. To split words, we will use regex:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">re.findall(r'\w+',text)</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:group -->
<div class="wp-block-group"><div class="wp-block-group__inner-container"></div></div>
<!-- /wp:group -->

<!-- wp:group -->
<div class="wp-block-group"><div class="wp-block-group__inner-container"><!-- wp:paragraph {"fontSize":"small"} -->
<p class="has-small-font-size">['as',  'i',  'was',  'waiting',  '',  'a',  'man',  'came',  'out',  'of',  'a',  'side',  'room',  '',  'and',  'at',  'a',  'glance',  'i',  'was',  'sure',  'he',  'must',  'be',  'long',  'john',  '',  'his',  'left',  'leg',  'was',  'cut',  'off',  'close',  'by',  'the',  'hip',  '',  'and',  'under',  'the',  'left',  'shoulder',  'he',  'carried',  'a',  'crutch',  '',  'which',  'he',  'managed',  'with',  'wonderful',  'dexterity',  '',  'hopping',  'about',  'upon',  'it',  'like',  'a',  'bird',  '',  'he',  'was',  'very',  'tall',  'and',  'strong',  '',  'with',  'a',  'face',  'as',  'big',  'as',  'a', <strong> 'ham',  'plain'</strong>,  'and',  'pale',  '',  'but',  'intelligent',  'and',  'smiling',  '',  'indeed',  '',  'he',  'seemed',  'in',  'the',  'most',  'cheerful',  'spirits',  '',  'whistling',  'as',  'he',  'moved',  'about',  'among',  'the',  'tables',  '',  'with',  'a',  'merry',  'word',  'or',  'a',  'slap',  'on',  'the',  'shoulder',  'for',  'the',  'more',  'favoured',  'of',  'his',  'guests',  '']</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>-- works as intended now. <code>re.findall('[a-zA-Z0-9_]+',text)</code> works the same way.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We can count occurrence of each word in a for loop and store it in a dictionary...</p>
<!-- /wp:paragraph -->

<!-- wp:group -->
<div class="wp-block-group"><div class="wp-block-group__inner-container"><!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">counts = {}
words = re.findall(r'\w+',t)
for word in words:
    if word in counts:
        counts[word] = counts[word]+1
    else:
        counts[word] = 1    
        </pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>… and sort them in descending order</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">sorted(counts.items(),key=lambda count:count[1],reverse=True)</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>Finally, we get the most and least common words:</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code {"language":"python"} -->
<pre class="wp-block-syntaxhighlighter-code">#Top 10 common words:
sorted_counts[:10]
#Top 10 least common words:
sorted_counts[-10:]</pre>
<!-- /wp:syntaxhighlighter/code -->
