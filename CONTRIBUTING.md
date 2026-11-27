# Cómo trabajamos en equipo

La idea: que cada quien suba sus propios commits, que todos podamos ver y revisar lo de los demás, y que `main` siempre funcione. El profe califica el último commit antes del 30/nov 23:59 y en la defensa nos puede preguntar a cualquiera sobre cualquier parte, así que nos conviene revisar el trabajo de los demás.

## Reglas cortas

1. **Nadie hace push directo a `main`.** Cada tarea va en su propia rama y entra por Pull Request (PR).
2. **Cada PR lo revisa alguien más** antes de hacer merge (aunque sea un "lo corrí y funciona 👍").
3. **Las tareas son Issues** asignados a cada quien. El PR cierra su issue escribiendo `Closes #número` en la descripción.
4. **Cada libreta tiene dueño** (los .ipynb chocan muy feo si dos personas los editan a la vez):

   | Libreta | Dueño |
   |---|---|
   | `01-exploracion.ipynb` | Ricardo |
   | `02-validacion-y-linea-base.ipynb` | Sebastián |
   | `03-modelos.ipynb`, `04-envio-final.ipynb` | Kevin |

   Si quieres cambiar una libreta que no es tuya, avísale al dueño o pídeselo en el PR. Las funciones que sirvan para más de una libreta van en `src/*.py`, y las libretas solo las importan; los `.py` se pueden editar entre varios sin bronca.
5. **`experimentos.csv`:** una fila por experimento, **siempre al final del archivo**, con `id` usando tus iniciales (`SA-01`, `RI-01`, `KN-01`). Está configurado con `merge=union`, así que si dos personas agregan filas a la vez se juntan solas al hacer merge en tu compu.
6. **Nada de datos, modelos ni `kaggle.json` en git.** El `.gitignore` ya los ignora; los datos viven en Kaggle.
7. **Si usas código o ideas de un notebook público, pon la liga** en un comentario (regla del profe).

## Configuración (una sola vez)

```bash
git clone https://github.com/SebastianAlcantarr/proyecto_RSNA_KNEE.git
cd proyecto_RSNA_KNEE

# que los commits salgan con TU cuenta de GitHub
git config user.name "Tu Nombre"
git config user.email "tu-correo-de-github"   # o el noreply: Settings > Emails

uv sync                              # crea el entorno igualito para todos
uv run nbdime config-git --enable    # diffs de libretas legibles (cuando nbdime esté en el pyproject)
```

Para Kaggle: pon tu `kaggle.json` en `~/.kaggle/` (no dentro del repo).

## El día a día

```bash
# 1. ponerte al día
git switch main
git pull

# 2. rama nueva para tu tarea
git switch -c tu-nombre/tarea          # ej. ricardo/eda-etiquetas

# 3. trabajar y hacer commits chicos
git add -p                             # revisa qué vas a subir
git commit -m "EDA: prevalencia de los 12 hallazgos"

# 4. subir y abrir el PR
git push -u origin tu-nombre/tarea
gh pr create                           # o desde la página de GitHub
```

Luego alguien más lo revisa en GitHub (pestaña "Files changed"), deja comentarios o lo aprueba, y se hace merge con el botón **"Squash and merge"** o **"Merge"**. Después, borra la rama.

### Si `main` avanzó mientras trabajabas

```bash
git switch tu-nombre/tarea
git pull origin main        # trae lo nuevo de main a tu rama
# si hay conflicto: abre los archivos marcados, deja lo correcto, y luego
git add archivo-arreglado
git commit
git push
```

Si GitHub dice que el PR tiene conflictos, haz esto mismo en tu compu: GitHub no aplica el `merge=union` del CSV, tu git sí.

### Agregar un paquete

```bash
uv add nombre-paquete
git add pyproject.toml uv.lock     # siempre los dos
```

## Tablero

Las tareas están en **Issues** y en el tablero de **Projects** del repo (Pendiente / En progreso / Listo). Antes de empezar algo, asígnate el issue para que no lo hagamos dos veces.

## Kaggle

- El código que corre en Kaggle (caché, entrenamiento, inferencia) también se guarda en `kaggle/` dentro de este repo, para que quede registro y se pueda reproducir.
- La caché preprocesada y los pesos preentrenados van en **Datasets privados de Kaggle** compartidos con los 3 (Settings → Sharing → agregar colaboradores). Lo mismo con los notebooks de Kaggle.
- Cada quien pone su usuario de Kaggle en la tabla del README (sección 8); el profe lo pide para verificar los envíos.
