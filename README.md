# Enderman's Scripts
The vault of scripts that I have written for fun.

## SAM Viewer

The Windows user information, including the password hash, is stored within the SAM (Security Account Manager) registry hive.
The following script reads the SAM file and extracts the password hash alongside extra potentially useful information.

**The password hash is encrypted in 3 layers:**
1. DES encryption with the user's RID (32-bit LE integer) as the key.
2. AES encryption with the «boot key».
3. AES encryption of the «boot key» with the «LSA key».

What I call an «LSA key» (don't confuse it with an LSA secret) is split into four 4-byte chunks and stored in **class names** of
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa` subkeys `JD`, `Skew1`, `GBG` and `Data`.

**The LSA key is calculated as follows:**  
    $B = JD_{class} \mathbin\Vert Skew1_{class} \mathbin\Vert GBG_{class} \mathbin\Vert Data_{class}$  
    $shuffle(B_i,\{8, 5, 4, 2, 11, 9, 13, 3, 0, 6, 1, 12, 14, 10, 15, 7\})$

*Might be useful in computer forensics.* **Watch a full explanation on [YouTube](https://youtu.be/Hq_RgcYL9_k).**

**To run the script, use the following command:**

```bash
python3 -m sam.samviewer
```

*Tested on Python 3.12.7*

### Arguments
- `-h`, `--help`: Show the help message and exit.

**Mutually exclusive:**
- `--reg`: Path to the `HKLM\SAM` non-binary registry **export** file.
- `--hive`:  Path to a directory containing SAM and SYSTEM hives (e.g. %systemroot%\System32\config), must not be in use.

**Optional:**
- `--jd`: $JD_{class}$
- `--skew1`: $Skew1_{class}$
- `--gbg`: $GBG_{class}$
- `--data`: $Data_{class}$
- `--pw`: Custom password to hash & encrypt for every user found.
- `--output`: Path for output file

### Known issues
- Custom password hashing not extensively tested yet. `--pw` argument might return wrong hash for now.

## License
This project is licensed under the GNU GPL-3.0 License - see the [LICENSE](LICENSE) file for details.

## Contributing
If you would like to contribute to this project, feel free to fork this repository and submit a pull request.

## Contact
If you have any questions or suggestions, feel free to [contact me](mailto:contact@enderman.ch).
