---
title: Azure izleme verilerini etkinlik merkezine aktarın
description: Verileri iş ortağı SIEM veya analiz aracına dönüştürmek için Azure izleme verilerinizi bir etkinlik merkezine nasıl aktarılacınız öğrenin.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 32bc90cc069ac82641c3aa7692c900c60db7ba87
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733093"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub"></a>Azure izleme verilerini bir olay hub'ına gönderme
Azure Monitor, Azure'daki uygulamalar ve hizmetler için, diğer bulutlarda ve şirket içinde tam bir yığın izleme çözümü sağlar. Bu verileri çözümlemek ve farklı izleme senaryoları için kullanmak için Azure Monitor'u kullanmanın yanı sıra, ortamınızdaki diğer izleme araçlarına da göndermeniz gerekebilir. Çoğu durumda izleme verilerini harici araçlara aktarmak için en etkili yöntem [Azure Etkinlik Hub'larını](/azure/event-hubs/)kullanmaktır. Bu makalede, izleme verilerini farklı kaynaklardan bir olay merkezine nasıl aktarabileceğiniz ve ayrıntılı kılavuza bağlantılar için kısa bir açıklama sağlanmaktadır.


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir veri kaynağı için akışı yapılandırmadan [önce, bir Olay Hub'ları ad alanı ve olay hub'ı oluşturmanız](../../event-hubs/event-hubs-create.md)gerekir. Bu ad alanı ve olay hub'ı, tüm izleme verilerinizin hedefidir. Olay Hub'ları ad alanı, aynı erişim ilkesini paylaşan olay hub'larının mantıksal bir gruplandırmasIdır, tıpkı bir depolama hesabının bu depolama hesabında tek tek blob'ları olması gibi. İzleme verilerini akışı için kullandığınız olay hub'ları ad alanı ve olay hub'ları hakkında aşağıdaki ayrıntıları göz önünde bulundurun:

* Üretim birimlerinin sayısı, olay hub'larınız için üretim ölçeğini artırmanıza olanak tanır. Genellikle yalnızca bir iş birimi gereklidir. Günlük kullanımınız arttıkça ölçeklendirmeniz gerekiyorsa, ad alanı için üretim birimi sayısını el ile artırabilir veya otomatik enflasyonu etkinleştirebilirsiniz.
* Bölüm sayısı, tüketimi birçok tüketici arasında paralelleştirmenize olanak tanır. Tek bir bölüm, saniyede 20 MBps'ye kadar veya yaklaşık 20.000 iletidestekleyebilir. Verileri tüketen araca bağlı olarak, birden çok bölümden tüketenleri destekleyebilir veya desteklemeyebilir. Ayarlayacak bölüm sayısı hakkında emin değilseniz, dört bölüm başlamak için makul.
* Olay merkezinizdeki ileti bekletme olayını en az 7 gün olarak ayarlarsınız. Tüketen aracınız bir günden uzun süre batılırsa, bu, aracın 7 güne kadar olan olaylar için kaldığı yerden devam etmesini sağlar.
* Olay merkeziniz için varsayılan tüketici grubunu kullanmalısınız. Aynı olay merkezinden aynı verileri iki farklı araç tüketmeyi planlamadığınız sürece, başka tüketici grupları oluşturmanıza veya ayrı bir tüketici grubu kullanmaya gerek yoktur.
* Azure Etkinliği günlüğü için bir Etkinlik Hub'ları ad alanı seçersiniz ve Azure Monitor bu ad alanı içinde _insights-logs-operational-logs_adı verilen bir olay hub'ı oluşturur. Diğer günlük türleri için, varolan bir olay hub'ı seçebilir veya Azure Monitor'un günlük kategorisi başına bir etkinlik hub'ı oluşturması gerekir.
* Giden bağlantı noktası 5671 ve 5672 genellikle bilgisayarda veya Olay hub'ından VNET tüketen veri açılmalıdır.

## <a name="monitoring-data-available"></a>Mevcut verileri izleme
[Azure Monitor için izleme verisi kaynakları,](data-sources.md) Azure uygulamaları için farklı veri katmanlarını ve her biri için kullanılabilir izleme verisi türlerini açıklar. Aşağıdaki tabloda bu katmanların her biri ve bu verilerin bir olay hub'ına nasıl aktarılabildiğinin açıklaması listelenir. Daha fazla ayrıntı için sağlanan bağlantıları izleyin.

| Katman | Veriler | Yöntem |
|:---|:---|:---|
| [Azure kiracısı](data-sources.md#azure-tenant) | Azure Active Directory denetim günlükleri | AAD kiracınızda kiracı tanılama ayarını yapılandırın. Bkz. Öğretici: Ayrıntılar için [Azure Etkin Dizin günlüğünü bir Azure etkinlik merkezine](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) aktarın. |
| [Azure aboneliği](data-sources.md#azure-subscription) | Azure Etkinlik Günlüğü | Etkinlik Günlüğü olaylarını Olay Hub'larına aktarmak için bir günlük profili oluşturun.  Ayrıntılar için Azure Platformu Akış [günlüğe](resource-logs-stream-event-hubs.md) bakın. |
| [Azure kaynakları](data-sources.md#azure-resources) | Platform ölçümleri<br> Kaynak günlükleri |Her iki veri türü de kaynak tanılama ayarı kullanılarak bir olay hub'ına gönderilir. Ayrıntılar için [Azure kaynak günlüğe](resource-logs-stream-event-hubs.md) aktarın. |
| [İşletim sistemi (misafir)](data-sources.md#operating-system-guest) | Azure sanal makineleri | Azure'daki Windows ve Linux sanal makinelerinde [Azure Tanılama Uzantısı'nı](diagnostics-extension-overview.md) yükleyin. Windows VM'ler hakkındaki ayrıntılar için [Olay Hub'larını kullanarak Azure Tanılama verilerini sıcak yolda akışına](diagnostics-extension-stream-event-hubs.md) bakın ve Linux SANAL'ları hakkındaki ayrıntıları kaydetmek [için ölçümleri ve günlükleri izlemek için Linux Tanı uzantısını kullanın.](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings) |
| [Uygulama kodu](data-sources.md#application-code) | Application Insights | Application Insights, olay merkezlerine veri akışı için doğrudan bir yöntem sağlamaz. Application Insights verilerinin bir depolama hesabına [sürekli dışa aktarılmasını ayarlayabilir](../../azure-monitor/app/export-telemetry.md) ve ardından verileri [Logic App ile Manuel akışta](#manual-streaming-with-logic-app)açıklandığı gibi bir etkinlik merkezine göndermek için bir Mantık Uygulaması kullanabilirsiniz. |

## <a name="manual-streaming-with-logic-app"></a>Mantık Uygulaması ile manuel akış
Doğrudan bir olay hub'ına aktaramadığınız veriler için Azure depolama alanına yazabilir ve ardından [blob depolamadan veri çeken](../../connectors/connectors-create-api-azureblobstorage.md#add-action) ve olay merkezine ileti olarak [iten](../../connectors/connectors-create-api-azure-event-hubs.md#add-action)zaman tetiklenmiş bir Mantık Uygulaması kullanabilirsiniz. 


## <a name="partner-tools-with-azure-monitor-integration"></a>Azure Monitor entegrasyonu ile iş ortağı araçları

Azure Monitor ile izleme verilerinizi bir etkinlik merkezine yönlendirmeniz, harici SIEM ve izleme araçlarıyla kolayca entegre etmenizi sağlar. Azure Monitor tümleştirmesi ile birlikte araçlara örnek olarak aşağıdakiler verilebilir:

| Araç | Azure'da barındırılan | Açıklama |
|:---|:---| :---|
|  IBM QRadar | Hayır | Microsoft Azure DSM ve Microsoft Azure Etkinlik Hub Protokolü [IBM destek web sitesinden](https://www.ibm.com/support)indirilebilir. [QRadar DSM yapılandırması](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0)nda Azure ile tümleştirme hakkında daha fazla bilgi edinebilirsiniz. |
| Splunk | Hayır | [Splunk için Azure Monitör Eklentisi,](https://splunkbase.splunk.com/app/3534/) Splunkbase'de bulunan açık kaynak kodlu bir projedir. Belgeler, Azure [Monitor Addon For Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk)adresinde mevcuttur.<br><br> Splunk örneğinize bir eklenti yükleyemiyorsanız, örneğin bir proxy kullanıyorsanız veya Splunk Cloud'da çalışıyorsanız, bu olayları olay merkezindeki yeni iletiler tarafından tetiklenen [Azure İşlevi Splunk Için Splunk İşletmesini](https://github.com/Microsoft/AzureFunctionforSplunkVS)kullanarak Splunk HTTP Olay Toplayıcısı'na iletebilirsiniz. |
| SumoLogic | Hayır | Etkinlik hub'ından Bir Etkinlik Hub'ından veri tüketecek SumoLogic'i ayarlama yönergeleri, [Etkinlik Hub'ından Azure Denetim Uygulaması için Bilgi İşlem Lerini Topla'da](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)kullanılabilir. |
| ArcSight | Hayır | ArcSight Azure Etkinlik Hub akıllı konektörü, [ArcSight akıllı konektör koleksiyonunun](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852)bir parçası olarak kullanılabilir. |
| Syslog sunucusu | Hayır | Azure Monitor verilerini doğrudan bir syslog sunucusuna aktarmak istiyorsanız, [Azure işlevini temel alan bir çözüm](https://github.com/miguelangelopereira/azuremonitor2syslog/)kullanabilirsiniz.
| LogRhythm | Hayır| Bir olay merkezinden günlükleri toplamak için LogRhythm'i ayarlama [yönergelerine buradan](https://logrhythm.com/six-tips-for-securing-your-azure-cloud-environment/)ulaşabilirsiniz. 
|Logz.io | Evet | Daha fazla bilgi için, [Azure'da çalışan Java uygulamaları için Logz.io kullanarak izleme ve günlüğe kaydetmeye başlama](https://docs.microsoft.com/azure/developer/java/fundamentals/java-get-started-with-logzio)


## <a name="next-steps"></a>Sonraki Adımlar
* [Etkinlik günlüğünü bir depolama hesabına arşivleme](../../azure-monitor/platform/archive-activity-log.md)
* [Azure Etkinliği günlüğüne genel bakışı okuyun](../../azure-monitor/platform/platform-logs-overview.md)
* [Etkinlik günlüğü olayına dayalı bir uyarı ayarlama](../../azure-monitor/platform/alerts-log-webhook.md)


