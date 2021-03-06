# Programar en Java, usando VSCode

Requisitos:

**Java SDK 17** (para windows se puede descargar de este link https://www.oracle.com/java/technologies/downloads/#jdk17-windows)

**Visual Studio Code** (se puede descargar de https://code.visualstudio.com/download )

## Java OpenJDK17:
```
java -version
```

```
>java version "17.0.2" 2022-01-18 LTS
>Java(TM) SE Runtime Environment (build 17.0.2+8-LTS-86)
>Java HotSpot(TM) 64-Bit Server VM (build 17.0.2+8-LTS-86, mixed mode, sharing)
```
Ver información en internet:

![image](https://user-images.githubusercontent.com/40076595/169920537-6863e0bb-fd5c-4ccf-bffc-f819957c2f75.png)


## Visual Studio Code

Recomendable, la última versión, se debe usar la aplicación, con las siguientes extensiones:

1. **Extension Pack for Java**: Incluye 6 extensiones

Con esta extensión se puede crear proyectos en java, tanto sin ninguna herramienta de build, como con Maven, para usar Gradle, y Spring Boot se debe instalar sus respectivas extensiones


## Ejercicio 1

Crear proyecto sin usar herramientas para el build, project1

Usar System.out.format en lugar de System.out.println(String.format(:, hay que agregar \n para cambio de linea, también se puede usar System.out.printf https://www.javatpoint.com/java-string-format

```java
public class App {
    public static void main(String[] args) throws Exception {
        
        System.out.format("My name is %s, my last name is %s, am %d years old, am %.2f meters tall  \n", "Max","Zavaleta",48, 1.74);

        System.out.printf("Qty args: %d\n", args.length);
        int i=1;
        for (String string : args) {
            System.out.format("Args[%d]: %s\n", i,string);
            i++;
        }
        Book book = new Book(1, "100 años de soledad", "Gabriel García Marquez");
        System.out.format("Book: %s", book);
    }
}
```

Uso de "Source Action", crear constructor, agregar getters/setter, toString, etc

Uso de "Go to definition", "Go to"


Uso de "Java Project", "Outline"

```java
package model;

public class Book {
    private int id;
    private String title;
    private String author;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getAuthor() {
        return author;
    }
    public void setAuthor(String author) {
        this.author = author;
    }
    public Book() {
    }
    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
    }
}
```

Uso de "Source Action" toString 

```console
java -cp bin App
```
## Ejercicio 2
Trabajando con dependencias, en este caso, se debe copiar la dependencia (.jar) en la carpeta lib del proyecto, en este caso el driver jcbc para H2 desde https://dbschema.com/jdbc-driver/H2.html y crear el código para crear una tabla, insertar registros y finalmente consultar registros.

Para identificar el modo de conectarse, hacer doble clik en el archivo descargardo y copiar los parámetros: driver, url, user, password

project2
   
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Start");
        testDB();
        System.out.println("End");
    }
    private static void testDB() throws ClassNotFoundException, SQLException{
        //Load driver
        Class.forName("org.h2.Driver");
        
        String url="jdbc:h2:mem:test";
        String user ="sa";
        String password="";
        Connection cn = DriverManager.getConnection(url, user, password);
        System.out.printf("Conected to %s@%s\n",user,url);

        //Create table
        var sql ="""
            CREATE TABLE  PRODUCT (
                id INTEGER not NULL, 
                description VARCHAR(50),   
                brand VARCHAR(30), 
                price number, 
                PRIMARY KEY ( id ))""";     
        var st = cn.createStatement();
        st.executeUpdate(sql);
        System.out.printf("Table %s created\n", "product");

        String insertString ="insert into PRODUCT (id, description, brand, price) values ('%s','%s','%s',%f)";
        
        //Populate table
        st.executeUpdate(String.format(insertString, 1, "Test product1", "Acme", 100.98));
        st.executeUpdate(String.format(insertString, 2, "Test product 1","TNT",1786.0));
        
        //Get rows from table
        sql= "select * from product";
        var rs = st.executeQuery(sql);
        while (rs.next()){
            System.out.printf("Product-> id:%d, description:%s, brand:%s, price: %.2f\n",
            rs.getInt(1),
            rs.getString(2),
            rs.getString(3),
            rs.getDouble(4)
            );
        }
        cn.close();
    }
}
```
para ejecutar, abrir terminal en la carpeta del proyecto y ejecutar

```console
java -cp "bin;lib/h2-2.1.210.jar" App
```
