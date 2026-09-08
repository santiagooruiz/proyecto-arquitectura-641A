# Requerimientos no funcionales

| # | Atributo | Metrica | Umbral | Condicion de carga | Verificacion | Consecuencia si no se cumple |
|---|---|---|---|---|---|---|
| 1 | Rendimiento | p95 de latencia del panel de obligaciones | menor a 400 ms | 50 usuarios concurrentes, con 24 meses de historial por usuario | Prueba de carga sobre el endpoint del panel | El usuario percibe la aplicacion lenta, deja de consultarla a diario y se le pasa un pago |
| 2 | Confiabilidad | Porcentaje de recordatorios despachados dentro de la ventana programada | 99.5% con desviacion menor a 5 minutos | Pico de fin de mes: 400 obligaciones venciendo el mismo dia | Auditoria del log de envios contra la agenda programada, durante 30 dias | El usuario paga tarde, asume intereses de mora y riesgo de reporte en centrales de riesgo. El sistema pierde su unica razon de ser |
| 3 | Seguridad | Porcentaje de datos sensibles cifrados en reposo y en transito | 100%: AES-256 en reposo, TLS 1.2 o superior en transito | Toda operacion, en cualquier condicion de carga | Revision del esquema de base de datos y escaneo del certificado | Exposicion del perfil financiero completo del usuario e incumplimiento de la Ley 1581 de 2012 |

## Escenarios completos

### Escenario 1

- Fuente: El planificador de notificaciones del sistema
- Estimulo: Se cumple la ventana de 72 horas antes de la fecha de vencimiento de una obligacion registrada
- Artefacto: Servicio de notificaciones y proveedor de correo y push
- Entorno: Operacion normal en pico de fin de mes, del dia 28 al 31, con 400 obligaciones venciendo el mismo dia
- Respuesta: El sistema genera el recordatorio, lo despacha al canal configurado por el usuario y registra el envio con marca de tiempo
- Medida: El 99.5% de los recordatorios sale con desviacion menor a 5 minutos frente a la hora programada, sin envios perdidos ni duplicados