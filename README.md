## Parte Teórica

## qué es Markdown

**Markdown** es un lenguaje de marcado ligero creado por John Gruber y Aaron Swartz en 2004. Su objetivo principal es permitir la escritura de texto con formato (títulos, negritas, listas, enlaces, bloques de código, etc.) de manera sencilla, legible y estructurada, utilizando caracteres de texto plano. Es ampliamente utilizado en plataformas como GitHub para la creación de archivos de documentación (`README.md`), páginas web estáticas, documentación técnica y notas.

---

## GIT

## 1. ¿Qué es un repositorio en Git y cómo se diferencia de un proyecto "normal"?
Un **repositorio en Git** es un contenedor o estructura de almacenamiento donde Git guarda todos los archivos de un proyecto junto con el historial completo de cambios, versiones, ramas (*branches*), metadatos y registros de confirmaciones (*commits*). 

* **Diferencia con un proyecto "normal":** Un proyecto normal es simplemente una carpeta local con archivos en su estado actual. Un repositorio de Git incluye una carpeta oculta llamada `.git`, la cual actúa como una base de datos que rastrea la evolución del código a lo largo del tiempo, permitiendo revertir cambios, trabajar en paralelo y colaborar sin sobrescribir el trabajo de otros.

---

## 2. ¿Cuáles son las tres áreas principales de Git (working directory, staging area/index y repository) y qué papel cumple cada una?

1. **Working Directory (Directorio de Trabajo):** Es el espacio local donde te encuentras trabajando actualmente. Contiene los archivos extraídos del repositorio para que los edites, agregues o elimines. Los cambios en esta área aún no están registrados en el historial de Git.
2. **Staging Area / Index (Área de Preparación):** Es una zona intermedia de paso (*borrador*). Aquí se colocan los archivos modificados que han sido seleccionados mediante el comando `git add`. Permite agrupar de forma organizada únicamente los cambios que deseas incluir en el próximo *commit*.
3. **Repository (Repositorio / `.git`):** Es la base de datos permanente donde Git almacena la historia confirmada del proyecto. Cuando ejecutas `git commit`, los cambios guardados en el *Staging Area* se registran de manera definitiva en esta área como un nuevo punto de restauración (*commit*).

---

## 3. ¿Cómo representa Git los cambios internamente? (objetos blob, tree, commit y tag)

Git es un sistema de archivos orientados a objetos en forma de grafo dirigido acíclico (DAG). Modela los datos mediante cuatro objetos fundamentales:

* **Blob (Binary Large Object):** Almacena el contenido crudo de un archivo individual sin sus metadatos (no guarda el nombre del archivo ni sus permisos, solo el contenido).
* **Tree (Árbol):** Representa directorios y carpetas. Un objeto *tree* contiene una lista de punteros hacia *blobs* (archivos) o hacia otros *trees* (subdirectorios), asociándoles sus nombres y permisos correspondientes.
* **Commit:** Representa un estado o foto puntual del repositorio en un momento dado. Contiene un puntero al *tree* raíz del proyecto, metadatos (autor, fecha, mensaje) y punteros a los *commits* padres.
* **Tag:** Es una etiqueta o puntero permanente asignado a un *commit* específico, comúnmente utilizado para marcar versiones de lanzamiento (ej. `v1.0.0`).

---

## 4. ¿Cómo se crea un commit y qué información almacena un objeto commit?

Un *commit* se crea cuando se ejecutan los siguientes pasos:
1. Se modifican archivos en el *Working Directory*.
2. Se añaden los cambios al *Staging Area* mediante `git add <archivo>`.
3. Se confirma el cambio guardándolo en la base de datos de Git ejecutando `git commit -m "Mensaje descriptivo"`.

**Información que almacena un objeto commit:**
* **Hash SHA-1 / SHA-256:** Un identificador único de 40 caracteres generado criptográficamente.
* **Puntero al Tree raíz:** Referencia al estado general del directorio en ese instante.
* **Punteros a Commits Padres:** Referencia al *commit* inmediatamente anterior (o múltiples padres en caso de un *merge*).
* **Metadatos del Autor y Committer:** Nombre, correo electrónico y fecha/hora de creación y confirmación.
* **Mensaje de Commit:** La descripción textual introducida por el desarrollador.

---

## 5. ¿Cuál es la diferencia entre `git pull` y `git fetch`?

* **`git fetch`:** Descarga las novedades, ramas y referencias desde el repositorio remoto hacia el repositorio local, pero **no modifica ni combina** nada en tu directorio de trabajo actual. Te permite revisar qué cambios existen antes de unirlos.
* **`git pull`:** Realiza una descarga de los cambios remotos y **los combina automáticamente** en la rama actual. En términos prácticos, `git pull` es la combinación de ejecutar `git fetch` seguido de `git merge`.

---

## 6. ¿Qué es un branch (rama) en Git y cómo Git gestiona los punteros a commits?

Un **branch (rama)** en Git no es una copia de la carpeta del proyecto, sino simplemente un **puntero móvil y ligero** que apunta a un *commit* específico dentro del historial de desarrollo. 

**Gestión de punteros:**
* Git utiliza la referencia especial llamada **`HEAD`** para saber en qué rama y en qué *commit* te encuentras trabajando actualmente.
* Cuando realizas un nuevo *commit*, el puntero de la rama actual se desplaza automáticamente hacia adelante para apuntar a la nueva confirmación realizada, mientras que los punteros de las demás ramas permanecen intactos.

---

## 7. ¿Cómo se realiza un merge y qué conflictos pueden surgir? ¿Cómo se resuelven?

Un **merge** integra los cambios de una rama origen en la rama de destino actual mediante el comando `git merge <nombre-rama>`.

* **Surgimiento de Conflictos:** Ocurren cuando dos personas han modificado las mismas líneas de un mismo archivo en ramas distintas con contenidos diferentes, o si un archivo fue eliminado en una rama pero editado en otra. Git no puede determinar automáticamente cuál versión conservar.
* **Resolución de Conflictos:**
  1. Git marca los archivos en conflicto agregando delimitadores visuales (`<<<<<<<`, `=======`, `>>>>>>>`).
  2. El desarrollador abre los archivos afectados, edita manualmente el código para conservar la versión correcta y elimina las marcas de conflicto.
  3. Se agregan los archivos resueltos al área de preparación mediante `git add <archivo>`.
  4. Se finaliza la integración realizando un `git commit`.

---

## 8. ¿Cómo funciona el área de staging (`git add`) y qué pasa si omito este paso?

El **área de staging** sirve como una capa de preparación donde se seleccionan y organizan los cambios exactos que formarán parte de la siguiente foto del proyecto. Al ejecutar `git add`, Git calcula el hash del contenido modificado, crea los objetos *blob* necesarios en la carpeta `.git` y actualiza el índice.

* **Si se omite el paso (`git add`):** Si intentas ejecutar `git commit` directamente sin pasar por *staging*, Git te indicará que no hay cambios preparados para confirmar (*no changes added to commit*), ignorando las modificaciones realizadas en el *working directory*.

---

## 9. ¿Qué es el archivo `.gitignore` y cómo influye en el seguimiento de archivos?

El archivo `.gitignore` es un archivo de texto plano ubicado en el proyecto que especifica patrones de nombres de archivos, directorios y extensiones que Git debe **ignorar deliberadamente**.

* **Influencia:** Los archivos que coincidan con las reglas de `.gitignore` no aparecerán como "no rastreados" (*untracked*) al ejecutar `git status`, impidiendo que por error se suban al repositorio archivos confidenciales (claves API, credenciales), dependencias pesadas (`node_modules`), o archivos temporales de compilación/IDE (`.class`, `target/`, `.vscode/`).

---

## 10. ¿Cuál es la diferencia entre un "commit amend" (`--amend`) y un nuevo commit?

* **`git commit --amend`:** Modifica o reemplaza el **último commit** realizado. Permite agregar archivos olvidados al *staging* o corregir el mensaje del último commit. No crea un commit adicional, sino que genera un objeto *commit* totalmente nuevo con el mismo padre y actualiza la referencia.
* **Nuevo Commit:** Crea una confirmación independiente registrada cronológicamente después de la anterior, aumentando la longitud del historial de commits.

---

## 11. ¿Cómo se utiliza `git stash` y en qué escenarios es útil?

`git stash` almacena temporalmente los cambios modificados en el *working directory* y *staging area* en una pila (*stack*) interna de almacenamiento, dejando el directorio de trabajo limpio y en el estado del último *commit*.

* **Comandos comunes:**
  * `git stash` / `git stash push`: Guarda los cambios temporales.
  * `git stash pop`: Recupera los últimos cambios guardados y los elimina de la pila.
  * `git stash list`: Muestra los estados almacenados.
* **Escenarios útiles:** Cuando estás a mitad de una tarea incompleta y necesitas cambiar urgentemente de rama para corregir un error (*hotfix*), o cuando necesitas hacer un `git pull` y tienes cambios locales no confirmados que interfieren.

---

## 12. ¿Qué mecanismos ofrece Git para deshacer cambios? (por ejemplo, `git reset`, `git revert`, `git checkout`)

* **`git checkout`:** Utilizado históricamente para descartar cambios no guardados en el *working directory* (`git checkout -- <archivo>`) o para moverse entre ramas/commits sin alterar el historial.
* **`git reset`:** Mueve el puntero de la rama a un *commit* anterior. Puede actuar de tres formas:
  * `--soft`: Mantiene los cambios en el *staging area*.
  * `--mixed` (por defecto): Mantiene los cambios en el *working directory*, eliminándolos del *staging*.
  * `--hard`: Borra todos los cambios del *staging* y del *working directory* de forma permanente.
* **`git revert`:** Crea un **nuevo commit** que deshace exactamente los cambios introducidos por un *commit* anterior. Es la alternativa más segura para repositorios públicos porque preserva la integridad del historial.

---

## 13. ¿Cómo funciona la configuración de remotos (origin, upstream) y qué comandos uso para gestión de forks?

* **`origin`:** Es el alias por defecto que Git le asigna al repositorio remoto principal del cual clonaste tu proyecto local.
* **`upstream`:** Es un alias secundario convención para referenciar al repositorio original central cuando trabajas desde una copia personal (*fork*).

**Comandos para gestión de remotos y forks:**
* `git remote add upstream <URL-repositorio-original>`: Añade el repositorio original.
* `git remote -v`: Muestra la lista de servidores remotos configurados.
* `git fetch upstream`: Descarga las actualizaciones del repositorio original.
* `git merge upstream/main`: Integra los cambios actualizados del proyecto original a tu copia local.

---

## 14. ¿Cómo puedo inspeccionar el historial de commits? (por ejemplo, `git log`, `git diff`, `git show`)

* **`git log`:** Muestra la lista cronológica de los *commits* realizados en la rama actual (incluye hashes, autores, fechas y mensajes).
* **`git diff`:** Muestra las diferencias línea por línea entre distintas áreas (entre *working directory* y *staging*, entre *commits*, o entre ramas).
* **`git show <commit_hash>`:** Muestra en detalle la información completa y los cambios específicos de contenido introducidos por un *commit* determinado.

---

## Programación

---

## 15. ¿Cuáles son los tipos de datos primitivos en Java?

Java posee **8 tipos de datos primitivos** clasificados en 4 categorías:

1. **Enteros:**
   * `byte` (8 bits)
   * `short` (16 bits)
   * `int` (32 bits)
   * `long` (64 bits)
2. **Punto Flotante (Decimales):**
   * `float` (32 bits)
   * `double` (64 bits)
3. **Caracteres:**
   * `char` (16 bits, código Unicode)
4. **Lógicos / Boleanos:**
   * `boolean` (`true` o `false`)

---

## 16. ¿Cómo funcionan las estructuras de control de flujo como `if`, `else`, `switch` y bucles en Java?

Las estructuras de control determinan el orden de ejecución de las instrucciones de un programa:

* **Estructuras Condicionales:**
  * **`if / else`:** Evalúa una condición booleana. Si es verdadera, ejecuta un bloque de código; de lo contrario, ejecuta el bloque alternativo.
  * **`switch`:** Permite seleccionar uno entre múltiples bloques de código a ejecutar según el valor de una expresión (compatible con tipos `int`, `char`, `String`, `enum`).
* **Estructuras Repetitivas (Bucles):**
  * **`for`:** Ejecuta un bloque de código un número conocido de veces, controlando una variable de iteración.
  * **`while`:** Evalúa una condición antes de cada iteración y ejecuta el bloque mientras sea verdadera.
  * **`do-while`:** Ejecuta el bloque de código al menos una vez y luego repite mientras la condición evaluada al final sea verdadera.

---

## 17. ¿Por qué es importante usar nombres significativos para variables y métodos?

Es fundamental por los siguientes aspectos técnicos y de ingeniería de software:
* **Mantenibilidad y Legibilidad:** Permite que cualquier desarrollador (o tú mismo en el futuro) entienda rápidamente qué almacena una variable o qué función cumple un método sin requerir comentarios excesivos.
* **Reducción de Errores:** Evita confusiones operativas con variables de propósito ambiguo (ej. usar `totalPrecioVenta` en lugar de `x`).
* **Autodocumentación del Código:** El código limpio debe expresarse por sí mismo, reflejando de forma explícita el dominio del problema.

---

## 18. ¿Qué es la Programación Orientada a Objetos (POO)?

La **Programación Orientada a Objetos (POO)** es un paradigma de programación estructurado en torno a **"objetos"** en lugar de funciones o lógica secuencial. Un objeto combina **estado** (atributos/datos) y **comportamiento** (métodos/funciones), modelando entidades del mundo real o conceptos abstractos para construir aplicaciones modulares, reutilizables y escalables.

---

## 19. ¿Cuáles son los cuatro pilares de la Programación Orientada a Objetos?

1. **Encapsulamiento:** Oculta los detalles internos del estado de un objeto y restringe el acceso directo desde el exterior, exponiendo solo interfaces seguras (métodos *getters* y *setters*).
2. **Abstracción:** Oculta la complejidad de la implementación y muestra únicamente las características esenciales necesarias para interactuar con la entidad.
3. **Herencia:** Permite que una clase hija (*subclase*) adquiera las propiedades y métodos de una clase padre (*superclase*), promoviendo la reutilización de código.
4. **Polimorfismo:** Permite que diferentes clases respondan al mismo mensaje o llamado de método de maneras distintas (mediante sobrecarga o sobrescritura de métodos).

---

## 20. ¿Qué es la herencia en POO y cómo se utiliza en Java?

La **herencia** es el mecanismo mediante el cual una clase deriva de otra, heredando sus atributos y métodos no privados.

* **Uso en Java:** Se implementa mediante la palabra clave **`extends`**. Java soporta herencia simple de clases (una clase solo puede extender directamente de una superclase).

```java
// Ejemplo conceptual
class Animal {
    void hacerSonido() {
        System.out.println("Sonido genérico");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("Guau guau");
    }
    
```
## 21. ¿Qué son los modificadores de acceso y cuáles son los más comunes en Java?

Los **modificadores de acceso** son palabras clave que definen el alcance, visibilidad o nivel de protección de clases, variables, métodos y constructores.

**Los más comunes en Java:**
1. **`private`:** Accesible únicamente dentro de la misma clase donde fue declarado (máximo nivel de encapsulamiento).
2. **`default` (paquete / sin palabra clave):** Accesible solo dentro de las clases pertenecientes al mismo paquete.
3. **`protected`:** Accesible en el mismo paquete y en subclases derivadas, incluso si están en otros paquetes.
4. **`public`:** Accesible desde cualquier clase en cualquier paquete del proyecto.

---

## 22. ¿Qué es una variable de entorno y por qué son importantes para Java o la programación en general?

Una **variable de entorno** es un valor dinámico guardado a nivel del sistema operativo que influye en el comportamiento de los procesos y programas en ejecución.

* **Importancia en Java y Programación:**
  * **Configuración del Entorno (`JAVA_HOME` y `PATH`):** Permite al sistema y a las herramientas de desarrollo (como VS Code, Maven, Gradle) localizar la instalación del JDK y ejecutar binarios de Java desde cualquier directorio de la terminal.
  * **Seguridad:** Permite separar credenciales sensibles (claves de API, contraseñas de bases de datos) del código fuente.
  * **Portabilidad:** Permite ejecutar la misma aplicación en diferentes entornos (Desarrollo, Pruebas, Producción) simplemente cambiando los valores de entorno sin modificar el código.

  ---

## Parte Práctica

## 1. Calculadora Básica (Suma, Resta, Multiplicación y División)

```java
import java.util.Scanner;

public class Calculadora {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("=== CALCULADORA BÁSICA ===");
        System.out.print("Ingrese el primer número: ");
        double num1 = scanner.nextDouble();

        System.out.print("Ingrese el segundo número: ");
        double num2 = scanner.nextDouble();

        System.out.println("\nSeleccione la operación:");
        System.out.println("1. Suma (+)");
        System.out.println("2. Resta (-)");
        System.out.println("3. Multiplicación (*)");
        System.out.println("4. División (/)");
        System.out.print("Opción: ");
        int opcion = scanner.nextInt();

        switch (opcion) {
            case 1:
                System.out.println("Resultado: " + (num1 + num2));
                break;
            case 2:
                System.out.println("Resultado: " + (num1 - num2));
                break;
            case 3:
                System.out.println("Resultado: " + (num1 * num2));
                break;
            case 4:
                if (num2 != 0) {
                    System.out.println("Resultado: " + (num1 / num2));
                } else {
                    System.out.println("Error: No se puede dividir entre cero.");
                }
                break;
            default:
                System.out.println("Opción no válida.");
                break;
        }

        scanner.close();
    }
}

```
## 2. Contador de Vocales y Consonantes

```java
import java.util.Scanner;

public class ContadorVocalesConsonantes {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese una palabra (en minúsculas y sin acentos): ");
        String palabra = scanner.nextLine();

        int vocales = 0;
        int consonantes = 0;

        for (int i = 0; i < palabra.length(); i++) {
            char letra = palabra.charAt(i);

            if (letra == 'a' || letra == 'e' || letra == 'i' || letra == 'o' || letra == 'u') {
                vocales++;
            } else if (letra >= 'a' && letra <= 'z') {
                consonantes++;
            }
        }

        System.out.println("Número de vocales: " + vocales);
        System.out.println("Número de consonantes: " + consonantes);

        scanner.close();
    }
}

```
## 3. Invertir una Cadena de Texto

```java
import java.util.Scanner;

public class InvertirCadena {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese una cadena de texto: ");
        String texto = scanner.nextLine();

        String textoInvertido = "";

        for (int i = texto.length() - 1; i >= 0; i--) {
            textoInvertido += texto.charAt(i);
        }

        System.out.println("Texto invertido: " + textoInvertido);

        scanner.close();
    }
}