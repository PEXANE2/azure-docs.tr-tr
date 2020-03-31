---
title: Azure API Management performansını artırmak için önbelleğe alma ekleme | Microsoft Belgeleri
description: API Management hizmeti çağrıları için gecikme, bant genişliği kullanımı ve web hizmeti yüklerini geliştirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: bf8d8a2c11962467300ae8d65fe5bbbe9a65cf92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75708364"
---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Azure API Management performansını artırmak için önbelleğe alma ekleme

API Management işlemleri yanıt önbelleğe alma için yapılandırılabilir. Yanıt önbelleğe alma, çok sık değişmeyen veriler için API gecikmesi, bant genişliği kullanımı ve web hizmeti yükünü önemli ölçüde azaltabilir.

Önbelleğe alma hakkında daha ayrıntılı bilgi için bkz. [API Management önbelleğe alma ilkeleri](api-management-caching-policies.md) ve [Azure API Management'te özel önbelleğe alma](api-management-sample-cache-by-key.md).

![önbellek ilkeleri](media/api-management-howto-cache/cache-policies.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * API'nize yanıt önbelleği ekleme
> * Eylem halinde önbelleğe alma işlemini doğrulama

## <a name="availability"></a>Kullanılabilirlik

> [!NOTE]
> Dahili önbellek, Azure API Yönetimi'nin **Tüketim** katmanında kullanılamaz. Bunun yerine [Redis için harici bir Azure Önbelleği kullanabilirsiniz.](api-management-howto-cache-external.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ [API içeri aktarma ve yayımlama](import-and-publish.md)

## <a name="add-the-caching-policies"></a><a name="caching-policies"> </a>Önbelleğe alma ilkelerini ekleme

Bu örnekte önbelleğe alma ilkeleri kullanılarak, **GetSpeakers** işlemine yapılan ilk istek işlemi arka uç hizmetinden bir yanıt döndürür. Bu yanıt, belirtilen üst bilgiler ve sorgu dizesi parametreleri tarafından önbelleğe alınır ve anahtarlanır. Eşleşen parametrelerle, işleme yapılan sonraki çağrılar, önbelleğe alma süresi aralığı sona erinceye kadar, önbelleğe alınan yanıtın döndürülmesini sağlar.

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. APIM örneğinize göz atın.
3. **API** sekmesini seçin.
4. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
5. **GetSpeakers**’ı seçin.
6. Ekranın üst kısmında **Tasarım** sekmesini seçin.
7. **Gelen işleme** bölümünde **</>** simgesine tıklayın.

    ![kod düzenleyicisi](media/api-management-howto-cache/code-editor.png)

8. **Gelen** öğesinde, şu ilkeyi ekleyin:

        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
            <vary-by-header>Authorization</vary-by-header>
        </cache-lookup>

9. **Giden** öğesinde, şu ilkeyi ekleyin:

        <cache-store duration="20" />

    **Süre** önbelleğe alınan yanıtların sona erme aralığını belirtir. Bu örnekte, aralık **20** saniyedir.

> [!TIP]
> [Azure API Yönetimi'nde Redis için harici bir Azure Önbelleği Kullan'da](api-management-howto-cache-external.md)açıklandığı gibi harici `caching-type` bir önbellek kullanıyorsanız, önbelleğe alma ilkelerinin özniteliğini belirtmek isteyebilirsiniz. Daha fazla ayrıntı için [API Yönetimi önbelleğe alma ilkelerine](api-management-caching-policies.md) bakın.

## <a name="call-an-operation-and-test-the-caching"></a><a name="test-operation"> </a>İşlem çağırma ve önbelleğe almayı test etme
Önbelleğe alma eylemini görmek için, işlemi geliştirici portalından çağırın.

1. Azure portalında APIM örneğinize göz atın.
2. **API'ler** sekmesini seçin.
3. Önbelleğe alma ilkelerini eklediğiniz API’leri seçin.
4. **GetSpeakers** işlemini seçin.
5. Sağ üst menüdeki **Test** sekmesine tıklayın.
6. **Gönder**’e basın.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar
* Önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz. [API Management ilke başvurusu][API Management policy reference]’nda [Önbelleğe alma ilkeleri][Caching policies].
* Anahtar kullanım ilkesi ifadeleri hakkında daha fazla bilgi için bkz. [Azure API Management’te özel önbelleğe alma](api-management-sample-cache-by-key.md).
* Redis için harici Azure Önbelleği kullanma hakkında daha fazla bilgi için [bkz.](api-management-howto-cache-external.md)

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: get-started-create-service-instance.md

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
