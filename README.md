# Poderosa-COMM Visual Engine

Motor visual para convertir datos JSON enviados desde Google Apps Script en imágenes PNG ejecutivas de 1600 × 900 px, listas para insertarse en Google Slides.

## Arquitectura

Google Sheets → Apps Script → Render → Express/EJS → Puppeteer → PNG → Google Slides.

## Diapositivas

10, 11, 12, 13, 14, 15, 17, 18 y 19.

## Ejecución local

```bash
npm ci
npm start
```

Pruebas:

- `http://localhost:3000/health`
- `http://localhost:3000/test-slide11-png`

## Despliegue en Render

1. Subir el contenido de esta carpeta a GitHub.
2. En Render: **New → Blueprint**.
3. Seleccionar el repositorio `Poderosa-COMM`.
4. Render leerá `render.yaml`.
5. Copiar la URL pública y colocarla en `RENDER_BASE_URL` del Apps Script.
6. Copiar la variable `RENDER_API_KEY` de Render a las propiedades del Apps Script.

## Seguridad

Si `RENDER_API_KEY` está definida, los POST `/render/slideXX` requieren el header:

```http
x-api-key: TU_CLAVE
```

## Mejoras incluidas

- Nombres y títulos propios de Poderosa.
- Sin datos ficticios en los endpoints de producción.
- Conversión correcta de números con formato peruano y estadounidense.
- API key opcional.
- Reutilización del navegador Puppeteer para acelerar la generación.
- Configuración de Render versionada mediante `render.yaml`.

## Apps Script

Los IDs del Google Sheets, Google Slides y carpeta de salida se configuran en Apps Script, no en este repositorio.

Ejemplo de llamada:

```javascript
const response = UrlFetchApp.fetch(RENDER_BASE_URL + '/render/slide11', {
  method: 'post',
  contentType: 'application/json',
  headers: {
    'x-api-key': PropertiesService.getScriptProperties().getProperty('RENDER_API_KEY')
  },
  payload: JSON.stringify(payload),
  muteHttpExceptions: true
});
```
