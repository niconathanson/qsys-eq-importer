# Changelog

## 0.1.0 - 2026-07-27

First public release.

- Imports REW "Filter Settings" exports and `Type,Freq,Gain,Q` CSV, auto-detected.
- Both parsers split on record-start patterns rather than newlines, so a paste that
  Q-Sys flattens onto a single line still parses correctly.
- Converts Q to bandwidth in octaves using the RBJ relationship, checked against the
  block's own defaults (1.00 octave ↔ Q 1.41421365).
- Discovers EQ blocks by inspecting components for per-band controls, so renamed or
  unusual blocks are still found.
- Dry Run prints the full band-by-band table without writing anything.
- After writing, every band is read back and compared against the file; a fresh
  component handle re-reads band 1 to confirm values reached the DSP.
- If the block exposes both `q.factor` and `bandwidth`, whichever one it treats as
  derived is detected automatically and the write is retried through the live one.
- Refuses to write when the file needs more bands than the block has, naming the number
  to set - band count is a design property no script can change.
- High-pass, low-pass, notch and all-pass filters are reported and skipped rather than
  approximated.
- Shelves with no Q specified default to 0.707 (RBJ S=1), matching the FIREQ convention.
- Unused bands are flattened to 0 dB so nothing survives from a previous import.
