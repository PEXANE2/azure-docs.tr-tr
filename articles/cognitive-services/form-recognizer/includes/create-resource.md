---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: dce9b4ffa05b48fc1789859328b221ac1a0f3a27
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68594448"
---
Form tanıyıcı kullanma erişimi verildiğinde, birkaç bağlantı ve kaynakla bir hoş geldiniz e-postası alacaksınız. Bu iletideki "Azure portal" bağlantısını kullanarak Azure portal açın ve bir form tanıyıcı kaynağı oluşturun. **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Name** | Kaynağınız için açıklayıcı bir ad. Örneğin, *Mynameformtanıyıcı*gibi açıklayıcı bir ad kullanmanızı öneririz. |
| **Abonelik** | Erişim izni verilen Azure aboneliğini seçin. |
| **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
| **Fiyatlandırma katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | Kaynağı içerecek [Azure Kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

> [!IMPORTANT]
> Genellikle Azure portal bir bilişsel hizmet kaynağı oluşturduğunuzda, çok sunuculu bir abonelik anahtarı (birden çok bilişsel hizmetler genelinde kullanılan) veya tek servis abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğiniz vardır. Ancak, form tanıyıcı bir önizleme sürümü olduğundan, bu, çoklu hizmet aboneliğine dahil değildir ve hoş geldiniz e-postasında sağlanan bağlantıyı kullanmadığınız müddetçe tek servis aboneliğini oluşturamazsınız.

Form tanıyıcı kaynağınız dağıtımı bitirdiğinde portalda **tüm kaynaklar** listesinden bulun ve seçin. Ardından, abonelik anahtarlarınızı görüntülemek için **anahtarlar** sekmesini seçin. Her iki anahtar de uygulamanıza kaynağa erişim sağlayacak. **Anahtar 1**değerini kopyalayın.