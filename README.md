caso #4, 25%
instituto tecnológico de costa rica, escuela de computación
bases de datos II
prof. rodrigo núñez
tipo: teams of 3 or 4

descripción
debido a los recientes avances en los asistentes basados en inteligencia articial en especial los gpts, ha iniciado una transformación en la forma de interacción en que se entregan servicios y contenido a las personas por medio del internet. el vasto panteón de aplicaciones ha impulsado al consumidor digital a tener una experience de "have the job done" en lugar de instalar aplicaciones para cada cosa que sea necesaria.

eso ha hecho emerger nuevas empresas y nuevos servicios a lo largo del globo compitiendo por el mercado de facilitar dicha experiencia, proveyendo comodidad al consumidor al integrar servicios regionalizados y personalizados. la empresa AIBulb no es la excepción en dicha competencia, actualmente es uno de los competidores más importantes en el mercado, tiene presencia en estados unidos, méxico, costa rica y guatemala, con planes de expandirse.

su grupo de trabajo ha sido llamado a la misión de preparar la nueva arquitectura de datos y flujos de trabajo para AIBulb que le permita atender con eficacia el mercado actual y facilite a los directivos alcanzar más servicios en cada país y más países.

cómo funciona el sistema de AIBulb?

realizan contratos con prestadores de servicios en cada país, por ejemplo, netflix, spotify, lavandería, plomería, taxi, venta de entradas a eventos, entre muchos. existen contratos que son globalizados como spotify por ejemplo, o bien, otros que con del país como lavanderías, plomería, entradas a eventos, entre otros, incluso algunos prestadores de servicio podrían operar de forma regionalizada.

estos proveedores de servicios al tener presencia o no en cada país, deben sujetarse a las leyes relacionadas a impuestos, anualidades y cualquier otro aspecto legal que esté relacionado a la prestación de servicios por medios digitales. normalmente los proveedores ofrecen modelos de negocio a sus clientes tales como in-service purchase, free, on-demand, by subscription, entre otros.

los proveedores compiten por calidad de servicios, extras, amplitud de opciones y precio obviamente. AIBulb ha desarrollado toda su plataforma en digital wallets, de tal forma que la única forma de pagar es por medio del wallet seleccionado en el smartphone o dispositivo personal. el sistema de geolocalización y perfil de usuario provee la información necesaria a la aplicación de AIBulb para determinar quién es la persona, donde se encuentra ubicada y que servicios están disponibles según esa información. esa identidad se la dá el smartphone al app, de la misma forma los derechos a enviar solicitudes de pago al wallet. de esa forma AIBuld puede identificar universalmente a los usuarios, la disponibilidad de servicios según donde se encuentre y una forma de pago.

la aplicación toma lo que el usuario quiere, ya sea de forma verbal o escrita, por ejemplo:

"quiero ver que están pasando en todos los canales de películas"

"buscar un lugar de vacaciones para semana santa en puerto viejo para 5 personas, con piscina, a menos de 1km de la playa, completamente equipado y en menos de $100 la noche"

"enviame un taxi que me lleve al teatro nacional"

"necesito que recojan mi ropa y me la regresen lavada y aplanchada"

la aplicación usando la identificación previa, más esta solicitud, con ayuda de los gestores de AI, determina del texto el servicio que mejor se ajusta a lo solicitado y el contexto del usuario, crea la información necesaria para hacer el request a un api del servicio, con los parámetros que deduce de la solicitud, sucedido esto, pueden ocurrir múlltiples escenarios de respuesta:

se encuentra un servicio al que la persona está debidamente subscrita y es el servicio que más utiliza para dicho tipo de tarea, con lo cual recibe la respuesta del servicio según lo solicitado, y de ser neceario nuevas preguntas al usuario para mejorar los resultados

se encuentra un servicio apropiado y aunque el usuario no tenga ningún plan con dicho proveedor de servicio, se le responde al usuario las opciones disponibles y a la vez la respuesta que daría alguno de los servicios rankeados para ese tipo de tarea

no se encuentra un servicio apropiado, pero se le ofrecen opciones aproximadas a la persona por si alguno de los servicios disponibles pueden efectivamente resolver lo solicitado

una vez que el usuario decide pagar por un servicio o no, se debe guardar registro en el sistema de:

el request procesado con todas las aclaraciones relacionadas al request, también si el request no fué usado por el usuario, es decir, que canceló
registro de la transacción exitosa con el wallet
registro de la transacción exitosa con el proveedor
la entrega del servicio al consumidor
en cuanto a las transacciones se deben registrar las transacciones desglozadas como uso de la plataforma, costo del proveedor, impuestos o cargos asociados al lugar y la forma en que se creo el contrato, pues cada transacción de esas va a tener una cuenta destino diferente
la bitácora de preguntas y respuestas del sistema y el usuario que se dieron para afinar el request
las transacciones de pago deben ser completamente consistentes, por lo que el arquitecto prefiere utilizar un SABD relacional para tal fin
la aplicación de AIBulb, siempre está activa si el usuario la deja así configurada, sin embargo al abrirse posee un home page donde basado en el usuario del smartphone, su localización y aquellos proveedores ya pagados, se le muestran los servicios disponibles, similar a como se ven las películas y series en netflix, es decir, imagen, nombre, año, rate, clasificados por categoría por ejemplo: hospitality, health, finance, services, transport, house keeping, etc. Y algunas veces podrían salir alerts o publicidad de nuevos servicios disponibles que podrían ser recomendables para dicho usuario.

AIBulb monetiza fees por las transacciones realizadas, es decir, cualquier compra que el consumidor haga se le hace un cargo. sin embargo hay servicios que son de subscripción, otros gratis, no todos son de pago on-demand; por otro lado, hay request que podrían solicitarse a los servicios pero que el consumidor decida no adquirir. eso puede causar mucho tráfico no monetizado en la plataforma, para ello AIBulb vende paquetes mensuales a los proveedores, dichos paquetes incluyen cierta cantidad de request que no terminan en pago y cantidad de request que si hacen uso de algun pago. El request se mide como una solicitud completa al app con todas las modificaciones, es decir, si una persona dice que quiere sacar una cita con un dentista, las mejoras a la solicitud que se van a ir pidiendo como por ejemplo el lugar, días de preferencia, precio máximo esperado de la consulta, etc... todo eso entra dentro del mismo request, como una sola unidad y eso debe controlarse los planes que cada proveedor tenga contratados con AIBulb en el país o internacionalmente.

cuando los proveedores desean actualizar la información de los servicios que ofrecen, lo hacen por medio de un API que provee AIBulb, dicha actualización es guardada en una base de datos en memoria, redis. para que así luego un proceso asíncrono se encargue de tomar las modificaciones pendientes, enviarle la información necesaria a las bases de datos y modelos del IA, actualizar los URLs y objetos de los servicios que actualiza el proveedor, y luego finalmente enviar los datos de la modificación a los lugares respectivos. recordar que un mismo proveedor puede operar en varios países y podría para un mismo servicio no ofrecer exactamente las mismas funciones, por ello, la actualización de URLs, versión y objetos se realiza localizadamente según las zonas indicadas en la actualización. con la actualización lista las aplicaciones inician a hacer uso de los nuevos servicios y funciones.

las aplicaciones, interfaces de análisis de AI, apis, y demás que permiten que este sistema funcionaria en la vida real, NO están dentro del alcance del caso, solo lo solicitado específicamente en los entregables de cada preliminar y final.

preliminar #1, data cluster, databases design and implementation, domingo 23 de abril, 40pts
dado los requerimientos anteriores proceda a diseñar las colecciones y json necesarios para el sistema
diseñe e implemente relacionalmente en el SABD de su elección lo que sea necesario para dar consistencia transaccional
diseñe, implemente y ponga en ejecución un shard cluster en mongodb que permita a AIBulb proveer con eficiencia (esto es reduciendo al máximo posible que un usuario tenga que consultar y evaluar aplicaciones que involucren hacer query a datos que estén fuera del alcance de su localización o sus preferencias)
esto se revisará con cita con el profesor
utilice docker compose para la configuración del cluster deberán ser mantenidos en un repositorio en github centralizado para todos los integrantes del grupo de trabajo
distribuya las cargas de los servidores entre las computadoras de todos los integrantes del grupo, para ello va a ser necesario utilizar un software de VPN como https://openvpn.net/ , https://vpn.net/ o similar .
pruebe el cluster anterior tanto su distribución de llaves (shard), consulta de datos focalizados en el consumidor, alta disponibilidad y tolerancia a fallas del cluster
todos los scripts, archivos de comandos y datos que sean necesarios para este ejercicio
fecha y hora del último commit : domingo 23 de abril, media noche
preliminar #2, real time notifications, async update for content and sales, TBD , 60pts
cuando un proveedor de servicio quiere actualizar su contenido de la página principal debe hacerlo por medio de un api
dado que esto es tan masivo, no se realiza la actualización en el cluster en tiempo real si no que se hace de forma asíncrona
el request del proveedor al api en lugar de ser salvado es enviado a un servidor de notificaciones persistente, en este caso se ha decidido que sea kafka
una vez que kafka recibe la notificación se dispara un consumidor que se encarga de actualizar basado en el request la información necesaria en el cluster de mongo
el otro proceso automático que debe existir se va a implementar en logstash, este se va encargar de cada cierto tiempo analizar los request de los usuarios y posteriormente indexar en un elastic search la información necesaria para poder mostrarle reportes por rangos de fecha a los provedores, de la cantidad y tipo de request que han estado procesando por ellos de forma exitosa, el monto de ventas que le han generado cada una; y también, la cantidad y tipo de request que los usuarios están buscando, que se acerca a hacer match con ellos pero que ellos no lo proveen y así crear oportunidades de negocio
dicho reporte anterior deberá generarse por proveedor y por rango de fechas utilizando Kibana
diseñe una arquitectura solución y obtenga aprobación de dicha arquitectura con el profesor para los objetivos de este preliminar
una vez con la arquitectura aprobada proceda a realizar la implementación necesaria
con respecto a las transacciones consistentes, estás podrían estar ocasionando problemas de: dirty read, phantom, lost update y deadlock. estudie los pasos de las transacciones de request y pago del sistema, y encuentre junto con el profesor cuáles son esos posibles casos. Implemente los stored procedures y simule dichas situaciones por medio de llamadas a los SP transaccionales para que así su equipo de trabajo analice posibles soluciones a nivel de diseño o SQL.
la revisión será demostrativa en las herramientas solicitadas y con la arquitectura diseñada por el grupo, será con cita de revisión con el profesor
