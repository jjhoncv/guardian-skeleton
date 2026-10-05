# guardian-skeleton

Esqueleto mínimo de un proyecto que usa el **[Guardián](https://github.com/jjhoncv/guardian)**. No se usa directamente: lo instancia `scripts/nuevo-proyecto.sh` del Guardián.

- **Marcadores** que reemplaza el script: `__NOMBRE__`, `__SLUG__`, `__REPO__`, `__SITIO__`, `__GUARDIAN__` (versión del Guardián).
- `.github/workflows-proyecto/` → pasan a ser los `.github/workflows/` del proyecto (llamadas cortas a los workflows del Guardián).
- `.github/workflows/validar.yml` y este archivo son **solo del esqueleto**: el script no los copia.
- Cambios al esqueleto: por PR, con el check **validar esqueleto** en verde (instancia el esqueleto con un nombre de ejemplo y corre lint, typecheck, pruebas, build y E2E).
