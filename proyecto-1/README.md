# 🚀 Proyecto 1: Hosting Estático en AWS con S3, CloudFront y Route 53

Este proyecto demuestra la creación de una arquitectura en AWS para alojar un sitio web estático utilizando **Amazon S3**, distribuido globalmente mediante **Amazon CloudFront**, y con dominio administrado en **Route 53**.

Es un proyecto orientado a prácticas reales de **Cloud Computing** y **Cloud Security**, ideal para un portafolio técnico profesional.

---

## 📌 Arquitectura Propuesta

La infraestructura implementa:

- **Amazon S3**  
  - Bucket configurado para hosting estático  
  - Políticas de seguridad con *Least Privilege*  
  - Bloqueo de acceso público 

- **AWS CloudFront**  
  - Distribución CDN global  
  - Origin Access Control (OAC) para proteger el bucket  
  - HTTPS habilitado  
  - Compresión automática  
  - Caching configurable

- **Route 53 (Opcional)**  
  - Hosted Zone  
  - Registro A o CNAME apuntando a CloudFront  
  - Gestión DNS segura  

---

## 🛠️ Servicios utilizados

| Servicio | Uso |
|---------|-----|
| **S3** | Hosting estático y almacenamiento |
| **CloudFront** | Distribución CDN global |
| **IAM** | Políticas mínimas necesarias |
| **Route 53** | DNS y dominio (opcional) |

---

## 🧩 Objetivos del Proyecto

- Implementar un sitio web estático seguro y escalable.  
- Asegurar el acceso al bucket mediante OAC.  
- Automatizar configuración base (si se desea en versiones futuras).  
- Demostrar habilidades prácticas de AWS para roles Cloud/Seguridad.

---

## 📁 Estructura del Repositorio

proyecto-1/
│── README.md # Documentación del proyecto
│── arquitectura.png # Diagrama de arquitectura (opcional)
│── sitio/ # Carpeta con archivos HTML/CSS/JS


---


---

## 🧪 Pasos para reproducir la infraestructura

1. Crear bucket S3 sin acceso público  
2. Habilitar hosting estático en S3  
3. Subir contenido HTML/CSS/JS  
4. Crear distribución CloudFront  
5. Configurar OAC para proteger el bucket  
6. (Opcional) Conectar dominio vía Route 53  
7. Probar disponibilidad global  

---

## 🔒 Seguridad Considerada

- Se utilizó OAC (Origin Access Control) para evitar acceso directo al bucket.  
- Se configuraron políticas IAM con **principio de mínimo privilegio**.  
- Se aseguró tráfico HTTPS mediante CloudFront.  
- Se deshabilitó el acceso público al bucket.

---

## 📸 Diagrama de Arquitectura

(Agregar aquí arquitectura.png cuando esté lista)

---

## 📂 Archivos Incluidos

- `sitio/` → sitio estático (index.html, estilos, imágenes, etc.)  
- `policy.json` → política IAM aplicada (opcional)  
- `cloudfront-config.txt` → detalles de configuraciones  

---

## 💡 Cosas que podría agregar en el futuro

- Automatización con Terraform  
- Automatización con AWS CLI  
- Landing page más completa  
- Logging y monitoreo con CloudWatch  

---

## 👤 Autor

**Víctor Matos**  
Cloud & Security Student  
Certificación: AWS Certified Security – Specialty (SCS-C02)

---

## ⭐ Si te gusta este proyecto

Puedes dejar una estrella ⭐ en el repositorio.

