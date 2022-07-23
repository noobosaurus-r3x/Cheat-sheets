https://github.com/ffuf/ffuf

**Syntaxe**
```bash

ffuf -c -w path/to/wordlist -u https://adresse_à_fuzzer/FUZZ
```


(-fs 4242 indique qu'on filtre toutes les reponses de 2424 bytes)
```bash
ffuf -w /path/to/vhost/wordlist -uhttps://adresse_à_fuzzer/ -H "Host: FUZZ" -fs 4242
```

```bash
ffuf -w /path/to/paramnames.txt -u https://adresse_à_fuzzer/script.php?FUZZ=test_value -fs 4242
```


(filtre code reponse 401)
```bash
ffuf -w /path/to/values.txt -u https://adresse_à_fuzzer/script.php?valid_name=FUZZ -fc 401
```

```bash
ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://adresse_à_fuzzer/login.php -fc 401
```

**FLAGS COMMUNS**

-c = colorize l'output

-maxtime = temps max en secondes pour le process

-p = delai en seconde entre deux requetes (0.1 par exemple)

-v = verbose

-t = nombre de threads (40 par défaut)

-mc = match status code(200,204,301,302,307,401,403,405,500 ou all)

-fc = filtrer codes

-w = worldist

-u = url cible

-s = silent mode

-recursion

-r = suivre redirection

-o = output

-of = output format (json, ejson, html, md, csv, ecsv ou all)

-b = cookies


 **Exemples**
 
  Fuzz file paths from wordlist.txt, match all responses but filter out those with content-size 42.

  Colored, verbose output.
```bash
ffuf -w wordlist.txt -u https://example.org/FUZZ -mc all -fs 42 -c -v
```
  Fuzz Host-header, match HTTP 200 responses.
```bash
ffuf -w hosts.txt -u https://example.org/ -H "Host: FUZZ" -mc 200
```
  Fuzz POST JSON data. Match all responses not containing text "error".
```bash
ffuf -w entries.txt -u https://example.org/ -X POST -H "Content-Type: application/json" \ -d '{"name": "FUZZ", "anotherkey": "anothervalue"}' -fr "error"
```
Fuzz multiple locations. Match only responses reflecting the value of "VAL" keyword. Colored.
```bash
ffuf -w params.txt:PARAM -w values.txt:VAL -u https://example.org/?PARAM=VAL -mr "VAL" -c
```

**options**

mode interactif en pressant ENTRÉE,

drop un shell avec possibilité de commandes pour reconfigurer les filtres, sauvegarder l'état actuel, etc


