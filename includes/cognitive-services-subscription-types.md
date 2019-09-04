---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 3b808f4eb853cf05eb08cd1acf08dedccabf71a6
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274604"
---
## <a name="azure-cognitive-service-resource-types"></a>Azure bilişsel hizmet kaynak türleri

<!-- > [!NOTE]
> Subscription owners can disable the creation of Cognitive Services resources for resource groups and subscriptions by applying [Azure policy](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), assigning a “Not allowed resource types” policy definition, and specifying **Microsoft.CognitiveServices/accounts** as the target resource type. -->
Azure bilişsel hizmetlere iki farklı kaynak üzerinden erişebilirsiniz: Çoklu hizmet kaynağı veya tek hizmet bir. Bu abonelikler, tek bir bilişsel hizmete veya aynı anda birden çok bilişsel hizmete bağlanmanızı sağlar.

### <a name="multi-service-resource"></a>Çoklu hizmet kaynağı

Çok hizmet bilişsel hizmetler kaynağına abone olma:
* Azure bilişsel hizmetler için tek bir Azure kaynağı kullanmanıza olanak sağlar.
* Birden çok Azure bilişsel hizmeti ile kullanılabilecek tek bir anahtar elde edersiniz.
* Kullandığınız hizmetlerden faturalandırmayı birleştirir. Ek bilgi için bkz. bilişsel [Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/) .

>[!WARNING]
> Şu anda, **Bu hizmetler çoklu** hizmet anahtarlarını desteklemez: Soru-Cevap Oluşturma, konuşma Hizmetleri, Özel Görüntü İşleme ve anomali algılayıcısı.

### <a name="single-service-resource"></a>Tek hizmet kaynağı

Tek hizmet bilişsel hizmetler kaynağına abone olma:
* Kaynağı oluşturduğunuz belirli bir bilişsel hizmeti (Görüntü İşleme veya konuşma hizmetleri gibi) kullanmanıza olanak tanır.
* İçin bir kaynak oluşturduğunuz bilişsel hizmete özgü bir anahtar elde edersiniz.