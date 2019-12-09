---
title: İlkeyi kullanarak Azure özel sağlayıcısı için ilişkilendirmeleri dağıtma
description: Azure Ilke hizmeti 'ni kullanarak özel bir sağlayıcının ilişkilendirmelerini dağıtma hakkında bilgi edinin.
author: msHich
ms.service: managed-applications
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: 94f7128a473266824242f52a9c50f09b2c24f1d4
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932651"
---
# <a name="deploy-associations-for-a-custom-provider-using-azure-policy"></a>Azure ilkesi kullanarak özel bir sağlayıcının ilişkilendirmelerini dağıtma

Azure ilkeleri, kaynakları özel bir sağlayıcıya ilişkilenmek üzere ilişkiler dağıtmak için kullanılabilir. Bu makalede, ilişkilendirmeleri dağıtan ve bu ilkeyi nasıl kullanabileceğiniz bir yerleşik ilke anlatılmaktadır.

## <a name="built-in-policy-to-deploy-associations"></a>İlişkilendirmeleri dağıtmaya yönelik yerleşik ilke

Özel bir sağlayıcının dağıtım ilişkilerini, bir kaynağı özel bir sağlayıcıyla ilişkilendirmek üzere ilişkilendirmeyi dağıtmak için kullanılabilen yerleşik bir ilkedir. İlke üç parametre kabul eder:

- Özel sağlayıcı kimliği-bu KIMLIK, kaynakların ilişkilendirilmesi gereken özel sağlayıcının kaynak KIMLIĞIDIR.
- İlişkilendirilecek kaynak türleri-bu kaynak türleri, özel sağlayıcıyla ilişkilendirilecek kaynak türleri listesidir. Aynı ilkeyi kullanarak birden çok kaynak türünü özel bir sağlayıcıyla ilişkilendirebilirsiniz.
- İlişkilendirme adı ön eki-bu dize, oluşturulmakta olan ilişkilendirme kaynağının adına eklenecek ön ektir. Varsayılan değer "DeployedByPolicy" ' dir.

İlke, DeployIfNotExists değerlendirmesi kullanır. Kaynak sağlayıcı bir oluşturma veya güncelleştirme kaynak isteği tamamladıktan sonra çalışır ve değerlendirme bir başarı durum kodu döndürdü. Bundan sonra ilişkilendirme kaynağı bir şablon dağıtımı kullanılarak dağıtılır.
İlişkilendirmeler hakkında daha fazla bilgi için bkz. [Azure özel sağlayıcılar kaynak ekleme](./concepts-custom-providers-resourceonboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Dağıtım ilişkilendirmelerini yerleşik ilkesini kullanma 

### <a name="prerequisites"></a>Önkoşullar
Özel sağlayıcının bir eylemi gerçekleştirmek için ilke kapsamında izinleri olması gerekiyorsa, ilişkilendirme kaynağının ilke dağıtımı izinleri vermeden çalışmaz.

### <a name="policy-assignment"></a>İlke ataması
Yerleşik ilkeyi kullanmak için, bir ilke ataması oluşturun ve özel bir sağlayıcı ilkesinin dağıtım ilişkilendirmelerini atayın. İlke daha sonra uyumlu olmayan kaynakları belirler ve bu kaynaklar için ilişkilendirmeyi dağıtır.

![Yerleşik ilke atama](media/builtin-policy/assign-builtin-policy-customprovider.png)

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
