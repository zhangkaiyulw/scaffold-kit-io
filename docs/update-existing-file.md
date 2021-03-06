---
id: update-existing-file
title: Update existing file
---

In the [previous post](create-first-command.md), we learned how to use
`createFile` instruction to create new files. In this section, let's learn to
use `updateFile` instruction to update files.

Recall the blog scaffolding tool structure in the
[previous post](create-first-command.md), this time we start to create the
`post` command.

```bash
scaffold-kit command post
```

## Design our command

The `post` command will have the following usage:

```bash
blog post "Post's Name"
```

By calling this command, we create a file named `posts-name.html` which
represents this post. And we update `index.html` with the link. Let's
now modify the generated command file `lib/commands/post/index.js`.

## Implement the command

1. Require `kebabCase` at the top of the file.

```js
const kebabCase = require('lodash/kebabCase');
```

2. Add these line to the top of execution block.

```js
options.title = args[0] || 'Untitled Post';
options.filename = `${kebabCase(options.title)}.html`;
```

We convert the post name that user passes into kebab cased filename. And
passing them into options for easy referencing and rendering.

3. Update filename remapping to like this.

```js
const nameMap = {
  'post.html': options.filename
};
```

This will have our `post.html` template file to be rendered into
`posts-name.html`.

After updating the command execution, lets update the templates. Rename the
autogenerated template file to `_post.html` and alter its content.

```html
<html>
<head>
  <title><%= title %></title>
</head>
<body>
  <a href="./index.html">Back</a>
  <h1><%= title %></h1>
  <p>TODO: update blog content here</p>
</body>
</html>
```

Now try our blog scaffolding tool, open the autogenerated web page. Page title
shows and the back button works nicely. However, there are two problems:

1. In the title bar, the page shows post title but doesn't show blog name.
2. In the home page, there isn't a link that link to this post.

Let now get the blog title from the `index.html`.

1. Adding the require `fs` module.

```js
const fs = require('fs');
```

2. Add these into execution block.

```js
options.blogName = fs.readFileSync(path.join(wd, 'index.html'))
  .toString()
  .match(/<title>Home - (.*)<\/title>/)[1];
```

3. Replace this line in the template file.

```html
  <title><%= title %> - <%= blogName %></title>
```

Now we fixed title display problem. Let's update the post list now.

1. Insert `updateFile` into require block.

```js
const {
  // omitted...
  createFile,
  updateFile
} = require('scaffold-kit/executor');
```

2. Append these at the end of execution block.

```js
updateFile({
  at: 'index.html',
  updator: (content) => {
    if (!content.match(new RegExp(options.title))) {
      return content.replace(
        / {2}<ul id="posts">(.*) {2}<\/ul>/ms,
        `  <ul id="posts">$1    <li><a href="./${options.filename}">\
${options.title}</a></li>\n  </ul>`
      );
    } else {
      return content;
    }
  },
  rollbacker: (content) => {
    return content.replace(
      new RegExp(`    <li><a href="./${options.filename}">\
${options.title}</a></li>\n`, 'm'),
      ''
    );
  }
});
```

`updateFile` is an execution instruction that updates the content of a file.
Basically it takes the filename, and return its new string content in the
updator. You noticed that there is a `rollbacker` in our code, we'll take use of
it in the next section.

## Try out our new command

Now let's create a new post with the following command:

```bash
blog post "Today's Post"
```

Now open `index.html` and navigate back and force. Isn't it a magic. If we try
to do this again, we see this:

```text
  up-to-date index.html
  up-to-date todays-post.html
```

This is the very smart behavior that we want an scaffolding tool to have.

In the next section, we are going to create a very simple yet powerful command:
destroy.
