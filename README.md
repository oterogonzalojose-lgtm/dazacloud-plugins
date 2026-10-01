# Plugins de DazaCloud

Skills de Claude Code para DazaCloud:

- `dazacloud-consultor`: para los consultores. `mis-tareas`, `inicio-tarea`, `pausar`, `nota-cliente` y `fin-tarea`.
- `dazacloud-admin`: para la coordinadora y los líderes de proyecto. `inicio-dia`, `aprobaciones`, `resumen-dia`, `reporte-horas` y `biblia`. Un consultor que no lidera ninguna empresa no puede usarlo.

Lo de abajo es para el plugin de consultores; el de gestión se instala igual, cambiando el nombre del plugin.

## Instalación

Necesitas Claude Code actualizado (`claude update`).

1. Agrega el marketplace:

   ```
   claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
   ```

2. Instala el plugin:

   ```
   claude plugin install dazacloud-consultor@dazacloud-plugins
   ```

3. Abre Claude Code. Cuando te pida el **Token de DazaCloud**, pega el que te dio la coordinadora. Claude Code lo guarda como dato sensible.

Para probar, dile a Claude "mis tareas".

Dentro de Claude Code también puedes usar `/plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins` y `/plugin install dazacloud-consultor@dazacloud-plugins`.

## Si el token no anda o hay que cambiarlo

Pídele a la coordinadora un token nuevo y reinstala el plugin:

```
claude plugin uninstall dazacloud-consultor@dazacloud-plugins
claude plugin install dazacloud-consultor@dazacloud-plugins
```

## Actualizaciones

Claude Code revisa el marketplace solo de vez en cuando. Para traer la última versión en el momento:

```
claude plugin marketplace update dazacloud-plugins
claude plugin update dazacloud-consultor@dazacloud-plugins
```

Después reinicia Claude Code.

## Líderes de proyecto

Un líder sigue siendo consultor en sus propias tareas: instala **los dos plugins** con **el mismo token**.

```
claude plugin install dazacloud-consultor@dazacloud-plugins
claude plugin install dazacloud-admin@dazacloud-plugins
```

Si el token cambia, actualízalo en los dos con `/plugin configure dazacloud-consultor@dazacloud-plugins` y `/plugin configure dazacloud-admin@dazacloud-plugins`.
