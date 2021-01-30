---
layout: default
---

# Welcome

Dear visitor,

Welcome to my personal coding page. I support open source software, especially for scientific and research purposes). The **applications** developed by me that are in a finished stage and hence also made open-sourced are (in order of finishing dates for v1.0.0):

1.  MAX IV soft RF sweeper (*done*)
2.  DynaGUI (*done*)
3.  SPMTUI (*done*)
4.  A Shift Scheduling tool (*beta version ready, final version in progress*)

# MAX IV soft RF sweeper
A very simple tool for slowly changing a storage ring's master oscillator's radio-frequency (RF). The steps in the application are small enough for the storage ring's orbit feedback to adjust the orbit such that sensitive beamlines do not register a disturbing fluctuation (in terms of beam intensity and position). Using this tool, the user can define how much the ring's RF is to be changed. The application was part of a publication, see link below.

[Link to Publication](https://www.mdpi.com/2410-390X/4/3/26)
[Link to Application](https://github.com/benjaminbolling/MAXIVsoftRFsweeper)

# DynaGUI
DynaGUI stands for Dynamic Graphical User Interface and is a method to construct temporary, permanent and/or a set of GUI:s for users in a simple and fast manner. Developed during shift works at a particle accelerator, the initial goal was to fill in some functions that were then missing: Fast dynamic construction of new control system GUI:s for various purposes. The code is fully built in Python.

[Link to DynaGUI](https://joss.theoj.org/papers/10.21105/joss.01942)

# SPMTUI
SPMTUI stands for Simple Project Management Text-based User Interface and was made for myself for logging and keeping track of tasks and projects to do, in progress, and completed. The code is fully built in Python. SPMTUI functionalities includes:
- Commands with tab-completion
- Colour-coded states of each task
- Description of each task
- Logbook tracking of task initiation and changes (both manual and automatic entries)

[Link to SPMTUI](https://github.com/benjaminbolling/SPMTUI)

# Shift Scheduling tool
A Computational Approach to Generate Multi-Shift Rotational Workforce Schedules.

Phase 1: The algorithm takes into account a list of inputs (constraints) and returns all possible solutions. The schedule maker can then select the most feasible solution(s) to proceed with.

Phase 2: A feasible solution was selected and is constructed to its final shape, and is then ready for exportation (in .txt or .CSV format).

The source code will be made fully open source once the code is in a fully ready shape.

[RSWalgo (Unix Executable)](https://github.com/benjaminbolling/RSWdownload/raw/master/RSWalgo)

[RSWalgo documentation](https://arxiv.org/abs/2009.05615)

### First time instructions for MAC and Linux users in terminal for running RSWalgo:
```
cd /FolderWithRSWAlgo

chmod +x ./RSWalgo

./RSWalgo
```
Note that the last command may require sudo privileges (sudo ./RSWalgo).

### Not first time instructions for MAC and Linux users in terminal for running RSWalgo:
```
cd /FolderWithRSWAlgo

./RSWalgo
```

# Contact
For information about me, check out my ORCID below. If you have any requests or questions, drop me an email.

Email address: benjaminbolling@icloud.com

<div style="color:FFFFFF; margin-left: 0%; margin-right: 0%; text-align:left" itemscope itemtype="https://schema.org/Person"><a style="color:78D160" itemprop="sameAs" content="https://orcid.org/0000-0002-6650-5365" href="https://orcid.org/0000-0002-6650-5365" target="orcid.widget" rel="me noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon">https://orcid.org/0000-0002-6650-5365</a></div>
