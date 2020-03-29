---
title: Azure sanal makine ölçek kümeleriyle otomatik ölçeklendirmeye genel bakış
description: Performansa veya sabit bir zamanlamaya göre ayarlanan Azure sanal makine ölçeğini otomatik olarak ölçeklendirmenin farklı yolları hakkında bilgi edinin
author: cynthn
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eb96be187502afcccfd3fb2c88f709facfbc3b59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278135"
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleriyle otomatik ölçeklendirmeye genel bakış
Azure sanal makine ölçeği kümesi, uygulamanızı çalıştıran VM örneklerinin sayısını otomatik olarak artırabilir veya azaltabilir. Bu otomatik ve elastik davranış, uygulamanızın performansını izlemek ve optimize etmek için yönetim ek yüküazaltır. Olumlu bir müşteri deneyimi için kabul edilebilir performansı tanımlayan kurallar oluşturursunuz. Bu tanımlanan eşikler karşılandığında, otomatik ölçek kuralları ölçek kümenizin kapasitesini ayarlamak için harekete geçer. Ayrıca, belirli zamanlarda ölçek kümenizin kapasitesini otomatik olarak artıracak veya azaltacak olayları zamanlayabilirsiniz. Bu makalede, hangi performans ölçümlerinin kullanılabilir olduğu ve otomatik ölçeklendirmenin hangi eylemleri gerçekleştirebileceğine genel bir bakış sunulmaktadır.


## <a name="benefits-of-autoscale"></a>Otomatik ölçek faydaları
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz.

Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya bellek gibi hangi ölçümlerin izlenecek, uygulama yükünün belirli bir eşiğe ne kadar süre yle karşılaması gerektiğini ve ölçek kümesine kaç VM örneği ekleyeceğini denetlersiniz.

Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Yük belirli bir süreye yayılarak tutarlı şekilde azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi ölçek kümenizi çalıştırma maliyetini azaltır.


## <a name="use-host-based-metrics"></a>Ana bilgisayar tabanlı ölçümleri kullanma
VM örneklerinizden edinilebilen yerleşik ana bilgisayar ölçümlerini içeren otomatik ölçeklendirme kuralları oluşturabilirsiniz. Ana bilgisayar ölçümleri, ek aracılar ve veri koleksiyonları yüklemenize veya yapılandırmaya gerek kalmadan, VM örneklerinin bir ölçek kümesindeki performansında görünürlük sağlar. Bu ölçümleri kullanan otomatik ölçeklendirme kuralları, CPU kullanımına, bellek talebine veya disk erişimine yanıt olarak vm örneğinin sayısını ölçekleyebilir veya bu sayıda ölçeklendirilebilir.

Konak tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları aşağıdaki araçlarla oluşturulabilir:

- [Azure portalında](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure şablonu](tutorial-autoscale-template.md)

Daha ayrıntılı performans ölçümleri kullanan otomatik ölçeklendirme kuralları oluşturmak için, Azure tanılama uzantısını VM örneklerine [yükleyebilir ve yapılandırabilir](#in-guest-vm-metrics-with-the-azure-diagnostics-extension) veya [uygulama kullanımıuygulama kullanımıNızı Uygulama Öngörüleri'ni yapılandırabilirsiniz.](#application-level-metrics-with-app-insights)

Ana bilgisayar tabanlı ölçümleri, Azure tanı uzantısı ile konuk içi VM ölçümlerini ve App Insights'ı kullanan otomatik ölçeklendirme kuralları aşağıdaki yapılandırma ayarlarını kullanabilir.

### <a name="metric-sources"></a>Metrik kaynaklar
Otomatik ölçeklendirme kuralları aşağıdaki kaynaklardan birinden ölçümler kullanabilir:

| Ölçüm kaynağı        | Kullanım örneği                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| Geçerli ölçek kümesi    | Ek aracıların yüklenmesini veya yapılandırılmasını gerektirmeyen ana bilgisayar tabanlı ölçümler için.                                  |
| Depolama hesabı      | Azure tanı uzantısı, performans ölçümlerini azure depolamasına yazar ve daha sonra otomatik ölçeklendirme kurallarını tetiklemek için tüketilir. |
| Service Bus Kuyruğu    | Uygulamanız veya diğer bileşenler, kuralları tetiklemek için bir Azure Hizmet Veri Yolundan iletileri iletebilir.                   |
| Application Insights | Uygulamanızda ölçümleri doğrudan uygulamadan aktaran bir enstrümantasyon paketi yüklenir.                         |


### <a name="autoscale-rule-criteria"></a>Otomatik ölçek kuralı ölçütleri
Otomatik ölçek kuralları oluşturduğunuzda, aşağıdaki ana bilgisayar tabanlı ölçümler kullanılabilir. Azure tanı uzantısını veya App Insights'ı kullanıyorsanız, otomatik ölçeklendirme kurallarıyla hangi ölçümlerin izlendiğini ve kullanılacağını tanımlarsınız.

| Ölçüm adı               |
|---------------------------|
| CPU yüzdesi            |
| Ağ Girişi                |
| Ağ Çıkışı               |
| Disk Okuma Baytları           |
| Disk Yazma Baytları          |
| Disk Okuma İşlemleri/Sn  |
| Disk Yazma İşlemleri/Sn |
| CPU Kredileri Kalan     |
| Cpu Kredileri Tüketilen      |

Belirli bir ölçütü izlemek için otomatik ölçeklendirme kuralları oluşturduğunuzda, kurallar aşağıdaki ölçümlerin toplama eylemlerinden birine bakar:

| Toplama türü |
|------------------|
| Ortalama          |
| Minimum          |
| Maksimum          |
| Toplam            |
| Son             |
| Sayı            |

Ölçümler tanımlanan eşiğinizle aşağıdaki işleçlerden biriyle karşılaştırıldığında otomatik ölçeklendirme kuralları tetiklenir:

| İşleç                 |
|--------------------------|
| Büyüktür             |
| Büyüktür veya eşittir |
| Küçüktür                |
| Küçüktür veya eşittir    |
| Eşittir                 |
| Eşit değildir             |


### <a name="actions-when-rules-trigger"></a>Kurallar tetiklendiğinde eylemler
Otomatik ölçek kuralı tetiklendiğinde, ölçek kümeniz otomatik olarak aşağıdaki yollardan birinde ölçeklenebilir:

| Ölçek lendirme işlemi     | Kullanım örneği                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Sayıyı şu kadar artır   | Oluşturmak için sabit sayıda VM örneği. Daha az sayıda VM içeren ölçek kümelerinde kullanışlıdır.                                           |
| Yüzde artırın | VM örneklerinin yüzde tabanlı bir artışı. Sabit bir artışın performansı belirgin şekilde artıramayabileceği daha büyük ölçekli kümeler için iyidir. |
| Sayıyı artırmak   | İstenilen maksimum tutara ulaşmak için gereken sayıda VM örneği oluşturun.                                                            |
| Sayıyı şu kadar azalt   | Kaldırılacak sabit sayıda VM örneği. Daha az sayıda VM içeren ölçek kümelerinde kullanışlıdır.                                           |
| Yüzde yi azaltma | VM örneklerinin yüzde tabanlı düşüşü. Sabit bir artışın kaynak tüketimini ve maliyetlerini belirgin şekilde azaltmayabileceği daha büyük ölçekli kümeler için iyidir. |
| Sayıyı azaltmak   | İstenilen minimum tutara ulaşmak için birçok VM örneğini kaldırın.                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Azure tanılama uzantısı ile konuk içi VM ölçümleri
Azure tanılama uzantısı, Bir VM örneğinin içinde çalışan bir aracıdır. Aracı, performans ölçümlerini izler ve Azure depolama alanına kaydeder. Bu performans ölçümleri, diskler için *AverageReadTime* veya CPU için *PercentIdleTime* gibi VM'nin durumu hakkında daha ayrıntılı bilgiler içerir. Sadece CPU kullanım veya bellek tüketimi yüzdesini değil, VM performansıyla ilgili daha ayrıntılı bir farkındalığa dayalı otomatik ölçeklendirme kuralları oluşturabilirsiniz.

Azure tanılama uzantısını kullanmak için, VM örnekleriniz için Azure depolama hesapları oluşturmanız, Azure tanılama aracısını yüklemeniz ve ardından depolama hesabına belirli performans sayaçları aktaracak şekilde VM'leri yapılandırmanız gerekir.

Daha fazla bilgi için Azure tanılama uzantısını [Linux VM](../virtual-machines/extensions/diagnostics-linux.md) veya [Windows VM](../virtual-machines/extensions/diagnostics-windows.md) için etkinleştirme makalelerini inceleyebilirsiniz.


## <a name="application-level-metrics-with-app-insights"></a>App Insights ile uygulama düzeyinde ölçümler
Uygulamalarınızın performansı hakkında daha fazla görünürlük elde etmek için Uygulama Öngörüleri'ni kullanabilirsiniz. Uygulamanızda uygulamayı izleyen ve Azure'a telemetri gönderen küçük bir enstrümantasyon paketi yüklersiniz. Uygulamanızın yanıt süreleri, sayfa yükleme performansı ve oturum sayıları gibi ölçümleri izleyebilirsiniz. Bu uygulama ölçümleri, müşteri deneyimini etkileyebilecek işlem yapılabilir öngörülere dayalı kuralları tetiklediğiniz için parçalı ve katıştırılmış düzeyde otomatik ölçeklendirme kuralları oluşturmak için kullanılabilir.

App Insights hakkında daha fazla bilgi için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).


## <a name="scheduled-autoscale"></a>Zamanlanmış otomatik ölçek
Zamanlamaları temel alan otomatik ölçeklendirme kuralları da oluşturabilirsiniz. Bu zamanlama tabanlı kurallar, belirli zamanlarda VM örneklerinin sayısını otomatik olarak ölçeklendirmenize olanak sağlar. Performans ayarı kurallarıyla, otomatik ölçek kuralları tetiklenmeden ve yeni VM örnekleri sağverilmeden önce uygulama üzerinde bir performans etkisi olabilir. Bu tür bir talebi tahmin edebilirseniz, ek VM örnekleri verilir ve ek müşteri kullanımı ve uygulama talebi için hazırdır.

Aşağıdaki örnekler, zamanlama tabanlı otomatik ölçeklendirme kurallarının kullanımından yararlanabilecek senaryolardır:

- Müşteri talebinin arttığı iş gününün başındaki VM örneklerinin sayısını otomatik olarak ölçeklendirin. Çalışma gününün sonunda, uygulama kullanımı düşük olduğunda bir gecede kaynak maliyetlerini en aza indirmek için otomatik olarak VM örneklerinin sayısını ölçeklendirin.
- Bir departman ayın belirli bölümlerinde veya mali döngüde yoğun bir uygulama kullanıyorsa, ek taleplerini karşılamak için VM örneklerinin sayısını otomatik olarak ölçeklendirin.
- Bir pazarlama etkinliği, promosyon veya tatil satışı olduğunda, beklenen müşteri talebinden önce VM örneklerinin sayısını otomatik olarak ölçeklendirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki araçlardan biriyle ana bilgisayar tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları oluşturabilirsiniz:

- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure şablonu](tutorial-autoscale-template.md)

Bu genel bakış, ölçek kümenizdeki VM örneklerinin *sayısını* yatay olarak ölçeklendirmek veya azaltmak için otomatik ölçeklendirme kurallarının nasıl kullanılacağını ayrıntılı olarak açıklanmıştır. VM örnek *boyutunu*artırmak veya azaltmak için dikey olarak ölçeklendirebilirsiniz. Daha fazla bilgi için Sanal [Makine Ölçeği kümeleriyle Dikey otomatik ölçeklendirmeye](virtual-machine-scale-sets-vertical-scale-reprovision.md)bakın.

VM örneklerinizi nasıl yöneteceğimiz hakkında bilgi için [Azure PowerShell ile sanal makine ölçeği kümelerini yönet'e](virtual-machine-scale-sets-windows-manage.md)bakın.

Otomatik ölçeklendirme kurallarınız tetiklendiğinde nasıl uyarı oluşturacağınızı öğrenmek için Azure [Monitor'da e-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanın'a](../azure-monitor/platform/autoscale-webhook-email.md)bakın. [Azure Monitor'da e-posta ve webhook uyarı bildirimleri göndermek için denetim günlüklerini de kullanabilirsiniz.](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
