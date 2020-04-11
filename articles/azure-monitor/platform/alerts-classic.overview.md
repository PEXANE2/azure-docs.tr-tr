---
title: Azure Monitör'de klasik uyarılara genel bakış
description: Klasik uyarılar küçümsenmektedir. Uyarılar, Azure kaynak ölçümlerini, olaylarını veya günlüklerini izlemenize ve belirttiğiniz bir koşul yerine getirildiğinde bilgilendirilmenize olanak tanır.
ms.topic: conceptual
ms.date: 05/19/2018
ms.subservice: alerts
ms.openlocfilehash: e9c269db870f582c176783a4654b5de251e24412
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114498"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft Azure'daki klasik uyarılar nedir?

> [!NOTE]
> Bu makalede, eski klasik metrik uyarıların nasıl oluşturulacağını açıklanmaktadır. Azure Monitor artık [daha yeni neredeyse gerçek zamanlı metrik uyarıları ve yeni bir uyarı deneyimini](../../azure-monitor/platform/alerts-overview.md)destekliyor. Klasik uyarılar, henüz yeni uyarıları desteklemeyen kaynaklar için sınırlı kullanımda olmasına [rağmen, kullanımdan](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement)kaldırılmışdurumdadır. 
>

Uyarılar, koşullar en son izleme verileriyle eşleştiğinde koşulları veriler üzerinden yapılandırmanıza ve bildirime dönüşmenize olanak sağlar.

## <a name="old-and-new-alerting-capabilities"></a>Eski ve yeni uyarı özellikleri

Geçmişte Azure Monitör, Uygulama Öngörüleri, Log Analytics ve Hizmet Durumu ayrı uyarı özelliklerine sahipti. Fazla mesai, Azure hem kullanıcı arabirimini hem de farklı uyarı yöntemlerini geliştirdi ve birleştirdi. Konsolidasyon hala devam ediyor.

Klasik uyarıları yalnızca Azure Portalı'ndaki klasik uyarılar kullanıcı ekranında görüntüleyebilirsiniz. Bu ekranı, uyarılar ekranındaki **klasik uyarıları Görüntüle** düğmesinden alırsınız. 

 ![Azure portalında uyarı seçenekleri](media/alerts-classic.overview/monitor-alert-screen2.png)

Yeni uyarılar kullanıcı deneyimi klasik uyarılar deneyimi üzerinde aşağıdaki avantajlara sahiptir:
- **Daha iyi bildirim sistemi** - Tüm yeni uyarılar, birden çok uyarıda yeniden kullanılabilen bildirim grupları ve eylemler olarak adlandırılan eylem gruplarını kullanır. Klasik metrik uyarılar ve eski Log Analytics uyarıları eylem grupları kullanmaz.
- **Birleştirilmiş yazma deneyimi** - Azure Monitor, Log Analytics ve Application Insights'ta ölçümler, günlükler ve etkinlik günlüğü için tüm uyarı oluşturma tek bir yerdedir.
- **Azure portalında ateşlenen Log Analytics uyarılarını görüntüleyin** - Artık aboneliğinizde ateşlenen Log Analytics uyarılarını da görebilirsiniz. Daha önce bu ayrı bir portal vardı.
- **Ateşuyarılarının ve uyarı kurallarının ayrılması** - Uyarı kuralları (bir uyarıyı tetikleyen koşulun tanımı) ve Ateş uyarıları (uyarı kuralının devreye sönme sinin bir örneği) farklılaştırılır, böylece operasyonel ve yapılandırma görünümleri ayrılır.
- **Daha iyi iş akışı** - Deneyim yazma yeni uyarılar uyarı kuralı yapılandırma süreci boyunca kullanıcı kılavuzlar, hangi daha kolay uyarı almak için doğru şeyleri keşfetmek için yapar.
- **Akıllı Uyarılar konsolidasyonu** ve **uyarı durumunu ayarlama** - Yeni uyarılar, kullanıcı arabirimindeki aşırı yükü azaltmak için benzer uyarıları birlikte gösteren otomatik gruplandırma işlevini içerir. 

Yeni metrik uyarılar, klasik metrik uyarılar üzerinde aşağıdaki avantajlara sahiptir:
- **Geliştirilmiş gecikme :** Yeni metrik uyarılar her dakika kadar sık çalıştırılabilir. Eski metrik uyarılar her zaman 5 dakikalık bir frekansta çalışır. Yeni uyarılar, sorun oluşumundan bildirim veya eyleme (3 ila 5 dakika) kadar daha küçük gecikmeleri artırır. Eski uyarılar türüne bağlı olarak 5 ila 15 dakikadır.  Günlük uyarıları genellikle günlükleri yutmak için gereken süre nedeniyle 10 ila 15 dakikalık gecikme var, ancak yeni işleme yöntemleri bu süreyi azaltıyor. 
- **Çok boyutlu ölçümler için destek**: Ölçümün ilginç bir bölümünü izlemenize olanak tanıyan boyutsal ölçümler konusunda uyarıda bulunabilirsiniz.
- **Metrik koşullar üzerinde daha fazla denetim**: Daha zengin uyarı kuralları tanımlayabilirsiniz. Yeni uyarılar, ölçümlerin maksimum, minimum, ortalama ve toplam değerlerini izlemeyi destekler.
- **Birden çok ölçümün birlikte izlenmesi**: Birden çok ölçümü (şu anda en fazla iki ölçüm) tek bir kuralla izleyebilirsiniz. Her iki ölçüm de belirtilen zaman dilimi için ilgili eşikleri ihlal ederse bir uyarı tetiklenir.
- **Daha iyi bildirim sistemi**: Tüm yeni uyarılar, birden çok uyarıda yeniden kullanılabilen bildirim grupları ve eylemler olarak adlandırılan [eylem gruplarını](../../azure-monitor/platform/action-groups.md)kullanır.  Klasik metrik uyarılar ve eski Log Analytics uyarıları eylem grupları kullanmaz. 
- **Günlüklerden ölçümler** (genel önizleme): Log Analytics'e giden günlük verileri artık ayıklanabilir ve Azure Monitor ölçümlerine dönüştürülebilir ve diğer ölçümler de olduğu gibi uyarılabilir. Klasik uyarılara özgü terminoloji için [Uyarılar (klasik)](alerts-classic.overview.md) konusuna bakın. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Azure Monitor verilerinde klasik uyarılar
İki tür klasik uyarı vardır : metrik uyarılar ve etkinlik günlüğü uyarıları.

* **Klasik metrik uyarılar** - Bu uyarı, belirli bir metnin değeri atadığınız bir eşiği geçtiğinde tetikler. Bu eşik aşıldığında ve uyarı koşulu karşılandığında uyarı bir bildirim oluşturur. Bu noktada, uyarı "Etkin" olarak kabul edilir. "Çözüldü" olduğunda başka bir bildirim oluşturur - yani eşik tekrar aşıldığında ve koşul artık karşılanmadığında.

* **Klasik etkinlik günlüğü uyarıları** - Filtre ölçütlerinize uyan bir Etkinlik Günlüğü olay girişinde tetiklenen akış günlüğü uyarısı. Bu uyarıların yalnızca bir durumu vardır: "Etkinleştirilmiş". Uyarı motoru, filtre ölçütlerini herhangi bir yeni olaya uygular. Eski girişleri bulmak için arama yapmaz. Bu uyarılar, yeni bir Hizmet Durumu olayı meydana geldiğinde veya bir kullanıcı veya uygulama aboneliğinizde "Sanal makineyi sil" gibi bir işlem gerçekleştirdiğinde sizi bilgilendirebilir.

Azure Monitor üzerinden kullanılabilen kaynak günlüğü verileri için, verileri Log Analytics'e yönlendirin ve günlük sorgu uyarısı kullanın. Log Analytics artık [yeni uyarı yöntemini](../../azure-monitor/platform/alerts-overview.md) kullanıyor 

Aşağıdaki diyagram, Azure Monitor'daki veri kaynaklarını ve kavramsal olarak bu verileri nasıl uyarabileceğinizi özetler.

![Uyarılar açıklandı](media/alerts-classic.overview/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Uyarıların taksonomisi (klasik)
Azure, klasik uyarıları ve işlevlerini açıklamak için aşağıdaki terimleri kullanır:
* **Uyarı** - karşılandığında etkinleştirilen ölçütlerin tanımı (bir veya daha fazla kural veya koşul).
* **Etkin** - klasik bir uyarı tarafından tanımlanan ölçütler karşılandığında durum.
* **Çözüldü** - klasik bir uyarı tarafından tanımlanan ölçütler daha önce karşılandıktan sonra artık karşılanmadığında durum.
* **Bildirim** - klasik bir uyarının etkin hale gelmesine dayalı olarak alınan eylem.
* **Eylem** - bir bildirimin alıcısına gönderilen belirli bir arama (örneğin, bir adresi e-postayla gönderme veya webhook URL'sine gönderme). Bildirimler genellikle birden çok eylemi tetikleyebilir.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Azure Monitor klasik bir uyarıdan nasıl bildirim alabiliyorum?
Tarihsel olarak, azure uyarıları farklı hizmetlerden kendi yerleşik bildirim yöntemlerini kullanır. 

Azure *Monitor, eylem grupları*adı verilen yeniden kullanılabilir bir bildirim grubu oluşturdu. Eylem grupları bir bildirim için bir alıcı kümesi belirtir. Eylem Grubu'na başvuran bir uyarı etkinleştirildiğinde, tüm alıcılar bu bildirimi alır. Eylem grupları, birçok uyarı nesnesi arasında alıcıların (örneğin, çağrı daki mühendis listeniz) bir gruplamasını yeniden kullanmanıza olanak sağlar. Eylem grupları, e-posta adreslerine, SMS numaralarına ve bir dizi diğer eyleme ek olarak bir webhook URL'sine göndererek bildirimi destekler.  Daha fazla bilgi için [eylem gruplarına](../../azure-monitor/platform/action-groups.md)bakın. 

Eski klasik Etkinlik Günlüğü uyarıları eylem grupları kullanır.

Ancak, eski metrik uyarılar eylem grupları kullanmayın. Bunun yerine, aşağıdaki eylemleri yapılandırabilirsiniz: 
- Hizmet yöneticisine, yardımcı yöneticilere veya belirttiğiniz ek e-posta adreslerine e-posta bildirimleri gönderin.
- Ek otomasyon eylemleri başlatmanızı sağlayan bir webhook'u arayın.

Webhooks otomasyon ve düzeltme sağlar, örneğin, kullanarak:
- Azure Otomasyonu Runbook
- Azure İşlevi
- Azure Mantık Uygulaması
- bir üçüncü taraf hizmeti

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları hakkında bilgi alın ve bunları kullanarak yapılandırın:

* [Ölçümler](data-platform.md) hakkında daha fazla bilgi edinin
* Azure [portalı üzerinden klasik Metrik Uyarıları](alerts-classic-portal.md) yapılandırma
* Klasik [Metrik Uyarıları PowerShell'i](alerts-classic-portal.md) yapılandırın
* Yapılandırma [klasik Metrik Uyarılar Komut satırı arabirimi (CLI)](alerts-classic-portal.md)
* Yapılandırma [klasik Metrik Uyarılar Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* [Etkinlik Günlüğü](platform-logs-overview.md) hakkında daha fazla bilgi edinin
* Azure [portalı üzerinden Etkinlik Günlüğü Uyarılarını](activity-log-alerts.md) Yapılandırma
* Kaynak [Yöneticisi aracılığıyla Etkinlik Günlüğü Uyarılarını](alerts-activity-log.md) Yapılandırma
* Etkinlik [günlüğü uyarısı webhook şema](activity-log-alerts-webhook.md) sını gözden geçirin
* [Eylem grupları](action-groups.md) hakkında daha fazla bilgi edinin
* Yeni [Uyarıları](alerts-metric.md) Yapılandırma
