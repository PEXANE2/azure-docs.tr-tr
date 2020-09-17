---
title: dosya dahil etme
description: dosya dahil etme
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3221c4f3e196cf1573bd7c0424fa3b4530c0b2ca
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709684"
---
## <a name="create-azure-context"></a>Azure bağlamı oluştur

Azure Container Instances kapsayıcıları çalıştırmak için Docker komutlarını kullanmak için önce Azure 'da oturum açın:

```bash
docker login azure
```

İstendiğinde, Azure kimlik bilgilerinizi girin veya seçin.


Çalıştırarak bir acı bağlamı oluşturun `docker context create aci` . Bu bağlam Docker 'ı bir Azure aboneliğiyle ve kaynak grubuyla ilişkilendirir, böylece kapsayıcı örnekleri oluşturabilir ve yönetebilirsiniz. Örneğin, *myacicontext*adlı bir bağlam oluşturmak için:

```
docker context create aci myacicontext
```

İstendiğinde, Azure abonelik KIMLIĞINIZI seçin, sonra var olan bir kaynak grubunu seçin veya **Yeni bir kaynak grubu oluşturun**. Yeni bir kaynak grubu seçerseniz, sistem tarafından oluşturulan bir adla oluşturulur. Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.


' `docker context ls` İ çalıştırarak, Docker bağlamlarınızın aci bağlamını eklediğini doğrulayın:

```
docker context ls
```