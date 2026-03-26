# How To Use

## Features

### Codebook

Export a complete inventory of every clip in an Final Cut Pro event or project timeline as a tab-delimited file suitable for database import (e.g., FileMaker).

- Reads projects, sync-clips, multicam clips, and standalone event clips
- Outputs columns for Name, Tape, Camroll, Source TC In/Out, Duration, Record TC In/Out, FPS, Description, and Comments
- Automatically discovers and includes all custom metadata fields (Shot Notes, custom columns)
- Optional prefix stripping for column headers (e.g., remove `MBF_` from metadata field names)
- Optional CSV export alongside the primary TXT
- Live formatted preview with horizontal scrolling

---

### VFX Pull

Extract VFX clips from your timeline with handles, versioning, and role assignment. Produces an FCPXML that can be imported directly into Final Cut Pro.

**Extraction**
- Three identification modes: Marker Only, Role Only, or Marker & Role
- Marker type filters: All, Standard, Chapter, To-Do, or Completed
- Configurable frame handles (head and tail)
- Compound clip wrapping (optional)
- Effect preservation -- carries over video/audio filters, color corrections, and spatial transforms
- Recurses into compound clips and multicam clips to find nested VFX markers

**Naming and Versioning**
- Plate-type suffix from subrole auto-detection (e.g., `_MP01`, `_BG`, `_CP`) with a dropdown populated from the source FCPXML
- Change-aware version tracking: appends `_v001`, `_v002`, etc., incrementing only when timecode or source reel actually changes
- Per-project version history stored in Application Support, with controls to create, reveal, and delete version projects
- Live clip name preview showing the assembled name format

**Export**
- VFX Scan TXT sidecar with Scan Name, VFX ID, Suffix, Start Frame, End Frame, Frame Duration, Handles, Tape, TC range, Pull Date, and Pull ID
- Sidecar CSV with version history for the current export
- Configurable Pull ID and Start Frame fields
- Role assignment with presets (VFX-Pull with date suffix, Custom, or None)
- Open in Final Cut Pro after save

---

### VFX Naming

Batch-rename VFX clips with sequential shot IDs based on scene metadata, a showcode, and incremental counters. Injects chapter markers carrying the generated shot ID into the FCPXML.

**Shot ID Format**
- Showcode prefix (e.g., `MBF`)
- Optional vendor code (e.g., `RSP`)
- Configurable scene digit padding (1, 2, or 3 digits)
- Configurable counter digit padding (2, 3, or 4 digits) and increment step
- Fallback scene number for clips without scene metadata
- Live example preview (e.g., `MBF_093_0010`)

**Identification**
- Identify clips by Role, Marker, or Role or Marker
- Trigger marker type: Chapter, Standard, or To-Do
- Configurable target role (default: VFX)

**Options**
- Overwrite existing IDs or skip clips that already carry a matching shot ID
- Reuse missing IDs (fill gaps in numbering) or continue after the highest existing counter
- Scene numbers are sanitized (e.g., `104.3` and `104pt2` both resolve to `104`)

---

### VFX Titles

Generate VFX title overlay clips from markers already present on your timeline. The marker name becomes the Shot ID and the marker note becomes the scope of work. The output is an FCPXML containing a gap with title clips anchored at each marker position, ready to be imported and copied onto your timeline.

- Title text format: `SHOT_ID -- Scope of Work`
- Yellow Menlo text with drop shadow on a transparent background
- Three position presets: Center, Upper-left (1080p), or Upper-left (2.35:1)
- Identification mode: Marker Only, Role Only, or Marker & Role
- Marker type filter: All, Standard, Chapter, To-Do, or Completed

---

### VFX Shot List

Extract a VFX shot list from your timeline as a tab-delimited TXT or a File129-format EDL.

**VFX Shot TXT**
- Columns: VFX Shot ID, Rec TC In/Out, Rec Duration, Frame Count, Src TC In/Out, Src Duration, Reel, Scene, Take, Clip Name, Marker Name, Marker Comment, plus any custom metadata
- Auto-detects frame rate from the FCPXML format resource
- Recurses into compound clips, multicam clips (active video angle preferred), and sync-clips
- Optional CSV export alongside TXT

**File129 EDL**
- Standard EDL header with title and FCM line
- Marker names serve as clip slates
- Source and record timecodes, reel names, LOC comments with marker color

**Shared Options**
- Identification mode: Marker Only, Role Only, or Marker & Role
- Marker type filter with role checkboxes auto-populated from the source FCPXML

---

### VFX Delivery

Match VFX delivery files back to your timeline by marker name and place them as anchored clips in a new FCPXML project.

- Drop a source timeline FCPXML and a folder of VFX files (MOV, MP4, MXF, M4V)
- Matching by marker name: exact case-insensitive match, then partial match (filename starts with marker name)
- All versions of each shot are stacked (sorted by version ascending, latest on top)
- Configurable handle frame trimming and slate frame compensation (+1 frame)
- Marker type filter: All, Standard, Chapter, To-Do, or Completed
- Role assignment with presets: VFX, Comp, VFX Shots, or Custom
- Keyword tagging with the delivery folder name
- Live preview showing matched files, unmatched markers, and unmatched files
- Open in Final Cut Pro after save

---

### VFX Compare

Compare two versions of a VFX shot list and generate a color-coded XLSX spreadsheet report.

**Input Modes**
- TXT Files: compare two previously exported VFX Shot TXT files
- FCPXML: compare two FCPXML sequences directly (extracts shots on the fly)

**Comparison Logic**
- Shots keyed by VFX Shot ID (first column)
- Five status categories: Identical, Changed, Rec TC Change (timecode shifted but duration unchanged), Added, Omitted
- Per-cell change highlighting for Changed and TC Change rows
- Auto-generated Change Notes column summarizing field-level differences

**Output**
- Native XLSX (OOXML ZIP archive) compatible with Excel and Numbers without warnings
- Two worksheets: VFX Comparison (full shot detail) and Summary (status counts)
- Extra columns inserted after VFX Shot ID: Scope of Work, Notes, Change Notes
- Color-coded rows: green (Identical), yellow (Changed), orange (TC Change), red (Omitted), blue (Added)

**UX**
- Live preview updates as soon as both files are dropped
- Auto-switches input mode when you drop an FCPXML onto TXT mode or vice versa

---

## Usage

### Pulling VFX Plates

1. In Final Cut Pro, add markers (Chapter, Standard, or To-Do) to each VFX clip and assign VFX subroles as needed.
2. Export your timeline as FCPXML, or drag the project directly from Final Cut Pro into VFX Toolbox.
3. Open the **VFX Pull** tab and drop the FCPXML onto the source zone.
4. Configure extraction: choose the identification mode, marker type, and frame handles.
5. Under Naming & Versioning, enable version tracking and select or create a project. Choose a plate suffix from the subrole dropdown if applicable.
6. Under Export, set the Pull ID and Start Frame, and enable the Scan TXT and/or Sidecar CSV exports.
7. Click **Save VFX Pull FCPXML** and import the result into Final Cut Pro.

---

### Naming VFX Shots

1. In Final Cut Pro, mark your VFX clips with markers or assign a VFX role.
2. Export the timeline as FCPXML and drop it onto the **VFX Naming** tab.
3. Enter your showcode (e.g., `MBF`), optional vendor code, scene/counter padding, and increment step.
4. Choose the identification mode and trigger marker type.
5. Review the preview and click **Save Named FCPXML**.
6. Import the named FCPXML back into FCP. Each clip now carries a chapter marker with the generated shot ID.

---

### Generating VFX Title Overlays

1. Ensure your timeline markers carry shot IDs as names and scope-of-work descriptions as notes.
2. Export the timeline and drop it onto the **VFX Titles** tab.
3. Choose the position preset and marker filter.
4. Click **Generate VFX Titles** and import the output FCPXML into FCP.
5. Select all the title clips in the new event and paste them onto your timeline.

---

### Importing VFX Deliveries

1. Export your current timeline as FCPXML.
2. Open the **VFX Delivery** tab. Drop the FCPXML onto the Source Timeline zone and the delivery folder onto the VFX Folder zone.
3. Review the match preview. Set handle frames, slate frame trimming, and the role.
4. Click **Save VFX Delivery** and import the result into Final Cut Pro. All matched files appear as connected clips at their marker positions.

---

### Exporting a VFX Shot List

1. Drop your FCPXML onto the **VFX Shot List** tab.
2. Choose the output format (VFX Shot TXT or File129 EDL), identification mode, and marker filter.
3. Click **Export**. Optionally enable "Also export as CSV" for the TXT format.

---

### Comparing VFX Versions

1. Open the **VFX Compare** tab and select the input mode (TXT Files or FCPXML).
2. Drop the previous version on the left and the current version on the right.
3. For FCPXML mode, configure the identification mode, marker filter, and roles.
4. Click **Compare & Export XLSX**. Open the spreadsheet to review color-coded changes.

---

### Exporting a Metadata Codebook

1. Drop an Final Cut Pro event or project FCPXML onto the **Codebook** tab.
2. Optionally enter a prefix to strip from metadata column headers.
3. Click **Export Codebook TXT**. Enable "Also export as CSV" if needed.