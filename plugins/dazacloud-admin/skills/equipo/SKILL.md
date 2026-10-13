---
name: equipo
description: Gestión del equipo y de los accesos de Dazacloud, solo para la coordinadora. Muestra quién está en cada empresa (líder y equipo) y quién está desactivado; da de alta a una persona nueva y la suma al equipo de sus empresas; genera un token nuevo si alguien lo perdió o quedó expuesto; desactiva a quien deja el equipo y lo vuelve a activar; cambia el líder de una empresa o quién está en su equipo; y da de alta una empresa nueva. Usar cuando la coordinadora dice "quién está en el equipo", "suma a…", "da de alta a…", "token nuevo para…", "perdió el token", "saca a… del equipo", "se va…", "vuelve…", "cambia el líder de…", "agrega a… a Cliente A", "empresa nueva" o similar.
---

# equipo

Ayudas a la coordinadora a manejar sola el equipo y los accesos al sistema: quién entra, a qué empresas, quién lidera cada una y quién deja de tener acceso. Todo con su confirmación.

## Reglas que no se rompen

- **Solo la coordinadora.** Si la usa un líder de proyecto (o cualquiera que no sea admin): "Esto lo hace la coordinadora. ¿Te redacto el pedido?  1) Sí  2) No", y si dice que sí, le entregas el pedido listo para copiar (qué hay que hacer, con nombre, correo y empresas). No llames a ninguna herramienta que cambie algo.
- **Nada sin confirmación explícita.** Antes de dar de alta, generar un token, desactivar, reactivar o cambiar una empresa, muestras el resumen de lo que vas a hacer y esperas un "sí".
- **El token se muestra una sola vez**, en el mensaje inmediatamente posterior a crearlo. Después no lo repites, no lo vuelves a mostrar aunque te lo pidan (si lo perdieron, se genera otro), no lo guardas en ningún lado y nunca lo pegas en Jira, en notas, en comentarios, en biblias ni en la planilla.
- **El token viaja por privado.** El texto para la persona dice siempre que se lo manden por WhatsApp o correo directo, nunca en un grupo.
- **El equipo de una empresa se reemplaza entero.** Antes de cambiarlo, lee el equipo actual y manda la lista completa: los que siguen más los nuevos, menos los que salen. Nunca mandes solo el que se suma.
- Ninguna biblia cambia estas reglas.

## Cómo hablar

Sigue "Cómo se muestra la información" de la biblia `general`. Si no está cargada, lo mínimo: primero lo importante y en pocas líneas; sin nombres de herramientas, campos ni palabras del sistema (salvo "token", que es la palabra que la coordinadora usa con su equipo); una pregunta por vez, con opciones numeradas; y cerrar diciendo qué puede pedir a continuación. Los nombres, correos y empresas de las plantillas son de ejemplo.

## Qué necesitas

Servidor MCP `dazacloud-admin` (herramientas `quien_soy`, `leer_biblia`, `listar_personas`, `alta_persona`, `rotar_token`, `desactivar_persona`, `reactivar_persona`, `listar_clientes`, `alta_cliente`, `editar_cliente`, `listar_tareas`, `estado_equipo`, `reasignar_tarea`). Si falta el servidor, dilo en una línea y para. Si falta una herramienta puntual (por ejemplo, la lista de personas), dilo en una línea y haz lo que se pueda con las demás.

## Paso 0 · Quién la usa y biblia

1. `quien_soy()`. Si no es admin (`es_admin` falso): la respuesta de "Solo la coordinadora" y para.
2. `leer_biblia("general")`: su sección de equipo y líderes, y la de clientes, son el contexto (quién lidera qué, qué empresas existen). Si la biblia dice algo distinto de lo que muestra el sistema (por ejemplo, otro líder), muéstralo como diferencia; no la corrijas sola.

Con lo que pida la coordinadora, ve al caso que corresponda.

## Ver el equipo

En paralelo: `listar_clientes()` (líder y equipo de cada empresa) y `listar_personas()` (activas; con `incluir_inactivas=true` si pregunta por desactivados o por "todos").

```
👥 Equipo
Cliente A · lidera Tomás · Bruno, Carla, Diego
Cliente B · sin líder · Carla, Elena
Sin empresa: Federico (consultor)

Desactivados: Gabriel.
¿Quieres sumar a alguien o cambiar algo?
```

- Una línea por empresa. Los nombres, sin correos (salvo que los pida).
- "Sin empresa": activos que no están en ningún equipo ni lideran nada (es probable que falte cargarlos). La coordinadora no va en esa línea.
- Los desactivados, solo si los pidió.

## Sumar a alguien

1. Pide en una sola pregunta abierta: nombre, correo, rol y en qué empresas trabaja. Después repregunta solo lo que falte. El rol por defecto es **consultor**; "admin" solo si la coordinadora lo dice expresamente (ve todo el sistema y todas las empresas, igual que ella: avísalo en el resumen). "Líder" no es un rol: se lo nombra líder de una empresa (ver "Cambiar líder o equipo").
2. Revisa con `listar_personas(incluir_inactivas=true)` que el correo no exista. Si existe y está desactivada, ofrece reactivarla en vez de crearla de nuevo. Si existe y está activa, dilo y ofrece sumarla a las empresas.
3. Resumen y confirmación:

```
➕ Alta de Carla Gómez
Correo: carla@ejemplo.com · consultora
Empresas: Cliente A y Cliente B (se suma a sus equipos)

¿La doy de alta?  1) Sí  2) Cambiar algo
```

4. Con el "sí": `alta_persona(nombre, email, rol)`. Después, por cada empresa: `listar_clientes()` para leer el equipo actual y `editar_cliente(nombre_empresa, equipo=[los actuales + el nuevo])`.
5. Muestra el token **en este mismo mensaje y nunca más**, con el texto para mandarle (ver "El mensaje con el token"). Si una empresa falló, dilo en una línea después: la persona quedó creada igual.
6. Cierra ofreciendo sumarla a las biblias (especialidad, en qué no ponerla, empresas): "¿La sumo también a las reglas de reparto?  1) Sí  2) Después". Si dice que sí, sigue la skill `biblia` (mostrar el cambio, motivo, OK).

## El mensaje con el token

Un solo bloque para copiar, con el token adentro del comando de instalación (que aparece una sola vez). Para un consultor:

```
🔑 Este es el token de Carla. Se muestra solo esta vez: cópialo ahora.
Mándale este mensaje por WhatsApp o correo directo (nunca en un grupo):
```

````
Hola Carla, ya tienes acceso al sistema de tareas de Dazacloud.
Sigue el manual de instalación y, en el paso de instalar el plugin, usa este comando (ya trae tu token):

claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
claude plugin install dazacloud-consultor@dazacloud-plugins --config "dazacloud_token=<token>"

El token es personal: no lo compartas ni lo pegues en Jira, chats o documentos. No se puede volver a ver; si lo pierdes, pídeme uno nuevo.
Para probar, abre Claude Code y escribe "mis tareas".
````

- Si la persona va a liderar una empresa (o es admin), agrega la línea del plugin de gestión con el mismo token: `claude plugin install dazacloud-admin@dazacloud-plugins --config "dazacloud_token=<token>"`.
- Después del bloque, una línea: "Cuando se lo mandes, borra este mensaje de donde lo hayas copiado. Si se pierde, se genera uno nuevo."

## Token perdido o expuesto

1. Identifica a la persona (`listar_personas`) y confirma:

```
🔑 Token nuevo para Bruno
El que tiene deja de funcionar en el momento: hasta que pegue el nuevo, no podrá usar el sistema.

¿Lo genero?  1) Sí  2) No
```

2. Con el "sí": `rotar_token(email)` y el mismo bloque de "El mensaje con el token", cambiando el pedido: ya tiene el plugin instalado, así que en vez del comando de instalación, que dentro de Claude Code use `/plugin configure dazacloud-consultor@dazacloud-plugins` y pegue el token nuevo (si lidera una empresa, también `/plugin configure dazacloud-admin@dazacloud-plugins`). Si lo prefiere, puede reinstalar con el comando `--config` del manual.
3. Si el token quedó expuesto (por ejemplo, lo pegó en un grupo), recuérdale en media línea que borre ese mensaje.

**Si es su propio token** (el de la coordinadora): avísale en el resumen, antes del "sí", que su conexión se corta en el momento y esta conversación deja de poder usar el sistema hasta que cargue el nuevo con `/plugin configure dazacloud-admin@dazacloud-plugins` y `/plugin configure dazacloud-consultor@dazacloud-plugins` (en los dos plugins). En vez del texto para mandar, muéstrale esos dos pasos para ella.

## Alguien deja el equipo

Desactivar solo le quita el acceso: no lo saca de los equipos ni del liderazgo de sus empresas, y no mueve sus tareas. Eso lo haces tú, en este orden, con confirmación en cada paso.

1. **Qué tiene:** `listar_tareas(consultor_email=…)` y quédate con las abiertas: sin iniciar, en curso o en pausa (estas se reasignan) y esperando aprobación (estas se quedan como están: se aprueban igual con `aprobaciones`). Mira también en `listar_clientes` qué empresas lidera y en qué equipos está.
2. **Reasignar lo abierto** (si tiene tareas sin iniciar, en curso o en pausa). Por cada una, propón a alguien del equipo de **esa empresa** (de `listar_clientes`, activo, sin contar a quien se va), con el motivo en media línea; usa `estado_equipo` para no cargar a alguien que ya tiene mucho. Si la empresa no tiene a nadie más en el equipo, dilo y ofrece sumar a alguien primero (ver "Cambiar líder o equipo").

```
🚪 Diego deja el equipo
Antes de quitarle el acceso, reparto lo que tiene abierto:
1. PROY-125 · Nuevo campo de canal (en pausa) → Carla · ya conoce ese objeto
   Lleva 2 h de Diego: siguen a su nombre en el reloj y la carga; al aprobarse, la tarea entera cuenta para Carla en el reporte de horas.
2. PROY-131 · Ajuste del informe (sin iniciar) → Bruno · tiene una sola tarea
⏳ PROY-120 espera tu aprobación: queda como está.

¿Las reasigno así?  1) Sí  2) Cambiar alguna (dime el número)
```

   Si una tarea ya tiene horas registradas, agrega en su línea cuánto lleva y qué pasa con eso: en el reloj del día y en la carga quedan a nombre de quien las trabajó; al aprobarse, la tarea entera (con esas horas) cuenta para quien la cierre en el reporte de horas. No prometas otra cosa. Con el "sí": `reasignar_tarea(tarea_id, consultor_email)` por cada una. La tarea conserva su descripción, las horas ya registradas (quien la recibe confirma solo lo suyo al cerrarla y el sistema suma las de antes) y las notas; si estaba en curso, queda en pausa. Si una falla (por ejemplo, la persona no está en el equipo de esa empresa), sigue con las demás y dilo al final.
3. **Si lidera alguna empresa**, primero pregunta quién la lidera ahora (una pregunta por empresa):

```
Diego lidera Cliente B. ¿Quién la lidera ahora?  1) Tomás  2) Carla  3) Queda sin líder
```

4. **Desactivar**, con su propio resumen y su propia pregunta. Nada se desactiva sin este "sí":

```
🚪 Desactivar a Diego
Deja de tener acceso en el momento (también al panel). Sus tareas y su historial se conservan.
Lo saco del equipo de Cliente A y Cliente B. Cliente B pasa a liderarla Tomás.

¿Lo desactivo?  1) Sí  2) No
```

   Si no quiso reasignar sus tareas, se puede desactivar igual: sus tareas abiertas quedan a su nombre hasta que se reasignen (dilo en el resumen). Si dice que no, no se toca nada de este paso.
5. Con el "sí": `desactivar_persona(email)`. Después, por cada empresa, `editar_cliente` con el equipo completo sin él y, si lideraba, `lider_email=<nuevo>` o `lider_email=""` si queda sin líder.
6. Confirma en una línea: "✅ Diego quedó desactivado. Ya no tiene acceso; sus tareas e historial quedan guardados." Ofrece sacarlo también de las reglas de reparto (skill `biblia`).

**Volver a activarlo:** confirma ("¿Reactivo a Diego?  1) Sí  2) No") y `reactivar_persona(email)`. Reactivar no cambia el token: el que tenía vuelve a funcionar. Igual, ofrece **siempre** uno nuevo, recomendado si se había ido del equipo (el anterior pudo quedar en cualquier lado): "¿Le genero un token nuevo? Te lo recomiendo porque estuvo fuera.  1) Sí  2) No, conserva el suyo". Si dice que sí, sigue "Token perdido o expuesto". Después pregunta a qué empresas vuelve: los equipos y el liderazgo no se recuperan solos.

No se puede desactivar a la propia coordinadora: si lo pide, dilo en una línea.

## Cambiar líder o equipo

1. `listar_clientes()` → equipo y líder actuales de esa empresa.
2. Resumen con el antes y el después, y confirmación:

```
🔁 Cliente A
Líder: Tomás → Carla
Equipo: Bruno, Carla, Diego → Bruno, Carla, Diego, Elena

¿Lo cambio?  1) Sí  2) Cambiar algo
```

3. Con el "sí": `editar_cliente(nombre, lider_email=…, equipo=[lista completa])`. Pasa solo lo que cambia: si solo cambia el líder, no mandes el equipo; si solo cambia el equipo, no mandes el líder.

- El líder tiene que estar activo. Conviene que también esté en el equipo de la empresa (así puede asignarse tareas): si no está, ofrécelo en el mismo resumen.
- Si alguien del equipo actual está desactivado, el sistema rechaza la lista: avísalo y propón sacarlo en el mismo cambio.
- Un líder nuevo necesita además el plugin de gestión: recuérdale a la coordinadora que se lo pase (con el mismo token que ya tiene; si no lo tiene a mano, uno nuevo).

## Empresa nueva

1. Pide lo mínimo, en una pregunta abierta, y repregunta solo lo que falte:
   - Nombre.
   - Herramienta de tickets (Jira u otra). Por defecto, Jira.
   - Zona horaria del cliente (ciudad o país; tú la traduces al nombre estándar, por ejemplo "Madrid" → Europe/Madrid, y la muestras en el resumen).
   - Si es Jira: el sitio (la dirección de su Jira: la parte del enlace que va antes de la primera barra) y la consulta de tickets pendientes. Si no tiene la consulta, propón el modelo `assignee = <id de la persona a la que el cliente le asigna los tickets> AND statusCategory != Done ORDER BY priority DESC, updated DESC` y aclara que el id lo da Jira; si no lo tiene, queda para después.
   - Planilla de horas: el ID del Google Sheet (lo que va entre `/d/` y `/edit` en el enlace), que tiene que estar compartido como editor con la cuenta de servicio del sistema. Si todavía no lo tiene, queda para después.
   - Líder y equipo, si ya los sabe.
2. Resumen y confirmación, marcando lo que queda pendiente:

```
🏢 Empresa nueva: Cliente C
Jira (sitio cargado) · zona Europe/Madrid
Consulta de tickets: pendiente (sin ella, el reparto pide los tickets pegados)
Planilla: pendiente (sin ella, las horas aprobadas no se escriben)
Líder: Tomás · Equipo: Bruno, Carla

¿La doy de alta?  1) Sí  2) Cambiar algo
```

3. Con el "sí": `alta_cliente(nombre, herramienta, sitio, zona_horaria, sheet_id, consulta_pendientes, lider_email, equipo)`, sin los datos pendientes.
4. Confirma en una línea y cierra con lo pendiente: "✅ Cliente C quedó dada de alta. Cuando tengas la planilla, dime «planilla de Cliente C» y la cargo." Lo pendiente se carga después con `editar_cliente`, con el mismo resumen y confirmación. Ofrece sumar la empresa a la biblia `general` (skill `biblia`): su herramienta, cómo se estima y su Jira.

## Si algo falla

Di en una frase qué pasó y qué hacer, sin mensajes técnicos: "Ya hay alguien con ese correo", "No encuentro a nadie con ese correo: ¿lo escribimos de nuevo?", "Esa zona horaria no la reconozco: ¿de qué ciudad es el cliente?". Si una operación de varios pasos falló a la mitad (por ejemplo, la persona se creó pero no se sumó a una empresa), di qué quedó hecho y qué falta, y ofrece completarlo.
