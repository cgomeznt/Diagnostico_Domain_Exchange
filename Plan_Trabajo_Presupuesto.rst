### **Plan de Trabajo y Presupuesto para Diagnóstico de Infraestructura de Dominio (Windows Server 2022) y Exchange Server 2019**  
**Empresa:** [Nombre de la Empresa]  
**Fecha:** [Fecha de Elaboración]  
**Elaborado por:** [Tu Nombre o Empresa]  

---

## **1. Plan de Trabajo para Diagnóstico de Controladores de Dominio (Windows Server 2022)**  

### **1.1. Objetivos**  
- Verificar el estado funcional de los 2 controladores de dominio.  
- Evaluar replicación de Active Directory, DNS, y políticas de grupo.  
- Identificar posibles errores críticos en logs del sistema.  

### **1.2. Pasos del Diagnóstico**  

#### **A. Revisión Inicial**  
- ✅ **Estado de Servicios Críticos:**  
  ```powershell
  Get-Service Netlogon, DNS, KDC | Select Name, Status
  ```
- ✅ **Uso de Recursos (CPU, RAM, Disco):**  
  ```powershell
  Get-Counter '\Processor(_Total)\% Processor Time', '\Memory\Available MBytes', '\LogicalDisk(C:)\% Free Space'
  ```

#### **B. Diagnóstico de Active Directory**  
- ✅ **Replicación entre DCs:**  
  ```powershell
  repadmin /replsummary
  repadmin /showrepl * /errorsonly
  ```
- ✅ **Integridad de la Base de Datos (NTDS.DIT):**  
  ```powershell
  ntdsutil "ac i ntds" "files" "integrity" quit quit
  ```
- ✅ **Verificación de DNS:**  
  ```powershell
  Test-DnsServer -IPAddress <IP_DNS> -Context Dns
  ```

#### **C. Revisión de Políticas de Grupo (GPO)**  
- ✅ **Consistencia de GPOs:**  
  ```powershell
  Get-GPOReport -All -ReportType Html -Path "C:\Informes\GPO_Report.html"
  ```

#### **D. Documentación de Hallazgos**  
- Reporte con errores críticos y recomendaciones.  

---

## **2. Plan de Trabajo para Diagnóstico de Exchange Server 2019**  

### **2.1. Objetivos**  
- Verificar funcionamiento de servicios de correo.  
- Revisar estado de bases de datos, colas y conectividad.  
- Identificar problemas de certificados o permisos.  

### **2.2. Pasos del Diagnóstico**  

#### **A. Revisión Inicial**  
- ✅ **Servicios de Exchange:**  
  ```powershell
  Test-ServiceHealth
  Get-Service MSExchange* | Select Name, Status
  ```
- ✅ **Uso de Recursos:**  
  ```powershell
  Get-Counter '\Processor(_Total)\% Processor Time', '\Memory\Available MBytes', '\LogicalDisk(C:)\% Free Space'
  ```

#### **B. Diagnóstico de Bases de Datos**  
- ✅ **Estado de las Bases de Datos:**  
  ```powershell
  Get-MailboxDatabaseCopyStatus -Server <Nombre_Servidor> | FL Name, Status, ContentIndexState
  ```
- ✅ **Espacio en Bases de Datos:**  
  ```powershell
  Get-MailboxDatabase -Status | FL Name, DatabaseSize, AvailableNewMailboxSpace
  ```

#### **C. Verificación de Conectividad**  
- ✅ **Test de Conectividad Externa (OWA, EWS, ActiveSync):**  
  ```powershell
  Test-OutlookWebServices -Identity <Usuario_Prueba>
  ```
- ✅ **Revisión de Certificados SSL:**  
  ```powershell
  Get-ExchangeCertificate | FL Thumbprint, Services, Subject, NotAfter
  ```

#### **D. Documentación de Hallazgos**  
- Reporte con problemas detectados y recomendaciones.  

---

## **3. Presupuesto en Bolívares (Bs. VE)**  

| **Descripción** | **Horas Estimadas** | **Costo por Hora (Bs. VE)** | **Subtotal (Bs. VE)** |
|----------------|----------------|----------------|----------------|
| Diagnóstico Controladores de Dominio (2x WS2022) | 6 | [X] Bs. | [X] Bs. |
| Diagnóstico Exchange Server 2019 (2 servidores) | 8 | [X] Bs. | [X] Bs. |
| Elaboración de Informe Técnico | 4 | [X] Bs. | [X] Bs. |
| **Total** | **18** | | **[X] Bs.** |

**Notas:**  
- Incluye diagnóstico detallado y recomendaciones técnicas.  
- No incluye corrección de errores (cotización aparte).  
- Precio sujeto a variaciones por complejidad adicional.  

---

### **4. Aprobación**  
Nombre: ___________________________  
Firma: ___________________________  
Fecha: ___________________________  

---  
**Documento profesional actualizado con la información proporcionada (Windows Server 2022 + Exchange 2019).** ¿Necesitas ajustar algún detalle adicional?
