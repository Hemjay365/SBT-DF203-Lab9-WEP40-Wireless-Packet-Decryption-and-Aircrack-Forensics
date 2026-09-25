# SBT-DF203 Lab 9 — WEP40 Wireless Packet Decryption and Aircrack Forensics

```
├── report/
│   ├── SBT-DF203-Lab9_2025-FWSD-11468_MaryJoyAdewole.docx   Full report (Word)
│   └── SBT-DF203-Lab9_2025-FWSD-11468_MaryJoyAdewole.pdf    Full report (PDF)
│
├── evidence_records/
│   Command output and analysis artifacts:
│   ├── file_xz_sha256.txt                     Original evidence hash
│   ├── working_hashes.txt                     Working-copy + decompressed hashes
│   ├── capinfos.txt                           Capture inventory (capinfos)
│   ├── protocol_hierarchy.txt                 tshark protocol hierarchy stats
│   ├── repeated_iv_summary_sample.txt         Repeated-IV frequency evidence
│   ├── aircrack_output.txt                    Full aircrack-ng key-recovery result
│   ├── validated_wep40_key_masked.txt         Recovered WEP40 key
│   ├── airdecap_output.txt                    Offline decryption summary
│   ├── all_working_file_hashes.txt            Hashes of all working/ files
│   ├── endpoints_summary.txt                  Ethernet/IP endpoints + TCP conversations
│   └── exported_objects_manifest_summary.txt  Sample of recovered HTTP/carved objects
│
└── screenshots/
    11 terminal screenshots referenced as Figures 1–10 in the report,
    covering evidence acquisition, WEP/IV identification, key recovery,
    offline decryption, endpoint analysis, and object extraction.
```

## Evidence Integrity

| File | SHA-256 |
|---|---|
| evidence/file.xz (original) | `dd54144caef34f228bfb4b87a9101ca7173969376f7ed357bd068061d4f4b8d6` |
| working/file_working.xz (working copy) | `dd54144caef34f228bfb4b87a9101ca7173969376f7ed357bd068061d4f4b8d6` |
| working/file_working (decompressed) | `c17a3f9b955e84f5befd476dbd55c67286d1e3eea9ab402d5359cac0874ebb2d` |
| working/file_working-dec (decrypted) | `167c91994c269777f9048227deb89882caf3fc3763977f2059604f9a6a40b04` |

## Environment Note

The analysis VM initially failed to resolve `kali.download` and
`raw.githubusercontent.com` ("Temporary failure resolving"), blocking tool
installation and evidence download. This was diagnosed as a broken DNS
resolver and fixed by setting `nameserver 8.8.8.8` in `/etc/resolv.conf`.
A second fault then surfaced during package installation — IPv6 routing
returned "Network is unreachable" — and was resolved by forcing IPv4-only
transport (`Acquire::ForceIPv4=true` for APT, `wget -4` for the evidence
download). All findings in this report were captured after both fixes were
verified; the failed attempts are documented in the report but not used as
evidence.

## Tools Used

- **aircrack-ng / airdecap-ng** — WEP40 key recovery and offline decryption
- **Wireshark / tshark** — 802.11 frame inspection, IV extraction, endpoint and object analysis
- **capinfos** — capture file inventory and integrity metadata
- **foremost** — signature-based file carving of the decrypted capture
- **sha256sum** — evidence integrity verification at every stage

## Academic Integrity Statement

I confirm that I completed this lab in an authorized environment, preserved
the supplied evidence, did not target any third-party system, and
accurately documented my own commands, observations and conclusions.
