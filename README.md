# SWAP – Servidores Web de Altas Prestaciones (UGR)

## 📌 Descripción
Proyecto académico desarrollado en la asignatura **Servidores Web de Altas Prestaciones** de la Universidad de Granada.  
El objetivo es diseñar, desplegar, securizar y evaluar una **granja web** de alto rendimiento utilizando tecnologías de contenedores, balanceo de carga y cortafuegos.

El proyecto se ha desarrollado en **5 prácticas**, cada una centrada en un aspecto clave:  
1. Configuración inicial de la granja web y balanceo básico  
2. Estrategias avanzadas de balanceo de carga (nginx, HAProxy y otros)
3. Seguridad en las comunicaciones (HTTPS/SSL)
4. Seguridad perimetral con IPTABLES (Firewall) 
5. Benchmarking y análisis de rendimiento

---

## 🛠 Tecnologías utilizadas
- **Docker** y **Docker Compose**  
- **Nginx** y **Apache HTTP Server**  
- **Traefik**, **HAProxy**, **GoBetween**, **Envoy** (balanceadores alternativos)  
- **OpenSSL** – Generación y gestión de certificados SSL  
- **iptables** – Cortafuegos a nivel de contenedor  
- **Apache Benchmark (ab)** y **Locust** – Herramientas de pruebas de carga  
- **Bash scripting** – Automatización de despliegues y pruebas  
- **Redes personalizadas de Docker**: `red_web` y `red_servicios`

---

## 📐 Metodología
Cada práctica ha seguido un flujo de trabajo común:
1. **Diseño del escenario**: definición de topología y requisitos
2. **Preparación del entorno**: creación de directorios y archivos base
3. **Implementación**: Dockerfile, docker-compose, scripts y configuraciones
4. **Pruebas**: funcionalidad, seguridad y rendimiento
5. **Documentación**: capturas, métricas y análisis de resultados

---
## 📊 Conclusiones
1. **Alta disponibilidad gracias al balanceo y escalabilidad**

2. **Seguridad reforzada en comunicación y perímetro**

3. **Rendimiento estable bajo carga alta**

4. **Escenarios reproducibles y fácilmente desplegables**
---

## 📂 Estructura del repositorio
```bash
├── README.md                # Este resumen general
├── P1/                      # Configuración inicial y almacenamiento
├── P2/                      # Balanceo de carga (nginx, HAProxy, Traefik, Envoy)
├── P3/                      # HTTPS/SSL (certificados y TLS)
├── P4/                      # Cortafuegos con IPTABLES
├── P5/                      # Benchmarking (ab, Locust)
└── docs/                    # Capturas y material adicional
