# Case data model

## CASE

Attributes: URN, operational name (optional), Date created, etc

Relationships:
- Has one or more DEFENDANTS
- Has zero or more WITNESSES
- Has zero or more VICTIMS
- Has one or more MATERIALS

## DEFENDANT

Attributes: name, date of birth, address, and other personal details

Relationships:
- Belongs to one or more CASES (many-to-many with CASE)
- Involved in one or more CHARGES (many-to-many with CHARGE)

At any given point, a defendant has either charges or counts — not both. A defendant whose case stays in the magistrates' court will only ever have charges. A defendant sent or electing Crown Court will have charges until the indictment is put at PTPH, at which point counts replace the charges and the charges are no longer valid.

## WITNESS

Attributes: name, personal details, service level, and other attributes

Relationships:
- Associated with one or more CASES (many-to-many with CASE)
- Associated with zero or more MATERIALS (their statements, recordings, and other items they produced)

Questions:

1. Do WITNESSES connect to anything below case level — for example, to specific CHARGES — or only to the CASE?

## VICTIM

Attributes: name and personal details

A victim is not modelled as a witness, but can have the same material associations. A victim personal statement or video recording is linked directly to the VICTIM record. A bereaved family member is also modelled as a victim — they may have no associated material, and contact goes to them rather than to a victim directly.

Relationships:
- Associated with one or more CASES (many-to-many with CASE)
- Links to one or more CHARGES (to support Victim Code requirements and prompt the right behaviour when a charge is made, dropped, or substantially altered)
- Associated with zero or more MATERIALS

Questions:

1. A victim may also give evidence at trial, functioning as a witness. Does the model need to represent this, or is the MATERIAL association sufficient?

## CHARGE

Covers both charges (magistrates' court and pre-indictment Crown Court) and counts (post-indictment Crown Court). The two are treated as the same type of object and labelled correctly in context. They have the same core attributes, but counts carry additional attributes such as custody time limits (CTLs). Charges are superseded when counts are put; the original charge record is preserved.

In most cases a count maps directly to the charge it replaced. In some cases — particularly child abuse and fraud — the original charges are specimen charges and counsel or an HCA will draft the indictment on a different basis. Where a count differs from the charge it was derived from, the linkage should be recorded and whether the alteration is substantial (to prompt Victim Services).

Attributes: offence description, statute, date of offence, anticipated plea, type (charge or count), CTL (counts only), status (active or superseded), substantial alteration flag

Relationships:
- Involves one or more DEFENDANTS (many-to-many with DEFENDANT)
- Links to zero or more VICTIMS
- Has one or more POINTS TO PROVE
- Optionally has one or more SUBCHARGES
- Optionally references the CHARGE(S) it was derived from (where a count differs from the original charge)

## SUBCHARGE

A variant within a charge. Not all charges have a subcharge. For example, a public order charge may be either "threatening violence" or "provoking violence in others" — each with its own distinct points to prove.

Attributes: variant description

Relationships:
- Belongs to one CHARGE
- Has one or more POINTS TO PROVE

Questions:

1. When a SUBCHARGE is present, does it replace the parent CHARGE's points to prove entirely, or does it add to them?

## POINT TO PROVE

The legal element that must be satisfied to prove the charge.

Attributes: description

Relationships:
- Belongs to one CHARGE or one SUBCHARGE
- Has one or more EVIDENCE ITEMS (through LINK)

## MATERIAL

A PDF, witness statement, video, audio file, photograph, forensic report, or any other item.

Attributes: title, type (PDF, video, audio, image, and so on)

Relationships:
- Belongs to one CASE
- Optionally associated with one WITNESS or one VICTIM (the person who produced it)
- Contains zero or more EVIDENCE ITEMS

## EVIDENCE ITEM

A specific piece of evidence extracted from a MATERIAL. This may be a passage, a paragraph, a time range in a video, or the entire document — if the whole thing is relevant, the evidence item references all of it.

Attributes: description, excerpt reference (optional — for example, a page and paragraph number, a timestamp range, or "whole document")

Relationships:
- Extracted from one MATERIAL
- Connected to one or more POINTS TO PROVE via LINK

## LINK

The connection between a piece of evidence and a legal argument. Specifies that an EVIDENCE ITEM proves a particular POINT TO PROVE for a particular DEFENDANT. Because a charge can involve multiple defendants, the same evidence item may prove a point for one defendant but not another — the link makes that explicit.

One evidence item can have many links — to different points to prove, on different charges, for different defendants.

Relationships:
- References one EVIDENCE ITEM
- References one POINT TO PROVE (which implies the CHARGE, since each point to prove belongs to a specific charge or subcharge)
- References one DEFENDANT

Questions:

1. Redaction, disclosure status, and unused material designation also create associations between objects. Are these types of LINK — in which case LINK needs a type attribute and a more generic definition — or are they separate concepts that belong on MATERIAL directly?

## INDICTMENT

The formal document required by legislation in Crown Court. Contains counts (charges of type "count") for one or more defendants.

Relationships:
- Belongs to one CASE
- References one or more CHARGES (of type "count")

Questions:

1. Does INDICTMENT need to be a separate object, or is it just a document generated from the counts already held on CHARGE? The distinction matters if an indictment can reference counts from more than one case (which is technically possible, though rare in practice).
2. Case splitting and merging: where one case becomes two, or two merge into one, how should the model handle the charges, counts, and indictment that existed before the split or merge?
