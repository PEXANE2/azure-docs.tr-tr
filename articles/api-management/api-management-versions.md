---
title: Azure API Management sürümler | Microsoft Docs
description: Azure API Management sürüm kavramı hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: johndowns
ms.service: api-management
ms.topic: article
ms.date: 06/12/2020
ms.author: jodowns
ms.custom: fasttrack-new
ms.openlocfilehash: 578bb511175d88a1507af9520265a1acd068b27c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099849"
---
# <a name="versions-in-azure-api-management"></a>Azure API Management sürümler

Sürümler, Geliştiricilerinize ilgili API gruplarını sunmanıza olanak tanır. API 'inizdeki değişiklikleri güvenli bir şekilde işlemek için sürümleri kullanabilirsiniz. İstemciler, mevcut olduğunda yeni API sürümünüzü kullanmayı tercih edebilir, ancak mevcut istemciler eski bir sürümü kullanmaya devam eder. Sürümler, bir sürüm tanımlayıcısı (seçtiğiniz herhangi bir dize değeri) ile farklılaştırırlar ve bir sürüm oluşturma düzeni istemcilerin hangi API sürümünü kullanmak istediğini belirlemesine izin verir.

Çoğu amaçla, her API sürümü kendi bağımsız API 'SI olarak kabul edilebilir. İki farklı API sürümü farklı işlem kümelerine ve farklı ilkelere sahip olabilir.

Sürümleriyle şunları yapabilirsiniz:

- API 'nizin birden fazla sürümünü aynı anda yayımlayın.
- Sürümler arasında ayrım yapmak için bir yol, sorgu dizesi veya üst bilgi kullanın.
- Sürümünüzü tanımlamak istediğiniz herhangi bir dize değerini kullanın; bu sayı, bir tarih veya bir ad olabilir.
- API sürümlerinizi Geliştirici Portalında birlikte gruplanmış olarak gösterin.
- Mevcut (sürümlenmemiş) bir API alın ve mevcut istemcileri bozmadan yeni bir sürümünü oluşturun.

[Kılavuzumuzu izleyerek sürümler ile çalışmaya başlayın.](./api-management-get-started-publish-versions.md)

## <a name="versioning-schemes"></a>Sürüm oluşturma şemaları

Farklı API geliştiricilerinin sürüm oluşturma için farklı gereksinimleri vardır. Azure API Management, sürüm oluşturma için tek bir yaklaşım değildir, bunun yerine çeşitli seçenekler sağlar.

### <a name="path-based-versioning"></a>Yol tabanlı sürüm oluşturma

Yol sürüm oluşturma düzeni kullanıldığında, API isteklerinin URL yoluna sürüm tanımlayıcısının dahil olması gerekir.

Örneğin, `https://apis.contoso.com/products/v1` ve `https://apis.contoso.com/products/v2` sürümlerini ve sırasıyla aynı API 'ye başvurabilir `products` `v1` `v2` .

Üst bilgi tabanlı sürüm oluşturma kullanılırken bir API isteği URL 'sinin biçimi: `https://{yourDomain}/{apiName}/{versionIdentifier}/{operationId}` .

### <a name="header-based-versioning"></a>Üst bilgi tabanlı sürüm oluşturma

Üst bilgi sürümü oluşturma düzeni kullanıldığında, herhangi bir API isteği için sürüm tanımlayıcısının bir HTTP istek üstbilgisine dahil olması gerekir. HTTP istek üstbilgisinin adını belirtebilirsiniz.

Örneğin, adlı bir özel üst bilgi oluşturabilirsiniz `Api-Version` ve istemciler `v1` `v2` Bu üst bilgi değerini belirtebilir.

### <a name="query-string-based-versioning"></a>Sorgu dize tabanlı sürüm oluşturma

Sorgu dizesi sürüm oluşturma düzeni kullanıldığında, sürüm tanımlayıcısının herhangi bir API isteği için bir sorgu dizesi parametresine dahil olması gerekir. Sorgu dizesi parametresinin adını belirtebilirsiniz.

Sorgu dize tabanlı sürüm oluşturma kullanılırken bir API isteği URL 'sinin biçimi: `https://{yourDomain}/{apiName}/{operationId}?{queryStringParameterName}={versionIdentifier}` .

Örneğin, `https://apis.contoso.com/products?api-version=v1` ve `https://apis.contoso.com/products/api-version=v2` sürümlerini ve sırasıyla aynı API 'ye başvurabilir `products` `v1` `v2` .

## <a name="original-versions"></a>Özgün sürümler

Sürümü bulunmayan bir API 'ye sürüm eklerseniz, bir sürüm `Original` otomatik olarak oluşturulur ve bir sürüm tanımlayıcısı belirtilmeden varsayılan URL 'ye yanıt verir. `Original`Sürüm, mevcut çağıranların sürüm ekleme işlemi tarafından kesilmemesini sağlar. Başlangıçta etkinleştirilmiş sürümlerle yeni bir API oluşturursanız, bir `Original` Sürüm oluşturulmaz.

## <a name="how-versions-are-represented"></a>Sürümlerin nasıl temsil edildiği

Azure API Management, tek bir mantıksal API için bir dizi sürümü temsil eden *sürüm kümesi*adlı bir kaynağı tutar. Sürümleri yönetmek için Azure portal kullandığınızda sürüm kümesini görmezsiniz, ancak PowerShell, Kaynak Yöneticisi şablonları veya Azure Resource Manager API 'sini kullanarak API Management hizmetinize etkileşime geçerek, sürüm kümelerini doğrudan görüntüleyebilir ve yönetebilirsiniz. Sürüm kümesi, hem sürümlü API 'nin görünen adını hem de istekleri belirtilen sürümlere yönlendirmek için [kullanılan sürüm oluşturma düzenini](#versioning-schemes) içerir.

Bir API 'nin her sürümü kendi API kaynağı olarak tutulur ve bu, daha sonra bir sürüm kümesiyle ilişkilendirilir. Bir sürüm kümesi, API 'nizin sürümleri arasında önemli değişiklikler yapabileceğiniz gerçeğini yansıtan çok farklı işlemlere veya ilkelere sahip API 'Ler içerebilir.

### <a name="migrating-a-non-versioned-api-to-a-versioned-api"></a>Sürümü olmayan bir API 'yi sürümlü API 'ye geçirme

Var olan bir API üzerinde sürüm oluşturmayı etkinleştirmek için Azure portal kullandığınızda API Management kaynaklarınız için aşağıdaki değişiklikler yapılır:

 * Yeni bir sürüm kümesi oluşturulur.
 * Mevcut sürüm, [ `Original` API sürümü olarak](#original-versions)tutulur ve yapılandırılır. API, sürüm kümesine bağlıdır, ancak bir sürüm tanımlayıcısının belirtilmesini gerektirmez.
 * Yeni sürüm yeni bir API olarak oluşturulur ve sürüm kümesine bağlanır. Sürüm oluşturma düzeni ve tanımlayıcı kullanılarak bu yeni API 'ye erişilmelidir.

## <a name="versions-and-revisions"></a>Sürümler ve düzeltmeler

Sürümler ve düzeltmeler farklı özelliklerdir. Her sürüm, sürümü olmayan bir API gibi birden çok düzeltmelere sahip olabilir. Sürümler veya başka bir şekilde sürümlerini kullanmadan düzeltmeler kullanabilirsiniz. Genellikle sürümler, API sürümlerini son değişikliklerle ayırmak için kullanılır, ancak düzeltmeler bir API 'de küçük ve olmayan değişiklikler için kullanılabilir.

Düzeltmede önemli değişiklikler olduğunu bulmalı veya düzeltmeyi bir beta/test sürümüne resmi olarak dönüştürmek istiyorsanız, düzeltmeden bir sürüm oluşturabilirsiniz. Azure portal kullanarak, düzeltmeler sekmesinin düzeltme bağlamı menüsünde ' düzeltmeden sürüm oluştur ' düğmesine tıklayın.

## <a name="developer-portal"></a>Geliştirici portalı

[Geliştirici Portalı](./api-management-howto-developer-portal.md) her bir API sürümünü ayrı olarak listeler.

![Sürümlü API 'lerin bir listesini görüntüleyen geliştirici portalı API Management](media/api-management-versions/portal-list.png)

Bir API 'nin ayrıntıları Ayrıca bu API 'nin tüm sürümlerinin bir listesini gösterir. Sürüm `Original` tanımlayıcısı olmadan bir sürüm görüntülenir.

![API 'nin ayrıntılarını ve bu API 'nin sürümlerin bir listesini görüntüleyen geliştirici portalı API Management](media/api-management-versions/portal-details.png)

> [!TIP]
> API sürümlerinin, Geliştirici Portalında görülebilmesi için bir ürüne eklenmesi gerekir.
