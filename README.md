import pandas as pd
import os
from google.colab import drive

def dividir_archivo(ruta_archivo, num_lineas=19999):
    # Leer el archivo
    df = pd.read_csv(ruta_archivo)

    # Dividir el DataFrame en múltiples DataFrames más pequeños
    chunks = [df[i:i + num_lineas] for i in range(0, len(df), num_lineas)]

    # Obtener la ruta base y el nombre del archivo
    ruta_base = os.path.dirname(ruta_archivo)
    nombre_base = os.path.basename(ruta_archivo)

    # Crear directorios para CSV y JSON si no existen
    ruta_csv = os.path.join(ruta_base, 'CSVs')
    os.makedirs(ruta_csv, exist_ok=True)

    ruta_json = os.path.join(ruta_base, 'JSONs')
    os.makedirs(ruta_json, exist_ok=True)

    # Escribir cada DataFrame más pequeño a un nuevo archivo CSV y JSON
    for i, chunk in enumerate(chunks):
        nombre_salida_csv = os.path.join(ruta_csv, f'parte{i+1}_{nombre_base}')
        chunk.to_csv(nombre_salida_csv, index=False)

        nombre_salida_json = os.path.join(ruta_json, f'parte{i+1}_{nombre_base}.json')
        chunk.to_json(nombre_salida_json, orient='records', lines=True)

if __name__ == "__main__":
    # Montar Google Drive
    drive.mount('/content/drive')

    # Especificar la ruta del archivo
    ruta_archivo = '/content/drive/MyDrive/nemobile Portugal/Operaciones nemobile Portugal/Clientes/Banco de Venezuela/Proyecto SIDIS Marketing/Campañas/21092023/Simple TV/DataSimpleTVSIDIS.csv'

    # Preguntar al usuario por el número de líneas por división
    num_lineas = int(input("¿Cuántas líneas desea por cada archivo? "))

    # Ejecutar la función
    dividir_archivo(ruta_archivo, num_lineas)
