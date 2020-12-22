# parcial-2canciones-solucion

[Enunciado](https://docs.google.com/document/d/1DGIFDVIsxbbpffSTyDEaR2m37GF4XSbDoXrUKoctvs4/edit#heading=h.gt33t48149l1)

# Resolución

## Parte A - Persistencia

```java
//Voy por la estrategia de single table para poder realizar consultas polimórficas por un lado. Por otro lado, es mejor en cuanto a performance y no perdemos demasiado en normalización teniendo en cuenta los pocos atributos que posee cada subclase.
//Ademas vuelvo a la clase una clase abstracta, ya que no veo que tenga sentido instanciar un objeto de la clase Contenido.
@Entity
@Inheritance(InheritanceStrategy=SINGLE_TABLE)
@DiscriminatorColum(name="tipo")
abstract class Contenido{
@Id
@GeneratedValue
long Id
reproducciones
imagenDeTapa
@Enumerated
Clasificacion clasificacion
@ManyToOne
Usuario propietario
@Embedded
Estadistica estadistica
}

@Entity
@DiscriminatorValue(value="Podcast")
class Podcast extends Contenido{
fechaInicio
fechaFin
}

@Entity
@DiscriminatorValue(value="Cancion")
class Cancion extends Contenido{
duracion
LocalDate subido
}

@Embedable
class Estadistica{
likes
dislikes
}

@Entity
class Usuario{
@Id
@GeneratedValue
long Id
}

//La interface Clasificacion se transforma en un enum con comportamiento.
enum Clasificacion{
Menores,
Adolescentes,
Adultos,

/*
Implementar validarAcceso
*/
}

@Entity
class Playlist{
@Id
@GeneratedValue
long Id
@ManyToOne
Usuario propietario
@ManyToMany
Colecction<Usuario> suscriptores
@ManyToMany
@OrderColum(name="posicion")
List<Contenido> contenidos
@Enumerated
Visibilidad visibilidad
}

//Los enums no requieren ser anotados.
enum Visibilidad{
PRIVADA,
PUBLICA,
NO_LISTADA,
}
```

### Diagrama de entidad relación

<img src="der2canciones.jpg">
