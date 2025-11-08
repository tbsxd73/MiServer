#!/usr/bin/env bash
# ===================================================
#  🧩 ZERO.sh — Script de arranque para servidor Minecraft
# ===================================================
# Autor: Edsthilaire (Zero)
# Versión: 1.0
#
# Descripción:
#   Este script inicia un servidor de Minecraft optimizado con parámetros G1GC.
#   Descarga automáticamente una extensión desde GitHub (si no existe),
#   y reinicia el servidor en caso de apagarse o crashear.
#
# ===================================================
# 📘 INSTRUCCIONES PARA USUARIOS:
#
# 1️⃣ Coloca este archivo (ZERO.sh) en la carpeta de tu servidor.
# 2️⃣ Asegúrate de que tu servidor se llame "server.jar".
# 3️⃣ Abre la terminal en esa carpeta.
# 4️⃣ Da permisos de ejecución con el siguiente comando:
#       chmod +x ZERO.sh
# 5️⃣ Ejecuta el script con:
#       ./ZERO.sh
#
# ✅ Si “Permission denied” aparece, usa:
#       bash ZERO.sh
#
# (Opcional) Detén el script con CTRL + C.
#
# ===================================================


# ==== CONFIGURACIÓN ====
FILE_NAME="server.jar"    # Nombre del archivo del servidor
MEMORY_MB=8096            # Memoria asignada al servidor (en MB)
EXT_URL="https://raw.githubusercontent.com/edsthilaire/ZERO/refs/heads/main/extension.sh"
EXT_FILE="extension.sh"   # Nombre local del archivo de extensión


# ==== VERIFICAR PERMISOS DEL ARCHIVO ====
if [ ! -x "$0" ]; then
    echo "⚙️  Dando permisos de ejecución automáticamente..."
    chmod +x "$0" 2>/dev/null || echo "❌ No se pudieron aplicar permisos automáticos. Usa: chmod +x ZERO.sh"
fi


# ==== COMPROBAR / DESCARGAR EXTENSIÓN ====
if [ ! -f "$EXT_FILE" ]; then
    echo "🔽 Descargando extensión desde GitHub..."
    if curl -fsSL "$EXT_URL" -o "$EXT_FILE"; then
        chmod +x "$EXT_FILE"
        echo "✅ Extensión descargada correctamente."
    else
        echo "❌ Error al descargar la extensión. Verifica tu conexión o el enlace."
    fi
else
    echo "✅ Extensión encontrada localmente."
fi


# ==== EJECUTAR EXTENSIÓN ====
if [ -f "$EXT_FILE" ]; then
    echo "🧩 Ejecutando extensión ZERO..."
    bash "./$EXT_FILE"
else
    echo "⚠️  No se encontró la extensión. Continuando sin ella..."
fi


# ==== INICIAR SERVIDOR MINECRAFT ====
while true; do
    echo "🚀 Iniciando servidor con ${MEMORY_MB}MB de RAM..."
    java -Xms${MEMORY_MB}M -Xmx${MEMORY_MB}M \
         -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 \
         -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch \
         -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 \
         -XX:InitiatingHeapOccupancyPercent=15 \
         -XX:G1MixedGCLiveThresholdPercent=90 \
         -XX:G1RSetUpdatingPauseTimePercent=5 \
         -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem \
         -XX:MaxTenuringThreshold=1 \
         -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true \
         -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 \
         -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 \
         -jar "$FILE_NAME" --nogui

    echo "🌀 El servidor se ha detenido."
    echo "Reiniciando en 5 segundos... (Presiona Ctrl+C para cancelar)"
    sleep 5
done
