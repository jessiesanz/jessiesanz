import xml.etree.ElementTree as ET
import os
import pandas as pd

# Ruta al directorio con los archivos XML
directorio_xml = r"C:\Users\PC\OneDrive\Escritorio\facturas\COBRANZA 2024\2.-FEBRERO 2024\CFDIS FEBRERO 2024-20240223T221038Z-001\CFDIS FEBRERO 2024"

# Lista para almacenar los datos de todos los archivos
datos_todos_archivos = []

# Función para extraer datos de un archivo XML
def extraer_datos(archivo_xml):
    arbol = ET.parse(archivo_xml)
    raiz = arbol.getroot()

    # Diccionario para los datos del archivo actual
    datos_archivo = {}

    # Iterar sobre todos los elementos y sus atributos
    for elem in raiz.iter():
        for nombre, valor in elem.attrib.items():
            # Limpiar el nombre del campo
            campo_limpio = nombre.split("}")[-1]  # Elimina el namespace
            parte_relevante = campo_limpio.split("@")[-1] if "@" in campo_limpio else campo_limpio
            datos_archivo[parte_relevante] = valor

    return datos_archivo

# Procesar cada archivo XML en el directorio
for archivo in os.listdir(directorio_xml):
    if archivo.endswith('.xml'):
        ruta_completa = os.path.join(directorio_xml, archivo)
        datos_archivo = extraer_datos(ruta_completa)
        datos_todos_archivos.append(datos_archivo)

# Convertir la lista de datos en un DataFrame
df_todos_archivos = pd.DataFrame(datos_todos_archivos)

# Ruta al archivo CSV donde se guardarán los resultados consolidados

# Guardar el DataFrame en un archivo CSV
df_todos_archivos.to_csv("pagos.csv", index=False)

Untitled Folderprint(f"Datos consolidados guardados en: {archivo_csv_final}")
