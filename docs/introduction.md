---
id: introduction
title: Introduction
sidebar_label: Introduction
---

Scaffold Kit is a set of building blocks for creating node.js scaffold tools. It
simplifies the process of creating a scaffold tool, reduces redundant
boilerplate code.

## Motivation

There are unlimited ways to setup a node.js project. Every team and every
developer has their own best practices. When start off a new project, we need
to keep the best practices that we've accumulated thus we copy code from
existing projects. Doing this is cumbersome and strange bugs may occur. Using a
scaffold tool to create a starter project with configurations is a more elegant
way to achieve this.

However, creating a scaffold tool requires a lot of extra work. We need to write
code to copy files, render templates, and parse command line arguments. It takes
longer than copying code from previous project. This prevents us from creating
a reusable and extendable scaffold tool.

When creating a complicated scaffold tool after created several scaffold tools,
we found there are a lot of common patterns and features. We pull out the common
and fundamental features from the scaffold tools, created Scaffold Kit.

With Scaffold Kit, creating a scaffold tool is as easy as following a tutorial,
and it takes even shorter time than copying code.

## Core concepts

* __app__ the app is the scaffold tool itself. Since the scaffold tool is a
command line tool, an app has several subcommands named commands and app level
options. Besides, the app defines the scaffold tool's description and version.

* __command__ a command represents a user's choice or action. It's the
subcommand that being executed. A command accepts it's own command line
arguments and options. The options defined in command are command level options.

* __instruction__ the instruction is what we tell Scaffold Kit to do. When
executing a command, we create instructions from user's input (aka arguments and
options). The instructions are pushed to the execution stack, and they are
rearranged and optimized.

* __executor__ the executor is the object that orchestrates the execution of
instructions. It manages where template files are located, and where to generate
the output files. It has global options that can be toggled. It has functions to
manipulate or reverse an instruction. And at last, it executes the instructions.

* __behavioral__ a complicated scaffold tool supports complex generation
configurations. Some configuration tweaks the entire behavior of the generated
app and it controls in what way the scaffold tool generates. There may be extra
command line options and code generation related to this special configuration.
We call this kind of configuration behavioral.

## Features included

* __instruction stack__ including template rendering, keeping directory, running
shell commands, installing and removing dependencies.

* __compound command__ Scaffold Kit supports command that is combination of
several commands, the execution and options are automatically merged.

* __reversible instruction__ with a builtin function, you can reverse an
instruction into its reversed instruction. For example, reverse installing into
removing, reverse creating a file into deleting a file.

* __behavioral__ create complicated scaffold tool with builtin behavioral
feature.

* __auto generated help__ the help message are automatically generated from the
commands and options that defined in the app object and the command objects.

* __serialized options__ scaffold Kit automatically serialize user's savable
options into a record file. These are the default options when user haven't
specify these options' value.

* __arranged instructions__ in a command, there are a lot of instructions to be
executed. Scaffold Kit rearrange them into alphabetical order and makes them
look and output cleanly.

* __unit testing made easy__ with scaffold Kit unit testing tool, you can unit
test your scaffold tool easily.

## Next

* [Read the Guide](start-a-new-project)
* [Read the API documentation](api-doc)
