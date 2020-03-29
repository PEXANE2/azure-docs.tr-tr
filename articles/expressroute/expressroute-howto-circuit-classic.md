---
title: 'Azure ExpressRoute: Bir devreyi değiştirin: PowerShell:klasik'
description: Bu makale, ExpressRoute klasik dağıtım modeli devrenizi durumunuzu denetlemek, güncelleştirmek veya silmek ve hükmünü yok etmek için size bu adımları yönlendirir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: cherylmc
ms.openlocfilehash: e421a534b04f74d2a2eb0bc06aeffa52684ae17a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931971"
---
# <a name="modify-an-expressroute-circuit-using-powershell-classic"></a>PowerShell kullanarak ExpressRoute devresini değiştirme (klasik)

> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-circuit-portal-resource-manager.md)
> * [Powershell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makale, ExpressRoute klasik dağıtım modeli devrenizi durumunuzu denetlemek, güncelleştirmek veya silmek ve hükmünü yok etmek için size bu adımları yönlendirir. Bu makale klasik dağıtım modeli için geçerlidir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

## <a name="get-the-status-of-a-circuit"></a>Bir devrenin durumunu alın

Bu bilgileri `Get-AzureCircuit` istediğiniz zaman cmdlet kullanarak alabilirsiniz. Herhangi bir parametre olmadan arama yapma tüm devreleri listeler.

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

Belirli bir ExpressRoute devresi hakkında, servis anahtarını çağrıya parametre olarak geçirerek bilgi alabilirsiniz.

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

Bir ExpressRoute devresinin belirli özelliklerini bağlantı etkilemeden değiştirebilirsiniz.

Aşağıdaki görevleri kapalı kalma süresi olmadan yapabilirsiniz:

* ExpressRoute devreniz için ExpressRoute premium eklentisini etkinleştirin veya devre dışı edin.
* Bağlantı noktasında kapasite olması koşuluyla ExpressRoute devrenizin bant genişliğini artırın. Bir devrenin bant genişliğini düşürme desteklenmez.
* Ölçüm planını Tarifeli Verilerden Sınırsız Veri'ye değiştirin. Ölçüm planının Sınırsız Verilerden Ölçülü Verilere değiştirilmesi desteklenmez.
* *Klasik İşlemler'e İzin Ver'i*etkinleştirebilir ve devre dışı kullanabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için [ExpressRoute SSS](expressroute-faqs.md) bölümüne bakın.

### <a name="enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini etkinleştirme

Aşağıdaki PowerShell cmdlet'i kullanarak mevcut devreniz için ExpressRoute premium eklentisini etkinleştirebilirsiniz:

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

Devreniz artık ExpressRoute premium eklenti özelliklerine sahip olacak. Komut başarıyla çalıştırıldığı anda, premium eklenti özelliği için faturalandırma başlar.

### <a name="disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini devre dışı

> [!IMPORTANT]
> Standart devre için izin verilenden daha büyük kaynaklar kullanıyorsanız, bu işlem başarısız olabilir.
>
>

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Devreyle bağlantılı sanal ağ sayısının, premium'dan standarda düşürülmeden önce 10'dan az olduğundan emin olun. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve prim oranları size faturalandırılır.
* Diğer jeopolitik bölgelerdeki tüm sanal ağların bağlantısını kalmalıdır. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve prim oranları size faturalandırılır.
* Rota tablonuz özel bakış için 4.000'den az rota olmalıdır. Rota tablo boyutunuz 4.000 rotadan büyükse, BGP oturumu düşer ve reklamı yapılan önek sayısı 4.000'in altına inene kadar yeniden etkinleştirilmez.

#### <a name="to-disable-the-premium-add-on"></a>Premium eklentiyi devre dışı atmak için

Aşağıdaki PowerShell cmdlet'i kullanarak mevcut devreniz için ExpressRoute premium eklentisini devre dışı bırakabilirsiniz:

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

### <a name="update-the-expressroute-circuit-bandwidth"></a>ExpressRoute devre bant genişliğini güncelleştirme

Sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS'yi](expressroute-faqs.md) kontrol edin. Fiziksel bağlantı noktası (devrenizin oluşturulduğu) izin verdiği sürece, varolan devrenizin boyutundan daha büyük herhangi bir boyutu seçebilirsiniz.

> [!IMPORTANT]
> Varolan bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. O konumda ek kapasite yoksa devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamazsınız. Bant genişliğini düşürmek, ExpressRoute devresini kaldırmanızı ve ardından yeni bir ExpressRoute devresini yeniden sağlamanızı gerektirir.
>
>

#### <a name="resize-a-circuit"></a>Bir devreyi yeniden boyutlandırma

Hangi boyuta ihtiyacınız olduğuna karar verildikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanabilirsiniz:

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

Devreniz Microsoft tarafında boyutlandıktan sonra, bu değişikliği eşleştirmek için kendi tarafındaki yapılandırmaları güncelleştirmek için bağlantı sağlayıcınıza başvurmanız gerekir. Bu noktadan itibaren güncelleştirilmiş bant genişliği seçeneği için faturalandırma başlar.

Devre bant genişliğini artırırken aşağıdaki hatayı görürseniz, varolan devrenizin oluşturulduğu fiziksel bağlantı noktasında yeterli bant genişliği kalmadığını görürsünüz. Bu devreyi silmeli ve ihtiyacınız olan boyutta yeni bir devre oluşturmalısınız.

```powershell
Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
update operation
At line:1 char:1
+ Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
  + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
```

## <a name="deprovision-and-delete-a-circuit"></a>Bir devreyi deprovision ve silme

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Bu işlemin başarılı olması için ExpressRoute devresindeki tüm sanal ağların bağlantısını çıkarmalısınız. Bu işlem başarısız olursa devrebağlı herhangi bir sanal ağları olup olmadığını kontrol edin.
* ExpressRoute devre servis sağlayıcısı sağlama durumu **Hükmünde** veya **Provisioned** ise, devreyi kendi taraflarında devreyi devreyi devreyi devreden çıkarmak için servis sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcı devreyi devreyi devreyi tamamlayıp bizi bize iletene kadar kaynak ayırmaya ve size fatura landırmaya devam ediyoruz.
* Hizmet sağlayıcısı devreyi devreyi devreyi devreyi iptal ettiyse (servis sağlayıcı sağlama durumu **değil**olarak ayarlanır), daha sonra devreyi silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

#### <a name="delete-a-circuit"></a>Bir devreyi silme

ExpressRoute devrenizi aşağıdaki komutu çalıştırarak silebilirsiniz:

```powershell
Remove-AzureDedicatedCircuit -ServiceKey "*********************************"
```
