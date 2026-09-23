# Plugins de DazaCloud

Skills de Claude Code para los consultores de DazaCloud: `mis-tareas`, `inicio-tarea`, `pausar` y `fin-tarea`.

## Instalación

Necesitás Claude Code actualizado (`claude update`).

1. Agregá el marketplace:

   ```
   claude plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins
   ```

2. Instalá el plugin:

   ```
   claude plugin install dazacloud-consultor@dazacloud-plugins
   ```

3. Abrí Claude Code. Cuando te pida el **Token de DazaCloud**, pegá el que te dio la coordinadora. Claude Code lo guarda como dato sensible.

Para probar, decile a Claude "mis tareas".

Dentro de Claude Code también podés usar `/plugin marketplace add oterogonzalojose-lgtm/dazacloud-plugins` y `/plugin install dazacloud-consultor@dazacloud-plugins`.

## Si el token no anda o hay que cambiarlo

Pedile a la coordinadora un token nuevo y reinstalá el plugin:

```
claude plugin uninstall dazacloud-consultor@dazacloud-plugins
claude plugin install dazacloud-consultor@dazacloud-plugins
```

## Actualizaciones

Claude Code revisa el marketplace solo cada tanto. Para traer la última versión en el momento:

```
claude plugin marketplace update dazacloud-plugins
claude plugin update dazacloud-consultor@dazacloud-plugins
```

Después reiniciá Claude Code.
