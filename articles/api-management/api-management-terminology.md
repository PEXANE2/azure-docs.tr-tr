---
title: Azure API Management terminolojisi | Microsoft Docs
description: Bu makale, API Management özgü koşullara yönelik tanımlar sağlar.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: 5bc76d2526c5585071a240af36b8a31e3de5708f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87024937"
---
# <a name="azure-api-management-terminology"></a>Azure API Management terminolojisi

Bu makale, API Management (APıM) için özel koşullara yönelik tanımlar sağlar.

## <a name="term-definitions"></a>Terim tanımları

* **Arka uç API 'si** -API 'nizi ve işlemlerini uygulayan bir http hizmeti. 
* **Ön uç API 'si** / **APIM API** 'si-bir APIM API API 'leri barındırmaz, bir arka uç API 'sine dokunmadan façlade 'yi gereksinimlerinize göre özelleştirmek için API 'niz için cephe 'ler oluşturur. Daha fazla bilgi için bkz. [BIR API 'Yi Içeri aktarma ve yayımlama](import-and-publish.md).
* **APIM ürünü** -bir ürün, bir veya daha fazla API ve kullanım koşulları ve kullanım koşulları içerir. Bir dizi API ekleyebilirsiniz ve bunları geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Daha fazla bilgi için bkz. [ürün oluşturma ve yayımlama](api-management-howto-add-products.md).
* **APıM API işlemi** -her APıM API 'si, geliştiriciler için kullanılabilen bir işlem kümesini temsil eder. Her APıM API 'si, API 'yi uygulayan arka uç hizmetine bir başvuru içerir ve işlemleri arka uç hizmeti tarafından uygulanan işlemlere eşlenir. Daha fazla bilgi için bkz. [sahte API yanıtları](mock-api-responses.md).
* **Sürüm** -bazı durumlarda bazı kullanıcılar için yeni veya farklı API özellikleri yayınlamak istersiniz, diğerleri ise API 'leri için geçerli olan API 'yi kontrol etmek istiyor. Daha fazla bilgi için bkz. [API 'nizin birden fazla sürümünü yayımlama](api-management-get-started-publish-versions.md).
* **Düzeltme** -API 'niz çalışmaya ve geliştiriciler tarafından kullanılmak üzere gönderilmeye başladığında, genellıkle bu API 'de değişiklik yapma ve API 'nizin çağıranlarını kesintiye uğramayan aynı zamanda bir işlem yapmanız gerekir. Yaptığınız değişiklikleri geliştiricilere bildirmeniz de yararlıdır. Daha fazla bilgi için bkz. [düzeltmeleri kullanma](api-management-get-started-revise-api.md).
* **Geliştirici Portalı** -müşterileriniz (geliştiriciler), API 'lerinize erişmek için geliştirici portalını kullanmalıdır. Geliştirici portalı özelleştirilebilir. Daha fazla bilgi için bkz. [Geliştirici Portalını Özelleştirme](api-management-customize-styles.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek oluşturma](get-started-create-service-instance.md)

