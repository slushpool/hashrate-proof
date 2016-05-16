Hash Rate Proof Verification
----------------------------
Slush Pool <support@bitcoin.cz>

This is a public script to verify pool hash rate from a proof file.


Proof file is a zipped text file with line-based jsons (each line is one json). The first line is header - just key-value
json. Other lines are submits, one submit per line. We provide data to verify submit difficulty and our mark in
coinbase (/slush/). The file name (eg. 2016-02-01_16.zip) denotes the timestamp when the sampling period ends. This
proof file contains submits from 15:00 01/02/2016 to 16:00 01/02/2016.

Validation has two main parts. The first one is validation of the hash origin and the second one is the target validation
(quality of the hash). Validation of origin checks if coinbase transaction input contains our mark (/slush/). This
coinbase transaction must be in the merkle root in the block header.

Target validation computes double hash of block header and checks if the hash target is lower than the hash rate proof
target (computed from hash rate proof difficulty).

Each submit is validated by both validations.

The Script has only one argument - the gzipped proof file - that it operates on. There is no need to explicitly unzip
the proof file. The script prints result status and hash rate counted from number of proof submits and the proof
difficulty. If everything is correct, status is OK otherwise the status is ERR. When the status is ERR, the script
prints a reason for the error.

Structure of Proof File
-----------------------

First line is header

```
    {
        'version': <int>,  # integer - version of the proof script
        'difficulty': <int>,  # integer - network difficulty
    }
```

Other lines are submits

```
    [
        <str>,  # string (hex encoded) - block header
        <str>,  # string (hex encoded) - coinbase (with our sign /slush/)
        [<str>, ...],  # list of strings (hex encoded) - merkle branch of transactions to verify if our coinbase is in
                       # the block header
    ]
```
