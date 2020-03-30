---
title: Azure'da uyarı ve bildirim izlemeye genel bakış
description: Azure'da uyarıya genel bakış. Uyarılar, klasik uyarılar ve uyarılar arabirimi.
ms.subservice: alerts
ms.topic: conceptual
ms.date: 01/28/2018
ms.openlocfilehash: 7ca77531ed3e1fae8ec297e430597452c7512aea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274794"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft Azure'da uyarılara genel bakış 

Bu makalede, uyarıların ne olduğu, avantajları ve bunları kullanmaya nasıl başlanoldukları açıklanmaktadır.  

## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft Azure'daki uyarılar nelerdir?
İzleme verilerinizde önemli koşullar bulunduğunda uyarılar sizi proaktif olarak bildirir. Sisteminizin kullanıcıları bunları fark etmeden önce sorunları tanımlamanızı ve çözmenizi sağlar. 

Bu makalede, Azure Monitor'da daha önce Log Analytics ve Application Insights tarafından yönetilen uyarıları içeren birleşik uyarı deneyimi açıklanmaktadır. [Önceki uyarı deneyimi](alerts-classic.overview.md) ve uyarı türleri klasik *uyarılar*olarak adlandırılır. Bu eski deneyimi ve eski uyarı türünü, uyarı sayfasının üst **kısmındaki klasik uyarıları** görüntüle'yi seçerek görüntüleyebilirsiniz. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagram uyarıların akışını temsil eder. 

![Uyarı akışı diyagramı](media/alerts-overview/Azure-Monitor-Alerts.svg)

Uyarı kuralları uyarılardan ve bir uyarı çıktığında gerçekleştirilen eylemlerden ayrılır. Uyarı kuralı, hedef ve uyarı için ölçütleri yakalar. Uyarı kuralı etkin veya devre dışı bırakılmış bir durumda olabilir. Uyarılar yalnızca etkinleştirildiğinde ateş eder. 

Bir uyarı kuralının temel öznitelikleri şunlardır:

**Hedef Kaynak**: Uyarı için kullanılabilir kapsam ve sinyalleri tanımlar. Hedef herhangi bir Azure kaynağı olabilir. Örnek hedefler: sanal makine, depolama hesabı, sanal makine ölçek kümesi, Log Analytics çalışma alanı veya Application Insights kaynağı. Belirli kaynaklar (sanal makineler gibi) için, uyarı kuralının hedefi olarak birden çok kaynak belirtebilirsiniz.

**Sinyal**: Hedef kaynak tarafından yayılan. Sinyaller aşağıdaki türlerden olabilir: metrik, etkinlik günlüğü, Uygulama İstatistikleri ve günlük.

**Ölçütler**: Hedef kaynağa uygulanan sinyal ve mantığın birleşimidir. Örnekler: 

- Yüzde CPU > % 70
- Sunucu Yanıt Süresi > 4 ms 
- Günlük sorgusunun sonuç sayısı 100 >

**Uyarı Adı**: Kullanıcı tarafından yapılandırılan uyarı kuralı için belirli bir ad.

**Uyarı Açıklaması**: Kullanıcı tarafından yapılandırılan uyarı kuralıiçin yapılan açıklama.

**Önem Derecesi**: Uyarı kuralında belirtilen ölçütler karşılandıktan sonra uyarının önem derecesi. Önem derecesi 0 ile 4 arasında değişebilir.

- Sev 0 = Kritik
- Sev 1 = Hata
- Sev 2 = Uyarı
- Sev 3 = Bilgilendirme
- Sev 4 = Tamşu 

**Eylem**: Uyarı ateşlendiğinde alınan belirli bir eylem. Daha fazla bilgi için [Eylem Grupları'na](../../azure-monitor/platform/action-groups.md)bakın.

## <a name="what-you-can-alert-on"></a>Neleri uyarabilirsiniz

Veri kaynaklarını izlemede açıklandığı gibi ölçümler ve günlükler konusunda uyarıda [bulunabilirsiniz.](../../azure-monitor/platform/data-sources.md) Bunlarla sınırlı olmamak üzere şunları içerir:

- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformunun durumu
- Web sitesi kullanılabilirliği için testler

Önceden, Azure Monitör ölçümleri, Uygulama Öngörüleri, Log Analytics ve Hizmet Durumu ayrı uyarı özelliklerine sahipti. Azure, zaman içinde hem kullanıcı arabirimini hem de farklı uyarı yöntemlerini geliştirmiş ve birleştirmiştir. Bu konsolidasyon hala devam ediyor. Sonuç olarak, yeni uyarı sisteminde henüz bazı uyarı özellikleri vardır.  

| **Monitör kaynağı** | **Sinyal türü**  | **Açıklama** |
|-------------|----------------|-------------|
| Hizmet durumu | Etkinlik günlüğü  | Desteklenmiyor. Bkz. [Hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturun.](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)  |
| Application Insights | Web kullanılabilirliği testleri | Desteklenmiyor. [Bkz. Web test uyarıları.](../../azure-monitor/app/monitor-web-app-availability.md) Application Insights'a veri gönderme aracılı herhangi bir web sitesi tarafından kullanılabilir. Bir web sitesinin kullanılabilirliği veya yanıt verme durumu beklentilerin altında olduğunda bir bildirim alın. |

## <a name="manage-alerts"></a>Uyarıları yönetme
Çözümleme işleminde nerede olduğunu belirtmek için uyarı durumunu ayarlayabilirsiniz. Uyarı kuralında belirtilen ölçütler karşılandığında, bir uyarı oluşturulur veya ateşlenir ve *Yeni*. Bir uyarıyı kabul ettiğinizde ve kapattığınızda durumu değiştirebilirsiniz. Tüm durum değişiklikleri uyarının geçmişinde depolanır.

Aşağıdaki uyarı durumları desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yeni algılandı ve henüz gözden geçirilmemiştir. |
| Onaylandı | Bir yönetici uyarıyı gözden geçirdi ve üzerinde çalışmaya başladı. |
| Closed | Sorun çözüldü. Bir uyarı kapatıldıktan sonra, başka bir duruma değiştirerek yeniden açabilirsiniz. |

*Uyarı durumu* farklı ve *monitör durumundan*bağımsız. Uyarı durumu kullanıcı tarafından ayarlanır. Monitör durumu sistem tarafından ayarlanır. Bir uyarı çıktığında, uyarının monitör durumu *ateşlenecek*şekilde ayarlanır. Uyarının yangına neden olan altta yatan durum temizlendiğinde, monitör durumu *çözülecek*şekilde ayarlanır. Kullanıcı değiştirene kadar uyarı durumu değiştirilmez. [Uyarılarınızın ve akıllı grupların durumunu nasıl değiştireceğinizi](https://aka.ms/managing-alert-smart-group-states)öğrenin.

## <a name="smart-groups"></a>Akıllı gruplar 

Akıllı gruplar, uyarı gürültüsünü azaltmaya ve sorun gidermede yardımcı olunmaya yardımcı olabilecek makine öğrenimi algoritmalarına dayalı uyarı toplamalarıdır. Akıllı Gruplar ve [akıllı gruplarınızı nasıl yönetebilirsiniz](https://aka.ms/managing-smart-groups) [hakkında daha fazla bilgi edinin.](https://aka.ms/smart-groups)


## <a name="alerts-experience"></a>Uyarıları deneyim 
Varsayılan Uyarılar sayfası, belirli bir zaman aralığında oluşturulan uyarıların bir özetini sağlar. Her önem derecesi için toplam uyarıları görüntüler ve her önem için her durumdaki toplam uyarı sayısını tanımlayan sütunlar bulunur. Bu öneme göre filtrelenen [Tüm Uyarılar](#all-alerts-page) sayfasını açmak için önemlerinden herhangi birini seçin.

Alternatif olarak, [REST API'lerini kullanarak aboneliklerinizde oluşturulan uyarı örneklerini programlı olarak sayısal olarak](#manage-your-alert-instances-programmatically)oyalayabilirsiniz.

> [!NOTE]
   >  Yalnızca son 30 gün içinde oluşturulan uyarılara erişebilirsiniz.

Klasik uyarıları göstermez veya izlemez. Sayfayı güncelleştirmek için abonelikleri değiştirebilir veya parametreleri filtreleyebilirsiniz. 

![Uyarılar sayfasının ekran görüntüsü](media/alerts-overview/alerts-page.png)

Sayfanın üst kısmındaki açılır menülerde değerleri seçerek bu görünümü filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarıları görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahil edilir. |
| Zaman aralığı | Yalnızca seçilen zaman aralığında ateşlenen uyarılar görünüme dahil edilir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Başka bir sayfa açmak için Uyarılar sayfasının üst kısmında aşağıdaki değerleri seçin:

| Değer | Açıklama |
|:---|:---|
| Toplam uyarılar | Seçili ölçütlerle eşleşen toplam uyarı sayısı. Tüm Uyarılar görünümünü filtresiz açmak için bu değeri seçin. |
| Akıllı gruplar | Seçilen ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupların toplam sayısı. Tüm Uyarılar görünümünde akıllı gruplar listesini açmak için bu değeri seçin.
| Toplam uyarı kuralları | Seçili abonelik ve kaynak grubundaki toplam uyarı kuralı sayısı. Seçili abonelik ve kaynak grubunda filtre uygulanmış Kurallar görünümünü açmak için bu değeri seçin.


## <a name="manage-alert-rules"></a>Uyarı kurallarını yönetin
**Kurallar** sayfasını göstermek için **uyarı kurallarını yönet'i**seçin. Kurallar sayfası, Azure aboneliklerinizdeki tüm uyarı kurallarını yönetmek için tek bir yerdir. Tüm uyarı kurallarını listeler ve hedef kaynaklara, kaynak gruplarına, kural adına veya duruma göre sıralanabilir. Ayrıca bu sayfadan uyarı kurallarını da edinebilir, etkinleştirebilir veya devre dışı kabilirsiniz.  

 ![Kurallar sayfasının ekran görüntüsü](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
İzleme hizmeti veya sinyal türünden bağımsız olarak uyarıları tutarlı bir şekilde yazabilirsiniz. Tüm uyarılar ve ilgili ayrıntılar tek sayfada mevcuttur.
 
Yeni bir uyarı kuralı nı şu şekilde oluşturabilirsiniz:
1. Uyarı için _hedefi_ seçin.
1. Hedef için mevcut sinyallerden _gelen sinyali_ seçin.
1. Sinyalden gelen verilere uygulanacak _mantığı_ belirtin.
 
Bu basitleştirilmiş yazma işlemi, bir Azure kaynağını seçmeden önce desteklenen izleme kaynağını veya sinyalleri artık bilmenizi gerektirmez. Kullanılabilir sinyallerin listesi, seçtiğiniz hedef kaynağa göre otomatik olarak filtrelenir. Ayrıca bu hedefe dayanarak, otomatik olarak uyarı kuralının mantığını tanımlayarak yönlendirilir.  

[Azure Monitor'u kullanarak uyarıları Oluştur, görüntüle ve yönetme'de](../../azure-monitor/platform/alerts-metric.md)uyarı kurallarının nasıl oluşturulabileceği hakkında daha fazla bilgi edinebilirsiniz.

Çeşitli Azure izleme hizmetlerinde uyarılar mevcuttur. Bu hizmetlerin her birinin nasıl ve ne zaman kullanılacağı hakkında bilgi için Azure [uygulamalarını ve kaynaklarını izleme](../../azure-monitor/overview.md)bölümüne bakın. 


## <a name="all-alerts-page"></a>Tüm Uyarılar sayfası 
**Tüm Uyarılar** sayfasını görmek için **Toplam Uyarılar'ı**seçin. Burada, seçili süre içinde oluşturulan uyarıların listesini görüntüleyebilirsiniz. Tek tek uyarıların listesini veya uyarıları içeren akıllı grupların listesini görüntüleyebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst kısmındaki banner'ı seçin.

![Tüm Uyarılar sayfasının ekran görüntüsü](media/alerts-overview/all-alerts-page.png)

Sayfanın üst kısmındaki açılır menülerde aşağıdaki değerleri seçerek görünümü filtreleyebilirsiniz:

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarıları görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahil edilir. |
| Kaynak türü | Bir veya daha fazla kaynak türü seçin. Görünüme yalnızca seçili türdeki hedeflere sahip uyarılar dahildir. Bu sütun yalnızca bir kaynak grubu belirtildikten sonra kullanılabilir. |
| Kaynak | Bir kaynak seçin. Görünüme yalnızca hedef olarak bu kaynağa sahip uyarılar dahildir. Bu sütun yalnızca kaynak türü belirtildikten sonra kullanılabilir. |
| Severity | Bir uyarı önem derecesi seçin veya tüm önem dereceleri uyarıları eklemek için **Tümü'nü** seçin. |
| Monitör durumu | Bir monitör koşulu seçin veya tüm koşullara ait uyarıları eklemek için **Tümü'nü** seçin. |
| Uyarı durumu | Bir uyarı durumu seçin veya tüm durumların uyarılarını eklemek için **Tümü'nü** seçin. |
| Hizmeti izleyin | Bir hizmet seçin veya tüm hizmetleri içerecek şekilde **Tümü'nü** seçin. Yalnızca hizmeti hedef olarak kullanan kurallar tarafından oluşturulan uyarılar dahildir. |
| Zaman aralığı | Yalnızca seçilen zaman aralığında ateşlenen uyarılar görünüme dahil edilir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Hangi sütunların gösterilen seçilecek lerini seçmek için sayfanın üst **kısmındaki Sütunları** seçin. 

## <a name="alert-details-page"></a>Uyarı ayrıntıları sayfası
Bir uyarı seçtiğinizde, bu sayfa uyarının ayrıntılarını sağlar ve durumunu değiştirmenizi sağlar.

![Uyarı ayrıntıları sayfasının ekran görüntüsü](media/alerts-overview/alert-detail2.png)

Uyarı ayrıntıları sayfası aşağıdaki bölümleri içerir:

| Section | Açıklama |
|:---|:---|
| Özet | Uyarıyla ilgili özellikleri ve diğer önemli bilgileri görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarıda yapılan değişiklikleri listeler. Şu anda durum değişiklikleri ile sınırlıdır. |
| Tanılama | Uyarının dahil edildiği akıllı grup hakkında bilgi. *Uyarı sayısı,* akıllı gruba dahil edilen uyarı sayısını ifade eder. Uyarılar listesi sayfasındaki zaman filtresinden bağımsız olarak, son 30 gün içinde oluşturulan aynı akıllı gruptaki diğer uyarıları içerir. Ayrıntılarını görüntülemek için bir uyarı seçin. |

## <a name="role-based-access-control-rbac-for-your-alert-instances"></a>Uyarı örnekleriniz için rol tabanlı erişim denetimi (RBAC)

Uyarı örneklerinin tüketimi ve yönetimi, kullanıcının [katılımcıyı veya](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) [izleme okuyucuyu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader)izleme yerleşik RBAC rollerine sahip olmasını gerektirir. Bu roller, abonelik düzeyinden kaynak düzeyindeki ayrıntılı atamalara kadar herhangi bir Azure Kaynak Yöneticisi kapsamında desteklenir. Örneğin, bir kullanıcı yalnızca sanal makine `ContosoVM1`için izleme katılımcısı erişimine sahipse, `ContosoVM1`bu kullanıcı yalnızca oluşturulan uyarıları tüketebilir ve yönetebilir.

## <a name="manage-your-alert-instances-programmatically"></a>Uyarı örneklerinizi programlı olarak yönetme

Aboneliğinize karşı oluşturulan uyarılar için programlı olarak sorgulama yapmak isteyebilirsiniz. Bu, Azure portalının dışında özel görünümler oluşturmak veya desenleri ve eğilimleri belirlemek için uyarılarınızı çözümlemek olabilir.

Aboneliklerinize karşı oluşturulan [uyarıları, Uyarı Yönetimi REST API'sini](https://aka.ms/alert-management-api) kullanarak veya [Azure Kaynak Grafiği'ni](../../governance/resource-graph/overview.md) ve Kaynaklar için [REST API'sini](/rest/api/azureresourcegraph/resourcegraph(2019-04-01)/resources/resources)kullanarak sorgulayabilirsiniz.

Kaynaklar için Kaynak Grafiği REST API ölçekte uyarı örnekleri için sorgulama yapmanızı sağlar. Bu, birçok abonelikte oluşturulan uyarıları yönetmeniz gerektiğinde önerilir. 

Kaynak Grafiği REST API'ye yapılan aşağıdaki örnek istek, tek bir abonelikteki uyarıların sayısını döndürür:

```json
{
  "subscriptions": [
    <subscriptionId>
  ],
  "query": "AlertsManagementResources | where type =~ 'Microsoft.AlertsManagement/alerts' | summarize count()"
}
```

Bu Kaynak Grafiği sorgusunun sonucunu Azure Kaynak Grafiği Gezgini ile portalda da görebilirsiniz: [portal.azure.com](https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AlertsManagementResources%20%7C%20where%20type%20%3D~%20%27Microsoft.AlertsManagement%2Falerts%27%20%7C%20summarize%20count())

Uyarıları [temel](alerts-common-schema-definitions.md#essentials) alanları için sorgulayabilirsiniz.

[Uyarı bağlamalanları](alerts-common-schema-definitions.md#alert-context) da dahil olmak üzere belirli uyarılar hakkında daha fazla bilgi almak için [Uyarı Yönetimi REST API'sını](https://aka.ms/alert-management-api) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı Gruplar hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups)
- [Eylem grupları hakkında bilgi edinin](../../azure-monitor/platform/action-groups.md)
- [Azure'da uyarı örneklerinizi yönetme](https://aka.ms/managing-alert-instances)
- [Akıllı Grupları Yönetme](https://aka.ms/managing-smart-groups)
- [Azure uyarıları fiyatlandırması hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/)






