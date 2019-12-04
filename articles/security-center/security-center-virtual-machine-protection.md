---
title: Makinelerinizi ve uygulamalarınızı koruyun
description: Bu belge, güvenlik merkezi 'ndeki, sanal makinelerinizi ve bilgisayarlarınızı ve Web uygulamalarınızı ve App Service ortamlarınızı korumanıza yardımcı olan önerilere yöneliktir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 4a6d733b490edd892136f6febcc90c29a5a865e1
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74766812"
---
# <a name="protect-your-machines-and-applications"></a>Makinelerinizi ve uygulamalarınızı koruyun
Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur. 

Bu makalede, Azure Güvenlik Merkezi 'nin kaynak güvenliği bölümünün **işlem ve uygulamalar** sayfası açıklanmaktadır. Burada gördüğünüz önerilerden bazılarını da açıklar.

Işlem ve uygulama hizmetleri önerilerinin tam listesi için bkz. [işlem ve uygulama önerileri](recommendations-compute-and-apps.md).

## <a name="view-the-security-of-your-compute-and-apps-resources"></a>İşlem ve uygulama kaynaklarınızın güvenliğini görüntüleme

![Güvenlik Merkezi panosu](./media/security-center-virtual-machine-recommendations/overview.png)

İşlem ve uygulama kaynaklarınızın durumunu görüntülemek için, güvenlik merkezi kenar çubuğu 'ndaki **kaynaklar** altındaki **işlem & uygulamalar** ' ı seçin. Aşağıdaki sekmeler mevcuttur:

* **Genel bakış**: tüm bilgi işlem ve uygulama kaynaklarının yanı sıra geçerli güvenlik durumlarını listeler 

* [**VM 'ler ve bilgisayarlar**](#vms-and-computers): sanal makinelerinize, bilgisayarlarınıza ve her birine ait geçerli güvenlik durumuna yönelik önerileri listeler

* [**VM Ölçek Kümeleri**](#vmscale-sets): ölçek kümelerinizin önerilerini listeler, 

* [**Cloud Services**](#cloud-services): Güvenlik Merkezi tarafından izlenen Web ve çalışan rollerinize yönelik önerileri listeler

* [**Uygulama hizmetleri**](#app-services): App Service ortamlarınızla ilgili önerileri ve her birinin geçerli güvenlik durumunu listeler

* **Kapsayıcılar**: kapsayıcılarınızın ve yapılandırmalarının güvenlik değerlendirmelerine yönelik önerileri listeler

* **İşlem kaynakları**: Service Fabric kümeleri ve Olay Hub 'ları gibi işlem kaynaklarınızın önerilerini listeler

### <a name="whats-in-each-tab"></a>Her bir sekmede ne var?

Her sekme birden çok bölüme sahiptir ve her bölümde gösterilen öğe hakkında ek ayrıntıları görmek için ayrıntıya gidebilirsiniz.

Her sekmede, izlenen ortamınızdaki ilgili kaynakların önerilerini de görürsünüz. İlk sütunda öneri listelenir, ikincisi etkilenen Toplam kaynak sayısını gösterir ve üçüncüsü sorunun önem derecesini gösterir.

Her öneri, onu seçtikten sonra gerçekleştirebileceğiniz bir dizi eylem içerir. Örneğin, **eksik sistem güncelleştirmeleri**' ni seçerseniz, yamaları eksik olan VM 'ler ve bilgisayar sayısı ve eksik güncelleştirmenin önem derecesi görüntülenir.

> [!NOTE]
> Güvenlik önerileri, **öneriler** sayfalıdakilerle aynıdır, ancak burada seçtiğiniz belirli kaynak türüne göre filtrelenmiştir. Önerilerin nasıl çözümleneceği hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nde güvenlik önerilerini uygulama](security-center-recommendations.md).
>

### <a name="vms-and-computers"></a>VM 'Ler ve bilgisayarlar
VM 'Ler ve bilgisayarlar bölümü, sanal makinelerinize ve bilgisayarlarınıza yönelik tüm güvenlik önerilerine genel bir bakış sağlar. Dört tür makine mevcuttur:

![Azure olmayan bilgisayar](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Azure olmayan bilgisayar.

![Azure Resource Manager VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Azure Resource Manager VM.

![Azure klasik VM](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Azure klasik VM.

![Çalışma alanından tanımlanan VM 'Ler](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Bu, Bu abonelikteki çalışma alanına rapor veren diğer aboneliklerden gelen VM 'Leri ve Operations Manager doğrudan aracıyla yüklenen ve kaynak KIMLIĞI olmayan VM 'leri içerir.

Her öneri altında görüntülenen simge, dikkat gerektiren VM ve bilgisayarı ve öneri türünü hızlıca tanımanıza yardımcı olur. Ayrıca, listeyi **kaynak türüne** göre ve **önem derecesine**göre aramak için filtreleri de kullanabilirsiniz.

Her VM 'nin güvenlik önerilerinin ayrıntılarına gitmek için VM 'ye tıklayın.
Burada, VM veya bilgisayar için güvenlik ayrıntılarını görürsünüz. En altta, önerilen eylemi ve her bir sorunun önem derecesini görebilirsiniz.
![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/recommendation-list.png)

### <a name="cloud-services"></a>Bulut Hizmetleri
Bulut hizmetleri için, işletim sistemi sürümü güncel olmadığında bir öneri oluşturulur.

![Bulut hizmetleri](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

Öneriniz olan bir senaryoda, işletim sistemini güncelleştirmek için önerideki adımları izleyin. Bir güncelleştirme kullanılabilir olduğunda, bir uyarısına (sorunun önem derecesine bağlı olarak kırmızı veya turuncu) sahip olursunuz. Bu öneriye ilişkin tam açıklama için, **Açıklama** sütununun altındaki **Işletim sistemi sürümünü Güncelleştir** ' e tıklayın.

### <a name="app-services"></a>Uygulama Hizmetleri
App Service bilgilerini görüntülemek için, güvenlik merkezi 'nin standart fiyatlandırma katmanında olmanız ve aboneliğinizde App Service etkinleştirmeniz gerekir. Bu özelliği etkinleştirme hakkında yönergeler için bkz. [Azure Güvenlik Merkezi ile App Service koruma](security-center-app-services.md).


**Uygulama hizmetleri**altında, uygulama hizmeti ortamlarınızın listesini ve gerçekleştirilen değerlendirme güvenlik merkezini temel alan sistem durumu özetini bulabilirsiniz.

![Uygulama hizmetleri](./media/security-center-virtual-machine-recommendations/app-services.png)

Üç tür uygulama hizmeti görüntülenir:

![App Services ortamı](./media/security-center-virtual-machine-recommendations/ase.png) App Services ortamı

![Web uygulaması](./media/security-center-virtual-machine-recommendations/web-app.png) Web uygulaması

![İşlev uygulaması](./media/security-center-virtual-machine-recommendations/function-app.png) İşlev uygulaması

Bir Web uygulaması seçerseniz, üç sekmeden oluşan bir Özet görünümü açılır:

   - **Öneriler**: Güvenlik Merkezi tarafından gerçekleştirilen değerlendirmelere göre başarısız oldu.
   - **Başarılı değerlendirmeler**: Güvenlik Merkezi tarafından gerçekleştirilen değerlendirmelerin listesi.
   - **Kullanılamayan değerlendirmeler**: bir hata nedeniyle çalıştırımayan değerlendirmelerin listesi veya öneri belirli bir App Service için uygun değil

   **Öneriler** altında seçili Web uygulaması için önerilerin bir listesi ve her bir önerinin önem derecesi listelenmiştir.

   ![Uygulama Hizmetleri önerileri](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

Önerinin açıklamasını ve sağlıksız kaynakların, sağlıklı kaynakların ve taranmamış kaynakların bir listesini görmek için bir öneri seçin.

   - **Geçirilen değerlendirmeler** sütunu, geçirilen değerlendirmelerin bir listesini gösterir. Bu değerlendirmelerin önem derecesi her zaman yeşildir.

   - Değerlendirmede bir açıklama, sağlıksız ve sağlıklı kaynakların bir listesi ve taranmamış kaynakların bir listesi için listeden bir geçmiş değerlendirmesi seçin. Sağlıksız kaynaklar için bir sekme bulunur, ancak bu liste, değerlendirme başarılı olduğu için her zaman boştur.

### <a name="vmscale-sets"></a>Sanal Makine Ölçek Kümeleri
Güvenlik Merkezi, ölçek kümelerine sahip olup olmadığını otomatik olarak bulur ve bunlara Microsoft Monitoring Agent yüklemenizi önerir.

Microsoft Monitoring Agent yüklemek için: 

1. **İzleme aracısını sanal makine ölçek kümesine yükler**önerisini seçin. İzlenmeyen ölçek kümelerinin bir listesini alırsınız.

1. Sağlıksız bir ölçek kümesi seçin. İzleme aracısını mevcut doldurulmuş bir çalışma alanını kullanarak yüklemek veya yeni bir tane oluşturmak için yönergeleri izleyin. Ayarlı değilse, çalışma alanı [fiyatlandırma katmanını](security-center-pricing.md) ayarladığınızdan emin olun.

   ![MMS 'yi yükler](./media/security-center-virtual-machine-recommendations/install-mms.png)

Yeni ölçek kümelerini Microsoft Monitoring Agent otomatik olarak yükleyecek şekilde ayarlamak için:
1. Azure Ilkesi ' ne gidin ve **tanımlar**' a tıklayın.

1. **Windows sanal makine ölçek kümeleri için Log Analytics aracısını dağıtma** ilkesi için arama yapın ve üzerine tıklayın.

1. **Ata**'ya tıklayın.

1. **Kapsamı** ve **Log Analytics çalışma alanını** ayarlayıp **ata**' ya tıklayın.

Microsoft Monitoring Agent yüklemek için tüm mevcut ölçek kümelerini ayarlamak istiyorsanız, Azure Ilkesinde, **Düzeltme** ' ye gidin ve mevcut ilkeyi mevcut ölçek kümelerine uygulayın.


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi 'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)
