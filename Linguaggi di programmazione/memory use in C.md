#C

---

un programma per essere eseguito deve sfruttare alcune risorse del computer ([[CPU]],[[RAM]], [[IO]], e altri componenti hardware).

Determinare quali risorse usare e in che quantità è compito del [[sistema operativo]]. Una delle risorse più importanti è la memoria, nello specifico la [[RAM]], memoria temporanea usata per l'esecuzione del programma.

int = 4 byte
double = 8 byte

La sezione della RAM allocata è un blocco di byte di cui il programma ha bisogno (e che sono liberi).

Ad ogni byte è associato un indirizzo numerato con il sistema esadecimale. Un byte di memoria può essere posizionato nell'indirizzo 0x200 e il byte successivo è posizionato all'indirizzo 0x201.
![[Pasted image 20221005154759.png]]

Si può accedere ad un byte di memoria usando un [[pointer]].