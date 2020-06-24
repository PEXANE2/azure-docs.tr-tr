---
title: Microsoft ticari Market 'te SaaS 'yi karşılama API 'Leri
description: SaaS tekliflerinizi Microsoft AppSource ve Azure Market 'te tümleştirmenizi sağlayan API 'Leri karşılama API 'Lerine giriş.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: dsindona
ms.openlocfilehash: 70515ca04e870fa435f8e9f46122a8e0dcb9b588
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84691365"
---
# <a name="saas-fulfillment-apis-in-microsoft-commercial-marketplace"></a>Microsoft ticari Market 'te SaaS 'yi karşılama API 'Leri

SaaS 'yi karşılama API 'Leri, bağımsız yazılım satıcıları (ISV 'Ler) olarak da bilinen yayımcıları etkinleştirerek, SaaS uygulamalarını Microsoft AppSource, Azure Marketi 'nde ve Azure portal yayımlamak ve satmak için etkinleştirir. Bu API 'Ler ISV uygulamalarının tüm ticari etkin kanallara katılmasını sağlar: doğrudan, iş ortağı LED (satıcı) ve alan-LED.  Bu API 'lerle tümleştirme, Iş Ortağı Merkezi 'nde bir transactable SaaS teklifi oluşturma ve yayımlama gereksinimidir.

ISV 'ler, hem ISV hem de Microsoft için aynı abonelik durumunu korumak üzere SaaS hizmet koduna ekleyerek aşağıdaki API akışlarını uygulamalıdır:

* Giriş sayfası akışı: Microsoft, yayımcıya yayımcının SaaS teklifinin Market 'teki bir müşteri tarafından satın alındığını bildirir.
* Etkinleştirme akışı: Yayımcı, Microsoft 'un yayımcının tarafında yeni satın alınan bir SaaS hesabının yapılandırıldığını bildirir.
* Güncelleştirme akışı: satın alınan planın ve/veya satın alınan bilgisayar sayısının değişikliği.
* Askıya alma ve yeniden kullanım akışı: müşterinin ödeme yönteminin artık geçerli olmaması durumunda satın alınan SaaS teklifini askıya alma. Ödeme yöntemiyle ilgili sorun çözüldüğünde, askıya alınmış teklif tekrar belirtilebilir.
* Web kancası akışları: Microsoft, yayımcıya Microsoft SIDE 'den müşteri tarafından tetiklenen SaaS Abonelik değişiklikleri ve iptali hakkında bilgilendirir.

Satın alınan SaaS Aboneliğinin iptali için, Microsoft SIDE 'den müşteri tarafından yapılabildiğinden tümleştirme isteğe bağlıdır.

SaaS karşılama API 'Leriyle doğru tümleştirme, aşağıdakileri sağlamak için önemlidir

* Yayımcının SaaS teklifini satın alan son müşteriler Microsoft tarafından doğru şekilde faturalandırılır.
* Son müşteriler Market 'te satın alınan SaaS aboneliklerini satın alma, yapılandırma, kullanma ve yönetme konusunda doğru Kullanıcı deneyimini alıyor.

Bu API 'Ler yayımcının tüm ticari özellikli kanallara katılmasını sağlar:

* DirectSound
* iş ortağı-LED 'i (satıcı, CSP)
* alan-LED

Satıcı (CSP) senaryosunda bir CSP, SaaS teklifini son müşteri adına satın alır. Müşterinin SaaS teklifini kullanması beklenmektedir, ancak CSP şunları yapan varlıktır:

* müşteriyi faturalama
* abonelik planlarını değiştirme/satın alınan bilgisayar miktarı
* abonelikleri iptal etme

Bu senaryo için yayımcı, API çağrısı akışlarını farklı bir şekilde uygulamak için gerekli değildir.

CSP hakkında daha fazla bilgi için lütfen adresine bakın https://partner.microsoft.com/en-us/licensing .

>[!Warning]
>Bu API 'nin geçerli sürümü, tüm yeni SaaS teklifleri için kullanılması gereken sürüm 2 ' dir. API sürüm 1 kullanım dışıdır ve mevcut teklifleri desteklemek için tutulmaktadır.

>[!Note]
>SaaS karşılama API 'Lerinin yalnızca yayımcının arka uç hizmetinden çağrılması amaçlanmıştır. Doğrudan yayımcının Web sayfasından API 'lerle tümleştirme desteklenmez. Yalnızca hizmetten hizmete kimlik doğrulama akışı kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, SaaS uygulamanızı [Azure AD uygulaması kaydetme](./pc-saas-registration.md)bölümünde açıklandığı gibi [Azure Portal](https://ms.portal.azure.com) kaydedin.  Daha sonra, geliştirme için bu arabirimin en güncel sürümünü kullanın: [SaaS API sürüm 2](./pc-saas-fulfillment-api-v2.md)' yi karşılama.
