#!/usr/bin/env stack
> -- stack script --resolver lts-8.12 --package turtle --package markdown-unlit -- "-pgmL markdown-unlit"

```haskell
{-# LANGUAGE OverloadedStrings #-}

import Turtle
```

# Literate README

[![Build status](https://travis-ci.org/silky/literate-readme.svg)](https://travis-ci.org/silky/literate-readme)

The readme that builds itself!

You can setup and build this project by running this very readme. This is what
the travis job does!

Example: `./README.lhs --setup --test`


```haskell
parser :: Parser (Bool, Bool, Bool)
parser = (,,) <$> switch "setup" 's' "Set up the stack environment."
              <*> switch "test"  't' "Build the project and run the tests."
              <*> switch "build" 'b' "Just build, don't run tests."
```

```haskell
main = void $ do
    (setup, test, build) <- options "Literate README" parser
    let ops = doSetup setup .&&. doBuild build .&&. doTest test
    ops .||. die "Step failed."

nop = shell "true" empty

stackOrNop op True = shell ("stack " <> op) empty
stackOrNop _  _    = nop
```

## Setup

```haskell
-- | Call this with: ./README.lhs --setup
doSetup = stackOrNop "setup"
```


## Build

```haskell
-- | Call this with: ./README.lhs --build
doBuild = stackOrNop "build"
```


## Test

```haskell
-- | Call this with: ./README.lhs --test
doTest = stackOrNop "test"
```
