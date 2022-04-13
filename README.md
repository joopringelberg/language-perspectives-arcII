# perspectives-arc README

This is the README for the language extension "perspectives-arc" in vscode. To benefit from syntax coloring in vscode while editing a perspectives model (written in the ARC language), you need to add two extensions to your vscode environment:

* a language extension for the Perspectives ARC language (provided in this repo);
* a theme that complements the language extension (see this repo).

## Getting it
Download the language extension from this link: [arc.tmLanguage](./perspectives-arc-0.0.1.vsix).

Download the theme extension from this link: XXX

## Installing it
Install the language extension with:

```
code --install-extension arc.tmLanguage-X.Y.Z.vsix
```

Install the theme extension ...

## Uninstalling it
VS Code makes it easy to manage your extensions. You can install, disable, update, and uninstall extensions through the Extensions view, the Command Palette (commands have the `Extensions:` prefix).



## Example files
Model file examples can be found in the `arc sources` directory.

# Syntax coloring for Perspectives
Both vscode and Atom use the TextMate approach to colorize source files. The TextMate approach is based on rules that are a combination of named regular expressions to select a part of the source text (the `scope`) and a string that identifies a particular style to format it. 

* the (operational) rules for the Perspectives language are kept in the file `arc.tmLanguage.json` (but their source is in the file arc.tmLanguage.yaml);
* the styles are kept in a theme file: `not yet available`.

TextMate has an elaborate convention of scope names. The objective is that if the various themes adhere to these scope names, a theme can style source files in a broad range of languages. 

I've tried to adhere to the TextMate conventions, but Perspectives differs quite a lot from general programming languages. What follows is a discussion of how I've mapped Perspectives concepts to TextMate conventions.

### Meta root group
TextMate distinguishes a number of 'root groups': broad categories for defining scopes. The `meta` group is one of them. It is used to define larger parts of the source text. These scopes are not used for formatting, but to contextualize behaviour, for example to expand an expression when the tab key is pressed. Currently, we don't have such behaviours in the IDE support for Perspectives. However, the following Perspectives constructs would fall in the meta root group, as they define lexical constructs with an indented body:

* all contexts;
* all roles;
* state
* screen
* view
* state transitions (on entry, on exit)
* automatic actions (do)
* actions
* notifications
* in state expressions.

As we don't yet have use for meta group rules, they have not been defined yet.

### Types
Contexts (domain, case, party, activity) are Perspectives types. So are all role types (context, user, external, thing) and properties. All these types map to the `entity` root group. We don't have rules for an entire entity scope, as this is not how TextMate is meant to be used (`meta` covers such areas). However, we use `entity.name`, `entity.other` etc.

### State related keywords
There is no explicit flow of control in Perspectives. However, using automatic actions in state transitions comes close. For that reason we map all keywords that have to do with state and automatic actions onto `keyword.control`.

### Treatment of User roles and their Perspectives
As Perspectives are arguably very important, we give all parts of Perspectives the same color as the `perspective` keyword itself. For the same reason we let user role identifiers stand out with a different color. This does not just hold for the definition of a user role, but for references to these identifiers, too.

### Simple values
We treat simple values such as Booleans, DateTimes, Strings and Numbers as constants, by collecting them in the constant root group.

### Operators
Both alphanumeric operators (such as binds, filter, binder) as non-alphanumeric operators (such as >> and <=) are given the same color. This causes queries (e.g. in calculated roles and expressions) to stand out in the text.

### Standard Variables
We have four standard variables in Perspectives. These are mapped to `variable.language`.

### User-defined variables
Regrettably we have no way (yet) of coloring variables defined in a let or let* expression.

### Property facets
Properties and roles can be defined with two standard facets: `mandatory` and `relational` (the default value for these facets are optional and functional - there are no keywords for those values).

Properties can be further defined with so-called 'constraining facets':

* minLength
* maxLength
* enumeration
* pattern
* maxInclusive
* minInclusive

All these keywords are colored in the same way.

### Assignment statements: side effects
State changing statements should stand out. Assignment as such is not recognized in the TextMate conventions. We lump them under keyword.control.other with the suffix .assignment.arc and color them red. 
  

# Development
Adapt the `arc.tmLanguage.yaml` file. Then run:

```
npx js-yaml syntaxes/arc.tmLanguage.yaml > syntaxes/arc.tmLanguage.json
```
Open a version of vscode with the extension loaded by pressing F5. While open, when the grammar has changed (and after recompiling it!) press cmd-r in the extension window to (re)load the changed definition.

Some useful links:

- [vscode language extension guide](https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide#tokenization)
- [textmate grammar explanation](https://macromates.com/manual/en/language_grammars)
- [yaml in y minutes](https://learnxinyminutes.com/docs/yaml/)
- [Onigurama regular expressions](https://macromates.com/manual/en/regular_expressions)

Trigger the scope inspector from the Command Palette with the `Developer: Inspect Editor Tokens and Scopes`. Alternatively, bring it up with the shortcut keys `cmd-shift-i`.

## Package for distribution
Apart from publishing this extension on a marketplace (which we do not cover here), it is possible to package the extension. It can then be distributed, e.g. from github. The instructions here are taken from [microsoft](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#packaging-extensions).

Package with this command:

```
vsce package
```

## Requirements

`js-yaml` is a local development requirement. `yo` and `vcse` are global requirements.

## Extension Settings

This language extension as no specific settings.

## Known Issues

* Make comments effective and disable other rules within comments.
* Keywords view, aspect, use, filledBy and indexed.


## Release Notes

### 1.0.0

Initial release of perspectives-arc.

