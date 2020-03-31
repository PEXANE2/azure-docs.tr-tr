---
title: İlke yi kullanarak yönetilen uygulama için ilişkilendirmeleri dağıtma
description: Azure İlkesi hizmetini kullanarak yönetilen bir uygulama için ilişkilendirmedağıtma hakkında bilgi edinin.
author: msHich
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: ec0fe8f66ef2ad2458b4ffad0e848591793e5b05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75650948"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>Azure İlkesi'ni kullanarak yönetilen bir uygulama için ilişkilendirmedağıtma

Azure ilkeleri, kaynakları yönetilen bir uygulamaya ilişkilendirmek için ilişkilendirmeleri dağıtmak için kullanılabilir. Bu makalede, ilişkilendirmeleri dağıtan yerleşik bir ilkeyi ve bu ilkeyi nasıl kullanabileceğinizi açıklarız.

## <a name="built-in-policy-to-deploy-associations"></a>Derneklerdağıtmak için yerleşik ilke

Yönetilen bir uygulama için çağrışımları dağıtma, bir kaynağı yönetilen bir uygulamaya ilişkilendirmek için ilişkilendirmeyi dağıtmak için kullanılabilecek yerleşik bir ilkedir. İlke üç parametreyi kabul eder:

- Yönetilen uygulama kimliği - Bu kimlik, kaynakların ilişkilendirilmesi gereken yönetilen uygulamanın kaynak kimliğidir.
- İlişkilenecek kaynak türleri - Bu kaynak türleri, yönetilen uygulamayla ilişkilendirilecek kaynak türlerinin listesidir. Aynı ilkeyi kullanarak yönetilen bir uygulama yla birden çok kaynak türünü ilişkilendirebilirsiniz.
- İlişkilendirme adı öneki - Bu dize, oluşturulan ilişkilendirme kaynağının adına eklenecek önektir. Varsayılan değer "DeployedByPolicy"dir.

İlke, DeployIfNotExists değerlendirmesini kullanır. Bir Kaynak Sağlayıcı, seçili kaynak türü(ler) için kaynak oluşturma veya güncelleştirme isteğini işledikten ve değerlendirme bir başarı durum kodu döndürürsonra çalışır. Bundan sonra, ilişkilendirme kaynağı bir şablon dağıtımı kullanarak dağıtılır.
Dernekler hakkında daha fazla bilgi için Azure [Özel Sağlayıcıları kaynağına bürünme](../custom-providers/concepts-resource-onboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Dağıtım çağrışımları yerleşik ilkesi nasıl kullanılır? 

### <a name="prerequisites"></a>Ön koşullar
Yönetilen uygulamanın bir eylemi gerçekleştirmek için abonelik için izinlere ihtiyacı varsa, ilişkilendirme kaynağının ilke dağıtımı izinleri vermeden çalışmaz.

### <a name="policy-assignment"></a>İlke ataması
Yerleşik ilkeyi kullanmak için bir ilke ataması oluşturun ve dağıt ilişkilendirmelerini yönetilen bir uygulama ilkesine atayın. İlke başarıyla atandıktan sonra, ilke uyumlu olmayan kaynakları tanımlar ve bu kaynaklar için ilişkilendirme dağıtacaktır.

![Yerleşik ilke atama](media/concepts-built-in-policy/assign-builtin-policy-managedapp.png)

## <a name="getting-help"></a>Yardım alma

Azure Özel Kaynak Sağlayıcıları geliştirmesi hakkında sorularınız varsa, [bunları Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-custom-providers)sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce önce kontrol edin. Hızlı yanıt ```azure-custom-providers``` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ilişkileri dağıtmak için yerleşik ilkeyi kullanmayı öğrendiniz. Daha fazla bilgi edinmek için bu makalelere bakın:

- [Kavramlar: Azure Özel Sağlayıcıları kaynak onboarding](../custom-providers/concepts-resource-onboarding.md)
- [Öğretici: Özel sağlayıcılarla onboarding kaynak](../custom-providers/tutorial-resource-onboarding.md)
- [Öğretici: Azure'da özel eylemler ve kaynaklar oluşturun](../custom-providers/tutorial-get-started-with-custom-providers.md)
- [Hızlı başlatma: Özel bir kaynak sağlayıcısı oluşturun ve özel kaynakları dağıtın](../custom-providers/create-custom-provider.md)
- [Nasıl yapilir: Azure REST API'sine özel eylemler ekleme](../custom-providers/custom-providers-action-endpoint-how-to.md)
- [Nasıl yapilir: Azure REST API'sine özel kaynak ekleme](../custom-providers/custom-providers-resources-endpoint-how-to.md)
