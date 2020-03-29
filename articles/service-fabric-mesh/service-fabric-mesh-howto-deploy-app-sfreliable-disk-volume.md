---
title: Servis Kumaş Örgü ile Servis Kumaş Güvenilir Disk Hacmi
description: Azure CLI'yi kullanarak servis kumaşı güvenilir disk tabanlı birimi konteynerin içine monte ederek bir Azure Hizmet Kumaş Kafes uygulamasında durumu nasıl depolayın öğrenin.
author: ashishnegi
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: asnegi
ms.custom: mvc, devcenter
ms.openlocfilehash: f26fe70afe7d9e2872f06ac6da7143556278b1b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75497959"
---
# <a name="mount-highly-available-service-fabric-reliable-disk-based-volume-in-a-service-fabric-mesh-application"></a>Bir Servis Kumaş Örgü uygulamasında son derece kullanılabilir Hizmet Kumaş Güvenilir Disk tabanlı hacim montaj 
Kapsayıcı uygulamalarda durum kalıcılığı yaygın yöntem, Azure Dosya Depolama veya Azure Cosmos DB gibi veritabanı gibi uzak depolama kullanmaktır. Bu, uzak mağazaya önemli okuma ve yazma ağı gecikmesine neden olabilir.

Bu makalede, bir Service Fabric Mesh uygulamasının konteyner içine bir hacim monte ederek son derece kullanılabilir Hizmet Kumaş Güvenilir Disk durum depolamak için nasıl gösterir.
Service Fabric Reliable Disk, yüksek kullanılabilirlik için Service Fabric Cluster içinde çoğaltılan yazılarla yerel okumalar için hacimsağlar. Bu, okuma lar için ağ çağrılarını kaldırır ve yazmaların ağ gecikmesüresini azaltır. Kapsayıcı yeniden başlatılırsa veya başka bir düğüme geçerse, yeni kapsayıcı örneği eskiyle aynı birimde görünür. Böylece hem verimli hem de yüksek kullanılabilir.

Bu örnekte, Sayaç uygulaması, tarayıcıda sayaç değerini gösteren bir web sayfası içeren bir ASP.NET Core hizmetine sahiptir.

Periyodik `counterService` olarak bir dosyadan sayaç değeri okur, onu aşamalı olarak okur ve dosyaya geri yazar. Dosya, Service Fabric Reliable Disk tarafından desteklenen ses birimine monte edilmiş bir klasörde depolanır.

## <a name="prerequisites"></a>Ön koşullar

Bu görevi tamamlamak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Bu makaleyle Azure CLI'yi `az --version` kullanmak için `azure-cli (2.0.43)`en az .  Azure Hizmet Kumaş ıstıraki CLI uzantı [modüllerini](service-fabric-mesh-howto-setup-cli.md)bu yönergeleri izleyerek yükleyin (veya güncelleyin).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulamanın dağıtılacağı kaynak grubunu oluşturun. Aşağıdaki komut, doğu AMERIKA `myResourceGroup` Birleşik Devletleri'nde bir konumda adlı bir kaynak grubu oluşturur. Kaynak grubu adını aşağıdaki komutta değiştirirseniz, izleyen tüm komutlarda değiştirmeyi unutmayın.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki komut [counter.sfreliablevolume.linux.json şablonunu](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.linux.json)kullanarak bir Linux uygulaması dağıtır. Bir Windows uygulamasını dağıtmak için [counter.sfreliablevolume.windows.json şablonunu](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.windows.json)kullanın. Daha büyük kapsayıcı görüntülerinin dağıtılmasının daha uzun sürebileceğini unutmayın.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/counter/counter.sfreliablevolume.linux.json
```

Komutla dağıtım Durumunu da görebilirsiniz

```azurecli-interactive
az group deployment show --name counter.sfreliablevolume.linux --resource-group myResourceGroup
```

Kaynak türü ne kadar `Microsoft.ServiceFabricMesh/gateways`dır. Bu, uygulamanın genel IP adresini almak için kullanılacaktır.

## <a name="open-the-application"></a>Uygulamayı açma

Uygulama başarıyla dağıtıldıktan sonra, uygulama için ağ geçidi kaynağının ipAddress'ini alın. Yukarıdaki bölümde fark ettiğiniz ağ geçidi adını kullanın.
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name counterGateway
```

Çıktı, hizmet bitiş `ipAddress` noktasıiçin genel IP adresi olan bir özelliğe sahip olmalıdır. Tarayıcıdan açın. Her saniye güncellenen sayaç değeri ne redeleyecek bir web sayfası görüntüler.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Uygulamanın birimi kullanabildiğini doğrulayın

Uygulama, klasörün içindeki `counter.txt` `counter/counterService` birimde adı geçen bir dosya oluşturur. Bu dosyanın içeriği, web sayfasında görüntülenen sayaç değeridir.

## <a name="delete-the-resources"></a>Kaynakları silme

Azure'da artık kullanmadığınız kaynakları sık sık silin. Bu örnekle ilgili kaynakları silmek için, dağıtıldıkları kaynak grubunu (kaynak grubuyla ilişkili her şeyi silen) aşağıdaki komutla silin:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [GitHub'da](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter)Servis Kumaşı Güvenilir Hacim Diski örnek uygulamasını görüntüleyin.
- Service Fabric Kaynak Modeli hakkında daha fazla bilgi için bkz. [Service Fabric Mesh Kaynak Modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
