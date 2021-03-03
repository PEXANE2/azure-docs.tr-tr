---
title: Bulut hizmeti (klasik) ayırma hatalarında sorun giderme | Microsoft Docs
description: Azure Cloud Services dağıtırken bir ayırma hatası giderme. Ayırmanın nasıl çalıştığını ve ayırmanın neden başarısız olabileceğini öğrenin.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 95fe4a8e1f6c6ee5f519311f8e756be89a09acf8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101738319"
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-classic-in-azure"></a>Azure 'da Cloud Services (klasik) dağıtırken ayırma hatası giderme

> [!IMPORTANT]
> [Azure Cloud Services (genişletilmiş destek)](../cloud-services-extended-support/overview.md) , Azure Cloud Services ürünü için yeni bir Azure Resource Manager tabanlı dağıtım modelidir.Bu değişiklik ile Azure Service Manager tabanlı dağıtım modelinde çalışan Azure Cloud Services, Cloud Services (klasik) olarak yeniden adlandırıldı ve tüm Yeni dağıtımlar [Cloud Services kullanmalıdır (genişletilmiş destek)](../cloud-services-extended-support/overview.md).


## <a name="summary"></a>Özet

Bir bulut hizmetine örnekler dağıtırken veya yeni Web ya da çalışan rolü örnekleri eklediğinizde Microsoft Azure işlem kaynaklarını ayırır. Azure abonelik limitlerine ulaşmadan önce bu işlemleri gerçekleştirirken zaman zaman bir hata alabilirsiniz. Bu makalede, bazı yaygın ayırma hatalarının nedenleri açıklanmaktadır ve olası düzeltme önerilir. Ayrıca, hizmetlerinizin dağıtımını planlarken bilgiler de yararlı olabilir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Arka plan – ayırmanın nasıl çalıştığı

Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Birden çok kümede yeni bir bulut hizmeti ayırma isteği yapılmaya çalışıldı. İlk örnek bir bulut hizmetine dağıtıldığında (herhangi bir hazırlama veya üretimde), bu bulut hizmeti bir kümeye sabitlenir. Bulut hizmeti için başka dağıtımlar da aynı kümede olur. Bu makalede, "bir kümeye sabitlenmiş" olarak bu makaleye başvuracağız. Aşağıdaki şekil 1 ' de, birden çok kümede denendiği normal ayırmanın durumu gösterilmektedir; Diyagram 2 ' de, var olan bulut hizmeti CS_1 barındırıldığından, küme 2 ' ye sabitlenmiş bir ayırmanın durumu gösterilmektedir.

![Ayırma diyagramı](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Ayırma hatası neden meydana gelir

Bir ayırma isteği bir kümeye sabitlendiğinde, kullanılabilir kaynak havuzu bir kümeyle sınırlı olduğundan, ücretsiz kaynakları bulmasının daha yüksek bir olasılığı vardır. Ayrıca, ayırma isteğiniz bir kümeye sabitlenir ancak istediğiniz kaynak türü bu küme tarafından desteklenmiyorsa, kümede boş kaynak olsa bile isteğiniz başarısız olur. Aşağıdaki şekil 3 ' te, tek aday kümenin ücretsiz kaynakları olmadığından, sabitlenmiş bir ayırmanın başarısız olduğu durum gösterilmektedir. Diyagram 4 ' te, küme boş kaynaklara sahip olsa bile, tek aday kümesi istenen VM boyutunu desteklemediğinden, sabitlenmiş bir ayırmanın başarısız olduğu durum gösterilmektedir.

![Sabitlenmiş ayırma hatası](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Cloud Services için ayırma başarısızlığının sorunlarını giderme

### <a name="error-message"></a>Hata İletisi

Azure portal, bulut hizmetinize gidin ve kenar çubuğu 'nda günlükleri görüntülemek için *işlem günlükleri (klasik)* seçeneğini belirleyin.

Aşağıdaki özel durumlar için daha fazla çözüm görün:

|Özel durum türü  |Hata İletisi  |Çözüm  |
|---------|---------|---------|
|Fabricınternalservererror     |İşlem, ' InternalError ' hata kodu ile başarısız oldu ve "sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin. '.|[Fabricınternalservererror sorunlarını giderme](cloud-services-troubleshoot-fabric-internal-server-error.md)|
|ServiceAllocationFailure     |İşlem, ' InternalError ' hata kodu ile başarısız oldu ve "sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin. '.|[ServiceAllocationFailure sorunlarını giderme](cloud-services-troubleshoot-fabric-internal-server-error.md)|
|LocationNotFoundForRoleSize     |' ' İşlemi `{Operation ID}` başarısız oldu: ' Istenen VM katmanı Şu anda `{Region ID}` Bu abonelik için bölgede () kullanılamıyor. Lütfen başka bir katman deneyin veya farklı bir konuma dağıtın. '.|[LocationNotFoundForRoleSize sorunlarını giderme](cloud-services-troubleshoot-location-not-found-for-role-size.md)|
|ConstrainedAllocationFailed     |' ' Azure işlemi `{Operation ID}` , kod işlem. ConstrainedAllocationFailed ile başarısız oldu. Ayrıntılar: ayırma başarısız oldu; istekteki kısıtlamalar karşılayamadı. İstenen yeni hizmet dağıtımı bir Benzeşim Grubuna bağlı veya bir Sanal Ağı hedefliyor ya da bu barındırılan hizmetin altında mevcut bir dağıtım var. Bu koşullardan herhangi biri, yeni dağıtımı belirli Azure kaynaklarına kısıtlar. Lütfen daha sonra yeniden deneyin veya sanal makine boyutunu ya da rol örneği sayısını azaltmayı deneyin. Alternatif olarak, mümkünse, yukarıda belirtilen kısıtlamaları kaldırın veya farklı bir bölgeye dağıtım yapmayı deneyin.|[ConstrainedAllocationFailed sorunlarını giderme](cloud-services-troubleshoot-constrained-allocation-failed.md)|
|OverconstrainedAllocationRequest     |Bu dağıtım için gerekli VM boyutu (veya VM boyutlarının birleşimi), dağıtım isteği kısıtlamaları nedeniyle sağlanamıyor. Mümkünse, sanal ağ bağlamaları gibi kısıtlamaları yeniden deneyin, içinde başka bir dağıtım olmadan barındırılan bir hizmete dağıtıp, farklı bir benzeşim grubuna veya benzeşim grubu olmadan veya farklı bir bölgeye dağıtım yapmayı deneyin.|[OverconstrainedAllocationRequest sorunlarını giderme](cloud-services-troubleshoot-overconstrained-allocation-request.md)|

Örnek hata iletisi:

> "' {Operation ID} ' adlı Azure işlemi, kod Işlem. ConstrainedAllocationFailed ile başarısız oldu. Ayrıntılar: ayırma başarısız oldu; istekteki kısıtlamalar karşılayamadı. İstenen yeni hizmet dağıtımı bir Benzeşim Grubuna bağlı veya bir Sanal Ağı hedefliyor ya da bu barındırılan hizmetin altında mevcut bir dağıtım var. Bu koşullardan herhangi biri, yeni dağıtımı belirli Azure kaynaklarına kısıtlar. Lütfen daha sonra yeniden deneyin veya sanal makine boyutunu ya da rol örneği sayısını azaltmayı deneyin. Alternatif olarak, mümkünse, belirtilen kısıtlamaları kaldırın veya farklı bir bölgeye dağıtımı deneyin. "

### <a name="common-issues"></a>Sık Karşılaşılan Sorunlar

Bir ayırma isteğinin tek bir kümeye sabitlenememesine neden olan ortak ayırma senaryoları aşağıda verilmiştir.

* Hazırlama yuvasına dağıtım-bir bulut hizmetinin her iki yuvada da bir dağıtımı varsa, tüm bulut hizmeti belirli bir kümeye sabitlenmiştir.  Diğer bir deyişle, üretim yuvasında bir dağıtım zaten varsa, yeni bir hazırlama dağıtımı yalnızca üretim yuvası ile aynı kümede ayrılabilir. Küme kapasiteye yaklaştığı takdirde istek başarısız olabilir.
* Ölçeklendirme - Mevcut bir bulut hizmetine yeni örnekler eklendiğinde aynı kümede ayrılmalıdır.  Küçük ölçeklendirme istekleri her zaman olmasa da genellikle ayrılabilir. Küme kapasiteye yaklaştığı takdirde istek başarısız olabilir.
* Benzeşim grubu-bulut hizmeti bir benzeşim grubuna sabitlenmemişse, boş bir bulut hizmetine yeni bir dağıtım, bu bölgedeki herhangi bir kümede yapı tarafından ayrılabilir. Aynı benzeşim grubuna yönelik dağıtımlar aynı kümede denenmeyecektir. Küme kapasiteye yaklaştığı takdirde istek başarısız olabilir.
* Benzeşim grubu vNet-daha eski sanal ağlar bölgeler yerine benzeşim gruplarına bağlandı ve bu sanal ağlardaki bulut hizmetleri benzeşim grubu kümesine sabitlenmiştir. Bu sanal ağ türüne yapılan dağıtımlar sabitlenmiş kümede denenmeyecektir. Küme kapasiteye yaklaştığı takdirde istek başarısız olabilir.

## <a name="solutions"></a>Çözümler

1. Yeni bir bulut hizmetine yeniden dağıtın-bu çözüm, platformun bu bölgedeki tüm kümelerden seçim yapmasına izin verdiğinden en başarılı olma olasılığı yüksektir.

   * İş yükünü yeni bir bulut hizmetine dağıtma  
   * CNAME veya bir kaydı, trafiği yeni bulut hizmetine işaret etmek üzere güncelleştirme
   * Sıfır trafik eski siteye gittikten sonra eski bulut hizmetini silebilirsiniz. Bu çözümün sıfır kapalı kalma süresi olması gerekir.
2. Hem üretim hem de hazırlama yuvalarını Sil-Bu çözüm, var olan DNS adınızı korur, ancak uygulamanızda kapalı kalma süresine yol açacaktır.

   * Bulut hizmetinin boş olması için mevcut bir bulut hizmetinin üretim ve hazırlama yuvalarını silin ve ardından
   * Mevcut bulut hizmetinde yeni bir dağıtım oluşturun. Bu, bölgedeki tüm kümelerdeki ayırmayı yeniden dener. Bulut hizmetinin bir benzeşim grubuna bağlı olmadığından emin olun.
3. Ayrılmış IP-bu çözüm, mevcut IP adresinizi koruyacaktır, ancak uygulamanızda kesinti yapılmasına neden olur.  

   * PowerShell kullanarak mevcut dağıtımınız için bir ReservedIP oluşturma

     ```azurepowershell
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```

   * Hizmetin CSCFG öğesinde yeni ReservedIP 'yi belirttiğinizden emin olmak için yukarıdaki #2 izleyin.
4. Yeni dağıtımlar için benzeşim grubunu Kaldır-benzeşim grupları artık önerilmez. Yeni bir bulut hizmeti dağıtmak için yukarıdaki 1 numaralı öğeye yönelik adımları izleyin. Bulut hizmetinin bir benzeşim grubunda olmadığından emin olun.
5. Bölgesel sanal ağa dönüştürme- [benzeşim gruplarından bir bölgesel sanal ağa (VNet) geçiş](/previous-versions/azure/virtual-network/virtual-networks-migrate-to-regional-vnet)yapma konusuna bakın.
