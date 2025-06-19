### **Documento Unificado - Diagnóstico de Infraestructura IT**  
**Empresa:** [Nombre de la Empresa]  
**Fecha:** [DD/MM/AAAA]  
**Elaborado por:** [Nombre del Consultor]  

---

## **1. Introducción**  
Este documento integra el plan de trabajo y presupuesto para el diagnóstico de:  
✅ **2 Controladores de Dominio (Windows Server 2022)**  
✅ **2 Servidores Exchange 2019**  
✅ **Verificación de sincronización NTP**  

---

## **2. Plan de Trabajo Detallado**  

### **2.1 Diagnóstico de Controladores de Dominio**  
#### **A. Verificación Básica**  
| **Parámetro**          | **Comando/Herramienta**                     |
|-------------------------|---------------------------------------------|
| Servicios críticos      | `Get-Service Netlogon, DNS, KDC \| Select Name, Status` |
| Uso de recursos         | `Get-Counter '\Processor(_Total)\% Processor Time'` |
| Sincronización NTP      | `w32tm /query /status`                     |

#### **B. Diagnóstico Avanzado**  
```powershell
# Replicación AD:
repadmin /replsummary
# Integridad AD:
ntdsutil "ac i ntds" "files" "integrity" quit quit
```

---

### **2.2 Diagnóstico de Exchange Server 2019**  
#### **A. Chequeos Esenciales**  
| **Componente**         | **Comando**                                |
|-------------------------|--------------------------------------------|
| Servicios Exchange     | `Test-ServiceHealth`                       |
| Bases de datos         | `Get-MailboxDatabaseCopyStatus`            |
| Conectividad           | `Test-OutlookWebServices`                  |

#### **B. Monitorización**  
```powershell
# Certificados:
Get-ExchangeCertificate \| FL Thumbprint, NotAfter
# Colas de correo:
Get-Queue \| Where-Object {$_.MessageCount -gt 50}
```

---

## **3. Presupuesto (Bs. VE)**  
| **Descripción**                | **Horas** | **Costo/Hora** | **Subtotal** |
|--------------------------------|-----------|----------------|--------------|
| Diagnóstico DCs (incluye NTP)  | 7         | [X] Bs.        | [X] Bs.      |
| Diagnóstico Exchange           | 8         | [X] Bs.        | [X] Bs.      |
| Informe técnico                | 4         | [X] Bs.        | [X] Bs.      |
| **TOTAL**                      | **19**    |                | **[X] Bs.**  |

**Notas:**  
- Costos válidos por 15 días  
- Horario de trabajo: L-V (8:00 AM - 5:00 PM)  

---

## **4. Anexos Técnicos**  
### **4.1 Ejemplo de Hallazgos NTP**  
```plaintext
[DC01] 
- Fuente de tiempo: time.windows.com 
- Desfase: +85ms (✅ dentro de umbral) 

[DC02] 
- Error: No sincronizado con PDC 
- Acción: w32tm /config /syncfromflags:domhier /update
```

### **4.2 Checklist Preliminar**  
- [ ] Acceso remoto a servidores  
- [ ] Credenciales de administrador  
- [ ] Backup reciente de AD y Exchange  

---

## **5. Firmas**  
**Cliente:** ________________________   Fecha: ___/___/___  
**Proveedor:** ______________________   Fecha: ___/___/___  

---

### **Formato para Word 2016:**  
1. Copie este documento completo  
2. Pegue en Word usando **"Conservar formato de origen"**  
3. Ajuste fuentes a:  
   - Títulos: **Calibri Light 14pt (azul #2F5496)**  
   - Cuerpo: **Calibri 11pt**  
4. Insertar logo de empresa en portada (opcional)  

**¿Requiere personalización adicional?** Estoy disponible para ajustes.
