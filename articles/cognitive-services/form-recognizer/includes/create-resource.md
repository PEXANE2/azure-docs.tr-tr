---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: 897f2b728dc068b09849d4f48f899b8630a87a51
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96004016"
---
Azure portal gidin ve <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title=" Yeni bir form tanıyıcı kaynağı oluşturun " target="_blank"> Yeni bir form tanıyıcı kaynağı oluşturun <span class="docon docon-navigate-external x-hidden-focus"></span> </a> . **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Ad** | Kaynağınız için açıklayıcı bir ad. Örneğin, *Mynameformtanıyıcı* gibi açıklayıcı bir ad kullanmanızı öneririz. |
| **Abonelik** | Erişim izni verilen Azure aboneliğini seçin. |
| **Konum** | Bilişsel hizmet örneğinizin konumu. Farklı konumlar gecikme gösterebilir, ancak kaynağınızın çalışma zamanı kullanılabilirliğini etkilemez. |
| **Fiyatlandırma katmanı** | Kaynağınızın maliyeti seçtiğiniz fiyatlandırma katmanına ve kullanımınıza bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | Kaynağı içerecek [Azure Kaynak grubu](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) . Yeni bir grup oluşturabilir veya önceden var olan bir gruba ekleyebilirsiniz. |

> [!NOTE]
> Genellikle Azure portal bir bilişsel hizmet kaynağı oluşturduğunuzda, çok sunuculu bir abonelik anahtarı (birden çok bilişsel hizmetler genelinde kullanılan) veya tek servis abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğiniz vardır. Ancak şu anda form tanıyıcı, çok hizmet aboneliğine dahil değildir.

Form tanıyıcı kaynağınız dağıtımı bitirdiğinde portalda **tüm kaynaklar** listesinden bulun ve seçin. Anahtarınız ve uç noktanız, kaynak yönetimi altında kaynağın anahtar ve uç nokta sayfasında yer alır. Bundan sonra her ikisini de geçici bir konuma kaydedin.