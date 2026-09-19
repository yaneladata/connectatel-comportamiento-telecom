# 📞 Análisis de Comportamiento de Usuarios y Segmentación | ConnectaTel LATAM

> 👤 **Rol:** Analista de Datos (Proyecto Individual)  
> 🏢 **Contexto:** Caso de Negocio / Proyecto de Portafolio (Bootcamp Analytics - Sprint Telecom)  
> 🎯 **Alcance:** Auditoría de Calidad de Datos, Limpieza/Tratamiento de Sentinels, Profiling Estadístico, Detección de Outliers y Diagnóstico Comercial.  
> 🛠️ **Stack Técnico:** Python (Pandas, NumPy, Seaborn, Matplotlib), Jupyter Notebook / Google Colab, Segmentación Demográfica.

---

## 🎯 Contexto y Desafío de Negocio

**ConnectaTel** es una empresa de telecomunicaciones con operaciones en **México y Colombia**. La dirección comercial requería un informe detallado para comprender cómo utilizan los clientes los servicios móviles (llamadas y mensajes) según su perfil demográfico y tipo de plan contratado, resolviendo tres necesidades estratégicas:

1. **Auditoría de Calidad:** Diagnosticar y corregir inconsistencias, valores nulos y registros inválidos (*sentinel values*) en las fuentes de datos.
2. **Segmentación de Clientes:** Clasificar la base de usuarios por edad y niveles de uso para identificar los grupos de mayor valor e interacción.
3. **Oportunidades de Retención y Migración:** Detectar patrones atípicos (*outliers*), riesgos de cancelación (*churn*) y oportunidades de *upselling* en los planes vigentes.

---

## 📂 Fuentes de Datos

El análisis integra tres datasets transaccionales y demográficos:

* `plans.csv`: Tarifas y capacidades de los planes (Básico y Premium: precio, minutos, GB incluidos y costos por excedente).
* `users_latam.csv`: Perfil del cliente (edad, ciudad, fecha de registro y plan contratado).
* `usage.csv`: Registro transaccional del uso real (duración de llamadas en minutos y longitud de mensajes).

---

## 🛠️ Auditoría de Calidad y Preparación de Datos

Durante la fase de exploración y saneamiento de datos en Python se identificaron y trataron los siguientes hallazgos técnicos:

* **Tratamiento de Inconsistencias y Valores Nulos:**
  * **Ubicación (`city` en `users`):** Se identificó un **11.7%** de nulos y 96 registros *sentinels*. Se imputaron bajo la categoría `"Desconocida"`.
  * **Estatus de Cliente (`churn_date` en `users`):** Presentaba **88.35%** de valores nulos, los cuales representan correctamente a los **clientes activos**.
  * **Transacciones Sin Consumo (`duration` y `lenght` en `usage`):** Registraron un **55.19%** y **44.70%** de nulos respectivamente. Se mantuvieron intactos al ser eventos reales (intentos de llamada sin conexión o envíos no realizados).
  * **Registros de Fecha (`date`):** Presentaban un **0.12%** de nulos (< 5% del total), por lo que no se aplicó ninguna acción destructiva.
* **Corrección de Valores Inválidos (Sentinels) y Fechas Futuras:**
  * **Edad (`age`):** Se detectaron *sentinels* con el valor `-999`, los cuales fueron imputados utilizando la **mediana**.
  * **Fechas Anómalas (`reg_date`):** Se identificaron 40 registros con fechas futuras (año 2026), convertidas correctamente a `NaT` para preservar la integridad del análisis temporal.

---

## 💡 Informe Ejecutivo & Diagnóstico de Negocio

### 🔍 1. Segmentación Demográfica (Por Edad)
* **Jóvenes (< 30 años):** Representan el **19.00%** de la base y son el grupo con menor consumo de servicios tradicionales (**3,364 minutos** de llamadas y **4,197 mensajes**).
* **Adultos (30 a 59 años):** Es el grupo prioritario del negocio; representa el **50.44%** de la base de usuarios y registra el mayor volumen de tráfico.
* **Adultos Mayores (≥ 60 años):** Conforman el **30.56%** de los clientes, manteniendo un consumo moderado y constante.

---

### 📊 2. Segmentación por Nivel de Uso
* **Uso Medio (Llamadas < 10, Mensajes < 10):** Concentra al **73.59%** de los usuarios de ConnectaTel.
* **Bajo Uso (Llamadas < 5, Mensajes < 5):** Representa el **19.45%** del total de clientes.
* **Alto Uso:** Corresponde a la minoría restante (**6.95%**).

---

## 🚀 Recomendaciones de Negocio & Oportunidades Comerciales

1. **Rediseño de Oferta para el Segmento Joven (Aumento de LTV):**
   * *Diagnóstico:* El grupo menor de 30 años (19% de la base) subutiliza llamadas y mensajes. Por comportamiento demográfico, su tráfico se desplaza hacia redes sociales (WhatsApp/Instagram).
   * *Acción:* Lanzar un **Plan Centrado en Datos Móviles** (mayor bolsa de GB con menor cuota de minutos). 
   * *Próximo Paso:* Integrar la medición de consumo de GB en los registros de `usage.csv` para validar técnicamente la demanda real de datos.

2. **Mitigación de Churn en Plan Premium ($25/mes):**
   * *Diagnóstico:* Se identificaron **266 clientes en Plan Premium con Bajo Uso** (19.45% del segmento), representando un alto riesgo de cancelación por baja percepción de valor.
   * *Acción:* Implementar campañas personalizadas de fidelización e incentivos de uso para evitar la baja del servicio y proteger el *ARPU* (Ingreso Promedio por Usuario).

3. **Estrategia de Upselling (Migración a Plan Premium):**
   * *Diagnóstico:* Se detectaron **66 usuarios en Plan Básico** (límite de 100 min) que registran un promedio de **89 minutos de uso**, operando muy cerca del tope y generando cobros extra por excedentes.
   * *Acción:* Ofrecerles una migración asistida al Plan Premium para mejorar su experiencia de cliente y asegurar un ingreso recurrente mayor.

---

## 📁 Estructura del Repositorio

```text
Estructura del Repositorio
├── data/
│   ├── plans.csv                                        <- Especificaciones y tarifas de planes
│   ├── users_latam.csv                                  <- Información demográfica de usuarios
│   └── usage.csv                                        <- Detalle transaccional de uso de servicios
├── visualizaciones/                                     <- Gráficos del análisis
├── notebook/
│   └── S7_Version_Estudiante_Project_ConnectaTel.ipynb  <- Notebook principal con limpieza y análisis
└── README.md                                            <- Informe ejecutivo y documentación del proyecto
