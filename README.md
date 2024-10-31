Este script automatiza la reproducción de videos en ciertos momentos del día y controla el apagado de la computadora. A continuación te explico las secciones clave, paso a paso, para que puedas documentarlo y subirlo a GitHub.

### 1. Verificación de VLC y Videos

```batch
set "vlc_path=C:\Program Files\VideoLAN\VLC\vlc.exe"
set "video_path=%~dp0"  :: Carpeta donde se ejecuta el .bat
```

- **Propósito**: Define la ruta de VLC y la ubicación de los videos.
- **Funcionamiento**: Primero verifica que VLC esté instalado en el sistema en la ruta especificada, y luego revisa que los videos (`DESAYUNO.mp4`, `ALMUERZO.mp4` y `CENA.mp4`) existan en la carpeta donde está el script.

### 2. Ciclo Principal: `:mainLoop`

```batch
:mainLoop
for /F "tokens=1 delims=:" %%h in ("%time%") do set /A hour=%%h
```

- **Propósito**: Este es el ciclo que controla el flujo del programa.
- **Funcionamiento**: Cada 60 segundos revisa la hora para saber cuál video debe reproducirse (`DESAYUNO`, `ALMUERZO` o `CENA`).

### 3. Control de VLC y Cambio de Videos

```batch
tasklist /fi "imagename eq vlc.exe" | find /i "vlc.exe" >nul
```

- **Propósito**: Verifica si VLC ya está ejecutándose.
- **Funcionamiento**: Si VLC no está en ejecución o está reproduciendo el video incorrecto, lo cierra y abre el video correspondiente a la hora actual.

### 4. Condición de Apagado a las 9 PM

```batch
if %hour% GEQ 21 (
    taskkill /im vlc.exe /f >nul
    choice /c CY /t 60 /d Y /m "Presione 'C' para cancelar el apagado o espere 60 segundos para continuar."
    if errorlevel 1 (
        goto mainLoop
    ) else (
        shutdown /s /f /t 0
    )
)
```

- **Propósito**: Cierra VLC y apaga el sistema a las 9 PM.
- **Funcionamiento**: Da la opción de cancelar el apagado presionando `C`, en cuyo caso se vuelve al ciclo principal. Si no se cancela, el sistema se apaga.

### Subiendo el Script a GitHub

Para subir este script a GitHub y documentarlo:

1. **Crea un nuevo repositorio en GitHub**:
   - Ve a [https://github.com/](https://github.com/) y selecciona **New Repository**.
   - Dale un nombre descriptivo, como `AutoVideoPlayer`.
   - Marca la opción para incluir un **README.md**.

2. **Agrega el script al repositorio**:
   - Puedes subir el archivo `.bat` directamente o clonarlo en tu computadora y añadir el archivo localmente.

3. **Documenta el código**:
   - En el archivo `README.md`, explica qué hace el script, incluyendo:
     - **Requisitos**: Asegúrate de que VLC esté en la ruta correcta y de que los videos estén en la misma carpeta que el script.
     - **Instrucciones de uso**: Explica cómo configurar y ejecutar el script, así como qué esperar en cada intervalo de tiempo.

### Ejemplo de Documentación en `README.md`

```markdown
# AutoVideoPlayer

Este script en `.bat` automatiza la reproducción de videos (Desayuno, Almuerzo, Cena) en momentos específicos del día y controla el apagado automático a las 9 PM.

## Requisitos
- **VLC Media Player**: Instalar en `C:\Program Files\VideoLAN\VLC\vlc.exe`.
- **Videos**: Colocar `DESAYUNO.mp4`, `ALMUERZO.mp4` y `CENA.mp4` en la misma carpeta que el script.

## Uso
1. Inicia el script al encender la computadora.
2. El script verifica la hora cada 60 segundos y reproduce el video adecuado.
3. A las 9 PM, VLC se cerrará y el sistema pedirá confirmación para apagarse.
4. Si se cierra VLC manualmente, el script lo reiniciará con el video correspondiente.

### Notas Adicionales
Si VLC o los videos cambian de ubicación, actualiza `vlc_path` o `video_path` en el script.
```

Una vez documentado, sube el archivo `README.md` y el `.bat` al repositorio. Esto facilitará que otros usuarios comprendan el propósito y funcionamiento del script.
