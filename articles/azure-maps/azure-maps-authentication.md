---
title: Kimlik doğrulama yöntemleri | Microsoft Azure Haritaları
description: Bu makalede, Azure Etkin Dizin (Azure AD) ve Paylaşılan Anahtar kimlik doğrulaması hakkında bilgi edineceksiniz. Her ikisi de Microsoft Azure Haritalar hizmetleri için kullanılır. Azure Haritalar abonelik anahtarını nasıl alacağınızı öğrenin.
author: philmea
ms.author: philmea
ms.date: 01/28/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 21d29cba85adfc147ec9deb6ab362a5da943bf10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335697"
---
# <a name="authentication-with-azure-maps"></a>Azure Haritalar ile kimlik doğrulaması

Azure Haritalar istekleri doğrulamanın iki yolunu destekler: Paylaşılan Anahtar kimlik doğrulaması ve Azure Etkin Dizin kimlik doğrulaması. Bu makalede, Azure Haritalar hizmetlerini uygulamanıza rehberlik etmek için bu kimlik doğrulama yöntemleri açıklanmaktadır.

> [!NOTE]
> Azure Haritalar ile güvenli iletişimi geliştirmek için artık Aktarım Katmanı Güvenliği (TLS) 1.2'yi destekliyoruz ve TLS 1.0 ve 1.1 desteğini geri çekiliyoruz. Herhangi bir hizmet kesintisini önlemek **için sunucularınızı ve uygulamalarınızı 2 Nisan 2020'den önce TLS 1.2'yi kullanacak şekilde güncelleyin.**  Şu anda TLS 1.x kullanıyorsanız, TLS 1.2 hazır lığınızı değerlendirin ve [TLS 1.0 Problemini Çözme'de](https://docs.microsoft.com/security/solving-tls1-problem)açıklanan testle bir geçiş planı geliştirin.

## <a name="shared-key-authentication"></a>Paylaşılan Anahtar kimlik doğrulaması

 Azure Haritalar hesabı oluşturulduktan sonra birincil ve ikincil anahtarlar oluşturulur. Azure Haritalar'ı ortak anahtar kimlik doğrulamasını kullanarak ararken birincil anahtarı abonelik anahtarı olarak kullanmanız için teşvik esiniz. Paylaşılan Anahtar kimlik doğrulaması, Bir Azure Haritalar hesabı tarafından oluşturulan bir anahtarı bir Azure Haritalar hizmetine geçirir. Azure Haritalar hizmetlerine yapılan her istek *için, abonelik anahtarını* URL'ye parametre olarak ekleyin. İkincil anahtar, anahtar değişiklikleri yuvarlama gibi senaryolarda kullanılabilir.  

Anahtarlarınızı Azure portalında görüntüleme hakkında bilgi [için](https://aka.ms/amauthdetails)bkz.

> [!Tip]
> Anahtarlarınızı düzenli olarak yenilemenizi öneririz. Diğerini yenilerken bir anahtarla bağlantınızı koruyabilmeniz için size iki anahtar sağlanır. Anahtarlarınızı yeniden oluşturduğunuzda, hesabınıza erişen tüm uygulamaları yeni anahtarlarla güncelleştirmeniz gerekir.

## <a name="authentication-with-azure-active-directory-preview"></a>Azure Etkin Dizinile Kimlik Doğrulama (Önizleme)

Azure Haritalar artık [Azure Etkin Dizin (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)kullanarak Azure Haritalar hizmetleri için istek kimlik doğrulaması sunuyor. Azure AD, rol tabanlı erişim [denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview)dahil olmak üzere kimlik tabanlı kimlik doğrulaması sağlar. RBAC, Azure Haritalar kaynaklarına kullanıcı düzeyinde, grup düzeyinde veya uygulama düzeyinde erişim sağlamak için kullanılır. Sonraki bölümlerde Azure Haritalar tümleştirmesinin kavramları ve bileşenleri Azure AD ile tartışılır.

## <a name="authentication-with-oauth-access-tokens"></a>OAuth erişim belirteçleri ile kimlik doğrulama

Azure Haritalar, Azure Haritalar hesabı içeren bir Azure aboneliğiyle ilişkili Azure AD kiracıları için **OAuth 2.0** erişim belirteçlerini kabul eder. Azure Haritalar, şu nedenlerle belirteçleri de kabul eder:

* Azure AD kullanıcıları
* Kullanıcılar tarafından yetkialan izinleri kullanan iş ortağı uygulamaları
* Azure kaynakları için yönetilen kimlikler

Azure Haritalar, her Azure Haritalar hesabı için benzersiz bir *tanımlayıcı (istemci kimliği)* oluşturur. Bu istemci kimliğini ek parametrelerle birleştirdiğinizde Azure AD'den belirteçler isteyebilirsiniz. Belirteç istemek için, Azure Ortamınızı temel alan aşağıdaki tablodaki değerleri belirtin.

| Azure Ortamı   | Azure AD belirteç bitiş noktası |
| --------------------|-------------------------|
| Azure Genel        | https://login.microsoftonline.com |
| Azure Kamu    | https://login.microsoftonline.us |


Azure AD'yi yapılandırma ve Azure Haritalar için belirteç isteği hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamasını yönet'e](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)bakın.

Azure AD'den belirteç leri isteme hakkında genel bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)bkz.

## <a name="request-azure-map-resources-with-oauth-tokens"></a>OAuth belirteçleri ile Azure Haritası kaynaklarını isteyin

Azure AD bir belirteç aldıktan sonra, Azure Haritalar gerekli istek üstbilgilerini içeren bir istek gönderir:

| İstek üst bilgisi    |    Değer    |
|:------------------|:------------|
| x-ms-istemci-id    | 30d7cc....9f55|
| Yetkilendirme     | Taşıyıcı eyJ0e....HNIVN |

> [!Note]
> `x-ms-client-id`Azure Haritalar kimlik doğrulama sayfasında görünen Azure Haritalar hesap tabanlı GUID'dir.

Aşağıda, OAuth belirteci kullanan bir Azure Haritalar rota isteğine bir örnek verilmiştir:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

İstemci kimliğinizi görüntüleme hakkında daha fazla bilgi için [bkz.](https://aka.ms/amauthdetails)

## <a name="control-access-with-rbac"></a>RBAC ile erişimi kontrol edin

Azure AD'de, güvenli kaynaklara erişimi denetlemek için RBAC'ı kullanın. Azure Haritalar hesabınızı ayarlayın ve Azure Haritalar Azure AD Kiracınızı kaydedin. Azure Haritalar, Azure kaynakları için yönetilen kimlikler aracılığıyla tek tek Azure REKLAM kullanıcıları, gruplar, uygulamalar, Azure kaynakları ve Azure hizmetleri için okuma erişim denetimini destekler. Azure Haritalar portalı sayfasında, seçtiğiniz roller için RBAC'ı ayarlayabilirsiniz.

![Azure Haritalar Veri Okuyucu (Önizleme)](./media/azure-maps-authentication/concept.png)

RBAC ayarlarınızı görüntüleme hakkında daha fazla bilgi için [Azure Haritalar için RBAC'ı nasıl yapılandırabilirsiniz'](https://aka.ms/amrbac)a bakın.

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Azure kaynakları ve Azure Haritaları için yönetilen kimlikler

[Azure kaynakları için yönetilen kimlikler,](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) Azure hizmetlerine Azure Haritalar hizmetlerine erişmeye yetkili olabilecek otomatik olarak yönetilen bir kimlik sağlar. Yönetilen kimliklere örnek olarak şunlar verilebilir: Azure Uygulama Hizmeti, Azure İşlevleri ve Azure Sanal Makineleri.

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD ve Azure Haritalar ile bir uygulamanın kimlik doğrulamasını doğrulama hakkında daha fazla bilgi edinmek için azure [haritalarda kimlik doğrulamasını yönet'e](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)bakın.

* Azure Haritalar Harita Denetimi ve Azure AD'nin kimliğini doğrulama hakkında daha fazla bilgi edinmek için [bkz.](https://aka.ms/amaadmc)
