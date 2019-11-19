![templast logo](./artwork/logo.svg)

**Templ**ate **A**bstract **S**yntax **T**ree (aka: templast )
> a unist compliant syntax tree spec for templating languages

## Why

Because there are plenty of "components" and views that are really nice but only available in some other template engine. It would be nice to make a bridge to common ground such that the template langauge could be ported to templast which would unlock a world of templates, view, components such that I am not limited to my epression types based on some historical choice of template language I learned.
  
## Install

```npm i @federa/templast @federa/templast-cli -D```

## Usage

pipe it in 
```sh
$> cat someTemplate.hbs | npx templastCLI -c config.js > someTemplate.apacheVTL
```

use the command line args
```sh
$> npx templastCLI -c config.js -i someTemplate.hbs -o someTemplate.apacheVTL
```

redirect a file in
```sh
$> npx templastCLI -c config.js < someTemplate.hbs > someTemplate.apacheVTL
```

use the lib/module in with es modules
```mjs
import templast from '@federa/templast'
import hbsParse from '@federa/handlebars-parse'
import vtlStringify from '@federa/vtl-stringify'
import vfile from 'to-vfile'

templast()
  .use(hbsParse)
  .use(vtlStringify)
  .process(vfile('./myexmaple.hbs')) // or change it to a real handlebars example

```

use the lib/module in within node
```js
const vfile = require('to-vfile')  
const templast = require('@federa/templast') 
const hbsParse = require('@federa/handlebars-parse') 
const vtlStringify = require('@federa/vtl-stringify')

templast()
  .use(hbsParse)
  .use(vtlStringify)
  .process(vfile('./myexmaple.hbs')) // or change it to a real handlebars example

```

## Wait What?

- let's say you have a ton of tempaltes written in `EJS` but you are trying to migrate to `handlebars`
- let's say you started working with API gateway and they have a templating language requirement of ApacheCTL and you just refuse to learn a new template language - you can now transpile another template language to apacheVTL
- lets's say that you are very comfortable in musctache - but want some exampeles of how to use `doT.js` you can now convert your own examples to that flavor of templating language

If you deal with enough template languages it would be nice to be able to move back and forth as much as is possible.

For exmaple `Mustache` template for example would not easily/beautifully support all the features from a `handlebars` template but for many other types it would be very nice.

the AST would need to support


AST Proposal
---------------------

- `ROOT` 
    - version=1
- `COMMENT`
    - isMultiline: false
    - isStructuredData: false
- `LITERAL`
    - `string`
    - ?? do we really need anything else?
    - `number`
    - `array`
    - `object`
    - `boolean`
- `PARTIAL`
    - `inlineDefinition`
        - name
        - content
        - usable arguments
    - `referenced`
        - name
        - location
        - params passed in
- `EXPRESSION`
    - `BlockStatement` // hash lookup to be replaced with string
        - escapeHTML: false
        - booleanInversion: false
        - stripWhiteSpace: false
        - noNewLine: false
     - `SubExpression` // is it needed, given children nodes?
     - `MustacheStatement` // hash.lookup with children conditionally availalbe 
     - `StateDeclaration` // ApacheVTL - - to a pure data in template - Warn that some data 'defaults' may be lost
     - `ContextLookup` // Blcok Expression
     - `Macro-Helper Block`
        doT + VTL + handlebars + EJS
        EJS Examples: 
        ```<% const someFunc = ()=>{} %>```
        ```<% const|let|var someFunc = function(){} %>```
    - `childrenContext`
        List = DATA[]
        INDEX = interger
    - `SetContext`
    - `ExitContext`
    - `ConditionalContext`

To give some assurances that this AST works well, it should be demonstrated that many template languages fit into the paradigm.

#### Handlebars Support
- __[templast-hbs-parse](#)__
  parse handlebars templates into `templast`
- __[templast-hbs-verify](#)__
  verify that the existing `templasTree` could be rendered to handlebars without issue
- __[templast-hbs-stringify](#)__
  render the tree to a handlebars template

#### mustache Support
- __[templast-mustache-parse](#)__
- __[templast-mustache-verify](#)__
  assigns `warnings` or `failures` to the template if the source file was from some other template system and is now going to be rendered as a mustache file.
- __[templast-mustache-stringify](#)__

### doT Support
- __[templast-doT-parse](#)__
  incorporate doT templates into `templast`
- __[templast-doT-verify](#)__
  incorporate doT templates into `templast`
- __[templast-doT-stringify](#)__
  incorporate doT templates into `templast`

#### apacheVTL Support
- __[templast-apacheVTL-parse](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-apacheVTL-verify](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-apacheVTL-stringify](#)__
  incorporate apacheVTL templates into `templast`

### js Tempalte Literals
- __[templast-jsTemplateLiterals-parse](#)__
  incorporate jsTemplateLiterals into `templast`
- __[templast-verify-jsTemplateLiterals](#)__
  incorporate jsTemplateLiterals into `templast`
- __[templast-jsTemplateLiterals-stringify](#)__
  incorporate js Template Literals into `templast` where they are rendered like: 
  
  ```js
  const t = (data)=>`some text ${data.attr} with more static content`
  ```


#### EJS Support
- __[templast-ejs-parse](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-verify](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-stringify](#)__
  incorporate apacheVTL templates into `templast`

#### EJS Support
- __[templast-ejs-parse](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-verify](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-stringify](#)__
  incorporate apacheVTL templates into `templast`


#### JADE Support
- __[templast-ejs-parse](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-verify](#)__
  incorporate apacheVTL templates into `templast`
- __[templast-ejs-stringify](#)__
  incorporate apacheVTL templates into `templast`

#### virtual DOM support
- attempt to render a templast into a mixture of `hast` nodes and `templast` nodes such that you might be able to represent a template as:
- this would let you compose your template in handlebars,etc, and then transpile your tempalte to a virtual dom function via an ℎ ƒunction.
- configure the `virtual-dom-strigify` with the fucntion token to use: _default is 'h'_ but could easily be `preact.h` or `React.createElement`

```mjs
/** @jsx Preact.h */
import attrs from 'myCSSinJSfile.js'
const t = (data) => h('div',attrs.div, [
                        h('div', attrs.div, [
                            h('h3', attrs.h3, data.title )
                        ]), 
                        h('form', attrs.form, 
                        h('input','button',data.buttonText)
                    ])
```

## Roadmap

- help needed for a `virtual DOM` _`parse`_ and _`stringify`_
- help needed for creating some type-system guarentees
- help needed for `jade`
- help needed for `pug`
- help needed for approaching a subset of `EJS`
- help needed for approaching a subset of `ERB`
- explore supporting some `PHP.twig` templates

### Usage

`npm start` will download a ton of other git repos into the packages folder. 
Where each of the packages have their own repo and lifecycle.

Then use the `npx meta` command to manage all those sub-repos in some type of `lerna-overlord-style`

### PIPEDREAMS

- to-and-fro-Apache-Velocity-Templates
  - Parse VTL to unist
  - Parse mustache to unist
  - Parse mustache to unist
  - VTL to mustache ?? https://mustache.github.io/mustache.5.html?
    + hash lookups
    + hash context windows - and subsequent attribute lookups
    + comments
    + partials
  - VTL to handlebars
    + hash lookups
    + hash context windows - and subsequent attribute lookups
    + HTML escaping
    + block expressions
    + consider it a foreach
        + Sets context to a list and allows attributes to be read for each item
    + nested attr.path
    + context stack can be traversed upward
    + comments
    + helpers can be registered
        + built in `#with` and `#each`
    + partials
  - TemplateAST
    - Mustache
    - Handlebars
    - Apache VTL
    - JS Template Strings
    - doT
    - EJS
    - Pug
    - Marko?
    - Jade
  - VTL to MDX?
  - mustache to VTL
  - handlebars to VTL

- Babel7Ast-to-mdxast

  - ref: https://github.com/babel/babel/blob/master/packages/babel-parser/ast/spec.md

- TSast-to-mdxast

- MDXAST

    - MDAST + 

    - JSX -> script

    - COMMENT

    - Import

    - Export

- MDXHAST

    - HAST +
        type: element
        tagName: div, span, form, input, 
        properties: {}
        children:[]
        ƒ h(tagType, {props}, [ children ] )

    - imports

    - exports

    - inlineCode

means that an .mdx file is really a fancy JS file 
a certain type of JS file that has some convention allowing it to be rendered when invoked with proper context 





### Handlebar Exmaple
+ hash lookups
+ hash context windows (aka: set context path) - and subsequent attribute lookups
+ HTML escaping options for rendered context
+ comments
+ block expressions that include some control flow
+ consider a foreach
+ Sets context to a list and allows attributes to be read for each item
+ nested attr.path
+ context stack can be traversed upward path(../parentdAttr)
+ built in `#with` and `#each`
+ helpers/macros can be registered and used
+ partials

What's the difference in a partial and a helper? 
other than the partial is often requiring a template fetch.

## SCRATCH PAD

parserconfig for every `*-parse` 
expandPartials = true
partialsProvided = {}
partialsResolverFunction = ()=>{}
configDelimiters  =null = `auto`/default

        handlebars.parse( string ): AST
        hbs.AST.MustacheStatement 
        hbs.AST.BlockStatement 
        hbs.AST.PartialStatement 
        hbs.AST.PartialBlockStatement 
        hbs.AST.ContentStatement 
        hbs.AST.CommentStatement 
        hbs.AST.SubExpression 
        hbs.AST.PathExpression 
        hbs.AST.StringLiteral 
        hbs.AST.BooleanLiteral 
        hbs.AST.NumberLiteral 
        hbs.AST.UndefinedLiteral 
        hbs.AST.NullLiteral 
        hbs.AST.Hash 
        hbs.AST.HashPair

Engines Considered:

- Mustache
- Handlebars
- Apache VTL
- JS Template Strings
- doT
- EJS
- Pug
- Marko?
- Jade


Can you view MDX as a fancy system of tons of partials?

If so what if templates that rendered HTML `<div> etc... </div>` could also render a virtual dom via h functions?

What if you could easily make interop on things that usually render a virtualDOM? - and coerece them to render strings?

### Handlebars Support
hbs-parse //  HBS source --> TASTY
verify-hbs // TASTY --> HBS dialect
hbs-stringify --> HBS Dialect --> string

### mustache Support
mustache-parse
verify-mustache // assigns `warnings` or `failures` to the template if the source file was from some other template system and is now going to be rendered as a mustache file.
mustache-stringify

### apacheVTL Support
apacheVTL-parse
verify-apacheVTL 
apacheVTL-stringify

### doT Support
doT-parse
verify-doT
doT-stringify

### jsTempalteLiterals Support
jsTemplateLiterals-parse
verify-jsTemplateLiterals
jsTemplateLiterals-stringify
