# Writing Letters in Markdown Using Pandoc and KOMA-Script

This template extends [Pandoc]'s original LaTeX-template by parsing variables and putting them into the appropriate [KOMA-script] `scrlttr2` variables.

## Changelog

- **2016-02-11**: Merged changes to Pandoc's `default.latex` template to fetch up with changes in Pandoc. Use [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) language codes instead of the old bable names now, for example `en` or `en-US` instead of `english`, `de-DE` instead of `ngerman`.
- **2015-12-30**: To and from address fields are now standard multiline markdown fields instead of lists, which is easier to read and handle and potentially available for all variables.

## Requirements, Installation and Usage

Obviously, you need Pandoc, a LaTeX distribution of your choice and KOMA-script installed. In Debian-based distributions, you can install both using `apt-get install texlive pandoc` (KOMA-script is included in texlive by default). For other operating systems, read the respective installation manuals.

### Installing the Template

To install the template `scrlttr2.latex`, either store it in the working directory or move it to the template folder in Pandoc's [data directory][general options], usually `~/.pandoc/templates`.

### Manual Typesetting

To typeset a letter, run Pandoc with a `--template scrlttr2` parameter, and either using the PDF or LaTeX writer. For creating a printable PDF file, run

    pandoc --template scrlttr2 -o example-letter.pdf example-letter.md

### Using the Wrapper Script

To make typesetting of letters more convenient, a wrapper script is included, which takes care of the most common use case: typesetting some `example-letter.md`, which is stored as `example-letter.pdf`.

To install the wrapper script `panletter`, copy or link it into your `$PATH`. The usual system-wide location would be `/usr/local/bin`, you can of course also choose any other location or reference the script directly. The basic usage is as easy as

    pandletter example-letter.md

For more details, view `panletter --help`.

## Letter Metadata in a YAML Metadata Block

There are two ways for setting those variables, either by passing them as [`pandoc` command line arguments][writer options] or storing them in a [YAML metadata block]. YAML also allows multi-line string values, which are introduced by a pipe symbol `|`. As the value is interpreted as markdown again, you might be required to add two spaces to enforce line wraps, for example in multiline address field. This also works great for the `fromaddress`, `opening`, `closing` and possibly other variables.

An example YAML metadata block:

```yaml
---
letteroption:
- DIN         # typeset following DIN norm
- example     # loads example style file example.lco
to: |         # required, YAML multiline value with double space linebreaks
  Maurice Moss  
  Reynholm Industries  
  123 Carenden Road  
  LONDON  
  EC5M 8AJ  
  GREAT BRITAIN
lang: english
subject: subject
opening: Dear Moss,
closing: Sincerely,
...
```

The only variable required by `scrlttr2` is `to`.

### KOMA-Script Variables

A bunch of KOMA-script variables for `scrlttr2` are exposed, especially all that reflect actual content (like recipient address, ...).  Variables not exposed are for example seperators.

Exposed variables, that can directly be used are `addresseeimage`, `backaddress`, `customer`, `date`, `fromaddress`, `fromaddress`, `frombank`, `fromemail`, `fromfax`, `fromlogo`, `frommobilephone`, `fromname`, `fromphone`, `fromurl`, `fromzipcode`, `invoice`, `location`, `myref`, `place`, `PPdatamatrix`, `PPcode`, `signature`, `specialmail`, `subject`, `title`, `toaddress`, `toname`, `yourmail` and `yourref`. For more details on their use, refer to the [KOMA-script manual].

### Template Variables

Some more variables can be set: `lang` (which actually is the default Pandoc language variable), `opening`, `closing` and `ps`. Latter three will be used for the `\opening{...}` and `\closing{...}` clauses in the letter. They get registered as KOMA-script variables by the template, and thus can also be set in a letter class option file. Such files can be load by setting `letteroption`.

## Letter Class Option Files

`scrlttr2` supports letter class files, which have two purposes: on one hand, they offer layout presets, eg. for following norms like the German [DIN 676] (in German language); on the other they can be used to define presets.

If you want to define your own address or letter template, create your own letter class option file. This may either be stored in the working directory, or at the recommended location [`~/texmf/tex/latex/`][texmf]. Read the [KOMA-script manual] for more details. For an example, refer to `example.lco`.

If you want to predefine opening and closing phrases, use the non-default `opening` and `closing` KOMA-script variables:

```latex
\setkomavar{opening}{Dear Sir or Madam,}
\setkomavar{closing}{Sincerely,}
```

You can also completely omit them, then the template will include empty `\opening{}` and `\closing{}` commands.

Everything you set as default in a custom letter class option file can later be overridden in the YAML metadata block. For example, you might have a default _"Dear Sir or Madam"_ opening in the option file, but can use a YAML block to change this to address somebody directly: `opening: Dear Moss` to address somebody directly.

## Copyright

This template, forked from the [pandoc-templates] is dual-licensed, under both the GPL (v2 or higher, same as pandoc) and the BSD 3-clause license (included below).

----

Copyright (c) 2014, John MacFarlane

Copyright (c) 2014, Jens Erat

All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

    * Redistributions of source code must retain the above copyright
      notice, this list of conditions and the following disclaimer.

    * Redistributions in binary form must reproduce the above
      copyright notice, this list of conditions and the following
      disclaimer in the documentation and/or other materials provided
      with the distribution.

    * Neither the name of John MacFarlane nor the names of other
      contributors may be used to endorse or promote products derived
      from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.



[Pandoc]: http://johnmacfarlane.net/pandoc/
[KOMA-script]: http://www.ctan.org/pkg/koma-script
[general options]: http://johnmacfarlane.net/pandoc/README.html#general-options
[writer options]: http://johnmacfarlane.net/pandoc/README.html#general-writer-options
[YAML metadata block]: http://johnmacfarlane.net/pandoc/README.html#extension-yaml_metadata_block
[KOMA-script manual]: http://ctan.mackichan.com/macros/latex/contrib/koma-script/doc/scrguien.pdf
[DIN 676]: http://de.wikipedia.org/wiki/DIN_676
[texmf]: http://tex.stackexchange.com/q/81710/11198
