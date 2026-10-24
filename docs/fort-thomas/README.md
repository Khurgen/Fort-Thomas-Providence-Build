# Fort Thomas museum catalog pilot

Status: configuration prototype, not a deployed or production-tested catalog.
Prepared October 24, 2026. Target: a supervised pilot by November 15, 2026.

## Scope and architecture

Use CollectiveAccess Providence for the museum's internal catalog. Plan for
10,000 objects, with multiple images for approximately 7,500 objects. Run one
shared application and database on a Linux server. Raspberry Pi workstations
connect with a browser, using individual staff accounts. Do not install separate
production catalogs on each workstation.

A Pi 5 with SSD storage is a candidate pilot server, subject to measurement.
Choose production hardware after representative media imports and concurrent
cataloging tests. Prefer SSD storage for the database and media; do not rely on
the operating-system microSD card as the only collection storage. Server model,
RAM, storage capacity, network and power protection remain to be confirmed.

Public online discovery is a later phase. Pawtucket2 is separate from this
Providence project. No public catalog is configured by this change.

## What this change supplies

`install/profiles/xml/fort_thomas.xml` is a complete derivative of the bundled
`default.xml`, retaining `base.xml` inheritance. The installer reads one XML base
directly, so this profile deliberately does not introduce a second inheritance
level through `default.xml`. Keep the upstream files unchanged.

The profile preserves the existing object editors, relationships, media screen,
and storage screen. It adds Artifact, Archival item, Oral history and Born-digital
record object types, with Artifact as the single default. Existing types,
including Photography and Document, remain available for compatibility.

The new Museum stewardship screen adds optional text fields:

| Code | Purpose |
| --- | --- |
| ft_legacy_identifier | Earlier identifiers, recorded without renumbering |
| ft_documentation_source | Evidence, citations and uncertainty |
| ft_rights_notes | Rights holder, permission evidence and reproduction conditions |
| ft_access_restrictions | Restriction basis and review information |
| ft_processing_notes | Cataloging work still to complete |
| ft_archival_arrangement | Original order and archival context |
| ft_local_significance | Documented connection to Fort Thomas |

These notes do not enforce permissions or create structured conservation,
movement, loan or rights-management workflows. Configure and verify those
separately. In particular, the inherited storage editor is not proof that dated
location history works correctly. Test and configure Providence 2.0 history
tracking before using it as the authoritative movement register.

## Installation in a test environment

1. Read the upstream [requirements](https://docs.collectiveaccess.org/providence/user/setup/systemReq)
   and [installation guide](https://docs.collectiveaccess.org/providence/user/setup/install/).
   The repository README identifies PHP 8.2 and 8.3 as supported. Check the
   selected release's complete dependency requirements on the actual host.
2. Record the exact application commit and dependency versions deployed. The
   fork's initial master commit is `f5c6214`; do not assume that its README's
   version number makes master identical to the 2.0.11 release tag.
3. Create a new, empty test database and a separate media directory. Never point
   the installer at an existing museum database.
4. Copy `setup.php-dist` to the ignored `setup.php`. Enter local database
   credentials, the museum display name, administrator email and a unique
   application name. Keep America/New_York as the timezone.
5. Follow the upstream installation instructions and choose **Fort Thomas
   Military and Community Museum (pilot)** as the installation profile.
6. Use fictitious records and non-sensitive sample images for initial tests.
   Create individual accounts and verify permissions before using real records.

Editing this XML after installation does not automatically update a live
database. Future production configuration changes require an explicit,
backup-protected update process. Do not reinstall to apply changes.

## Information stewardship

Preserve existing accession and object numbers. Confirm the museum's numbering
policy before enabling automatic numbering or importing records. A temporary
inventory number must not imply legal accession or ownership.

Use collection relationships and a reviewed archival hierarchy for archival
materials. Digitized images of one letter normally belong as media attached to
that letter's record. Decide the collection, series, folder and item levels
before describing entire archival collections at item level.

The inherited base profile defaults access to internal only. Verify that default
on objects and media in the installed system. Record access is different from
staff permissions and from legal permission to reproduce an image. Keep donor
contact information, valuations and restricted documentation out of this public
source repository. Use invented data in examples and bug reports.

## November pilot acceptance

Before routine cataloging, demonstrate:

- A representative 50-record sample, including artifacts, photographs, a
  multipage letter, an archival collection relationship and an oral history.
- Multiple images per object, preserved originals and usable derivatives.
- Search by identifier, title and descriptive terms, plus a usable export.
- Location changes with dates and a retrievable history.
- Two simultaneous staff sessions without lost edits, including a same-record
  edit conflict test and a documented staff procedure for that case.
- A cataloger who cannot administer users, and a reviewer who can approve work.
  These proposed roles are not provisioned by this XML.
- Restricted objects and media remain inaccessible to unauthorized accounts.
- A complete backup of database, original media and local configuration, restored
  into an isolated installation with sample records and images checked.

Record task times, failures and server memory/storage use. The 50-record sample
validates workflows only. Separately test a generated 10,000-record dataset and
representative media volume before claiming collection-scale performance.

## Delivery sequence

| Period | Deliverable |
| --- | --- |
| October 24 to 30 | Confirm host, existing data format, identifiers and cataloging fields; install test system |
| October 31 to November 6 | Test representative records, images, archival structure and staff permissions |
| November 7 to 14 | Correct workflow problems, test restoration, train staff and rehearse pilot |
| November 15 onward | Begin supervised cataloging only after acceptance checks pass; expand imports gradually |

This is a target sequence, not a guarantee of production readiness. Do not bulk
import the collection until identifier mapping, media matching and rollback have
been rehearsed. Preserve all source spreadsheets and original image files.

## Decisions needed next

Confirm the intended server (Pi model/RAM or another Linux computer), SSD size,
number of simultaneous catalogers, current catalog format and numbering system,
approximate image sizes, existing backup destination, and whether the catalog
must work when internet service is unavailable. A local network deployment can
be designed for that requirement; any external services must be checked.

## Validation so far

The profile passes the bundled `profile.xsd` using Python lxml. Its base reference,
new element codes and editor placements have been checked statically. This
workspace has no PHP or database runtime, so installation, browser behavior,
authorization, media processing, ARM compatibility and performance have not yet
been verified.
