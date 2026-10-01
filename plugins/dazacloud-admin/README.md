# dazacloud-admin

Skills de gestión de Dazacloud: `inicio-dia`, `aprobaciones`, `resumen-dia`, `reporte-horas` y `biblia`. Se conectan a `/admin/mcp` del backend.

## Quién lo usa

- **La coordinadora** (admin): todas las empresas, todo el detalle, incluida la estimación de la IA, y es la única que cambia las biblias.
- **Un líder de proyecto** (persona que lidera al menos una empresa): reparte, aprueba y ve horas, notas y bloqueos **solo de sus empresas**, y reparte solo a su equipo. Ve la estimación de la IA y puede asignarse tareas a sí mismo; no edita las biblias (las lee) y sus propias tareas las aprueba la coordinadora. La estimación de la IA nunca le llega a un consultor. Cada skill lo detecta sola al empezar (`quien_soy`).

Un consultor que no lidera ninguna empresa no puede usar este plugin (el backend le niega el acceso).

El criterio de negocio (equipo, clientes, Jira de cada cliente, reglas de reparto y de aprobación) no está en las skills: vive en las biblias del backend. Los textos iniciales de las biblias no vienen en este plugin.

## Instalación

Requisitos: Claude Code 2.1.269 o superior.

```bash
claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
claude plugin install dazacloud-admin@dazacloud-plugins --config "dazacloud_token=<token>"
```

Desde la terminal, `claude plugin install` **no pide el token**: se pasa con `--config` o después, dentro de Claude Code, con `/plugin configure dazacloud-admin@dazacloud-plugins`. Verificar con `claude mcp list`: el servidor del plugin tiene que figurar "Connected".

En PowerShell, para no mostrar el token: `--config "dazacloud_token=$(Get-Content <archivo del token>)"`.

### Si lo tenías instalado desde el repo privado

Hasta la 0.5.0 el plugin venía del marketplace `dazacloud`. Ahora sale solo del marketplace público: desinstala el viejo antes de instalar el nuevo, para no tener dos copias.

```bash
claude plugin uninstall dazacloud-admin@dazacloud
claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
claude plugin install dazacloud-admin@dazacloud-plugins --config "dazacloud_token=<token>"
```

### Un líder de proyecto

El líder sigue siendo consultor en sus propias tareas, así que instala **los dos plugins** del mismo marketplace, con **el mismo token personal** (no hay un token aparte para gestionar):

```bash
claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
claude plugin install dazacloud-consultor@dazacloud-plugins --config "dazacloud_token=<su token>"
claude plugin install dazacloud-admin@dazacloud-plugins --config "dazacloud_token=<su token>"
```

Antes de que funcione, la coordinadora tiene que nombrarlo líder de la empresa y cargar el equipo. Si el token cambia, hay que actualizarlo en los dos plugins: `/plugin configure dazacloud-consultor@dazacloud-plugins` y `/plugin configure dazacloud-admin@dazacloud-plugins`.
