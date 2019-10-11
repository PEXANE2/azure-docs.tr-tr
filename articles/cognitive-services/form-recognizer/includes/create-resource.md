---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: c2d3119283b5b75eeca6b0e0737f62eee00e81c6
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72237474"
---
Form tanıyıcı kullanma erişimi verildiğinde, birkaç bağlantı ve kaynakla bir hoş geldiniz e-postası alacaksınız. Bu iletideki "Azure portal" bağlantısını kullanarak Azure portal açın ve bir form tanıyıcı kaynağı oluşturun. **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Ad** | Kaynağınız için açıklayıcı bir ad. Örneğin, *Mynameformtanıyıcı*gibi açıklayıcı bir ad kullanmanızı öneririz. |
| **Aboneliğiniz** | Erişim izni verilen Azure aboneliğini seçin. |
| **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
| **Fiyatlandırma Katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | Kaynağı içerecek [Azure Kaynak grubu](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

> [!IMPORTANT]
> Genellikle Azure portal bir bilişsel hizmet kaynağı oluşturduğunuzda, çok sunuculu bir abonelik anahtarı (birden çok bilişsel hizmetler genelinde kullanılan) veya tek servis abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğiniz vardır. Ancak, form tanıyıcı bir önizleme sürümü olduğundan, bu, çoklu hizmet aboneliğine dahil değildir ve hoş geldiniz e-postasında sağlanan bağlantıyı kullanmadığınız müddetçe tek servis aboneliğini oluşturamazsınız.

Form tanıyıcı kaynağınız dağıtımı bitirdiğinde portalda **tüm kaynaklar** listesinden bulun ve seçin. Ardından, abonelik anahtarlarınızı görüntülemek için **anahtarlar** sekmesini seçin. Her iki anahtar de uygulamanıza kaynağa erişim sağlayacak. **Anahtar 1**değerini kopyalayın.