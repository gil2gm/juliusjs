JuliusJS
====

> A speech recognition library for the web

JuliusJS is an opinionated port of Julius to JavaScript. <br>
It __actively listens to the user to transcribe what they are saying__ through a callback.

```js
// bootstrap JuliusJS
var julius = new Julius();

julius.onrecognition = function(sentence) {
  console.log(sentence);
};

// say "Hello, world!"
// console logs: `> HELLO WORLD`
```

###### Features:

- Real-time transcription
 - Use the provided grammar, or write your own
- 100% JavaScript implementation
 - All recognition is done in-browser through a `Worker`
 - Familiar event-inspired API
 - No external server calls

## Quickstart

##### Using Express 4.0

1. Grab the latest version with bower
 - `bower install juliusjs --save`
1. Include `julius.js` in your html
 - `<script src="julius.js"></script>`
1. Make the scripts available to the client through your server
  ```js
  var express = require('express'),
    app     = express();
  
  app.use(express.static('path/to/dist'));
  ```
1. In your main script, bootstrap JuliusJS and register an event listener for recognition events
  ```js
  // bootstrap JuliusJS
  var julius = new Julius();
  
  // register listener
  julius.onrecognition = function(sentence) {
    // ...
    console.log(sentence);
  };
  ```

- Your site now has real-time speech recognition baked in!

### Configure your own recognition grammar

In order for JuliusJS to use it, your grammar must follow the [Julius grammar specification](http://julius.sourceforge.jp/en_index.php?q=en_grammar.html). The site includes a tutorial on writing grammars.<br>
By default, phonemes are defined in `voxforge/hmmdefs`, though you might find [other sites](http://www.boardman.k12.oh.us/bdms/phonological/44Phonemes.pdf) more useful as reference.

- Building your own grammar requires the `mkdfa.pl` script and associated binaries, distributed with Julius.
 - _On Mac OS X_
   - Use `./bin/mkdfa.pl`, included with this repo
 - _On other OS_
   - Run `emscripten.sh` to populate `bin` with the necessary files

1. Write a `yourGrammar.voca` file with words to be recognized
 - The `.voca` file defines "word candidates" and their pronunciations.
1. Write a `yourGrammar.grammar` file with phrases composed of those words
 - The `.grammar` file defines "category-level syntax, i.e. allowed connection of words by their category name."
1. Compile the grammar using `./bin/mkdfa.pl yourGrammar`
 - The `.voca` and `.grammar` must be prefixed with the same name
 - This will generate `yourGrammar.dfa` and `yourGrammar.dict`
1. Give the new `.dfa` and `.dict` files to the `Julius` constructor
  
  ```js
  // when bootstrapping JuliusJS
  var julius = new Julius('path/to/dfa', 'path/to/dict');
  ```

## Advanced Use

### Configuring the engine

The `Julius` constructor takes three arguments which can be used to tune the engine:

```js
new Julius('path/to/dfa', 'path/to/dict', options)
```

_Both 'path/to/dfa' and 'path/to/dict' must be set to use a custom grammar_

##### 'path/to/dfa'
- path to a valid `.dfa` file, generated as described [above](#configure-your-own-recognition-grammar)
- if left `null`, the default grammar will be used

##### 'path/to/dict'
- path to a valid `.dict` file, generated as described [above](#configure-your-own-recognition-grammar)
-if left `null`, the default grammar will be used

##### options
- `options.verbose` - _if `true`, JuliusJS will log to the console_
- `options.stripSilence` - _if `true`, silence phonemes will not be included in callbacks_
 - `true` by default
- `options.*`
 - Julius supports a wide range of options. Most of these are made available here, by specifying the flag name as a key. For example: `options.zc = 30` will lower the zero-crossing threshold to 30.<br> _Some of these options will break JuliusJS, so use with caution._
 - A reference to available options can be found in the [JuliusBook](http://julius.sourceforge.jp/juliusbook/en/).
 - Currently, the only supported hidden markov model is from voxforge. The `h` and `hlist` options are unsupported.

## Examples

### In the wild

_If you use `JuliusJS` let me know, and I'll add your project to this list (or issue a pull request yourself)._

1. Coming soon...

## Motivation

## Developers

___Contributions are welcome! See `CONTRIBUTING.md` for guidelines.___

### Build from source

#### Emscripten

#### Codemap

---

*JuliusJS is a port of the "Large Vocabulary Continuous Speech Recognition Engine Julius" to JavaScript*
