# perspectives-arc README

This is the README for the language extension "perspectives-arc" in vscode. To benefit from syntax coloring in vscode while editing a perspectives model (written in the ARC language), you need to add two extensions to your vscode environment:

* a language extension for the Perspectives ARC language (provided in this repo);
* a theme that complements the language extension (see the repo [theme-perspectives-arc](https://github.com/joopringelberg/theme-perspectives-arc.git); the theme extension may be downloaded there).

## Getting it

Go to the [latest release](https://github.com/joopringelberg/language-perspectives-arcII/releases) (on Github: Code tab, section releases on the right, click `latest`). Download the file named `language-perspectives-arc-X.Y.Z.vsix`, where X, Y and Z are the components of the semantic version number.

## Installing it
You can manually install both the theme extension and the ARC language extension (as it is packaged in a .vsix file). Use the `Install from VSIX` command in the Extensions view command dropdown, or the `Extensions: Install from VSIX command` in the Command Palette, point to the .vsix file.

## Uninstalling it
VS Code makes it easy to manage your extensions. You can install, disable, update, and uninstall extensions through the Extensions view, the Command Palette (commands have the `Extensions:` prefix).


## Example files
A model file example can be found in the `arc sources` directory.

## Release Notes

### 0.0.1
Initial release.

### 0.0.2
Modifications to readme and various house holding issues in the package file.

### 0.0.3
* adapted the fileTypes key in the yaml source for compatibility with Atom (atom needs an array of names and cannot handle the leading dot).

### 0.0.4
The tokenization is now aimed towards support of the base16 color schemes, to provide a broad range of themes.

Initial release of perspectives-arc.

## Requirements

`js-yaml` is a local development requirement. `yo` and `vcse` are global requirements.

## Extension Settings

This language extension as no specific settings.

## Known Issues

* Make comments effective and disable other rules within comments.
* Keywords view, aspect, use, filledBy and indexed.


# Syntax coloring for Perspectives
Both vscode and Atom use the TextMate approach to colorize source files. The TextMate approach is based on rules that are a combination of named regular expressions to select a part of the source text (the `scope`) and a string that identifies a particular style to format it. 

* the (operational) rules for the Perspectives language are kept in the file `arc.tmLanguage.json` (but their source is in the file `arc.tmLanguage.yaml`);

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

# Base16
[Base16](http://chriskempson.com/projects/base16/) is a system of 16 hexadecimal numbers mapped to colors. Designers can choose different colors to create a new palette. Each number is mapped onto a number of syntactical constructs familiar from programming languages. For example: 

```
base08 - Variables, XML Tags, Markup Link Text, Markup Lists, Diff Deleted
```

For Textmate, a mapping has been designed from the scope naming conventions to base16 variables. This makes it possible to construct a theme from a base16 color scheme. For example:

```
base08
    - variable
    - entity.name.tag
    - string.other.link, punctuation.definition.string.end.markdown, punctuation.definition.string.begin.markdown
    - markup.list
    - markup.deleted
    - invalid.illegal
```

Consequently, a tokenizer that assigns these scope names combines well with such themes. For example, a rule that assigns scope name `markup.list` to a string, will cause it to have color base08 - whatever actual color that may be in a particular theme.

## Base16 for Perspectives ARC
We aim to make use of base16. This will give the modeller a wide range of well-known themes to choose from when writing an ARC file. To this aim we map ARC constructs as well as we can to the base16 categories. We then apply the Textmate-base16 mapping in reverse to choose Textmate scope names to use in the rules for the Textmate ARC tokenizer.

**NOTE** It so happens that not all base16 themes map the sixteen color variable names to sixteen different colors. Consequently, in some themes, the difference between some constructs do not show up. Reasonable themes are the Monokai themes in the [Base16 Themes](https://marketplace.visualstudio.com/items?itemName=AndrsDC.base16-themes) by AndrsDC. 

## The Textmate-base16 mapping
The table below has been derived from the template file [default.moustache](https://github.com/chriskempson/base16-textmate/blob/master/templates/default.mustache) in the Base16 repository on Github.

|Base16 color variable|Textmate name|Textmate scope names|
|---|---|---|
|base02|Separator|meta-separator|
|base03|Comments|comment, punctuation.definition.comment|
||Unimplemented|invalid.unimplemented||
|base04|selectionForeground| `a gutterSettings key`|
|base05|Text|variable.parameter.function
||Punctuation|punctuation.definition.string, punctuation.definition.variable, punctuation.definition.string, punctuation.definition.parameters, punctuation.definition.string, punctuation.definition.array|
||Delimiters||
||Operators|keyword.operator|
||Separator|meta.separator|
|base06|||
|base07|Classes|meta.class|
||Illegal|invalid.illegal (`foreground`)|
||Depracated|invalid.deprecated (`foreground`)|
||Unimplemented|invalid.unimplemented|
|base08|Illegal|invalid.illegal (`background`)|
||Variables|variable|
||Tags|entity.name.tag|
||Link text|string.other.link, punctuation.definition.string.end.markdown, punctuation.definition.string.begin.markdown|
||Lists|markup.list|
||Deleted|markup.deleted|
||Illegal|invalid.illegal (`background`)|
|base09|Integers|constant.numeric|
||Floats||
||Boolean||
||Constants|constant|
||Attributes|entity.other.attribute-name|
||Values||
||Units|keyword.other.unit|
||Link url|meta.link|
||Quotes|markup.quote|
||Broken|invalid.broken|
|base0A|Bold|markup.bold, punctuation.definition.bold|
||Classes|support.class, entity.name.class, entity.name.type.class|
|base0B|Strings, Inherited Class|string, constant.other.symbol, entity.other.inherited-class|
||Code|markup.raw.inline|
||Inserted|markup.inserted|
|base0C|Support|support.function|
||Colors|constant.other.color|
||Regular expressions|string.regexp|
||Escape charactes|constant.character.escape|
|base0D|Headings|markup.heading punctuation.definition.heading, entity.name.section|
||Methods|keyword.other.special-method|
||Attribute Ids|entity.other.attribute-name.id, punctuation.definition.entity|
||Headings|markup.heading punctuation.definition.heading, entity.name.section|
|base0E|Keywords|keyword|
||Storage|storage|
||Selector|meta.selector|
||Italic|markup.italic, punctuation.definition.italic|
||Changed|markup.changed|
||Embedded|punctuation.section.embedded, variable.interpolation|
|base0F|Labels|entity.name.label|
||Deprecated|invalid.deprecated (`background`)|

## ARC categories
The table below gives a grouping of ARC keywords. The TOKENIZER RULE defines semantically appropriate groups of keywords as they are lumped together by our tokenizer for ARC. These are lumped together in even larger groups (under COLOR GROUP), where a Base64 color is assigned to each COLOR GROUP. In other words, the table gives the mapping from tokenizer rules to Base16 colors.

|COLOR GROUP|TOKENIZER RULE|ARC KEYWORDS|BASE64|
|---|---|---|---|
|Contexts|Context kinds|domain, case, party, activity|base0A|
|Roles|Role kinds|external, thing, context|base0B|
|||extern ||
|||View||
|||filledBy||
|User role|User role|user|base03|
|Property|Property|property|base08|
||Property facet|mandatory, relational, unlinked, minLength, maxLength, enumeration, pattern, maxInclusive, minInclusive||
|Perspective|Perspective|view, verbs, props, only, except, defaults, all roleverbs|base0D|
|Control|State|state|base0E|
||State transition|on entry [of <statekind>], on exit [of <statekind>]||
||Notification|notify [user]||
||Automatic action|do [for <user>]||
||Assignment|remove, delete, create, etc.||
|||action||
|Simple values|Boolean|true, false|base09|
||date|||
||number|||
||regular expression|||
||Property Range|String, Boolean, DateTime, Number||
|Expressions|Operators|either, both, binds, matches, and, or, not, exists, available, boundBy, binder, context|base0C|
|||filter…with||
|||>>=, >>, *, /, +, -, ==, >=, <, >=, >||
||Let|letA, letE||
|Variables|Standard variables|currentcontext, nofifieduser, origin, currentactor|base06|
|Verbs|Role verbs|Remove, Delete, Create, CreateAndFill, Fill, Unbind, RemoveFiller, Move|base0F|
||Property verbs|RemovePropertyValue, DeleteProperty, AddPropertyValue, SetPropertyValue, Consult||
|Meta|Meta|aspect, use, indexed|base05|

## Tokenizer scope names
Finally, we can assemble the table that gives each of our tokenizer rules (the entries under "patterns" in the `arc.tmLanguage.json` file) a value for its "name" property (taken from the default scope names). Note that there is considerable arbitrariness in this assignment, as we link on Base16 color variable names and these appear in more than one Textmate scope name.

|TOKENIZER RULE|BASE64|TEXTMATE SCOPE NAME|
|---|---|---|
|Context kinds|base0A|entity.name.class|
|Role kinds|base0B|entity.other.inherited-class|
|User role|base03|comment|
|Property|base08|entity.name.tag|
|Property facet|base08|entity.name.tag|
|Perspective|base0D|keyword.other.special-method|
|State|base0E|keyword|
|State transition|base0E|keyword|
|Notification|base0E|keyword|
|Automatic action|base0E|keyword|
|Assignment|base0E|keyword|
|Boolean|base09|constant|
|Date|base09|constant|
|Number|base09|constant|
|Property Range|base09|constant|
|Regular expression|base09|constant|
|Operators|base0C|support.function|
|Let|base0C|support.function|
|Standard Variables|base03|comment|
|Role verbs|base0F|entity.name.label|
|Property Verbs|base0F|entity.name.label|
|Meta|base05|keyword.operator|

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

**NOTE**: copy development results to the repo for the [Atom Perspectives tokenizer](https://github.com/atom/language-perspectives-arc). Copy the contents of the `arc.tmLanguage.json` file into the `perspectives-arc.json` file in `grammars`.

## Package for distribution
Apart from publishing this extension on a marketplace (which we do not cover here), it is possible to package the extension. It can then be distributed, e.g. from github. The instructions here are taken from [microsoft](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#packaging-extensions).

Package with this command:

```
vsce package
```

## Releasing
Follow these steps:
* set the new version number in the package file.
* run vsce package again to produce a new .vsix file (it will have the right version number).
* push all changes to github.
* create a new tag, use the semantic version number preceded by 'v', e.g. `v0.1.0`.
* push the tag on the command line (vscode won't do it):

```
git push origin <tagname>
```

* on Github: create a new release using that tag.
* add the `language-perspectives-arc-X.Y.Z.vsix` to it.

