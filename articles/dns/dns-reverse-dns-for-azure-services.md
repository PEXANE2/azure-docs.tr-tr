---
title: Azure hizmetleri için Ters DNS - Azure DNS
description: Bu öğrenme yolu ile Azure'da barındırılan hizmetler için ters DNS aramalarını yapılandırmaya başlayın.
services: dns
documentationcenter: na
author: rohinkoul
manager: KumudD
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: rohink
ms.openlocfilehash: 073e84ece11f6817bfe2c5a94735ec6e16dac4fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76932365"
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Azure'da barındırılan hizmetler için ters DNS yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede, Azure'da barındırılan hizmetler için ters DNS aramalarının nasıl yapılandırılabildiğini açıklanmaktadır.

Azure'daki hizmetler, Azure tarafından atanan ve Microsoft'a ait OLAN IP adreslerini kullanır. Bu ters DNS kayıtları (PTR kayıtları) ilgili Microsoft'a ait ters DNS arama bölgelerinde oluşturulmalıdır. Bu makalede, bunun nasıl yapılacağını açıklar.

Bu senaryo, [Azure DNS'de atanan IP aralıklarını zedilen ters DNS arama bölgelerini barındırma](dns-reverse-dns-hosting.md)olanağıyla karıştırılmamalıdır. Bu durumda, ters arama bölgesi tarafından temsil edilen IP aralıkları genellikle ISS'niz tarafından kuruluşunuza atanmalıdır.

Bu makaleyi okumadan önce, [Azure'daki ters DNS ve desteğe genel bakışa](dns-reverse-dns-overview.md)aşina olmalısınız.

Azure DNS'de, bilgi işlem kaynakları (sanal makineler, sanal makine ölçek kümeleri veya Service Fabric kümeleri gibi) PublicIpAddress kaynağı aracılığıyla açığa alınır. Ters DNS aramaları PublicIpAddress'in 'ReverseFqdn' özelliği kullanılarak yapılandırılır.


Ters DNS şu anda Azure Uygulama Hizmeti için desteklenmez.

## <a name="validation-of-reverse-dns-records"></a>Ters DNS kayıtlarının doğrulanması

Üçüncü bir taraf, Azure hizmet eşlemeleri için DNS etki alanlarınızla ters DNS kayıtları oluşturamamalıdır. Bunu önlemek için Azure, yalnızca ters DNS kaydında belirtilen etki alanı adının aynı Azure aboneliğinde bir PublicIpAddress veya Cloud Service'in DNS adı veya IP adresiyle aynı olduğu veya bu şekilde çözüldüğü ters bir DNS kaydı oluşturulmasına izin verir.

Bu doğrulama yalnızca ters DNS kaydı ayarlandığında veya değiştirildiğinde gerçekleştirilir. Periyodik yeniden doğrulama yapılmaz.

Örneğin: PublicIpAddress kaynağının DNS adı contosoapp1.northus.cloudapp.azure.com ve IP adresi 23.96.52.53 olduğunu varsayalım. PublicIpAddress için ReverseFqdn olarak belirtilebilir:
* PublicIpAddress için DNS adı, contosoapp1.northus.cloudapp.azure.com
* Aynı abonelikteki farklı bir PublicIpAddress'in DNS adı contosoapp2.westus.cloudapp.azure.com
* Bu ad ilk olarak contosoapp1.northus.cloudapp.azure.com cname olarak *veya* aynı abonelikteki farklı bir PublicIpAddress için yapılandırıldığı sürece, app1.contoso.com gibi bir vanity DNS adı.
* Bu ad *ilk* olarak 23.96.52.53 IP adresine veya aynı abonelikteki farklı bir PublicIpAddress'in IP adresine A kaydı olarak yapılandırıldıkça, app1.contoso.com gibi bir vanity DNS adı.

Bulut Hizmetleri için DNS'yi tersine çevirmek için de aynı kısıtlamalar uygulanır.


## <a name="reverse-dns-for-publicipaddress-resources"></a>PublicIpAddress kaynakları için Ters DNS

Bu bölümde, Azure PowerShell, Azure klasik CLI veya Azure CLI kullanarak Kaynak Yöneticisi dağıtım modelinde PublicIpAddress kaynakları için ters DNS'nin nasıl yapılandırılabildiğini anlatan ayrıntılı yönergeler sağlanmaktadır. PublicIpAddress kaynakları için ters DNS yapılandırması şu anda Azure portalı üzerinden desteklenmez.

Azure şu anda yalnızca IPv4 PublicIpAddress kaynakları için ters DNS'yi destekler. IPv6 için desteklenmez.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Varolan PublicIpAddresses'e ters DNS ekleme

#### <a name="powershell"></a>PowerShell

Ters DNS'yi varolan bir PublicIpAddress'e güncelleştirmek için:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

Zaten DNS adı olmayan varolan bir PublicIpAddress'e ters DNS eklemek için bir DNS adı da belirtmeniz gerekir:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

Varolan bir PublicIpAddress'e ters DNS eklemek için:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Zaten DNS adı olmayan varolan bir PublicIpAddress'e ters DNS eklemek için bir DNS adı da belirtmeniz gerekir:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI

Varolan bir PublicIpAddress'e ters DNS eklemek için:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Zaten DNS adı olmayan varolan bir PublicIpAddress'e ters DNS eklemek için bir DNS adı da belirtmeniz gerekir:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Ters DNS ile Ortak IP Adresi Oluşturma

Önceden belirtilen ters DNS özelliğine sahip yeni bir PublicIpAddress oluşturmak için:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Varolan bir PublicIpAddress için ters DNS'yi görüntüleme

Varolan bir PublicIpAddress için yapılandırılan değeri görüntülemek için:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Varolan Genel IP Adreslerinden ters DNS'yi kaldırma

Varolan bir PublicIpAddress'ten ters Bir DNS özelliğini kaldırmak için:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Bulut Hizmetleri için ters DNS'yi yapılandırma

Bu bölümde, Azure PowerShell kullanarak Klasik dağıtım modelinde Bulut Hizmetleri için ters DNS'nin nasıl yapılandırılabildiğini anlatan ayrıntılı yönergeler sağlanmaktadır. Bulut Hizmetleri için ters DNS yapılandırması Azure portalı, Azure klasik CLI veya Azure CLI üzerinden desteklenmez.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Varolan Bulut Hizmetlerine ters DNS ekleme

Varolan bir Bulut Hizmetine ters DNS kaydı eklemek için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Ters DNS ile Bulut Hizmeti Oluşturma

Önceden belirtilen ters DNS özelliğine sahip yeni bir Bulut Hizmeti oluşturmak için:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Varolan Bulut Hizmetleri için ters DNS'yi görüntüleme

Varolan bir Bulut Hizmeti için ters DNS özelliğini görüntülemek için:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Varolan Bulut Hizmetlerinden ters DNS'yi kaldırma

Varolan bir Bulut Hizmetinden ters Bir DNS özelliğini kaldırmak için:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>SSS

### <a name="how-much-do-reverse-dns-records-cost"></a>Ters DNS kayıtlarının maliyeti ne kadardır?

Onlar özgür!  Ters DNS kayıtları veya sorguları için ek bir maliyet yoktur.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Ters DNS kayıtlarım internetten çözülür mü?

Evet. Azure hizmetiniz için ters DNS özelliğini ayarladıktan sonra, Azure, ters DNS kaydının tüm Internet kullanıcıları için çözülmesini sağlamak için gereken tüm DNS delegasyonlarını ve DNS bölgelerini yönetir.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Azure hizmetlerim için varsayılan ters DNS kayıtları mı oluşturuldu?

Hayır. Ters DNS bir opt-in özelliğidir. Yapılandırmamayı seçerseniz varsayılan ters DNS kayıtları oluşturulmaz.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Tam nitelikli alan adı (FQDN) için biçim nedir?

FQDN'ler ileri sırada belirtilir ve bir nokta ile sonlandırılmalıdır (örneğin, "app1.contoso.com.").

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Belirttiğim ters DNS için doğrulama denetimi başarısız olursa ne olur?

Ters DNS doğrulama denetimi başarısız olduğunda, ters DNS kaydıyapılandırma işlemi başarısız olur. Ters DNS değerini gerektiği gibi düzeltin ve yeniden deneyin.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Azure Uygulama Hizmeti için ters DNS yapılandırabilir miyim?

Hayır. Ters DNS, Azure Uygulama Hizmeti için desteklenmez.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Azure hizmetim için birden çok ters DNS kaydı yapılandırabilir miyim?

Hayır. Azure, her Azure Bulut Hizmeti veya PublicIpAddress için tek bir ters DNS kaydını destekler.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>IPv6 PublicIpAddress kaynakları için ters DNS yapılandırabilir miyim?

Hayır. Azure şu anda yalnızca IPv4 PublicIpAddress kaynakları ve Bulut Hizmetleri için ters DNS'yi destekler.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Azure İşlem hizmetlerimden harici etki alanlarına e-posta gönderebilir miyim?

Doğrudan bir Azure dağıtımından e-posta gönderme teknik yeteneği abonelik türüne bağlıdır. Abonelik türünden bağımsız olarak, Microsoft giden posta göndermek için güvenilir posta geçiş hizmetlerini kullanmanızı önerir. Daha fazla bilgi [için, E-posta göndermek için Gelişmiş Azure Güvenliği 'ne bakın – Kasım 2017 Güncellemesi](https://blogs.msdn.microsoft.com/mast/2017/11/15/enhanced-azure-security-for-sending-emails-november-2017-update/).

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi [için, Vikipedi'de ters DNS arama](https://en.wikipedia.org/wiki/Reverse_DNS_lookup)bakın.
<br>
[Azure DNS'de ISS'ye atanmış IP aralığınızın ters arama bölgesini nasıl barındırışla düzenleyeceklerinizi](dns-reverse-dns-for-azure-services.md)öğrenin.

