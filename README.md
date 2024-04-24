Lavoro di gruppo
===
Lavoro di gruppo con i compagni: Matteo C., Jomar N e Giorgia R.

## Consegna:
Modellare la struttura di un database per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d’Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d’Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.
Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.



Esercizio 23.04.2024
===

Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema allegato, eseguite le query del file allegato.
**Cosa consegnare?**
Dopo aver testato le vostre query con phpMyAdmin, riportatele in un file txt e caricatelo nella vostra repo.

1. Selezionare tutti gli studenti nati nel 1990 (160)
SELECT `date_of_birth`
FROM `students`
WHERE YEAR (`date_of_birth`) = '1990'

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT `cfu`
FROM `courses`
WHERE (`cfu`) > '10'

3. Selezionare tutti gli studenti che hanno più di 30 anni
SELECT *
FROM `students`
WHERE YEAR (`date_of_birth`) < '1994'

SELECT * 
FROM `students` 
WHERE TIMESTAMPDIFF(YEAR,`date_of_birth`, CURDATE())>30

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)
SELECT * 
FROM `courses`
WHERE `period` = 'I semestre'
AND `year` = 1

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)
SELECT * 
FROM `exams` 
WHERE `date` = '2020-06-20'
AND HOUR(`hour`) >= 14

6. Selezionare tutti i corsi di laurea magistrale (38)
SELECT * 
FROM `degrees` 
WHERE `level` = 'magistrale'

7. Da quanti dipartimenti è composta l'università? (12)
SELECT COUNT(*)
FROM `departments`

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT * 
FROM `teachers` 
WHERE `phone` IS NULL

BONUS
===

1. Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(*), YEAR (`enrolment_date`)
FROM `students`
GROUP BY YEAR (`enrolment_date`); 

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT COUNT(*), `office_address`
FROM `teachers` 
GROUP BY `office_address`

3. Calcolare la media dei voti di ogni appello d'esame
SELECT AVG(`vote`), `exam_id`
FROM `exam_student`
GROUP BY `exam_id`

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT COUNT(*), `department_id`
FROM `degrees` 
GROUP BY `department_id`


giorno 3
===
1. elezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT `students`.*
FROM `students`
JOIN `degrees` ON `degrees`. `id` = `students`. `degree_id`
WHERE `degrees`.`name` = 'Corso di laurea in economia'

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze
SELECT `degrees`.* 
FROM `degrees` 
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze'

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `courses`.* 
FROM `courses` 
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id`= 44 

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome