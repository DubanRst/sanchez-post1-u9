# Recordatorios Citas Service

## Descripción

Repositorio del post-contenido de la Unidad 9 de Patrones de Diseño de Software. Un único proyecto Spring Boot que envía recordatorios de citas por el canal preferido del paciente (correo o SMS), construido desde el inicio con Strategy (ReminderChannel) y Factory Method (ReminderChannelFactory) inyectados por constructor.

## Parte 1: Pruebas unitarias con JUnit 5 y Mockito

CitaReminderServiceImplTest aísla CitaReminderServiceImpl de sus tres colaboradores (CitaRepository, ReminderChannelFactory y Clock).

![Pruebas de la Parte 1](docs/01-pruebas-parte1.png)

## Decisiones de diseño

### Mock, stub o fake para cada colaborador

**CitaRepository: Mock.** No basta con stubbear `findByFechaHoraCitaBetweenAndRecordatorioEnviadoFalse` para que devuelva una lista: también importa verificar que `save()` se llama exactamente una vez y con la cita marcada como enviada. Cuando una prueba necesita controlar la respuesta y verificar la interacción, es un mock y no un simple stub.

**ReminderChannelFactory y ReminderChannel: Mock.** El comportamiento bajo prueba es precisamente la selección de estrategia: que el servicio pida el canal EMAIL cuando la cita lo prefiere así y que ese canal reciba el mensaje. Eso solo se comprueba verificando la interacción, no con un valor de retorno.

**Clock: Fake.** Mockear Clock exigiría stubbear `instant()` y `getZone()` por separado, con el riesgo de que `LocalDateTime.now(clock)` no se comporte como con un reloj real. `Clock.fixed(...)` es una implementación completa y correcta provista por el propio JDK, así que es más simple, más legible y más fiel al comportamiento real.

## Ejecutar las pruebas

```powershell
mvn test
```