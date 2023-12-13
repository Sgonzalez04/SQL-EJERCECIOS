1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

    ```sql
    SELECT CONCAT(apellido1,' ', apellido2,' ', nombre)
    FROM persona
    WHERE tipo = 'alumno'
    ORDER BY
    apellido1 ASC,apellido2 ASC,nombre ASC;
    ```

2. Averigua el nombre y los dos apellidos de los alumnos que **no** han dado de alta su número de teléfono en la base de datos.

    ```sql
    SELECT CONCAT(apellido1,' ', apellido2,' ', nombre)
    FROM persona
    WHERE tipo = 'alumno' AND telefono IS NULL
    ```

3. Devuelve el listado de los alumnos que nacieron en `1999`.

    ```sql
    SELECT
    id,nombre,apellido1,apellido2,fecha_nacimiento
    FROM persona
    WHERE
    tipo = 'alumno' AND YEAR(fecha_nacimiento) = 1999
    ```

4. Devuelve el listado de `profesores` que **no** han dado de alta su número de teléfono en la base de datos y además su nif termina en `K`.

    ```sql
    SELECT id,nombre,apellido1,apellido2,nif
    FROM persona
    WHERE tipo = 'profesor' AND telefono IS NULL AND nif LIKE '%K'
    ```

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador `7`.

    ```sql
    SELECT id,nombre,cuatrimestre,id_grado
    FROM asignatura
    WHERE cuatrimestre = 1 AND id_grado = 7
    ```

6. Devuelve un listado con los datos de todas las **alumnas** que se han matriculado alguna vez en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT DISTINCT CONCAT(P.nombre,' ',P.apellido1,' ', P.apellido2) AS nombre, P.sexo, G.nombre AS nombre_grado
    FROM persona P
    INNER JOIN alumno_se_matricula_asignatura MA ON MA.id_alumno = P.id
    INNER JOIN asignatura A ON A.id = MA.id_asignatura
    INNER JOIN grado G ON G.id = A.id_grado
    WHERE P.sexo = 'M' AND G.nombre ='Grado en Ingeniería Informática (Plan 2015)' AND P.tipo = 'alumno'
    ```

7. Devuelve un listado con todas las asignaturas ofertadas en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT A.nombre AS Asignatura, G.nombre AS nombre_grado
    FROM asignatura A
    INNER JOIN grado G ON G.id = A.id_grado
    WHERE G.nombre ='Grado en Ingeniería Informática (Plan 2015)'
    ```

8. Devuelve un listado de los `profesores` junto con el nombre del `departamento` al que están vinculados. El listado debe devolver cuatro columnas, `primer apellido, segundo apellido, nombre y nombre del departamento.` El resultado estará ordenado alfabéticamente de menor a mayor por los `apellidos y el nombre.`

    ```sql
    SELECT CONCAT(P.apellido1,' ',P.apellido2,' ',P.nombre) AS nombre, D.nombre AS nombre_Departamento
    FROM persona P
    INNER JOIN profesor PR ON PR.id_profesor = P.id
    INNER JOIN departamento D ON D.id = PR.id_departamento
    ORDER BY p.apellido1 ASC, p.apellido2 ASC, p.nombre ASC
    ```

9. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif `26902806M`.

    ```sql
    SELECT CONCAT(P.apellido1,' ',P.apellido2,' ',P.nombre) AS nombre, A.nombre AS asignaturas, C.anyo_inicio, C.anyo_fin
    FROM persona P
    INNER JOIN alumno_se_matricula_asignatura MA ON MA.id_alumno = P.id
    INNER JOIN asignatura A ON A.id = MA.id_asignatura 
    INNER JOIN curso_escolar C ON C.id = MA.id_curso_escolar
    WHERE P.nif='26902806M'
    ```

10. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el `Grado en Ingeniería Informática (Plan 2015)`.

     ```sql
    SELECT DISTINCT D.nombre AS nombre_departamento
    FROM departamento D
    INNER JOIN profesor P ON P.id_departamento = D.id
    INNER JOIN asignatura A ON A.id_profesor = P.id_profesor
    INNER JOIN grado G ON G.id = A.id_grado
    WHERE G.nombre = 'Grado en Ingeniería Informática (Plan 2015)'
     ```

11. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

     ```sql
    SELECT DISTINCT P.id, CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
    FROM persona P
    INNER JOIN alumno_se_matricula_asignatura MA ON MA.id_alumno = P.id
    INNER JOIN curso_escolar C ON C.id = MA.id_curso_escolar
    WHERE C.anyo_inicio = 2018 AND C.anyo_fin = 2019;
     ```

12. Devuelve un listado con los nombres de **todos** los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

     ```sql
    SELECT P.nombre, P.apellido1, P.apellido2, D.nombre
    FROM persona P
    INNER JOIN profesor PR ON PR.id_profesor = P.id
    INNER JOIN departamento D ON D.id = PR.id_departamento
    ORDER BY D.nombre, P.apellido1, P.apellido2, P.nombre;
     ```

13. Devuelve un listado con los profesores que no están asociados a un departamento.Devuelve un listado con los departamentos que no tienen profesores asociados.

     ```sql
    SELECT CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
    FROM persona P
    WHERE P.tipo='profesor' AND P.id NOT IN (SELECT id_departamento FROM profesor)

    SELECT D.nombre AS departamento 
    FROM departamento D
    WHERE D.id NOT IN (SELECT id_departamento FROM profesor)
     ```

14. Devuelve un listado con los profesores que no imparten ninguna asignatura.

     ```sql
    SELECT CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
    FROM persona P
    INNER JOIN profesor PR ON PR.id_profesor = P.id
    LEFT JOIN asignatura A ON A.id_profesor = PR.id_profesor
    WHERE A.id_profesor IS NULL
     ```

15. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

     ```sql
    SELECT A.nombre
    FROM asignatura A
    LEFT JOIN profesor P ON P.id_profesor = A.id_profesor
    WHERE A.id_profesor IS NULL
     ```

16. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

     ```sql
    SELECT D.nombre AS nombre_departamento, A.nombre AS nombre_asignatura
    FROM departamento D
    INNER JOIN asignatura A ON A.id = D.id
    LEFT JOIN curso_escolar C ON C.id = A.id
    WHERE C.id IS NULL
    ORDER BY D.nombre, A.nombre;
     ```

17. Devuelve el número total de **alumnas** que hay.

     ```sql
    SELECT COUNT(*) AS alumnas
    FROM persona P
    WHERE P.tipo = 'alumno' and P.sexo='M';
     ```

18. Calcula cuántos alumnos nacieron en `1999`.

     ```sql
    SELECT COUNT(*) AS '1999'
    FROM persona P
    WHERE YEAR(P.fecha_nacimiento) = 1999;
     ```

19. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

     ```sql
    SELECT D.nombre, COUNT(*) AS numero_profesores
    FROM departamento D
    INNER JOIN profesor P ON P.id_departamento = D.id
    GROUP BY D.nombre
    ORDER BY numero_profesores DESC;
     ```

20. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

     ```sql
    SELECT D.nombre, COUNT(DISTINCT P.id_profesor) AS numero_profesores
    FROM departamento D
    LEFT JOIN profesor P ON P.id_departamento = D.id
    GROUP BY D.nombre
    ORDER BY numero_profesores DESC;
     ```

21. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

     ```sql
       # Consulta Aqui
     ```

22. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de `40` asignaturas asociadas.

     ```sql
       # Consulta Aqui
     ```

23. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

     ```sql
       # Consulta Aqui
     ```

24. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

     ```sql
       # Consulta Aqui
     ```

25. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

     ```sql
       # Consulta Aqui
     ```

26. Devuelve todos los datos del alumno más joven.

     ```sql
       # Consulta Aqui
     ```

27. Devuelve un listado con los profesores que no están asociados a un departamento.

     ```sql
       # Consulta Aqui
     ```

28. Devuelve un listado con los departamentos que no tienen profesores asociados.

     ```sql
       # Consulta Aqui
     ```

29. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

     ```sql
       # Consulta Aqui
     ```

30. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

     ```sql
       # Consulta Aqui
     ```

31. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

     ```sql
       # Consulta Aqui
     ```
