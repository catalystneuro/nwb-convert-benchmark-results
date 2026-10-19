# nwb-convert benchmark results

Full results behind the evaluation on our SfN 2026 poster (PSTR339.15, "Conversational AI agents for converting neurophysiology data to NWB and reanalyzing the DANDI Archive").

We repeated one of our past conversions, the Lerner Lab's photometry and optogenetics data from Seiler et al. (2022), and graded each run against our published conversion, [DANDI:000971](https://dandiarchive.org/dandiset/000971) (152 subjects, 3,986 sessions). Claude Code ran in a fresh Docker container per run, with no route to GitHub or the DANDI Archive, and uploaded its conversion to the DANDI sandbox. The skill arms had the [nwb-convert skill](https://github.com/catalystneuro/claude-skills/tree/main/nwb-convert); the vanilla arms had Claude Code alone. A simulated lab scientist (Claude Opus 5.5 with the lab's fact sheet) answered only what the agent asked. Each arm ran once, from the same opening prompt.

Agent models: Claude Opus 4.5 (`claude-opus-4-5-20251101`) and Claude Opus 5.5 (`claude-opus-5-5`). Two reference sessions where our conversion used another mouse's run are left out of every row.

## Coverage

Metadata items is the mean, over the 49 items the reference fills, of the share of matched sessions where the run fills the item; the items are listed under Metadata items below.

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Subjects (of 152) | 55% | 55% | 99% | 100% |
| Sessions (of 3,986) | 19% | 40% | 99% | 99% |
| Photometry stream (of 63 sessions) | 83% | 98% | 97% | 100% |
| Behavior stream (of 3,986 sessions) | 19% | 40% | 99% | 99% |
| Optogenetics stream (of 2,120 sessions) | 0% | 0% | 99% | 99% |
| Western blot images (of 16) | 0% | 100% | 0% | 100% |
| Metadata items (of 49) | 48% | 53% | 92% | 98% |

## Data fidelity

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Photometry samples identical to reference | 100% | 100% | 100% | 100% |
| Behavior event times match reference | 88.3% | 97.3% | 99.5% | 100% |
| Rewards at nosepoke times | 88% | 100% | 100% | 100% |
| Paired rewards at light onsets | 53% | 100% | 100% | 100% |
| Omission rewards not at nosepokes | 100% | 100% | 99% | 100% |
| Synchronization error, median | 0 ms | 0 ms | 14 ms | 13 ms |
| Photometry chunked and compressed | 0% | 100% | 100% | 100% |

## NWB Inspector (DANDI configuration)

Distinct issues: Inspector messages with the same check, neurodata type and object path count once, however many files they appear in.

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Critical issues | 0 | 0 | 0 | 0 |
| Best-practice violations | 4 | 1 | 0 | 0 |
| &nbsp;&nbsp;&nbsp;&nbsp;Timestamps not ascending | 3 | 0 | 0 | 0 |
| &nbsp;&nbsp;&nbsp;&nbsp;Regular timestamps stored explicitly | 1 | 1 | 0 | 0 |
| Best-practice suggestions | 0 | 3 | 2 | 7 |
| &nbsp;&nbsp;&nbsp;&nbsp;Missing description | 0 | 2 | 0 | 0 |
| &nbsp;&nbsp;&nbsp;&nbsp;Single-row table | 0 | 1 | 2 | 5 |
| &nbsp;&nbsp;&nbsp;&nbsp;All-NaN column | 0 | 0 | 0 | 1 |
| &nbsp;&nbsp;&nbsp;&nbsp;Non-standard processing module name | 0 | 0 | 0 | 1 |
| Duplicate object identifiers | 0 | 69 | 0 | 0 |

## Edge cases

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Should be converted: No subject ID, MED-PC file in mouse folder | 20/575 | 221/575 | 575/575 | 573/575 |
| Should be converted: Photometry restarted mid-session | 0/3 | 0/3 | 0/3 | 3/3 |
| Should be converted: Mouse ran the wrong program first | 2/2 | 2/2 | 2/2 | 2/2 |
| Should be skipped: MED-PC run in the wrong mouse’s file | 3/3 | 0/3 | 3/3 | 3/3 |
| Should be skipped: Mice missing from the demographics sheet | 5/15 | 12/15 | 15/15 | 15/15 |
| Should be skipped: No subject ID, MED-PC file sorted by date | 11/11 | 11/11 | 11/11 | 11/11 |
| Should be skipped: Corrupted photometry | 1/2 | 0/2 | 0/2 | 0/2 |

## Neurodata types

Share of what each run converted that is stored in the preferred type (NWB schema 2.11.0, ndx-fiber-photometry 0.2.3, ndx-optogenetics 0.4.0).

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Photometry in FiberPhotometryResponseSeries | 100% | 100% | 100% | 100% |
| Behavior in EventsTable | 0% | 4% | 100% | 100% |
| Optogenetics in ndx-optogenetics tables | – | – | 0% | 100% |
| Photometry metadata in preferred types | 100% | 0% | 100% | 100% |
| Optogenetics metadata in preferred types | – | – | 0% | 100% |

## Ontology identifiers

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Terms with a correct identifier (of 11) | 0% | 0% | 0% | 91% |
| &nbsp;&nbsp;&nbsp;&nbsp;Species (NCBITaxon) | 0/1 | 0/1 | 0/1 | 1/1 |
| &nbsp;&nbsp;&nbsp;&nbsp;Strain (RRID) | 0/1 | 0/1 | 0/1 | 1/1 |
| &nbsp;&nbsp;&nbsp;&nbsp;Institution (ROR) | 0/1 | 0/1 | 0/1 | 1/1 |
| &nbsp;&nbsp;&nbsp;&nbsp;Experimenters (ORCID) | 0/6 | 0/6 | 0/6 | 6/6 |
| &nbsp;&nbsp;&nbsp;&nbsp;Brain regions (Allen atlas, UBERON) | 0/2 | 0/2 | 0/2 | 1/2 |

## Dandiset metadata

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Items matching our Dandiset (of 25) | 12% | 12% | 24% | 92% |
| &nbsp;&nbsp;&nbsp;&nbsp;Authors, in order, with ORCID and affiliation | 0/8 | 0/8 | 1/8 | 8/8 |
| &nbsp;&nbsp;&nbsp;&nbsp;Data collectors | 0/4 | 0/4 | 0/4 | 4/4 |
| &nbsp;&nbsp;&nbsp;&nbsp;Funders with award numbers | 0/3 | 0/3 | 0/3 | 3/3 |
| &nbsp;&nbsp;&nbsp;&nbsp;Anatomy and disorder terms | 0/3 | 0/3 | 0/3 | 2/3 |
| &nbsp;&nbsp;&nbsp;&nbsp;Related links, license and text fields | 3/7 | 3/7 | 5/7 | 6/7 |

## Metadata items

Share of matched sessions where the run fills each item the reference fills. A dash means the run matched no reference session with the item.

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| subject: sex | 100% | 100% | 100% | 100% |
| subject: species | 100% | 100% | 100% | 100% |
| subject: age | 100% | 100% | 100% | 100% |
| subject: genotype | 0% | 100% | 100% | 100% |
| subject: strain | 0% | 100% | 100% | 100% |
| subject: description | 100% | 100% | 100% | 100% |
| session: session_description | 100% | 100% | 100% | 100% |
| session: experimenter | 100% | 100% | 100% | 100% |
| session: institution | 100% | 100% | 100% | 100% |
| session: lab | 100% | 100% | 100% | 100% |
| session: keywords | 100% | 100% | 100% | 100% |
| session: related_publications | 100% | 100% | 100% | 100% |
| session: experiment_description | 100% | 100% | 100% | 100% |
| session: surgery | 0% | 0% | 100% | 100% |
| session: virus | 0% | 0% | 100% | 0% |
| indicator (jGCaMP7b) | 100% | 100% | 100% | 100% |
| virus (AAV5-CAG-FLEX-jGCaMP7b) | 0% | 100% | 100% | 100% |
| injection sites (medial and lateral SNc) | 0% | 0% | 100% | 100% |
| fiber locations (DMS and DLS) | 100% | 100% | 100% | 100% |
| injection coordinates (AP -3.1; ML 0.8 / 1.3; DV -4.7 / -4.2) | 0% | 0% | 100% | 100% |
| fiber coordinates (DMS AP 0.8 ML 1.5 DV -2.8; DLS AP -0.1 ML 2.8 DV -3.5) | 0% | 0% | 100% | 100% |
| fiber core diameter (400 um) | 0% | 0% | 100% | 100% |
| fiber numerical aperture (0.48) | 0% | 0% | 100% | 100% |
| excitation wavelengths (465 and 405 nm) | 0% | 0% | 97% | 100% |
| emission wavelength (about 525 nm) | 83% | 0% | 97% | 100% |
| photodetector (Newport femtowatt photoreceiver) | 83% | 0% | 0% | 100% |
| LED modulation frequencies (211/330/450/270 Hz) | 0% | 0% | 97% | 100% |
| filters and dichroic (Doric FMC4 minicube) | 83% | 0% | 0% | 100% |
| effector (ChR2) | 100% | 100% | 100% | 100% |
| effector (NpHR) | 100% | – | 100% | 100% |
| effector (EYFP control) | 100% | 100% | 100% | 100% |
| light wavelength (460 nm) | 0% | 0% | 100% | 100% |
| light wavelength (625 nm) | 0% | – | 100% | 100% |
| light power (15 mW) | 0% | 0% | 100% | 100% |
| pulse width (5 ms) | 0% | 0% | 0% | 100% |
| pulse frequency (20 Hz) | 0% | 0% | 100% | 100% |
| train duration (1 s) | 0% | 100% | 100% | 100% |
| stimulation site (DMS) | 100% | 100% | 100% | 100% |
| stimulation site (DLS) | 98% | 100% | 100% | 100% |
| stimulation coordinates (DMS AP 0.8 ML 1.5 DV -2.8) | 0% | 0% | 100% | 100% |
| stimulation coordinates (DLS AP -0.1 ML 2.8 DV -3.5) | 0% | 0% | 100% | 99% |
| virus injection site (SNc) | 100% | 100% | 100% | 100% |
| virus injection coordinates (AP -3.1 ML 0.8 DV -4.7, medial SNc) | 0% | 0% | 100% | 100% |
| virus injection coordinates (AP -3.1 ML 1.3 DV -4.2, lateral SNc) | 0% | 0% | 100% | 99% |
| virus (ChR2, NpHR or EYFP construct) | 0% | 100% | 100% | 100% |
| light source (Prizmatix) | 0% | 0% | 100% | 100% |
| MED-PC program | 100% | 100% | 100% | 100% |
| box number | 0% | 0% | 0% | 100% |
| event descriptions (what nosepoke, port entry and reward events mean) | 99% | 100% | 100% | 100% |

## Example notebook

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Notebook written | no | no | no | yes |

## Session checks

A mangled subject ID reads as the same number (121.28 for 121.280) or keeps only the part before the dot (239 for 239.388). These sessions count as missing under Coverage.

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Start time within 1 s of the reference | 0.0% | 95.9% | 98.4% | 98.4% |
| Start time more than 5 min off | 96.8% | 2.0% | 0.7% | 0.7% |
| Of those, a wrong UTC offset | 243 | 0 | 0 | 0 |
| Missing sessions converted under a mangled subject ID | 153 | 542 | 0 | 0 |
| Sessions written to more than one file | 17 | 71 | 2 | 3 |

## Metadata values (share of files)

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Paper DOI in related_publications | 0.0% | 100.0% | 100.0% | 100.0% |
| All seven authors, in order, as experimenters | 0.0% | 100.0% | 100.0% | 100.0% |
| Subject age at least 10 weeks | 0.0% | 100.0% | 100.0% | 100.0% |

## Process

| | Opus 4.5 vanilla | Opus 4.5 skill | Opus 5.5 vanilla | Opus 5.5 skill |
|---|---|---|---|---|
| Messages asking the scientist | 0 | 3 | 2 | 7 |
| Conversion time | 0.6 h | 1.4 h | 0.6 h | 2.3 h |
| Upload time (DANDI sandbox) | 3.6 h | 3.2 h | 3.6 h | 4.8 h |
| Input tokens | 24.6 M | 24.0 M | 31.5 M | 99.0 M |
| Output tokens | 78 k | 94 k | 124 k | 314 k |
| Lines of Python written | 1,623 | 1,228 | 1,515 | 4,511 |
