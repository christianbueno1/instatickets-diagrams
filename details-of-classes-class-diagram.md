Perfecto, Christian. A continuación te detallo cada clase para el **diagrama de clases completo del sistema InstaTickets**, incluyendo:

* Qué representa
* Atributos típicos (con tipo y ejemplo)
* Relaciones con otras clases

---

### 🎭 **Dominio de Escenarios y Mapas**

#### 1. `Mapa`

* **Representa:** un plano editable de un evento.
* **Atributos:**

  * `id: UUID` → `"mapa-001"`
  * `nombre: String` → `"Concierto RockFest"`
  * `estado: EstadoMapa` → `"Publicado"`
  * `clienteId: UUID` → referencia al cliente
  * `eventoId: UUID` → referencia al evento
* **Relaciones:**

  * 1 `Mapa` → \* `Localidad`
  * 1 `Mapa` → \* `PuntoDeInteres`
  * 1 `Mapa` → \* `HistorialCambios`, \* `VersionMapa`
  * 1 `Mapa` → 1 `ConfiguracionMapa`

---

#### 2. `Localidad`

* **Representa:** una zona de asientos dentro de un mapa.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"Palco A"`
  * `precio: Decimal` → `60.00`
  * `color: String` → `"#FF0000"`
  * `tipoNumeracion: String` → `"Alfanumérica"`
* **Relaciones:**

  * 1 `Localidad` → \* `Asiento`
  * 1 `Localidad` → 1 `ConfiguracionLocalidad`
  * * `Localidad` → 1 `Mapa`

---

#### 3. `Asiento`

* **Representa:** un asiento individual.
* **Atributos:**

  * `id: UUID`
  * `numero: String` → `"A12"`
  * `estado: String` → `"Disponible"`
* **Relaciones:**

  * 1 `Asiento` → 1 `Localidad`
  * 1 `Asiento` → opcional 1 `GrupoAsientos`

---

#### 4. `PuntoDeInteres`

* **Representa:** íconos de accesos, baños, etc.
* **Atributos:**

  * `id: UUID`
  * `tipo: String` → `"Baño"`
  * `coordenadas: (x: Int, y: Int)`
* **Relaciones:**

  * * `PuntoDeInteres` → 1 `Mapa`

---

#### 5. `Snippet`

* **Representa:** código embebido para visualizar un mapa.
* **Atributos:**

  * `id: UUID`
  * `html: Text`
  * `tokenAccesoId: UUID`
* **Relaciones:**

  * 1 `Snippet` → 1 `Mapa`
  * 1 `Snippet` → 1 `TokenAcceso`

---

### 🗓️ **Dominio de Eventos y Clientes**

#### 6. `Cliente`

* **Representa:** la empresa/persona que organiza eventos.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"ShowPro Ecuador"`
  * `emailContacto: String`
* **Relaciones:**

  * 1 `Cliente` → \* `Evento`
  * 1 `Cliente` → \* `Mapa`

---

#### 7. `Evento`

* **Representa:** espectáculo o función.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"Luis Miguel 2025"`
  * `fecha: DateTime`
  * `sedeId: UUID`
* **Relaciones:**

  * 1 `Evento` → 1 `Sede`
  * 1 `Evento` → \* `Mapa`

---

#### 8. `Sede`

* **Representa:** lugar físico del evento.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"Coliseo Voltaire"`
  * `direccion: String`
* **Relaciones:**

  * 1 `Sede` → \* `Evento`

---

### 👤 **Dominio de Usuarios y Permisos**

#### 9. `Usuario`

* **Representa:** usuarios del sistema.
* **Atributos:**

  * `id: UUID`
  * `nombre: String`
  * `rolId: UUID`
* **Relaciones:**

  * * `Usuario` → 1 `Rol`
  * * `Usuario` → \* `HistorialCambios`

---

#### 10. `Rol`

* **Representa:** perfil de usuario.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"Administrador"`
* **Relaciones:**

  * 1 `Rol` → \* `Permiso`
  * 1 `Rol` → \* `Usuario`

---

#### 11. `Permiso`

* **Representa:** acción autorizada.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"editar_mapa"`
* **Relaciones:**

  * * `Permiso` → \* `Rol`

---

### 🎨 **Dominio Visual y Configuración**

#### 12. `EditorVisual`

* **Representa:** interfaz de edición.
* **Atributos:**

  * `id: UUID`
  * `modo: String` → `"Selección"`
* **Relaciones:**

  * 1 `EditorVisual` → 1 `Mapa` (controla edición)

---

#### 13. `ConfiguracionLocalidad`

* **Representa:** opciones visuales por localidad.
* **Atributos:**

  * `alineacion: String` → `"Horizontal"`
  * `mostrarEtiqueta: Boolean`
* **Relaciones:**

  * 1 `ConfiguracionLocalidad` → 1 `Localidad`

---

#### 14. `ConfiguracionMapa`

* **Representa:** opciones visuales globales.
* **Atributos:**

  * `modoOscuro: Boolean`
  * `numeracionGlobal: Boolean`
* **Relaciones:**

  * 1 `ConfiguracionMapa` → 1 `Mapa`

---

### 🔐 **Dominio de Seguridad y Auditoría**

#### 15. `HistorialCambios`

* **Representa:** log de cambios.
* **Atributos:**

  * `fecha: DateTime`
  * `usuarioId: UUID`
  * `descripcion: String`
* **Relaciones:**

  * 1 `HistorialCambios` → 1 `Usuario`
  * 1 `HistorialCambios` → 1 `Mapa`

---

#### 16. `VersionMapa`

* **Representa:** versiones del mapa.
* **Atributos:**

  * `numeroVersion: Int`
  * `fechaGuardado: DateTime`
* **Relaciones:**

  * * `VersionMapa` → 1 `Mapa`

---

#### 17. `TokenAcceso`

* **Representa:** token para acceder al mapa.
* **Atributos:**

  * `token: String`
  * `tipo: String` → `"solo_lectura"`
* **Relaciones:**

  * 1 `TokenAcceso` → 1 `Snippet`

---

### 🛡️ **Dominio de Validaciones**

#### 18. `ReglaAforo`

* **Representa:** validación de aforo.
* **Atributos:**

  * `tipo: String` → `"Máximo 1000"`
  * `valor: Int`
* **Relaciones:**

  * 1 `ReglaAforo` → 1 `Mapa`

---

#### 19. `RestriccionEdicion`

* **Representa:** bloqueos en el editor.
* **Atributos:**

  * `motivo: String` → `"Revisión en curso"`
* **Relaciones:**

  * 1 `RestriccionEdicion` → 1 `Mapa`

---

#### 20. `EstadoMapa`

* **Representa:** estado del mapa.
* **Atributos:**

  * `nombre: Enum` → `Borrador | Publicado | EnRevisión`

---

### 📊 **Adicionales**

#### 21. `GrupoAsientos`

* **Representa:** agrupación de asientos.
* **Atributos:**

  * `id: UUID`
  * `nombre: String` → `"VIP Frontal"`
* **Relaciones:**

  * 1 `GrupoAsientos` → \* `Asiento`

---

#### 22. `EstadisticaMapa`

* **Representa:** métricas de uso.
* **Atributos:**

  * `vistas: Int`
  * `clicks: Int`
* **Relaciones:**

  * 1 `EstadisticaMapa` → 1 `Mapa`

---

¿Deseas que lo genere en formato de tabla para guardar como PDF o DOCX? ¿O quieres avanzar ya con la representación PlantUML del diagrama de clases completo?
