# Literate Coding Parser

> *A node based literate coding parser. A new way to code! Code by writing documentation!*

---

- **GitHub:** https://github.com/8rents/literate-coding-parser
- **NPM:** N/A

---

## About Literate Coding

> *The following about section is taken from the [Wikipedia article on Literate Programming](https://en.m.wikipedia.org/wiki/Literate_programming)*
>
> ---
> 
> Literate programming is writing out the program logic in a human language with included (separated by a primitive markup) code snippets and macros.
> It was first introduced in 1984 by Donald Knuth, who intended it to create programs that were suitable literature for human beings. He implemented it at Stanford University as a part of his research on algorithms and digital typography.

---



## File Extensions

Since we are writing a **literate javascript file**, `.js` is used and then appended with `.lit` after the codes file extension. 

**Note:** *That since the file extension is `.js` the compiler will know all the code is javascript, so we won't need to specify the language in any of the markdown code fences.*

---

### Configuration with YAML Front matter

Each file is started with YAML front matter. Then you write all about how the file works, what it does, and eventually within the markdown write a code snippet that you will document.

Front matter will be used to add metadata to code and documentation and to toggle file specific compilation options.

```yaml
---
title: app.js
description: The main entry point for the application
version: 1.0
---
```

## Example: `app.js`

This example `app.js` file is the main application file of the hello world of the javascript express app.

When coding using literate coding you write all of your code within markdown files. 

`app.js.lit`

~~~markdown
# `app.js` (v1.0)

> *The main entry point for the application*

---

## About

`app.js` serves as the main entry point for the whole application.

## Initial variable declaration

`app.js` starts by defining three variables as constants. 

#1 the express application itself

```javascript
const express = require('express')
```

#2 The instance of the express app

```javascript
const app = express()
```

#3 The port that the application will run on

```javascript
const port = 3000
```

## Application Routing

Once the three variables are defined we move into the application routing. 

### Express `get` method

Routes are defined using the `get` method of the express `app`. `get` takes a URI as the first parameter. The second parameter is a function with parameters for request (`req`) an result (`res`).

The **reqest (`req`)** is the first parameter or the path. In this case `/`.

The **result (`res`)** is what should happen if the request is met. The result takes a method `.send`. In this case we are sending the string 'Hello World'

#### Index Page

The first and only get request is the index page. It's defined with a single `/`. It returns 'Hello World!'

```javascript
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

## Port declaration

After defining the routing we tell the application what port to listen to with `app.listen`. We log to the console 'Example app listening on port 3000'

```javascript
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

~~~

This is a literate JavaScript file that has the code for an express hello world app.

---

## File Compilation

Two types of files are produced when a `.lit` file is compiled. 

- Application files - These are files for the computer to read.
- Documentation files - These are files for the end user to read. 

### Application Files Compiled: `app.js`

```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
````

All of the markdown is stripped away leaving just the JavaScript.

### Documentation Files Compiled: `app.js.md`

![markdown-example](markdown-example.png)

## Configuring compiled literate output using YAML

Using a yaml file we tell the compiler where to compile the files to:

### Global configuration file: `config.yml`

The `config.yml` file is located in the root directory of a literate coding project. 

The following are the default values:

```yaml
lit:
# all literate coding configs
  name: express-hello-world
  description: A simple hello world app using the nodejs express framework
  # app name
  src:
  # Source file options
    dir: src
    # Directory where the source files are
    lit-ext: lit
    # File extension of lit files
    format: markdown
    # Format the lit files are written in
  compile:
  # compile options
    dir: app
    # where src files compile to
    comments: none
    # should compiled source files have any comments  
    # options are: none, headers, all
    minify: no
    # should the files be minified
  docs:
  # autogenerated documentation
    dir: docs
    # where documentation is autogenerated
    formats:
    # What formats of documentation should be generated
    # different formats have different defaults
      markdown:
        converter: gfm
        # which markdown converter. default: gfm
        extension: md
        # Markdown file name extension.
        # default: md
        index: README
        # What autogenerated directory index files filename should be.
        # default: README
      html:
        converter: gfm
        extension: html
        index: index
        stylesheet: styles.css
```

#### This will create the directory structure:

```bash
express-hello-world/
├───config.yml
├───README.md
├───LICENSE
├───package.json
├───src/
│   └───app.js.lit
├───app/
│   └───app.js
└───docs/
    ├───markdown/
    │   ├───app.js.md
    │   ├───flow.md
    │   ├───modules/
    │   │   └───README.md
    │   └───README.md
    └───html/
        ├───app.js.html
        ├───flow.html
        ├───modules/
        │   └───index.html
        └───index.html
```
> ***Note that the root level `README.md` is not going to be compiled. It is only used as a project description.***
>
> ***Only `.lit` files in the `src` directory will be compiled.***

## Automatic linking of files in markdown

The parser will automatically create index files (`README.md`) and link all sub files and folders within them. So for example two files are auto created on their own in the `docs/markdown` and the `docs/html` folders and are not based off of any existing files. 

### Example README file (`docs/markdown/README.md`)

~~~markdown
# express-hello-world

> *A simple hello world app using the nodejs express framework*

---

## Application flow

- **Application [flow](flow.md)** - Shows the complete flow of code logic and files in a flow chart format.
- **Entry Point: [app.js](app.js.md)** - This is the starting point of the application. All code comes after this file.\

### Modules

Check the [list of modules](modules/README.md) in the express-hello-world application.

*No modules are listed, when you create the first module in the application, it will show upm in a list here.*

~~~

### Individual file yaml configuration using frontmatter

Indivudual literate files can be configured using yaml frontmatter. 

Here is an example of tweaking just the `app.js` yaml so instead of stripping all markdown out of the `.js` files only the headers remain in the code. 

Atm the top of `app.js.lit`

```yaml
---
lit:
  compile:
    comments: headers
---
```

#### Compiled `app.js` javascript file (With comment headers enabled)

```javascript
// Initial variable declaration
const express = require('express')
const app = express()
const port = 3000

// Application Routing
app.get('/', (req, res) => {
  res.send('Hello World!')
})

// Port declaration
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

Only the headers for each section are included into the production code.

---

**🤍 2025** [**Brenton Holiday**](https://brenton.holiday/) **|** [GitHub](https://github.com/8rents?tab=repositories)

