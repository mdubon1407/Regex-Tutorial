# Title (Regex Tutorial)

Introductory paragraph (replace this with your text)

## Summary

Regular expressions (RegEx) are patterns used to match character combinations in strings. In JavaScript, regular expressions are also objects. These patterns are used with the exec() and test() methods of RegExp, and with the match(), matchAll(), replace(), replaceAll(), search(), and split() methods of String. This chapter describes JavaScript regular expressions.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components
Matching Fixed Strings
Matching Special Characters
Matching Character Sets
Specifying Groups and Fields
Evaluating Occurrences
Specifying Location
Specifying Alternatives
Matching Information from a Table
Capturing Multiple Lines
Managing the Scope of Analysis

### Anchors
Anchors are a different breed. They do not match any character at all. Instead, they match a position before, after, or between characters. They can be used to “anchor” the regex match at a certain position. The caret ^ matches the position before the first character in the string. Applying ^a to abc matches a. ^b does not match abc at all, because the b cannot be matched right after the start of the string, matched by ^. See below for the inside view of the regex engine.

Similarly, $ matches right after the last character in the string. c$ matches c in abc, while a$ does not match at all.

A regex that consists solely of an anchor can only find zero-length matches. This can be useful, but can also create complications that are explained near the end of this tutorial.

### Quantifiers
Exact count {n}
A number in curly braces {n}is the simplest quantifier. When you append it to a character or character class, it specifies how many characters or character classes you want to match.
For example, the regular expression /\d{4}/ matches a four-digit number. It is the same as /\d\d\d\d/:

The following table lists the quantifiers:

Quantifier	Description
*	Match zero or more times.
+	Match one or more times.
?	Match zero or one time.
{ n }	Match exactly n times.
{ n ,}	Match at least n times.
{ n , m }	Match from n to m times.
### OR Operator
 You can combine regular expressions with the following characters,
called "regular expression operators", or "metacharacters", to increase
the power and versatility of regular expressions.
a(b|c)     matches a string that has a followed by b or c (and captures b or c) -> Try it!
a[bc]      same as previous, but without capturing b or c

### Character Classes
A character class is a special notation that matches any symbol from a certain set.
There exist following character classes:

That was a character class for digits. There are other character classes as well.

Most used are:

\d (“d” is from “digit”)
A digit: a character from 0 to 9.
\s (“s” is from “space”)
A space symbol: includes spaces, tabs \t, newlines \n and few other rare characters, such as \v, \f and \r.
\w (“w” is from “word”)
A “wordly” character: either a letter of Latin alphabet or a digit or an underscore _. Non-Latin letters (like cyrillic or hindi) do not belong to \w.
For instance, \d\s\w means a “digit” followed by a “space character” followed by a “wordly character”, such as 1 a.

We can search by these properties as well. That requires flag u, 
Inverse Classes
For every character class there exists an “inverse class”, denoted with the same letter, but uppercased.

The “inverse” means that it matches all other characters, for instance:

\D
Non-digit: any character except \d, for instance a letter.
\S
Non-space: any character except \s, for instance a letter.
\W
Non-wordly character: anything but \w, e.g a non-latin letter or a space
Summary:
\d – digits.
\D – non-digits.
\s – space symbols, tabs, newlines.
\S – all but \s.
\w – Latin letters, digits, underscore '_'.
\W – all but \w.
. – any character if with the regexp 's' flag, otherwise any except a newline \n.
…Furthermore!

Unicode encoding, used by JavaScript for strings, provides many properties for characters, like: which language the letter belongs to (if it’s a letter) it is it a punctuation sign, etc.

### Flags
We are learning how to construct a regex but forgetting a fundamental concept: flags.
A regex usually comes within this form /abc/, where the search pattern is delimited by two slash characters /. At the end we can specify a flag with these values (we can also combine them each other):
g (global) does not return after the first match, restarting the subsequent searches from the end of the previous match
m (multi-line) when enabled ^ and $ will match the start and end of a line, instead of the whole string
i (insensitive) makes the whole expression case-insensitive (for instance /aBc/i would match AbC)

### Grouping and Capturing
Regular expressions allow us to not just match text but also to extract information for further processing. This is done by defining groups of characters and capturing them using the special parentheses ( and ) metacharacters. Any subpattern inside a pair of parentheses will be captured as a group. In practice, this can be used to extract information like phone numbers or emails from all sorts of data.

Imagine for example that you had a command line tool to list all the image files you have in the cloud. You could then use a pattern such as ^(IMG\d+\.png)$ to capture and extract the full filename, but if you only wanted to capture the filename without the extension, you could use the pattern ^(IMG\d+)\.png$ which only captures the part before the period.
### Bracket Expressions
Square brackets ([ ]) designate a character class and match a single character in the string. Inside a character class, only the character class metacharacters (backslash, circumflex anchor and hyphen) have special meaning.
### Greedy and Lazy Match
Quantifiers are very simple from the first sight, but in fact they can be tricky.

We should understand how the search works very well if we plan to look for something more complex than /\d+/.

Let’s take the following task as an example.

We have a text and need to replace all quotes "..." with guillemet marks: «...». They are preferred for typography in many countries.

For instance: "Hello, world" should become «Hello, world». There exist other quotes, such as „Witam, świat!” (Polish) or 「你好，世界」 (Chinese), but for our task let’s choose «...».

The first thing to do is to locate quoted strings, and then we can replace them.

A regular expression like /".+"/g (a quote, then something, then the other quote) may seem like a good fit, but it isn’t!

Let’s try it:

let regexp = /".+"/g;

let str = 'a "witch" and her "broom" is one';

alert( str.match(regexp) ); // "witch" and her "broom"
### Boundaries
A word boundary \b is a test, just like ^ and $.

When the regexp engine (program module that implements searching for regexps) comes across \b, it checks that the position in the string is a word boundary.

There are three different positions that qualify as word boundaries:

At string start, if the first string character is a word character \w.
Between two characters in the string, where one is a word character \w and the other is not.
At string end, if the last string character is a word character \w.
For instance, regexp \bJava\b will be found in Hello, Java!, where Java is a standalone word, but not in Hello, JavaScript!.

alert( "Hello, Java!".match(/\bJava\b/) ); // Java
alert( "Hello, JavaScript!".match(/\bJava\b/) ); // null
In the string Hello, Java! following positions correspond to \b:


So, it matches the pattern \bHello\b, because:

At the beginning of the string matches the first test \b.
Then matches the word Hello.
Then the test \b matches again, as we’re between o and a comma.
The pattern \bHello\b would also match. But not \bHell\b (because there’s no word boundary after l) and not Java!\b (because the exclamation sign is not a wordly character \w, so there’s no word boundary after it).

alert( "Hello, Java!".match(/\bHello\b/) ); // Hello
alert( "Hello, Java!".match(/\bJava\b/) );  // Java
alert( "Hello, Java!".match(/\bHell\b/) );  // null (no match)
alert( "Hello, Java!".match(/\bJava!\b/) ); // null (no match)
We can use \b not only with words, but with digits as well.

For example, the pattern \b\d\d\b looks for standalone 2-digit numbers. In other words, it looks for 2-digit numbers that are surrounded by characters different from \w, such as spaces or punctuation (or text start/end).

alert( "1 23 456 78".match(/\b\d\d\b/g) ); // 23,78
alert( "12,34,56".match(/\b\d\d\b/g) ); // 12,34,56
Word boundary \b doesn’t work for non-latin alphabets
The word boundary test \b checks that there should be \w on the one side from the position and "not \w" – on the other side.

But \w means a latin letter a-z (or a digit or an underscore), so the test doesn’t work for other characters, e.g. cyrillic letters or hieroglyphs.
### Back-references
Backreferences match the same text as previously matched by a capturing group. Suppose you want to match a pair of opening and closing HTML tags, and the text in between. By putting the opening tag into a backreference, we can reuse the name of the tag for the closing tag. Here’s how: <([A-Z][A-Z0-9]*)\b[^>]*>.*?</\1>. This regex contains only one pair of parentheses, which capture the string matched by [A-Z][A-Z0-9]*. This is the opening HTML tag. (Since HTML tags are case insensitive, this regex requires case insensitive matching.) The backreference \1 (backslash one) references the first capturing group. \1 matches the exact same text that was matched by the first capturing group. The / before it is a literal character. It is simply the forward slash in the closing HTML tag that we are trying to match.

To figure out the number of a particular backreference, scan the regular expression from left to right. Count the opening parentheses of all the numbered capturing groups. The first parenthesis starts backreference number one, the second number two, etc. Skip parentheses that are part of other syntax such as non-capturing groups. This means that non-capturing parentheses have another benefit: you can insert them into a regular expression without changing the numbers assigned to the backreferences. This can be very useful when modifying a complex regular expression.

You can reuse the same backreference more than once. ([a-c])x\1x\1 matches axaxa, bxbxb and cxcxc.

Most regex flavors support up to 99 capturing groups and double-digit backreferences. So \99 is a valid backreference if your regex has 99 capturing groups.
### Look-ahead and Look-behind
Sometimes we need to find only those matches for a pattern that are followed or preceeded by another pattern.

There’s a special syntax for that, called “lookahead” and “lookbehind”, together referred to as “lookaround”.

For the start, let’s find the price from the string like 1 turkey costs 30€. That is: a number, followed by € sign.

Lookahead
The syntax is: X(?=Y), it means "look for X, but match only if followed by Y". There may be any pattern instead of X and Y.

For an integer number followed by €, the regexp will be \d+(?=€):

let str = "1 turkey costs 30€";

alert( str.match(/\d+(?=€)/) ); // 30, the number 1 is ignored, as it's not followed by €
Please note: the lookahead is merely a test, the contents of the parentheses (?=...) is not included in the result 30.

When we look for X(?=Y), the regular expression engine finds X and then checks if there’s Y immediately after it. If it’s not so, then the potential match is skipped, and the search continues.

More complex tests are possible, e.g. X(?=Y)(?=Z) means:

Find X.
Check if Y is immediately after X (skip if isn’t).
Check if Z is also immediately after X (skip if isn’t).
If both tests passed, then the X is a match, otherwise continue searching.
In other words, such pattern means that we’re looking for X followed by Y and Z at the same time.

That’s only possible if patterns Y and Z aren’t mutually exclusive.

For example, \d+(?=\s)(?=.*30) looks for \d+ only if it’s followed by a space, and there’s 30 somewhere after it:

let str = "1 turkey costs 30€";

alert( str.match(/\d+(?=\s)(?=.*30)/) ); // 1
In our string that exactly matches the number 1.

Negative lookahead
Let’s say that we want a quantity instead, not a price from the same string. That’s a number \d+, NOT followed by €.

For that, a negative lookahead can be applied.

The syntax is: X(?!Y), it means "search X, but only if not followed by Y".

let str = "2 turkeys cost 60€";

alert( str.match(/\d+(?!€)/) ); // 2 (the price is skipped)
Lookbehind
Lookahead allows to add a condition for “what follows”.

Lookbehind is similar, but it looks behind. That is, it allows to match a pattern only if there’s something before it.

The syntax is:

Positive lookbehind: (?<=Y)X, matches X, but only if there’s Y before it.
Negative lookbehind: (?<!Y)X, matches X, but only if there’s no Y before it.
For example, let’s change the price to US dollars. The dollar sign is usually before the number, so to look for $30 we’ll use (?<=\$)\d+ – an amount preceded by $:

let str = "1 turkey costs $30";

// the dollar sign is escaped \$
alert( str.match(/(?<=\$)\d+/) ); // 30 (skipped the sole number)
And, if we need the quantity – a number, not preceded by $, then we can use a negative lookbehind (?<!\$)\d+:

let str = "2 turkeys cost $60";

alert( str.match(/(?<!\$)\d+/) ); // 2 (skipped the price)
Capturing groups
Generally, the contents inside lookaround parentheses does not become a part of the result.

E.g. in the pattern \d+(?=€), the € sign doesn’t get captured as a part of the match. That’s natural: we look for a number \d+, while (?=€) is just a test that it should be followed by €.

But in some situations we might want to capture the lookaround expression as well, or a part of it. That’s possible. Just wrap that part into additional parentheses.

In the example below the currency sign (€|kr) is captured, along with the amount:

let str = "1 turkey costs 30€";
let regexp = /\d+(?=(€|kr))/; // extra parentheses around €|kr

alert( str.match(regexp) ); // 30, €
And here’s the same for lookbehind:

let str = "1 turkey costs $30";
let regexp = /(?<=(\$|£))\d+/;

alert( str.match(regexp) ); // 30, $
## Author

A short section about the author with a link to the author's GitHub profile (Mario Dubon )
