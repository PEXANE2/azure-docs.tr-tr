---
title: 'Azure ExpressRoute: bir devreyi değiştirme: PowerShell: klasik'
description: Bu makalede, ExpressRoute klasik dağıtım modeli devrenizin durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma adımlarında size kılavuzluk eder.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 11/05/2019
ms.author: cherylmc
ms.openlocfilehash: 2f2aac6dd7e073634d4114d33fae6bebd51fe176
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84736331"
---
# <a name="modify-an-expressroute-circuit-using-powershell-classic"></a>PowerShell kullanarak bir ExpressRoute devresini değiştirme (klasik)

> [!div class="op_single_selector"]
> * [Azure portalındaki](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede, ExpressRoute klasik dağıtım modeli devrenizin durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma adımlarında size kılavuzluk eder. Bu makale klasik dağıtım modeli için geçerlidir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

## <a name="get-the-status-of-a-circuit"></a>Devrenin durumunu al

Bu bilgileri, cmdlet 'ini kullanarak istediğiniz zaman alabilirsiniz `Get-AzureCircuit` . Hiçbir parametre olmadan çağrının yapılması tüm devreleri listeler.

```powershell
Get-AzureDedicatedCircuit

Bandwidth                        : 200
CircuitName                      : MyTestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled

Bandwidth                        : 1000
CircuitName                      : MyAsiaCircuit
Location                         : Singapore
ServiceKey                       : #################################
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Hizmet anahtarını çağrıya parametre olarak geçirerek belirli bir ExpressRoute devresi hakkında bilgi alabilirsiniz.

```powershell
Get-AzureDedicatedCircuit -ServiceKey "*********************************"

Bandwidth                        : 200
CircuitName                      : MyTestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Aşağıdaki örneği çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
get-help get-azurededicatedcircuit -detailed
```

## <a name="modify-a-circuit"></a>Devreyi değiştirme

Bağlantıyı etkilemeden bir ExpressRoute devresine ait belirli özellikleri değiştirebilirsiniz.

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* ExpressRoute devreniz için bir ExpressRoute Premium eklentisini etkinleştirin veya devre dışı bırakın.
* Bağlantı noktasında kullanılabilir kapasite bulunduğundan ExpressRoute devreniz bant genişliğini artırın. Bağlantı hattının devre dışı olması desteklenmiyor.
* Ölçüm planını ölçülen verilerden sınırsız veriye değiştirin. Ölçüm planının sınırsız verilerden ölçülen verilerle değiştirilmesi desteklenmez.
* *Klasik Işlemlere Izin ver*' i etkinleştirebilir ve devre dışı bırakabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md) .

### <a name="enable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini etkinleştirin

Aşağıdaki PowerShell cmdlet 'ini kullanarak mevcut bağlantı hattınızı ExpressRoute Premium eklentisi ' ni etkinleştirebilirsiniz:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Premium
Status                           : Enabled
```

Devreniz artık ExpressRoute Premium eklenti özellikleri etkin olur. Komut başarılı bir şekilde çalıştırıldığında, Premium eklenti özelliği için faturalandırmaya başlanır.

### <a name="disable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini devre dışı bırakma

> [!IMPORTANT]
> Standart devre için izin verilen miktardan daha büyük kaynaklar kullanıyorsanız bu işlem başarısız olabilir.
>
>

#### <a name="considerations"></a>Önemli noktalar

* Premium 'dan standart sürümüne düşürme yapmadan önce, bağlantı hattına bağlı sanal ağların sayısının 10 ' dan küçük olduğundan emin olun. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve Premium ücretler üzerinden faturalandırılırsınız.
* Diğer geopolitik bölgelerdeki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve Premium ücretler üzerinden faturalandırılırsınız.
* Yol tablonuz, özel eşleme için 4.000 rotadan az olmalıdır. Yol tablonuzun boyutu 4.000 rotadan büyükse BGP oturumu, tanıtılan ön eklerin sayısı 4.000 altına çıkana kadar yeniden etkinleştirilmez.

#### <a name="to-disable-the-premium-add-on"></a>Premium eklentiyi devre dışı bırakmak için

Aşağıdaki PowerShell cmdlet 'ini kullanarak mevcut bağlantı hattınızı ExpressRoute Premium eklentisini devre dışı bırakabilirsiniz:

```powershell

Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

### <a name="update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirme

Sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS bölümüne](expressroute-faqs.md) bakın. Mevcut devrenin boyutundan daha büyük bir boyut seçebilirsiniz ve bu fiziksel bağlantı noktası (bağlantı hattının oluşturulduğu) izin verir.

> [!IMPORTANT]
> Mevcut bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. Bu konumda kullanılabilir ek kapasite yoksa, devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamaz. Bant genişliğini düşürme işlemi, ExpressRoute bağlantı hattının sağlanması ve ardından yeni bir ExpressRoute devresine yeniden sağlanması gerektirir.
>
>

#### <a name="resize-a-circuit"></a>Devre yeniden boyutlandırma

Hangi boyuta ihtiyacınız olduğuna karar verdikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

Bandwidth                        : 1000
CircuitName                      : TestCircuit
Location                         : Silicon Valley
ServiceKey                       : *********************************
ServiceProviderName              : equinix
ServiceProviderProvisioningState : Provisioned
Sku                              : Standard
Status                           : Enabled
```

Devreniz Microsoft tarafında boyutlandırıldıktan sonra, bu değişikliği eşleştirmek için kendi taraflarındaki yapılandırmaların güncelleştirilmesini sağlamak üzere bağlantı sağlayıcınızla iletişim kurmanız gerekir. Faturalandırma, bu noktadan itibaren güncelleştirilmiş bant genişliği seçeneği için başlar.

Devre bant genişliğini artırdığınızda aşağıdaki hatayı görürseniz, bu, mevcut devrenin oluşturulduğu fiziksel bağlantı noktasında yeterli bant genişliği olmadığı anlamına gelir. Bu devreyi silmeniz ve ihtiyacınız olan boyutta yeni bir bağlantı oluşturmanız gerekir.

```powershell
Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
update operation
At line:1 char:1
+ Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
  + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
```

## <a name="deprovision-and-delete-a-circuit"></a>Devre sağlamasını kaldırma ve silme

### <a name="considerations"></a>Önemli noktalar

* Bu işlemin başarılı olması için tüm sanal ağların ExpressRoute bağlantı hattından bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, bağlantı hattına bağlı sanal ağlarınızın olup olmadığını denetleyin.
* ExpressRoute bağlantı hattı hizmeti sağlayıcısı sağlama **durumu sağlandıysa veya** sağlanmışsa **Provisioned** , devre dışı bırakmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcısı, devre sağlamasını kaldırmayı ve bize haber verene kadar, kaynakları ayırmaya ve sizi faturalandırmaya devam ediyoruz.
* Hizmet sağlayıcı devre sağlamasını kaldırmışsa (hizmet sağlayıcı sağlama durumu **sağlanmadı**olarak ayarlandıysa), ardından devreyi silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

#### <a name="delete-a-circuit"></a>Devre silme

Aşağıdaki komutu çalıştırarak ExpressRoute devrenizi silebilirsiniz:

```powershell
Remove-AzureDedicatedCircuit -ServiceKey "*********************************"
```
