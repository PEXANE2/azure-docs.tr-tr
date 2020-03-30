---
title: İlkeleri kullanarak ilişkilendirmeleri dağıtma
description: Azure İlkesi hizmetini kullanarak özel bir sağlayıcı için çağrışımdağıtma hakkında bilgi edinin.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: 536b95eb07619d0ce2d02ec01e1f51ed52c1b5e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650480"
---
# <a name="deploy-associations-for-a-custom-provider-using-azure-policy"></a>Azure ilkesini kullanarak özel bir sağlayıcı için ilişkilendirmedağıtma

Azure ilkeleri, kaynakları özel bir sağlayıcıya ilişkilendirmek için ilişkilendirmeleri dağıtmak için kullanılabilir. Bu makalede, ilişkilendirmeleri dağıtan yerleşik bir ilkeyi ve bu ilkeyi nasıl kullanabileceğinizi açıklarız.

## <a name="built-in-policy-to-deploy-associations"></a>Derneklerdağıtmak için yerleşik ilke

Özel sağlayıcı için çağrışımları dağıtma, bir kaynağı özel bir sağlayıcıya ilişkilendirmek için ilişkilendirme dağıtmak için kullanılabilecek yerleşik bir ilkedir. İlke üç parametreyi kabul eder:

- Özel sağlayıcı kimliği - Bu kimlik, kaynakların ilişkilendirilmesi gereken özel sağlayıcının kaynak kimliğidir.
- İlişkileştirilecek kaynak türleri - Bu kaynak türleri, özel sağlayıcıyla ilişkilendirilecek kaynak türlerinin listesidir. Aynı ilkeyi kullanarak birden çok kaynak türünü özel bir sağlayıcıyla ilişkilendirebilirsiniz.
- İlişkilendirme adı öneki - Bu dize, oluşturulan ilişkilendirme kaynağının adına eklenecek önektir. Varsayılan değer "DeployedByPolicy"dir.

İlke, DeployIfNotExists değerlendirmesini kullanır. Kaynak Sağlayıcı kaynak isteği oluşturma veya güncelleştirme yi ele aldıktan ve değerlendirme bir başarı durum kodu döndürür sonra çalışır. Bundan sonra, ilişkilendirme kaynağı bir şablon dağıtımı kullanarak dağıtılır.
Dernekler hakkında daha fazla bilgi için Azure [Özel Sağlayıcıları kaynağına bürünme](./concepts-resource-onboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Dağıtım çağrışımları yerleşik ilkesi nasıl kullanılır? 

### <a name="prerequisites"></a>Ön koşullar
Özel sağlayıcının bir eylem gerçekleştirmek için ilke kapsamında izinlere ihtiyacı varsa, ilişkilendirme kaynağının ilke dağıtımı izinleri vermeden çalışmaz.

### <a name="policy-assignment"></a>İlke ataması
Yerleşik ilkeyi kullanmak için bir ilke ataması oluşturun ve özel sağlayıcı ilkesi için Dağıtım ilişkilendirmelerini atayın. İlke daha sonra uyumlu olmayan kaynakları tanımlar ve bu kaynaklar için ilişkilendirme dağıtAcaktır.

![Yerleşik ilke atama](media/concepts-built-in-policy/assign-builtin-policy-customprovider.png)

## <a name="getting-help"></a>Yardım alma

Azure Özel Kaynak Sağlayıcıları geliştirmesi hakkında sorularınız varsa, [bunları Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-custom-providers)sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce önce kontrol edin. Hızlı yanıt ```azure-custom-providers``` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ilişkileri dağıtmak için yerleşik ilkeyi kullanmayı öğrendiniz. Daha fazla bilgi edinmek için bu makalelere bakın:

- [Kavramlar: Azure Özel Sağlayıcıları kaynak onboarding](./concepts-resource-onboarding.md)
- [Öğretici: Özel sağlayıcılarla onboarding kaynak](./tutorial-resource-onboarding.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](./tutorial-get-started-with-custom-providers.md)
- [Hızlı başlatma: Özel bir kaynak sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Nasıl yapilir: Azure REST API'sine özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapilir: Azure REST API'sine özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
