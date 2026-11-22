# Repositorio AWM SEXX (`repo/`)

Esta carpeta sirve como fuente de contenido compatible con **AWM SEXX** sin tener que recompilar ni reinstalar la `.ipa` cada vez que agregues un paquete nuevo (fondos `.tendies`, temas de marcador `.3105pass` o parches de configuración válidos `.3105`).

## URLs de tu repositorio
- **Catálogo por defecto (ya conectado en la app):**  
  `https://raw.githubusercontent.com/luisdavidgarza385/ios-hacks/main/repo/sources.json`
- **URL directa de la fuente (`Sources` → `+`):**  
  `https://raw.githubusercontent.com/luisdavidgarza385/ios-hacks/main/repo/repo.json`

## Requisitos si lo sirves desde tu servidor Linux 24/7
La app valida las URLs con `PackageRepositoryURLPolicy`:
- Debe usar **HTTPS** en el **puerto 443** (no `http://` ni puertos personalizados).
- Debe usar un **nombre de dominio** (no una dirección IP directa ni `.local`).

## Cómo agregar un paquete nuevo en `repo/repo.json`
1. Copia el archivo en `repo/packages/` (por ejemplo `mi-fondo.tendies` o `mi-tema.3105pass`).
2. Calcula su SHA-256 y tamaño en bytes en Linux:
   ```bash
   sha256sum repo/packages/mi-paquete.tendies
   stat -c%s repo/packages/mi-paquete.tendies
   ```
3. Agrega una entrada dentro del arreglo `"packages"` en `repo/repo.json`:
   ```json
   {
     "identifier": "mi-paquete",
     "kind": "wallpaper",
     "name": "Mi Paquete",
     "author": "@xDavid",
     "version": "1.0.0",
     "summary": "Descripción breve del paquete.",
     "category": "Wallpaper",
     "tags": ["Custom"],
     "download": "packages/mi-paquete.tendies",
     "sha256": "<hash-sha256-en-64-hex>",
     "size": 123456,
     "supportedOS": [
       { "minimum": "17.0", "maximum": "18.7.1" },
       { "minimum": "26.0", "maximum": "26.6.1" },
       {
         "minimum": "27.0",
         "maximum": "27.0",
         "builds": ["24A5355q", "24A5370h", "24A5380h", "24A5390f"]
       }
     ]
   }
   ```
   *(Para parches `.3105` o temas `.3105pass`, cambia `"kind"` a `"patch"`.)*
4. Sube los cambios (`git add repo && git commit -m "Update repo" && git push`) o actualiza el archivo en tu servidor HTTPS. Los usuarios solo recargan **Sources / Home** en la app para ver lo nuevo.
