---
layout: page
title: "JavaScript is_unicode function"
comments: true
sharing: true
footer: true
alias:
- /functions/view/is_unicode:593
- /functions/view/is_unicode
- /functions/view/593
- /functions/is_unicode:593
- /functions/593
---
<!-- Generated by Rakefile:build -->
A JavaScript equivalent of PHP's is_unicode

{% codeblock var/is_unicode.js lang:js https://raw.github.com/kvz/phpjs/master/functions/var/is_unicode.js raw on github %}
function is_unicode (vr) {
  // +   original by: Brett Zamir (http://brett-zamir.me)
  // %        note 1: Almost all strings in JavaScript should be Unicode
  // *     example 1: is_unicode('We the peoples of the United Nations...!');
  // *     returns 1: true
  if (typeof vr !== 'string') {
    return false;
  }

  // If surrogates occur outside of high-low pairs, then this is not Unicode
  var arr = [],
    any = '([\s\S])',
    highSurrogate = '[\uD800-\uDBFF]',
    lowSurrogate = '[\uDC00-\uDFFF]',
    highSurrogateBeforeAny = new RegExp(highSurrogate + any, 'g'),
    lowSurrogateAfterAny = new RegExp(any + lowSurrogate, 'g'),
    singleLowSurrogate = new RegExp('^' + lowSurrogate + '$'),
    singleHighSurrogate = new RegExp('^' + highSurrogate + '$');

  while ((arr = highSurrogateBeforeAny.exec(vr)) !== null) {
    if (!arr[1] || !arr[1].match(singleLowSurrogate)) { // If high not followed by low surrogate
      return false;
    }
  }
  while ((arr = lowSurrogateAfterAny.exec(vr)) !== null) {
    if (!arr[1] || !arr[1].match(singleHighSurrogate)) { // If low not preceded by high surrogate
      return false;
    }
  }
  return true;
}
{% endcodeblock %}

 - [view on github](https://github.com/kvz/phpjs/blob/master/functions/var/is_unicode.js)
 - [edit on github](https://github.com/kvz/phpjs/edit/master/functions/var/is_unicode.js)

### Example 1
This code
{% codeblock lang:js example %}
is_unicode('We the peoples of the United Nations...!');
{% endcodeblock %}

Should return
{% codeblock lang:js returns %}
true
{% endcodeblock %}


### Other PHP functions in the var extension
{% render_partial _includes/custom/var.html %}
