# Processing BED files (Part 1)

In this, and the next, project we will look at [BED files](https://en.wikipedia.org/wiki/BED_(file_format)), or at least a slightly simplified version of them. BED files are a textual file format developed for storing genomic information in a form that is readable for both humans and computers, and while it is on the simpler side of file formats, it resembles other formats you are likely to work with in your future career. The two projects we will do with BED files will show you have a little bit of computational thinking will help you when working with this kind of data.

You can read the full specification for BED files on the Wikipedia page linked to above, but we will restrict ourself to a subset of the format (that is still valid BED, however).

Full BED files can contain a header, which we will not allow, and then between 3 and 12 space separated (3 mandatory and 9 optional) columns. When it comes to such tabular data, some file formats are very restrictive, requiring space separation (a single space) or tab separated (a single tab character) between columns. BED allows both, but does specify that tab is preferable. What this means is that some tools will not accept anything else, making tab-separation the de facto standard. Bioinformatics is full of such crap. 

We will not have header-data and we will ignore the kind of data a file can contain beyond genomic locations and a feature name. For us, a BED file consists of four space separated columns, with the interpretation

```
1: chrom      -- chromosome name
2: chromStart -- chromosomal position (zero indexed)
3: chromEnd   -- chromosomal position (zero indexed)
4: name       -- the name of this line (think of it as a feature name)
```

For columns two and three, zero indexed means that we index into the genome the same way we do into Python lists: the first index is zero. There is absolutely no consistency in the file formats used in bioinformatics when it comes to indexing from zero or one, and you have to check it every time you get data in a new format, but for BED files we index from zero.

Since BED files are allowed to have either space or tabs between columns, we will allow the same, but we will only allow that for input. If we output BED data, we hold ourselves to higher standards and always use a single tab between columns. Be liberal in how you read input and conservative in how you output is a good way to code, and it will make your life easier if you get used to that early.

You should interpret a row such as

```
chrFoo  200 250 qux
```

as saying that from position 200 to 250 chromosome `chrFoo` has feature `qux`. For intervals, BED files include the start index and exclude the end index, the same way that Python does (and no, there is no consistency in what formats do with intervals either). So if you have a chromsome and the interval `start` to `end`, think `chrom[start:end]`. BED files work a lot like Python here (and it isn't an accident that I chose BED for these projects).

We will make one more simplifying assumption about our data: all the intervals are a single nucleoptide, so `chromEnd = chromStart + 1`. This simplifies the exercises we have to do (but who knows, maybe we will remove this simplification in future exercises?).

## Reading BED files

The first thing we need to do is write code such that we can read a BED file. By itself that isn't much use, but we might be in a situation where we have BED files output by some fool program that uses mixed spaces and tabs, and we need that data in a tool that only reads tabs. While it might not be the most straightforward solution to the problem, there are existing tools that can solve this for us, if we do have a tool that can read in BED data and then write it out again, we certainly do have a solution. So, we will write such a tool; then once we have the code for that, we can start doing more useful things with it.

I've already done most of the work for you. In the module `bed` I have put two functions, `parse_line()` and `print_line()` that parses a single line of BED information and prints it to a file, respectively. In the file `format_bed.py` I have written most of a tool for solving this exercise; you just need to fill in the final few details.

(In this file, notice that I have changed how we parse options compared to the previous project. In the last project, you saw the basic way processes work with options, but different languages and different environmments typically have better solutions. The `argparse` module is one of the prefered ways to handle options in Python, and you might want to use that for your own projects).
