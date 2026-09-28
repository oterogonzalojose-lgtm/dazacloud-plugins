# Plugins de DazaCloud

Skills de Claude Code para los consultores de DazaCloud: `mis-tareas`, `inicio-tarea`, `pausar`, `nota-cliente` y `fin-tarea`.

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
