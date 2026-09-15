# TP14 — Modelado de Amenazas con Threagile

## 1. Introducción

Este trabajo práctico implementa **modelado de amenazas** sobre la arquitectura de la **Notes App**, integrando la herramienta **Threagile** dentro del pipeline de CI/CD.

El objetivo es incorporar una práctica de **DevSecOps**, de manera que los cambios realizados sobre la arquitectura puedan ser analizados automáticamente y se generen reportes de seguridad como parte del proceso de integración continua.

El trabajo se desarrolla sobre la aplicación y la infraestructura construida durante los trabajos prácticos anteriores.

---

## 2. Objetivos

Los principales objetivos del TP14 son:

* Incorporar modelado de amenazas a la aplicación.
* Representar los componentes principales de la arquitectura mediante Threagile.
* Identificar activos técnicos y flujos de datos.
* Integrar el análisis de amenazas al pipeline de GitHub Actions.
* Ejecutar y validar el modelo localmente.
* Ejecutar automáticamente el análisis en GitHub Actions.
* Generar reportes de seguridad como artefactos del pipeline.
* Incorporar el análisis de seguridad como parte del proceso DevSecOps.

---

## 3. Tecnologías utilizadas

* Git
* GitHub
* GitHub Actions
* Docker
* Docker Compose
* Python
* YAML
* Threagile
* Docker Hub

---

## 4. Arquitectura modelada

El modelo de amenazas representa los principales componentes utilizados por la Notes App y su infraestructura.

Entre los activos técnicos modelados se encuentran:

* Cliente web
* Nginx
* Backend
* PostgreSQL
* Prometheus
* Node Exporter
* cAdvisor
* Grafana

El archivo principal del modelo es:

```text
threagile.yaml
```

Este archivo describe los activos, relaciones y flujos relevantes de la arquitectura.

---

## 5. Modelo de amenazas

Threagile permite representar la arquitectura desde una perspectiva de seguridad y analizar posibles riesgos asociados a los componentes y flujos de información.

El modelo fue construido y validado utilizando el archivo:

```text
threagile.yaml
```

Antes de integrarlo al pipeline se realizó una ejecución local para comprobar que el modelo fuera válido y que Threagile pudiera generar correctamente sus resultados.

La ejecución local permitió obtener:

* Reporte PDF
* Diagrama de flujo de datos
* Diagrama de activos
* Información de riesgos
* Estadísticas
* Información de activos técnicos

---

## 6. Integración con GitHub Actions

El análisis de amenazas fue integrado al pipeline existente mediante un nuevo job:

```yaml
threat-modeling:
```

El job utiliza la acción oficial:

```yaml
threagile/run-threagile-action@v1
```

y analiza:

```yaml
model-file: 'threagile.yaml'
```

Los resultados son almacenados mediante:

```yaml
actions/upload-artifact@v4
```

con el nombre:

```text
threagile-report
```

De esta manera, los resultados del análisis quedan disponibles como artefacto de GitHub Actions.

La consigna del TP establece precisamente que el reporte generado debe ser descargado desde la sección **Artifacts** y utilizado como evidencia para la presentación.

---

## 7. Ejecución manual del pipeline

Además de los disparadores habituales del pipeline, se incorporó:

```yaml
workflow_dispatch:
```

Esto permite ejecutar manualmente el workflow desde la interfaz de GitHub Actions.

Esta opción fue utilizada para validar la ejecución completa del pipeline después de integrar el análisis de amenazas.

---

## 8. Secretos utilizados

El pipeline utiliza secretos de GitHub para la autenticación con Docker Hub.

Los secretos configurados en el repositorio son:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

El login se realiza mediante:

```yaml
- name: Login a Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKERHUB_USERNAME }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}
```

Los valores de los secretos no se almacenan dentro del código fuente.

---

## 9. Validación del pipeline

Durante la validación se realizaron las siguientes comprobaciones:

1. Validación de la sintaxis YAML.
2. Ejecución local de Threagile.
3. Integración del job `threat-modeling`.
4. Configuración de GitHub Actions.
5. Configuración de los secretos requeridos.
6. Ejecución del pipeline.
7. Verificación de los jobs.
8. Generación del artefacto `threagile-report`.

La ejecución final del pipeline terminó correctamente, con los jobs en estado exitoso.

El job:

```text
Threat Model Analysis
```

finalizó correctamente y generó el artefacto correspondiente.

---

## 10. Artefacto generado

GitHub Actions generó el artefacto:

```text
threagile-report
```

El artefacto contiene los resultados generados por Threagile:

```text
data-asset-diagram.png
data-flow-diagram.png
report.pdf
risks.json
risks.xlsx
stats.json
tags.xlsx
technical-assets.json
```

El archivo comprimido utilizado como evidencia para la presentación es:

```text
threagile-report.zip
```

Este archivo se conserva fuera del repositorio como material de entrega de la presentación.

---

## 11. Evidencias

Como evidencia de la implementación se dispone de:

* `threagile.yaml`
* `.github/workflows/cicd.yml`
* Ejecución exitosa de GitHub Actions
* Job `Threat Model Analysis` en estado exitoso
* Artefacto `threagile-report`
* Reporte PDF generado por Threagile
* Diagrama de flujo de datos
* Diagrama de activos
* Archivos de riesgos y estadísticas

---

## 12. Flujo DevSecOps implementado

El flujo final del trabajo puede resumirse de la siguiente manera:

```text
Código / Arquitectura
        │
        ▼
   Git / GitHub
        │
        ▼
 GitHub Actions
        │
        ├── Lint
        ├── Tests
        ├── Build
        ├── Docker
        │
        └── Threat Model Analysis
                    │
                    ▼
               Threagile
                    │
                    ▼
             Reportes de seguridad
                    │
                    ▼
             Artifact: threagile-report
```

La integración permite que el análisis de amenazas forme parte del ciclo de desarrollo y no sea una actividad exclusivamente manual.

---

## 13. Resultado final

El TP14 permite incorporar una instancia de **modelado de amenazas automatizado** al proceso de desarrollo de la Notes App.

La implementación final permite:

* modelar la arquitectura;
* analizar amenazas mediante Threagile;
* validar el modelo;
* ejecutar el análisis automáticamente;
* generar reportes;
* conservar los resultados como artefactos de GitHub Actions;
* utilizar dichos resultados como evidencia del análisis de seguridad.

De esta manera se completa la integración de una práctica de **Security by Design / DevSecOps** dentro del pipeline de la aplicación.

---

## 14. Archivos principales del TP14

```text
trabajo-14/
├── .github/
│   └── workflows/
│       └── cicd.yml
├── threagile.yaml
├── guia-06/
├── guia-08/
├── guia-09/
├── guia-10/
├── guia-11/
├── guia-12/
├── devops-TP06/
├── devops-tp12/
└── README.md
```

Los archivos temporales, reportes generados localmente y copias de respaldo utilizadas durante la resolución no forman parte del código fuente necesario para ejecutar el proyecto.
