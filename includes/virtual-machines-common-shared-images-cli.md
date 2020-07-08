---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/24/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: b49dc6ef2bfee311bc3ca524a5ccb0a4e4b5ca9c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793651"
---
## <a name="create-an-image-gallery"></a>Görüntü galerisi oluşturma 

Görüntü Galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. 

Galeri adı için izin verilen karakterler büyük veya küçük harflerden, rakamlardan, noktalardan ve noktalardan oluşur. Galeri adı tire içeremez.   Galeri adları, aboneliğiniz dahilinde benzersiz olmalıdır. 

[Az Sig Create](/cli/azure/sig#az-sig-create)kullanarak bir görüntü galerisi oluşturun. Aşağıdaki örnek, *Doğu ABD*Içinde *mygallerrg* adlı Galeri adlı bir kaynak grubu ve *MyGallery*adlı bir galeri oluşturur.

```azurecli-interactive
az group create --name myGalleryRG --location eastus
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="share-the-gallery"></a>Galeriyi paylaşma

Rol tabanlı Access Control (RBAC) kullanarak, abonelikler arasında görüntü paylaşabilirsiniz. Galeri, görüntü tanımı veya görüntü sürümü düzeyinde görüntü paylaşabilirsiniz. Abonelikler arasında bile, görüntü sürümü üzerinde okuma izinlerine sahip olan tüm kullanıcılar, görüntü sürümünü kullanarak bir sanal makine dağıtacaktır.

Galeri düzeyindeki diğer kullanıcılarla paylaşmanızı öneririz. Galerinizin nesne KIMLIĞINI almak için [az SIG Show](/cli/azure/sig#az-sig-show)' ı kullanın.

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Nesne KIMLIĞINI bir e-posta adresi ile birlikte kullanın ve bir kullanıcıya paylaşılan görüntü galerisine erişim sağlamak için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create) ' u kullanın. `<email-address>`Ve `<gallery iD>` bilgilerini kendi bilgileriniz ile değiştirin.

```azurecli-interactive
az role assignment create \
   --role "Reader" \
   --assignee <email address> \
   --scope <gallery ID>
```

RBAC kullanarak kaynakları paylaşma hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).
