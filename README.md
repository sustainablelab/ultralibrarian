This is a rant about UltraLibrarian from my point of view as an
EAGLE user. I thought I wouldn't have to create EAGLE `.lbr`
files anymore. But I still need to create my own EAGLE `.lbr`
files. And it's not about paid version vs free version. I think
the business model just prevents the vision from living up to its
potential.

I'm sure there's a use case where UltraLibrarian is a real
time-saver, but it's not my use case.

## UltraLibrarian shows me how to write a lbr scr

The most useful thing about UltraLibrarian is that it generates
an EAGLE `scr`. This `scr` creates the symbol, footprint, and
deviceset.

This is a good template or starting point. It is definitely
simpler than generating XML from scratch in Python.

[Frank Frank (click to see Frank's
projects)](https://www.ultralibrarian.com/franks-garage) is the
"architect" of UltraLibrarian. He seems like a reasonable person
who knows what he's doing. Unfortunately, the UltraLibrarian
business model is not compatible with people like me.

## UltraLibrarian lbr quality is hit or miss

This tool is not a replacement for making the lbr from scratch. I
say that as someone who hates making lbrs and would love it if
this tool actually lived up to its name.

*For passives, the lbr it generates is usable, though I still
prefer to go in and make manual changes to the exact pad size and
to add a `tKeepout` layer.*

I can't speak to other CAD packages, but for EAGLE, the lbr is
usually low-quality. Maybe this is because the free version has
too many limitations for EAGLE.

Here is a screenshot from the desktop tool listing the free
version limitations for EAGLE:

![UltraLibrarian desktop free version limits for
EAGLE](img/UltraLibrarian-free_version_limitations_for_EAGLE.PNG)

*As an aside, there is no motivation to use the desktop version.
It's a 2GB install (why?) and it doesn't do anything
different/better than the web version. Plus there is no way to
tell it "where" to put the `scr` it generates, so I have to dig
around in the install folder to find where my output was placed,
then move it to my project folder.*

The web tool is the "gold" version of UltraLibrarian ("gold" is
the expensive version meant for large organizations). Compare the
`.scr` file that are generated in the free desktop version versus
the `.scr` file generated in the web version.

This is the `.scr` header in the free desktop version:

```free-version-scr-header
# Created by Ultra Librarian Lite 8.3.301 Copyright © 1999-2021
# Ultra Librarian Free Reader, Accelerated-Designs Inc.
```

This is the `.scr` header in the web (gold) version:

```web-version-scr-header
# Created by Ultra Librarian Gold 8.3.307 Copyright © 1999-2021
# Frank Frank, Accelerated Designs
```

Looking at the rest of the file, the web (gold) version of the
`scr` does contain a lot more stuff. The free version `scr` for
`LT3063` is 295 lines, and the gold version `scr` is 556 lines.

So the stuff you get from the UltraLibrarian web tool does not
have the same limitations, at least in theory.

But this extra stuff is mostly noise from my point of view. It's
all on layers that I turn off. I think it's there to help the
user pick which package footprint they want to use or to see if
they have the right part when they are searching part numbers in
the web tool.

I tried UltraLibrarian for about ten parts, some ICs, some
passives. I have yet to see a lbr take advantage of the useful
"gold" features for EAGLE.

For example, if multiple pads connect to the same net, give me a
symbol that has one pin with that net name, and give me a
footprint that uses the EAGLE `@` convention (`GND@8`, `GND@9`)
and connects those physical pads to the same symbol pin.

Would I get this stuff if I paid for the tool? Probably not. At
best, I get a version of the desktop tool where I have some way
to modify what it does before I generate the EAGLE `scr`.
See the [UltraLibrarian standards (click to open page where following
quote is from)](https://www.ultralibrarian.com/about/standards):

> Our symbols are built based on the ANSI Y32.2-1975 (Reaffirmed
> 1989) standard. By default, symbols are entered numerically by
> pin number, not arranged by function. However, users can
> auto-partition using the Online Reader or the desktop software.
> Users using the Ultra Librarian Silver and Gold software have
> access to easy to use, manual pin arrangement tools if a unique
> presentation is required.

So now I've moved the GUI for defining the lbr from EAGLE (which
I paid for) to UltraLibrarian (which I also have to pay far).
Yay? Boo. The promise was I wouldn't have to do any work to
generate a lbr. Instead, I have a second tool I have to purchase
(which is *more expensive* than EAGLE!) and learn to use. Clearly
the tool addresses some pain point that's off my radar as an
individual user working with a single electronics CAD tool.

### UltraLibrarian web tool

UltraLibrarian offers a web service that generates EAGLE lbr
scripts for just about any part.

![UltraLibrarian web version
screenshot](img/UltraLibrarian-web_version.PNG)

The example above is for `LT3063`. This is an example of a
low-quality `lbr`. Even worse it has mistakes.

Look at the package above, right-hand side. This `LT3063` package
is missing the exposed `GND` pad on the bottom. I put in the
right part number. I tried this with the web tool and the desktop
tool. Same problem. That's a serious error in the package artwork
that would prevent the PCB from being assembled.

Look at the symbol above, left-hand side. It is a literal
pin-for-pin representations of the package. I do not like this. I
think it's a level of detail that does not belong on the
schematic.

This is part of what I mean by low-quality. The manufacturer's
datasheet shows schematics of the IC in typical applications.
These schematics show the appropriate level of detail. Is it
really so difficult to use those datasheet schematic symbols to
define the symbol in the BXL? Fail.

Plus there's an error in the pin direction. `!SHTDN` is shown as
an output pin. It's an input pin.

EAGLE has many pin directions. The only purpose of these pin
directions is so the EAGLE ERC can catch schematic mistakes.
UltraLibrarian tries to be high-quality here. They *attempt* to
use specific EAGLE pin directions to leverage the ERC. But it
looks like there's no guarantee that UltraLibrarian gets it right?

Or, more likely, is that the BXL itself is not defined correctly.
But the BXL is from the manufacturer... Wasn't that the whole
point of the UltraLibrarian vision? This kind of mistake is not a
big deal, but it's a red flag to me that calls into question the
accuracy of all of the CAD data in the BXL. Worst case here, the
ERC throws an error, I realize what happened, then manually
correct the `lbr` symbol to remove the ERC error.

I think the missing GND pad on the package shows that the entire
premise of what UltraLibrarian is trying to do is just not
practical.

I think the problem is with the `.bxl` file, and not
UltraLibrarian software itself. But who makes the `.bxl` files
that the IC manufacturer's host on their website? From the
[UltraLibrarian website (click to view page with the following
quote)](https://www.ultralibrarian.com/2018/01/02/bxl-files-bring-crucial-cad-library-data-to-all-pcb-designers):

> You may have heard the term BXL before, either on our website
> or from IC vendors, but what does it mean? The term BXL stands
> for binary exchange language and represents a vendor-neutral
> binary library file format. BXL is our proprietary file format
> for vendor-neutral CAD data, created specifically to service
> PCB designers. These small files can be translated into a
> variety of different CAD formats and contain all the essential
> component data necessary to move smoothly from schematic design
> to production.

It's a nice idea. A single file format for presenting CAD data,
then generate any CAD-tool-specific files from that universal CAD
data file.

But even something as objective as an electronic component
library definition is hard to centralize, as the `LT3063` example
illustrates.

The BXL closed format is the real show stopper. An open file
format essentially defines a standard. A closed file format is
tied to the success/whim of a private entity.

An open file format means the community collaborates on fixing
mistakes (like in the `LT3063` example).

Also, a closed format prevents me from working directly from the
BXL. I'm forced to push the BXL through an UltraLibrarian program
before I can access the BXL data. That prevents me from using the
BXL format to write a tool that generates high-quality EAGLE
lbrs.

### UltraLibrarian work flow

Get the lbr from UltraLibrarian:

- get [LT3063 .bxl file from Analog Devices](https://www.analog.com/en/design-center/packaging-quality-symbols-footprints/symbols-and-footprints/LT3063.html)
- upload to [UltraLibrarian web tool](https://app.ultralibrarian.com/UploadBXL?open=exports&exports=EagleV6)
- if the `.bxl` is not directly available from the manufacturer's
  website, then [try searching
  UltraLibrarian](https://app.ultralibrarian.com/Search?queryText)
  with the manufacturer's part number
- UltraLibrarian generates an EAGLE scr that adds a symbol,
  footprint, and deviceset to the open `lbr`

Use the lbr from UltraLibrarian:

- create a new `lbr` (to test out what the UltraLibrarian scr
  generates) and save the `lbr` using the part name
  (e.g., `lt3063.lbr`)
- open the empty lbr and run the script
- the IC symbols are underwhelming and exactly what you'd expect
  from an automated tool
- the questionable bit is always the footprints
- `Ctrl+L` then `dis -41 -47` to turn off `tRestrict` and `Measures`
- prepare to be utterly underwhelmed by the package artwork

The question now is whether it's faster to use the UltraLibrarian
junk as a starting point or to just start from scratch.

