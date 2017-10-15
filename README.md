# ArgoAPI [PHP]
API per visualizzare i dati del registro elettronico "Argo ScuolaNext", per account studenti e genitori

*Shortlink: [`https://git.io/v9LKg`](https://git.io/v9LKg)*

*Leggimi in alte lingue: [Italiano](README.md), [English](README.en.md)*

## Tabella dei contenuti
  - [1. Includi le API nel tuo progetto](#includi-le-api-nel-tuo-progetto)
  - [2. Effettua il login](#effettua-il-login)
  - [3. Fai una richiesta](#fai-una-richiesta)
    - [Oggi a scuola](#oggi-a-scuola)
    - [Assenze](#assenze)
    - [Note disciplinari](#note-disciplinari)
    - [Voti giornalieri](#voti-giornalieri)
    - [Voti scrutinio](#voti-scrutinio)
    - [Compiti](#compiti)
    - [Argomenti della lezione](#argomenti-della-lezione)
    - [Promemoria](#promemoria)
    - [Orario classe](#Orario-classe)
    - [Docenti](#docenti)

## Includi le API nel tuo progetto
Devi usare la funzione `require_once()` di PHP per includere le API.
```php
require_once("argoapi.php");
```

## Effettua il login
Per effettuare il login devi creare un oggetto `argoUser`, con codice scuola, username e password come parametri.
```php
try {
	$argo = new argoUser("SCHOOL_CODE", "USERNAME", "PASSWORD");
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```
### Importante!
Se hai bisogno di salvare una sessione, NON salvare la password, ma salva il token:
```php
$token = $argo->authToken;
```
Successivamente puoi effettuare il login utilizzando il token al posto della password in questo modo:
```php
try {
	$argo = new argoUser("SCHOOL_CODE", "USERNAME", "TOKEN", 1);
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```

## Fai una richiesta
```php
try {
	$request = $argo->REQUEST(parameters);
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```
Negli esempi sotto ometterò l'handling delle eccezioni.
Voi dovreste usarlo.

### Oggi a scuola
Oggi:
```php
$request = $argo->oggiScuola();
```
Giorno specifico (yyyy-mm-dd):
```php
$request = $argo->oggiScuola("2017-03-14");
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [dati] => Array
                (
                    [datGiorno] => 2017-03-14
                    [desMateria] => S.I. CHIMICA
                    [numAnno] => 2016
                    [prgMateria] => 62
                    [prgClasse] => 1967
                    [desCompiti] => Studiare da pag.158 a pag.161
                    [prgScuola] => 2
                    [docente] => (Prof. ROSSI ASIA)
                    [codMin] => SG18224
                )

            [giorno] => 2017-03-14
            [numAnno] => 2016
            [prgAlunno] => 13349
            [prgScheda] => 1
            [prgScuola] => 2
            [tipo] => COM
            [titolo] => Compiti assegnati
            [ordine] => 40
            [codMin] => SG18224
        )

    [1] => Array
        (
            [dati] => Array
                (
                    [datGiorno] => 2017-03-14
                    [desMateria] => S.I. CHIMICA
                    [numAnno] => 2016
                    [prgMateria] => 62
                    [prgClasse] => 1967
                    [prgScuola] => 2
                    [desArgomento] => L'equilibrio chimico: reazioni incomplete e reversibili
                    [docente] => (Prof. ROSSI ASIA)
                    [codMin] => SG18224
                )

            [giorno] => 2017-03-14
            [numAnno] => 2016
            [prgAlunno] => 13349
            [prgScheda] => 1
            [prgScuola] => 2
            [tipo] => ARG
            [titolo] => Argomenti lezione
            [ordine] => 50
            [codMin] => SG18224
        )
        [...]
)
```

### Assenze
```php
$request = $argo->assenze();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [codEvento] => A
            [numOra] => 
            [datGiustificazione] => 2017-03-27
            [prgScuola] => 2
            [prgScheda] => 1
            [binUid] => ec8c5069-6d4c-4d89-a16d-dc3bb209be90
            [codMin] => SG18224
            [datAssenza] => 2017-03-25
            [numAnno] => 2016
            [prgAlunno] => 13349
            [flgDaGiustificare] => 1
            [giustificataDa] => (Prof. ROSSI ASIA)
            [desAssenza] => 
            [registrataDa] => (Prof. ROSSI MARIO)
        )
        [...]
)
```

### Note disciplinari
```php
$request = $argo->noteDisciplinari();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [prgAlunno] => 13349
            [numAnno] => 2016
            [flgVisualizzata] => S
            [prgAnagrafe] => 1387
            [prgNota] => 1
            [prgScheda] => 1
            [prgScuola] => 2
            [desNota] => L'alunno disturba la lezione
            [datNota] => 2016-12-15
            [docente] => (Prof. ROSSI ASIA)
            [codMin] => SG18224
        )
        [...]
)
```

### Voti giornalieri
```php
$request = $argo->votiGiornalieri();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [datGiorno] => 2017-04-19
            [desMateria] => GEOGRAFIA
            [prgMateria] => 89
            [prgScuola] => 2
            [prgScheda] => 1
            [codVotoPratico] => N
            [decValore] => 7.5
            [codMin] => SG18224
            [desProva] => 
            [codVoto] => 7½
            [numAnno] => 2016
            [prgAlunno] => 13349
            [desCommento] => 
            [docente] => (Prof ROSSI ASIA)
        )
        [...]
)
```

### Voti scrutinio
```php
$request = $argo->votiScrutinio();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [ordineMateria] => 2
            [desMateria] => LINGUA E LET. ITA.
            [votoOrale] => Array
                (
                    [codVoto] => 7
                )

            [prgMateria] => 16
            [prgScuola] => 2
            [prgScheda] => 1
            [votoUnico] => 1
            [prgPeriodo] => 1
            [assenze] => 1
            [codMin] => SG18224
            [suddivisione] => SO
            [numAnno] => 2016
            [prgAlunno] => 13349
            [giudizioSintetico] => 
            [prgClasse] => 1967
        )
        [...]
)
```

### Compiti
```php
$request = $argo->compiti();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [datGiorno] => 2017-04-22
            [desMateria] => S.I. BIOLOGIA
            [numAnno] => 2016
            [prgMateria] => 67
            [prgClasse] => 1967
            [desCompiti] => Rivedere la struttura degli acidi nucleici.
            [prgScuola] => 2
            [docente] => (Prof. ROSSI ASIA)
            [codMin] => SG18224
        )
        [...]
)
```

### Argomenti della lezione
```php
$request = $argo->argomenti();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [datGiorno] => 2017-04-22
            [desMateria] => S.I. BIOLOGIA
            [numAnno] => 2016
            [prgMateria] => 67
            [prgClasse] => 1967
            [prgScuola] => 2
            [desArgomento] => Recuperi e chiarimenti sugli esercizi del compito
            [docente] => (Prof. ROSSI ASIA)
            [codMin] => SG18224
        )
        [...]
)
```

### Promemoria
```php
$request = $argo->promemoria();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [desAnnotazioni] => Verifica di italiano sulla poesia
            [datGiorno] => 2017-05-11
            [numAnno] => 2016
            [prgProgressivo] => 1
            [prgClasse] => 1967
            [prgAnagrafe] => 23
            [prgScuola] => 2
            [desMittente] => A. ROSSI
            [codMin] => SG18224
        )
        [...]
)
```

### Orario classe
```php
$request = $argo->orario();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [numOra] => 1
            [giorno] => Lunedì
            [prgClasse] => 1967
            [prgScuola] => 2
            [lezioni] => Array
                (
                    [0] => Array
                        (
                            [materia] => DIRITTO ED ECON.
                            [docente] => (Prof. ROSSI ASIA)
                        )

                )

            [numGiorno] => 1
            [codMin] => SG18224
        )
        [...]
)
```

### Docenti
```php
$request = $argo->docenti();
```
Esempio output:
```
Array
(
    [0] => Array
        (
            [prgClasse] => 1967
            [prgAnagrafe] => 14
            [prgScuola] => 2
            [materie] => (S.I. BIOLOGIA)
            [docente] => Array
                (
                    [email] => 
                    [nome] => ASIA
                    [cognome] => ROSSI
                )

            [codMin] => SG18224
	    )
	    [...]
)
```
