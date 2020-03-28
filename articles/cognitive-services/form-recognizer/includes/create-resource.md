---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: 33624ab800bd1155b52efbc05f317122a99bb479
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78205839"
---
Azure portalına gidin <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="ve yeni bir" target="_blank">Form Tanıyıcı kaynağı <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>oluşturun yeni bir Form Tanıyıcı kaynağı oluşturun. **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Adı** | Kaynağınız için açıklayıcı bir ad. Biz açıklayıcı bir ad kullanmanızı öneririz, örneğin *MyNameFormRecognizer*. |
| **Abonelik** | Erişim izni verilen Azure aboneliğini seçin. |
| **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme süresine neden olabilir, ancak kaynağınızın çalışma zamanı kullanılabilirliği üzerinde hiçbir etkisi yoktur. |
| **Fiyatlandırma katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için API [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/cognitive-services/)bakın.
| **Kaynak grubu** | Kaynağınızı içerecek [Azure kaynak grubu.](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) Yeni bir grup oluşturabilir veya önceden varolan bir gruba ekleyebilirsiniz. |

> [!IMPORTANT]
> Normalde Azure portalında bir Bilişsel Hizmet kaynağı oluşturduğunuzda, çok hizmetli abonelik anahtarı (birden çok bilişsel hizmette kullanılır) veya tek hizmetli abonelik anahtarı (yalnızca belirli bir bilişsel hizmetle birlikte kullanılır) oluşturma seçeneğiniz vardır. Ancak, Form Ayırıcı bir önizleme sürümü olduğundan, çok hizmet li aboneye dahil edilmez ve Hoş Geldiniz e-postasında sağlanan bağlantıyı kullanmadığınız sürece tek hizmet aboneliği oluşturamazsınız.

Form Recognizer kaynağınız dağıtımı bitirdiğinde, onu portaldaki **Tüm kaynaklar** listesinden bulun ve seçin. Ardından abonelik verilerinizi görüntülemek için **Hızlı başlat** sekmesini seçin. **Key1** ve **Endpoint** değerlerini geçici bir konuma kaydedin. Bunları aşağıdaki adımlarda kullanırsınız.
