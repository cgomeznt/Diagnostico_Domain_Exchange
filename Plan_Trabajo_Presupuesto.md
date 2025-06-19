### **Documento Unificado - Diagnóstico de Infraestructura IT**  
**Empresa:** SERVICIO MAS QUE DIGITAL, C.A.

**Fecha:** 16/06/2025

**Elaborado por:**   Carlos Gómez / José V. Echeverria


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
| **Descripción**                | **Horas** |  **Subtotal** |
|--------------------------------|-----------|--------------|
| Diagnóstico DCs (incluye NTP)  | 7         |  1250 $.      |
| Diagnóstico Exchange           | 8         |  1250 $.      |
| Informe técnico                | 4         |  500 $.      |
| **TOTAL**                      | **19**    |  **3.500 $**  |

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


