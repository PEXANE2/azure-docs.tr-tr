---
title: Quickstart - Docker konteynerini konteyner örneğine dağıtın - PowerShell
description: Bu hızlı başlatmada, yalıtılmış bir Azure kapsayıcı örneğinde çalışan kapsayıcı web uygulamasını hızla dağıtmak için Azure PowerShell'i kullanırsınız
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: 9901b3f18973365dc9ceb8c85ff8587b6c2ea894
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74533619"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak Azure'da bir kapsayıcı örneğini dağıtma

Azure'da sunucusuz Docker kapsayıcılarını basitlik ve hızla çalıştırmak için Azure Kapsayıcı Örnekleri'ni kullanın. Azure Kubernetes Hizmeti gibi tam bir kapsayıcı düzenleme platformuna ihtiyacınız olmadığında uygulamayı isteğe bağlı bir kapsayıcı örneğine dağıtın.

Bu hızlı başlatmada, yalıtılmış bir Windows kapsayıcısı dağıtmak ve uygulamasını tam nitelikli bir etki alanı adı (FQDN) ile kullanılabilir hale getirmek için Azure PowerShell'i kullanırsınız. Tek bir dağıtım komutunu çalıştırdıktan birkaç saniye sonra, kapsayıcıda çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][qs-powershell-01]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici azure PowerShell modüllerini gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

İlk olarak, aşağıdaki [Yeni-AzResourceGroup][New-AzResourceGroup] komutuyla *eastus* konumunda *myResourceGroup* adında bir kaynak grubu oluşturun:

 ```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Artık bir kaynak grubuna sahip olduğunuza göre Azure'da kapsayıcı çalıştırabilirsiniz. Azure PowerShell ile bir kapsayıcı örneği oluşturmak için, [Yeni-AzContainerGroup][New-AzContainerGroup] cmdlet'ine bir kaynak grubu adı, kapsayıcı örnek adı ve Docker kapsayıcı görüntüsü sağlayın. Bu hızlı başlangıçta, genel `mcr.microsoft.com/windows/servercore/iis:nanoserver` görüntüyü kullanırsınız. Bu resim, Microsoft Internet Information Services 'ı (IIS) Nano Server'da çalıştırmak üzere paketler.

Açılacak bir veya daha fazla bağlantı noktası, DNS ad etiketi ya da ikisini birden belirterek kapsayıcılarınızı internete açabilirsiniz. Bu hızlı başlangıçta, IIS'ye genel olarak erişilebilen bir DNS ad etiketiiçeren bir kapsayıcı dağıtırsınız.

Kapsayıcı örneğini başlatmak için aşağıdakine benzer bir komut uygulayın. Örneği `-DnsNameLabel` oluşturduğunuz Azure bölgesinde benzersiz bir değer ayarlayın. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

 ```azurepowershell-interactive
New-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image mcr.microsoft.com/windows/servercore/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Birkaç saniye içinde Azure’dan bir yanıt almanız gerekir. Kapsayıcının `ProvisioningState` değeri başlangıçta **Oluşturuluyor** şeklindedir ancak birkaç dakika içinde **Başarılı** olarak değişmesi gerekir. [Get-AzContainerGroup][Get-AzContainerGroup] cmdlet ile dağıtım durumunu kontrol edin:

 ```azurepowershell-interactive
Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Kapsayıcının sağlama durumu, tam etki alanı adı (FQDN) ve IP adresi, cmdlet çıkışında görüntülenir:

```console
PS Azure:\> Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Kapsayıcının `ProvisioningState` bilgisi **Başarılı** olduğunda tarayıcınızdan `Fqdn` adresine gidin. Aşağıdakine benzer bir web sayfası görüyorsanız kendinizi tebrik edebilirsiniz! Docker kapsayıcısında çalışan bir uygulamayı başarıyla Azure'a dağıttınız.

![Azure Container Instances kullanılarak dağıtılmış IIS tarayıcıda görüntüleniyor][qs-powershell-01]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıile bitirdiğinizde, [Kaldır-AzContainerGroup][Remove-AzContainerGroup] cmdlet ile çıkarın:

 ```azurepowershell-interactive
Remove-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub deposundaki bir görüntüden bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzResourceGroup]: /powershell/module/az.resources/new-Azresourcegroup
[New-AzContainerGroup]: /powershell/module/az.containerinstance/new-Azcontainergroup
[Get-AzContainerGroup]: /powershell/module/az.containerinstance/get-Azcontainergroup
[Remove-AzContainerGroup]: /powershell/module/az.containerinstance/remove-Azcontainergroup
