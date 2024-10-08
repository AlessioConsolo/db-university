Query con SELECT ---------------------------------------------------------------------------

1. Selezionare tutti gli studenti nati nel 1990 (160)

   SELECT \*
   FROM `students`
   WHERE `date_of_birth` BETWEEN '1990-01-01' AND '1990-12-31';

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

   SELECT \*
   FROM `courses`
   WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni

   SELECT \*
   FROM `students`
   WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)

   SELECT \*
   FROM `courses`
   WHERE `period` = 'I semestre' AND `year` = '1';

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)

   SELECT \*
   FROM `exams`
   WHERE `date` = '2020-06-20' AND `hour` >= '14:00:00';

6. Selezionare tutti i corsi di laurea magistrale (38)

   SELECT \*
   FROM `degrees`
   WHERE `level` = 'magistrale';

7. Da quanti dipartimenti è composta l'università? (12)

   SELECT COUNT(\*) AS "Totale dipartimenti"
   FROM `departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

   SELECT COUNT(\*) AS "Numero insegnanti"
   FROM `teachers`
   WHERE `phone` IS NOT NULL;

Bonus SELECT: -----------------------------------------------------------------------------

9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo
   degree_id, inserire un valore casuale)

10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126

11. Eliminare dalla tabella studenti il record creato precedentemente al punto 9

Query con GROUP BY ------------------------------------------------------------------------

1. Contare quanti iscritti ci sono stati ogni anno

   SELECT YEAR(`enrolment_date`) AS "Anno", COUNT(\*) AS "Numero Iscrizioni"
   FROM `students`
   GROUP BY YEAR(`enrolment_date`)
   ORDER BY YEAR(`enrolment_date`)

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

   SELECT `office_number`, COUNT(\*) AS "Numero Insegnanti"
   FROM `teachers`
   GROUP BY `office_number`

3. Calcolare la media dei voti di ogni appello d'esame

   SELECT `student_id`, AVG(`vote`) AS "Media Voti"
   FROM `exam_student`
   GROUP BY `student_id`

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

   SELECT `department_id` AS "ID Dipartimento",
   COUNT(`department_id`) AS "Corsi di Laurea"
   FROM `degrees`
   GROUP BY `department_id`

Query con JOIN ----------------------------------------------------------------------------

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

   SELECT students.id, students.name AS "Nome", students.surname AS "Cognome", degrees.name AS "Corso di Laurea"
   FROM `students`
   JOIN degrees ON students.degree_id = degrees.department_id
   WHERE degrees.name = "Corso di Laurea in Economia";

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

   SELECT departments.name AS "Dipartimento", degrees.name AS "Corso di Laurea", degrees.level AS "Livello"
   FROM degrees
   JOIN departments ON departments.id LIKE 7 = degrees.department_id
   WHERE degrees.level = "magistrale"
   AND departments.id = 7;

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

   SELECT course_teacher.teacher_id AS "ID Insegnante", teachers.name AS "Nome", teachers.surname AS "Cognome", course_teacher.course_id AS "ID Corso"
   FROM `course_teacher`
   JOIN teachers ON course_teacher.teacher_id = teachers.id
   WHERE course_teacher.teacher_id = 44

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

   SELECT students.id, students.name AS "Nome", students.surname AS "Cognome", degrees.name AS "Corso di Laurea", departments.name AS "Dipartimento"
   FROM `students`
   JOIN degrees ON students.degree_id = degrees.id
   JOIN departments ON degrees.department_id = departments.id
   ORDER BY students.surname, students.name;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

   SELECT degrees.name AS "Corso di Laurea", course_teacher.course_id AS "ID Corso", teachers.name AS "Nome", teachers.surname AS "Cognome"
   FROM `degrees`
   JOIN course_teacher ON degrees.id = course_teacher.course_id
   JOIN teachers ON teachers.id = course_teacher.teacher_id;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

Bonus JOIN: -------------------------------------------------------------------------------

7. Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
