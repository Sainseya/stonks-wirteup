<a name="readme-top"></a>

<div align="center">

# DFIR Investigation Report

### Digital Forensics & Incident Response

<a href="https://github.com/Sainseya">Sainseya</a>

**HackTheBox challenge — Difficulty: Insane**

</div>

<details>
<summary>Table of Contents</summary>

- [About The Project](#about-the-project)
- [Report Scope](#report-scope)
- [Methodology](#methodology)
- [Contents](#contents)

</details>

## About The Project

This repository contains a Digital Forensics & Incident Response (DFIR) investigation report for a **HackTheBox challenge rated Insane**. It documents the forensic investigation of **Operation "Stonks"**: recovery of a deleted financial report from a Windows Server's Data Deduplication store, and the discovery that the company's public 2024 accounts had been falsified.

Deliverable: [`DFIR_Report_Operation_Stonks.pdf`](./DFIR_Report_Operation_Stonks.pdf)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Report Scope

| Case | Focus |
|---|---|
| **Operation "Stonks"** (host FSERVER01) | Windows Data Deduplication forensics: NTFS `$MFT` analysis, ChunkStore reconstruction of a deleted `.docx`, and comparison against a falsified public financial statement |

Evidence container: `evidence.ad1` (AccessData AD1 v4, logical image, 166 MB), a Windows Server in the SE Asia Standard Time (UTC+7) zone.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Methodology

The investigation follows a structured, repeatable DFIR workflow:

1. **Evidence integrity**: chain-of-custody verification via FTK Imager MD5/SHA-1 hashes.
2. **Reconnaissance**: identification of the two NTFS volumes (C: system, D: 'Data') and the Deduplication ChunkStore.
3. **Investigation**: timeline reconstruction from Windows Setup/Deduplication event logs and scheduled tasks, `$MFT` analysis of the deleted file, and reconstruction of its content from the Dedup ChunkStore (stream map + chunk data), driven by the NTFS reparse point.
4. **Conclusion**: comparison of the recovered internal report against the public version, and actionable recommendations.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Tooling

- **FTK Imager 8.2.0.59** (Exterro): mounting the AD1 image, browsing NTFS structures, hex inspection and file export.
- **Windows Event Viewer**: examination of the Setup and Deduplication operational logs.
- **PowerShell**: archive extraction and metadata inspection.
- **recover.py**: a custom Python tool developed for this case that parses the `$MFT` for the deleted, deduplicated file, extracts the stream-map hash from its reparse point, and reconstructs the file from the ChunkStore, validating it by SHA-256.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Contents

- Executive summary
- Methodology & evidence handling (tooling, time-zone handling, chain of custody)
- Reconnaissance of the evidence
- Investigation: Deduplication installation/configuration timeline, `$MFT` analysis of the deleted file, ChunkStore reconstruction, authorship attribution, and profit overstatement analysis
- Conclusion & actionable intelligence: indicators of compromise and recommendations

**Key finding**: the organisation maintained two parallel versions of its 2024 Consolidated Financial Statements — a publicly presented, audited version reporting a profit, and an internal version (deleted to conceal the losses) revealing a real loss. Both were deduplicated on D:, so the deleted internal report's content survived in the ChunkStore and was fully reconstructed and validated by SHA-256.

Full details are available in the [PDF report](./DFIR_Report_Operation_Stonks.pdf).

<p align="right">(<a href="#readme-top">back to top</a>)</p>
