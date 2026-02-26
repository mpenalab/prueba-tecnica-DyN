# Caso de Estudio – Data Engineer: Validación Alertas Smart (Dichter & Neira)

Este repositorio contiene la solución a la prueba técnica para el diseño de un flujo de validación automatizado de auditorías de campo.

## Estructura del Proyecto

El proyecto ha sido estructurado siguiendo las mejores prácticas de Ingeniería de Datos:
* `notebooks/validacion_alertas_smart.ipynb`: Contiene el pipeline principal de datos (End-to-End).
* `data/alertas_generadas.xlsx`: Archivo autogenerado con el output de las alertas.
* `.env` / `.gitignore`: Manejo seguro de credenciales de entorno.
* `requirements.txt`: Dependencias necesarias para replicar el entorno.

## Enfoque Técnico y Decisiones

El flujo fue diseñado priorizando la eficiencia, la escalabilidad y la claridad del código, dividido en 4 fases principales:

1. **Modelado y Generación de Datos Simulados:**
   Se utilizó `pandas` y la librería estándar `random` para crear una base de datos relacional ficticia (Clientes, Categorías, Cliente_Categoria, Auditorias, FotosCargadas). Se programó una lógica de inyección de anomalías (semilla de alertas) para asegurar casos de prueba donde el número de fotos cargadas fuera menor al mínimo esperado.

2. **Transformación y Lógica de Validación:**
   En lugar de iterar con bucles `for`, se priorizó el uso de operaciones vectorizadas y agrupaciones nativas de Pandas (`groupby` y `merge`). Esto asegura que, en un entorno de producción con miles de registros, el cálculo del mínimo de fotos (categorías auditadas + 2 fotos de control) y la detección de faltantes se ejecute en fracciones de segundo.

3. **Automatización y Notificación Segura:**
   La exportación de las alertas a formato Excel se realiza con el motor `openpyxl`. Para el envío de correos, se utilizó `smtplib` y `email.message`. Como medida de seguridad clave, las credenciales no están quemadas en el código, sino que se inyectan dinámicamente a través de variables de entorno usando `python-dotenv`.   

4. **Visualización de Datos (Dashboard):**
   Se implementó un panel visual usando `matplotlib` y `seaborn` directamente en el notebook. El objetivo es doble: ofrecer una vista macro del estado de la operación (proporción de alertas) y una vista micro para priorizar acciones correctivas (impacto de fotos faltantes por cliente).

## Cómo ejecutar este proyecto
1. Instalar las dependencias: `pip install -r requirements.txt`
2. Configurar las variables de entorno en un archivo `.env` (EMAIL_SENDER, EMAIL_PASSWORD, EMAIL_RECEIVER).
3. Ejecutar las celdas del archivo `validacion_alertas_smart.ipynb` de forma secuencial.