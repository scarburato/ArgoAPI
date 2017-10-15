# ArgoAPI [PHP]
API to view "Argo ScuolaNext" data, for students and parentss.

*Shortlink: [`https://git.io/v9LKg`](https://git.io/v9LKg)*

*Read this in other languages: [Italiano](README.md), [English](README.en.md)*

## Table of Contents
  - [1. Include APIs in your project](#include-apis-in-your-project)
  - [2. Log in](#log-in)
  - [3. Make a request](#make-a-request)
    - [Today at school](#today-at-school)
    - [Absences](#absences)
    - [Disciplinary notes](#disciplinary-notes)
    - [Daily marks](#daily-marks)
    - [Final marks](#final-marks)
    - [Homework](#homework)
    - [Lesson topics](#lesson-tolpics)
    - [Class reminder](#class-reminder)
    - [Class schedule](#class-schedule)
    - [Teachers](#teachers)

## Include APIs in your project
You must use PHP `require_once()` function to include the APIs.
```php
require_once("argoapi.php");
```

## Log in
To log in you must create an `argoUser` object, with school code, username and password as parameters.
```php
try {
	$argo = new argoUser("SCHOOL_CODE", "USERNAME", "PASSWORD");
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```
### Important!
If you need to save a session, DO NOT save the password, but save the token:
```php
$token = $argo->authToken;
```
You can log in using the token instead of the password in this way:
```php
try {
	$argo = new argoUser("SCHOOL_CODE", "USERNAME", "TOKEN", 1);
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```

## Make a request
```php
try {
	$request = $argo->REQUEST(parameters);
}
catch (Exception $e) {
	echo 'ArgoAPI exception: ', $e->getMessage(), "\n";
}
```
In the following examples, I will omit the code for handling exceptions.
You should use it.

### Today at school
Current day:
```php
$request = $argo->oggiScuola();
```
Specified day (yyyy-mm-dd):
```php
$request = $argo->oggiScuola("2017-03-14");
```
Example output:
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

### Absences
```php
$request = $argo->assenze();
```
Example output:
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

### Disciplinary notes
```php
$request = $argo->noteDisciplinari();
```
Example output:
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

### Daily marks
```php
$request = $argo->votiGiornalieri();
```
Example output:
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

### Final marks
```php
$request = $argo->votiScrutinio();
```
Example output:
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

### Homeworks
```php
$request = $argo->compiti();
```
Example output:
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

### Lesson topics
```php
$request = $argo->argomenti();
```
Example output:
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

### Class reminder
```php
$request = $argo->promemoria();
```
Example output:
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

### Class schedule
```php
$request = $argo->orario();
```
Example output:
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

### Teachers
```php
$request = $argo->docenti();
```
Example output:
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

### Translation errors
I'm sorry for any translation errors.
Please report to cristian@cristianlivella.com.
Thank you :)
