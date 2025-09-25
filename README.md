# 1. ACTIVIDAD BASE: "GitHub Actions + Python Script + Auto Commit"

## 📝 Preparación del repositorio base

Estructura:

```
mi-proyecto-actions/
 ├── main.py
 ├── test_main.py
 ├── update_readme.py   👈 nuevo script
 └── README.md
```

### README.md inicial

```markdown
# Mi Proyecto con GitHub Actions

Este proyecto sirve para aprender a usar GitHub Actions 🚀

## Estado de los tests
*Aún no ejecutados...*
```

### main.py

```python
def saludo(nombre: str) -> str:
    return f"Hola, {nombre}!"
```

### test\_main.py

```python
from main import saludo

def test_saludo():
    assert saludo("Mundo") == "Hola, Mundo!"
```

---

## 🐍 Script en Python (`update_readme.py`)

Este script ejecuta los tests y actualiza el README:

```python
import subprocess

def run_tests():
    try:
        subprocess.check_call(["pytest", "-q"])
        return "✅ Tests correctos"
    except subprocess.CalledProcessError:
        return "❌ Tests fallidos"

def update_readme(status: str):
    with open("README.md", "r", encoding="utf-8") as f:
        lines = f.readlines()

    new_lines = []
    for line in lines:
        new_lines.append(line)
        if line.strip() == "## Estado de los tests":
            new_lines.append(status + "\n")
            break

    with open("README.md", "w", encoding="utf-8") as f:
        f.writelines(new_lines)

if __name__ == "__main__":
    status = run_tests()
    update_readme(status)
```

👉 Lo que hace:

1. Ejecuta los tests con `pytest`.
2. Según el resultado, genera un estado ✅ o ❌.
3. Modifica el `README.md` justo debajo de la sección `## Estado de los tests`.

---

## ⚙️ Workflow (`.github/workflows/ci.yml`)

```yaml
name: CI con AutoCommit

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  test-and-update:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Configurar Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Instalar dependencias
        run: pip install pytest

      - name: Ejecutar script de tests y actualizar README
        run: python update_readme.py

      - name: Commit automático del README
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "Update README con estado de tests"
          file_pattern: README.md
```

---

## 🚦 Flujo de la actividad

1. Alumno hace un **push** en `main`.
2. El workflow ejecuta el script en Python.
3. El script corre los tests y modifica el `README.md`.
4. La acción `git-auto-commit-action` hace commit automático con los cambios.
5. El alumno ve en el repo cómo el `README.md` se actualiza con:
    
    * ✅ Tests correctos
    * ❌ Tests fallidos

---

## 📑 Entregable del alumno

* Enlace a su repositorio con el `README.md` actualizado automáticamente.
* Evidencia de haber provocado un test fallido y un test correcto.
* Explicación breve de:
    
    * Qué hace el script.
    * Qué hace el workflow.
    * Qué aporta GitHub Actions a un proyecto real.

---

👉 Con esta versión los alumnos **programan un pequeño script** y ven cómo GitHub Actions:

* Ejecuta código propio.
* Modifica el repositorio automáticamente.
* Se integra con acciones externas (auto-commit).


---

Este proyecto sirve para aprender a usar GitHub Actions 🚀

## Estado de los tests
❌ Tests fallidos
