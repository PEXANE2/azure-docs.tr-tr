---
title: Azure Güvenlik Merkezi uyarılarını ve önerilerini Sıems 'ye aktarma | Microsoft Docs
description: Bu makalede, güvenlik uyarılarının ve önerilerin Sıems 'ye sürekli olarak dışarı aktarılması nasıl ayarlanacağı açıklanır
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: memildin
ms.openlocfilehash: 4d5cff416c1ac54e54d06e8def121db65bb7d191
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89433950"
---
# <a name="export-security-alerts-and-recommendations"></a>Güvenlik uyarılarını ve önerilerini dışarı aktarma

Azure Güvenlik Merkezi, ayrıntılı güvenlik uyarıları ve önerileri oluşturur. Bunları portalda veya programlı araçlar aracılığıyla görüntüleyebilirsiniz. Bu bilgileri dışarı aktarmanız veya ortamınızdaki diğer izleme araçlarına göndermeniz de gerekebilir. 

Bu makalede, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmaya izin veren araç kümesi açıklanır.

Bu araçları kullanarak şunları yapabilirsiniz:

* Log Analytics çalışma alanlarına sürekli dışa aktarma
* Azure Event Hubs sürekli olarak dışarı aktarma (üçüncü taraf Sıems ile Tümleştirmeler için)
* CSV 'ye aktar (bir kez)



## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir|
|Fiyat|Ücretsiz katmanı|
|Gerekli roller ve izinler:|Kaynak grubunda (veya **sahip**) **Güvenlik Yöneticisi rolü**<br>Ayrıca hedef kaynak için yazma izinlerine sahip olmalıdır|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov<br>![Yes](./media/icons/yes-icon.png) Çin gov (Olay Hub 'ına), diğer gov|
|||



## <a name="set-up-a-continuous-export"></a>Sürekli dışarı aktarma ayarlama

Aşağıdaki adımlar Log Analytics çalışma alanına veya Azure Event Hubs sürekli bir dışarı aktarma işlemi yapılıp yapılmayacağını belirtir.

1. Güvenlik Merkezi 'nin kenar çubuğundan **fiyatlandırma & ayarları**' nı seçin.

1. Veri dışarı aktarmayı yapılandırmak istediğiniz belirli aboneliği seçin.
    
1. Bu aboneliğin Ayarlar sayfasının kenar çubuğundan **sürekli dışarı aktarma**' yı seçin.

    [ ![ Azure Güvenlik Merkezi 'nde dışarı aktarma seçenekleri](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) burada dışa aktarma seçeneklerini görürsünüz. Kullanılabilir her dışa aktarma hedefi için bir sekme vardır. 

1. Dışarı aktarmak istediğiniz veri türünü seçin ve her bir türdeki filtrelerden birini seçin (örneğin, yalnızca yüksek önem derecesine sahip uyarıları dışarı aktarın).

1. İsteğe bağlı olarak, seçiminiz bu dört önerinden birini içeriyorsa, güvenlik açığı değerlendirmesi bulgularını bunlarla birlikte dahil edebilirsiniz:

    - SQL veritabanlarındaki güvenlik açığı değerlendirmesi bulguları düzeltildi
    - Makinelerdeki SQL sunucularınızda bulunan güvenlik açığı değerlendirmesi (Önizleme) düzeltilmelidir.
    - Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir (Qualys tarafından desteklenir)
    - Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir

    Bu önerilerin bulgularını dahil etmek için **güvenlik bulgularını dahil et** seçeneğini etkinleştirin.

    :::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Sürekli dışa aktarma yapılandırmasında güvenlik bulgularını dahil et" :::


1. "Dışarı aktarma hedefi" alanından, verilerin kaydedilmesini istediğiniz yeri seçin. Veriler farklı bir abonelikteki hedefe kaydedilebilir (örneğin, merkezi bir olay hub 'ı örneği veya merkezi bir Log Analytics çalışma alanı).

1. **Kaydet**’i seçin.


## <a name="set-up-continuous-export-via-the-rest-api"></a>REST API aracılığıyla sürekli dışarı aktarmayı ayarlama

Sürekli dışa aktarma özelliği Azure Güvenlik Merkezi tahmin [API 'si](https://docs.microsoft.com/rest/api/securitycenter/automations)aracılığıyla yapılandırılabilir ve yönetilebilir. Aşağıdaki olası hedeflerin herhangi birine dışarı aktarmak için bu API 'yi kullanarak akışlarını otomatikleştirin oluşturun veya güncelleştirin:

- Azure Event Hub
- Log Analytics çalışma alanı
- Azure Logic Apps 

API, Azure portal kullanılamayan ek işlevler sağlar, örneğin:

* **Daha büyük birim** -API, tek bir abonelikte birden çok dışa aktarma yapılandırması oluşturmanıza olanak sağlar. Güvenlik Merkezi 'nin Portal Kullanıcı arabirimindeki **sürekli dışarı aktarma** sayfası, her abonelik için yalnızca bir dışarı aktarma yapılandırmasını destekler.

* **Ek özellikler** -API, Kullanıcı arabiriminde görüntülenmeyen ek parametreler sunar. Örneğin, Otomasyon kaynağına Etiketler ekleyebilir ve ayrıca, güvenlik merkezi 'nin Portal Kullanıcı arabirimindeki **sürekli dışarı aktarma** sayfasında sunulmadan daha geniş bir uyarı ve öneri özellikleri kümesine göre dışarı aktarma işlemini tanımlayabilirsiniz.

* **Daha odaklı kapsam** -API, dışa aktarma yapılandırmalarınızın kapsamı için daha ayrıntılı bir düzey sağlar. API ile bir dışarı aktarma tanımlarken, bunu kaynak grubu düzeyinde yapabilirsiniz. Güvenlik Merkezi 'nin Portal Kullanıcı arabiriminde **sürekli dışarı aktarma** sayfasını kullanıyorsanız, bunu abonelik düzeyinde tanımlamanız gerekir.

    > [!TIP]
    > API kullanarak birden çok dışarı aktarma yapılandırması ayarladıysanız veya yalnızca API parametreleri kullandıysanız, bu ek özellikler Güvenlik Merkezi Kullanıcı arabiriminde gösterilmez. Bunun yerine, diğer yapılandırmaların var olduğunu bildiren bir başlık görüntülenir.

[REST API belgelerindeki](https://docs.microsoft.com/rest/api/securitycenter/automations)akışlarını otomatikleştirin API 'si hakkında daha fazla bilgi edinin.



## <a name="configure-siem-integration-via-azure-event-hubs"></a>Azure Event Hubs ile SıEM tümleştirmesini yapılandırma

Azure Event Hubs, tüm akış verilerini kullanan program aracılığıyla için harika bir çözümdür. Azure Güvenlik Merkezi uyarıları ve önerileri için, üçüncü taraf SıEM ile tümleştirme için tercih edilen bir yoldur.

> [!NOTE]
> Çoğu durumda, izleme verilerinin dış araçlara akışını sağlamak için en etkili yöntem Azure Event Hubs kullanmaktır. [Bu makalede](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) , farklı kaynaklardan bir olay hub 'ına izleme verileri akışını ve ayrıntılı kılavuza bağlantıları nasıl alabileceğiniz hakkında kısa bir açıklama sunulmaktadır.

> [!NOTE]
> Daha önce Azure etkinlik günlüğü 'nü kullanarak güvenlik merkezi uyarılarını bir SıEM 'e aktardıysanız, aşağıdaki yordam bu metodolojide yerini alır.

Aktarılmış veri türlerinin olay şemalarını görüntülemek için, [Olay Hub 'ı olay şemaları](https://aka.ms/ASCAutomationSchemas)' nı ziyaret edin.


### <a name="to-integrate-with-a-siem"></a>SıEM ile tümleştirme için 

Seçtiğiniz güvenlik merkezi verilerinizin sürekli olarak dışarı aktarılmasını Azure Event Hubs yapılandırdıktan sonra SıEM 'niz için uygun bağlayıcıyı ayarlayabilirsiniz:

* **Azure Sentinel** -burada sunulan yerel Azure Güvenlik Merkezi uyarıları [veri bağlayıcısını](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) kullanın.
* **Splunk** - [splunk Için Azure izleyici eklentisini](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) kullanma
* **IBM QRadar** - [el ile yapılandırılmış bir günlük kaynağı](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_dsm_guide_microsoft_azure_enable_event_hubs.html) kullanın
* **Arcgörüş** – [smartconnector](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292) kullanın

Ayrıca, sürekli olarak dışarıya aktarılmış verileri otomatik olarak yapılandırılmış olay hub 'ından Azure Veri Gezgini taşımak istiyorsanız, [Olay Hub 'ından azure Veri Gezgini veri](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub)alma içindeki yönergeleri kullanın.



## <a name="continuous-export-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanına sürekli dışarı aktarma

Azure Güvenlik Merkezi verilerini bir Log Analytics çalışma alanı içinde çözümlemek veya Azure uyarılarını Güvenlik Merkezi ile birlikte kullanmak istiyorsanız, Log Analytics çalışma alanınıza sürekli dışarı aktarma ayarlayın.

Log Analytics çalışma alanına aktarmak için, güvenlik merkezi 'nin çalışma alanınızda etkin olan Log Analytics çözümlerinin etkinleştirilmiş olması gerekir. Azure portal kullanıyorsanız, sürekli dışarı aktarmayı etkinleştirdiğinizde güvenlik merkezi 'nin ücretsiz katman çözümü otomatik olarak etkinleştirilir. Ancak, sürekli dışa aktarma ayarlarınızı programlama yoluyla yapılandırıyorsanız, gerekli çalışma alanı için **fiyatlandırma & ayarları**içinden ücretsiz veya standart fiyatlandırma katmanını el ile seçmeniz gerekir.  

### <a name="log-analytics-tables-and-schemas"></a>Log Analytics tabloları ve şemaları

Güvenlik uyarıları ve önerileri sırasıyla *Securityalert* ve *securityöneriler* tablolarında depolanır. Bu tabloları içeren Log Analytics çözümünün adı, ücretsiz veya Standart katmanda olup olmadığına bağlıdır (bkz. [fiyatlandırma](security-center-pricing.md)): güvenlik (' güvenlik ve denetim ') veya securitycenterfree.

![Log Analytics içindeki * SecurityAlert * tablosu](./media/continuous-export/log-analytics-securityalert-solution.png)

Aktarılmış veri türlerinin olay şemalarını görüntülemek için [Log Analytics tablo şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin.

###  <a name="view-exported-security-alerts-and-recommendations-in-azure-monitor"></a>Azure Izleyici 'de, verilmiş güvenlik uyarılarını ve önerilerini görüntüleme

Bazı durumlarda, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)'de, verilmiş güvenlik uyarılarını ve/veya önerilerini görüntülemeyi tercih edebilirsiniz. 

Azure Izleyici, Log Analytics çalışma alanı sorgularını temel alan tanılama günlüğü, ölçüm uyarıları ve özel uyarılar dahil olmak üzere çeşitli Azure uyarıları için Birleşik bir uyarı deneyimi sağlar.

Azure Izleyici 'de Güvenlik Merkezi 'ndeki uyarıları ve önerileri görüntülemek için Log Analytics sorgularına göre bir uyarı kuralı yapılandırın (günlük uyarısı):

1. Azure Izleyici **uyarıları** sayfasında **Yeni uyarı kuralı**' nı seçin.

    ![Azure Izleyici 'nin uyarılar sayfası](./media/continuous-export/azure-monitor-alerts.png)

1. Kural Oluştur sayfasında, Yeni kuralınızı yapılandırın ( [Azure izleyici 'de bir günlük uyarısı kuralını](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)yapılandırdığınız şekilde):

    * **Kaynak**için güvenlik uyarılarını ve önerilerini verdiğiniz Log Analytics çalışma alanını seçin.

    * **Koşul**için **özel günlük araması**' nı seçin. Görüntülenen sayfada sorguyu, geriye doğru dönemi ve sıklık dönemini yapılandırın. Log Analytics özelliğini sürekli dışarı aktarmayı etkinleştirerek, arama sorgusunda, güvenlik merkezi 'nin sürekli olarak dışa aktardığı veri türlerini sorgulamak için *Securityalert* veya *securityönerisi* yazabilirsiniz. 
    
    * İsteğe bağlı olarak, tetiklemek istediğiniz [Eylem grubunu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) yapılandırın. Eylem grupları e-posta gönderme, ıTSM biletleri, Web kancaları ve daha fazlasını tetikleyebilir.
    ![Azure Izleyici uyarı kuralı](./media/continuous-export/azure-monitor-alert-rule.png)

Artık, Azure Izleyici uyarıları 'ndaki yeni Azure Güvenlik Merkezi uyarılarını veya önerilerini (yapılandırmanıza bağlı olarak), bir eylem grubunun otomatik olarak tetiklemesini (sağlanmışsa) görürsünüz.

## <a name="manual-one-time-export-of-security-alerts"></a>Güvenlik uyarılarını el ile tek seferlik dışarı aktarma

Uyarılar veya öneriler için bir CSV raporu indirmek için, **güvenlik uyarıları** veya **öneriler** sayfasını açın ve **CSV raporu indir** düğmesini seçin.

[![Uyarı verilerini CSV dosyası olarak indir](media/continuous-export/download-alerts-csv.png)](media/continuous-export/download-alerts-csv.png#lightbox)

> [!NOTE]
> Bu raporlar Şu anda seçili olan aboneliklerdeki kaynaklara yönelik uyarıları ve önerileri içerir.



## <a name="faq---continuous-export"></a>SSS-sürekli dışarı aktarma

### <a name="what-are-the-costs-involved-in-exporting-data"></a>Verileri dışarı aktarma ile ilgili maliyetler nelerdir?

Sürekli dışarı aktarmayı etkinleştirme maliyeti yoktur. Yapılandırmaya bağlı olarak, Log Analytics çalışma alanınızda verilerin alımı ve saklanması için maliyetler tahakkuk edebilir. 

[Log Analytics çalışma alanı fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgi edinin.

[Azure Olay Hub 'ı fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/)hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, önerilerinizi ve uyarılarınızı sürekli dışarı aktarmaların nasıl yapılandırılacağını öğrendiniz. Ayrıca, uyarı verilerinizi bir CSV dosyası olarak nasıl indirileceğini öğrendiniz. 

İlgili malzemeler için aşağıdaki belgelere bakın: 

- [Azure Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/)
- [Azure Sentinel belgeleri](https://docs.microsoft.com/azure/sentinel/)
- [Azure İzleyici belgeleri](https://docs.microsoft.com/azure/azure-monitor/)
- [İş akışı otomasyonu ve sürekli dışarı aktarma veri türleri şemaları](https://aka.ms/ASCAutomationSchemas)
