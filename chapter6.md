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


#### !importante en el documento esta la documentacion de las prueba unitarias y integrales si deseas ver el codigo de las pruebas estan en la carpetas test en el repositorio de la web para fines academicos puedes clonarlo y verlo por tu mismo con el comando "nmp test" ejecutas todas las pruebas


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

## 6.2. Static Testing & Verification

La presente sección describe las actividades de verificación estática aplicadas durante el desarrollo del sistema, con el propósito de garantizar altos estándares de calidad, mantenibilidad, seguridad y consistencia del código fuente antes de su ejecución. La verificación estática permite identificar defectos de diseño, incumplimientos de estándares, vulnerabilidades de seguridad y oportunidades de mejora en etapas tempranas del ciclo de vida del software, reduciendo significativamente el costo asociado a su corrección.

Las actividades de análisis estático y revisión de código forman parte de la estrategia de aseguramiento de calidad del proyecto y se integran dentro del flujo de desarrollo continuo, permitiendo detectar incidencias antes de que estas sean desplegadas a entornos de integración o producción.

### Objetivos de la Verificación Estática

| Objetivo              | Descripción                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------------------ |
| Calidad del código    | Garantizar que el código cumpla con estándares de calidad definidos por el equipo de desarrollo. |
| Mantenibilidad        | Facilitar la comprensión, modificación y evolución futura del sistema.                           |
| Seguridad             | Detectar vulnerabilidades y riesgos de seguridad durante la implementación.                      |
| Consistencia          | Mantener uniformidad en la estructura, nomenclatura y arquitectura del software.                 |
| Reducción de defectos | Identificar errores tempranamente para disminuir retrabajos y costos de mantenimiento.           |

---

### 6.2.1. Static Code Analysis

El análisis estático de código consiste en la evaluación sistemática del código fuente sin necesidad de ejecutar la aplicación. Esta práctica permite detectar defectos sintácticos, problemas de diseño, duplicación de código, incumplimientos de estándares de desarrollo y posibles vulnerabilidades de seguridad.

Para la ejecución de esta actividad se emplean herramientas automatizadas y revisiones manuales que proporcionan retroalimentación continua a los desarrolladores durante todo el proceso de construcción del software.

Los resultados obtenidos del análisis estático son considerados criterios de calidad obligatorios antes de aprobar cualquier integración de código dentro del repositorio principal.

#### 6.2.1.1. Coding Standard & Code Conventions

Con el objetivo de asegurar la uniformidad y legibilidad del código fuente, el equipo de desarrollo adopta estándares y convenciones de programación basados en buenas prácticas de ingeniería de software.

##### Principios de Clean Code

Se aplicarán los principios propuestos por Robert C. Martin (Clean Code), priorizando:

* Uso de nombres descriptivos y significativos para variables, métodos, clases y módulos.
* Métodos pequeños con una única responsabilidad.
* Eliminación de código duplicado.
* Reducción de dependencias innecesarias.
* Eliminación de código muerto o no utilizado.
* Comentarios únicamente cuando aporten valor al entendimiento del código.
* Estructuración clara de responsabilidades mediante separación de capas.

##### Aplicación de Domain-Driven Design (DDD)

La arquitectura del sistema sigue los principios de Domain-Driven Design para garantizar una representación adecuada del dominio del negocio.

Los principales lineamientos son:

* Uso de un lenguaje ubicuo compartido entre desarrolladores y expertos del dominio.
* Definición explícita de Bounded Contexts.
* Implementación de Entidades (Entities) y Objetos de Valor (Value Objects).
* Encapsulamiento de reglas de negocio dentro del dominio.
* Uso de Repositorios para la persistencia de entidades.
* Implementación de Servicios de Dominio cuando la lógica no pertenezca directamente a una entidad.
* Minimización del acoplamiento entre contextos mediante contratos bien definidos.

##### Convenciones de Código

| Elemento   | Convención                           |
| ---------- | ------------------------------------ |
| Clases     | PascalCase                           |
| Interfaces | Prefijo "I" + PascalCase             |
| Métodos    | camelCase                            |
| Variables  | camelCase                            |
| Constantes | UPPER_SNAKE_CASE                     |
| Archivos   | Nombre representativo del componente |
| Paquetes   | Minúsculas y segmentados por dominio |

##### Principios Arquitectónicos

El desarrollo del sistema seguirá además los siguientes principios:

* Single Responsibility Principle (SRP).
* Open/Closed Principle (OCP).
* Dependency Inversion Principle (DIP).
* Separation of Concerns (SoC).
* Low Coupling y High Cohesion.
* Arquitectura orientada al dominio.

---

#### 6.2.1.2. Code Quality & Code Security

La calidad y seguridad del código constituyen factores críticos para garantizar la confiabilidad, disponibilidad y mantenibilidad del sistema.

##### Calidad del Código

La calidad será monitoreada mediante indicadores cuantificables que permitan evaluar continuamente el estado del producto.

| Métrica                   | Objetivo        |
| ------------------------- | --------------- |
| Cobertura de pruebas      | ≥ 80%           |
| Duplicación de código     | ≤ 5%            |
| Complejidad ciclomática   | ≤ 15 por método |
| Bugs críticos             | 0               |
| Code Smells críticos      | 0               |
| Vulnerabilidades críticas | 0               |

Para el monitoreo de estas métricas se utilizarán herramientas de análisis estático que permitan generar reportes periódicos y trazabilidad de los hallazgos encontrados.

##### Seguridad del Código

Se implementarán prácticas de desarrollo seguro orientadas a prevenir vulnerabilidades comunes identificadas por OWASP.

Entre los principales controles se encuentran:

* Validación y sanitización de entradas del usuario.
* Protección contra inyección SQL.
* Protección contra Cross-Site Scripting (XSS).
* Gestión segura de credenciales y secretos.
* Manejo adecuado de autenticación y autorización.
* Validación de permisos de acceso.
* Uso de conexiones seguras mediante HTTPS.
* Protección contra exposición de información sensible.
* Gestión adecuada de excepciones y registros.

##### Herramientas de Análisis Estático

Para fortalecer el proceso de aseguramiento de calidad se emplearán las siguientes herramientas:

| Herramienta          | Propósito                                         |
| -------------------- | ------------------------------------------------- |
| SonarLint            | Análisis en tiempo real dentro del IDE.           |
| SonarQube            | Monitoreo continuo de calidad y seguridad.        |
| ESLint               | Validación de estándares JavaScript/TypeScript.   |
| Prettier             | Estandarización automática del formato de código. |
| GitHub Code Scanning | Detección automatizada de vulnerabilidades.       |

SonarLint será utilizado durante la etapa de desarrollo para proporcionar retroalimentación inmediata al programador, mientras que SonarQube se integrará dentro del pipeline de integración continua para validar automáticamente los criterios de calidad establecidos por el proyecto.

---

### 6.2.2. Reviews

Las revisiones de código constituyen una actividad obligatoria dentro del proceso de aseguramiento de calidad. Su finalidad es verificar que los cambios implementados cumplan con los estándares técnicos, arquitectónicos y de negocio definidos para el proyecto.

Las revisiones permiten detectar defectos tempranamente, compartir conocimiento entre los miembros del equipo y garantizar la sostenibilidad del sistema a largo plazo.

#### Tipos de Revisiones

##### Revisión por Pares (Peer Review)

Consiste en la evaluación del código por parte de otro desarrollador del equipo antes de su integración.

Objetivos:

* Detectar errores de implementación.
* Verificar el cumplimiento de estándares.
* Compartir conocimiento técnico.
* Mejorar la mantenibilidad del código.

##### Revisión Formal

Proceso estructurado realizado mediante reuniones de inspección donde se utiliza una lista de verificación previamente definida.

Objetivos:

* Detectar defectos complejos.
* Revisar decisiones arquitectónicas.
* Validar requisitos críticos.

##### Revisión Automática

Se realiza mediante herramientas especializadas que verifican aspectos relacionados con:

* Calidad del código.
* Seguridad.
* Complejidad.
* Duplicación.
* Cobertura de pruebas.

---

#### Proceso de Revisión

##### 1. Creación del Pull Request (PR)

Todo cambio deberá ser enviado mediante un Pull Request que incluya:

* Descripción de la funcionalidad implementada.
* Relación con historias de usuario o requisitos.
* Evidencias de pruebas realizadas.
* Capturas de pantalla cuando corresponda.
* Resultados del análisis estático.

##### 2. Ejecución de Validaciones Automáticas

Antes de iniciar la revisión manual se deberá verificar:

* Compilación exitosa.
* Ejecución satisfactoria de pruebas.
* Cumplimiento de métricas de calidad.
* Ausencia de vulnerabilidades críticas.

##### 3. Revisión Técnica

Los revisores evaluarán:

* Correctitud funcional.
* Diseño de la solución.
* Legibilidad del código.
* Cumplimiento de estándares.
* Impacto arquitectónico.
* Seguridad.

##### 4. Retroalimentación

Los comentarios deberán:

* Ser objetivos y verificables.
* Incluir propuestas de mejora.
* Estar sustentados técnicamente.
* Mantener un enfoque constructivo.

##### 5. Aprobación y Fusión

El Pull Request podrá fusionarse únicamente cuando:

* Todas las observaciones hayan sido resueltas.
* Se obtenga la aprobación requerida.
* Las validaciones automáticas sean satisfactorias.

---

#### Checklist de Revisión

| Criterio                          | Verificación |
| --------------------------------- | ------------ |
| Cumple estándares de codificación | □            |
| Sigue principios DDD              | □            |
| Sigue principios SOLID            | □            |
| No presenta código duplicado      | □            |
| Incluye pruebas automatizadas     | □            |
| Cobertura mínima alcanzada        | □            |
| No introduce vulnerabilidades     | □            |
| Manejo adecuado de errores        | □            |
| Documentación actualizada         | □            |
| Cumple requerimientos funcionales | □            |

---

#### Criterios de Aceptación

Un Pull Request será aceptado únicamente si cumple con los siguientes criterios:

* Cobertura mínima de pruebas igual o superior al 80%.
* Ausencia de vulnerabilidades críticas o altas.
* Ausencia de errores bloqueantes.
* Cumplimiento de estándares de codificación.
* Cumplimiento de principios arquitectónicos definidos.
* Validación satisfactoria por las herramientas de análisis estático.
* Aprobación de al menos un revisor distinto al autor del cambio.

---

#### Frecuencia de Revisiones

Las revisiones serán realizadas de manera continua durante el desarrollo del proyecto.

| Actividad                | Frecuencia                   |
| ------------------------ | ---------------------------- |
| Análisis SonarLint       | En cada desarrollo           |
| Revisión de Pull Request | Antes de cada merge          |
| Análisis SonarQube       | En cada integración continua |
| Revisión Formal          | Al cierre de cada sprint     |
| Auditoría de Calidad     | Al finalizar cada release    |

La aplicación sistemática de estas prácticas de análisis estático y revisión de código permitirá mantener altos niveles de calidad, seguridad y mantenibilidad, contribuyendo a la construcción de un producto de software robusto y alineado con las mejores prácticas de ingeniería de software.

## 6.3. Validation Interviews.

### 6.3.1. Diseño de Entrevistas.

**1. Introducción de la Entrevista**

Objetivo:
Validar la propuesta de valor, comprensión, navegabilidad y experiencia de usuario del sistema PlantCare en sus tres componentes: Landing Page, Aplicación Móvil y Aplicación Web.

Script para el entrevistador:

“Queremos validar una solución basada en IoT para el monitoreo y cuidado de plantas.”

“No estamos evaluando su desempeño, sino la claridad y facilidad de uso del producto.”

“Le pedimos que navegue libremente y piense en voz alta mientras interactúa con la plataforma.”

“Cualquier duda, comentario o sugerencia es bienvenida.”

**2. Preguntas Generales sobre el Usuario**

¿Tiene plantas en casa o en su entorno cercano?

¿Cuántas plantas tiene aproximadamente?

¿Ha utilizado previamente alguna aplicación relacionada al cuidado de plantas?

¿Qué tan familiarizado está con sistemas o dispositivos IoT?

¿Qué problemas enfrenta actualmente para cuidar adecuadamente sus plantas?

¿Qué tan cómodo se siente utilizando aplicaciones móviles y aplicaciones web?

**3. Validación de la Landing Page**
**3.1 Preguntas antes de usar la Landing Page**

¿Qué cree que ofrece el producto con base en la primera impresión?

¿Qué elementos llaman más su atención al ingresar?

¿Percibe el diseño como confiable y profesional?

**3.2 Tareas (User Flows – Landing Page)**

“Identifique qué problema soluciona el sistema.”

“Revise la sección donde se explica cómo funciona la solución.”

“Acceda a la aplicación web o a la aplicación móvil mediante los botones o enlaces disponibles.”

**3.3 Preguntas después de usar la Landing Page**

¿El mensaje principal del producto le resultó claro?

¿Pudo comprender adecuadamente el funcionamiento del sistema IoT?

¿La navegación le resultó intuitiva?

¿Hubo alguna parte que le generó confusión o que modificaría?

¿La Landing Page le motivaría a usar la aplicación?

**4. Validación de la Aplicación Móvil**
**4.1 Preguntas antes de usar la Aplicación Móvil**

¿Qué espera encontrar en una aplicación destinada al cuidado de plantas?

¿Qué información considera más importante monitorear?

**4.2 Tareas (User Flows – Aplicación Móvil)**

Flujo 1 – Visualización del estado general (Dashboard)

“Acceda al Dashboard e indique qué información comprende.”

Flujo 2 – Visualizar detalles de una planta

“Entre al detalle de una planta e indique qué información encuentra y cómo la interpreta.”

Flujo 3 – Acceder a Configuración

“Ingrese a la sección de Configuración e intente modificar algún ajuste disponible.”

**4.3 Preguntas después de usar la Aplicación Móvil**

¿Qué tan fácil fue encontrar la información o funciones que buscaba?

¿La información del Dashboard le resultó clara y útil?

¿Qué parte de la aplicación considera más útil y cuál menos útil?

¿Experimentó confusión en algún punto del proceso?

¿Cómo percibe el diseño y la organización visual de la aplicación?

¿Agregarías o eliminaría alguna funcionalidad?

**5. Validación de la Aplicación Web**
**5.1 Preguntas antes de usar la Aplicación Web**

¿Qué funcionalidades esperaría encontrar en una plataforma web para el monitoreo de plantas?

¿Qué información o herramientas considera más importantes desde la versión de escritorio?

**5.2 Tareas (User Flows – Aplicación Web)**

Flujo 1 – Inicio de sesión

“Ingrese a la plataforma utilizando el formulario de acceso.”

Flujo 2 – Visualización del estado general de las plantas

“Revise el Dashboard general e indique qué información comprende.”

Flujo 3 – Agregar o editar un dispositivo IoT

“Agregue un nuevo dispositivo IoT o edite uno existente, si es posible.”

Flujo 4 – Visualizar reportes o datos históricos

“Busque información histórica, métricas o reportes relevantes.”

**5.3 Preguntas después de usar la Aplicación Web**

¿La navegación en la plataforma le pareció intuitiva?

¿Los títulos y nombres de las secciones son claros y comprensibles?

¿La distribución de la información es adecuada?

¿Hubo alguna parte que le generó confusión?

¿Percibe coherencia visual y funcional respecto a la versión móvil?

**6. Cierre de la Entrevista**

¿Qué valoración general (en una escala del 1 al 10) asignaría a las plataformas?

¿Utilizaría el sistema PlantCare si estuviera disponible actualmente?

¿Qué aspecto considera más convincente del producto?

¿Qué aspecto considera menos convincente?

¿Tiene algún comentario adicional o sugerencia?

### 6.3.2. Registro de Entrevistas.

#### Segmento objetivo 1: Personas ocupadas (departamentos/oficinas)
[![Image-validation-interview.png](https://i.postimg.cc/Xq9DZ7Ws/Captura-de-pantalla-2025-11-15-110522.png)](https://postimg.cc/sGDJqzvW)

**Entrevistado:** Carlos <br>
**Sexo:** Masculino <br>
**Edad:** 21 años<br>
**Domicilio:** San Martin de Porres <br>
**Inicio de la Entrevista:** 00:00 <br>
**Duración de la Entrevista:** 09:55   <br>

**Resumen de la Entrevista:** <br>
Carlos comprendió claramente la propuesta de PlantCare sobre el cuidado automatizado de plantas mediante IoT y una aplicación. Consideró que la Landing Page es atractiva y motivadora, y confirmó que utilizaría el producto, valorándolo con un 9 sobre 10.
En cuanto a la usabilidad, aunque la navegación y los títulos son intuitivos y el Dashboard es claro y relevante (humedad, temperatura), se hizo una observación importante sobre la velocidad, indicando que el sistema se percibe como lento. Adicionalmente, se sugiere aclarar la interpretación de los datos de los sensores para novatos, simplificar la configuración inicial y permitir mayor personalización (frecuencia de notificaciones, más tipos de plantas) sin perder la simplicidad.

[![Image-validation-interview.png](https://i.postimg.cc/TwsXjPZ1/image.png)](https://postimg.cc/TLnSfT2v)

**Entrevistado:** Facundo <br>
**Sexo:** Masculino <br>
**Edad:** 23 años<br>
**Domicilio:** Los Olivos <br>
**Inicio de la Entrevista:** 09:55 <br>
**Duración de la Entrevista:** 17:56   <br>

**Resumen de la Entrevista:** <br>
Facundo evaluó la aplicación con un notable 8.5 sobre 10, ya que comprendió muy bien su propósito y la encontró tanto útil como práctica. Resaltó que la función más valiosa para él fue el sistema de alertas de agua.

Durante su análisis de la usabilidad, Facundo fue claro en que el diseño de la interfaz y la presentación de los indicadores (luz, humedad, etc.) le parecían correctos y fáciles de entender. Sin embargo, identificó un único problema significativo: la velocidad. Su observación central fue que la aplicación se sentía lenta en su funcionamiento, siendo este el único punto de fricción que experimentó.

#### Segmento objetivo 2: Aficionados que cuidan varias macetas en casa

[![Image-validation-interview.png](https://i.postimg.cc/wMCGcrFG/Image-validation-interview.png)](https://postimg.cc/GTjKbqWF)

**Entrevistado:** Ana <br>
**Sexo:** Femenino <br>
**Edad:** 57 años<br>
**Domicilio:** Puente Piedra <br>
**Inicio de la Entrevista:** 17:56 <br>
**Duración de la Entrevista:** 25:02   <br>

**Resumen de la Entrevista:** <br>
En general, Ana encontró el mensaje del producto claro: la idea de cuidar plantas de forma automatizada con IoT y la app se entiende bien. La Landing Page resulta atractiva y motivadora, afirmó que sí usaría PlantCare (valoración general: 8/10), destacando la automatización y las recomendaciones personalizadas como lo más convincente.

En cuanto a usabilidad, la navegación y los títulos son intuitivos y la información está bien distribuida; el Dashboard muestra datos relevantes (humedad, temperatura) y es fácil de leer. La función más valorada son las notificaciones de cuidado; en cambio, la sección de análisis avanzado se percibe como menos útil para ella.

Como observaciones, se sugiere aclarar un poco más la interpretación de los datos de los sensores para usuarios novatos; también convendría simplificar la configuración inicial. Finalmente, propone mayor personalización (frecuencia de notificaciones, más tipos de plantas) para aumentar la utilidad sin perder la simplicidad.

[![Captura-de-pantalla-2025-11-14-235934.png](https://i.postimg.cc/Z5PVzKFP/Captura-de-pantalla-2025-11-14-235934.png)](https://postimg.cc/LJXzzSWh)
**Entrevistado:** Natalia Ramirez <br>
**Sexo:** Femenino <br>
**Edad:** 61 años<br>
**Domicilio:** San Martin de Porres <br>
**Inicio de la Entrevista:** 25:02 <br>
**Duración de la Entrevista:** 30:46   <br>

**Resumen de la Entrevista:** <br>
En general, el usuario entendió bien la propuesta de la aplicación: la pantalla principal comunica claramente el estado de la planta y los datos esenciales como temperatura, humedad y luz. Consideró que la app es útil y práctica para quienes suelen olvidar el riego, valorando especialmente el calendario que muestra la última y la próxima fecha estimada (valoración general: 8/10). También destacó que recibir alertas cuando la planta necesita agua sería una de las funciones más importantes.

Sobre la usabilidad, el usuario mencionó que la interfaz es clara, pero algo cargada: las tarjetas son grandes y la información aparece un poco dispersa. Aunque la temperatura es fácil de interpretar, indicadores como el nivel de luz (por ejemplo, 750 lm) no resultan evidentes para usuarios sin experiencia. La descripción de la planta le pareció útil, pero demasiado larga para leerla completa de inmediato.

Entre las observaciones, sugirió agrupar los indicadores ambientales en un solo bloque para hacer la pantalla más compacta y rápida de leer. También señaló que el botón de eliminar planta está muy expuesto y puede generar errores, recomendando añadir una confirmación adicional. Finalmente, comentó que sería útil entender mejor cómo se calcula la próxima fecha de riego y ofrecer una versión corta del texto descriptivo para mejorar la experiencia sin perder información.

[![image.png](https://i.postimg.cc/3N0JkwKW/image.png)](https://postimg.cc/9wVjSCwj)

**Entrevistado:** Derek <br>
**Sexo:** Masculino <br>
**Edad:** 21 años<br>
**Domicilio:** San Miguel <br>
**Inicio de la Entrevista:** 30:46 <br>
**Duración de la Entrevista:** 37:49   <br>

**Resumen de la Entrevista:** <br>
El entrevistado, Derek, tiene 21 años y vive en San Miguel. Cuenta con varias plantas en casa y, aunque disfruta cuidarlas, suele organizar el riego únicamente por memoria, lo que a veces genera problemas por olvido o desconocimiento de cuánta agua o luz requiere cada especie. Tiene conocimientos básicos sobre IoT y se siente cómodo utilizando tecnología en su día a día. Usa una computadora personal, el navegador Brave y un smartphone Samsung S20 FE, lo que demuestra familiaridad con plataformas digitales y disposición a adoptar soluciones tecnológicas siempre que sean claras, accesibles y visualmente agradables.

La Landing Page le pareció clara, profesional e intuitiva. Comprendió rápidamente que PlantCare es un sistema IoT orientado al monitoreo de plantas y destacó que la estructura facilita entender el funcionamiento general. Como sugerencia, indicó la necesidad de detallar mejor el proceso de instalación y conexión de los sensores para evitar dudas iniciales. La página le motivaría a usar la aplicación.

En la aplicación móvil encontró la información organizada y fácil de localizar. Valoró el monitoreo en tiempo real y el diseño moderno de la interfaz. Consideró menos útil la sección de recomendaciones porque, según él, podría personalizarse más. Propuso añadir un historial de riego y crecimiento para visualizar la evolución de cada planta.

En la aplicación web destacó la navegación clara, los títulos comprensibles y la buena distribución de la información. No encontró elementos confusos y percibió coherencia visual y funcional con la versión móvil. Valoró la manera en que los gráficos y el estado de las plantas se presentan en una vista más amplia.

[![image.png](https://i.postimg.cc/hvNvsdyW/image.png)](https://postimg.cc/ctMd4rGX)

**Entrevistado:** Vasco <br>
**Sexo:** Masculino <br>
**Edad:** 24 años<br>
**Domicilio:** San Miguel <br>
**Inicio de la Entrevista:** 37:49 <br>
**Duración de la Entrevista:** 45:33   <br>

**Resumen de la Entrevista:** <br>
El entrevistado, Vasco, tiene 24 años y cuida aproximadamente 15 plantas en su departamento, principalmente suculentas y helechos. Aunque se interesa por el ambiente y disfruta el cuidado de sus plantas, suele organizar el riego de manera intuitiva y sin herramientas, lo que en ocasiones ha provocado problemas por exceso o falta de agua. Se siente cómodo usando tecnología y emplea con frecuencia su laptop con Windows, su smartphone Redmi 13C y el navegador Opera GX. Le interesan herramientas digitales que ofrezcan información clara, gráficos simples e historial del estado de sus plantas, además de recordatorios y recomendaciones basadas en el clima.

La Landing Page le pareció clara, profesional y motivadora. Comprendió sin dificultad la propuesta del sistema IoT de PlantCare, destacando la buena organización del contenido. Su única sugerencia fue mejorar la explicación sobre cómo se conectan e instalan los sensores, especialmente para usuarios con menor experiencia técnica.

En la aplicación móvil encontró la información bien estructurada y fácil de ubicar. Consideró particularmente útiles el Dashboard y las alertas de riego, ya que responden directamente a sus principales necesidades. La configuración inicial de los sensores fue el único punto que le generó confusión. Valoró el diseño limpio de la app y sugirió incorporar consejos simples que se adapten al clima o condiciones del entorno.

En la versión web destacó la navegación intuitiva, la claridad de los títulos y la organización de los datos. Apreció especialmente los gráficos de humedad y riego, así como la posibilidad de visualizar varias plantas en una pantalla más amplia. Señaló que la configuración avanzada resultó un poco técnica, pero percibió coherencia visual y funcional respecto a la aplicación móvil.

<div style="page-break-after: always;"></div>


### 6.3.3. Evaluaciones según heurísticas.

Esta sección contiene el proceso de evaluación realizado durante las sesiones de validación de usabilidad, con base en heurísticas de Usabilidad (Nielsen), Arquitectura de Información e Inclusive Design de la experiencia propuesta. El análisis identifica los problemas encontrados, la heurística violada, su impacto y recomendaciones, aplicando la Escala de Severidad correspondiente según el Anexo D: Formato para Evaluación UX según Heurísticas.

### Escala de Severidad

| Nivel | Descripción |
|-------|-------------|
| **1** | Problema superficial: puede superarse fácilmente y ocurre con baja frecuencia. No requiere acción inmediata. |
| **2** | Problema menor: ocurre con más frecuencia, afecta usabilidad general. Prioridad baja para próxima versión. |
| **3** | Problema mayor: impacta de manera significativa en la experiencia. Debe ser resuelto con prioridad alta. |
| **4** | Problema crítico: impide continuar con el uso del sistema. Debe corregirse antes del lanzamiento. |

---

### Tabla Resumen de Problemas Detectados

| # | Problema identificado | Escala | Heurística / Principio violado |
|---|-----------------------|--------|--------------------------------|
| 1 | La configuración inicial del sensor IoT resulta confusa para usuarios nuevos. | **3** | Usabilidad – Ayuda y documentación |
| 2 | Indicadores ambientales como luminosidad (lm) no son fáciles de interpretar. | **3** | Usabilidad – Correspondencia con el mundo real |
| 3 | El botón “Eliminar planta” está muy expuesto y puede presionarse accidentalmente. | **3** | Usabilidad – Prevención de errores |
| 4 | El detalle de la planta contiene demasiado texto, lo que sobrecarga visualmente. | **2** | Arquitectura de Información – Minimizar carga cognitiva |
| 5 | La sección de análisis avanzado no es clara y no se percibe como útil. | **2** | Usabilidad – Visibilidad de información relevante |
| 6 | La Landing Page no explica claramente el proceso de instalación de los sensores. | **2** | Usabilidad – Reconocimiento antes que recuerdo |
| 7 | Personalización limitada en notificaciones y recomendaciones. | **1** | Flexibilidad y eficiencia de uso |
| 8 | La configuración avanzada en la versión web genera confusión para usuarios generales. | **2** | Usabilidad – Consistencia y estándares |

---

### Descripción de Problemas

#### **Problema #1 – Configuración inicial del sensor IoT confusa**
**Escala:** 3  
**Heurística violada:** Usabilidad – Ayuda y documentación  
**Descripción:**  
Durante la configuración inicial del dispositivo IoT, los usuarios indicaron incertidumbre sobre los pasos necesarios y cómo validar que el sensor está activo correctamente.  
**Recomendación:**  
Implementar un asistente paso a paso con imágenes o video corto y estados visuales durante el proceso.

---

#### **Problema #2 – Indicadores ambientales poco interpretables**
**Escala:** 3  
**Heurística violada:** Usabilidad – Correspondencia con el mundo real  
**Descripción:**  
Valores como 750 lm (luminosidad) o porcentajes de humedad no resultan claros sin referencia contextual.  
**Recomendación:**  
Usar etiquetas visuales y rangos interpretables (Ej. *Baja / Adecuada / Alta*) o descripción comparativa.

---

#### **Problema #3 – Botón “Eliminar planta” expuesto**
**Escala:** 3  
**Heurística violada:** Usabilidad – Prevención de errores  
**Descripción:**  
El botón de eliminación está ubicado en una zona de acceso inmediato, lo cual puede llevar a activaciones accidentales.  
**Recomendación:**  
Reubicar la acción dentro de un menú secundario e incluir ventana de confirmación.

---

#### **Problema #4 – Texto extenso dentro del detalle de planta**
**Escala:** 2  
**Heurística violada:** Arquitectura de Información – Minimizar carga cognitiva  
**Descripción:**  
Los usuarios describieron la vista como “cargada” y difícil de leer rápidamente.  
**Recomendación:**  
Agregar opción *Leer más / Resumen*, mostrando inicialmente la versión corta.

---

#### **Problema #5 – Sección de análisis avanzado poco comprensible**
**Escala:** 2  
**Heurística violada:** Usabilidad – Visibilidad y relevancia de información  
**Descripción:**  
La sección no comunica claramente su propósito ni beneficios, por lo que se percibe poco relevante.  
**Recomendación:**  
Simplificar contenido o incluir ejemplos prácticos de valor agregado.

---
<!-- Esto Se Hace en plena clase -->


### 6.4.1. Auditoría realizada.
#### 6.4.1.1. Información del grupo auditado.
#### 6.4.1.2. Cronograma de auditoría realizada.
#### 6.4.1.3. Contenido de auditoría realizada.
### 6.4.2. Auditoría recibida.
#### 6.4.2.1. Información del grupo auditor.
#### 6.4.2.2. Cronograma de auditoría recibida.
#### 6.4.2.3. Contenido de auditoría recibida.
#### 6.4.2.4. Resumen de modificaciones para subsanar hallazgos.
