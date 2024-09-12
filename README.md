# 🚩 Common Flag Scoring System (CFSS)

## Description

The Common Flag Scoring System (CFSS) is a framework designed to give a score to a given flag from a Capture The Flag (CTF) challenge. The name is derived from the [Common Vulnerability Scoring System](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System) (CVSS), widely used in the computer security industry to score software vulnerabilities.

## Goal

The goal of CFSS is to reduce subjectivity from challenge designers when determining the score a flag should have by providing clear guidelines and criteria. The CFSS score can also be shared with participants to justify how the scoring of the flag was determined, which increases transparency and reinforce competitive integrity of CTF competitions.

## Guiding Principles

CFSS was created in respect with these principles. They are in order, from most important to least important:

1. **Competitive Integrity.** This refers to the fact that skill should be the single most deciding factor in who wins and who loses a CTF and their position in the scoreboard. This is done by limiting factors based on luck or other unpredictable events.
2. **Accuracy**. Accuracy refers to how close a measurement (here, the score) is close to the truth. In this context, accuracy means that scores calculated using CFSS in a given CTF, when ordered ascendingly, presents the challenges sorted by the skill and effort required to solve them.
3. **Precision**. Precision refers to how close many measurements (here, the score) are close to each others. Precision is independant of accuracy. In this context, precision means that if five different people calculate the CFSS for a given flag, they would all have the same score.
4. **Maximizing Fun**. This mostly refers to the “gamified” aspect of CTFs, including minimizing unfun experiences, such as losing points.

When questions arise where the answer is not obvious, one must refer to these guiding principles, in order, to determine how to correctly apply CFSS.

## What CFSS is NOT for

- CFSS does not score for the quality of a challenge. One might have trouble applying CFSS to challenges with questionable design, such as challenges involving guessing (because guessing does not translate well to technical skill nor effort).
- CFSS does not determine which categories are “high skill floor”. This must be decided in the context of the CTF in which CFSS is used in. For example, a CTF targeted at software developers will most likely not consider categories involving a lot of scripting or programming as a high skill floor category, but a CTF targeted at Sysadmins or people with little to no programming experience might.

## License

CFSS is released under the [MIT license](https://choosealicense.com/licenses/mit/).

## Versions

CFSS follows [Simple Versioning](https://simver.org/) (simver). 

|Date|Version|Changes|
|----|-------|-------|
|2024-05-26|CFSSv0.0|First draft|
|2024-08-13|CFSSv0.1|Small corrections and precisions + new section regarding flags in a multiple-flags track|
|2024-09-12|CFSSv0.2|Small corrections + first release as a git repo|

## CFSS Specification

The scoring system algorithm takes as input:
- Technical Skillset ( `L`/`B`/`I`/`A` )
- Effort ( `L`/`M`/`H`)
- High Skill Floor Category ( `Y`/`N` )
And **outputs a score range from <u>1 to 20</u>**. ****The higher the score, the more difficult the flag is to obtain and thus the more skill and effort is required to find it.

To calculate the score, sum all of the inputs’ minimum weight and their maximum weight separately, and the score range is those two values.

### CFSS expression

A CFSS score can be expressed like this:

`CFSS:<cfss_version>/TS:<technical skillset value shorthand>/E:<effort value shorthand>/HSFC:<high skill floor category shorthand>=<flag point range>` 

Example: **CFSS:0.2/TS:L/E:L/HSFC:N=1-2**

### CFSS Criteria

|Criteria|Value|Description|Examples|Weight|
|--------|-----|-----------|--------|------|
|Technical Skillset|Little to None|The flag can be obtained with little to no technical knowledge of the challenge category. Anyone has the ability to find the flag.|- Flags that can be Googled almost as-is where understanding the answer can be done by almost anyone regardless of their technical skills in the challenge category. <br>- Trivia challenges flags <br>- Rule flags|1|
|Technical Skillset|Beginner|The flag can be obtained by having beginner-level skills in the challenge category, typically by well known actions. Participants who have been introduced to the challenge category by introductory blog posts/articles, beginner workshops or light mentoring/teaching has the ability to find the flag.|- Flags that can be found by running strings on a binary <br>- Flags that can be found by opening a web page’s HTML/JS code <br>- Flags in a robots.txt page <br>- Flags located in /etc/passwd or by running ps aux on a privilege escalation challenge as the only step. <br>- Flags encoded or encrypted with well-known, widely used encodings/encryptions such as ROT-13/Caesar’s Cipher/XOR encryption, base64/binary/hex/ascii encoding. <br>- Flags from a forensics challenge where the flag is hidden in EXIF metadata.|2-3|
|Technical Skillset|Intermediate|The flag can be obtained by having intermediate-level skills in the challenge category. Participants with a good understanding of the basics of the challenge category AND with prior CTF/work/personnal experience with the challenge category have the ability to find the flag. Some research AND/OR advanced tools AND/OR skills outside of the challenge category might be required to find the flag.|- Flags from web challenges that require exploitation of XSS vulnerabilities <br>- Flags from reverse engineering challenges that require a disassembler and knowledge of an assembly language <br>- Flags from cryptography challenges that require knowledge of algorithm implementations and/or potential vulnerabilities in well-known cryptography algorithms. <br>- Flags from web challenges that require knowing common implementation mistakes, such as using pseudo-random number generation instead of OS-random number generation, or concatenating OS commands with user inputs. <br>- Flags from a privilege escalation challenge that require some reconnaissance and exploitation of a misconfiguration or known vulnerability.  <br>- Flags in a forensics challenge that require parsing forensics artifacts such as RDP bitmap cache/NTFS journal. <br>- Flags from a cryptography challenge that requires to use a padding oracle attack.|4-6|
|Technical Skillset|Advanced|The flag can be obtained by having advanced knowledge in the challenge category. Participants with a deep understanding of the challenge category AND with a lot of prior CTF/work/personnal experience with the challenge category have the ability to find the flag. Some research AND/OR advanced tools AND/OR new tools AND/OR skills outside of the challenge category are definitely required to find the flag.|- Flags from web challenges that require exploitation using techniques with little documentation and/or where the exploit complexity is high and/or require deep knowledge of how various web technologies work. For example: timeless timing attacks. <br>- Flags from reverse engineering challenges that require to write custom parsers for a custom virtual machine inside the binary. <br>- Flags from a privilege escalation challenge that require deep OS internals knowledge. <br>- Flags from a forensics challenge that require parsing a little-known artifact for which no tool and little documentation exists, such as parsing the emoji history from a windows filesystem. <br>- Flags from a cryptography challenge that require finding an obscure and rare flaw in a custom encryption/signing algorithm.|7-10|
|Effort|Low|The amount of steps that need to be done to find the flag is low AND/OR the number of hypothesis to test to find the flag is low AND/OR the implementation complexity to find the flag is low. If the three effort metrics (amount of steps, number of hypothesis to test, implementation complexity) are not the same, choose the highest effort value.|- Rule flags <br>- Flags from a trivia challenge <br>- Flags from a web challenge where the vulnerability to exploit is explicit AND require a single well-formed HTTP request that one can build manually by typing it. <br>- Flags from a forensics challenge where the artifact to parse is explicit AND a well-known, easy to use tool is available to parse it.
|0-1|
|Effort|Medium|The amount of steps that need to be done to find the flag is medium AND/OR the number of hypothesis to test to find the flag is medium AND/OR the implementation complexity to find the flag is medium. If the three effort metrics (amount of steps, number of hypothesis to test, implementation complexity) are not the same, choose the highest effort value.|- Flags from a web challenge that require to understand a multi-step attack chain, such as exploiting a stored XSS, then receiving inputs from a victim that visited a given link. <br>- Flags from a forensics challenge where the artifact is easy to parse but is not explicit and requires trying some hypothesis <br>- Flags from a web challenge that requires some scripting or usage of advanced features in a web proxy.|2-4|
|Effort|High|The amount of steps that need to be done to find the flag is high AND/OR the number of hypothesis to test to find the flag is high AND/OR the implementation complexity to find the flag is high. If the three effort metrics (amount of steps, number of hypothesis to test, implementation complexity) are not the same, choose the highest effort value.||5-8|
|High Skill Floor Category|No|The flag in from a challenge category where the skillset is not rare OR the prior skills it takes to learn the challenge category is not particularly high OR the technical skillset is NOT intermediate OR advanced.|- Flags from most challenge categories|0|
|High Skill Floor Category|Yes|The flag in from a challenge category where the skillset is rare AND the prior skills it takes to learn the challenge category is particularly high AND the technical skillset is intermediate OR advanced. The rarity of a skillset is dependant on the type of CTF hosted as well as the profile of participants.|- In a lot of CTFs: flags from cryptography/pwn/reverse engineering/hardware challenges.|1-2|

## Special cases

### How to apply CFSS in a (semi-)linear track featuring multiple flags

CFSS is applied for each flag individually. To apply CFSS to a flag which is part of a bigger track (featuring multiple flags), consider that the participant has found all strictly-required flags with the intended solution, and assume that they possess all the knowledge from the previous strictly-required flags. Then, you can apply the three criteria (technical skill, effort and high skill floor) to this flag normally.

See [Fifth flag of a track featuring 8 flags total](#fifth-flag-of-a-track-featuring-8-flags-total) for an example.

## Examples

### Rule Flag

**CFSS:0.2/TS:L/E:L/HSFC:N=1-2**

**Technical Skillset (TS)**: Little to None (L)

**Effort (E)**: Low (L)

**High Skill Floor Category (HSFC)**: No (N)

**Score range**:
Minimum: (1 + 0 + 0) = 1
Maximum: (1 + 1 + 0) = 2
The score range is 1-2 (1 or 2)

### Flag from Exploiting a basic PHP File Upload Vulnerability

In this example, the vulnerability to exploit is pretty explicit and requires uploading a PHP file with custom code and then accessing the uploaded file, where the path is known.

**CFSS:0.2/TS:B/E:L/HSFC:N=2-4**

**Technical Skillset (TS)**: Beginner (B)

**Effort (E)**: Low (L)

**High Skill Floor Category (HSFC)**: No (N)

**Score range**:
Minimum: (2 + 0 + 0) = 2
Maximum: (3 + 1 + 0) = 4
The score range is 2-4 (2 or 3 or 4)

### Flag from Reverse Engineering a Rust Binary Without Debugging Symbols with Custom Obfuscation and Strong Anti-Debugging

**CFSS:0.2/TS:A/E:H/HSFC:Y=13-20**

**Technical Skillset (TS)**: Advanced (A)

**Effort (E)**: High (H)

**High Skill Floor Category (HSFC)**: Yes (Y)

**Score range**:
Minimum: (7 + 5 + 1) = 13
Maximum: (10 + 8 + 2) = 20
The score range is 13-20 (13 or 14 or 15 or 16 or 17 or 18 or 19 or 20)

### Flag from a Forensics Challenge to parse an Undocumented Windows Artifact

In this example, it is explicit what you need to do, but how to do it is not clear and requires research and skills to understand what to research.

**CFSS:0.2/TS:A/E:M/HSFC:N=13-20**

**Technical Skillset (TS)**: Advanced (A)

**Effort (E)**: Medium (M)

**High Skill Floor Category (HSFC)**: No (N)

**Score range**:
Minimum: (7 + 2 + 0) = 9
Maximum: (10 + 4 + 0) = 14
The score range is 9-14 (9 or 10 or 11 or 12 or 13 or 14)

### Password Cracking Challenge Where you Need to Build your Own Custom Wordlist

In this example, it is explicit what you need to do, but building your wordlist requires scripting a custom web scraping tool and trying many hypothesis on what passwords have been used and how they have been generated.

**CFSS:0.2/TS:I/E:H/HSFC:N=13-20**

**Technical Skillset (TS)**: Intermediate (I)

**Effort (E)**: High (H)

**High Skill Floor Category (HSFC)**: No (N)

**Score range**:
Minimum: (4 + 5 + 0) = 9
Maximum: (6 + 8 + 0) = 14
The score range is 9-14 (9 or 10 or 11 or 12 or 13 or 14)

### Fifth flag of a track featuring 8 flags total

Consider the following track where the intended solving path is this:

```
Here, 
Flag X --> Flag Y
           Flag Z
Means "Flag X is required to find Flag Y and Flag Z"
--------------------
Flag 1 --> Flag 3 --> Flag 4 --> Flag 6 --> Flag 8
           Flag 2     Flag 5 --> Flag 7
```

To apply CFSS to `Flag 5`, we need to assume that the participant has found `Flag 1` and `Flag 3` (because they are required to solve `Flag 5`), but NOT `Flag 2` nor `Flag 4`( because they are NOT required to solve `Flag 5`). 

**The technical skill, effort and high skill floor criteria need to be decided assuming that the participant has all the information from the beginning of the challenge, plus the information to solve `Flag 1`, plus the information to solve `Flag 3`.**