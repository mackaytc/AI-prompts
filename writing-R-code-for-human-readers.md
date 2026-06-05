# R Style Guide: Writing R Code for Human Readers

***KEY GOAL: All code should be written as as to be human-friendly. Avoid lots of cat() and print() calls and generating intermediate summary output that may confuse or overwhelm a human reader.***

**Scope**

- These rules govern formatting and presentation. Apply them as you write new code—prioritize clarity and readability.
- As you think about what comments to include, and how verbose to be in your comments, assume that the audience/future reader of this code is relatively familiar with R, particularly tidyverse. 
  - Do NOT assume a high level of proficiency with base-R or data.table; you are welcome to use these tools as needed but provide more comments/guidance when doing so. 
  - NONE of the guidelines below are intended to constrain your choice of tool or analysis strategy; all guidelines below govern level of detail in comments and approach to code writing (not code content per se). 
- Please follow the "General Code File Template" below for creating new files.
  - All new files should have a header that explains what they do as well as listing inputs/outputs.

**Line width**

- Hard wrap at 80 characters.
- Break long pipes, function calls, and mutate()/summarise() blocks across lines with one argument per line.
- Indent continuation lines consistently (2 spaces under the opening call).

**Comments**

- One-liner comments throughout the code are welcome only if they help the reader understand what is going on over and above the actual code itself. 
  - Comments are not needed for obvious steps. For example, print() or filter() commands do not require a comment (unless the filter is non-obvious for who it is selecting). 
- Reserve longer comments for: non-obvious data structure (panel keys, unit of observation, what one row represents); transformation points where the data shape changes (reshapes, collapses, merges that drop or duplicate rows); and assumptions or gotchas (e.g. "balanced panel required here," "NAs are intentional").
- One comment per logical block, not per line. Keep them sparse.

**Avoid these AI-isms**

- Redundant intermediate objects named like df.step1, df.step2, temp. Either chain operations in a pipe or give objects meaningful names.
  - Only create intermediate objects when doing so either 1) facilitates ease of reading the code or 2) facilitates running the code interactively. 
  - For example, loading a large data set may be very slow. You should separate this load step from later processing/subsetting steps which may run faster, to allow the user to run code interactively without having to re-run the load step each time.
- Defensive boilerplate that never triggers (excessive if (!exists()), repeated suppressWarnings(), wrapping everything in tryCatch) unless it serves a real purpose.
  - In general, do not worry about package load issues ex ante; try running the code, and install the package if something breaks. Don't be too defensive; the user will add packages as needed for analysis.
- Verbose print()/cat() progress chatter.

**Structure**

- Add a one-line header at each stage explaining what we're doing.
- Keep related operations visually together; separate distinct stages with a single blank line.
- Spell out the unit of observation and panel structure once, near the top, in a short comment block.

**Keep the focus on readability**

- These are presentation rules. Don't let them justify convoluted logic, unnecessary reordering of computation, or premature optimization.
- Write code that's clear and direct.

## General Code File Template

```markdown
Every script other than the project's setup file should follow the structure
below. The setup file (e.g. 00-START-HERE.R) is sourced first and defines
directory/path variables and loads packages; downstream scripts inherit that
environment rather than redefining it. 

Three elements are required.

1. A header block at the top, framed by full-width `#` rules, containing in
   order:
   - FILE: the filename
   - OVERVIEW: what the script does, in a few sentences
   - INPUTS: files or objects the script consumes (list each; use N/A if none)
   - OUTPUTS: files or objects the script produces (list each)
   - AUTHOR: name and contact -- list Taylor Mackay (tmackay@fullerton.edu) 

2. A "Getting Started" section immediately after the header that sources the
   setup file. This is the only setup a downstream script does — it does not
   re-load packages or redefine paths:

   source(paste0(MXD.code, "00-START-HERE.R"))

3. An end-of-file block as the final lines:

################################################################################
# End of File
################################################################################

Within the body, separate major stages with full-width `#` rules and a short
title (e.g. "Load and Clean CFO Data," "Save Output"). Reference paths through
the variables defined in the setup file (MXD.raw.data, MXD.pro.data, etc.)
rather than hardcoding them.

Example skeleton:

################################################################################
#
# FILE: 01-data-cleaning-housing-sites.R
#
################################################################################

# OVERVIEW: This file processes LA City Certificate of Final Occupancy (CFO)
# data to identify mixed-use residential developments. It filters permits,
# applies manual quality checks, and maps sites to Census blocks to create
# block-by-quarter panel data.

# INPUTS:
#  - Building_and_Safety_Certificate_of_Occupancy_20251014.csv
#  - LA County 2020 Census block boundaries (via tigris package)

# OUTPUTS:
#  - LA-county-2020-census-blocks.rds (Census block shapefile)
#  - MXD-sites-to-Census-block-quarter-panel.rds (block-by-quarter panel)
#  - CFO-permit-level-data-cleaned.rds (cleaned permit-level data w/ block IDs)

# AUTHOR: Taylor Mackay (tmackay@fullerton.edu)

################################################################################
# Getting Started
################################################################################

# Run the general project housekeeping file that defines working space
# variables and loads required packages

source(paste0(MXD.code, "00-START-HERE.R"))

################################################################################
# [First processing stage]
################################################################################

# ... code ...

################################################################################
# End of File
################################################################################
```