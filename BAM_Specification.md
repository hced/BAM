# **SPECIFICATION**


## ***B**lender **A**dd-on **M**anager*

N.B: Words between **[ square brackets ]** are open for discussion; representable by multiple alternatives; not set in stone.

### Description

Official Blender add-on for maintaining installed add-ons.


### Main Objective

- Inform user of new updates. (Default: Manual checks; Options: Manual | Automatic.)
- Install updates. (Requires user confirmation.)


### Architecture/Operation

`Blender Add-on Manager` (abbreviated `BAM`) should be included as a pre-installed add-on in official Blender builds. (Disabled by default.)

It connects to a Git-versioned [ json ] file called `blender_addon_versions.json` on [ GitHub ] in order to compare installed versions with the latest available and update outdated add-ons.

`blender_addon_versions.json` will contain the following key-value pairs:
- `bam_title`                               => str (eg. "My Add-on")
- `bam_latest_version`             => int (eg. 1.2.3)
- `bam_latest_version_date`   => date (using JS Date's toJSON method, eg. 2021-03-29T13:58:39.512Z)

Developers make their add-ons available for updates with `BAM` by assigning a value to a class variable, `release_url`, which corresponds to the public source url of the add-on. (Important: `BAM` should be able to flexibly handle any of these three distribution types: repos/.zip files/.py scripts.)

For every add-on that has a `release_url` value that is `!= None`, `BAM` compares the installed version against `bam_latest_version`.

If `bam_latest_version` is newer than installed version, `BAM` downloads the add-on from the source defined in `release_url` and replaces old version with new.

For version-parsing, `BAM` prioritizes common integer version tuples; if add-ons use exotic version styles, maybe there's a way it could utilize `bam_latest_version_date`?

### UI

Location (needs discussion):

- Alt 1: `BAM` attaches itself in [ Preferences > Add-on panel ].
- Alt 2: `BAM` attaches itself in [ N-panel ].
- Alt 3: `BAM` attaches itself in [ View 3D > Header ].

TODO:
- Graphic prototype of UI elements and operational workflow. (Assignee: Henrik Cederblad.)

### Collected ideas from various people

This list gathers a collection of possible future ideas.

- Search for add-ons by feature/tag/name (requires additional key-vals to be added in `blender_addon_versions.json`).
- Organization feature: Method to affect appearance of N-panel.
- Report bugs: when add-ons break or crash Blender, maybe something of value could be sent to their developers, including application states and add-on log output?
- Rating system (poses the question how to secure no one is tampering with ratings).
- Install older versions (no info collected on the actual need for such feature).