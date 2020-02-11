---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: 0a4faad8bfe92a5389e0ee440aa3ccc404535955
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77118076"
---
Azure portalına gidin ve [Yeni bir form tanıyıcı kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer). **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Ad** | Kaynağınız için açıklayıcı bir ad. Örneğin, *Mynameformtanıyıcı*gibi açıklayıcı bir ad kullanmanızı öneririz. |
| **Abonelik** | Erişim izni verilen Azure aboneliğini seçin. |
| **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
| **Fiyatlandırma katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | Kaynağı içerecek [Azure Kaynak grubu](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

> [!IMPORTANT]
> Genellikle Azure portal bir bilişsel hizmet kaynağı oluşturduğunuzda, çok sunuculu bir abonelik anahtarı (birden çok bilişsel hizmetler genelinde kullanılan) veya tek servis abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğiniz vardır. Ancak, form tanıyıcı bir önizleme sürümü olduğundan, bu, çoklu hizmet aboneliğine dahil değildir ve hoş geldiniz e-postasında sağlanan bağlantıyı kullanmadığınız müddetçe tek servis aboneliğini oluşturamazsınız.

Form tanıyıcı kaynağınız dağıtımı bitirdiğinde portalda **tüm kaynaklar** listesinden bulun ve seçin. Ardından, abonelik verilerinizi görüntülemek için **hızlı başlangıç** sekmesini seçin. **KEY1** ve **uç nokta** değerlerini geçici bir konuma kaydedin. Bunları aşağıdaki adımlarda kullanacaksınız.