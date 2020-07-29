---
title: Azure 'da uyarı ve bildirim izlemeye genel bakış
description: Azure 'da uyarı konusuna genel bakış. Uyarılar, klasik uyarılar ve uyarılar arabirimi.
ms.subservice: alerts
ms.topic: conceptual
ms.date: 01/28/2018
ms.openlocfilehash: e0741a23d7e5ece0898d83c53782afc353d9a7e5
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371609"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft Azure'da uyarılara genel bakış 

Bu makalede, uyarıların ne olduğu, avantajları ve kullanmaya nasıl başladıklarından bazıları açıklanmaktadır.  

## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft Azure uyarılar nelerdir?
Uyarılar, izleme verilerinizde önemli koşullar bulunduğunda size bir bildirim gönderir. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. 

Bu makalede, daha önce Log Analytics ve Application Insights tarafından yönetilen uyarıları içeren Azure Izleyici 'de birleştirilmiş uyarı deneyimi ele alınmaktadır. [Önceki uyarı deneyimi](alerts-classic.overview.md) ve Uyarı türleri *Klasik uyarılar*olarak adlandırılır. Uyarı sayfasının en üstünde **Klasik Uyarıları görüntüle** seçeneğini belirleyerek bu eski deneyimi ve eski uyarı türünü görüntüleyebilirsiniz. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagram, uyarıların akışını temsil eder. 

![Uyarı akışı diyagramı](media/alerts-overview/Azure-Monitor-Alerts.svg)

Uyarı kuralları uyarılardan ve bir uyarı tetiklendiğinde gerçekleştirilen eylemlerle ayrılır. Uyarı kuralı, uyarıya yönelik hedefi ve ölçütleri yakalar. Uyarı kuralı etkin veya devre dışı durumda olabilir. Uyarılar yalnızca etkinleştirildiğinde ateşlenir. 

Aşağıda bir uyarı kuralının anahtar öznitelikleri verilmiştir:

**Hedef kaynak**: uyarı için kullanılabilen kapsamı ve sinyalleri tanımlar. Hedef, herhangi bir Azure kaynağı olabilir. Örnek hedefler: bir sanal makine, depolama hesabı, bir sanal makine ölçek kümesi, Log Analytics çalışma alanı veya bir Application Insights kaynağı. Belirli kaynaklar (sanal makineler gibi) için, uyarı kuralının hedefi olarak birden çok kaynak belirtebilirsiniz.

**Sinyal**: hedef kaynak tarafından verilmiş. Sinyaller şu türlerde olabilir: ölçüm, etkinlik günlüğü, Application Insights ve günlük.

**Ölçüt**: bir hedef kaynağa uygulanan sinyal ve mantık birleşimi. Örnekler: 

- Yüzde 70 CPU >
- Sunucu yanıt süresi > 4 MS 
- Günlük sorgusunun sonuç sayısı > 100

**Uyarı adı**: Kullanıcı tarafından yapılandırılan uyarı kuralı için özel bir ad.

**Uyarı açıklaması**: Kullanıcı tarafından yapılandırılan uyarı kuralı için bir açıklama.

**Önem derecesi**: uyarı kuralında belirtilen ölçütlerle sonra uyarının önem derecesi karşılanır. Önem derecesi 0 ile 4 arasında olabilir.

- Sev 0 = kritik
- Sev 1 = hata
- Sev 2 = uyarı
- Sev 3 = bilgilendirici
- Sev 4 = ayrıntılı 

**Eylem**: uyarı harekete geçirildiğinde gerçekleştirilecek belirli bir eylem. Daha fazla bilgi için bkz. [eylem grupları](./action-groups.md).

## <a name="what-you-can-alert-on"></a>Uyarı yapabilecekleriniz

[Veri kaynaklarını izleme](./data-sources.md)bölümünde açıklandığı gibi ölçümler ve Günlükler hakkında uyarı alabilirsiniz. Bunlarla sınırlı olmamak üzere şunları içerir:

- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformunun durumu
- Web sitesi kullanılabilirliği için testler

Daha önce Azure Izleyici ölçümleri, Application Insights, Log Analytics ve hizmet durumu ayrı uyarı yeteneklerine sahipti. Zaman içinde Azure, hem Kullanıcı arabirimini hem de farklı uyarı yöntemlerini geliştirmiştir ve birleştirilemez. Bu birleştirme işlemi hala devam ediyor. Sonuç olarak, henüz yeni uyarılar sisteminde olmayan bazı uyarı özellikleri de vardır.  

| **Kaynağı izle** | **Sinyal türü**  | **Açıklama** |
|-------------|----------------|-------------|
| Hizmet durumu | Etkinlik günlüğü  | Desteklenmez. Bkz. [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../../service-health/alerts-activity-log-service-notifications-portal.md).  |
| Application Insights | Web kullanılabilirlik testleri | Desteklenmez. Bkz. [Web test uyarıları](../app/monitor-web-app-availability.md). Application Insights veri gönderecek şekilde işaretlenmiş Web siteleri için kullanılabilir. Bir Web sitesinin kullanılabilirliği veya yanıt verme işlemi beklentileri altında olduğunda bir bildirim alın. |

## <a name="manage-alerts"></a>Uyarıları yönetme
Bir uyarının durumunu, çözüm sürecinde nerede olduğunu belirtmek için ayarlayabilirsiniz. Uyarı kuralında belirtilen ölçütler karşılandığında, bir uyarı oluşturulur veya tetiklenir ve *Yeni*durumuna sahiptir. Bir uyarıyı onayladığınızda ve kapattığınızda durumu değiştirebilirsiniz. Tüm durum değişiklikleri uyarının geçmişine depolanır.

Aşağıdaki uyarı durumları desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun henüz algılandı ve henüz gözden geçirilmedi. |
| Onaylandı | Bir yönetici uyarıyı inceetti ve üzerinde çalışmaya başladı. |
| Kapalı | Sorun çözüldü. Bir uyarı kapatıldıktan sonra, başka bir durumla değiştirerek dosyayı yeniden açabilirsiniz. |

*Uyarı durumu* , *izleyici koşulunun*farklıdır ve bağımsızdır. Uyarı durumu Kullanıcı tarafından ayarlanır. İzleme koşulu sistem tarafından ayarlanır. Bir uyarı tetiklendiğinde, uyarının izleyici koşulu *tetiklenir*olarak ayarlanır. Uyarının tetiklenmesine neden olan temeldeki durum temizlediğinde, izleme koşulu *çözüldü*olarak ayarlanır. Uyarı durumu Kullanıcı tarafından değiştirilene kadar değiştirilmez. [Uyarılarınızın ve akıllı grupların durumunu değiştirme hakkında](https://aka.ms/managing-alert-smart-group-states)bilgi edinin.

## <a name="smart-groups"></a>Akıllı gruplar 

Akıllı gruplar, uyarı gürültüsünü azaltmaya ve sorun gidermeye yardımcı olabilecek makine öğrenimi algoritmalarına dayalı uyarıların toplamasıdır. Akıllı gruplar ve [akıllı gruplarınızı yönetme](https://aka.ms/managing-smart-groups) [hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups) .


## <a name="alerts-experience"></a>Uyarı deneyimi 
Varsayılan uyarılar sayfası, belirli bir zaman aralığı içinde oluşturulan uyarıların bir özetini sağlar. Her önem derecesine yönelik toplam uyarı sayısını, her önem derecesine göre her bir durum için toplam uyarı sayısını tanımlayan sütunlarla görüntüler. Bu önem derecesine göre filtrelenen [tüm uyarılar](#all-alerts-page) sayfasını açmak için tüm önem derecelerinin herhangi birini seçin.

Alternatif olarak, [REST API 'lerini kullanarak aboneliklerinizde oluşturulan uyarı örneklerini programlı](#manage-your-alert-instances-programmatically)bir şekilde sıralayabilirsiniz.

> [!NOTE]
   >  Yalnızca son 30 gün içinde oluşturulan uyarılara erişebilirsiniz.

Klasik uyarıları göstermez veya izlemez. Sayfayı güncelleştirmek için abonelikleri veya filtre parametrelerini değiştirebilirsiniz. 

![Uyarı sayfasının ekran görüntüsü](media/alerts-overview/alerts-page.png)

Bu görünümü, sayfanın en üstündeki açılan menülerde bulunan değerler ' i seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarılarını görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak, tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahildir. |
| Zaman aralığı | Yalnızca seçili zaman aralığı içinde tetiklenen uyarılar görünüme dahildir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Başka bir sayfa açmak için uyarılar sayfasının en üstünde bulunan aşağıdaki değerleri seçin:

| Değer | Açıklama |
|:---|:---|
| Toplam uyarı sayısı | Seçilen ölçütlerle eşleşen toplam uyarı sayısı. Filtre olmadan tüm uyarılar görünümünü açmak için bu değeri seçin. |
| Akıllı gruplar | Seçili ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupların toplam sayısı. Tüm uyarılar görünümündeki akıllı gruplar listesini açmak için bu değeri seçin.
| Toplam uyarı kuralları | Seçili abonelik ve kaynak grubundaki uyarı kurallarının toplam sayısı. Seçilen abonelikte ve kaynak grubunda filtrelenmiş kurallar görünümünü açmak için bu değeri seçin.


## <a name="manage-alert-rules"></a>Uyarı kurallarını yönetin
**Kurallar** sayfasını görüntülemek için, **Uyarı kurallarını yönet**' i seçin. Kurallar sayfası, Azure aboneliklerinizde tüm uyarı kurallarını yönetmek için tek bir yerdir. Tüm uyarı kurallarını listeler ve hedef kaynaklara, kaynak gruplarına, kural adına veya duruma göre sıralanabilir. Ayrıca, bu sayfadan uyarı kurallarını düzenleyebilir, etkinleştirebilir veya devre dışı bırakabilirsiniz.  

 ![Kurallar sayfasının ekran görüntüsü](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Uyarı kurallarını izleme hizmeti veya sinyal türünden bağımsız olarak tutarlı bir şekilde yazabilirsiniz.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tflw]

 
Yeni bir uyarı kuralı oluşturmak için aşağıdaki adımları uygulayın:
1. Uyarı için _hedefi_ seçin.
1. Hedef için kullanılabilir sinyallerden _sinyal_ seçin.
1. Sinyalden verilere uygulanacak _mantığı_ belirtin.
 
Bu basitleştirilmiş yazma işlemi artık, bir Azure kaynağı seçmeden önce desteklenen izleme kaynağını veya sinyalleri bilmeniz için gerekli değildir. Kullanılabilir sinyallerin listesi, seçtiğiniz hedef kaynağa göre otomatik olarak filtrelenir. Ayrıca, bu hedefe göre, uyarı kuralının mantığını otomatik olarak tanımlayarak size kılavuzluk edilir.  

[Azure izleyici kullanarak uyarı oluşturma, görüntüleme ve yönetme](./alerts-metric.md)konusunda uyarı kuralları oluşturma hakkında daha fazla bilgi edinebilirsiniz.

Uyarılar, birkaç Azure izleme hizmeti arasında kullanılabilir. Bu hizmetlerin her birinin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Azure uygulamalarını ve kaynaklarını izleme](../overview.md). 


## <a name="all-alerts-page"></a>Tüm uyarılar sayfası 
**Tüm uyarılar** sayfasını görmek Için **Toplam uyarı**' yı seçin. Burada, seçilen süre içinde oluşturulan uyarıların bir listesini görebilirsiniz. Tek tek uyarıların bir listesini ya da uyarıları içeren akıllı grupların bir listesini görebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst kısmındaki başlık ' ı seçin.

![Tüm uyarılar sayfasının ekran görüntüsü](media/alerts-overview/all-alerts-page.png)

Sayfanın en üstündeki açılan menülerde aşağıdaki değerleri seçerek görünümü filtreleyebilirsiniz:

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarılarını görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak, tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahildir. |
| Kaynak türü | Bir veya daha fazla kaynak türü seçin. Yalnızca seçilen türdeki hedefleri olan uyarılar görünüme dahildir. Bu sütun yalnızca bir kaynak grubu belirtilmişse kullanılabilir. |
| Kaynak | Bir kaynak seçin. Yalnızca hedef olarak bu kaynağa sahip olan uyarılar görünüme dahil edilir. Bu sütun yalnızca bir kaynak türü belirtilmişse kullanılabilir. |
| Severity | Bir uyarı önem derecesi seçin veya tüm önem derecelerinin uyarılarını dahil etmek için **Tümü** ' nü seçin. |
| İzleme koşulu | Bir izleyici koşulu seçin veya tüm koşulların uyarılarını dahil etmek için **Tümü** ' nü seçin. |
| Uyarı durumu | Bir uyarı durumu seçin veya tüm durumların uyarılarını dahil etmek için **Tümü** ' nü seçin. |
| Hizmeti izle | Bir hizmet seçin veya tüm hizmetleri dahil etmek için **Tümü** ' nü seçin. Yalnızca hizmeti hedef olarak kullanan kurallar tarafından oluşturulan uyarılar dahildir. |
| Zaman aralığı | Yalnızca seçili zaman aralığı içinde tetiklenen uyarılar görünüme dahildir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Görüntülenecek sütunları seçmek için sayfanın üst kısmındaki **sütunları** seçin. 

## <a name="alert-details-page"></a>Uyarı ayrıntıları sayfası
Bir uyarı seçtiğinizde, Bu sayfa uyarının ayrıntılarını sağlar ve durumunu değiştirmenizi sağlar.

![Uyarı Ayrıntıları sayfasının ekran görüntüsü](media/alerts-overview/alert-detail2.png)

Uyarı ayrıntıları sayfası aşağıdaki bölümleri içerir:

| Section | Açıklama |
|:---|:---|
| Özet | Uyarı hakkındaki özellikleri ve diğer önemli bilgileri görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarıya yapılan tüm değişiklikleri listeler. Şu anda durum değişiklikleriyle sınırlı. |
| Tanılama | Uyarının dahil olduğu akıllı grup hakkında bilgi. *Uyarı sayısı* , akıllı gruba dahil edilen uyarı sayısını ifade eder. Son 30 gün içinde oluşturulan aynı akıllı gruptaki diğer uyarıları, uyarılar listesi sayfasındaki zaman filtresinden bağımsız olarak içerir. Ayrıntılarını görüntülemek için bir uyarı seçin. |

## <a name="role-based-access-control-rbac-for-your-alert-instances"></a>Uyarı örneklerinizin rol tabanlı erişim denetimi (RBAC)

Uyarı örneklerinin tüketimine ve yönetimine yönelik olarak kullanıcının Azure yerleşik rollerinin, [katkıda](../../role-based-access-control/built-in-roles.md#monitoring-contributor) bulunan veya [izleme okuyucularından](../../role-based-access-control/built-in-roles.md#monitoring-reader)birine sahip olmasını gerektirir. Bu roller her bir Azure Resource Manager kapsamında, abonelik düzeyinden kaynak düzeyindeki ayrıntılı atamalara kadar desteklenir. Örneğin, bir Kullanıcı yalnızca sanal makine için katkıda bulunan erişimi izmışsa `ContosoVM1` , bu kullanıcı yalnızca üzerinde oluşturulan uyarıları kullanabilir ve yönetebilir `ContosoVM1` .

## <a name="manage-your-alert-instances-programmatically"></a>Uyarı örneklerinizi programlama yoluyla yönetme

Aboneliğinize göre oluşturulan uyarılar için programlı olarak sorgulamak isteyebilirsiniz. Bu, Azure portal dışında özel görünümler oluşturmak veya desenleri ve eğilimleri belirlemek için uyarılarınızı analiz etmek olabilir.

Aboneliklerinizde oluşturulan uyarıları, [Uyarı Yönetimi REST API](https://aka.ms/alert-management-api) kullanarak veya [Azure Kaynak grafiğini](../../governance/resource-graph/overview.md) ve [kaynaklar için REST API](/rest/api/azureresourcegraph/resourcegraph(2019-04-01)/resources/resources)kullanarak sorgulayabilirsiniz.

Kaynak grafik REST API, uyarı örneklerini ölçekteki sorgulamanızı sağlar. Birçok abonelik üzerinde oluşturulan uyarıları yönetmeniz gerektiğinde bu önerilir. 

Kaynak Graph REST API aşağıdaki örnek istek, bir abonelik içindeki uyarı sayısını döndürür:

```json
{
  "subscriptions": [
    <subscriptionId>
  ],
  "query": "AlertsManagementResources | where type =~ 'Microsoft.AlertsManagement/alerts' | summarize count()"
}
```

Ayrıca, bu kaynak grafiği sorgusunun sonucunu Azure Resource Graph Explorer ile Portalda görebilirsiniz: [Portal.Azure.com](https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AlertsManagementResources%20%7C%20where%20type%20%3D~%20%27Microsoft.AlertsManagement%2Falerts%27%20%7C%20summarize%20count())

Uyarıları, [önemli](alerts-common-schema-definitions.md#essentials) alanları için sorgulayabilirsiniz.

[Uyarı bağlamı](alerts-common-schema-definitions.md#alert-context) alanları da dahil olmak üzere belirli uyarılar hakkında daha fazla bilgi almak için [uyarı yönetimi REST API](https://aka.ms/alert-management-api) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı gruplar hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups)
- [Eylem grupları hakkında bilgi edinin](./action-groups.md)
- [Azure 'da uyarı örneklerinizi yönetme](https://aka.ms/managing-alert-instances)
- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)
- [Azure uyarıları fiyatlandırması hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/monitor/)
