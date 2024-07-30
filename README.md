# 1- CONSTRUCCION DEL MODELO DE DATOS:
## Tablas:
### LENGUAJES
CREATE TABLE Lenguajes (
    id_lenguaje NUMBER PRIMARY KEY,
    nombre VARCHAR2(100) NOT NULL,
    estado CHAR(1) CHECK (estado IN ('A', 'I')),
    usuario VARCHAR2(50),
    fecha_actualizacion DATE
);

### PROGRAMADORES

CREATE TABLE Programadores (
    id_programador NUMBER PRIMARY KEY,
    primer_nombre VARCHAR2(50) NOT NULL,
    segundo_nombre VARCHAR2(50),
    primer_apellido VARCHAR2(50) NOT NULL,
    segundo_apellido VARCHAR2(50),
    edad NUMBER CHECK (edad > 0),
    anos_experiencia NUMBER CHECK (anos_experiencia > 0),
    sexo CHAR(1) CHECK (sexo IN ('M', 'F')),
    usuario VARCHAR2(50),
    fecha_actualizacion DATE
);


### PROYECTOS
CREATE TABLE Proyectos (
    id_proyecto NUMBER PRIMARY KEY,
    nombre VARCHAR2(100) NOT NULL,
    valor NUMBER(10, 2),
    fecha_inicio DATE,
    fecha_fin DATE,
    cant_programadores NUMBER CHECK (cant_programadores > 0),
    lenguaje_requerido NUMBER,
    estado CHAR(1) CHECK (estado IN ('V', 'T')),
    usuario VARCHAR2(50),
    fecha_actualizacion DATE,
    CONSTRAINT fk_lenguaje FOREIGN KEY (lenguaje_requerido) REFERENCES Lenguajes(id_lenguaje)
);

# 2- SECUENCIAS Y TRIGGERS USADOS:
## SECUENCIAS 
CREATE SEQUENCE seq_lenguaje START WITH 1;
CREATE SEQUENCE seq_programador START WITH 1;
CREATE SEQUENCE seq_proyecto START WITH 1;

## TRIGGERS

CREATE OR REPLACE TRIGGER trg_lenguaje
BEFORE INSERT ON Lenguajes
FOR EACH ROW
BEGIN
    :NEW.id_lenguaje := seq_lenguaje.NEXTVAL;
    :NEW.fecha_actualizacion := SYSDATE;
    :NEW.usuario := USER;
END;

CREATE OR REPLACE TRIGGER trg_programador
BEFORE INSERT ON Programadores
FOR EACH ROW
BEGIN
    :NEW.id_programador := seq_programador.NEXTVAL;
    :NEW.fecha_actualizacion := SYSDATE;
    :NEW.usuario := USER;
END;

CREATE OR REPLACE TRIGGER trg_proyecto
BEFORE INSERT ON Proyectos
FOR EACH ROW
BEGIN
    :NEW.id_proyecto := seq_proyecto.NEXTVAL;
    :NEW.fecha_actualizacion := SYSDATE;
    :NEW.usuario := USER;
END;

# 3-INSERTAR REGISTROS

## TABLA LENGUAJES:
INSERT INTO Lenguajes (id_lenguaje, nombre, estado, usuario, fecha_actualizacion) VALUES (seq_lenguaje.NEXTVAL, 'Java', 'A', USER, SYSDATE);


## TABLA PROGRAMADORES

INSERT INTO Programadores (id_programador, primer_nombre, primer_apellido, edad, anos_experiencia, sexo, usuario, fecha_actualizacion) VALUES (seq_programador.NEXTVAL, 'Juan', 'Pérez', 30, 5, 'M', USER, SYSDATE);

## TABLA PROYECTOS

INSERT INTO Proyectos (id_proyecto, nombre, lenguaje_requerido, usuario, fecha_actualizacion) VALUES (seq_proyecto.NEXTVAL, 'Sistema de Gestión Escolar', (SELECT id_lenguaje FROM Lenguajes WHERE nombre = 'Java'), USER, SYSDATE);






