---
title: Azure Güvenlik Merkezi uyarılarını ve önerilerini SIEM'lere dışa aktarma | Microsoft Dokümanlar
description: Bu makalede, SIEM'lere sürekli güvenlik uyarıları ve önerileri nin nasıl ayarılanılabildiğini açıklamaktadır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: memildin
ms.openlocfilehash: 19fdcc7b590c6ad6873c7808ae26d218bbda7f5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158987"
---
# <a name="export-security-alerts-and-recommendations"></a>Güvenlik uyarılarını ve önerilerini dışarı aktarma

Azure Güvenlik Merkezi ayrıntılı güvenlik uyarıları ve öneriler oluşturur. Bunları portalda veya programlı araçlarla görüntüleyebilirsiniz. Ayrıca, bu bilgileri dışa aktarmanız veya ortamınızdaki diğer izleme araçlarına göndermeniz gerekebilir. 

Bu makalede, uyarı ve önerileri el ile veya sürekli bir şekilde dışa aktarmanıza olanak tanıyan araçlar kümesi açıklanmaktadır.

Bu araçları kullanarak şunları yapabilirsiniz:

* Log Analytics çalışma alanlarına sürekli olarak dışa aktarma
* Azure Etkinlik Hub'larına sürekli olarak dışa aktarma (üçüncü taraf SIEM'lerle tümleştirmeler için)
* CSV'ye aktarma (bir kerelik)


## <a name="setting-up-a-continuous-export"></a>Sürekli bir dışa aktarım ayarlama

Log Analytics çalışma alanına veya Azure Etkinlik Hub'larına sürekli bir dışa aktarım ayarlarken aşağıdaki adımlar gereklidir.

1. Güvenlik Merkezi'nin kenar çubuğundan **Fiyatlandırma & ayarlarını**seçin.

1. Veri dışa aktarma yapılandırmak istediğiniz belirli aboneliği seçin.
    
1. Bu abonelik için ayarlar sayfasının kenar çubuğundan **Sürekli Dışa Aktarma'yı**seçin.

    [Azure Güvenlik Merkezi'nde dışa aktarma seçenekleri ![](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) Burada dışa aktarma seçeneklerini görüyorsunuz. Kullanılabilir her dışa aktarma hedefi için bir sekme vardır. 

1. Dışa aktarmak istediğiniz veri türünü seçin ve her türdeki filtrelerden seçim yapın (örneğin, yalnızca yüksek önem uyarılarını dışa aktarın).

1. "Hedef dışa aktar" alanından, verilerin kaydedildiği yeri seçin. Veriler farklı bir abonelikte (örneğin Merkezi Olay Hub örneğinde veya merkezi bir Log Analytics çalışma alanında) bir hedefe kaydedilebilir.

1. **Kaydet**'e tıklayın.



## <a name="configuring-siem-integration-via-azure-event-hubs"></a>Azure Etkinlik Hub'ları aracılığıyla SIEM tümleştirmesi yapılandırma

Azure Etkinlik Hub'ları, akış verilerini programlı olarak tüketmek için mükemmel bir çözümdür. Azure Güvenlik Merkezi uyarıları ve önerileri için, üçüncü taraf bir SIEM ile tümleştirmenin tercih edilen yoludur.

> [!NOTE]
> Çoğu durumda izleme verilerini harici araçlara aktarmak için en etkili yöntem Azure Etkinlik Hub'larını kullanmaktır. [Bu makalede,](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) izleme verilerini farklı kaynaklardan bir Olay Hub'ına nasıl aktarabileceğiniz ve ayrıntılı kılavuza bağlantılar için kısa bir açıklama sağlanmaktadır.

> [!NOTE]
> Azure Etkinliği günlüğünü kullanarak bir SIEM'e daha önce Güvenlik Merkezi uyarıları verdiyseniz, aşağıdaki yordam bu metodolojinin yerini alır.

Dışa aktarılan veri türlerinin olay şemalarını görüntülemek için [Olay Hub olay şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin.


### <a name="to-integrate-with-a-siem"></a>Bir SIEM ile entegre etmek 

Seçtiğiniz Güvenlik Merkezi verilerini Azure Etkinlik Hub'larına sürekli olarak yapılandırdıktan sonra, SIEM'iniz için uygun bağlayıcıyı ayarlayabilirsiniz:

* **Azure Sentinel** - Azure Güvenlik Merkezi'ni kullanın, burada sunulan [veri bağlayıcısını](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) uyarır.
* **Splunk** - [Splunk için Azure Monitör Eklentisini](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) Kullanma
* **IBM QRadar** - [El ile yapılandırılan günlük kaynağını](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_dsm_guide_microsoft_azure_enable_event_hubs.html) kullanma
* **ArcSight** – [SmartConnector'u](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292) Kullan

Ayrıca, sürekli olarak aktarılan verileri otomatik olarak yapılandırılmış Etkinlik Hub'ınızdan Azure Veri Gezgini'ne taşımak istiyorsanız, [Olay Hub'ından Azure Veri](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub)Gezgini'ne veri alma yönergelerini kullanın.



## <a name="continuous-export-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanına sürekli dışa aktarma

Bir Log Analytics çalışma alanı içinde Azure Güvenlik Merkezi verilerini analiz etmek veya Güvenlik Merkezi ile birlikte Azure uyarıları kullanmak istiyorsanız, Log Analytics çalışma alanınıza sürekli dışa aktarma yı ayarlayın.

Bir Log Analytics çalışma alanına dışa aktarmak için, çalışma alanınızda Security Center'ın Log Analytics çözümlerini etkinleştirmiş olmalısınız. Azure portalını kullanıyorsanız, sürekli dışa aktarmayı etkinleştirdiğinizde Security Center'ın ücretsiz katman çözümü otomatik olarak etkinleştirilir. Ancak, sürekli dışa aktarma ayarlarınızı programlı olarak yapılandırıyorsanız, **Fiyatlandırma & ayarlarından**gerekli çalışma alanı için ücretsiz veya standart fiyatlandırma katmanını el ile seçmeniz gerekir.  

### <a name="log-analytics-tables-and-schemas"></a>Günlük Analytics tabloları ve şemaları

Güvenlik uyarıları ve öneriler sırasıyla *SecurityAlert* ve *SecurityRecommendations* tablolarında depolanır. Bu tabloları içeren Log Analytics çözümünün adı, ücretsiz veya standart katmanda (bkz. [fiyatlandırma):](security-center-pricing.md)Security('Security and Audit') veya SecurityCenterFree'de olup olmadığınıza bağlıdır.

![Log Analytics'teki *SecurityAlert* tablosu](./media/continuous-export/log-analytics-securityalert-solution.png)

Dışa aktarılan veri türlerinin olay şemalarını görüntülemek [için, Günlük Analizi tablosu şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin.

###  <a name="view-exported-security-alerts-and-recommendations-in-azure-monitor"></a>Azure Monitör'de dışa aktarılan güvenlik uyarılarını ve önerilerini görüntüleme

Bazı durumlarda, [Azure Monitor'da](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)dışa aktarılan Güvenlik Uyarılarını ve/veya önerilerini görüntülemeyi seçebilirsiniz. 

Azure Monitor, Günlük Analizi çalışma alanı sorgularını temel alan Tanıgünlüğü, Metrik uyarıları ve özel uyarılar gibi çeşitli Azure uyarıları için birleşik bir uyarı deneyimi sağlar.

Azure Monitor'daki Güvenlik Merkezi'nden gelen uyarıları ve önerileri görüntülemek için, Günlük Analizi sorgularına (Günlük Uyarısı) dayalı bir Uyarı kuralı nı yapılandırın:

1. Azure Monitor'un **Uyarılar** sayfasından **Yeni uyarı kuralını**tıklatın.

    ![Azure Monitor'un uyarılar sayfası](./media/continuous-export/azure-monitor-alerts.png)

1. Kural oluştur sayfasında, yeni kuralınızı yapılandırın [(Azure Monitor'da günlük uyarı kuralını](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)yapılandırdığınız gibi):

    * **Kaynak**için, güvenlik uyarıları ve önerileri dışa aktardığınız Log Analytics çalışma alanını seçin.

    * **Durum**için, **Özel günlük arama**seçin. Görünen sayfada, sorguyu, geri arama dönemini ve sıklık dönemini yapılandırın. Arama sorgusunda, Log Analytics özelliğine sürekli dışa aktarmayı etkinleştirirken Güvenlik Merkezi'nin sürekli olarak dışa aktardığı veri türlerini sorgulamak için *SecurityAlert* veya *SecurityRecommendation* yazabilirsiniz. 
    
    * İsteğe bağlı olarak, tetiklemek istediğiniz [Eylem Grubu'nu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) yapılandırın. Eylem grupları e-posta gönderme, ITSM biletleri, WebHooks ve daha fazlasını tetikleyebilir.
    ![Azure Monitör uyarı kuralı](./media/continuous-export/azure-monitor-alert-rule.png)

Artık Azure Monitor uyarılarında (yapılandırmanıza bağlı olarak) yeni Azure Güvenlik Merkezi uyarıları veya önerileri görürsünüz ve bir eylem grubunun otomatik olarak tetiklendiğini (sağlanırsa).

## <a name="manual-one-time-export-of-security-alerts"></a>Güvenlik uyarılarının manuel bir kerelik dışa aktarımı

Uyarılar veya öneriler için bir CSV raporu indirmek için **Güvenlik uyarıları** veya **Öneriler** sayfasını açın ve **CSV rapor larını indir** düğmesini tıklatın.

[![Uyarıları csv dosyası olarak indirme](media/continuous-export/download-alerts-csv.png)](media/continuous-export/download-alerts-csv.png#lightbox)

> [!NOTE]
> Bu raporlar, şu anda seçili aboneliklerden kaynaklar için uyarılar ve öneriler içerir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, önerilerinizi ve uyarılarınızın sürekli dışa aktarımını nasıl yapılandırabileceğinizi öğrendiniz. Ayrıca, uyarı verilerinizi CSV dosyası olarak nasıl indirdiğinizi de öğrendiniz. 

İlgili materyaller için aşağıdaki belgelere bakın: 

- [Azure Etkinlik Hub'ları belgeleri](https://docs.microsoft.com/azure/event-hubs/)
- [Azure Sentinel belgeleri](https://docs.microsoft.com/azure/sentinel/)
- [Azure İzleyici belgeleri](https://docs.microsoft.com/azure/azure-monitor/)
- [İş akışı otomasyonu ve sürekli ihracat veri türleri şemaları](https://aka.ms/ASCAutomationSchemas)
