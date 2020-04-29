---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: 33624ab800bd1155b52efbc05f317122a99bb479
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78205839"
---
Azure portal gidin <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="ve yeni bir form tanıyıcı kaynağı oluşturun yeni bir" target="_blank">form tanıyıcı kaynağı <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>oluşturun. **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Adı** | Kaynağınız için açıklayıcı bir ad. Örneğin, *Mynameformtanıyıcı*gibi açıklayıcı bir ad kullanmanızı öneririz. |
| **Abonelik** | Erişim izni verilen Azure aboneliğini seçin. |
| **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
| **Fiyatlandırma Katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | Kaynağı içerecek [Azure Kaynak grubu](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

> [!IMPORTANT]
> Genellikle Azure portal bir bilişsel hizmet kaynağı oluşturduğunuzda, çok sunuculu bir abonelik anahtarı (birden çok bilişsel hizmetler genelinde kullanılan) veya tek servis abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğiniz vardır. Ancak, form tanıyıcı bir önizleme sürümü olduğundan, bu, çoklu hizmet aboneliğine dahil değildir ve hoş geldiniz e-postasında sağlanan bağlantıyı kullanmadığınız müddetçe tek servis aboneliğini oluşturamazsınız.

Form tanıyıcı kaynağınız dağıtımı bitirdiğinde portalda **tüm kaynaklar** listesinden bulun ve seçin. Ardından, abonelik verilerinizi görüntülemek için **hızlı başlangıç** sekmesini seçin. **KEY1** ve **uç nokta** değerlerini geçici bir konuma kaydedin. Bunları aşağıdaki adımlarda kullanacaksınız.
