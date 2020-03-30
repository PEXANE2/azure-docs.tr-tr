---
title: Azure API Yönetimi örneği için özel etki alanı adını yapılandırma
titleSuffix: Azure API Management
description: Bu konu, Azure API Yönetimi örneğiniz için özel bir etki alanı adının nasıl yapılandırılabildiğini açıklar.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 01/13/2020
ms.author: apimpm
ms.openlocfilehash: 4587909ad6fca6cdf21d54d11d89f797bbb29833
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335846"
---
# <a name="configure-a-custom-domain-name"></a>Özel bir etki alanı adı yapılandırma

Bir Azure API Yönetimi hizmeti örneği oluşturduğunuzda, Azure `azure-api.net` bu hizmete bir alt etki alanı (örneğin, `apim-service-name.azure-api.net`) atar. Ancak, **API**Yönetimi uç noktalarınızı, contoso.com gibi kendi özel etki alanı adınızı kullanarak ortaya çıkarabilirsiniz. Bu öğretici, varolan özel bir DNS adının bir API Yönetimi örneği tarafından açığa çıkarılan uç noktalarıyla nasıl eşleneceğimi gösterir.

> [!IMPORTANT]
> API Yönetimi, yalnızca [ana bilgisayar üstbilgi](https://tools.ietf.org/html/rfc2616#section-14.23) değerleri varsayılan etki alanı adı veya yapılandırılmış özel etki alanı adlarından herhangi biriyle eşleşen istekleri kabul eder.

> [!WARNING]
> Uygulamalarının güvenliğini artırmak için sertifika sabitleme kullanmak isteyen müşteriler, varsayılan sertifikayı değil, yönettikleri özel bir etki alanı adı ve sertifikası nı kullanmalıdır. Bunun yerine varsayılan sertifikayı sabitleyen müşteriler, denetledikleri sertifikanın özelliklerine sıkı bir bağımlılık uygular, bu da önerilen bir uygulama değildir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunları yapmalısınız:

-   Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   Bir API Yönetimi örneği. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)
-   Size veya kuruluşunuza ait özel bir etki alanı adı. Bu konu, özel bir etki alanı adı satın almak için nasıl talimatlar sağlamaz.
-   Özel etki alanı adını API Yönetimi örneğinizin varsayılan etki alanı adıyla eşleyen bir DNS sunucusunda barındırılan bir CNAME kaydı. Bu konu, cname kaydının nasıl barındırılalacıla açıklaşacağına ilişkin yönergeler sağlamaz.
-   Ortak ve özel anahtara sahip geçerli bir sertifikanız olmalıdır (. PFX). Konu veya konu alternatif adı (SAN) etki alanı adı eşleşmiştir (bu TLS üzerinden URL'leri güvenli bir şekilde ortaya çıkarmak için API Yönetimi örneğini sağlar).

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Özel bir etki alanı adı ayarlamak için Azure portalını kullanma

1. [Azure portalındaki](https://portal.azure.com/)API Yönetimi örneğinize gidin.
1. **Özel etki alanlarını**seçin.

    Özel bir etki alanı adı atayabileceğiniz birkaç uç nokta vardır. Şu anda, aşağıdaki uç noktalar kullanılabilir:

    - **Ağ Geçidi** (varsayılan: `<apim-service-name>.azure-api.net`),
    - **Portal** (varsayılan: `<apim-service-name>.portal.azure-api.net`),
    - **Yönetim** (varsayılan: `<apim-service-name>.management.azure-api.net`),
    - **SCM** (varsayılan: `<apim-service-name>.scm.azure-api.net`),
    - **NewPortal** (varsayılan: `<apim-service-name>.developer.azure-api.net`).

    > [!NOTE]
    > Tüketim katmanında yapılandırma için yalnızca **Ağ Geçidi** bitiş noktası kullanılabilir.
    > Tüm uç noktaları veya bazılarını güncelleştirebilirsiniz. Genellikle, müşteriler **Ağ Geçidi'ni** günceller (bu URL API Yönetimi ve **Portal** (geliştirici portalı URL'si) aracılığıyla açığa çıkarılan API'yi aramak için kullanılır.
    > **Yönetim** ve **SCM** uç noktaları yalnızca API Yönetimi örnek sahipleri tarafından dahili olarak kullanılır ve bu nedenle daha az sıklıkta özel bir etki alanı adı atanır.
    > **Premium** katman, **Ağ Geçidi** bitiş noktası için birden çok ana bilgisayar adı ayarlamayı destekler.

1. Güncelleştirmek istediğiniz bitiş noktasını seçin.
1. Sağdaki pencerede **Özel'i**tıklatın.

    - Özel **alan adı,** kullanmak istediğiniz adı belirtin. Örneğin, `api.contoso.com`.
    - **Sertifika'da,** Key Vault'tan bir sertifika seçin. Ayrıca geçerli bir yük yükleyebilirsiniz. PFX dosya ve **şifresini**sağlamak , sertifika bir şifre ile korunuyorsa.

    > [!NOTE]
    > Joker karakter alan adları, `*.contoso.com` örneğin Tüketim katmanı dışındaki tüm katmanlarda desteklenir.

    > [!TIP]
    > Sertifikaları yönetmek ve otomatik döndürme için ayarlamak için Azure Key Vault'u kullanmanızı öneririz.
    > Özel etki alanı TLS/SSL sertifikasını yönetmek için Azure Key Vault kullanıyorsanız, sertifikanın _bir sır_olarak değil, anahtar kasasına [ _sertifika_olarak](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate)ekildiğinden emin olun.
    >
    > TLS/SSL sertifikası almak için API Yönetimi'nin listeye sahip olması ve sertifikayı içeren Azure Anahtar Kasası'nda sırlar izinleri alması gerekir. Azure portalı kullanırken gerekli tüm yapılandırma adımları otomatik olarak tamamlanır. Komut satırı araçları veya yönetim API'si kullanırken, bu izinler el ile verilmelidir. Bu iki adımda yapılır. İlk olarak, Yönetilen Kimlik etkin olduğundan emin olmak ve bu sayfada gösterilen asıl kimliği not almak için API Yönetimi örneğinizdeki Yönetilen kimlikler sayfasını kullanın. İkinci olarak, sertifikayı içeren Azure Anahtar Kasası'ndaki bu ana kimlik için izin listesi verin ve gizli izinler alın.
    >
    > Sertifika otomatik olarak ayarlanırsa, API Yönetimi hizmette herhangi bir kesinti olmaksızın en son sürümü otomatik olarak alır (API Yönetimi katmanınızda SLA varsa - yani Geliştirici katmanı hariç tüm katmanlarda).

1. Uygula'yı tıklatın.

    > [!NOTE]
    > Sertifikaatama işlemi, dağıtımBoyutuna bağlı olarak 15 dakika veya daha uzun sürebilir. Geliştirici SKU kapalı kalma süresi vardır, Temel ve daha yüksek SKU'lar kapalı kalma süresi yok.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="dns-configuration"></a>DNS yapılandırması

Özel etki alanı adınız için DNS yapılandırırken iki seçeneğiniz vardır:

-   Yapılandırılmış özel etki alanı adınızın bitiş noktasına işaret eden bir CNAME kaydı yapılandırın.
-   API Yönetimi ağ geçidi IP adresinizi işaret eden bir A kaydı yapılandırın.

> [!NOTE]
> API Managment instance IP adresi statik olsa da, birkaç senaryoda değişebilir. Bu nedenle özel etki alanı yapılandırırken CNAME kullanılması önerilir. DNS yapılandırma yöntemini seçerken bunu göz önünde bulundurun. [IP dokümantasyon makalesi](api-management-howto-ip-addresses.md#changes-to-the-ip-addresses) nde ve [API Yönetimi SSS'sinde](api-management-faq.md#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmetinizi yükseltin ve ölçeklendirin](upgrade-and-scale.md)
