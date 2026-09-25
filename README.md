# Granja web de altas prestaciones

Ocho servidores web (cuatro Apache y cuatro Nginx) detrás de un balanceador, con cuatro
balanceadores intercambiables (Nginx, HAProxy, Traefik y Envoy), HTTPS, cortafuegos iptables
en cada contenedor, autoescalado guiado por Prometheus y pruebas de carga y de ataque.

[![Demo: la granja arrancada y el reparto round-robin, 6 peticiones por servidor](https://florintodor.dev/media/poster/granja-web.jpg)](https://florintodor.dev/proyectos/granja-web/)

**Vídeo de la demo y ficha del proyecto:** [florintodor.dev/proyectos/granja-web](https://florintodor.dev/proyectos/granja-web/)

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

---

Trabajo de la asignatura Servidores Web de Altas Prestaciones (SWAP), Universidad de
Granada, 2025.
