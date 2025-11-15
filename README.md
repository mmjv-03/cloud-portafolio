#  Proyecto 1: Hosting Estático en AWS con S3, CloudFront y Route 53

Este proyecto demuestra la creación de una arquitectura en AWS para alojar un sitio web estático utilizando **Amazon S3**, distribuido globalmente mediante **Amazon CloudFront**, y con dominio administrado en **Route 53**.

Es un proyecto orientado a prácticas reales de **Cloud Computing** y **Cloud Security**, ideal para un portafolio técnico profesional.

---

##  Arquitectura Propuesta

La infraestructura implementa:

- **Amazon S3**  
  - Bucket configurado para hosting estático  
  - Políticas de seguridad con *Least Privilege*  
  - Bloqueo de acceso público innecesario  

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

##  Objetivos del Proyecto

- Implementar un sitio web estático seguro y escalable.  
- Asegurar el acceso al bucket mediante OAC.  
- Automatizar configuración base (si se desea en versiones futuras).  
- Demostrar habilidades prácticas de AWS para roles Cloud/Seguridad.

---

## 📁 Estructura del Repositorio

