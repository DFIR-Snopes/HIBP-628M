<h1 align="center">
Hash Cracking Resources
</h1>

### Quick Links:
 - [Wordlists](#wordlists)
 - [Masks and Tokens](#masks-and-tokens)
 - [Rules](#rules)
 - [Summary](#summary)

## What is this?
I needed a large sample of actual password hashes for analysis to improve password security. The HaveIBeenPwned repository was a great collection of password hashes from actual breaches and covered a great diversity of environments. The hashes were made public and over some time I cracked them for analysis:

- `HIBPv7`: 602,796,439/613,584,246 (98.24%) passwords
- `HIBPv8`: 25,408,946/25,459,382 (99.80%) passwords (selective sample) 
- `Total`: 627,918,158/639,043,628 (98.26%)

Resources in this repository use this sample as a base of real-world passwords and are utilized in a few different applications.

While looking at the HIBP dataset, I realized that many passwords were low quality and would not meet AD password complexity requirements. Because I primarily used this resource for internal assessments, I wanted to clean up the input data because the better the input set, the better the output. I created [PwdStat](https://github.com/JakeWnuk/PwdStat) to filter down password lists and provide meaningful analysis that could then be used to develop other resources.

## Wordlists
**HIBPv7_7M.txt**: Contains the top 7 million most popular passwords from HIBPv7 in no particular order. From the top 7 million most popular passwords, I cracked 99.65% of them, and this is the resulting list.

**HIBPv7_100M-min-reqs.txt**: From the top 100 million most popular passwords in HIBPv7, I cracked 99.18% of them, and this is the result after filtering for minimum complexity requirements. The wordlist contains around 8.5 million passwords.

**HIBPv7_Top15_Masks-min-reqs.txt**: Wordlist from the filtered down HIBP data (~70m passwords) and the set's top 15 most popular password masks. The wordlist contains around 30 million passwords and would meet the minimum complexity requirements of an AD domain.

## Masks and Tokens
**Password Masks**: These password masks are in `Hashcat` format taken from the filtered down HIBP dataset and sorted by most popular. These masks can be greatly helpful in masks attacks as they are the most likely masks from real users' passwords.

**Common tokens**: Using [PwdStat](https://github.com/JakeWnuk/PwdStat), a list of password tokens is generated using the NTLK library. The passwords were passed to a parser that attempted to filter down the passwords to their base tokens and then sorted by most common/popular. This list results from parsing tokens from the filtered down HIBP set.

## Rules
The repository contains various rules generated from the unfiltered and filtered HIBP set using unique methods. They are separated into smaller lists to account for different hashing algorithms and sorted by most effective (most cracks) to least effective.
A methodology writeup can be found [here for Squid Rules](https://jakewnuk.com/posts/cracking-half-billion-passwords-custom-rules-wordlists/) and [here for Leo Rules](https://jakewnuk.com/posts/cracking-half-billion-passwords-analysis/).

**Squid Rules**: This ruleset was created using rounds of randomized hashes, wordlists, and rule order to sort Hashcat rules by effectiveness. The set was "trained" on the entire collection of HIBP passwords and only included rules found in public hash cracking rule sets. The set is sorted by most effective to least effective and does not contain any generated rules. 

**Leo Rules**: This ruleset was created using rounds of randomized hashes, wordlists, and rule order to sort Hashcat rules by effectiveness. The set was "trained" on the filtered collection of HIBP passwords and only included generated rules (raking) not found in public hash cracking rule sets. The set is sorted by most effective to least effective and contains generated rules. I performed around 3000 rounds of raking to achieve this optimized set. 

***

## Summary:
- Wordlists:
    - HIBPv7_7m.txt
        - Top 7M passwords (99.65%) from HIBPv7
    - HIBPv7_100M_min-reqs.txt
        - 8.5m passwords from HIBPv7 that would meet min AD complexity requirements
    - HIBPv7_Top15_Masks-min-reqs.txt
        - 30m passwords from HIBPv7 based on the top 15 masks that would meet min AD complexity requirements
- Masks and Tokens
    - password_masks_min-reqs
        - Most popular password masks from all HIBP data filtered for minimum complexity requirements.
    - password_masks_no-reqs
        - Most popular password masks from all HIBP data filtered for passwords greater than 8 characters.
    - common_tokens_min-reqs
        - Most popular password words/tokens from all HIBP data filtered for minimum complexity requirements.
    - *Text files are just the words and csv files contain additional metadata*

- Rules
    - Squid Rule
        - Hashcat rules sorted from most effective to least effective from public hashcracking sets
        - Same rules broken into multiple sizes for specific applications
    - Leo Rule
        - Generated Hashcat rules sorted from most effective to least effective from only passwords that meet min AD complexity requirements
        - Same rules broken into multiple sizes for specific applications
