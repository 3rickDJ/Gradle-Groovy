# Gradle-Groovy
#### Using Groovy with Gradle. Implementing test with JUnit from GroovyTestCase

Si quieres usar Groovy con Gradle, este último te ofrece un [tutorial de muestra]. A través del tutorial con ayuda del commando `gradle init`. Te creará un projecto de Groovy, con una clase **App** y su respectiva clase de prueba **AppTest**. El problema surge cuando quieres usar JUnit 3, pues la clase **AppTest** fue escrita con base a Spock framework. Puedes hacer un refactor en la prueba, para utilizar JUnit (o GroovyTestCase), y en efecto correrá la tarea **test** exitosamente. Mas, te darás cuenta que si la tarea se ha ejecutado exitosamente no nos asegura que los test hayan sido corridos, pues hice fallar las pruebas a propósito, y la tarea seguía siendo exitosa.

En la estructura de la tarea test se encuentran otras tareas de las que depende. Si el código fuente fue modificado, entonces tendrá que correr las tareas **compileGroovy** o **compileTestGroovy** (lo mismo para Java). Luego de compilar los .groovy, gradle inicia la fase de escaneo dentro de los archivos .class , en ellos busca las clases que:
+ hayan heredado de `TestCase` o `GroovyTestCase`
+ hayan sido anotadas con `@RunWith`
+ algún método de la clase (o súperclase) haya sido anotado con `@Test`

> Posterior a ello, procede con el siguiente *paso*, donde ejecuta las pruebas encontradas.

De esta manera Gradle automáticamente escanean nuestras clases en búsqueda de los test.

Aunque en la documentación de Gradle, en la sección de [Test detection] hay una nota, donde nos advierte que al emplear **[JUnitPlatform]** el escaneo de clases se desactiva y en su lugar deberíamos de utilizar los métodos `include` o `exclude` para filtrar nuestras clases de pruebas.

De tal forma que si solo planeas utilizar JUnit 3 o 4, es mejor que elimines de la tarea *test* el método `useJUnitPlatform`. O en su defecto, si necesitas utilizar JUnit 5 y sus antiguas versiones, puedes agregar a tus dependencias la siguiente línea:
```
testRuntimeOnly 'org.junit.vintage:junit-vintage-engine'
```
Para poder agregar el motor JUnit Vintage, y ejecutar tus pruebas escritas en JUnit 5, sin perder la compatibilidad con JUnit 3 ó 4.

### Pruébalo

En este repositorio cada commit representa:
1. Sample inicial de Gradle
2. Refactorizar AppTest, desde Spock a JUnit 3
3. Eliminar el uso de JUnitPlatform
4. Emplear JUnitPlatform y el engine JUnit Vintage

En estos dos últimos, puedes observar que las pruebas fallan (eso es lo que esperamos). Aunque si viajas al commit 2, al ejecutarl la tarea *test* con `./gradlew test` te darás cuenta que se ejecuta exitosamente. A pesar de que la prueba está diseñada para fallar.
Te recomiendo hacer `git checkout` y `git diff` para ir viendo los cambios al projecto.
<!-- LINKS -->
[Tutorial de muestra]: https://docs.gradle.org/current/samples/sample_building_groovy_applications.html 'tutorial de gradle'
[Test detection]: https://docs.gradle.org/current/userguide/java_testing.html#sec:test_detection 'detección de tests'
[JUnitPlatform]: https://junit.org/junit5/docs/current/user-guide/ 'JUnit5'
