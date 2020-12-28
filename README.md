# Bars

Bars is a cli util that visualizes stdin input data as bar graphs / histograms.

## Installation

Grab the script and optionally symlink somewhere under `$PATH`.

```sh
$ git clone https://github.com/yjyao/bars.git && ln -s "$PWD/bars/bars" ~/.local/bin/bars
```

## Usage

Each line is an entry. Each entry has a number and optionally a label, separated by at least a white space. The output stretches across the `$width` which is by default 80.

Options are passed thru environment variables (which is bad, but I kinda wanted to keep it as a "one-liner"). Available options and defaults:
- **width**: Width of the graph (80)
- **symbol**: Symbol used to plot the bars (`+`)

## Examples

```sh
$ bars <<eof
7 Days in a week
30 Days in a month
365 Days in a year
eof
 Days in a week   7 |+
Days in a month  30 |+++++
 Days in a year 365 |++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```

Passing options as environment variables.

```sh
$ width=50 symbol='#' bars <<eof
7 Days in a week
30 Days in a month
eof
 Days in a week  7 |#######
Days in a month 30 |#################################
```

If the input data is out-of-format, you need to preprocess it.

```sh
$ awk <<eof -F: '{print $2, $1}' | bars
Days in a week: 7
Days in a month: 30
Days in a year: 365
eof
 Days in a week   7 |+
Days in a month  30 |+++++
 Days in a year 365 |++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```

## Why did you make this?

I happened to need a histogram for some data so I quickly came up with

```sh
$ ruby -aF: -ne 'puts "%9s: %5d |%s" % [$F[0], $F[1], "+"*($F[1].to_i/350)]' < datafile
```

Afterwards I wondered if I can make it smarter (without the magic numbers). Plus I'm currently learning the [Ruby one-liners cookbook](https://github.com/learnbyexample/learn_ruby_oneliners) and wanted to see where I can push my Ruby to.

## Known bugs

- Decimal numbers will be striped to integers

- If `width` is smaller than the legend, bars disappear

- If `symbol` consists of multiple characters, `width` is a lie
