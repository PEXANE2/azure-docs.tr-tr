---
title: İlkeyi kullanarak yönetilen uygulama için ilişkilendirmeleri dağıtma
description: Azure Ilke hizmeti 'ni kullanarak yönetilen bir uygulama için ilişkilendirmeleri dağıtma hakkında bilgi edinin.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: ec0fe8f66ef2ad2458b4ffad0e848591793e5b05
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75650948"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>Azure Ilkesi kullanarak yönetilen bir uygulama için ilişkileri dağıtma

Azure ilkeleri, kaynakları yönetilen bir uygulamayla ilişkilendirmek için ilişkilendirmeler dağıtmak üzere kullanılabilir. Bu makalede, ilişkilendirmeleri dağıtan ve bu ilkeyi nasıl kullanabileceğiniz bir yerleşik ilke anlatılmaktadır.

## <a name="built-in-policy-to-deploy-associations"></a>İlişkilendirmeleri dağıtmaya yönelik yerleşik ilke

Yönetilen bir uygulama için ilişkilendirmeleri dağıtmak, bir kaynağı yönetilen bir uygulamayla ilişkilendirmek için ilişkilendirmeyi dağıtmak üzere kullanılabilen yerleşik bir ilkedir. İlke üç parametre kabul eder:

- Yönetilen uygulama KIMLIĞI-bu KIMLIK, kaynakların ilişkilendirilmesi gereken yönetilen uygulamanın kaynak KIMLIĞIDIR.
- İlişkilendirilecek kaynak türleri-bu kaynak türleri, yönetilen uygulamayla ilişkilendirilecek kaynak türleri listesidir. Aynı ilkeyi kullanarak birden çok kaynak türünü yönetilen bir uygulamayla ilişkilendirebilirsiniz.
- İlişkilendirme adı ön eki-bu dize, oluşturulmakta olan ilişkilendirme kaynağının adına eklenecek ön ektir. Varsayılan değer "DeployedByPolicy" ' dir.

İlke, DeployIfNotExists değerlendirmesi kullanır. Kaynak sağlayıcı seçili kaynak türlerine yönelik bir oluşturma veya güncelleştirme kaynağı isteği gerçekleştirdikten sonra çalışır ve değerlendirme bir başarı durum kodu döndürdü. Bundan sonra ilişkilendirme kaynağı bir şablon dağıtımı kullanılarak dağıtılır.
İlişkilendirmeler hakkında daha fazla bilgi için bkz. [Azure özel sağlayıcılar kaynak ekleme](../custom-providers/concepts-resource-onboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Dağıtım ilişkilendirmelerini yerleşik ilkesini kullanma 

### <a name="prerequisites"></a>Ön koşullar
Yönetilen uygulamanın bir eylem gerçekleştirmek için abonelik için izinleri olması gerekiyorsa, ilişkilendirme kaynağının ilke dağıtımı izinleri vermeden çalışmaz.

### <a name="policy-assignment"></a>İlke ataması
Yerleşik ilkeyi kullanmak için, bir ilke ataması oluşturun ve yönetilen uygulama ilkesi için dağıtım ilişkilendirmelerini atayın. İlke başarıyla atandıktan sonra, ilke uyumlu olmayan kaynakları belirler ve bu kaynaklar için ilişkilendirmeyi dağıtır.

![Yerleşik ilke atama](media/concepts-built-in-policy/assign-builtin-policy-managedapp.png)

## <a name="getting-help"></a>Yardım alma

Azure özel kaynak sağlayıcıları geliştirme hakkında sorularınız varsa [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)üzerinde soru sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce kontrol edin. ```azure-custom-providers```Hızlı bir yanıt almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ilişkilendirmeleri dağıtmak için yerleşik ilke kullanmayı öğrendi. Daha fazla bilgi için şu makalelere göz atın:

- [Kavramlar: Azure özel sağlayıcılar kaynak ekleme](../custom-providers/concepts-resource-onboarding.md)
- [Öğretici: özel sağlayıcılarla kaynak ekleme](../custom-providers/tutorial-resource-onboarding.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](../custom-providers/tutorial-get-started-with-custom-providers.md)
- [Hızlı başlangıç: özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](../custom-providers/create-custom-provider.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](../custom-providers/custom-providers-action-endpoint-how-to.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](../custom-providers/custom-providers-resources-endpoint-how-to.md)
