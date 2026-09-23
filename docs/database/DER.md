# Diagrama Entidad-Relación (DER) - EcoBite

```mermaid
erDiagram
    USUARIO ||--o{ COMERCIO : administra
    USUARIO ||--o{ PEDIDO : realiza
    COMERCIO ||--|{ PRODUCTO : publica
    COMERCIO ||--o{ PEDIDO : recibe
    PEDIDO ||--|{ DETALLE_PEDIDO : contiene
    PRODUCTO ||--o{ DETALLE_PEDIDO : incluye

    USUARIO {
        int id PK
        string nombre
        string email UK
        string password_hash
        string rol "CLIENTE | COMERCIO | ADMIN"
        string telefono
        datetime created_at
    }

    COMERCIO {
        int id PK
        int usuario_id FK
        string nombre_comercial
        string direccion
        decimal latitud
        decimal longitud
        string telefono
    }

    PRODUCTO {
        int id PK
        int comercio_id FK
        string titulo
        string descripcion
        decimal precio_original
        decimal precio_descuento
        int stock
        time hora_inicio_retiro
        time hora_fin_retiro
        string estado "DISPONIBLE | AGOTADO"
    }

    PEDIDO {
        int id PK
        int usuario_id FK
        int comercio_id FK
        decimal total
        string estado "PENDIENTE | RETIRADO | CANCELADO"
        string codigo_retiro
        datetime fecha_creacion
    }

    DETALLE_PEDIDO {
        int id PK
        int pedido_id FK
        int producto_id FK
        int cantidad
        decimal precio_unitario
    }
