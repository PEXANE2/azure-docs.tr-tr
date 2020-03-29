---
title: Azure Güvenlik Kontrolü - Günlük ve İzleme
description: Güvenlik Kontrolü Günlüğü ve İzleme
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: ae9c678d9dfca895ec74ed92bcb1b541db6b134e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76545509"
---
# <a name="security-control-logging-and-monitoring"></a>Güvenlik Kontrolü: Günlük ve İzleme

Güvenlik günlüğü ve izleme, Azure hizmetleri için denetim günlüklerini etkinleştirme, satın alma ve depolamayla ilgili etkinliklere odaklanır.

## <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.1 | 6.1 | Microsoft |

Microsoft, Azure kaynakları için zaman kaynakları tutar, ancak bilgi işlem kaynaklarınız için zaman eşitleme ayarlarını yönetme seçeneğiniz vardır.

Azure bilgi işlem kaynakları için zaman eşitleme yapılandırması nasıl yapılandırılır:

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

## <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,2 | 6.5, 6.6 | Müşteri |

Uç nokta aygıtları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Monitor üzerinden günlükleri alın. Azure Monitor'da, analitiği sorgulamak ve gerçekleştirmek için Log Analytics Workspace(ler)'i kullanın ve uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Alternatif olarak, Azure Sentinel'e veya bir üçüncü taraf SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. Azure Sentinel'e nasıl binilir:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır:

https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings

Azure Monitor ile Azure Sanal Makine dahili ana bilgisayar günlükleri nasıl toplanır:

https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm

Azure Monitor ve üçüncü taraf SIEM tümleştirmesi ile nasıl başlayın:

https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.3 | 6.2, 6.3 | Müşteri |

Denetim, güvenlik ve tanılama günlüklerine erişmek için Azure kaynaklarında Tanılama Ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri, olay kaynağı, tarih, kullanıcı, zaman damgası, kaynak adresleri, hedef adresleri ve diğer yararlı öğeleri içerir.

Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır:

https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings

Azure'da günlük ve farklı günlük türlerini anlayın:

https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.4 | 6.2, 6.3 | Müşteri |

Bilgi işlem kaynağı Microsoft'a aitse, bu kaynağı izlemekten Microsoft sorumludur. Bilgi işlem kaynağı kuruluşunuz tarafından sahipleniyorsa, onu izlemek sizin sorumluluğunuzdadır. İşletim sistemi izlemek için Azure Güvenlik Merkezi'ni kullanabilirsiniz. İşletim sisteminden Security Center tarafından toplanan veriler işletim sistemi türü ve sürümünü, İşletim Sistemi Günlüklerini (Windows Olay Günlükleri), çalıştırma işlemlerini, makine adını, IP adreslerini ve kullanıcı oturumlarını içerir. Log Analytics Agent da kilitlenme dökümü dosyaları toplar.

Azure Monitor ile Azure Sanal Makine dahili ana bilgisayar günlükleri nasıl toplanır:

https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm

Azure Güvenlik Merkezi veri toplamayı anlayın:

https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection

## <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,5 | 6.4 | Müşteri |

Azure Monitör'de, Log Analytics Çalışma Alanı saklama sürenizi kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Log Analytics Çalışma Alanları için günlük bekletme parametreleri nasıl ayarlanır:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

## <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,6 | 6.7 | Müşteri |

Anormal davranışlar için günlükleri analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi Çalışma Alanını kullanın.

Alternatif olarak, Azure Sentinel'e veya üçüncü taraf bir SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. 

Azure Sentinel'e nasıl binilir:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Günlük Analizi Çalışma Alanını Anlayın:

https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:

https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

## <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.7 | 6.8 | Müşteri |

Güvenlik günlüklerinde ve olaylarında bulunan anormal etkinlikleri izlemek ve uyarmak için Log Analytics Çalışma Alanına sahip Azure Güvenlik Merkezi'ni kullanın.

Alternatif olarak, Azure Sentinel'e veri etkinleştirebilir ve yerleşik veriler de dahil olabilirsiniz.

Azure Sentinel'e nasıl binilir:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Güvenlik Merkezi'nde uyarılar nasıl yönetilir:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

Günlük analitiği günlük verilerinde nasıl uyarı olun:

https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

## <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.8 | 8.6 | Müşteri |

Azure Sanal Makineleri ve Bulut Hizmetleri için kötü amaçlı yazılımdan koruma olayı koleksiyonunu etkinleştirin.

Sanal Makineler için Microsoft Antimalware yapılandırmak için nasıl:

https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmmicrosoftantimalwareextension?view=azuresmps-4.0.0

Bulut Hizmetleri için Microsoft Antimalware nasıl yapılandırılır:

https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureserviceantimalwareextension?view=azuresmps-4.0.0

Microsoft Antimalware anlayın:

https://docs.microsoft.com/azure/security/fundamentals/antimalware

## <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.9 | 8.7 | Müşteri |

DNS günlüğe kaydetme için bir üçüncü taraf çözümü uygulayın.

## <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.1 | 8.8 | Müşteri |

Konsol günlüğe kaydetmeyi ve PowerShell Transkripsiyon'u düğüm başına göre el ile yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki güvenlik denetimine bakın: [Kimlik ve Erişim Denetimi](security-control-identity-access-control.md)
