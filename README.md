# Patrones
Uso de los patrones de diseño factory method, command y bridge

**Patrón creacional: FACTORY METHOD**
- proporciona una interfaz para crear familias de objetos relacionados o dependientes sin especificar sus clases concretas.
### Factory method: 
Te permite aislar instanciaciones condicionales  en un método y cuando necesitas cambiar esa lógica solo tienes que cambiar un método, no todo el código base.
Veamos el siguiente bloque de código:
'''ruby
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
''''

**Patrón estructural: BRIDGE**
- convierte la interfaz de programación de una clase en otra interfaz (a veces más simple) que los clientes esperan, o desacopla la interfaz de una abstracción de su implementación, para inyección de dependencia o rendimiento.


**Patrón de comportamiento: COMMAND**
- encapsula una solicitud de operación como un objeto, lo que le permite parametrizar clientes con diferentes solicitudes, colas o solicitudes de registro, y admite operaciones que se pueden deshacer
