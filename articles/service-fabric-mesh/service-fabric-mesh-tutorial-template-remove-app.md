---
title: Öğretici-Azure Service Fabric ağı 'nda çalışan bir uygulamayı kaldırma
description: Bu öğreticide, Service Fabric Mesh’te çalışan bir uygulamayı kaldırma ve kaynakları silme hakkında bilgi edineceksiniz.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: d449343fd00ff958470a71ecb3a37d585d7ff8ed
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99626619"
---
# <a name="tutorial-remove-an-application-and-resources"></a>Öğretici: Bir uygulamayı ve kaynakları kaldırma

> [!IMPORTANT]
> Azure Service Fabric ağı önizlemesi devre dışı bırakıldı. Yeni dağıtımlar Service Fabric kafes API 'SI aracılığıyla artık izin verilmeyecektir. Mevcut dağıtımlar için destek 28 Nisan 2021 ' den devam edecektir.
> 
> Ayrıntılar için bkz. [Azure Service Fabric kafes önizleme kullanımdan](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/)kaldırma.

Bu öğretici, bir serinin dördüncü bölümüdür. [Daha önce Service Fabric Mesh’e dağıtılmış](service-fabric-mesh-tutorial-template-deploy-app.md) bir çalışan uygulamayı kaldırmayı öğreneceksiniz. 

Serinin dördüncü kısmında öğrenecekleriniz:

> [!div class="checklist"]
> * Service Fabric Mesh’te çalışan bir uygulamayı silme
> * Uygulama kaynaklarını silme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Şablon kullanarak Service Fabric Mesh’e uygulama dağıtma](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Service Fabric Mesh’te çalışan bir uygulamayı ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Service Fabric Mesh’te çalışan bir uygulamayı yükseltme](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * Uygulamayı kaldırma

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

* [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md)’i açın veya [Azure CLI ve Service Fabric Mesh CLI’sini yerel olarak yükleyin](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="delete-the-resource-group-and-all-the-resources"></a>Kaynak grubunu ve tüm kaynakları silme

Artık gerekli değilse oluşturduğunuz tüm kaynakları silebilirsiniz. Daha önce, Azure Container Registry örneğini ve Service Fabric Mesh uygulama kaynaklarını barındıracak [yeni bir kaynak grubu oluşturdunuz](service-fabric-mesh-tutorial-template-deploy-app.md#create-a-container-registry).  Bu kaynak grubunu ve sonuç olarak tüm ilişkili kaynakları silebilirsiniz.

```azurecli
az group delete --resource-group myResourceGroup
```

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="individually-delete-the-resources"></a>Kaynakları tek tek silme
ACR örneğini, Service Fabric Mesh uygulamasını ve ağ kaynaklarını tek tek de silebilirsiniz.

ACR örneğini silmek için:

```azurecli
az acr delete --resource-group myResourceGroup --name myContainerRegistry
```

Service Fabric Mesh uygulamasını silmek için:

```azurecli
az mesh app delete --resource-group myResourceGroup --name todolistapp
```

Ağı silmek için:
```azurecli
az mesh network delete --resource-group myResourceGroup --name todolistappNetwork
```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Service Fabric Mesh’te çalışan bir uygulamayı silme
> * Uygulama kaynaklarını silme
