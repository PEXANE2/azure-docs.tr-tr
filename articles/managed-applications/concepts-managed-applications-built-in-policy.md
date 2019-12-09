---
title: İlkeyi kullanarak Azure yönetilen uygulaması için ilişkilendirmeleri dağıtma
description: Azure Ilke hizmeti 'ni kullanarak yönetilen bir uygulama için ilişkilendirmeleri dağıtma hakkında bilgi edinin.
author: msHich
ms.service: managed-applications
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: e3dda343c1d5664e188b434c54e4c288b515d420
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932534"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>Azure Ilkesi kullanarak yönetilen bir uygulama için ilişkileri dağıtma

Azure ilkeleri, kaynakları yönetilen bir uygulamayla ilişkilendirmek için ilişkilendirmeler dağıtmak üzere kullanılabilir. Bu makalede, ilişkilendirmeleri dağıtan ve bu ilkeyi nasıl kullanabileceğiniz bir yerleşik ilke anlatılmaktadır.

## <a name="built-in-policy-to-deploy-associations"></a>İlişkilendirmeleri dağıtmaya yönelik yerleşik ilke

Yönetilen bir uygulama için ilişkilendirmeleri dağıtmak, bir kaynağı yönetilen bir uygulamayla ilişkilendirmek için ilişkilendirmeyi dağıtmak üzere kullanılabilen yerleşik bir ilkedir. İlke üç parametre kabul eder:

- Yönetilen uygulama kimliği-bu KIMLIK, kaynakların ilişkilendirilmesi gereken yönetilen uygulamanın kaynak KIMLIĞIDIR.
- İlişkilendirilecek kaynak türleri-bu kaynak türleri, yönetilen uygulamayla ilişkilendirilecek kaynak türleri listesidir. Aynı ilkeyi kullanarak birden çok kaynak türünü yönetilen bir uygulamayla ilişkilendirebilirsiniz.
- İlişkilendirme adı ön eki-bu dize, oluşturulmakta olan ilişkilendirme kaynağının adına eklenecek ön ektir. Varsayılan değer "DeployedByPolicy" ' dir.

İlke, DeployIfNotExists değerlendirmesi kullanır. Kaynak sağlayıcı seçili kaynak türlerine yönelik bir oluşturma veya güncelleştirme kaynağı isteği gerçekleştirdikten sonra çalışır ve değerlendirme bir başarı durum kodu döndürdü. Bundan sonra ilişkilendirme kaynağı bir şablon dağıtımı kullanılarak dağıtılır.
İlişkilendirmeler hakkında daha fazla bilgi için bkz. [Azure özel sağlayıcılar kaynak ekleme](./concepts-custom-providers-resourceonboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Dağıtım ilişkilendirmelerini yerleşik ilkesini kullanma 

### <a name="prerequisites"></a>Önkoşullar
Yönetilen uygulamanın bir eylem gerçekleştirmek için abonelik için izinleri olması gerekiyorsa, ilişkilendirme kaynağının ilke dağıtımı izinleri vermeden çalışmaz.

### <a name="policy-assignment"></a>İlke ataması
Yerleşik ilkeyi kullanmak için, bir ilke ataması oluşturun ve yönetilen uygulama ilkesi için dağıtım ilişkilendirmelerini atayın. İlke başarıyla atandıktan sonra, ilke uyumlu olmayan kaynakları belirler ve bu kaynaklar için ilişkilendirmeyi dağıtır.

![Yerleşik ilke atama](media/builtin-policy/assign-builtin-policy-managedapp.png)

## <a name="getting-help"></a>Yardım alma

Azure özel kaynak sağlayıcıları geliştirme hakkında sorularınız varsa [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)üzerinde soru sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce kontrol edin. Hızlı bir yanıt almak için etiketi ```azure-custom-providers``` ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ilişkilendirmeleri dağıtmak için yerleşik ilke kullanmayı öğrendi. Daha fazla bilgi için şu makalelere göz atın:

- [Kavramlar: Azure özel sağlayıcılar kaynak ekleme](./concepts-custom-providers-resourceonboarding.md)
- [Öğretici: özel sağlayıcılarla kaynak ekleme](./tutorial-custom-providers-resource-onboarding.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-custom-providers-101.md)
- [Hızlı başlangıç: özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
