# Data Quality Report.

## 1. Exploración inicial.

- 754 valores nulos en `user_age`.
- 172 filas duplicadas exactas (coinciden 1:1 con `post_id` duplicados → no hay ambigüedad, son registros repetidos, no post_id reciclados con datos distintos).
- 1 fecha no parseable en `created_at`: fila con user_id=U20001, post_id=P0099001 — posible registro corrupto/de prueba.
- 1 fecha no parseable en `post_created_at`: fila con user_id=U20007, post_id=P0099007 — posible registro corrupto/de prueba.
- 1.14% filas donde `post_created_at < created_at` (post antes del signup) — pendiente de confirmar % exacto y decidir tratamiento (quarantine).

## 2. Limpieza básica.

- Normalización de plan_type, device_type y post_category:

Correcciones de formato (typos, mayúsculas, espacios): Todos sin ambigüedades.

Decisión de criterio:
    
    plan_type:
        enterprise+ → enterprise porque no existe un tier superior definido en las reglas del challenge.
    
    device_type:
        phone y tablet son dos tipos distintos de mobiles por ende aunque no sean errores de tipeo entran en la categoria mobile.
    
Van a Quarantine:

    Vip, premium, console, politics y mistery. No hay forma de normalizar a ninguno.

### Recalculo de fechas.

Al menos el 50% de los casos con days_since_signup_calc negativo tienen days_since_signup original = 0, lo cual es consistente con la hipótesis de que el sistema aplicaba un clamping a 0 para evitar valores negativos. El otro 50% no sigue ese patrón, sugiriendo que puede haber múltiples causas de inconsistencia en la columna original, no una sola.

