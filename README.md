# 🏟️ n8n-stadium-print-automation

Sistema automatizado para la gestión e impresión de comprobantes de venta (vales) mediante n8n. Este flujo está diseñado para operar en un estadio, permitiendo procesar ventas desde múltiples puntos de facturación (POS) y dirigirlas a impresoras térmicas específicas de forma remota.

## 📋 Descripción del Flujo
El flujo de trabajo realiza un monitoreo constante de nuevas facturas, genera un diseño visual personalizado en HTML y gestiona la cola de impresión sin intervención manual.

### Componentes Técnicos
* **Disparador:** Ejecución automática cada 10 segundos para consulta de API.
* **Integración de Facturación:** Conexión con la API de Siigo POS para obtener documentos tipo "FV" (Factura de Venta).
* **Validación de Datos:** Uso de PostgreSQL para verificar si una factura ya fue impresa, evitando duplicidad de vales.
* **Gestión de Medios:** Descarga de imágenes y recursos desde Google Drive según el producto facturado.
* **Renderizado:** Conversión de plantillas HTML dinámicas a PDF mediante la API de api2pdf.
* **Impresión Remota:** Envío de trabajos a impresoras físicas a través de PrintNode, con enrutamiento inteligente basado en la caja de origen (Caja 1, POS2, POS3).

## 🛠️ Tecnologías Utilizadas
* **n8n:** Motor de automatización de flujos de trabajo.
* **PostgreSQL:** Base de datos relacional para el control de integridad y registro de impresión.
* **Node.js (Code Nodes):** Procesamiento de buffers, conversión de binarios a Base64 y lógica de unión de datos.
* **HTML/CSS:** Diseño de la estructura visual del vale térmico.

## ⚙️ Configuración y Nodos Clave
1. **Identificar Impresora:** Nodo `Switch` que redirige el flujo según el ID de la caja vendedora.
2. **Generación de HTML:** Nodo `Set` que construye el voucher con dimensiones específicas (6cm de ancho) para papel térmico.
3. **Control de Duplicados:** Query SQL que utiliza `ON CONFLICT DO NOTHING` para asegurar que cada producto de la factura se procese una sola vez.

## 🚀 Mejoras Futuras
* Implementación de una tabla intermedia en PostgreSQL para cachear imágenes de Drive y reducir tiempos de respuesta.
* Integración de un generador de PDF local en el VPS para eliminar la dependencia de APIs externas y reducir la latencia.

---
**Nota de Seguridad:** Este repositorio contiene plantillas de exportación con datos de credenciales ofuscados. Asegúrese de configurar sus propias credenciales en n8n antes de importar el archivo JSON.
