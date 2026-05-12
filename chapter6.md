# Capítulo VI: Product Verification & Validation
## 6.1. Testing Suites & Validation
### 6.1.1. Core Entities Unit Tests.





#### Bounded Context: Analytics
Las 4 pruebas están en analytics-entity.spec.ts.
Estas pruebas son unitarias porque no levantan Vue, no usan Supabase real, no consultan red, no usan Pinia ni componentes visuales. Solo prueban directamente la lógica de AnalyticsAssembler, que es la clase encargada de transformar y calcular datos de la entidad analítica.

##### 1. Prueba: mapea una métrica cruda de Supabase a SensorData

 `it('mapea una metrica cruda de Supabase al modelo SensorData', () => {`

_Esta prueba valida el método:_

`AnalyticsAssembler.mapSensorData(...) `

Lo que se prueba es que un dato como viene desde Supabase, con nombres en snake_case, se convierta correctamente al modelo usado por la aplicación.
~~~ 
Entrada simulada:
{
  id: 10,
  device_id: 'device-01',
  temperature_c: 24.5,
  air_humidity_pct: 68,
  light_level: 720,
  soil_moisture_pct: 42,
  timestamp: '2026-05-11T10:00:00.000Z'
}

Resultado esperado:
{
  id: 10,
  device_id: 'device-01',
  temperature: 24.5,
  humidity: 68,
  light: 720,
  soil_humidity: 42,
  created_at: '2026-05-11T10:00:00.000Z'
}

~~~

##### Qué requisito valida:

- Que temperature_c se transforme a temperature.

- Que air_humidity_pct se transforme a humidity.

- Que soil_moisture_pct se transforme a soil_humidity.

- Que timestamp se use como created_at.

Que no se pierdan datos importantes del sensor.
Esta prueba evita errores internos en la conversión de datos entre backend y frontend.

##### 2. Prueba: calcula correctamente el resumen estadístico

~~~
it('calcula correctamente el resumen estadistico de las lecturas', () => {
~~~

_Esta prueba valida el método:_

~~~
AnalyticsAssembler.calculateSummary(...)
~~~

Se le pasan dos lecturas ya normalizadas:
temperature: 20 y 30
humidity: 60 y 80
light: 500 y 700
soil_humidity: 40 y 50
La función debe calcular promedios, mínimos, máximos y cantidad total de lecturas.

~~~
Resultado esperado:
{
  avgTemperature: 25,
  avgHumidity: 70,
  avgSoilMoisture: 45,
  avgLight: 600,
  minTemperature: 20,
  maxTemperature: 30,
  totalReadings: 2
}

~~~


##### Cálculos:

- Temperatura promedio: (20 + 30) / 2 = 25

- Humedad promedio: (60 + 80) / 2 = 70

- Humedad de suelo promedio: (40 + 50) / 2 = 45

- Luz promedio: (500 + 700) / 2 = 600

- Temperatura mínima: 20

- Temperatura máxima: 30

- Total de lecturas: 2

##### Qué requisito valida:

- Que la lógica estadística funciona correctamente.

- Que la entidad analítica genera valores consistentes.

- Que los cálculos internos no fallan con datos normales.

##### 3. Prueba: retorna valores por defecto cuando no hay lecturas

~~~
it('retorna valores por defecto cuando no hay lecturas', () => {
~~~

_Esta prueba también valida:_

~~~
AnalyticsAssembler.calculateSummary([])
~~~

Pero en este caso se le pasa un arreglo vacío.
Esto es importante porque en una app real puede pasar que una planta todavía no tenga sensores registrados, o que Supabase retorne una lista vacía.

~~~

Resultado esperado:
{
  avgTemperature: 0,
  avgHumidity: 0,
  avgSoilMoisture: 0,
  avgLight: 0,
  minTemperature: 0,
  maxTemperature: 0,
  totalReadings: 0
}
~~~

##### Qué requisito valida:

- Que la función no revienta si no hay datos.

- Que no produce NaN.

- Que no hace Math.min(...[]), lo cual podría generar resultados inválidos.

Que devuelve una estructura segura para mostrar en pantalla.
Esta prueba cubre un caso límite importante.

##### 4. Prueba: agrega analíticas por planta y filtra por dispositivo

~~~
it('agrega analiticas por planta y filtra por dispositivo', () => {
~~~

_Esta prueba valida el método:_

~~~
AnalyticsAssembler.aggregateByPlant(...)

~~~

##### Se pasan 3 métricas:

- Dos pertenecen a device-01.

- Una pertenece a device-02.

Luego se llama así:
AnalyticsAssembler.aggregateByPlant(data, 7, 'device-01')
Eso significa: “genera la analítica de la planta 7, pero solo usando datos del dispositivo device-01”.

La prueba espera:
~~~
expect(result.plantId).toBe(7);
expect(result.deviceId).toBe('device-01');
expect(result.sensorData).toHaveLength(2);
~~~

##### Esto confirma que:

- La analítica pertenece a la planta correcta.

- Se usa el dispositivo correcto.

- Se descartó la lectura de device-02.

Solo quedaron 2 lecturas válidas.
También valida el período:

~~~
expect(result.periodStart).toBe('2026-05-11T07:00:00.000Z');
expect(result.periodEnd).toBe('2026-05-11T09:00:00.000Z');
~~~

Y valida el resumen calculado:


~~~
{
  avgTemperature: 24,
  avgHumidity: 70,
  avgSoilMoisture: 50,
  avgLight: 400,
  totalReadings: 2
}

~~~

##### Cálculos usando solo device-01:

- Temperaturas: 22 y 26, promedio 24.

- Humedades: 65 y 75, promedio 70.

- Humedad suelo: 55 y 45, promedio 50.

- Luz: 300 y 500, promedio 400.

##### Qué requisito valida:

- Que la analítica se construye correctamente por planta.

- Que el filtro por dispositivo funciona.

- Que no se mezclan sensores distintos.

- Que el resumen final solo considera los datos correctos.

En conjunto, estas 4 pruebas validan la lógica interna principal de la entidad analítica: transformación de datos, cálculo estadístico, manejo de casos vacíos y agregación filtrada por planta/dispositivo.


#### Bounded Context: Plants

Las 4 pruebas están en `plant-entity.spec.ts`.

Todas son pruebas unitarias porque prueban funciones individuales de la entidad planta sin usar Supabase, componentes Vue, rutas, Pinia ni llamadas externas. Solo validan lógica interna.

---

### Preparación General

Antes de las pruebas hay una función auxiliar:

```ts
const createPlant = (overrides: Partial<Plant> = {}): Plant => ({
  ...
  ...overrides
});
```

Esta función crea una planta base con datos por defecto. Sirve para no repetir todo el objeto `Plant` en cada prueba.

Por ejemplo, la planta base tiene:

```ts
{
  id: 1,
  userId: 'user-1',
  name: 'Monstera',
  type: 'Tropical',
  status: 'healthy',
  metrics: [],
  wateringLogs: []
}
```

Luego cada prueba puede sobrescribir solo lo que necesita:

```ts
createPlant({
  metrics: [...]
});
```

También se usan timers falsos:

```ts
beforeEach(() => {
  vi.useFakeTimers();
  vi.setSystemTime(new Date('2026-05-11T12:00:00.000Z'));
});
```

Esto fija la fecha actual en:

```txt
2026-05-11 12:00:00
```

Es importante porque las funciones de riego usan `new Date()`.  
Si no se fija la fecha, la prueba podría dar resultados diferentes cada día.

Después de cada prueba se restauran los timers reales:

```ts
afterEach(() => {
  vi.useRealTimers();
});
```

---

##### 1. Prueba: planta saludable con métricas correctas

```ts
it('calcula estado healthy cuando las metricas estan dentro de rangos correctos', () => {
```

_Esta prueba valida la función:_

```ts
computePlantHealth(plant)
```

El objetivo es comprobar que, si una planta tiene todas sus métricas dentro de rangos adecuados, el sistema la clasifica como saludable.

La planta usada tiene estas métricas:

```ts
{
  soilMoisturePct: 55,
  temperatureC: 24,
  lightIntensityLux: 350,
  airHumidityPct: 60,
  battery: 80
}
```

Según la lógica de `computePlantHealth`, estos valores son correctos:

- Humedad del suelo 55: saludable porque está por encima de 45.
- Temperatura 24: saludable porque está entre 18 y 26.
- Luz 350: saludable porque está por encima de 200.
- Humedad ambiental 60: saludable porque está entre 40 y 70.
- Batería 80: saludable porque está por encima de 25.

Luego se ejecuta:

```ts
const result = computePlantHealth(plant);
```

Y se espera:

```ts
expect(result.status).toBe('healthy');
```

Esto valida que el estado general sea `healthy`.

También se espera:

```ts
expect(result.reason).toBe('all metrics optimal');
```

Eso significa que la función no encontró ningún problema en las métricas.

Finalmente:

```ts
expect(result.scores).toEqual({
  soil: 0,
  temp: 0,
  light: 0,
  airHumidity: 0,
  battery: 0
});
```

En esta lógica, los scores funcionan así:

- 0: correcto/saludable.
- 1: advertencia.
- 2: crítico.

Por eso todos deben ser 0.

##### Qué requisito valida:

- Que la lógica de salud reconoce correctamente una planta saludable.
- Que todas las métricas se evalúan correctamente.
- Que no se generen alertas falsas.
- Que los scores internos funcionen correctamente.

Esta prueba confirma que el sistema puede identificar una planta en buen estado sin producir errores de clasificación.

---

##### 2. Prueba: planta crítica por humedad baja del suelo

```ts
it('calcula estado critical cuando la humedad del suelo esta por debajo del minimo', () => {
```

_Esta prueba también valida:_

```ts
computePlantHealth(plant)
```

Pero ahora se prueba un caso negativo: una planta con humedad de suelo demasiado baja.

La métrica clave es:

```ts
soilMoisturePct: 18
```

En la función `computePlantHealth`, la humedad del suelo es crítica cuando está por debajo de 30.

Entonces 18 debe producir estado crítico.

Las demás métricas están en valores normales:

```ts
{
  temperatureC: 24,
  lightLevel: 350,
  airHumidityPct: 60,
  battery: 80
}
```

Luego se ejecuta:

```ts
const result = computePlantHealth(plant);
```

Y se valida:

```ts
expect(result.status).toBe('critical');
```

Esto confirma que el estado general de la planta es crítico.

También se valida:

```ts
expect(result.reason).toContain('soil critical');
```

Esto comprueba que la razón del problema menciona específicamente el suelo.

Finalmente:

```ts
expect(result.scores?.soil).toBe(2);
```

Esto confirma que el score interno del suelo es 2, que significa crítico.

##### Qué requisito valida:

- Que la función detecta condiciones críticas correctamente.
- Que el estado general cambia a `critical` cuando una métrica es peligrosa.
- Que el sistema identifica específicamente el origen del problema.
- Que los scores críticos se asignan correctamente.

Esta prueba es importante porque valida que el sistema puede detectar plantas en riesgo y generar alertas reales ante condiciones peligrosas.

---

##### 3. Prueba: riego inmediato cuando el suelo está crítico

```ts
it('programa riego inmediato cuando la humedad del suelo es critica', () => {
```

_Esta prueba valida la función:_

```ts
computeNextWatering(plant)
```

Esta función calcula cuándo debe regarse una planta.

La planta tiene:

```ts
soilMoisturePct: 20
```

Según la lógica de `computeNextWatering`, si la humedad del suelo es menor o igual a 20, la planta debe regarse inmediatamente.

Se ejecuta:

```ts
const result = computeNextWatering(plant);
```

Y se espera:

```ts
expect(result.urgency).toBe('now');
```

Esto significa que la urgencia del riego es inmediata.

También:

```ts
expect(result.daysUntilWatering).toBe(0);
```

Esto valida que no debe esperar ningún día.

Luego:

```ts
expect(result.nextWatering).toBe('2026-05-11T12:00:00.000Z');
```

Como la prueba fijó la fecha actual en `2026-05-11T12:00:00.000Z`, el próximo riego debe ser exactamente esa fecha.

Finalmente:

```ts
expect(result.reason).toBe('watering.reason.urgent');
```

Esto comprueba que la función devuelve la razón correspondiente al caso urgente.

##### Qué requisito valida:

- Que el sistema detecta condiciones críticas de riego.
- Que una planta seca se programe para riego inmediato.
- Que la lógica de urgencia funciona correctamente.
- Que el cálculo de fechas usa correctamente la fecha actual simulada.

Esta prueba valida que el sistema reacciona correctamente ante una condición crítica de humedad del suelo.

---

##### 4. Prueba: intervalo por defecto de 7 días cuando no hay métricas

```ts
it('usa el intervalo por defecto de siete dias cuando no existen metricas', () => {
```

_Esta prueba valida el comportamiento de respaldo de:_

```ts
computeNextWatering(plant)
```

En este caso, la planta no tiene métricas:

```ts
metrics: []
```

Pero sí tiene fecha de último riego:

```ts
lastWatered: '2026-05-08T12:00:00.000Z'
```

La lógica dice que, si no hay métricas de sensores, se usa un intervalo por defecto de 7 días.

Entonces:

```txt
Último riego: 2026-05-08 12:00
+ 7 días
Próximo riego: 2026-05-15 12:00
```

Por eso se espera:

```ts
expect(result.nextWatering).toBe('2026-05-15T12:00:00.000Z');
```

Como la fecha actual de la prueba es:

```txt
2026-05-11 12:00
```

Faltan 4 días para el próximo riego:

```txt
2026-05-15 - 2026-05-11 = 4 días
```

Por eso se valida:

```ts
expect(result.daysUntilWatering).toBe(4);
```

También se espera:

```ts
expect(result.urgency).toBe('normal');
```

Porque todavía no es urgente.

Y:

```ts
expect(result.reason).toBe('watering.reason.noMetrics');
```

Esto indica que la razón del cálculo fue que no había métricas disponibles.

##### Qué requisito valida:

- Que el sistema funciona incluso sin datos de sensores.
- Que existe un comportamiento de respaldo seguro.
- Que el cálculo de fechas sigue funcionando sin métricas.
- Que no se generan errores por listas vacías.

Esta prueba cubre un caso importante: cuando el sensor no tiene datos, la aplicación todavía debe funcionar y calcular un próximo riego razonable.

---
#### Bounded Context: Usuario

Las 4 pruebas están en:

```txt
tests/pruebasUnitarias/Ernesto/user-entity.spec.ts
```

Todas prueban directamente la clase `UserEntity`, definida en:

```txt
src/auth/domain/UserEntity.ts
```

Son pruebas unitarias porque no usan Supabase, no usan Vue, no usan Pinia, no usan rutas ni servicios externos. Solo validan la lógica interna de la entidad usuario.

---

##### 1. Prueba: crea un usuario con todos sus atributos principales

```ts
it('crea un usuario con todos sus atributos principales', () => {
```

Esta prueba valida que el constructor de `UserEntity` guarde correctamente todos los datos recibidos.

Primero crea una fecha:

```ts
const lastSignInAt = new Date('2026-05-11T10:30:00.000Z');
```

Luego crea un usuario:

```ts
const user = new UserEntity(
  'user-123',
  'ernesto@example.com',
  'authenticated',
  false,
  lastSignInAt
);
```

Los parámetros significan:

- `id`: identificador del usuario.
- `email`: correo del usuario.
- `role`: rol dentro del sistema.
- `isAnonymous`: indica si es usuario anónimo.
- `lastSignInAt`: fecha del último inicio de sesión.
- `requiresEmailConfirmation`: no se pasa, entonces debe tomar su valor por defecto: `false`.

Después se valida campo por campo:

```ts
expect(user.id).toBe('user-123');
expect(user.email).toBe('ernesto@example.com');
expect(user.role).toBe('authenticated');
expect(user.isAnonymous).toBe(false);
expect(user.lastSignInAt).toBe(lastSignInAt);
expect(user.requiresEmailConfirmation).toBe(false);
```

##### Qué requisito valida:

- Que el constructor conserve correctamente todos los datos recibidos.
- Que los atributos no cambien ni se alteren internamente.
- Que el valor por defecto de `requiresEmailConfirmation` funcione correctamente.
- Que la entidad usuario almacene información consistente.

Esta prueba asegura que la entidad no pierde información al construirse y mantiene correctamente el estado inicial del usuario.

---

##### 2. Prueba: identifica como autenticado a un usuario con id y no anónimo

```ts
it('identifica como autenticado a un usuario con id y no anonimo', () => {
```

_Esta prueba valida el método:_

```ts
isAuthenticatedUser()
```

Se crea un usuario normal:

```ts
const user = new UserEntity(
  'user-123',
  'ernesto@example.com',
  'authenticated',
  false,
  new Date('2026-05-11T10:30:00.000Z')
);
```

Lo importante aquí es:

```ts
id = 'user-123'
isAnonymous = false
```

La lógica de la entidad dice:

```ts
return !this.isAnonymous && !!this.id;
```

Eso significa que un usuario está autenticado solo si:

- No es anónimo.
- Tiene un id válido.

Por eso la prueba espera:

```ts
expect(user.isAuthenticatedUser()).toBe(true);
```

##### Qué requisito valida:

- Que un usuario real sea reconocido correctamente como autenticado.
- Que el sistema valide correctamente la combinación entre `id` y `isAnonymous`.
- Que la lógica de autenticación básica funcione correctamente.
- Que usuarios válidos puedan ser identificados correctamente dentro del sistema.

Esta prueba confirma que un usuario con identificador válido y no anónimo se reconoce correctamente como autenticado.

---

##### 3. Prueba: rechaza como autenticado a un usuario anónimo aunque tenga id

```ts
it('rechaza como autenticado a un usuario anonimo aunque tenga id', () => {
```

_Esta prueba también valida:_

```ts
isAuthenticatedUser()
```

Esta prueba valida el caso contrario.

Se crea un usuario así:

```ts
const user = new UserEntity(
  'anonymous-123',
  '',
  'anonymous',
  true,
  null
);
```

Datos importantes:

```ts
id = 'anonymous-123'
isAnonymous = true
```

Aunque el usuario tiene un id, está marcado como anónimo:

```ts
true
```

Entonces el método:

```ts
isAuthenticatedUser()
```

Debe devolver `false`.

La validación es:

```ts
expect(user.isAuthenticatedUser()).toBe(false);
```

##### Qué requisito valida:

- Que los usuarios anónimos no sean tratados como autenticados.
- Que el sistema no dependa únicamente de la existencia de un id.
- Que la validación lógica de autenticación sea segura.
- Que se eviten falsos positivos de autenticación.

Esta prueba es importante porque evita un error lógico de seguridad: no basta con tener un identificador; el usuario también debe no ser anónimo.

---

##### 4. Prueba: conserva el estado de confirmación de email pendiente

```ts
it('conserva el estado de confirmacion de email pendiente', () => {
```

Esta prueba valida el sexto parámetro del constructor:

```ts
requiresEmailConfirmation
```

Se crea un usuario así:

```ts
const user = new UserEntity(
  'user-pending',
  'pendiente@example.com',
  'authenticated',
  false,
  null,
  true
);
```

Datos importantes:

- `id`: `'user-pending'`
- `email`: `'pendiente@example.com'`
- `isAnonymous`: `false`
- `lastSignInAt`: `null`
- `requiresEmailConfirmation`: `true`

Luego se valida:

```ts
expect(user.requiresEmailConfirmation).toBe(true);
```

Esto confirma que la entidad conserva correctamente el estado de email pendiente.

También se valida:

```ts
expect(user.lastSignInAt).toBeNull();
```

Esto comprueba que la entidad permite usuarios sin último inicio de sesión registrado.

Finalmente:

```ts
expect(user.isAuthenticatedUser()).toBe(true);
```

Esto pasa porque, según la lógica actual de `UserEntity`, un usuario se considera autenticado si tiene id y no es anónimo. La confirmación pendiente de email se guarda como dato adicional, pero no afecta directamente el resultado de `isAuthenticatedUser()`.

##### Qué requisito valida:

- Que el estado de confirmación pendiente se almacene correctamente.
- Que la entidad permita usuarios sin historial de inicio de sesión.
- Que la autenticación no dependa directamente del estado de confirmación de email.
- Que el constructor soporte correctamente parámetros opcionales y estados especiales.

Esta prueba valida que la entidad puede representar correctamente usuarios pendientes de confirmación sin romper la lógica de autenticación existente.

---

#### Bounded Context: Métrica IoT

Las 4 pruebas están en:

```txt
tests/pruebasUnitarias/Juan/iot-metric-entity.spec.ts
```

Todas prueban la entidad Métrica IoT en aislamiento usando métodos de `AnalyticsAssembler`, que es la clase encargada de convertir y preparar métricas de sensores. No usan Supabase real, Vue, Pinia, componentes ni red.

---

##### 1. Prueba: normaliza una métrica IoT cruda del backend

```ts
it('normaliza una metrica IoT cruda del backend al modelo de planta', () => {
```

_Esta prueba valida el método:_

```ts
AnalyticsAssembler.mapRawToPlantMetric(...)
```

El objetivo es comprobar que una métrica como viene del backend, con nombres en `snake_case`, se transforme al formato que usa la aplicación internamente.

### Entrada:

```ts
{
  id: 21,
  device_id: 'iot-device-01',
  temperature_c: 23.7,
  air_humidity_pct: 61,
  light_level: 850,
  soil_moisture_pct: 47,
  timestamp: '2026-05-11T11:00:00.000Z'
}
```

### Resultado esperado:

```ts
{
  id: 21,
  deviceId: 'iot-device-01',
  temperatureC: 23.7,
  airHumidityPct: 61,
  lightLevel: 850,
  soilMoisturePct: 47,
  timestamp: '2026-05-11T11:00:00.000Z'
}
```

##### Lo que valida:

- `device_id` se convierte en `deviceId`.
- `temperature_c` se convierte en `temperatureC`.
- `air_humidity_pct` se convierte en `airHumidityPct`.
- `light_level` se convierte en `lightLevel`.
- `soil_moisture_pct` se convierte en `soilMoisturePct`.
- `timestamp` se conserva correctamente.

##### Qué requisito valida:

- Que la transformación de datos del backend funcione correctamente.
- Que los nombres de campos sean compatibles con la lógica interna de la aplicación.
- Que no se pierda información durante el mapeo.
- Que las métricas IoT puedan ser procesadas correctamente por las entidades de planta y analítica.

Esta prueba asegura que la métrica IoT pueda ser usada por la lógica de plantas y analíticas sin errores de nombres de campos.

---

##### 2. Prueba: asigna valores por defecto cuando la métrica llega incompleta

```ts
it('asigna valores por defecto cuando la metrica IoT llega incompleta', () => {
```

_Esta prueba también valida:_

```ts
AnalyticsAssembler.mapRawToPlantMetric({})
```

Aquí se simula que el backend devuelve una métrica vacía o incompleta.

### Entrada:

```ts
{}
```

### Resultado validado:

```ts
expect(result.id).toBe(0);
expect(result.deviceId).toBe('unknown');
expect(result.temperatureC).toBe(0);
expect(result.airHumidityPct).toBe(0);
expect(result.lightLevel).toBe(0);
expect(result.soilMoisturePct).toBe(0);
expect(result.timestamp).toEqual(expect.any(String));
```

##### Qué significa:

- Si no hay `id`, se usa `0`.
- Si no hay dispositivo, se usa `'unknown'`.
- Si no hay temperatura, humedad, luz o humedad de suelo, se usa `0`.
- Si no hay fecha, se genera un string de fecha actual.

##### Qué requisito valida:

- Que la función soporte datos incompletos sin fallar.
- Que no existan valores `undefined` en campos críticos.
- Que la aplicación pueda seguir funcionando aun con métricas inválidas o parciales.
- Que exista una estrategia segura de valores por defecto.

Esta prueba valida un caso límite importante: si llega información incompleta, la función no debe romperse ni devolver valores inválidos.

---

##### 3. Prueba: convierte datos de sensor al formato esperado por backend

```ts
it('convierte datos de sensor al formato esperado por el backend', () => {
```

_Esta prueba valida el método inverso:_

```ts
AnalyticsAssembler.toBackend(...)
```

Aquí la entrada está en formato de la entidad `SensorData`, que usa la aplicación:

```ts
{
  device_id: 'iot-device-01',
  temperature: 25.2,
  humidity: 64,
  light: 900,
  soil_humidity: 52,
  created_at: '2026-05-11T12:00:00.000Z'
}
```

### La salida esperada es:

```ts
{
  deviceId: 'iot-device-01',
  airTemperatureC: 25.2,
  airHumidityPct: 64,
  lightIntensityLux: 900,
  soilMoisturePct: 52,
  timestamp: '2026-05-11T12:00:00.000Z'
}
```

##### Lo que valida:

- `device_id` pasa a `deviceId`.
- `temperature` pasa a `airTemperatureC`.
- `humidity` pasa a `airHumidityPct`.
- `light` pasa a `lightIntensityLux`.
- `soil_humidity` pasa a `soilMoisturePct`.
- `created_at` pasa a `timestamp`.

##### Qué requisito valida:

- Que la aplicación pueda preparar correctamente datos para otras capas o servicios.
- Que la conversión hacia backend mantenga consistencia estructural.
- Que los nombres internos puedan adaptarse correctamente al formato externo.
- Que no se pierdan datos durante la transformación inversa.

Esta prueba asegura que los datos puedan prepararse correctamente para enviarse a otra capa o backend.

---

##### 4. Prueba: construye analíticas desde métricas IoT de una planta

```ts
it('construye analiticas desde metricas IoT de una planta', () => {
```

_Esta prueba valida:_

```ts
AnalyticsAssembler.fromPlantMetrics(5, [...])
```

Se pasan dos métricas IoT de una misma planta.

### Primera métrica:

```ts
{
  temperatureC: 20,
  airHumidityPct: 50,
  lightLevel: 400,
  soilMoisturePct: 35,
  timestamp: '2026-05-11T08:00:00.000Z'
}
```

### Segunda métrica:

```ts
{
  temperatureC: 30,
  airHumidityPct: 70,
  lightLevel: 800,
  soilMoisturePct: 55,
  timestamp: '2026-05-11T10:00:00.000Z'
}
```

La planta usada es:

```ts
plantId = 5
```

La prueba valida:

```ts
expect(result.plantId).toBe(5);
expect(result.deviceId).toBe('iot-device-01');
expect(result.periodStart).toBe('2026-05-11T08:00:00.000Z');
expect(result.periodEnd).toBe('2026-05-11T10:00:00.000Z');
expect(result.sensorData).toHaveLength(2);
```

##### Esto confirma que:

- La analítica pertenece a la planta 5.
- El dispositivo asociado es `iot-device-01`.
- El período inicia con la métrica más antigua.
- El período termina con la métrica más reciente.
- Se conservaron las dos lecturas.

También valida el resumen estadístico:

```ts
{
  avgTemperature: 25,
  avgHumidity: 60,
  avgSoilMoisture: 45,
  avgLight: 600,
  minTemperature: 20,
  maxTemperature: 30,
  totalReadings: 2
}
```

##### Cálculos:

- Temperatura promedio: `(20 + 30) / 2 = 25`
- Humedad promedio: `(50 + 70) / 2 = 60`
- Humedad de suelo promedio: `(35 + 55) / 2 = 45`
- Luz promedio: `(400 + 800) / 2 = 600`
- Temperatura mínima: `20`
- Temperatura máxima: `30`
- Total de lecturas: `2`

##### Qué requisito valida:

- Que las métricas IoT puedan convertirse correctamente en analíticas.
- Que el sistema agrupe correctamente lecturas de una planta.
- Que el período de análisis se calcule correctamente.
- Que los cálculos estadísticos produzcan resultados consistentes.

Esta prueba valida que las métricas IoT se puedan convertir en analíticas útiles para la planta.

---


### 6.1.2. Core Integration Tests.

#### Pruebas Integrales

Las 4 pruebas integrales están en:

```txt
tests/pruebasIntegrales/system-modules.integration.spec.ts
```

Son pruebas integrales porque no prueban una función aislada únicamente, sino la interacción entre varios módulos del sistema: adaptadores, servicios, stores, Supabase simulado y `localStorage`.

---

### Preparación General

Antes de las pruebas se crea un mock global de Supabase:

```ts
const { supabaseMock } = vi.hoisted(() => ({
  supabaseMock: {
    auth: {
      signInWithPassword: vi.fn(),
      getUser: vi.fn(),
      getSession: vi.fn(),
      signOut: vi.fn(),
      signUp: vi.fn()
    },
    from: vi.fn()
  }
}));
```

Luego se reemplaza el cliente real:

```ts
vi.mock('../../src/utils/supabase', () => ({
  supabase: supabaseMock
}));
```

Esto permite validar comunicación frontend/backend sin usar una base de datos real.

---

##### 1. Integración entre SupabaseAuthAdapter y Supabase Auth

```ts
it('integra SupabaseAuthAdapter con la API de autenticacion de Supabase', async () => {
```

Esta prueba valida la comunicación entre:

- `SupabaseAuthAdapter`
- API de autenticación de Supabase simulada
- `UserEntity`

Se importa el adaptador real:

```ts
const { SupabaseAuthAdapter } = await import('../../src/auth/adapters/SupabaseAuthAdapter');
const adapter = new SupabaseAuthAdapter();
```

Luego se simula una respuesta exitosa de Supabase:

```ts
supabaseMock.auth.signInWithPassword.mockResolvedValueOnce({
  data: {
    user: {
      id: 'user-1',
      email: 'user@example.com',
      role: 'authenticated',
      is_anonymous: false,
      last_sign_in_at: '2026-05-11T09:00:00.000Z'
    }
  },
  error: null
});
```

Después se ejecuta:

```ts
const result = await adapter.login('user@example.com', 'StrongPass1!');
```

La prueba verifica que el adaptador llamó correctamente a Supabase:

```ts
expect(supabaseMock.auth.signInWithPassword).toHaveBeenCalledWith({
  email: 'user@example.com',
  password: 'StrongPass1!'
});
```

Y que la respuesta fue convertida a una entidad del dominio:

```ts
expect(result).toBeInstanceOf(UserEntity);
expect(result.id).toBe('user-1');
expect(result.email).toBe('user@example.com');
expect(result.isAuthenticatedUser()).toBe(true);
```

##### Qué valida integralmente:

- El frontend envía credenciales correctamente.
- El adaptador usa la API correcta de Supabase.
- La respuesta del backend se transforma en `UserEntity`.
- La entidad resultante funciona correctamente con `isAuthenticatedUser()`.

Esta prueba asegura que el flujo de autenticación funcione correctamente entre frontend, adaptador y backend simulado.

---

##### 2. Integración entre PlantsService, WateringLogsService y Supabase

```ts
it('integra PlantsService con WateringLogsService y Supabase al registrar un riego', async () => {
```

Esta prueba valida el flujo completo de registrar un riego.

Intervienen:

- `PlantsService`
- `WateringLogsService`
- Tabla `plants`
- Tabla `watering_logs`
- Mapeo de datos backend a entidad `Plant`

Se crea el servicio real:

```ts
const { PlantsService } = await import('../../src/plants/infrastructure/plants.services');
const service = new PlantsService();
```

Luego se simulan tres operaciones de Supabase:

1. Actualizar `last_watered` en la tabla `plants`.
2. Insertar un registro en `watering_logs`.
3. Consultar nuevamente la planta actualizada.

El flujo que se prueba es:

```ts
const result = await service.waterPlant(
  10,
  'user-1',
  'Riego manual',
  '2026-05-11T12:00:00.000Z'
);
```

Esto representa:

- Planta: `10`
- Usuario: `user-1`
- Nota: `Riego manual`
- Fecha de riego: `2026-05-11T12:00:00.000Z`

La prueba valida que se haya llamado a la tabla correcta:

```ts
expect(supabaseMock.from).toHaveBeenCalledWith('plants');
```

Valida que se actualice la planta:

```ts
expect(update).toHaveBeenCalledWith({
  last_watered: '2026-05-11T12:00:00.000Z'
});
```

Valida que se filtre por id:

```ts
expect(updateEq).toHaveBeenCalledWith('id', 10);
```

Valida que se inserte el log de riego:

```ts
expect(insert).toHaveBeenCalledWith({
  plant_id: 10,
  user_id: 'user-1',
  watered_at: '2026-05-11T12:00:00.000Z',
  notes: 'Riego manual'
});
```

Finalmente valida que el resultado sea la planta actualizada:

```ts
expect(result.data.id).toBe(10);
expect(result.data.lastWatered).toBe('2026-05-11T12:00:00.000Z');
```

##### Qué valida integralmente:

- `PlantsService.waterPlant()` coordina múltiples operaciones correctamente.
- Se actualiza la tabla `plants`.
- Se inserta correctamente un registro en `watering_logs`.
- Se recupera la planta actualizada.
- Los datos backend en `snake_case` se convierten correctamente al formato frontend.

Esta prueba valida que el flujo completo de riego funcione correctamente entre servicios, tablas y entidades.

---

##### 3. Integración entre AnalyticsService y Supabase al importar métricas IoT

```ts
it('integra AnalyticsService con Supabase al importar metricas IoT', async () => {
```

Esta prueba valida la comunicación entre:

- `AnalyticsService`
- Tabla `plant_metrics`
- Datos IoT enviados desde frontend hacia backend

Se crea el servicio real:

```ts
const { AnalyticsService } = await import('../../src/analytics/infrastructure/analytics.service');
const service = new AnalyticsService();
```

Luego se simula Supabase:

```ts
supabaseMock.from.mockReturnValueOnce({ insert });
```

Se ejecuta:

```ts
const result = await service.importSensorData([
  {
    plantId: 5,
    deviceId: 'device-01',
    temperatureC: 24,
    airHumidityPct: 60,
    soilMoisturePct: 45,
    lightLevel: 700,
    timestamp: '2026-05-11T12:00:00.000Z'
  }
]);
```

La prueba valida que se use la tabla correcta:

```ts
expect(supabaseMock.from).toHaveBeenCalledWith('plant_metrics');
```

Luego valida que los datos se conviertan al formato esperado por Supabase:

```ts
expect(insert).toHaveBeenCalledWith([
  {
    plant_id: 5,
    timestamp: '2026-05-11T12:00:00.000Z',
    air_humidity_pct: 60,
    temperature_c: 24,
    soil_moisture_pct: 45,
    light_level: 700,
    device_id: 'device-01'
  }
]);
```

Esto comprueba la conversión:

- `plantId` → `plant_id`
- `deviceId` → `device_id`
- `temperatureC` → `temperature_c`
- `airHumidityPct` → `air_humidity_pct`
- `soilMoisturePct` → `soil_moisture_pct`
- `lightLevel` → `light_level`

Finalmente valida que el servicio retorna la respuesta del backend:

```ts
expect(result.data).toEqual([
  { id: 1, plant_id: 5, device_id: 'device-01' }
]);
```

##### Qué valida integralmente:

- El servicio recibe correctamente datos del frontend.
- Los datos se convierten correctamente al formato backend.
- Se usa la tabla correcta de Supabase.
- El servicio retorna correctamente la respuesta recibida.

Esta prueba asegura que el flujo de importación de métricas IoT funcione correctamente entre frontend, servicio y backend simulado.

---

##### 4. Integración entre AuthStore, servicio de autenticación, perfil y localStorage

```ts
it('integra AuthStore con servicio de autenticacion, perfil y localStorage durante el registro', async () => {
```

Esta prueba valida el flujo de registro desde el store.

Intervienen:

- `AuthStore`
- Servicio de autenticación simulado
- Servicio de perfil simulado
- `UserEntity`
- `localStorage`
- `Pinia`

Primero se crea un servicio de autenticación falso:

```ts
const authService = {
  login: vi.fn(),
  logout: vi.fn(),
  getCurrentUser: vi.fn(),
  signUp: vi.fn().mockResolvedValue(
    new UserEntity('user-2', 'new@example.com', 'authenticated', false, null)
  ),
  getSessionToken: vi.fn().mockResolvedValue('session-token-2')
};
```

Este servicio simula que el registro devuelve un usuario real.

Luego se crea un servicio de perfil:

```ts
const profileService = {
  createProfile: vi.fn().mockResolvedValue({})
};
```

Después se construye el store con esas dependencias:

```ts
const useAuthStore = setupAuthStore(authService, profileService);
const store = useAuthStore();
```

Se ejecuta el registro:

```ts
await store.signUp('new@example.com', 'StrongPass1!');
```

La prueba valida que el store llame al servicio de autenticación:

```ts
expect(authService.signUp).toHaveBeenCalledWith(
  'new@example.com',
  'StrongPass1!'
);
```

Valida que se cree el perfil:

```ts
expect(profileService.createProfile).toHaveBeenCalledWith('user-2');
```

Valida que se solicite el token de sesión:

```ts
expect(authService.getSessionToken).toHaveBeenCalled();
```

Valida que el estado del store se actualice:

```ts
expect(store.user?.email).toBe('new@example.com');
expect(store.token).toBe('session-token-2');
```

Y valida que se persista la sesión:

```ts
expect(localStorage.getItem('token')).toBe('session-token-2');
expect(localStorage.getItem('userUuid')).toBe('user-2');
```

##### Qué valida integralmente:

- El store coordina correctamente registro, perfil y sesión.
- El usuario se guarda correctamente en el estado global.
- El token se almacena correctamente en el store.
- La sesión se persiste correctamente en `localStorage`.
- El perfil se crea automáticamente después del registro.

Esta prueba valida el flujo completo de registro y persistencia de sesión dentro del sistema.

---

En conjunto, estas 4 pruebas aseguran que módulos distintos funcionen correctamente cuando interactúan entre sí: autenticación, plantas, riego, analíticas IoT, Supabase simulado, perfil, Pinia y almacenamiento local.

### 6.1.3. Core Behavior-Driven Development

#### Core Behavior-Driven Development (BDD)

En esta sección se aplican escenarios BDD (Behavior-Driven Development) para validar el comportamiento esperado del sistema desde la perspectiva del usuario.  

Los escenarios están escritos usando sintaxis Gherkin (`Given`, `When`, `Then`) y representan flujos reales del sistema relacionados con autenticación, monitoreo de plantas, riego y procesamiento de métricas IoT.

---

## Escenario 1: Inicio de sesión exitoso de usuario autenticado

```gherkin
Feature: Autenticación de usuarios

  Scenario: Usuario inicia sesión correctamente con credenciales válidas
    Given que existe un usuario registrado con email "user@example.com"
    And su contraseña es correcta
    When el usuario ingresa sus credenciales
    And presiona el botón de iniciar sesión
    Then el sistema debe autenticar al usuario
    And debe crear una sesión activa
    And debe almacenar el token de sesión
    And debe redirigir al dashboard principal
```

### Qué comportamiento valida:

- Que el sistema permita autenticación válida.
- Que se cree correctamente una sesión.
- Que el usuario autenticado pueda acceder al sistema.
- Que el token se persista correctamente.

---

## Escenario 2: Detección de planta en estado crítico

```gherkin
Feature: Monitoreo de salud de plantas

  Scenario: Planta entra en estado crítico por humedad baja del suelo
    Given una planta con humedad de suelo de 18%
    And temperatura ambiental de 24 grados
    And humedad ambiental de 60%
    When el sistema evalúa el estado de salud de la planta
    Then la planta debe clasificarse como "critical"
    And el sistema debe indicar "soil critical"
    And el score de humedad de suelo debe ser crítico
```

### Qué comportamiento valida:

- Que el sistema detecte condiciones peligrosas.
- Que la lógica de salud clasifique correctamente estados críticos.
- Que se identifique específicamente el problema del suelo.
- Que se generen alertas correctas para el usuario.

---

## Escenario 3: Registro manual de riego de una planta

```gherkin
Feature: Gestión de riego

  Scenario: Usuario registra un riego manual exitosamente
    Given una planta registrada con id 10
    And un usuario autenticado con id "user-1"
    When el usuario registra un riego manual
    And agrega la nota "Riego manual"
    Then el sistema debe actualizar la fecha de último riego
    And debe registrar un nuevo evento en el historial de riego
    And debe devolver la planta actualizada
```

### Qué comportamiento valida:

- Que el sistema permita registrar riegos manuales.
- Que se actualice correctamente la planta.
- Que el historial de riego conserve trazabilidad.
- Que múltiples módulos trabajen correctamente en conjunto.

---

## Escenario 4: Importación de métricas IoT desde sensores

```gherkin
Feature: Importación de métricas IoT

  Scenario: Sistema importa métricas IoT enviadas por sensores
    Given un dispositivo IoT con id "device-01"
    And métricas de temperatura, humedad, luz y suelo válidas
    When el sistema recibe las métricas del sensor
    Then debe almacenar los datos en la tabla de métricas
    And debe convertir los datos al formato esperado por backend
    And debe asociar las métricas a la planta correspondiente
    And debe permitir generar analíticas posteriores
```

### Qué comportamiento valida:

- Que el sistema procese correctamente métricas IoT.
- Que exista integración correcta entre frontend y backend.
- Que las métricas se almacenen correctamente.
- Que los datos puedan utilizarse posteriormente en analíticas y monitoreo.

---

En conjunto, estos escenarios BDD validan comportamientos centrales del sistema desde la perspectiva del usuario: autenticación, monitoreo de salud de plantas, gestión de riego e integración de sensores IoT.

### 6.1.4. Core System Tests. 

Son pruebas de de sistema para validar que la aplicación
funciona correctamente en su totalidad, tanto en entorno web como móvil. Estas
pruebas deben cubrir funcionalidades completas, como la navegación, la interacción con
APIs y la respuesta del sistema en diferentes escenarios

_para ello se utliza selenium_

##### Crear cuenta 
<a href="https://ibb.co/BH10yw2G"><img src="https://i.ibb.co/DPq09LfY/S3.png" alt="S3" border="0"></a>

##### Crear Planta
<a href="https://ibb.co/NgrVzXjv"><img src="https://i.ibb.co/ympfJDy9/S1.png" alt="S1" border="0"></a>

##### Update Profile
<a href="https://ibb.co/DPVtwzRk"><img src="https://i.ibb.co/6cnmDrWb/S2.png" alt="S2" border="0"></a>


En flujo es bueno cumple con todos los requirimientos nesesarios por ello la prueba de performance es la siguiente:


<a href="https://ibb.co/m50Qtsxf"><img src="https://i.ibb.co/qYCb9GZT/Captura-de-pantalla-2026-05-12-113234.png" alt="Captura de pantalla 2026 05 12 113234" border="0"></a>

