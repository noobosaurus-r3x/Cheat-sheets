**Flags :**

`-h` help :)

`-a` type d'attaque

`-m` type fonction de hashage

`-o` output du mot de passe en clair

`--session` nom de la session de travail

`--restore` restaurer session de travail

`--increment-min` nombre de caracteres minimum

`--increment-max` nombre de caracteres max

* * *

**Exemple de syntaxes par type d'attaque :**

straight :
```bash
hashcat -a 0 -m <type de hash> hash.txt wordlist.txt -o hashcracked.txt
```

combinaison :
```bash
hashcat -a 1 -m <type de hash> hash.txt wordlist1.txt wordlist2.txt -o hashcracked.txt
```

bruteforce :
```bash
hashcat -a 3 -m <type de hash> --increment-min=6 --increment-max=12 hash.txt ?l?l?l?l?l?l?l?l?l?l?l?l -o hashcracked.txt
```

=\> lettres miniscules, 6 minimum, 12 max

mask :
```bash
hashcat -a 6 -m <type de hash> hash.txt wordlist.txt ?d?d?d
```

=\> Ajoute 3 caracteres numeriques a la fin du mot (admin123 par exemple)

* * *

**types de cracteres :**

`?l` abcdefghijklmnopqrstuvwxyz

`?u` ABCDEFGHIJKLMNOPQRSTUVWXYZ

`?d` 0123456789

`?h` 0123456789abcdef

`?H` 0123456789ABCDEF

`?s` «space»!"#$%&'()*+,-./:;&lt;=&gt;?@\[\]^_{|}~

`?b` 0x00 - 0xff
