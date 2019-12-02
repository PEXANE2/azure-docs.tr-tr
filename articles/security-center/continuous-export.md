---
title: Azure Güvenlik Merkezi uyarılarını ve önerilerini Sıems 'ye aktarma | Microsoft Docs
description: Bu makalede, güvenlik uyarılarının ve önerilerin Sıems 'ye sürekli olarak dışarı aktarılması nasıl ayarlanacağı açıklanır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: f994f4ec6d41fa0aab37e36d713eaefb22e85b28
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2019
ms.locfileid: "74665090"
---
# <a name="export-security-alerts-and-recommendations-preview"></a>Güvenlik uyarılarını ve önerilerini dışarı aktarma (Önizleme)

Azure Güvenlik Merkezi, ayrıntılı güvenlik uyarıları ve önerileri oluşturur. Bunları portalda veya programlı araçlar aracılığıyla görüntüleyebilirsiniz. Bu bilgileri dışarı aktarmanız veya ortamınızdaki diğer izleme araçlarına göndermeniz de gerekebilir. 

Bu makalede, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmaya izin veren (Önizleme) araçları açıklanmaktadır.

Bu araçları kullanarak şunları yapabilirsiniz:

* Ayrıntılı raporları CSV olarak oluşturma
* Log Analytics çalışma alanlarına aktar
* Azure Event Hubs 'e aktarma (üçüncü taraf Sıems ile Tümleştirmeler için)

## <a name="setting-up-a-continuous-export"></a>Sürekli dışarı aktarma ayarlama

1. Güvenlik Merkezi 'nin kenar çubuğundan **fiyatlandırma & ayarları**' na tıklayın.

1. Veri dışarı aktarmayı yapılandırmak istediğiniz belirli aboneliği seçin.
    
1. Bu aboneliğin Ayarlar sayfasının kenar çubuğundan **sürekli dışarı aktar (Önizleme)** seçeneğini belirleyin.

    [Azure Güvenlik Merkezi 'Nde dışarı aktarma seçenekleri![](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) Burada dışa aktarma seçeneklerini görürsünüz. Kullanılabilir her dışa aktarma hedefi için bir sekme vardır. 

1. Dışarı aktarmak istediğiniz veri türünü seçin ve her bir türdeki filtrelerden birini seçin (örneğin, yalnızca yüksek önem derecesine sahip uyarıları dışarı aktarın).

1. "Dışarı aktarma hedefi" alanından, verilerin kaydedilmesini istediğiniz yeri seçin. Veriler farklı bir abonelikteki hedefe kaydedilebilir (örneğin, merkezi bir olay hub 'ı örneği veya merkezi bir Log Analytics çalışma alanı).

1. **Kaydet** düğmesine tıklayın.

## <a name="continuous-export-through-azure-event-hubs"></a>Azure Event Hubs aracılığıyla sürekli dışarı aktarma  

> [!NOTE]
> Çoğu durumda, izleme verilerinin dış araçlara akışını sağlamak için en etkili yöntem Azure Event Hubs kullanmaktır. [Bu makalede](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) , farklı kaynaklardan bir olay hub 'ına izleme verileri akışını ve ayrıntılı kılavuza bağlantıları nasıl alabileceğiniz hakkında kısa bir açıklama sunulmaktadır.

> [!NOTE]
> Daha önce Azure etkinlik günlüğü 'nü kullanarak güvenlik merkezi uyarılarını bir SıEM 'e aktardıysanız, aşağıdaki yordam bu metodolojide yerini alır.

### <a name="to-integrate-with-a-siem"></a>SıEM ile tümleştirme için 

Seçtiğiniz güvenlik merkezi verilerinizin sürekli olarak dışarı aktarılmasını Azure Event Hubs yapılandırdıktan sonra, aşağıdaki yönergeleri izleyerek SıEM 'niz üzerinde uygun bağlayıcıyı ayarlayabilirsiniz.

[Bu sayfadan](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/?cdn=disable) SIEM 'inizle ilgili yönergeleri izleyin ve ilgili bağlayıcıyı kullanın:

* **Splunk** - [splunk Için Azure izleyici eklentisini](https://splunkbase.splunk.com/app/3534/) kullanma
* **IBM QRadar** - [el ile yapılandırılmış bir günlük kaynağı](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_logsource_microsoft_azure_event_hubs.html) kullanın
* **Arcgörüş** – [smartconnector](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292) kullanın

**Azure Sentinel**kullanıyorsanız, burada sunulan yerel Azure Güvenlik Merkezi uyarıları [veri bağlayıcısını](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) kullanın.

Ayrıca, sürekli olarak dışarıya aktarılmış verileri otomatik olarak yapılandırılmış olay hub 'ından Azure Veri Gezgini taşımak istiyorsanız, [Olay Hub 'ından azure Veri Gezgini veri](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub)alma içindeki yönergeleri kullanın.


## <a name="continuous-export-to-log-analytics-workspace"></a>Log Analytics çalışma alanına sürekli dışarı aktarma

Log Analytics çalışma alanına aktarmak için, güvenlik merkezi 'nin ücretsiz veya standart katmanı çalışma alanınızda etkinleştirilmiş Log Analytics çözümleri olmalıdır. Azure portal kullanıyorsanız, sürekli dışarı aktarmayı etkinleştirdiğinizde güvenlik merkezi ücretsiz katman çözümü otomatik olarak etkinleştirilir. Ancak, sürekli dışa aktarma ayarlarınızı programlama yoluyla yapılandırıyorsanız, gerekli çalışma alanı için **fiyatlandırma & ayarları**içinden ücretsiz veya standart fiyatlandırma katmanını el ile seçmeniz gerekir.  

Güvenlik uyarıları ve önerileri sırasıyla *Securityalert* ve *securityöneriler* tablolarında depolanır. Bu tabloları içeren Log Analytics çözümünün adı, ücretsiz veya Standart katmanda olup olmadığına bağlıdır (bkz. [fiyatlandırma](security-center-pricing.md)): güvenlik veya securitycenterfree.

![Log Analytics içindeki * SecurityAlert * tablosu](./media/continuous-export/log-analytics-securityalert-solution.png)

###  <a name="view-exported-security-alerts-and-recommendations-in-azure-monitor"></a>Azure Izleyici 'de, verilmiş güvenlik uyarılarını ve önerilerini görüntüleme

Bazı durumlarda, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)'de, verilmiş güvenlik uyarılarını ve/veya önerilerini görüntülemeyi tercih edebilirsiniz. 

Azure Izleyici, Log Analytics çalışma alanı sorgularını temel alan tanılama günlüğü, ölçüm uyarıları ve özel uyarılar dahil olmak üzere çeşitli Azure uyarıları için Birleşik bir uyarı deneyimi sağlar.

Azure Izleyici 'de Güvenlik Merkezi 'ndeki uyarıları ve önerileri görüntülemek için Log Analytics sorgularına göre bir uyarı kuralı yapılandırın (günlük uyarısı):

1. Azure Izleyici **uyarıları** sayfasında **Yeni uyarı kuralı**' na tıklayın.

    ![Azure Izleyici 'nin uyarılar sayfası](./media/continuous-export/azure-monitor-alerts.png)

1. Kural Oluştur sayfasında, Yeni kuralınızı yapılandırın ( [Azure izleyici 'de bir günlük uyarısı kuralını](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)yapılandırdığınız şekilde):

    * **Kaynak**için güvenlik uyarılarını ve önerilerini verdiğiniz Log Analytics çalışma alanını seçin.

    * **Koşul**için **özel günlük araması**' nı seçin. Görüntülenen sayfada sorguyu, geriye doğru dönemi ve sıklık dönemini yapılandırın. Log Analytics özelliğini sürekli dışarı aktarmayı etkinleştirerek, arama sorgusunda, güvenlik merkezi 'nin sürekli olarak dışa aktardığı veri türlerini sorgulamak için *Securityalert* veya *securityönerisi* yazabilirsiniz. 
    
    * İsteğe bağlı olarak, tetiklemek istediğiniz [Eylem grubunu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) yapılandırın. Eylem grupları e-posta gönderme, ıTSM biletleri, Web kancaları ve daha fazlasını tetikleyebilir.
    Azure Izleyici uyarı kuralını ![](./media/continuous-export/azure-monitor-alert-rule.png)

Artık, Azure Izleyici uyarıları 'ndaki yeni Azure Güvenlik Merkezi uyarılarını veya önerilerini (yapılandırmanıza bağlı olarak), bir eylem grubunun otomatik olarak tetiklemesini (sağlanmışsa) görürsünüz.

## <a name="manual-one-time-export-of-security-alerts"></a>Güvenlik uyarılarını el ile tek seferlik dışarı aktarma

Uyarılar veya öneriler için bir CSV raporu indirmek için, **güvenlik uyarıları** veya **öneriler** sayfasını açın ve **CSV raporu indir (Önizleme)** düğmesini tıklatın.

[Uyarı verilerini CSV dosyası olarak Indirme ![](media/continuous-export/download-alerts-csv.png)](media/continuous-export/download-alerts-csv.png#lightbox)

> [!NOTE]
> Bu raporlar, Azure portalındaki Dizin + abonelik filtresindeki Şu anda seçili olan aboneliklerdeki kaynaklara yönelik uyarıları ve önerileri içerir: Dizin + abonelik seçme filtresini ![](./media/continuous-export/filter-for-export-csv.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, önerilerinizi ve uyarılarınızı sürekli dışarı aktarmaların nasıl yapılandırılacağını öğrendiniz. Ayrıca, uyarı verilerinizi bir CSV dosyası olarak nasıl indirileceğini öğrendiniz. 

İlgili malzemeler için aşağıdaki belgelere bakın: 

- [Azure Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/)
- [Azure Sentinel belgeleri](https://docs.microsoft.com/azure/sentinel/)
- [Azure İzleyici belgeleri](https://docs.microsoft.com/azure/azure-monitor/)