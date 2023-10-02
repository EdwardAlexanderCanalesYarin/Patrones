# Patrones
Uso de los patrones de diseño factory method, command y bridge

**Patrón creacional: FACTORY METHOD**
- proporciona una interfaz para crear familias de objetos relacionados o dependientes sin especificar sus clases concretas.
### Factory method: 
Te permite aislar instanciaciones condicionales  en un método y cuando necesitas cambiar esa lógica solo tienes que cambiar un método, no todo el código base.
Veamos el siguiente bloque de código:

```ruby
class Endpoint
	def home(params)
		if params[:user_type] == “admin”
			Admin.new
		elsif params[:user_type] == “member”
			Member.new
		else
			Guest.new
		end
	end
	def contact(params)
		if params[:user_type] == “admin”
			Admin.new
		elsif params[:user_type] == “member”
			Member.new
		else
			Guest.new
		end
	end
end
```
¿Cúal es el problema con este código? tenemos el mismo bloque de codigo usado dos veces dentro de esta clase, por lo que tenemos dos rutas, a saber, home y contact, y ambas toman un tipo de usuario en los parámetros y, en funcion de él, crean un objeto de usuario para determinar el tipo de usuario. Ambos necesitan pasar por el mismo proceso, pero esta lógica podría usarse en múltiples lugares a lo largo de la aplicación, por lo que cuando llegue el momento de agregar un nuevo tipo de usuario a su aplicación, tendrá que pasar por cada uno de estos bloques de código y agregue el nuevo tipo a todos ellos. Con suerte, no olvide actualizar uno de ellos, asi que ESE ES UN PROBLEMA, peor hay uno más. Una vez que crea un objeto de usuario, debe asegurarse de que todos los objetos de usuario se comporten de la misma manera porque los usará de la misma manera, es decir, llamará a los mismos métodos en el objeto resultante sin importar de que clase se creó y una forma de asegurarse de que todos estos objetos se comporten de la misma manera, es decir, respondan a los mismos métodos es crear una clase abstracta de la cual heredan todas estas clases de usuario:
```ruby
class UserBase
	def first_name = raise(“not implemented”)
	def last_name = raise(“not implemented”)
end

class Admin < UserBase; end
class Member < UserBase; end
class Guest < UserBase; end
```
La UserBase de usuario no hace mucho en términos de comportamiento, pero lo sí hace es forzar a todas sus subclases a anular sus métodos, por lo que si su Endpoint está utilizando esos métodos, deben estar disponibles en todos los objetos, de lo contrario obtendrá una excepción.
Lo que hace el patrón factory method es que le ayuda con escenarios como estos en los  que tiene algo de lógica para crear distintos objetos de la misma clase, lo hace encapsulando esa lógica en un método, así que creamos una nueva clase y movamos ese condicional allí.
```ruby
class UserFactory
	def call(user_type)
		if user_type == “admin”
			Admin.new
		elsif user_type == “member”
			Member.new
		else
			Guest.new
		end
	end
end

class Endpoint
	def home(params)
		user = UserFactory.call(params[:user_type])
		full_name = [user.first_name, user.last_name].join(“ “)
		{ name: full_name }.to_json
	end

	def home(params)
		user = UserFactory.call(params[:user_type])
		{ first_name: user.first_name }.to_json
	end
end
```
Al hacerlo, hemos aislado la creación del objeto de usuario en una clase diferente que ahora podemos reutilizar en toda nuestra base de código y cuando llega el momento de agregar un nuevo tipo de usuario a nuestra aplicación, solo necestiamos cambiar ese método, todo lo demás permanece igual y debido a que el cambio es tan pequeño, es mucho más difícil de introducir nuevos errores en tu aplicación 

**Patrón estructural: BRIDGE**
- convierte la interfaz de programación de una clase en otra interfaz (a veces más simple) que los clientes esperan, o desacopla la interfaz de una abstracción de su implementación, para inyección de dependencia o rendimiento.


**Patrón de comportamiento: COMMAND**
- encapsula una solicitud de operación como un objeto, lo que le permite parametrizar clientes con diferentes solicitudes, colas o solicitudes de registro, y admite operaciones que se pueden deshacer
