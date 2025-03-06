### ℹ️ About
Generates **`.scope`** compatible format for [**ScopeView**](https://github.com/pkgforge-security/scopeview) (based on TomNomNom's [Inscope](https://github.com/pkgforge-security/inscope))

### 🖳 Installation
Use [soar](https://github.com/pkgforge/soar) & Run:
```bash
soar add 'scopegen#github.com.pkgforge-security.scopegen'
```

### 🧰 Usage
```mathematica
❯ scopegen --help
Usage: scopegen [OPTIONS]
Options:
  -in           generate in-scope domains
  -os           generate out-of-scope domains
  -t            path to file containing domain list
  -wl           generate wildcard in-scope domains
Examples:
cat domains.txt | scopegen -in           # Generate in-scope domains 
cat domains.txt | scopegen -wl           # Generate wildcard in-scope domains
cat domains.txt | scopegen -os           # Generate wildcard out-of-scope domains
```
---
**Additional examples**: 
- #### Inputs
 `cat inscope-domains.txt`
```bash 
 example.com
 example.org
 abc.example.com
 ```
 `cat outscope-domains.txt`
 ```bash
 oos.example.com
 oos.abc.example.org
 ```
---
- Generate **`inscope`** Domain Regexes:
```bash
scopegen -t inscope-domains.txt -in
!# Or Via STDIN
cat inscope-domains.txt | scopegen -in
```
- #### Output
```bash
 .*\.example\.com$
.*\.example\.org$
.*\.abc\.example\.com$
```
---
- Generate **`outscope`**  Domain Regexes:
```bash
scopegen -t outscope-domains.txt -os
!# Or Via STDIN
cat outscope-domains.txt | scopegen -os
```
- #### Output
```bash
!.*oos\.example\.com$
!.*oos\.abc\.example\.org$
```
---
- **`Wildcard`** `*.` Regexes:
> Note on `wildcards`:
> - Use [subxtract](https://github.com/pkgforge/subxtract) to filter first
> ```bash
> #using subxtract, extract only root domains
> subxtract -i inscope-domains.txt | scopegen -wl
> #this will only print main domain
> .*example.*
> ```
```bash
!# Inscope Wildcards
scopegen -t inscope-domains.txt -wl
!# Ouput:
.*example.*
.*abc.*

!# OutScope Wildcards
!# This is kind of meaningless as you should never filter outscope assets based on regex.
!# Only Filter Strictly, and thus, piping anything to scopegen -wl is treated as `Inscope`
!# If you really want to filter Outscope domains based on Wildcards, for whatever reason:
scopegen -t outscope-domains.txt -wl | awk '{print "!" $0}'
!# Output:
!.*oos.*
.*oos.* (Original Output) + Prepended by `!`
!# In the example above, we simply append `!` to make it outscope
```
---