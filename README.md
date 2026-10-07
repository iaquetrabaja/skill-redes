# skill-redes

Una skill para Claude que lleva una cuenta de **TikTok e Instagram en español**.
Trece modos en un solo fichero y cinco herramientas en Python que se ejecutan de
verdad, sin dependencias y sin conectar nada.

Escribe guiones de vídeo corto con el gancho puntuado y una hoja de ritmo con
tiempos, busca lo que está funcionando en tu nicho y lo ordena por cuánto supera
a su propia cuenta, revisa el texto del post enseñando lo que se ve antes del
«... más», puntúa tu perfil sobre 100, te prepara la semana y quita a tus textos
el tufo a IA antes de que nadie los lea.

**No publica nada.** Escribe. Publicas tú.

## Instalación

Pega esto en Claude:

```
https://github.com/iaquetrabaja/skill-redes

Instala esta skill y comprueba que funciona pidiéndome un guion.
```

O a mano, en Claude Code:

```bash
git clone https://github.com/iaquetrabaja/skill-redes.git ~/.claude/skills/redes
```

O como plugin:

```
/plugin marketplace add iaquetrabaja/skill-redes
/plugin install redes
```

¿Sin Claude Code? Pega `SKILL.md` al principio de un chat y funciona como modo.
Pierdes las herramientas en Python, que son la mitad de la gracia, pero el resto va.

Después dedica diez minutos a `plantillas/voz.md`: cópialo en
`~/.claude/redes/voz.md` y rellénalo, o pásale a Claude tres vídeos tuyos y dile
«escríbeme el voz.md a partir de esto». Todos los modos lo leen.

## Los trece modos

| modo | qué hace |
| --- | --- |
| **1 · Guion** | Una idea en un vídeo corto: tres ganchos de [26 fórmulas](herramientas/ganchos.json), puntuados, el guion, el texto en pantalla y la hoja de ritmo. |
| **2 · Viral** | Busca lo que funciona en tu nicho, lo ordena por múltiplo sobre la mediana de cada cuenta y dice qué fórmula usa. |
| **3 · Texto** | El texto del post o la descripción de TikTok, revisado. Enseña lo que se ve antes del «... más». |
| **4 · Carrusel** | Portada que hace deslizar, una idea por diapositiva, resumen para capturar y petición. |
| **5 · Historias** | La secuencia del día, qué sticker hace qué trabajo y el embudo honesto hacia el privado. |
| **6 · Perfil** | Puntúa tu perfil con una [rúbrica de 100 puntos](herramientas/rubrica_perfil.json) y reescribe por orden de puntos perdidos. |
| **7 · Plan** | La semana: qué publicar, en qué formato, con qué fórmula, y las diez cuentas donde comentar. |
| **8 · Humanizar** | Quita invisibles, tipografía de máquina y más de 200 muletillas en español; avisa de las estructuras que hay que reescribir. |
| **9 · Comentar** | Comentarios en posts de otros. Nueve tipos, según lo que sea el post. Nunca «🔥🔥🔥». |
| **10 · Responder** | Los comentarios de tus posts: los clasifica (palabra clave, cliente, aporta, pregunta, apoyo, ruido) y contesta en ese orden. |
| **11 · Mensajes** | El mensaje de la palabra clave, el primero a alguien conocido, la propuesta de colaboración y dos seguimientos. |
| **12 · Reutilizar** | Un vídeo largo, un pódcast o una newsletter en una semana de vídeos y carruseles que se sostienen solos. |
| **13 · Auditoría** | Lo que ya publicaste, ordenado por múltiplo y envíos por alcance, no por visitas. |

## Las herramientas

Solo librería estándar de Python (3.10 o más). Nada sale de tu ordenador. Todas
aceptan `--json` y también se pueden importar:

```python
from herramientas import puntuar_gancho, hoja_de_ritmo, revisar_pie, humanizar, puntuar, ranking_viral
```

### Ganchos

```bash
python -m herramientas.ganchos ganchos.txt
```

```
RANKING DE GANCHOS
==============================================================================
->  81.4 FUERTE  Nadie te cuenta que tus primeros 30 vídeos tienen que fra...
        lo más flojo: tensión (70)
    81.4 FUERTE  18.000 euros me costó no poner una cláusula en el contrato.
        lo más flojo: a quién le habla (70)
    80.2 FUERTE  Deja de publicar tres veces al día.
        lo más flojo: tensión (70)
     0.0 FLOJO   Hola chicos, en el vídeo de hoy os voy a enseñar algo de ...
        lo más flojo: lo importante delante (0)
        rompe tratos: Preámbulo de vídeo. Bórralo y empieza por el resultado.
        rompe tratos: Saludo. Nadie ha venido a la app a que le saluden.

Graba el primero. Si el primero no llega a 50, ninguno de estos es el gancho.
```

Mira cinco cosas: que se diga en menos de tres segundos, que tenga algo concreto
(una cifra, un nombre, una marca, también dichos en letra: «veinte mil euros»),
que haya algo en juego, que lo importante vaya delante y que le hable a alguien.
Y caza los saludos, «en el vídeo de hoy» y «deja de hacer scroll».

### Ritmo

```bash
python -m herramientas.ritmo guion.txt --objetivo 20
```

```
HOJA DE RITMO  ·  61 palabras  ·  ~21.5 s a 170 ppm  ·  objetivo 20 s
========================================================================
  0:00.0   3.2s  GANCHO     Los presupuestos me llevaban cinco horas. Ahora, veinte minutos.
                            ^ el gancho dura 3.2 s, pasa de los 3 s
  0:03.2   4.2s             Dicto la visita en una nota de voz al salir del cliente.
                            ^ 4.2 s en un solo plano
  0:07.4   5.7s  MEDIO      ChatGPT me lo convierte en un presupuesto con mis precios y me lo deja en Gmail.
                            ^ 5.7 s en un solo plano
  0:13.1   3.9s             Lo reviso, cambio dos cosas y lo mando desde la furgoneta.
  0:16.9   4.6s  CTA        Comenta PRESU y te paso la plantilla. Cinco horas menos a la semana.
                            ^ 4.6 s en un solo plano
------------------------------------------------------------------------
  - La línea 1 tarda 3.2 s en decirse. Déjala en 8 palabras o menos, o el gancho llega cuando ya han decidido.
  - La(s) línea(s) 2, 3, 5 pasan de 4 s. Pártelas o cambia lo que se ve en pantalla dentro de ellas. Un plano quieto es donde la gente se va.
  - Bucle: la última línea repite «cinco, horas» del gancho. Las segundas vueltas son alcance gratis.
  - Duración en el objetivo (21.5 s de 20.0 s).
```

Una línea es un plano. Usa 170 palabras por minuto, que es un ritmo normal de
vídeo corto en español; mídete y ajústalo con `--ppm`.

### Texto del post

```bash
python -m herramientas.pie texto.txt --palabras "autónomos,presupuestos"
```

```
REVISIÓN DEL TEXTO (instagram)  ·  339 / 2200  ·  7 hashtags  ·  2 petición(es)
===============================================================================

  LO QUE SE VE ANTES DEL «MÁS»
  +------------------------------------------------------+
  | Hola a todos! En el mundo actual, la IA juega un     |
  | papel fundamental en los negocios 🚀🔥                 |
  | Sin embargo, es importante destacar que              |
  +--------------------------------------------- ... más +

  PASS  LONGITUD               339 / 2200 caracteres
  FAIL  PRIMERA LÍNEA          empieza saludando. Nadie hace scroll para que le saluden
  PASS  GANCHO CONCRETO        1 número(s) o nombre(s) en lo que se ve
  FAIL  HASHTAGS               7 hashtags, por encima del tope de 5 de Instagram. Los que pasan del quinto no cuentan y el bloque se lee como antiguo
  PASS  SITIO DE LOS HASHTAGS  los hashtags quedan debajo del corte
  PASS  ENLACES                ningún enlace muerto en el texto
  WARN  UNA SOLA PETICIÓN      2 peticiones (compartirlo, seguir la cuenta). Pedir dos cosas es como no pedir ninguna
  PASS  EMOJIS                 2 emoji(s), 0.6 por cada 100 caracteres
  WARN  TÉRMINOS DE BÚSQUEDA   1/2 presentes, 0 en lo que se ve. Faltan: presupuestos
-------------------------------------------------------------------------------
  VEREDICTO  ARREGLAR
```

Con `--red tiktok` el corte pasa a unos 90 caracteres, el límite a 4.000 y los
hashtags dejan de tener tope duro (Instagram lo bajó a cinco el 18 de diciembre
de 2025; TikTok no tiene tope, pero más de cinco o seis no ayuda).

### Humanizar y detectar

```bash
python -m herramientas.humanizar texto.txt --informe > limpio.txt
python -m herramientas.detectar texto.txt limpio.txt
```

```
INFORME DE HUMANIZADO
----------------------
7 restos de máquina quitados, 4 señales de estructura para reescribir a mano

3. MULETILLAS
    1x  comparte con quien lo necesite  -> (borrado)
    1x  es importante destacar que  -> (borrado)
    1x  juega un papel fundamental  -> pesa mucho
    1x  sígueme para más contenido  -> (borrado)
    1x  en el mundo actual  -> hoy
    1x  en este vídeo  -> (borrado)
    1x  sin embargo  -> pero

4. SEÑALES DE ESTRUCTURA (no se arreglan solas: reescríbelas tú)
    1x  Tríada de tres elementos
        Dos o cuatro. Tres es el ritmo por defecto de los modelos.
    2x  Emoji de cohete, fuego, bombilla, chispas o diana
        Un emoji como mucho, y que no sea uno de estos cinco.
    1x  Muro de hashtags (6 o más)
        Tres o cuatro del tema. En Instagram, cinco es el tope.
    5x  Frases de largo uniforme (variación 0.31)
        Parte una frase por la mitad. Deja que otra se alargue. Las máquinas escriben parejo.
```

```
  19.2 MARCADO  ->  39.4 MARCADO   (+20.2)
```

Fíjate en que sigue MARCADO: el humanizador quita muletillas, pero la tríada, los
emojis, los hashtags y las frases todas iguales los tienes que reescribir tú. Eso
es lo que de verdad sube la nota, y es la parte que ningún script puede hacer bien.

Lo que se quita solo: caracteres invisibles (espacios de ancho cero, uniones,
guiones blandos, etiquetas Unicode, espacios duros), tipografía de máquina (raya
en mitad de frase, comillas curvas, «…» en un carácter) y el léxico de
[`muletillas.json`](herramientas/muletillas.json), que está para editarlo. Las
comillas latinas «», los signos ¿ ¡ y la raya de diálogo a principio de línea se
respetan: son español.

### Viral

```bash
python -m herramientas.viral recogidos.tsv --md ~/.claude/redes/viral.md
```

```
REFERENCIAS  ·  3 vídeos  ·  3 cuentas  ·  base: mediana de la cuenta
==============================================================================
    60.0x  gancho   77  #9  Cópialo                  @c                  180.000
           «Copia este mensaje de seguimiento, me costó dos años»
    37.5x  gancho   81  #3  Nadie te cuenta          @a                  412.000
           «Nadie te cuenta que tus primeros 30 vídeos van a fracasar»
     1.3x  gancho   13  -   sin clasificar           @b                1.200.000
           «en este vídeo os enseño mi rutina de mañana»
```

El de 1,2 millones de visitas es el último: para una cuenta con una mediana de
900.000, fue un día normal. El de 180.000, en una cuenta que suele hacer 3.000,
encontró algo, y eso es lo que se puede copiar.

## Lo que hay que saber (la parte honesta)

- **No publica en TikTok ni en Instagram.** Publicar, comentar, seguir o mandar
  mensajes con un navegador automatizado va contra las normas de las dos apps y
  te bloquean la cuenta. Cada modo acaba con un bloque listo para copiar.
- **La excepción son las respuestas automáticas a quien comenta una palabra
  clave**, que las dos plataformas permiten con sus herramientas oficiales o con
  socios aprobados, y solo después de que la persona comente.
- **El modo Viral lee, no rastrea.** Diez cuentas, una docena de vídeos, a ritmo
  humano, con tu navegador y tú delante. Nunca pide contraseñas.
- **El detector son heurísticas locales.** No es GPTZero, Originality, Copyleaks,
  Winston ni Turnitin; no llama a sus APIs y no promete su veredicto. Mide las
  mismas señales de fondo, por eso arreglarlas suele mover también sus números.
  Nadie puede venderte un texto «indetectable».
- **Quitar invisibles es real y es limitado.** Quita los caracteres de formato que
  se cuelan en los textos generados. No es romper ninguna marca de agua estadística.
- **El puntuador de ganchos caza los malos, no elige ganadores.** En la prueba del
  original en inglés separó bien un gancho real de uno malo a propósito (AUC
  0,83) y casi nada los aciertos de un creador de sus fallos (AUC 0,56). La
  adaptación al español mantiene el método, pero no se ha vuelto a medir con un
  corpus español: tómalo como filtro, no como oráculo. Lo que decide entre dos
  ganchos decentes es tu cara, tu montaje, el audio y a quién se lo enseña la app.
- **No se inventa nada.** Si un borrador necesita una cifra que no has dado, vuelve
  con `{{tu dato}}` y un aviso.
- **Las cifras de las plataformas caducan.** Si algo de aquí choca con lo que hacen
  hoy las apps, mandan las apps.

## Ficheros

```
SKILL.md                            la skill: 13 modos
herramientas/ganchos.json           26 fórmulas de gancho con plantilla, ejemplo, texto en pantalla,
                                    para qué sirven, cómo se estropean y regex para reconocerlas
herramientas/ganchos.py             puntuación del gancho en cinco propiedades
herramientas/ritmo.py               del guion a una hoja de ritmo con tiempos
herramientas/pie.py                 lo que se ve antes del «más» y la revisión del texto
herramientas/muletillas.json        el léxico: 204 muletillas, 18 invisibles, 16 señales
herramientas/humanizar.py           las tres pasadas de limpieza
herramientas/detectar.py            las cinco comprobaciones
herramientas/viral.py               orden por múltiplo y nombre de la fórmula
herramientas/rubrica_perfil.json    la rúbrica de perfil de 100 puntos
plantillas/voz.md                   tu voz. Rellénalo primero.
tests/                              python -m unittest discover -s tests
```

## Créditos

Es una adaptación al español y a TikTok de
[instagram-agent-skill](https://github.com/Jakeschincariol/instagram-agent-skill)
de **Jake Schincariol**, publicada con licencia MIT. Las ideas de fondo (las 26
fórmulas, el puntuador de ganchos, la hoja de ritmo, el revisor de textos, el
humanizador con su detector y el orden por múltiplo) son suyas; aquí se han
unido en una sola skill, traducido, adaptado a cómo se habla en español y
ampliado a TikTok.

Adaptación: David García, [IA que trabaja](https://iaquetrabaja.com).

## Licencia

MIT. Cógelo, cámbialo y úsalo.
