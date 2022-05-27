## Name: Redaction gone wrong
#### Points: 100
#### Description: Now you DON’T see me. This report has some critical data in it, some of which have been redacted correctly, while some were not. Can you find an important key that was not redacted properly?
#### Files: `Financial_Report_for_ABC_Labs.pdf`

The first thing I usually do with these files is to determine whether it is a pdf file or not:

```console
┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/redaction-gone-wrong]
└─$ file Financial_Report_for_ABC_Labs.pdf
Financial_Report_for_ABC_Labs.pdf: PDF document, version 1.7, 1 pages
```
The file seems to actually be a PDF-file, and therefore we should probably open it next to see what the redaction looks like.
Beside one of the redaction bars, the text says `just painted over in MS word`. This is useful, as it means it's not actually redacted, it's just 
some color that obscures some text - meaning that the text is still actually there. 

Since the text is still there, we have multiple options. We could make use of the `read aloud` option to have the text read for us, or we can simply 
highlight and copy the text as you would in a normal text document. 

Since the report is quite small, we can `ctrl+a ctrl+c` and paste the text in a new document. 

The pasted text removes the redaction bars, and reveals the flag 🚩:



<details>
  <summary>Flag [Spoiler]</summary>

  ```
  Financial Report for ABC Labs, Kigali, Rwanda for the year 2021. 
  Breakdown - Just painted over in MS word. 

  Cost Benefit Analysis
  Credit Debit
  This is not the flag, keep looking
  Expenses from the 
  picoCTF{C4n_Y0u_S33_m3_fully}
  Redacted document.

  ```

</details>
