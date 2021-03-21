---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: a87fbbb276fd8813492cc293bc08b958ee3d6b7b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99070291"
---
Önceki adımdan kaynak grubunda bir Key Vault oluşturmak için Azure CLı [az keykasa Create](/cli/azure/keyvault#az_keyvault_create) komutunu kullanın. Bazı bilgileri sağlamanız gerekir:

- Anahtar Kasası adı: yalnızca rakam (0-9), harf (A-z, A-Z) ve kısa çizgi (-) içerebilen 3 ile 24 karakter arasında bir dize

  > [!Important]
  > Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. -Unique-keykasa-adı> <aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.

- Kaynak grubu adı: **Myresourcegroup**.
- Konum: **EastUS**.

```azurecli
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup" --location "EastUS"
```

Bu komutun çıktısı, yeni oluşturulan anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa adı**: Yukarıdaki--name parametresine verdiğiniz addır.
- **Kasa URI 'si**: örnekte bu https://- &lt; Unique-keykasasına-Name &gt; . Vault.Azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.
