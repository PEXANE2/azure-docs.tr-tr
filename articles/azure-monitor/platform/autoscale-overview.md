---
title: Sanal Makineler, Bulut Hizmetleri ve Web Uygulamalarında otomatik ölçeklere genel bakış
description: Microsoft Azure'da otomatik ölçeklendirme. Sanal Makineler, Sanal makine Ölçeği setleri, Bulut Hizmetleri ve Web Uygulamaları için geçerlidir.
ms.subservice: autoscale
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a60c03f1928b38c78a59edca4b5493307d7d19d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75364382"
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Microsoft Azure Sanal Makineler, Cloud Services ve Web Apps için otomatik ölçeklendirmeye genel bakış
Bu makalede, Microsoft Azure otomatik ölçeknesinin ne olduğu, avantajları ve kullanmaya nasıl başlanolduğu açıklanmaktadır.  

Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri,](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)için geçerlidir.

> [!NOTE]
> Azure'un iki otomatik ölçeklendirme yöntemi vardır. Otomatik ölçeklendirmenin eski bir sürümü Sanal Makineler (kullanılabilirlik kümeleri) için geçerlidir. Bu özellik sınırlı desteğe sahiptir ve daha hızlı ve daha güvenilir otomatik ölçek desteği için sanal makine ölçeği kümelerine geçiş yapmanızı öneririz. Eski teknolojinin nasıl kullanılacağına ilişkin bir bağlantı bu makalede yer almaktadır.  
>
>

## <a name="what-is-autoscale"></a>Otomatik ölçek nedir?
Otomatik ölçeklendirme, uygulamanızdaki yükü işlemek için doğru miktarda kaynağa sahip olmasını sağlar. Bu yük artışları işlemek için kaynak eklemek ve aynı zamanda boşta oturan kaynakları kaldırarak para dan tasarruf sağlar. Bir dizi kurala bağlı olarak otomatik olarak VM'leri çalıştırmak ve eklemek veya kaldırmak için en az ve en fazla sayıda örnek belirtirsiniz. Minimum olması, uygulamanızın hiçbir yük altında bile her zaman çalışmasını sağlar. Maksimum olması, olası toplam saatlik maliyetinizi sınırlar. Oluşturduğunuz kuralları kullanarak bu iki uç arasında otomatik olarak ölçeklendirilirsiniz.

 ![Otomatik ölçek açıkladı. VM’leri ekleme ve kaldırma](./media/autoscale-overview/AutoscaleConcept.png)

Kural koşulları karşılandığında, bir veya daha fazla otomatik ölçeklendirme eylemi tetiklenir. VM'ler ekleyebilir ve kaldırabilir veya başka eylemler gerçekleştirebilirsiniz. Aşağıdaki kavramsal diyagram bu işlemi gösterir.  

 ![Otomatik Ölçek Akış Diyagramı](./media/autoscale-overview/Autoscale_Overview_v4.png)

Aşağıdaki açıklama önceki diyagramın parçaları için geçerlidir.   

## <a name="resource-metrics"></a>Kaynak Ölçümleri
Kaynaklar ölçümleri yontarak, bu ölçümler daha sonra kurallartarafından işlenir. Ölçümler farklı yöntemlerle gelir.
Sanal makine ölçeği kümeleri Azure tanılama aracılarından gelen telemetri verilerini kullanırken, Web uygulamaları ve Bulut hizmetleri için telemetri doğrudan Azure Altyapısı'ndan gelir. Sık kullanılan bazı istatistikler CPU Kullanımı, bellek kullanımı, iş parçacığı sayıları, sıra uzunluğu ve disk kullanımını içerir. Kullanabileceğiniz telemetri verilerinin listesi için [bkz.](../../azure-monitor/platform/autoscale-common-metrics.md)

## <a name="custom-metrics"></a>Özel Ölçümler
Ayrıca, uygulamanızın yaydığı kendi özel ölçümlerinizden de yararlanabilirsiniz. Uygulama Öngörüleri'ne metrik gönderecek şekilde uygulamanızı yapılandırmışsanız, ölçeklendirilip ölçeklendirilip ölçeklenmeyeceğine karar vermek için bu ölçümlerden yararlanabilirsiniz.

## <a name="time"></a>Zaman
Zamanlama tabanlı kurallar UTC'ye dayanır. Kurallarınızı ayarlarken saat diliminizi düzgün ayarlamanız gerekir.  

## <a name="rules"></a>Kurallar
Diyagram yalnızca bir otomatik ölçek kuralını gösterir, ancak bunların çoğuna sahip olabilirsiniz. Durumunuz için gerektiğinde karmaşık çakışan kurallar oluşturabilirsiniz.  Kural türleri şunlardır  

* **Metrik tabanlı** - Örneğin, CPU kullanımı %50'nin üzerinde olduğunda bu eylemi yapın.
* **Zaman tabanlı** - Örneğin, belirli bir saat diliminde Cumartesi günü her 08:00'de bir webhook tetikler.

Metrik tabanlı kurallar uygulama yükünü ölçer ve bu yüke göre VM ekler veya kaldırır. Zamanlama tabanlı kurallar, yüklemenizde zaman desenleri gördüğünüzde ölçeklendirmenize olanak sağlar ve olası bir yük artışı veya azalmadan önce ölçeklendirmek ister.  

## <a name="actions-and-automation"></a>Eylemler ve otomasyon
Kurallar bir veya daha fazla eylem türünü tetikleyebilir.

* **Ölçek** - VM'leri içeri veya dışarı ölçeklendir
* **E-posta** - Belirttiğiniz abonelik yöneticilerine, eş yöneticilere ve/veya ek e-posta adresine e-posta gönderin
* **Webhooks üzerinden otomatikleştirin** - Azure içinde veya dışında birden çok karmaşık eylemi tetiklenebilen webhook'ları arayın. Azure'da Azure Otomasyon uyruyla bir çalışma kitabı, Azure İşlevi veya Azure Mantık Uygulaması başlatabilirsiniz. Azure dışındaki örnek üçüncü taraf URL'si, Slack ve Twilio gibi hizmetleri içerir.

## <a name="autoscale-settings"></a>Otomatik Ölçeklendirme Ayarları
Otomatik ölçek aşağıdaki terminoloji ve yapıyı kullanın.

- Otomatik **ölçeklendirme ayarı,** yukarı mı yoksa aşağı mı ölçeklendirilip küçültmeyeceğini belirlemek için otomatik ölçeklendirme motoru tarafından okunur. Bir veya daha fazla profil, hedef kaynak hakkında bilgi ve bildirim ayarları içerir.

  - **Otomatik ölçeklendirme profili,** aşağıdakilerin bir leşimidir:

    - örnek sayısı için en az, en büyük ve varsayılan değerleri gösteren **kapasite ayarı.**
    - her biri bir tetikleyici (zaman veya metrik) ve bir ölçek eylemi (yukarı veya aşağı) içeren **kurallar kümesi.**
    - **yineleme,** otomatik ölçek bu profili yürürlüğe koymak gerektiğini gösterir.

      Farklı çakışan gereksinimleri karşılamanızı sağlayan birden çok profiliniz olabilir. Örneğin, günün farklı saatleri veya haftanın günleri için farklı otomatik ölçeklendirme profillerine sahip olabilirsiniz.

  - **Bildirim ayarı,** otomatik ölçeklendirme ayarlarından birinin ölçütlerini karşılamaya dayalı bir otomatik ölçek olayı oluştuğunda hangi bildirimlerin oluşması gerektiğini tanımlar. Otomatik ölçeklendirme, bir veya daha fazla e-posta adresini bildirebilir veya bir veya daha fazla web hooks'a çağrı yapabilir.


![Azure otomatik ölçeklendirme ayarı, profil ve kural yapısı](./media/autoscale-overview/AzureResourceManagerRuleStructure3.png)

Yapılandırılabilir alanların ve açıklamaların tam listesi [Autoscale REST API'de](https://msdn.microsoft.com/library/dn931928.aspx)mevcuttur.

Kod örnekleri için bkz.

* [VM Ölçek Kümeleri için Kaynak Yöneticisi şablonlarını kullanarak gelişmiş Otomatik Ölçek yapılandırması](../../azure-monitor/platform/autoscale-virtual-machine-scale-sets.md)  
* [Otomatik ölçekLENDIRME REST API](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Yatay vs dikey ölçekleme
Otomatik ölçek yalnızca yatay olarak ölçeklenir, bu da VM örneklerinin sayısında bir artış ("çıkış") veya azalma ("in") anlamına gelir.  Yatay, yüklemeyi işlemek için binlerce VM çalıştırmanızı sağladığından, bulut durumunda daha esnektir.

Buna karşılık, dikey ölçekleme farklıdır. Aynı sayıda VM tutar, ancak VM'leri daha güçlü ("yukarı") veya daha az ("aşağı") güçlü kılar. Güç bellek, CPU hızı, disk alanı, vb ölçülür.  Dikey ölçeklemenin daha fazla sınırlaması vardır. Hızlı bir üst sınıra vurur ve bölgeye göre değişebilir büyük donanım, kullanılabilirliğine bağlıdır. Dikey ölçekleme de genellikle durdurmak ve yeniden başlatmak için bir VM gerektirir.


## <a name="methods-of-access"></a>Erişim yöntemleri
Otomatik ölçeklendirmeyi

* [Azure portalında](../../azure-monitor/platform/autoscale-get-started.md)
* [Powershell](../../azure-monitor/platform/powershell-quickstart-samples.md#create-and-manage-autoscale-settings)
* [Platformlar arası Komut Satırı Arabirimi (CLI)](../../azure-monitor/platform/cli-samples.md#autoscale)
* [Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Otomatik ölçeklendirme için desteklenen hizmetler
| Hizmet | Şema & Dokümanları |
| --- | --- |
| Web Apps |[Web Uygulamalarını Ölçekleme](../../azure-monitor/platform/autoscale-get-started.md) |
| Cloud Services |[Bulut Hizmetini Otomatik Ölçeklendirme](../../cloud-services/cloud-services-how-to-scale-portal.md) |
| Sanal Makineler: Klasik |[Klasik Sanal Makine Kullanılabilirlik Setlerini Ölçekleme](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Sanal Makineler: Windows Ölçek Setleri |[Windows'da sanal makine ölçek kümelerini ölçeklendirme](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) |
| Sanal Makineler: Linux Ölçek Setleri |[Linux'ta sanal makine ölçek kümelerini ölçeklendirme](../../virtual-machine-scale-sets/tutorial-autoscale-cli.md) |
| Sanal Makineler: Windows Örneği |[VM Ölçek Kümeleri için Kaynak Yöneticisi şablonlarını kullanarak gelişmiş Otomatik Ölçek yapılandırması](../../azure-monitor/platform/autoscale-virtual-machine-scale-sets.md) |
| API Yönetimi hizmeti|[Bir Azure API Management örneğini otomatik olarak ölçeklendirme](https://docs.microsoft.com/azure/api-management/api-management-howto-autoscale)

## <a name="next-steps"></a>Sonraki adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi edinmek için, daha önce listelenen Otomatik Ölçeklendirme Walkthroughs'u kullanın veya aşağıdaki kaynaklara bakın:

* [Azure Monitörotomatik ölçeklendirme ortak ölçümleri](../../azure-monitor/platform/autoscale-common-metrics.md)
* [En iyi Azure İzleyici otomatik ölçeklendirme yöntemleri](../../azure-monitor/platform/autoscale-best-practices.md)
* [E-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanma](../../azure-monitor/platform/autoscale-webhook-email.md)
* [Otomatik ölçekLENDIRME REST API](https://msdn.microsoft.com/library/dn931953.aspx)
* [Sorun Giderme Sanal Makine Ölçeği Otomatik Ölçek Ayarlar](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

