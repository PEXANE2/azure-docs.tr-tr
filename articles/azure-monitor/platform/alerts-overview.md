---
title: Azure 'da uyarı ve bildirim izlemeye genel bakış
description: Azure 'da uyarı konusuna genel bakış. Uyarılar, klasik uyarılar, uyarılar arabirimi.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/28/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 67318fee540195fc913739d78e80649100c54e70
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034826"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft Azure uyarılara genel bakış 

Bu makalede, uyarıların ne olduğu, avantajları ve kullanmaya nasıl başladıklarından bazıları açıklanmaktadır.  




## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft Azure uyarılar nelerdir?
Uyarılar, izleme verilerinizde önemli koşullar bulunduğunda size bir bildirim gönderir. Bunlar, sisteminizin kullanıcıları tarafından bildirilmeksizin sorunları tanımlamanızı ve adreslerinizi belirlemenizi sağlar. 

Bu makalede, artık Log Analytics ve Application Insights tarafından yönetilen uyarılar içeren Azure Izleyici 'de birleştirilmiş uyarı deneyimi ele alınmaktadır. [Önceki uyarı deneyimi](alerts-classic.overview.md) ve Uyarı türleri **Klasik uyarılar**olarak adlandırılır. Uyarı sayfasının en üstündeki **Klasik Uyarıları görüntüle** ' ye tıklayarak bu eski deneyimi ve eski uyarı türünü görüntüleyebilirsiniz. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagram, uyarıların akışını temsil eder. 

![Uyarı akışı](media/alerts-overview/Azure-Monitor-Alerts.svg)

Uyarı kuralları uyarılardan ve bir uyarı tetiklendiğinde gerçekleştirilen eylemlerden ayrılır. 

**Uyarı kuralı** -uyarı kuralı, uyarıya yönelik hedefi ve ölçütleri yakalar. Uyarı kuralı etkin veya devre dışı durumda olabilir. Uyarılar yalnızca etkinleştirildiğinde ateşlenir. 

Bir uyarı kuralının anahtar öznitelikleri şunlardır:

**Hedef kaynak** -uyarı için kullanılabilen kapsamı ve sinyalleri tanımlar. Hedef, herhangi bir Azure kaynağı olabilir. Örnek hedefler: bir sanal makine, depolama hesabı, bir sanal makine ölçek kümesi, Log Analytics çalışma alanı veya bir Application Insights kaynağı. Belirli kaynaklar (sanal makineler gibi) için, uyarı kuralının hedefi olarak birden çok kaynak belirtebilirsiniz.

**Sinyal** sinyalleri hedef kaynak tarafından dağıtılır ve çeşitli türlerde olabilir. Ölçüm, etkinlik günlüğü, Application Insights ve günlük.

**Ölçüt** -ölçüt, bir hedef kaynakta uygulanan sinyal ve mantık birleşimidir. Örnekler: 
   - Yüzde 70 CPU >
   - Sunucu yanıt süresi > 4 MS 
   - Günlük sorgusunun sonuç sayısı > 100

**Uyarı adı** : Kullanıcı tarafından yapılandırılan uyarı kuralı için belirli bir ad

**Uyarı açıklaması** – Kullanıcı tarafından yapılandırılan uyarı kuralı için bir açıklama

**Önem derecesi** : uyarı kuralında belirtilen kriterler karşılandığında uyarının önem derecesi. Önem derecesi 0 ile 4 arasında olabilir.

**Eylem** -uyarı harekete geçirildiğinde gerçekleştirilecek belirli bir eylem. Daha fazla bilgi için bkz. [eylem grupları](../../azure-monitor/platform/action-groups.md).

## <a name="what-you-can-alert-on"></a>Uyarı yapabilecekleriniz

[Veri kaynaklarını izleme](../../azure-monitor/platform/data-sources-reference.md)bölümünde açıklandığı gibi ölçümler ve Günlükler hakkında uyarı alabilirsiniz. Bunlar arasında şunlar yer alır ancak bunlarla sınırlı değildir:
- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformunun durumu
- Web sitesi kullanılabilirliği için testler

Daha önce Azure Izleyici ölçümleri, Application Insights, Log Analytics ve hizmet durumu ayrı uyarı yeteneklerine sahipti. Zaman içinde Azure, hem Kullanıcı arabirimini hem de farklı uyarı yöntemlerini geliştirmiştir ve birleştirilemez. Bu birleştirme işlemi hala devam ediyor. Sonuç olarak, henüz yeni uyarılar sisteminde olmayan bazı uyarı özellikleri de vardır.  

| **Kaynağı izle** | **Sinyal türü**  | **Açıklama** | 
|-------------|----------------|-------------|
| Hizmet durumu | Etkinlik günlüğü  | Desteklenmiyor. Bkz. [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).  |
| Application Insights | Web kullanılabilirlik testleri | Desteklenmiyor. Bkz. [Web test uyarıları](../../azure-monitor/app/monitor-web-app-availability.md). Application Insights veri gönderecek şekilde işaretlenmiş Web siteleri için kullanılabilir. Bir Web sitesinin kullanılabilirliği veya yanıt verme işlemi beklentileri altında olduğunda bir bildirim alın. |

## <a name="manage-alerts"></a>Uyarıları yönetme
Bir uyarının durumunu, çözüm sürecinde nerede olduğunu belirtmek için ayarlayabilirsiniz. Uyarı kuralında belirtilen ölçütler karşılandığında, bir uyarı oluşturulur veya tetiklenir, *Yeni*durumuna sahiptir. Bir uyarıyı onayladığınızda ve kapattığınızda durumu değiştirebilirsiniz. Tüm durum değişiklikleri uyarının geçmişine depolanır.

Aşağıdaki uyarı durumları desteklenir.

| State | Açıklama |
|:---|:---|
| Yeni | Sorun algılandı ve henüz gözden geçirilmedi. |
| Onaylandı | Bir yönetici uyarıyı inceetti ve üzerinde çalışmaya başladı. |
| Kapatıldı | Sorun çözüldü. Bir uyarı kapatıldıktan sonra, başka bir durumla değiştirerek dosyayı yeniden açabilirsiniz. |

**Uyarı durumu** , **izleyici koşulunun**farklıdır ve bağımsızdır. Uyarı durumu Kullanıcı tarafından ayarlanır. İzleme koşulu sistem tarafından ayarlanır. Bir uyarı tetiklendiğinde, uyarının izleyici koşulu *tetiklenir*olarak ayarlanır. Uyarının tetiklenmesine neden olan temeldeki durum temizlediğinde, izleme koşulu *çözüldü*olarak ayarlanır. Uyarı durumu Kullanıcı tarafından değiştirilene kadar değiştirilmez. [Uyarılarınızın ve akıllı grupların durumunu değiştirme hakkında](https://aka.ms/managing-alert-smart-group-states)bilgi edinin.

## <a name="smart-groups"></a>Akıllı gruplar 
Akıllı gruplar önizlemededir. 

Akıllı gruplar, uyarı gürültüsünü azaltmaya ve sorun giderme konusunda yardımcı olabilecek makine öğrenimi algoritmalarına dayalı uyarıların toplamasıdır. Akıllı gruplar ve [akıllı gruplarınızı yönetme](https://aka.ms/managing-smart-groups) [hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups) .


## <a name="alerts-experience"></a>Uyarı deneyimi 
Varsayılan uyarılar sayfası, belirli bir zaman penceresi içinde oluşturulan uyarıların bir özetini sağlar. Her önem derecesi için her bir durum için toplam uyarı sayısını tanımlayan sütunlarla her önem derecesine ait toplam uyarı görüntüler. Bu önem derecesine göre filtrelenen [tüm uyarılar](#all-alerts-page) sayfasını açmak için tüm önem derecelerinin herhangi birini seçin.

Alternatif olarak, [REST API 'lerini kullanarak abonelikleriniz üzerinde oluşturulan uyarı örneklerini program aracılığıyla sıralayabilirsiniz](#manage-your-alert-instances-programmatically).

> [!NOTE]
   >  Yalnızca son 30 gün içinde oluşturulan uyarılara UX veya REST API 'Leri aracılığıyla erişilebilir.

[Klasik eski uyarıları](#classic-alerts)göstermez veya izlemez. Sayfayı güncelleştirmek için abonelikleri veya filtre parametrelerini değiştirebilirsiniz. 

![Uyarılar sayfası](media/alerts-overview/alerts-page.png)

Bu görünümü, sayfanın en üstündeki açılan menülerde bulunan değerler ' i seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Subscription | Uyarılarını görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak, tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Resource group | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahildir. |
| Zaman aralığı | Yalnızca seçilen zaman penceresi içinde tetiklenen uyarılar görüntülenir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Başka bir sayfa açmak için uyarılar sayfasının en üstünde bulunan aşağıdaki değerleri seçin.

| Value | Açıklama |
|:---|:---|
| Toplam uyarı sayısı | Seçilen ölçütlerle eşleşen toplam uyarı sayısı. Filtre olmadan tüm uyarılar görünümünü açmak için bu değeri seçin. |
| Akıllı gruplar | Seçili ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupların toplam sayısı. Tüm uyarılar görünümündeki akıllı gruplar listesini açmak için bu değeri seçin.
| Toplam uyarı kuralı sayısı | Seçili abonelik ve kaynak grubundaki uyarı kurallarının toplam sayısı. Seçilen abonelikte ve kaynak grubunda filtrelenmiş kurallar görünümünü açmak için bu değeri seçin.


## <a name="manage-alert-rules"></a>Uyarı kurallarını yönetin
**Kurallar** sayfasını göstermek için **Uyarı kurallarını yönet** ' e tıklayın. **Kurallar** , Azure aboneliklerinizde tüm uyarı kurallarını yönetmek için tek bir yerdir. Tüm uyarı kurallarını listeler ve hedef kaynaklara, kaynak gruplarına, kural adına veya duruma göre sıralanabilir. Uyarı kuralları da bu sayfada düzenlenebilir, etkinleştirilebilir veya devre dışı bırakılabilir.  

 ![Uyarılar-kurallar](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Uyarılar, izleme hizmeti veya sinyal türünden bağımsız olarak tutarlı bir şekilde yazılabilir. Tetiklenen tüm uyarılar ve ilgili ayrıntılar tek sayfada kullanılabilir.
 
Aşağıdaki üç adımla yeni bir uyarı kuralı oluşturursunuz:
1. Uyarı için _hedefi_ seçin.
1. Hedef için kullanılabilir sinyallerden _sinyal_ seçin.
1. Sinyalden verilere uygulanacak _mantığı_ belirtin.
 
Bu basitleştirilmiş yazma işlemi artık, bir Azure kaynağı seçmeden önce desteklenen izleme kaynağını veya sinyalleri bilmeniz için gerekli değildir. Kullanılabilir sinyallerin listesi, seçtiğiniz hedef kaynağa göre otomatik olarak filtrelenir. Ayrıca, bu hedefe göre, uyarı kuralının mantığını otomatik olarak tanımlayarak size kılavuzluk edilir.  

[Azure izleyici kullanarak uyarı oluşturma, görüntüleme ve yönetme](../../azure-monitor/platform/alerts-metric.md)konusunda uyarı kuralları oluşturma hakkında daha fazla bilgi edinebilirsiniz.

Uyarılar, birkaç Azure izleme hizmeti arasında kullanılabilir. Bu hizmetlerin her birinin nasıl ve ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. [Azure uygulamalarını ve kaynaklarını izleme](../../azure-monitor/overview.md). 


## <a name="all-alerts-page"></a>Tüm uyarılar sayfası 
Tüm uyarılar sayfasını görmek için toplam Uyarılar ' a tıklayın. Burada, seçili zaman penceresi içinde oluşturulan uyarıların bir listesini görebilirsiniz. Tek tek uyarıların bir listesini ya da uyarıları içeren akıllı grupların bir listesini görebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst kısmındaki başlık ' ı seçin.

![Tüm uyarılar sayfası](media/alerts-overview/all-alerts-page.png)

Sayfanın en üstündeki açılan menülerde bulunan aşağıdaki değerleri seçerek görünüme filtre uygulayabilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Subscription | Uyarılarını görüntülemek istediğiniz Azure aboneliklerini seçin. İsteğe bağlı olarak, tüm aboneliklerinizi seçebilirsiniz. Yalnızca seçili aboneliklerde erişiminiz olan uyarılar görünüme dahil edilir. |
| Resource group | Tek bir kaynak grubu seçin. Yalnızca seçili kaynak grubunda hedefleri olan uyarılar görünüme dahildir. |
| Kaynak türü | Bir veya daha fazla kaynak türü seçin. Yalnızca seçilen türdeki hedefleri olan uyarılar görünüme dahildir. Bu sütun yalnızca bir kaynak grubu belirtilmişse kullanılabilir. |
| Resource | Bir kaynak seçin. Yalnızca hedef olarak bu kaynağa sahip olan uyarılar görünüme dahil edilir. Bu sütun yalnızca bir kaynak türü belirtilmişse kullanılabilir. |
| severity | Bir uyarı önem derecesi seçin veya tüm önem derecelerinin uyarılarını dahil etmek için *Tümü* ' nü seçin. |
| İzleme koşulu | Bir izleyici koşulu seçin veya koşulların uyarılarını dahil etmek için *Tümü* ' nü seçin. |
| Uyarı durumu | Bir uyarı durumu seçin veya durumların uyarılarını dahil etmek için *Tümü* ' nü seçin. |
| İzleme hizmet | Bir hizmet seçin veya tüm hizmetleri dahil etmek için *Tümü* ' nü seçin. Yalnızca hizmeti hedef olarak kullanan kurallar tarafından oluşturulan uyarılar dahildir. |
| Zaman aralığı | Yalnızca seçilen zaman penceresi içinde tetiklenen uyarılar görüntülenir. Desteklenen değerler son saat, son 24 saat, son 7 gün ve son 30 gündür. |

Görüntülenecek sütunları seçmek için sayfanın üst kısmındaki **sütunları** seçin. 

## <a name="alert-details-page"></a>Uyarı ayrıntıları sayfası
Uyarı ayrıntısı sayfası, bir uyarı seçtiğinizde görüntülenir. Bu, uyarının ayrıntılarını sağlar ve durumunu değiştirmenize olanak sağlar.

![Uyarı ayrıntıları](media/alerts-overview/alert-detail2.png)

Uyarı ayrıntıları sayfası aşağıdaki bölümleri içerir.

| `Section` | Açıklama |
|:---|:---|
| Özet | Uyarı hakkındaki özellikleri ve diğer önemli bilgileri görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarıya yapılan tüm değişiklikleri listeler. Şu anda durum değişiklikleriyle sınırlı. |
| Tanılama | Uyarının dahil olduğu akıllı grup hakkında bilgi. *Uyarı sayısı* , akıllı gruba dahil edilen uyarı sayısını ifade eder. Son 30 gün içinde, uyarı listesi sayfasındaki zaman filtresinden bağımsız olarak oluşturulan aynı akıllı gruptaki diğer uyarıları içerir. Ayrıntılarını görüntülemek için bir uyarı seçin. |

## <a name="role-based-access-control-rbac-for-your-alert-instances"></a>Uyarı örneklerinizin rol tabanlı erişim denetimi (RBAC)

Uyarı örneklerinin tüketimi ve yönetimi, kullanıcının, [katkıda](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) bulunan veya [izleme okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader)için yerleşik RBAC rollerine sahip olmasını gerektirir. Bu roller, herhangi bir Azure Resource Manager kapsamında, abonelik düzeyinden kaynak düzeyindeki ayrıntılı atamalara kadar desteklenir. Örneğin, bir kullanıcının yalnızca ' ContosoVM1 ' sanal makinesi için ' katkıda bulunan ' için izleme ' erişimi varsa, yalnızca ' ContosoVM1 ' üzerinde oluşturulan uyarıları kullanabilir ve yönetebilir.

## <a name="manage-your-alert-instances-programmatically"></a>Uyarı örneklerinizi programlama yoluyla yönetme

Aboneliğinize göre oluşturulan uyarıları programlı bir şekilde sorgulamak istediğiniz birçok senaryo vardır. Bu, Azure portal dışında özel görünümler oluşturmak veya desenleri ve eğilimleri belirlemek için uyarılarınızı analiz etmek olabilir.

Aboneliklerinizde oluşturulan uyarıları, [Uyarı Yönetimi REST API](https://aka.ms/alert-management-api) veya [Uyarılar Için Azure Kaynak Grafiği REST API](https://docs.microsoft.com/rest/api/azureresourcegraph/resources/resources)kullanarak sorgulayabilirsiniz.

[Uyarılar Için Azure Kaynak grafiği REST API](https://docs.microsoft.com/rest/api/azureresourcegraph/resources/resources) , uyarı örneklerini ölçekteki sorgulamanızı sağlar. Bu, birçok abonelik genelinde oluşturulan uyarıları yönetmeniz gereken senaryolar için önerilir. 

Aşağıdaki örnek API isteği bir abonelik içindeki uyarı sayısını döndürür:

```json
{
  "subscriptions": [
    <subscriptionId>
  ],
  "query": "where type =~ 'Microsoft.AlertsManagement/alerts' | summarize count()",
  "options": {
            "dataset":"alerts"
  }
}
```
Uyarılar [' temel '](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields) alanları için sorgulanabilir.

[Uyarı Yönetimi REST API](https://aka.ms/alert-management-api) , [' uyarı bağlamı '](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) alanları da dahil olmak üzere belirli uyarılar hakkında daha fazla bilgi almak için kullanılabilir.

## <a name="classic-alerts"></a>Klasik uyarılar 

Haziran 2018 ' den önce Azure Izleyici ölçümleri ve etkinlik günlüğü uyarısı özelliği "Uyarılar (klasik)" olarak adlandırılmaktadır. 

Daha fazla bilgi için bkz. [Klasik uyarılar](./../../azure-monitor/platform/alerts-classic.overview.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı gruplar hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups)
- [Eylem grupları hakkında bilgi edinin](../../azure-monitor/platform/action-groups.md)
- [Azure 'da uyarı örneklerinizi yönetme](https://aka.ms/managing-alert-instances)
- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)






