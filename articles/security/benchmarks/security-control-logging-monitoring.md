---
title: Azure Güvenlik Kontrolü - Günlük ve İzleme
description: Azure Güvenlik Denetimi Günlük ve İzleme
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: e58df0997c62a131a3c9987369f8e076a62d6654
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408337"
---
# <a name="security-control-logging-and-monitoring"></a>Güvenlik Kontrolü: Günlük ve İzleme

Güvenlik günlüğü ve izleme, Azure hizmetleri için denetim günlüklerini etkinleştirme, satın alma ve depolamayla ilgili etkinliklere odaklanır.

## <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.1 | 6.1 | Microsoft |

Microsoft, Azure kaynakları için zaman kaynakları tutar, ancak bilgi işlem kaynaklarınız için zaman eşitleme ayarlarını yönetme seçeneğiniz vardır.

- [Azure Windows bilgi işlem kaynakları için zaman eşitleme yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync)

- [Azure Linux bilgi işlem kaynakları için zaman eşitleme nasıl yapılandırılır?](https://docs.microsoft.com/azure/virtual-machines/linux/time-sync)

## <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,2 | 6.5, 6.6 | Müşteri |

Uç nokta aygıtları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Monitor üzerinden günlükleri alın. Azure Monitor'da, analitiği sorgulamak ve gerçekleştirmek için Log Analytics Workspace(ler)'i kullanın ve uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Alternatif olarak, Azure Sentinel'e veya bir üçüncü taraf SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. 

- [Azure Sentinel'e nasıl binilir?](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır?](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Azure Monitor ile Azure Sanal Makine dahili ana bilgisayar günlükleri nasıl toplanır?](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Azure Monitör ve üçüncü taraf SIEM entegrasyonu ile nasıl başlarsınız?](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.3 | 6.2, 6.3 | Müşteri |

Denetim, güvenlik ve tanılama günlüklerine erişmek için Azure kaynaklarında Tanılama Ayarlarını etkinleştirin. Otomatik olarak kullanılabilen etkinlik günlükleri, olay kaynağı, tarih, kullanıcı, zaman damgası, kaynak adresleri, hedef adresleri ve diğer yararlı öğeleri içerir.

- [Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır?](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Azure'da günlük ve farklı günlük türlerini anlama](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.4 | 6.2, 6.3 | Müşteri |

Bilgi işlem kaynağı Microsoft'a aitse, bu kaynağı izlemekten Microsoft sorumludur. Bilgi işlem kaynağı kuruluşunuz tarafından sahipleniyorsa, onu izlemek sizin sorumluluğunuzdadır. İşletim sistemi izlemek için Azure Güvenlik Merkezi'ni kullanabilirsiniz. İşletim sisteminden Security Center tarafından toplanan veriler işletim sistemi türü ve sürümü, işletim sistemi (Windows Olay Günlükleri), çalışan süreçleri, makine adını, IP adreslerini ve kullanıcıoturumlarını içerir. Log Analytics Agent da kilitlenme dökümü dosyaları toplar.

- [Azure Monitor ile Azure Sanal Makine dahili ana bilgisayar günlükleri nasıl toplanır?](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Azure Güvenlik Merkezi veri toplamayı anlama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

## <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,5 | 6.4 | Müşteri |

Azure Monitör'de, Log Analytics Çalışma Alanı saklama sürenizi kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

- [Log Analytics'te veri saklama süresini değiştirme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Azure Depolama hesap günlükleri için bekletme ilkesi nasıl yapılandırılabilen](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

## <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,6 | 6.7 | Müşteri |

Anormal davranışlar için günlükleri analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi Çalışma Alanını kullanın.

Alternatif olarak, Azure Sentinel'e veya üçüncü taraf bir SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz. 

- [Azure Sentinel'e nasıl binilir?](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Log Analytics Çalışma Alanını Anlayın](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

- [Azure Monitor'da özel sorgular nasıl gerçekleştirilir?](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

## <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Anormal aktiviteler için uyarıları etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.7 | 6.8 | Müşteri |

Güvenlik günlüklerinde ve olaylarında bulunan anormal etkinlikleri izlemek ve uyarmak için Log Analytics Çalışma Alanına sahip Azure Güvenlik Merkezi'ni kullanın.

Alternatif olarak, Azure Sentinel'e veri etkinleştirebilir ve yerleşik veriler de dahil olabilirsiniz.

- [Azure Sentinel'e nasıl binilir?](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Azure Güvenlik Merkezi'nde uyarılar nasıl yönetilir?](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Günlük analitiği günlük verilerinde nasıl uyarı kullanılır?](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

## <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.8 | 8.6 | Müşteri |

Azure Sanal Makineleri ve Bulut Hizmetleri için kötü amaçlı yazılımdan koruma olayı koleksiyonunu etkinleştirin.

- [Sanal Makineler için Microsoft Antimalware nasıl yapılandırılır?](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmmicrosoftantimalwareextension?view=azuresmps-4.0.0)

- [Bulut Hizmetleri için Microsoft Antimalware nasıl yapılandırılır?](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureserviceantimalwareextension?view=azuresmps-4.0.0)

- [Microsoft Antimalware'i anlayın](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

## <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2.9 | 8.7 | Müşteri |

Kuruluşlarınizin ihtiyacına göre DNS günlük çözümü için Azure Marketplace'ten üçüncü taraf çözümü uygulayın.  

## <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 2,10 | 8.8 | Müşteri |

İşlem oluşturma olayını ve CommandLine alanını günlüğe kaydetmek için desteklenen tüm Azure Windows sanal makinelerinde Microsoft İzleme Aracısı'nı kullanın.   Desteklenen Azure Linux Sanal makineleri için konsol günlüğünü düğüm başına el ile yapılandırabilir ve verileri depolamak için Syslog'u kullanabilirsiniz.  Ayrıca, günlükleri gözden geçirmek ve Azure Virtual makinelerinden günlüğe kaydedilmiş verilerüzerinde sorgular gerçekleştirmek için Azure Monitor'un Log Analytics çalışma alanını kullanın. 

- [Azure Güvenlik Merkezinde veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

- [Azure Monitor'da özel sorgular nasıl gerçekleştirilir?](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

- [Azure İzleyici'de Syslog veri kaynakları](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Denetimine bakın: [Kimlik ve Erişim Denetimi](security-control-identity-access-control.md)