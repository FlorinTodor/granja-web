# High performance web farm

**English** · [Español](#granja-web-de-altas-prestaciones)

Eight web servers (four Apache and four Nginx) behind a load balancer, with four
interchangeable balancers (Nginx, HAProxy, Traefik and Envoy), HTTPS, iptables firewalls in
every container, Prometheus-driven autoscaling, and load and attack testing.

[![Demo: the farm up and round-robin distribution, 6 requests per server](https://florintodor.dev/media/poster/granja-web.jpg)](https://florintodor.dev/en/proyectos/granja-web/)

**Demo video and project page:** [florintodor.dev/en/proyectos/granja-web](https://florintodor.dev/en/proyectos/granja-web/)
**What broke a year later:** ["FROM ubuntu:latest" is going to break your project](https://florintodor.dev/en/blog/from-ubuntu-latest-rompe-tu-proyecto/)

## What's in it

- **Load balancing.** One `docker-compose_<balancer>_balanceador.yaml` per balancer: Nginx,
  HAProxy, Traefik and Envoy in front of the same eight backends, on two Docker networks
  with fixed IPs (`red_web` and `red_servicios`).
- **HTTPS.** Certificates generated with OpenSSL, mounted on the balancer and the backends.
- **Firewall.** Default `DROP` policy in every container; backends only accept traffic from
  the balancer (`flotodor-iptables-web.sh`, `flotodor-iptables-balanceador.sh`).
- **Autoscaling.** `escalador.sh` asks Prometheus for each node's CPU (node-exporter) every
  15 s: above 50 % it starts another backend, below 20 % it removes one, between 8 and 20
  containers. It rewrites the HAProxy configuration and Prometheus' `file_sd` so the new
  backend receives traffic and is monitored. Grafana has its dashboard in `dashboard.json`.
- **Load and attacks.** Apache Benchmark and Locust in their own containers, and
  `ataques.sh` with a parallel-request DoS and Slowloris (`slowhttptest`). The output is in
  `resultados_ataques.txt`.

## Layout

Each assignment builds on the previous one, so `P5-granjaweb/` is the complete version.

| Folder | What it adds |
|---|---|
| `P1/` | The initial farm: Apache and Nginx backends with custom images |
| `P2/` | The four balancers and autoscaling with Prometheus and Grafana |
| `P3/` | HTTPS with self-issued certificates |
| `P4/` | iptables in every container and the attack script |
| `P5-granjaweb/` | Benchmarking with `ab` and Locust on the complete farm |
| `memorias/` | PDF reports (in Spanish) for the first two assignments |

## Running it

```bash
cd P5-granjaweb
./init.sh -b all          # builds the images
./init.sh -u nginx rb     # nginx|haproxy|traefik|envoy|escalado, optional strategy (rb, pd, lc)
for i in $(seq 1 48); do curl -sk https://localhost | grep -oE '192.168.10.[0-9]+'; done | sort | uniq -c
```

Coursework for High Performance Web Servers (SWAP), University of Granada, 2025.

---

# Granja web de altas prestaciones

[English](#high-performance-web-farm) · **Español**

Ocho servidores web (cuatro Apache y cuatro Nginx) detrás de un balanceador, con cuatro
balanceadores intercambiables (Nginx, HAProxy, Traefik y Envoy), HTTPS, cortafuegos iptables
en cada contenedor, autoescalado guiado por Prometheus y pruebas de carga y de ataque.

**Vídeo de la demo y ficha del proyecto:** [florintodor.dev/proyectos/granja-web](https://florintodor.dev/proyectos/granja-web/)
**Lo que se rompió un año después:** [«FROM ubuntu:latest» te va a romper el proyecto](https://florintodor.dev/blog/from-ubuntu-latest-rompe-tu-proyecto/)

## Qué hay montado

- **Balanceo.** Un `docker-compose_<balanceador>_balanceador.yaml` por cada uno: Nginx,
  HAProxy, Traefik y Envoy delante de los mismos ocho backends, en dos redes Docker con IP
  fija (`red_web` y `red_servicios`).
- **HTTPS.** Certificados generados con OpenSSL, montados en el balanceador y en los
  backends.
- **Cortafuegos.** Política por defecto `DROP` en cada contenedor; los backends sólo aceptan
  tráfico del balanceador (`flotodor-iptables-web.sh`, `flotodor-iptables-balanceador.sh`).
- **Autoescalado.** `escalador.sh` consulta a Prometheus la CPU de cada nodo (node-exporter)
  cada 15 s: por encima del 50 % arranca otro backend, por debajo del 20 % retira uno, entre
  8 y 20 contenedores. Reescribe la configuración de HAProxy y el `file_sd` de Prometheus
  para que el nuevo backend reciba tráfico y se monitorice. Grafana tiene su panel en
  `dashboard.json`.
- **Carga y ataques.** Apache Benchmark y Locust en sus propios contenedores, y
  `ataques.sh` con un DoS de peticiones paralelas y Slowloris (`slowhttptest`). La salida
  está en `resultados_ataques.txt`.

## Estructura

Cada práctica parte de la anterior, así que `P5-granjaweb/` es la versión completa.

| Carpeta | Qué añade |
|---|---|
| `P1/` | La granja inicial: backends Apache y Nginx con imagen propia |
| `P2/` | Los cuatro balanceadores y el autoescalado con Prometheus y Grafana |
| `P3/` | HTTPS con certificados propios |
| `P4/` | iptables en cada contenedor y el script de ataques |
| `P5-granjaweb/` | Benchmarking con `ab` y Locust sobre la granja completa |
| `memorias/` | Memorias en PDF de las dos primeras prácticas |

## Arrancarlo

```bash
cd P5-granjaweb
./init.sh -b all          # construye las imágenes
./init.sh -u nginx rb     # nginx|haproxy|traefik|envoy|escalado, con estrategia opcional (rb, pd, lc)
for i in $(seq 1 48); do curl -sk https://localhost | grep -oE '192.168.10.[0-9]+'; done | sort | uniq -c
```

Trabajo de la asignatura Servidores Web de Altas Prestaciones (SWAP), Universidad de
Granada, 2025.
