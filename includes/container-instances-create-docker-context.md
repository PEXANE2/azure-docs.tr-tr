---
title: include dosyası
description: include dosyası
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3221c4f3e196cf1573bd7c0424fa3b4530c0b2ca
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90709684"
---
## <a name="create-azure-context"></a>Azure bağlamı oluştur

Azure Container Instances kapsayıcıları çalıştırmak için Docker komutlarını kullanmak için önce Azure 'da oturum açın:

```bash
docker login azure
```

İstendiğinde, Azure kimlik bilgilerinizi girin veya seçin.


Çalıştırarak bir acı bağlamı oluşturun `docker context create aci` . Bu bağlam Docker 'ı bir Azure aboneliğiyle ve kaynak grubuyla ilişkilendirir, böylece kapsayıcı örnekleri oluşturabilir ve yönetebilirsiniz. Örneğin, *myacicontext* adlı bir bağlam oluşturmak için:

```
docker context create aci myacicontext
```

İstendiğinde, Azure abonelik KIMLIĞINIZI seçin, sonra var olan bir kaynak grubunu seçin veya **Yeni bir kaynak grubu oluşturun**. Yeni bir kaynak grubu seçerseniz, sistem tarafından oluşturulan bir adla oluşturulur. Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.


' `docker context ls` İ çalıştırarak, Docker bağlamlarınızın aci bağlamını eklediğini doğrulayın:

```
docker context ls
```