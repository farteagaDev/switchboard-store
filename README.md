# switchboard-store

Tienda de botones y perfiles descargables para [Switchboard](https://github.com/farteagaDev/switchboard). Todo el contenido son archivos JSON estáticos, servidos directamente desde este repo (`raw.githubusercontent.com`) — sin backend.

## Estructura

```
catalog.json     índice de todo lo disponible (lo que la app consulta primero)
buttons/         exports individuales de tipo "buttons" (uno o varios botones sueltos)
profiles/        exports de tipo "profile" (un perfil completo)
```

## Formato de los archivos

Cada archivo en `buttons/` o `profiles/` es un `ExportFile`, el mismo formato que ya genera Switchboard al exportar (ver `src/shared/exportTypes.ts` en el repo de la app):

```ts
type ExportFile =
  | { kind: 'streamdeck-export'; version: 1; type: 'profile'; profiles: ExportedProfile[] }
  | { kind: 'streamdeck-export'; version: 1; type: 'buttons'; buttons: ExportedButton[] }
```

Sin IDs (se generan al importar) y sin rutas de imagen — las imágenes van embebidas en base64 (`imageDataUrl`).

## catalog.json

Cada entrada agregada a `catalog.json` describe un archivo de `buttons/` o `profiles/`:

```json
{
  "id": "slug-unico",
  "type": "buttons",
  "title": "Nombre visible",
  "description": "Qué hace",
  "author": "quien lo subió",
  "path": "buttons/slug-unico.json",
  "imageDataUrl": "data:image/png;base64,...",
  "icon": "Power"
}
```

`id` y `path` deben coincidir con el nombre del archivo real agregado a la carpeta correspondiente.

`imageDataUrl`/`icon` son opcionales — se muestran como miniatura en la lista de la tienda, con la misma prioridad que usa un botón real: imagen primero, ícono como respaldo si no hay imagen. Van directo en el catálogo (no se leen del archivo del botón) para que listar la tienda no tenga que descargar cada archivo, solo `catalog.json`. `icon` debe ser uno de los nombres en `ICON_NAMES` de `src/renderer/src/utils/iconOptions.ts` en el repo de la app.
