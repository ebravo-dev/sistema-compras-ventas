# 💼 Sistema de Compras y Ventas

<div align="center">

[![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://php.net)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-4.x-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Livewire](https://img.shields.io/badge/Livewire-3.x-FB70A9?style=for-the-badge&logo=livewire&logoColor=white)](https://livewire.laravel.com)

**Sistema integral de gestión de transacciones comerciales para el control de flujo de efectivo diario.**

[Características](#-características) · [Arquitectura](#-arquitectura) · [Instalación](#-instalación)

</div>

---

## 💡 ¿Qué es este sistema?

Sistema completo para la gestión de compras y ventas de una empresa, permitiendo el registro diario de ingresos y egresos, control de contactos (clientes/proveedores), gestión de facturas con soporte para archivos adjuntos, y un dashboard financiero con reportes en tiempo real.

> 🎯 **Objetivo:** Automatizar el control financiero diario, reemplazando hojas de cálculo por un sistema robusto con persistencia en base de datos.

---

## ✨ Características

### 💰 Control Financiero
- **Saldo Diario Automático:** Cálculo en tiempo real de ingresos, egresos y saldo neto.
- **Reportes Temporales:** Vista semanal y mensual con acumulados.
- **Folios Automáticos:** Generación secuencial `ING-001-2025` / `EGR-001-2025`.

### 👥 Gestión de Contactos
- CRUD completo de contactos con **soft deletes**.
- Tipos: Cliente, Proveedor o Ambos.
- Búsqueda por nombre, RFC o email.
- Validación de unicidad en nombres.

### 📝 Transacciones
- Registro de compras (egresos) y ventas (ingresos).
- Campos JSONB flexibles para referencias y datos de factura.
- Soporte para múltiples métodos de pago.
- Sistema de archivos adjuntos para facturas/documentos.

### 📊 Dashboard
- Saldo del día, semana y mes.
- Últimas 10 transacciones recientes.
- Interfaz limpia con Tailwind CSS y componentes Flux.

---

## 🏗️ Arquitectura

```text
app/
├── Http/Controllers/
│   ├── AuthController.php          # Autenticación personalizada
│   └── DashboardController.php     # Métricas financieras
├── Livewire/
│   ├── ContactosIndex.php          # CRUD de contactos
│   ├── TransaccionesIndex.php      # CRUD de transacciones
│   └── Forms/
│       └── ContactoForm.php        # Formulario reutilizable
└── Models/
    ├── Contacto.php                # SoftDeletes, scopes, relaciones
    ├── Transaccion.php             # Folios automáticos, JSONB, archivos
    └── MetodoPago.php              # Catálogo de métodos de pago

database/migrations/
├── contactos                       # tipo, nombre, email, teléfono, RFC
├── metodos_pago                    # nombre, activo
└── transacciones                   # folio, tipo, fecha, contacto_id,
                                    # referencia_datos (jsonb),
                                    # factura_datos (jsonb),
                                    # factura_archivos (jsonb),
                                    # metodo_pago_id, total, observaciones
```

### Tech Stack
- **Laravel 12.x** — Framework PHP
- **Livewire + Volt** — Componentes reactivos sin JavaScript
- **Tailwind CSS 4.x** — Estilos utilitarios
- **PostgreSQL 15+** — Base de datos con JSONB
- **Playwright** — Testing end-to-end
- **Pest PHP** — Testing unitario

---

## 🚀 Instalación

### Requisitos
- PHP `^8.2`
- PostgreSQL `15+`
- Composer `2.x`
- Node.js `18+`

```bash
# 1. Clonar
git clone https://github.com/ebravo-dev/sistema-compras-ventas.git
cd sistema-compras-ventas

# 2. Dependencias
composer install
npm install

# 3. Configuración
cp .env.example .env
php artisan key:generate

# 4. Base de datos
# Crear DB PostgreSQL y configurar .env
php artisan migrate
php artisan db:seed

# 5. Iniciar
composer run dev
```

### Credenciales por defecto
| Usuario | Email | Password |
|---------|-------|----------|
| Admin | admin@admin.com | admin |

---

## 🗄️ Esquema de Base de Datos

```sql
-- Contactos (Soft Deletes)
contactos
├── id, tipo, nombre, email, telefono, direccion, rfc
├── activo (boolean)
├── deleted_at (soft delete)
└── timestamps

-- Transacciones
transacciones
├── id, folio (único), tipo (ingreso/egreso), fecha
├── contacto_id (FK), metodo_pago_id (FK)
├── referencia_tipo, referencia_nombre, referencia_datos (jsonb)
├── factura_tipo, factura_numero, factura_datos (jsonb), factura_archivos (jsonb)
├── referencia_pago, total (decimal 15,2), observaciones
└── timestamps

-- Índices GIN para JSONB
CREATE INDEX idx_referencia_datos_gin ON transacciones USING gin(referencia_datos);
CREATE INDEX idx_factura_datos_gin ON transacciones USING gin(factura_datos);
```

---

## 🧪 Testing

```bash
# Tests PHP
composer test

# Tests E2E con Playwright
npx playwright test

# Tests específicos
php artisan test tests/Feature/DashboardTest.php
```

---

## 📄 Licencia

Proyecto privado de gestión comercial.  
Desarrollado por [Eder J. G. Bravo](https://github.com/ebravo-dev).

---

> *"Hecho para llevar el control financiero al siguiente nivel."* 💰
