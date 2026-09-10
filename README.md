DROP DATABASE IF EXISTS amedida;
CREATE DATABASE amedida;
USE amedida;

CREATE TABLE usuario (
    id              INT AUTO_INCREMENT PRIMARY KEY,
    nombre          VARCHAR(100) NOT NULL,
    email           VARCHAR(100) NOT NULL UNIQUE,
    password        VARCHAR(100) NOT NULL,
    telefono        VARCHAR(20),
    fecha_registro  DATE NOT NULL
);

CREATE TABLE traje (
    id             INT AUTO_INCREMENT PRIMARY KEY,
    nombre         VARCHAR(100)   NOT NULL,
    categoria      VARCHAR(20)    NOT NULL,
    ocasion        VARCHAR(20),
    tela           VARCHAR(30),
    precio         DECIMAL(10,2)  NOT NULL,
    descripcion    VARCHAR(255),
    color          VARCHAR(30),
    incluye_forro  BOOLEAN        NOT NULL,
    stock          INT            NOT NULL
);

CREATE TABLE medida (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    usuario_id  INT NOT NULL,
    pecho       INT NOT NULL,
    cintura     INT NOT NULL,
    cadera      INT NOT NULL,
    largo       INT NOT NULL,
    FOREIGN KEY (usuario_id) REFERENCES usuario(id)
);

CREATE TABLE pedido (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    usuario_id  INT NOT NULL,
    fecha       DATE NOT NULL,
    estado      VARCHAR(20)   NOT NULL,
    total       DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (usuario_id) REFERENCES usuario(id)
);

CREATE TABLE detalle_pedido (
    id               INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id        INT NOT NULL,
    traje_id         INT NOT NULL,
    cantidad         INT NOT NULL,
    talla            VARCHAR(20) NOT NULL,
    precio_unitario  DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (pedido_id) REFERENCES pedido(id),
    FOREIGN KEY (traje_id)  REFERENCES traje(id)
);

INSERT INTO usuario (nombre, email, password, telefono, fecha_registro) VALUES
('Ana Martinez',  'ana@correo.com',   'ana123',   '3001234567', '2026-01-15'),
('Carlos Perez',  'carlos@correo.com','carlos123','3109876543', '2026-02-03');

INSERT INTO traje (nombre, categoria, ocasion, tela, precio, descripcion, color, incluye_forro, stock) VALUES
('Traje Ejecutivo Azul',    'Caballero', 'Ejecutivo', 'Lana peinada', 480000, 'Corte clasico en lana peinada, forro completo.', 'Azul',      TRUE,  12),
('Esmoquin Ceremonia',      'Caballero', 'Ceremonia', 'Terciopelo',   620000, 'Solapa de raso, para eventos formales.',         'Negro',     TRUE,   6),
('Traje Gris Oxford',       'Caballero', 'Ejecutivo', 'Oxford',       450000, 'Tejido transpirable, corte entallado moderno.',  'Gris',      FALSE, 10),
('Traje Sastre Rosa Palo',  'Dama',      'Ejecutivo', 'Crepe',        410000, 'Blazer y falda a juego, hombros estructurados.', 'Rosa palo', TRUE,   8),
('Conjunto Pantalon Verde', 'Dama',      'Casual',    'Lino',         395000, 'Blazer oversize y pantalon recto.',              'Verde',     FALSE,  9),
('Traje Sastre Marfil',     'Dama',      'Ceremonia', 'Crepe',        430000, 'Elegancia minimalista con botones dorados.',     'Marfil',    TRUE,   5);

INSERT INTO medida (usuario_id, pecho, cintura, cadera, largo) VALUES
(1, 92, 74, 98, 60),
(2, 104, 88, 102, 66);

INSERT INTO pedido (usuario_id, fecha, estado, total) VALUES
(1, '2026-03-10', 'Entregado',  410000),
(2, '2026-03-12', 'Pendiente', 1100000);

INSERT INTO detalle_pedido (pedido_id, traje_id, cantidad, talla, precio_unitario) VALUES
(1, 4, 1, 'A medida', 410000),
(2, 1, 1, 'L',        480000),
(2, 2, 1, 'A medida', 620000);
