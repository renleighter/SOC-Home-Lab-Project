# SOC-Home-Lab-Project
# 🛡️ Enterprise Home SOC & Telemetry Detection Lab

## 📌 Proje Özeti
Bu proje, kurumsal bir ağ ortamındaki saldırı vektörlerini tespit etmek, sistem telemetrisini (loglarını) toplamak ve SIEM paneli üzerinde analiz etmek amacıyla oluşturulmuş canlı bir Mavi Takım (Blue Team) laboratuvarıdır.

## 📐 Ağ ve Sistem Mimarisi
- **Kurban Makine (Target):** Windows 10/11 Enterprise (Sysmon Entegreli)
- **SIEM & Log Toplayıcı (Analist):** Ubuntu Server / Wazuh SIEM & Elastic Stack
- **Saldırgan Makine (Attacker):** Kali Linux
- **Ağ Yapılandırması:** Isolated Host-Only Network (192.168.56.0/24)

## 🛠️ Kullanılan Teknolojiler ve Araçlar
- **Sistem & İzleme:** Windows Event Logs, Sysmon (SwiftOnSecurity Config)
- **SIEM / EDR:** Wazuh Dashboard / Elastic Stack (ELK)
- **Trafik Analizi:** Wireshark, Zeek
- **Otomasyon & Scripting:** Python (Log parser), PowerShell

## 🎯 Test Edilen Saldırı Senaryoları ve Event ID Karşılıkları
| Saldırı Türü | Kullanılan Araç | Tespit Edilen Event ID / Kural |
| :--- | :--- | :--- |
| **RDP Brute Force** | Hydra / Crowbar | Windows Event ID 4625 (Failed Logon) |
| **Zararlı Kod Çalıştırma** | PowerShell Empire / Mimikatz | Sysmon Event ID 1 (Process Creation) & Event ID 10 (ProcessAccess) |
| **Ağ Taraması** | Nmap | Suricata / Wireshark TCP SYN Flood Alert |

## 📸 Ekran Görüntüleri ve Analiz Raporları
*(Laboratuvar kurulumu tamamlandıkça SIEM paneli ve log ekran görüntüleri buraya eklenecektir.)*  
---

### 🟢 Tamamlanan Faz 1: Sysmon & Wazuh Telemetri Entegrasyonu

Windows 10 kurban makinesindeki Sysmon loglarının (`Microsoft-Windows-Sysmon/Operational`) Wazuh SIEM ortamına aktarılması için ajan konfigürasyonu tamamlanmıştır.

#### 🛠️ Uygulanan Ajan Konfigürasyonu (`ossec.conf`)
```xml
<ossec_config>
  <client>
    <server>
      <address>192.168.42.130</address>
      <port>1514</port>
      <protocol>tcp</protocol>
    </server>
  </client>
  <localfile>
    <location>Microsoft-Windows-Sysmon/Operational</location>
    <log_format>eventchannel</log_format>
  </localfile>
</ossec_config>
