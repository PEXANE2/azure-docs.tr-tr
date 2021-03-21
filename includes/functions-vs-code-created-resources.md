---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/31/2021
ms.author: glenga
ms.openlocfilehash: 9a5c143927f549124cb6e69a4c8eb4829709a9e9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99493492"
---
+ İlgili kaynaklar için mantıksal kapsayıcı olan bir [kaynak grubu](../articles/azure-resource-manager/management/overview.md).
+ Projeleriniz hakkında durum ve diğer bilgileri tutan standart bir [Azure depolama hesabı](../articles/storage/common/storage-account-create.md).
+ Sunucusuz işlev uygulamanız için temel Konağı tanımlayan bir tüketim planı. 
+ İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması, aynı barındırma planı içindeki kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.
+ Sunucusuz işlevinizin kullanımını izleyen işlev uygulamasına bağlı bir Application Insights örneği.