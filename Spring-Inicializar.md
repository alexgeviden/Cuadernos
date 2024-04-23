# Como Inicializar Springboot para realizar un CRUD

## Mapear URL En springboot

#### Colocamos anotaciones que van con el '@' , con esto le decimos que esto es el controlador y que redireccione a la ruta especificada en RequestMapping


```Java

@Controller
@ResponseBody
public class EmpleadoControl {

@RequestMapping("/empleados")
    public List<Empleado> listarEmpleados(){
        List<Empleado> listaEmpleados = new ArrayList<>();

        // Creamos 5 objetos Empleado y los agregamos a la lista
        listaEmpleados.add(new Empleado(1, "Martin Gonzalez", "Ventas"));
        listaEmpleados.add(new Empleado(2, "Ana Lopez", "Marketing"));
        listaEmpleados.add(new Empleado(3, "Juan Ramirez", "Recursos Humanos"));
        listaEmpleados.add(new Empleado(4, "Maria Sanchez", "Contabilidad"));
        listaEmpleados.add(new Empleado(5, "Carlos Martinez", "Tecnología"));
        return listaEmpleados;
    }
}

```

## Añadimos las dependencias







```Java
spring.application.name=EmpleadoAplication
spring.datasource.url=jdbc:mysql://localhost:3306/springproject
spring.datasource.username=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

### Creamos las clases que despues seran conectadas con la Base de Datos
Tienen que tener constructores , getters y setters  para poder correr el proyecto 


```Java
package alexander.EmpleadoAplication.Entidades;

public class Empleado {
    int idempleado;
    String Nombre;
    String Depart;

    public Empleado(int idempleado, String nombre, String depart) {
        this.idempleado = idempleado;
        Nombre = nombre;
        Depart = depart;
    }}
```

## Metodo Get para conectar el servicio con las clases



En el servicio usamos el comentario @Service para que se conozca como un servicio y se pueda inyectar en las clases que lo necesiten  

Ademas esta sera la clase que llame a los objetos que hemos creado. Y la que llamaremos con el controlador para enviar los datos


```Java
@Service
public class ServicioEmpleado {

    List<Empleado> listaEmpleados = Arrays.asList(
            new Empleado(1, "Martin Gonzalez", "Ventas"),
            new Empleado(2, "Ana Lopez", "Marketing"),
            new Empleado(3, "Juan Ramirez", "Recursos Humanos"),
            new Empleado(4, "Maria Sanchez", "Contabilidad"),
            new Empleado(5, "Carlos Martinez", "Tecnología")

    );

public List<Empleado> getAllEmpleados(){
            return listaEmpleados;
    }

    public Empleado getAnEmpleado(int id){
    return listaEmpleados.stream().filter(e ->(
            e.getIdempleado() == id)).findFirst().get();

    }

}
```

En el Controlador usamos la anotacion @Controler y el metodo que queremos que se ejecute cuando se acceda a la ruta.  

Usamos @RequestingMapping para decirle la ruta que queremos en la URL.  

Ponemos @AutoWired para que sepa que la clase que estamos empleando para crear el objeto esta en el servicio.  


```Java
@Controller
@ResponseBody
public class EmpleadoControl {

    @Autowired
    ServicioEmpleado ser;

@RequestMapping("/empleados")
    public List<Empleado> listarEmpleados(){


        return ser.getAllEmpleados();
    }
    @RequestMapping("/empleados/{id}")
    public Empleado findAnEmpleado(@PathVariable int id){
    return ser.getAnEmpleado(id);
    }
}
```

Para declarar la variable que viene entregada desde la url usamos las llaves {} y el nombre de la variable.   

Luego en nuestra funcion usamos @PathVariable para recoger la variable que se nos envia por el navegador y la guardamos en una variable.    

Para devolver el valor de la variable usamos el metodo .get() y el nombre de la variable.

## Metodo Post

Añadimos esos valores a @RequestMapping ya que po defecto usa el metodo get y hay que especificar que queremos usar otro metodo  
  
Si lo que buscamos es recoger lo enviado desde nuestro body usamos @Requestbody en la declaracion de variable.


```Java
    @RequestMapping(value = "/empleados" , method = RequestMethod.POST)
    public void createEmpleado(@RequestBody Empleado emp){
    ser.createEmpleado(emp);
    }
```

Ese metodo de crear empleado se encuentra en ServicioEmpleado , aligual que la variable 'ser' que se encuentra declarada al inicio del Servicio


```
    public void createEmpleado(Empleado emp){

    listaEmpleados.add(emp);

    }
```

## Redirecciones con anotacion 

Podemos obvias @RequestMapping usando esta avrebiatura : 

```
@GetMapping

@PostMapping

@PutMapping

@DeleteMapping

@PatchMapping
```

## Creacion de Interfaz para la conexion SQL

Creamos un nuevo paquete de repositorios , dentro vamos a crear interfaces para cada clases.  
Extenderemos estas clases usando el repositorio JPA para que sea capaz de hacer y ejecutar consultas.


```Java
package alexander.EmpleadoAplication.Repo;

import alexander.EmpleadoAplication.Entidades.Empleado;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.config.JpaRepositoryConfigExtension;

public interface EmpleadoRepo extends JpaRepository<Empleado, Integer> {

    // Crud para las clases

    
}

```

Ademas hemos de crear un repositorio por cada clase  
El cual anotamos como @Entity para que sea reconocible  
Usamos @Id para que cree una tabla identificadora y @GeneratedValue para que sea autoincrementable.  
Despues creara las tablas necesarias con la informacion que le hemos proporcionado  



```Java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Prueba {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;

```

Para la conexion con la base hemos de escribir algo como esto en Aplicationpropperties  
Donde marcaos la url de la base de datos y el usuario y contraseña ademas del dialectoq ue usara spring para la comunacion.  
En la ultimainstancia entregamos los permisos a springboot para que haga registrso o eliminaciones en la base de datos.


```Java
spring.datasource.url=jdbc:sqlite:proyecto.db
spring.datasource.driver-class-name=org.sqlite.JDBC
spring.jpa.database-platform=org.hibernate.community.dialect.SQLiteDialect
spring.jpa.hibernate.ddl-auto=update
```

## Manipulacion de datos con JPA

### Creamos la clase repositorio para incluir los de nuestras clases  
Aqui nombramos con '@Repositoti'


```Java
@Repository
public interface EmpleadoRepo extends JpaRepository<Empleado, Integer> {

    // Crud para las clases


}
```

Esto lo añadimos a la clase del Servicio


```Java
  @Autowired
    EmpleadoRepo repemp;

public List<Empleado> getAllEmpleados(){
            return repemp.findAll();
    }

    public Optional<Empleado> getAnEmpleado(int id){
    return repemp.findById(id);

    }
    public void createEmpleado(Empleado emp){


        repemp.save(emp);
    }
```

Para finalmente añadirlo al control


```Java
  @Autowired
        ServicioEmpleado ser;

    @RequestMapping("/empleados")
        public List<Empleado> listarEmpleados(){


            return ser.getAllEmpleados();
        }
        @RequestMapping("/empleados/{id}")
            public Optional<Empleado> findAnEmpleado(@PathVariable int id){
            return ser.getAnEmpleado(id);
            }

        @RequestMapping(value = "/empleados" , method = RequestMethod.POST)
            public void createEmpleado(@RequestBody Empleado emp){
            ser.createEmpleado(emp);
            }
```

## Funciones JPA utiles  

1. **Save/Update (Guardar/Actualizar)**:
   - `save(entity)`: Guarda una nueva entidad o actualiza una entidad existente.
   - `saveAll(entities)`: Guarda una colección de entidades.

2. **Retrieve (Recuperar)**:
   - `findById(id)`: Recupera una entidad por su ID.
   - `findAll()`: Recupera todas las entidades del repositorio.
   - `findAllById(ids)`: Recupera entidades por una lista de IDs.
   - `findAll(Sort)`: Recupera todas las entidades ordenadas según el criterio especificado.
   - `findAll(Pageable)`: Recupera una página de entidades paginadas.

3. **Delete (Eliminar)**:
   - `delete(entity)`: Elimina una entidad.
   - `deleteById(id)`: Elimina una entidad por su ID.
   - `deleteAll()`: Elimina todas las entidades del repositorio.
   - `deleteAll(entities)`: Elimina una colección de entidades.

4. **Count (Contar)**:
   - `count()`: Devuelve el número total de entidades en el repositorio.

5. **Exists (Verificar existencia)**:
   - `existsById(id)`: Verifica si una entidad con el ID dado existe en el repositorio.

6. **Custom Queries (Consultas personalizadas)**:
   - **Consultas basadas en convenciones de nombres**:
     - Spring Data JPA puede generar automáticamente consultas basadas en el nombre de los métodos en tu repositorio. Por ejemplo, si tienes un método en tu repositorio llamado `findByNombre`, Spring Data JPA generará una consulta para recuperar las entidades por el atributo `nombre`.

     Ejemplo:
     ```java
     public interface EmpleadoRepo extends JpaRepository<Empleado, Integer> {
         List<Empleado> findByNombre(String nombre);
     }
     ```

   - **Consultas JPQL**:
     - Puedes definir tus propias consultas utilizando JPQL. Para esto, puedes usar la anotación `@Query` en los métodos de tu repositorio y proporcionar la consulta JPQL como valor de la anotación.

     Ejemplo:
     ```java
     import org.springframework.data.jpa.repository.Query;

     public interface EmpleadoRepo extends JpaRepository<Empleado, Integer> {
         @Query("SELECT e FROM Empleado e WHERE e.departamento = ?1")
         List<Empleado> findByDepartamento(String departamento);
     }
     ```

   - **Consultas nativas de SQL**:
     - También puedes ejecutar consultas SQL nativas utilizando la anotación `@Query` con la opción `nativeQuery` establecida en `true`.

     Ejemplo:
     ```java
     import org.springframework.data.jpa.repository.Query;

     public interface EmpleadoRepo extends JpaRepository<Empleado, Integer> {
         @Query(value = "SELECT * FROM empleado WHERE departamento = ?1", nativeQuery = true)
         List<Empleado> findByDepartamento(String departamento);
     }
     ```

   Estas son algunas formas comunes de definir consultas personalizadas en Spring Data JPA. Puedes elegir la opción que mejor se adapte a tus necesidades y preferencias. Recuerda que estas son solo algunas posibilidades, y Spring Data JPA ofrece muchas más opciones para manejar consultas personalizadas y complejas.

## Apuntes sobre JPA mapping 

Para mapear con cardinalidad usamos anotaciones como   

**@OneToOne, @OneToMany, @ManyToOne, @ManyToMany**  

Estas han de mapear la otra clase usando el nombre de la variable que las conecte , y no usando el @Joincolumn que le pusimos de nombre a la columna de la tabla.  

- Tabla / Entidad / Clase **Direccion**  

```java
@Entity
@Table(name = "direccion")
public class Direccion {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    int cod_direccion;
    String municipio;

    @OneToOne(mappedBy = "dir")
    Empleado empleado;

```
- Tabla / Entidad / Clase **Empleado**  
```java
@Entity

public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    int idempleado;
    String Nombre;
    String Depart;

    @OneToOne
    @JoinColumn(name = "fk_dir")
    private Direccion dir;

```