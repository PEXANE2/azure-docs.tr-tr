---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a9a837d8a486e10ea2a62be69b889e657b1dbc05
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72601179"
---
Verileri bir taksonomiye mantıksal olarak düzenlemek için meta veriler sunarak Azure kaynaklarınıza Etiketler uygularsınız. Her etiket bir ad ve bir değer çiftinden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Etiketleri uyguladıktan sonra aboneliğinizde bu etiket adını ve değerini taşıyan tüm kaynakları alabilirsiniz. Etiketler, farklı kaynak gruplarından ilgili kaynakları almanızı sağlar. Bu yaklaşım, faturalama veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Taksonominiz, kullanıcıların yükünü azaltmak ve doğruluğu artırmak için otomatik etiketleme stratejisine ek olarak bir self servis meta veri etiketleme stratejisini göz önünde bulundurmalıdır.

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Tüm kaynak türleri etiketleri desteklemez. Bir kaynak türüne etiket uygulayıp uygulayacağınızı öğrenmek için bkz. [Azure kaynakları Için etiket desteği](../articles/azure-resource-manager/tag-support.md).
* Her kaynak veya kaynak grubu en fazla 50 etiket adı/değer çiftine sahip olabilir. Şu anda, depolama hesapları yalnızca 15 etiketi destekler, ancak bu sınır gelecek sürümlerde 50 'e yükseltilir. İzin verilen maksimum sayıdan daha fazla etiket uygulamanız gerekiyorsa, etiket değeri için bir JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bir kaynak grubu, her birinin 50 etiket adı/değer çifti olan çok sayıda kaynak içerebilir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Genelleştirilmiş VM 'Ler etiketleri desteklemez.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.
* Etiketler, Cloud Services gibi klasik kaynaklara uygulanamaz.
* Etiket adları şu karakterleri içeremez: `<`, `>`, `%`, `&`, `\`, `?`, `/`

   > [!NOTE]
   > Şu anda Azure DNS bölgeler ve trafik yöneticisi hizmetleri de etikette boşluk kullanılmasına izin vermez. 
