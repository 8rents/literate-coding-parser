# Literate Coding Parser

> *A node based literate coding parser. A new way to code! Code by writing documentation!*

---

- GitHub: https://github.com/8rents/literate-coding-parser
- NPM: —

---

## Example

When coding using literate coding you write all of your code within markdown files. 

The files are given their normal extension and appended with `.md`. 

Each file is started with YAML front matter. Then you write all about how the file works, what it does, and eventually within the markdown write a code snippet that you will document.

### `app.js.md`

````markdown
---
title: app.js
description: The main entry point for the application
version: 1.0
---

# app.js (v1.0)

> *The main entry point for the application*

---

## Abstract 

`app.js` serves as the main entry point for the whole application.

## Initial variable declaration

`app.js` starts by defining three variables as constants. 

The first variable is the express application itself

```
const express = require('express')
```

The second is the instance of the express app

```
const app = express()
```
The third is the port that the application will run on

```
const port = 3000
```

## Application Routing

Once the three variablers are defined we move into the application routing

### Index Page

The first route is the index page. It's defined with a single `/`. It returns 'Hello World!'

```
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

## Port declaration

After defining the routing we tell the application what port to listen to with `app.listen`. We log to the console 'Example app listening on port 3000'

```
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
````

This is a literate JavaScript file that has the code for an express hello world app.

---

### Compiled version

This file will be compiled to:  

#### `app.js` and will look like this:

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
```

All of the markdown is stripped away leaving just the JavaScript.

#### app.js.md will look like this:

![markdown-example](./markdown-example.png)

## Configuration 

Using a yaml file we tell the compiler where to compile the files to:

```yaml
app: express-hello-world
source_dir: src
compilation_dir: app
docs_dir: docs
index_filename: README
```

##### This will create the directory structure:

```css
express-hello-world/
	README.md
	LICENSE
	package.json
	src/
		app.js.md
	app/
		app.js
	docs/
		README.md
		flow.md
		app.js.md
			modules/
				README.md
```

## Automatic linking of files in markdown

The parser will automatically create index files (README.md) and link all sub files and folders within them.

---

**🤍 2025** [**Brenton Holiday**](https://brenton.holiday/) **|** [GitHub](https://github.com/8rents?tab=repositories)

