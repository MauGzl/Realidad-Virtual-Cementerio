# Ficha técnica – La última luz del cementerio

**Asignatura:** Realidad Virtual y Aumentada (TILE12) · **Unidad 2** · Desafío de Experiencia Inmersiva
**Estudiante(s):** Mauricio González
**Unity:** 6000.0.84f1 (Unity 6) · Google Cardboard XR Plugin · Android (IL2CPP, ARM64, OpenGLES3)

## Temática y misión
Son las 23:57 y el farol del cementerio se apagó. El jugador debe encender el farol, cruzar la cripta y despertar la campana del guardián para abrir el portal y escapar antes de medianoche.

## Zonas y acciones
| | Zona | Acción (autoclick por mirada) | Consecuencia |
|---|---|---|---|
| 1 | **Entrada del cementerio** (exterior, azul oscuro) | **Acción 1: encender el farol** | Cambia de color, enciende luz y sonido, habilita la puerta de la cripta |
| 2 | **La cripta** (interior cerrado, luz roja) | **Acción 2: despertar la campana** | Se balancea, cambia de color, suena y habilita el portal |

## Flujo del recorrido
Inicio (diálogo de misión) → teletransportación por pads → Acción 1 → puerta de la cripta (si el farol está apagado: «El sello todavía está apagado») → **ScreenFade** → Zona 2 (diálogo del guardián) → Acción 2 → portal → mensaje final → botón verde **Reiniciar** (vuelve al inicio con fundido).

## Requisitos cubiertos
- **R1** dos zonas diferenciadas conectadas por teletransporte · **R2** dos acciones distintas · **R3** retícula + autoclick (cancela al apartar la mirada, no se repite) · **R4** 4 pads (2 por zona) · **R5** ScreenFade en todo traslado · **R6** diálogo World Space en cada zona · **R7** ambientación (paquete de cementerio + calabazas, lápidas, velas, pilares) · **R8** prueba en teléfono · **R9** inicio, final y reinicio · **R10** proyecto organizado.

## Scripts (Assets/Scripts)
| Script | Función |
|---|---|
| `HeadPoseDriver` | Orienta la cámara con el visor/giroscopio; en el editor, con el mouse (clic derecho) |
| `GazePointer` | Retícula, rayo de mirada, temporizador de autoclick, cancelación y anti-repetición |
| `GazeInteractable` | Objeto mirable: enter/exit visual y `Activate()` |
| `FadeController` | ScreenFade (FadeOut → trasladar → FadeIn) |
| `DialogBubble` | Burbuja de diálogo World Space |
| `GameManager` | Condiciones, acciones, teleport, final y reinicio |
| `Editor/VRSceneBuilder` | Herramienta de editor que arma la escena y configura Android |

## Instrucciones de ejecución
1. Abrir con Unity 6000.0.84f1 con módulo Android instalado.
2. Editor: Play; **mantener clic derecho** para mirar; mantener la mirada sobre un objeto ~1,5 s (o clic izquierdo).
3. Teléfono: File → Build Profiles → Android → Build and Run, y usar con visor Cardboard.

## Fuentes y licencias de assets
- Paquete de cementerio (escena Demo6): _(nombre, autor, enlace y licencia del Asset Store)_.
- Paquete «BK_AlchemistHouse»: _(nombre, autor, licencia)_ — si se usa.
- Sonidos: generados por código. Objetos de interacción: primitivas de Unity.

## Declaración de uso de IA
- **Herramienta:** Claude (Anthropic).
- **Propósito:** generación y adaptación de scripts (mirada, autoclick, fade, diálogos, lógica) y script de editor para armar la escena.
- **Verificación:** lectura y comprensión del código, prueba en el editor 

## Estado de la prueba en Android y errores conocidos
_(completar: modelo de teléfono, si funcionó con visor, y problemas que encontraste)_

---

# Guía para la explicación individual (5 min)

- **¿Qué objeto detecta la mirada y cómo se comunica con la acción?** `GazePointer.LateUpdate` lanza un `Physics.Raycast` desde la cámara hacia adelante; busca un `GazeInteractable` en el objeto golpeado; al cumplirse el tiempo llama `Activate()`, que avisa a `GameManager.HandleAction` con el `kind` del objeto.
- **¿Cómo cancela el autoclick y evita repetir?** Si el objeto mirado cambia (o se mira al vacío) el temporizador vuelve a 0. La bandera `fired` bloquea nuevas activaciones hasta que se aparte la mirada.
- **¿Qué condición habilita el acceso o la salida?** En `GameManager.HandleAction`: `case Gate` exige `lanternOn`; `case Portal` exige `bellRung`.
- **¿Cuándo ocurren Fade Out, traslado y Fade In?** En `GameManager.TeleportRoutine`: `yield return fade.FadeOut()` → `MovePlayerTo(dest)` → `yield return fade.FadeIn()`.
- **¿Qué problema detectaste en el teléfono y cómo lo resolviste?** _(escribe uno real que te haya pasado)_
- **¿Qué desarrollaste, reutilizaste e IA verificaste?** _(completa según tu caso)_
