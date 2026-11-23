# Dockerlabs-CLI

Script en Bash para descargar máquinas de [DockerLabs](https://dockerlabs.es) desde la terminal,
junto con un writeup de cada una para cuando te atasques.

Puedes buscar las máquinas por nombre o por dificultad, o dejar que el script elija una al azar.

> ⚠️ **Proyecto no oficial.** Este script no está hecho ni respaldado por DockerLabs. Úsalo respetando
> sus [condiciones de uso](https://dockerlabs.es/condiciones-uso).

## Requisitos

- Linux (pensado para Kali)
- `curl` y `python3` (vienen de serie en Kali)
- `docker` y `unzip` para desplegar las máquinas

## Instalación

```bash
git clone https://github.com/luigi70289/Dockerlabs-CLI
cd Dockerlabs-CLI
chmod +x dockerlabs.sh
```

## Uso

| Comando | Qué hace |
|---|---|
| `./dockerlabs.sh -l` | Lista todas las máquinas con su dificultad |
| `./dockerlabs.sh -x <dificultad>` | Lista las máquinas de esa dificultad y te deja elegir una para descargar (`r` = una al azar) |
| `./dockerlabs.sh -xr <dificultad>` | Descarga una máquina al azar de esa dificultad y un writeup al azar de ella |
| `./dockerlabs.sh -n <nombre>` | Descarga solo la máquina |
| `./dockerlabs.sh -w <nombre>` | Descarga la máquina y te deja elegir uno de sus writeups |
| `./dockerlabs.sh -wr <nombre>` | Descarga la máquina y un writeup al azar |
| `./dockerlabs.sh -w <nombre> -e <número o autor>` | Descarga la máquina y el writeup que indiques, sin preguntar |
| `./dockerlabs.sh -s <nombre> -u <url>` | Envía (propone) un writeup de esa máquina a DockerLabs (requiere cuenta) |
| `./dockerlabs.sh -h` | Muestra la ayuda |

Dificultades: `muy-facil`, `facil`, `medio` y `dificil`.

### Ejemplos

```bash
./dockerlabs.sh -x muy-facil   # ver las máquinas muy fáciles y elegir una
./dockerlabs.sh -xr facil      # una máquina fácil cualquiera
./dockerlabs.sh -n trust       # la máquina Trust
./dockerlabs.sh -wr trust      # Trust y un writeup al azar
```

No hace falta escribir mayúsculas ni tildes: `-n "perrito magico"` encuentra *Perrito Mágico*.
Si escribes solo una parte del nombre y hay varias máquinas que coinciden, el script te las enseña.

## Qué se descarga

Todo se guarda en la carpeta desde la que ejecutas el script:

- `<maquina>.zip`: la máquina. Dentro vienen `<maquina>.tar` y `auto_deploy.sh`.
- `<Maquina>_writeup_<autor>.md`: el writeup, en Markdown.

## Enviar un writeup

Si has resuelto una máquina y quieres proponer tu writeup para que aparezca en DockerLabs, puedes
enviarlo con `-s`. **No sube un archivo: manda el enlace** de tu writeup (por ejemplo, tu `.md` en
GitHub), y queda pendiente de que el equipo de DockerLabs lo revise y lo publique.

Necesitas tu cuenta de DockerLabs. El script pide el usuario y la contraseña en el momento (la
contraseña no se ve al teclear) y no los guarda. Si prefieres no escribirlos cada vez, puedes
ponerlos en las variables `DOCKERLABS_USER` y `DOCKERLABS_PASS`.

```bash
./dockerlabs.sh -s trust -u https://github.com/tuusuario/writeups/blob/main/trust.md
./dockerlabs.sh -s trust -u https://youtu.be/xxxx -t video   # si es un vídeo
```

- `-u` es la URL de tu writeup (obligatoria).
- `-t` es el tipo: `texto` (por defecto) o `video`.
- Envía **solo writeups tuyos**. No subas el trabajo de otra persona como si fuera tuyo.

## Desplegar la máquina

```bash
unzip trust.zip
sudo bash auto_deploy.sh trust.tar
```

Cuando termina, te dice la IP de la máquina (normalmente `172.17.0.2`). Para pararla y borrarla,
pulsa `Ctrl+C` en esa terminal.

## Uso responsable

- El script hace las mismas peticiones que harías tú con el navegador, y solo cuando lo ejecutas.
  No lo metas en bucles ni lo uses para descargar todas las máquinas de golpe: no sobrecargues
  los servidores de DockerLabs.
- Se identifica ante la web como `Dockerlabs-CLI`, para que el creador sepa de dónde
  vienen las peticiones.
- Las máquinas se descargan siempre del servidor oficial. No las vuelvas a subir a otros sitios.
- Las máquinas son vulnerables a propósito. Despliégalas solo en tu equipo y nunca ataques sistemas
  reales sin permiso.
- Si el creador de DockerLabs pide que este script no se use o que cambie algo, se hará caso.

## Sobre los writeups

- Solo se descargan los writeups que están en GitHub como archivo `.md`. Los que son vídeos o están
  en otras webs (Medium, Notion, Google Drive…) se ignoran porque no se pueden bajar desde un script.
- Con `-wr`, si el writeup elegido falla, el script prueba con otro al azar.
- Si una máquina no tiene writeups en GitHub, el script avisa y descarga la máquina igualmente.

Intenta resolver la máquina tú primero y abre el writeup solo si te atascas 😉

## Cómo funciona

El script lee la lista de máquinas de la página principal de dockerlabs.es, saca el enlace del `.zip`
de la página de descarga de cada máquina y pide los writeups a `dockerlabs.es/api/writeups/<máquina>`. Si DockerLabs cambia su web, puede que el script deje de
funcionar hasta que se actualice.

## Créditos

Las máquinas son de [DockerLabs](https://dockerlabs.es), creado por El Pingüino de Mario, y los writeups
son de sus autores. Este script solo facilita la descarga.
