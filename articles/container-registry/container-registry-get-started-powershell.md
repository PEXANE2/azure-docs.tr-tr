---
title: Quickstart - Kayıt defteri oluşturma - Powershell
description: PowerShell ile Azure Konteyner Kayıt Defteri'nde özel bir Docker kayıt defteri oluşturmayı hızla öğrenin
ms.topic: quickstart
ms.date: 01/22/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: 872b2a29444e5278db34ce44741e2ca90d885702
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74456380"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak özel bir kapsayıcı kayıt defteri oluşturma

Azure Container Registry, Docker kapsayıcı görüntülerini oluşturmak, depolamak ve sunmak için kullanılan özel bir yönetilen Docker kapsayıcı kayıt defteridir. Bu hızlı başlangıçta PowerShell kullanarak Azure Container Registry oluşturmayı öğreneceksiniz. Ardından, bir kapsayıcı görüntüsünü kayıt defterine itmek için Docker komutlarını kullanın ve son olarak resmi kayıt defterinizden çekip çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlatma, Azure PowerShell modülü gerektirir. Yüklü sürümünüzü belirlemek için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker, [macOS][docker-mac], [Windows][docker-windows] ve [Linux][docker-linux] sistemleri için paketler sağlar.

Azure Cloud Shell gerekli tüm Docker bileşenlerini (`dockerd` daemon) içermediğinden, bu hızlı başlangıçta Cloud Shell’i kullanamazsınız.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount][Connect-AzAccount] komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Azure ile kimlik doğrulaması alındıktan [sonra, New-AzResourceGroup][New-AzResourceGroup]ile bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarınızın dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Ardından, [Yeni-AzContainerRegistry][New-AzContainerRegistry] komutu yla yeni kaynak grubunuzda bir kapsayıcı kayıt defteri oluşturun.

Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnek, "myContainerRegistry007" adlı bir kayıt defteri oluşturur. Aşağıdaki komutta *myContainerRegistry007* değerini değiştirin ve kayıt defterini oluşturmak için komutu çalıştırın:

```powershell
$registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

Bu hızlı başlangıçta, Azure Konteyner Kayıt Defteri'ni öğrenen geliştiriciler için maliyet için en uygun seçenek olan *bir Temel* kayıt defteri oluşturursunuz. Kullanılabilir hizmet katmanları hakkında ayrıntılı bilgi için [Konteyner kayıt defteri SUS'larına][container-registry-skus]bakın.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce kayıt defterinizde oturum açmalısınız. Üretim senaryolarında konteyner kayıt defteri erişimi için tek bir kimlik veya hizmet ilkesi kullanmanız gerekir, ancak bu hızlı başlangıç kısa tutmak için, [Get-AzContainerRegistryCredential][Get-AzContainerRegistryCredential] komutu ile kayıt defterinizde yönetici kullanıcı etkinleştirin:

```powershell
$creds = Get-AzContainerRegistryCredential -Registry $registry
```

Ardından, oturum açmak için [docker login][docker-login] komutunu çalıştırın:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Bu komut tamamlandığında `Login Succeeded` döndürülür.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlatmada oluşturduğunuz kaynaklarla çalışmayı bitirdikten sonra, kaynak grubunu, kapsayıcı kayıt defterini ve depolanan kapsayıcı görüntülerini kaldırmak için [Kaldır-AzResourceGroup][Remove-AzResourceGroup] komutunu kullanın:

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure PowerShell içeren bir Azure Kapsayıcı Kayıt Defteri oluşturdunuz, bir kapsayıcı görüntüsünü ittiniz ve resmi kayıt defterinden çekip çalıştırdın. ACR'ye daha derinlemesine bakmak için Azure Konteyner Kayıt Defteri öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Azure Konteyner Kayıt Defteri öğreticileri][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Get-AzContainerRegistryCredential]: /powershell/module/az.containerregistry/get-azcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzContainerRegistry]: /powershell/module/az.containerregistry/New-AzContainerRegistry
[New-AzResourceGroup]: /powershell/module/az.resources/new-azresourcegroup
[Remove-AzResourceGroup]: /powershell/module/az.resources/remove-azresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
