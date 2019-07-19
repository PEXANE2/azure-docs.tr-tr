---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 674dd30ff3e493ec4c4036f032f82624a6ca5749
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334236"
---
## <a name="azure-cognitive-service-resource-types"></a>Azure bilişsel hizmet kaynak türleri

> [!NOTE]
> Abonelik sahipleri, [Azure ilkesini](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition)uygulayarak, "izin verilmeyen kaynak türleri" ilke tanımına atayarak ve şunları belirterek **kaynak grupları ve abonelikler için bilişsel hizmetler kaynaklarının oluşturulmasını devre dışı bırakabilir Hedef kaynak türü olarak Microsoft. Biliveservices/hesapları** .

Azure bilişsel hizmetlere iki farklı kaynak üzerinden erişebilirsiniz: Çoklu hizmet kaynağı veya tek hizmet bir. Bu abonelikler, aynı anda tek bir hizmete veya birden çok hizmete bağlanmanızı sağlar.

### <a name="multi-service-resource"></a>Çoklu hizmet kaynağı

>[!WARNING]
> Şu anda, **Bu hizmetler çoklu** hizmet anahtarlarını desteklemez: Soru-Cevap Oluşturma, konuşma Hizmetleri, Özel Görüntü İşleme ve anomali algılayıcısı.

Çok hizmet bilişsel hizmetler kaynağına abone olma:
* Azure bilişsel hizmetler için tek bir Azure kaynağı kullanmanıza olanak sağlar.
* Kullandığınız hizmetlerden faturalandırmayı birleştirir. Ek bilgi için bkz. bilişsel [Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/) .

### <a name="single-service-resource"></a>Tek hizmet kaynağı

Tek hizmet kaynakları (Görüntü İşleme veya konuşma hizmetleri gibi), belirtilen hizmet ile kısıtlıdır.