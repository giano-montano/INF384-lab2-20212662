# Errores encontrados

1. Las versiones de dependencias a instalar no son de un archivo de bloqueo, sino que descarga de rangos abiertos de versiones. Ello aumenta la probabilidad de que el sistema sea vulnerado.
.github/workflows/pipeline.yml:26 y .github/workflows/pipeline.yml:56

2. Las dependencias se instalan por segunda vez en lugar de recuperarlas de la instalación en el job anterior (caché); esto afecta directamente la eficiencia en el uso de computación. **Este es el error que aumenta el tiempo de ejecución (56 segundos en promedio)**.
.github/workflows/pipeline.yml:55-56 

3. Si el primer job "validar" falla, no impide que el job "publicar" se ejecuta, dado que este último no incluye la cláusula "needs". Publica paquetes cuyo código fuente no pasó el análisis de calidad.
.github/workflows/pipeline.yml:41

4. No existe nombramiento y versionado semántico automático cuando se publica el paquete, sino que se guarda con un nombre genérico "paquete". No hay forma de saber a qué versión se refiere el paquete publicado fuera de otros metadatos, lo que dificulta trazabilidad.
.github/workflows/pipeline.yml:64

# Relación con caso transversal

El error número 3 se relaciona con el caso 2 (Financiera Los Andes) en el sentido de que la empresa va al otro extremo: sus revisiones de seguridad son muy largas y hacen que el pipeline "se detenga" (tome mucho más tiempo del que podría tomarse).

# Métrica DORA a intervenir principalmente

Tiempo de entrega del cambio: Al reutilizar las dependencias descargadas, se ahorra tiempo.
Por otro lado, también podría ser porcentaje de fallas en el cambio, dado que menos cambios llegarán sin haber pasado por el análisis de calidad (Quality Gate).

# El proxy
Duración de la ejecución del pipeline; se espera que baje.



