# HinoProtections 0.3.2

Plugin de protecciones progresivas para **Paper 26.2**. Un jugador fabrica un
nucleo, lo coloca y obtiene un terreno protegido desde la altura minima hasta
la altura maxima del mundo. Esta distribución contiene el JAR listo para
instalar; no requiere dependencias adicionales.

## Requisitos

- Paper 26.2 (probado al compilar contra `26.2.build.87-stable`).
- Java 25.

Paper 26.2 y Java 25 son requisitos; el plugin no esta destinado a Spigot,
Bukkit antiguo ni versiones 1.21.x.

## Instalacion

1. Apaga el servidor.
2. Haz una copia de seguridad del mundo y de `plugins/HinoProtections/`.
3. Copia `HinoProtections-0.3.2.jar` dentro de `plugins/`.
4. Inicia el servidor con Java 25.
5. Revisa `plugins/HinoProtections/config.yml` y reinicia o usa `/hp reload`.

Las protecciones se guardan en `plugins/HinoProtections/protections.yml`.

## Crafteo inicial

La proteccion de carbon se fabrica en una mesa normal, como cualquier receta
de Minecraft:

```text
Palo          + Tronco de roble + Palo
Tronco roble  + Bloque carbon   + Tronco roble
Palo          + Tronco de roble + Palo
```

El resultado es un nucleo `COAL_ORE` con un area de 8x8.

## Mejoras

Haz clic derecho sobre tu nucleo para abrir el menu. Si haces clic derecho
mientras estas agachado, abres directamente la mesa de mejora. El bloque
central no se consume: se transforma al completar una receta valida.

| Nivel actual | Siguiente nivel | Area | Esquinas | Laterales |
|---|---|---:|---|---|
| Mineral de carbon | Mineral de cobre | 16x16 | Lingote de cobre | Bloque de cobre |
| Mineral de cobre | Mineral de hierro | 32x32 | Lingote de hierro | Bloque de hierro |
| Mineral de hierro | Mineral de oro | 64x64 | Lingote de oro | Bloque de oro |
| Mineral de oro | Mineral de diamante | 128x128 | Bloque de diamante | Diamante |
| Mineral de diamante | Ancient Debris | 256x256 | Netherite Scrap | Netherite Scrap |
| Ancient Debris | Bloque de hierro | 512x512 | Bloque de hierro | Bloque de hierro |
| Bloque de hierro | Bloque de oro | 1024x1024 | Bloque de oro | Bloque de oro |
| Bloque de oro | Bloque de diamante | 1536x1536 | Bloque de diamante | Bloque de diamante |
| Bloque de diamante | Bloque de netherite | 2064x2064 | Bloque de netherite | Lingote de netherite |

El nucleo actual siempre va en el centro. Antes de mejorar, el plugin valida
que el area nueva no se superponga con otra proteccion.

## Funciones

- Bloqueo de romper, colocar, usar puertas, botones y almacenamientos ajenos.
- Proteccion contra explosiones, fluidos, fuego, pistones, entidades y hoppers
  que intenten cruzar el limite.
- PvP configurable por proteccion.
- Hasta 5 amigos por protección, con permiso para construir y usar almacenamientos.
- Teletransporte a protecciones propias y, si el dueño lo permite, de amigos.
- Avisos discretos en la barra de accion al entrar y salir.
- Bordes con particulas personalizables y desbloqueables por nivel.
- Particulas adicionales desde Mineral de Hierro: llama, electrica y nube.
- Modo de bloques estatico: dibuja el limite con bloques falsos del mismo
  mineral del nucleo, sin alterar el mundo ni seguir al jugador.
- Deteccion de combate: durante 30 segundos, hasta mostrar 1 segundo, empuja
  fuera a quien intente entrar en una proteccion.
- Persistencia por UUID y almacenamiento YAML.
- Portal web integrado para consultar todos los crafteos, amigos y bases.
- Iconos PNG oficiales de Minecraft 26.2 en cada casilla de crafteo.
- El dueño configura de forma independiente PvP, brillo de jugadores, modo
  bloques, teletransporte de amigos y efectos visuales en cada base.
- MagmaCube brillante desbloqueable desde Bloque de Hierro.

## Portal web

El portal muestra los crafteos con los PNG de los materiales de Minecraft, tus
amigos y las bases a las que puedes ir. Las recetas son públicas, pero los
amigos, las coordenadas y las bases necesitan una sesión personal. Dentro del
juego usa:

```text
/hp web
```

El enlace recibido caduca en 120 segundos y sólo puede utilizarse una vez. La
sesión web dura 60 minutos y permite consultar información, pero no modificarla.

### Sólo en el equipo del servidor

Esta es la configuración predeterminada. El portal se abre en
`http://localhost:8765` y no está expuesto a Internet:

```yml
web:
  enabled: true
  bind-address: "127.0.0.1"
  port: 8765
  public-url: "http://localhost:8765"
```

### Acceso directo con DDNS

Para abrir el panel por un dominio DDNS, habilita/reenvía el puerto TCP en el
firewall y el router. Sustituye el dominio y puerto por los tuyos:

```yml
web:
  enabled: true
  bind-address: "0.0.0.0"
  port: 8766
  public-url: "http://tu-dominio-ddns.net:8766"
```

`bind-address` es una dirección local: debe ser `127.0.0.1` o `0.0.0.0`, nunca
el dominio DDNS. `public-url` es el enlace que recibe el jugador y debe incluir
siempre `http://` o `https://`. Por ejemplo,
`tu-dominio-ddns.net:8766` es inválido, mientras que
`http://tu-dominio-ddns.net:8766` es válido.

### Recomendado: proxy inverso HTTPS

Para un servidor público se recomienda Caddy, Nginx u otro proxy con HTTPS.
El proxy se expone a Internet y el plugin sólo escucha localmente:

```yml
web:
  enabled: true
  bind-address: "127.0.0.1"
  port: 8766
  public-url: "https://tu-dominio.net"
```

Configura el proxy para redirigir `https://tu-dominio.net` a
`http://127.0.0.1:8766`. No publiques el puerto interno si utilizas esta
alternativa. Tras modificar `bind-address`, `port` o `public-url`, reinicia el
servidor completo o ejecuta `/hp reload`; no uses `/reload` de Minecraft.

## Amigos

Debes estar dentro de una proteccion propia:

```text
/hp trust <jugador>
/hp untrust <jugador>
/hp friends
```

Un amigo puede construir e interactuar dentro de esa proteccion. El dueño
puede activar el teletransporte de amigos desde el menu principal.

## Comandos

```text
/hp menu
/hp list
/hp friends
/hp trust <jugador>
/hp untrust <jugador>
/hp tp <numero>
/hp web
/hp remove
/hp give <jugador> <nivel> [cantidad]
/hp reload
```

`give` y `reload` son administrativos. Para retirar el nucleo de forma normal,
el dueño tambien puede agacharse y romperlo; recibira el item del nivel actual.

## Permisos

| Permiso | Predeterminado | Uso |
|---|---|---|
| `hinoprotections.use` | Todos | Usar `/hp` |
| `hinoprotections.place` | Todos | Colocar nucleos |
| `hinoprotections.menu` | Todos | Abrir menus |
| `hinoprotections.trust` | Todos | Gestionar amigos |
| `hinoprotections.tp` | Todos | Teletransportarse |
| `hinoprotections.web` | Todos | Generar acceso personal al portal |
| `hinoprotections.admin` | OP | Ignorar protecciones y limites |
| `hinoprotections.give` | OP | Entregar nucleos |
| `hinoprotections.reload` | OP | Recargar configuracion |

## Configuracion importante

```yaml
combat:
  protection-entry-block-enabled: true
  protection-entry-block-seconds: 30
  block-trusted-players: true
  repel-strength: 0.75
  repel-y: 0.28

visuals:
  static-border:
    minimum-step: 1
    max-blocks: 1024

web:
  enabled: true
  bind-address: "127.0.0.1"
  port: 8765
  public-url: "http://localhost:8765"
  login-token-seconds: 120
  session-minutes: 60

limits:
  max-members-per-protection: 5
```

`max-blocks` limita cuantos bloques falsos se envian por borde. En niveles
grandes el plugin aumenta automaticamente la separacion para evitar miles de
cambios visuales por jugador.

También puedes ajustar:

- `limits.max-protections-per-player`: número máximo de protecciones propias.
- `limits.max-members-per-protection`: amigos por protección; el valor
  predeterminado es 5.
- `limits.allow-overlap-same-owner`: permite o prohíbe solapamientos del mismo
  dueño.
- `security.block-ender-pearl-entry`: bloquea entrar mediante perlas de ender.
- `security.prevent-fire-spread`, `prevent-natural-entity-grief` y
  `prevent-hopper-boundary-transfer`: controles extra contra daños indirectos.
- `visuals.particles.*` y `visuals.static-border.*`: reducen la carga visual
  en servidores con muchos jugadores. Los tiempos se expresan en ticks (20
  ticks = 1 segundo).

## Uso y solución de problemas

- Para retirar una protección, usa `/hp remove` dentro de ella. El dueño
  también puede agacharse y romper el núcleo para recuperar su objeto.
- `/hp tp <número>` sólo teletransporta a protecciones propias. Las bases de
  amigos se consultan desde el menú; el dueño debe activar allí el TP de
  amigos.
- Si el panel genera enlaces a `localhost`, revisa `web.public-url`: debe tener
  `http://` o `https://` y el puerto si no usas 80/443.
- Si el panel no abre desde Internet, comprueba el puerto, firewall, reenvío
  del router y que `bind-address` sea `0.0.0.0` para acceso directo.
- Las protecciones se guardan por UUID en
  `plugins/HinoProtections/protections.yml`. Haz una copia de seguridad antes
  de actualizar Paper, el plugin o ese archivo.

## Compilar

```text
mvn clean package
```

El resultado queda en `target/HinoProtections-0.3.2.jar`. La compilacion
ejecuta pruebas de progresion, recetas, geometria, coordenadas negativas y
sesiones seguras del portal.

## Actualizacion desde 0.1.x o 0.2.x

El formato de `protections.yml` sigue siendo compatible. La configuracion nueva
se combina con la existente al iniciar. Si el límite de amigos seguía en el
valor predeterminado anterior de 50, se cambia automáticamente a 5; los valores
personalizados se conservan. Aun asi, haz siempre una copia de seguridad antes
de actualizar Paper o el plugin.
