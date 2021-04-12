---
title: İki Azure Cloud Services arasında takas/geçiş (genişletilmiş destek)
description: İki Azure Cloud Services arasında takas/geçiş (genişletilmiş destek)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachandw
ms.date: 04/01/2021
ms.custom: ''
ms.openlocfilehash: 6f96656af9afd9874cc6273a9cea9ed43e8c69cc
ms.sourcegitcommit: af6eba1485e6fd99eed39e507896472fa930df4d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2021
ms.locfileid: "106294337"
---
# <a name="swapswitch-between-two-azure-cloud-services-extended-support"></a>İki Azure Cloud Services arasında takas/geçiş (genişletilmiş destek)
Cloud Services (genişletilmiş destek) ile iki bağımsız bulut hizmeti dağıtımı arasında takas yapabilirsiniz. Cloud Services (klasik) aksine, yuva kavramı Azure Resource Manager modelde yok. Bir bulut hizmetinin yeni bir sürümünü dağıtmaya karar verirken (genişletilmiş destek), bu dağıtımı kullanarak yeni yayınınızı hazırlamayabilmeniz ve test etmeniz için, başka bir mevcut bulut hizmeti (genişletilmiş destek) ile "takas edilebilir" yapabilirsiniz. Bulut hizmeti, yalnızca ikinci bulut hizmetini dağıtma sırasında (çiftin) başka bir bulut hizmeti ile ' swapyönetilebilir ' hale getirilebilir. ARM şablon tabanlı dağıtım yöntemi kullanılırken, bu işlem, bulut hizmeti nesnesinin ağ profili içindeki SwappableCloudService özelliğinin eşleştirilmiş bulut hizmeti KIMLIĞINE ayarlanarak yapılır. 

```
"networkProfile": {
 "SwappableCloudService": {
              "id": "[concat(variables('swappableResourcePrefix'), 'Microsoft.Compute/cloudServices/', parameters('cloudServicesToBeSwappedWith'))]"
            },
```
> [!Note] 
> Bulut hizmeti (klasik) ile bulut hizmeti arasında geçiş yapılamaz (genişletilmiş destek)

İki bulut hizmetinin giderildiği URL 'Leri değiştirmek için **değiştirme** 'yi kullanın, yeni bir bulut hizmetini (hazırlanmış) üretim sürümüne yükselterek etkilidir.
Dağıtımları Cloud Services sayfasından veya Panodan takas edebilirsiniz.

1. [Azure Portal](https://portal.azure.com), güncelleştirmek istediğiniz bulut hizmetini seçin. Bu adım, bulut hizmeti örneği dikey penceresini açar.
2. Dikey pencerede **takas** resmi ' ni seçin 
    :::image type="content" source="media/swap-cloud-service-1.png" alt-text="bulut hizmeti değiştirme seçeneğini gösterir":::
   
3. Aşağıdaki onay istemi açılır
   
   :::image type="content" source="media/swap-cloud-service-2.png" alt-text="Görüntü, bulut hizmetini değiştirmeyi gösterir":::
   
4. Dağıtım bilgilerini doğruladıktan sonra, dağıtımları değiştirmek için Tamam ' ı seçin.
Tek şey, iki bulut hizmeti için sanal IP adresleri (VIP) olduğundan, takas hızlı bir şekilde gerçekleşir.

İşlem maliyetlerini kaydetmek için, bulunan bulut hizmetinizin beklendiği gibi çalıştığını doğruladıktan sonra bulut hizmetlerinden birini (uygulamanızın dağıtımı için hazırlama ortamı olarak belirlenmiş) silebilirsiniz.

İki bulut hizmeti genişletilmiş destek dağıtımı arasında bir ' takas ' gerçekleştirmeye yönelik REST API 'SI aşağıda verilmiştir:
```http
POST https://management.azure.com/subscriptions/subId/providers/Microsoft.Network/locations/region/setLoadBalancerFrontendPublicIpAddresses?api-version=2020-11-01
```
```
{
  "frontendIPConfigurations": [
    {
    "id": "#LBFE1#",
    "properties": {
    "publicIPAddress": {
    "id": "#PIP2#"
    }
      }
    },
   {
    "id": "#LBFE2#",
    "properties": {
    "publicIPAddress": {
    "id": "#PIP1#"
     }
       }
    }
  ]
 }
```
## <a name="common-questions-about-swapping-deployments"></a>Dağıtımları değiştirme hakkında sık sorulan sorular

### <a name="what-are-the-prerequisites-for-swapping-between-two-cloud-services"></a>İki bulut hizmeti arasında takas önkoşulları nelerdir?
Başarılı bir bulut hizmeti (genişletilmiş destek) takası için iki temel önkoşul vardır:
* Değiştirilebilir bulut hizmetlerinden biri için statik/ayrılmış bir IP adresi kullanmak istiyorsanız, diğer bulut hizmeti de ayrılmış bir IP kullanmalıdır. Aksi takdirde, değiştirme başarısız olur.
* Değiştirme işlemini gerçekleştirebilmeniz için rollerinizin tüm örneklerinin çalışıyor olması gerekir. Örneklerinizin durumunu Azure portal genel bakış dikey penceresinde kontrol edebilirsiniz. Alternatif olarak, Windows PowerShell 'de Get-AzRole komutunu da kullanabilirsiniz.

Konuk işletim sistemi güncelleştirmeleri ve hizmet düzeltme işlemleri de dağıtım yamasının başarısız olmasına neden olabilir. Daha fazla bilgi için bkz. Cloud Service dağıtım sorunlarını giderme.

### <a name="can-i-perform-a-vip-swap-in-parallel-with-another-mutating-operation"></a>Bir VIP takas işlemini başka bir değiştirici işlemle paralel olarak gerçekleştirebilir miyim?
Hayır. VIP takas, bulut hizmetinde başka bir işlem işlemi yapılmadan önce yalnızca tamamlanmalı bir ağ değişikdir. Bulut hizmetinde bir güncelleştirme, silme veya otomatik ölçeklendirme işlemi gerçekleştirme, bir VIP takası devam ederken veya başka bir işlem işlemi devam ederken bir VIP takas tetikleniyor, bulut hizmetini kurtarmanın mümkün olmadığı istenmeyen bir durumda bırakabilir. 

### <a name="does-a-swap-incur-downtime-for-my-application-how-should-i-handle-it"></a>Bir değiştirme uygulamam için kapalı kalma süresine neden olur mu? Bunu nasıl işleymem gerekir?
Önceki bölümde açıklandığı gibi, Azure Yük dengeleyicide yalnızca bir yapılandırma değişikliği olduğu için bir bulut hizmeti takası genellikle hızlıdır. Bazı durumlarda, 10 veya daha fazla saniye sürebilir ve geçici bağlantı hatalarıyla sonuçlanır. Müşterilerinizin etkisini sınırlandırmak için, istemci yeniden deneme mantığını uygulamayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar 
- Cloud Services için [dağıtım önkoşullarını](deploy-prerequisite.md) gözden geçirin (genişletilmiş destek).
- Cloud Services için [sık sorulan soruları](faq.md) gözden geçirin (genişletilmiş destek).
- [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), [şablon](deploy-template.md) veya [Visual Studio](deploy-visual-studio.md)kullanarak bir bulut hizmeti (genişletilmiş destek) dağıtın.
