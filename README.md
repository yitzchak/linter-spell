# linter-spell [![Travis-CI Build Status](https://img.shields.io/travis/yitzchak/linter-spell/master.svg?label=Linux/OSX%20build)](https://travis-ci.org/yitzchak/linter-spell) [![AppVeyor Build Status](https://img.shields.io/appveyor/ci/yitzchak/linter-spell/master.svg?label=Windows%20build)](https://ci.appveyor.com/project/yitzchak/linter-spell)

Linter plugin for Atom using ispell compatible interface such as aspell,
hunspell, enchant, etc.

## Installing

Use the Atom package manager and search for "linter-spell", or run
`apm install linter-spell` from the command line.

## Prerequisites

This package relies on a `ispell` compatible package such as `hunspell`.

## Usage

Spell checking is done upon document save with misspellings displayed by
[linter](https://atom.io/packages/linter). Misspellings can be
corrected or added to personal dictionary using the
[intentions](https://atom.io/packages/intentions) package via
`ctrl` + `enter` on OSX and `alt` + `enter` on Linux and Windows.

## Providers

Spell checking plain text documents is included in the package. To spell check
other document types use a `linter-spell-grammar` provider:

*  LaTeX - [linter-spell-latex](https://atom.io/packages/linter-spell-latex)

## Creating New Providers

New grammars can be added by implementing a `linter-spell-grammar` provider.
This can be done by adding the following to `package.json`

    "providedServices": {
      "linter-spell-grammar": {
        "versions": {
          "1.0.0": "provideGrammar"
        }
      }
    }

The provided service should be as follows

    provideGrammar () {
      return [{
        grammarScopes: ['text.plain'],
        getDictionaries: textEditor => { return ['en_US'] },
        getRanges: (textEditor, ranges) => { return { ranges: ranges, ignoredRanges: [] } }
      }]
    }

The `getDictionaries` method is optional and should scan `texEditor` for a
file specific override of the user's default dictionaries and return `[]` if
no dictionary references were found. See
[linter-spell-latex](https://atom.io/packages/linter-spell-latex)
for an implementation using TeX magic comments.

The `getRanges` method should check the `ranges` parameter for text to ranges
which are valid to spell check. It should return a list of modified ranges in
`ranges` and can also return `ignoredRanges` for ranges that should not be
checked. The interval difference between `ranges` and `ignoredRanges` will
actually be checked.

## Status

Please note that this package is in an **alpha** state.
