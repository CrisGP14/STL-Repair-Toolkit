# STL Repair Toolkit

Herramienta local para diagnosticar y reparar modelos STL dañados antes de impresión 3D o uso en CAD.

## Descripción

Este proyecto toma un archivo `.stl`, analiza su geometría, detecta problemas comunes como huecos, caras degeneradas, vértices duplicados y mallas no watertight, y luego intenta repararlas con herramientas como `trimesh` y `pymeshfix`.

La idea principal es ofrecer una solución simple, rápida y útil para limpiar archivos STL que suelen venir con errores de topología o geometría.

## Características

- Diagnóstico de la malla antes y después de la reparación
- Detección de:
  - vértices duplicados
  - caras degeneradas
  - vértices no referenciados
  - aristas abiertas / huecos
  - mallas no watertight
- Reparación con enfoque básico y avanzado
- Exportación directa del archivo corregido
- Compatible con flujo local y simple de uso desde terminal

## Requisitos

- Python 3.10 o superior
- `numpy`
- `trimesh`
- `pymeshfix`

## Instalación

```bash
cd STL-Repair-Toolkit
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Uso

```bash
python3 Flix.py modelo.stl
```

Por defecto, el archivo de salida se genera con el nombre:

```bash
modelo_fixed.stl
```

### Opciones disponibles

- `-o, --output`: define la ruta del archivo STL corregido
- `--only-trimesh`: usa solo la reparación básica de `trimesh`

### Ejemplo

```bash
python3 Flix.py entrada.stl --output salida_reparada.stl
python3 Flix.py entrada.stl --only-trimesh
```

## Flujo de trabajo

1. Carga el archivo STL
2. Analiza la malla y muestra el estado inicial
3. Elimina elementos problemáticos
4. Intenta cerrar huecos y corregir geometría
5. Genera una versión reparada en disco

## Nota importante

Este repositorio se enfoca en la utilidad del script y su documentación. El archivo principal `Flix.py` se mantiene intacto como herramienta funcional, sin alterar su lógica interna.

## Consideraciones

- Los archivos STL grandes suelen mantenerse fuera del repositorio mediante `.gitignore`.
- Si `pymeshfix` falla durante la reparación, el proyecto intenta continuar con la salida generada por `trimesh`.
- Si el archivo sigue teniendo problemas complejos, puede requerirse revisión manual en Blender, Meshmixer o herramientas CAD más avanzadas.

## Estructura del proyecto

```text
STL-Repair-Toolkit/
├── Flix.py
├── README.md
├── requirements.txt
├── .gitignore
└── .venv/   # opcional, generado al crear el entorno local
```
