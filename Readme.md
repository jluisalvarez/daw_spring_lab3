# RestFul con Spring Boot

## Recurso (Java 14) Mensaje

```java
package daw.examples.rfws.resources;

public record  Mensaje(long id, String content) { }
```

## RestController 

```java
package daw.examples.rfws.controllers;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import daw.examples.rfws.resources.Mensaje;


@RestController
public class AppController {

	private final AtomicLong counter = new AtomicLong();
	List<Mensaje> mensajes = new ArrayList<>();

	@GetMapping("/")
	public List<Mensaje>  list() {
		return mensajes;
	}

	@PostMapping("/")
	public Mensaje add(@RequestBody Mensaje msj) {
		Mensaje nuevo = new  Mensaje(counter.incrementAndGet(), msj.content());
		mensajes.add(nuevo);
		return nuevo;
	}

	@DeleteMapping("/{id}")
	public Mensaje delete(@PathVariable("id") Long id) {
		int index = id.intValue()-1;
		Mensaje borrar = mensajes.get(index);
		mensajes.remove(index);
		return borrar;
	}

}
```