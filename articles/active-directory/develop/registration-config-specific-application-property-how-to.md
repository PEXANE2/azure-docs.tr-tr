---
title: Özel olarak geliştirilen uygulamalar için Azure portalı kayıt alanları
description: Azure AD ile özel olarak geliştirilmiş bir uygulama kaydetme kılavuzu
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.openlocfilehash: c44575ca43063388d5c65855542cf15700d2cb5a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883177"
---
# <a name="azure-portal-registration-fields-for-custom-developed-apps"></a>Özel olarak geliştirilen uygulamalar için Azure portalı kayıt alanları

Bu makalede, [Azure portalındaki](https://portal.azure.com)uygulama kayıt formundaki tüm kullanılabilir alanların kısa bir açıklaması sunulmaktadır.

## <a name="register-a-new-application"></a>Yeni uygulama kaydetme

-   Yeni bir uygulama kaydetmek için [Azure portalına](https://portal.azure.com)gidin.

-   Sol gezinti bölmesinden **Azure Etkin Dizini'ni tıklatın.**

-   **Uygulama kayıtlarını** seçin ve **Ekle'yi**tıklatın.

-   Bu, başvuru kayıt formunu açar.

## <a name="fields-in-the-application-registration-form"></a>Başvuru kayıt formundaki alanlar

| Alan            | Açıklama                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Adı             | Uygulamanın adı. En az dört karakter olmalıdır.                |
| Desteklenen hesap türleri| Uygulamanızın hangi hesapları desteklemesini istediğinizi seçin: yalnızca bu kuruluş dizinindeki hesaplar, herhangi bir kuruluş dizinindeki hesaplar veya herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesaplarındaki hesaplar.  |
| URI'yi yeniden yönlendirme (isteğe bağlı) | Oluşturmakta olduğunuz uygulama türünü, **Web** veya **Kamu istemcisini (mobil & masaüstü)** seçin ve ardından uygulamanız için yeniden yönlendirme URI'sini (veya yanıt URL'sini) girin. Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin http://localhost:31544 yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır. Genel istemci uygulamaları için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. myapp://auth gibi uygulamanıza özgü bir değer girin. Web uygulamaları veya yerel uygulamalar için belirli örnekleri görmek için [hızlı başlangıçlarımıza](https://docs.microsoft.com/azure/active-directory/develop)göz atın.|

Yukarıdaki alanları doldurduktan sonra, uygulama Azure portalına kaydedilir ve uygulamaya genel bakış sayfasına yönlendirilirsiniz. **Yönet'in** altındaki sol bölmedeki ayarlar sayfalarında uygulamanızı özelleştirmeniz için daha fazla alan vardır. Aşağıdaki tablolar tüm alanları açıklar. Bir web uygulaması veya ortak istemci uygulaması oluşturup oluşturmadığınıza bağlı olarak, bu alanların yalnızca bir alt kümesini görürsünüz.

### <a name="overview"></a>Genel Bakış

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uygulama Kimliği  | Bir uygulamayı kaydettirdiğinizde, Azure AD uygulamanıza bir Uygulama Kimliği atar. Uygulama kimliği, Azure AD'ye yapılan kimlik doğrulama isteklerinde uygulamanızı benzersiz olarak tanımlamak ve Grafik API gibi kaynaklara erişmek için kullanılabilir.                                                          |
| Uygulama Kimliği URI'si      | Bu benzersiz bir URI, genellikle **form&lt;\_https://&gt;/&lt;\_kiracı&gt;adı uygulama adı olmalıdır.** Bu, belirteç için verilmesi gereken kaynağı belirtmek için benzersiz bir tanımlayıcı olarak yetkilendirme hibe akışı sırasında kullanılır. Ayrıca verilen erişim belirteci 'aud' iddia olur. |

### <a name="branding"></a>Markalama

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yeni logo yükleyin | Bunu, uygulamanız için bir logo yüklemek için kullanabilirsiniz. Logo .bmp, .jpg veya .png formatında olmalı ve dosya boyutu 100 KB'den az olmalıdır. Görüntünün boyutları 215x215 piksel olmalı ve merkezi görüntü boyutları 94x94 piksel olmalıdır.|
| Ana sayfa URL'si   | Bu, başvuru kaydı sırasında belirtilen oturum açma URL'sidir.|

### <a name="authentication"></a>Kimlik Doğrulaması

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Giriş URL'si      | Bu, tek oturum açma giriş URL'sidir. Azure AD, kullanıcı azure AD ile oturumunu diğer kayıtlı uygulamaları kullanarak temizlediğinde bu URL'ye bir oturum açma isteği gönderir.|
| Desteklenen hesap türleri  | Bu anahtar, uygulamanın birden çok kiracı tarafından kullanılıp kullanılamayacağını belirtir. Genellikle, bu, dış kuruluşların uygulamanızı kiracılarına kaydederek ve kuruluşlarının verilerine erişim sağlayarak kullanabileceği anlamına gelir.|
| Yeniden Yönlendirme URL'leri      | Yönlendirme veya yanıt, URL'ler Azure AD'nin uygulamanızın istediği belirteçleri döndürdüğü uç noktalardır. Yerel uygulamalar için, kullanıcı nın başarılı yetkilendirmeden sonra gönderildiği yerdir. Azure AD, OAuth 2.0 isteğindeki UYGULAMA SARF MALZEMELERİnE YENIDEN YÖNLENDIRME UYGULAMA SİsteMÜNÜN portaldaki kayıtlı değerlerden biriyle eşleşin.|

### <a name="certificates-and-secrets"></a>Sertifikalar ve sırlar

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İstemci gizli dizileri            | Azure AD tarafından güvenli web API'lerine kullanıcı etkileşimi olmadan programlı bir şekilde erişmek için istemci sırları veya anahtarlar oluşturabilirsiniz. Yeni **istemci gizli** sayfasından, anahtar açıklamasını ve son kullanma tarihini girin ve anahtarı oluşturmak için kaydedin. Daha sonra erişemeyeceğinden güvenli bir yere kaydettiğinizi unutmayın.             |

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamaları Azure Active Directory ile yönetme](../manage-apps/what-is-application-management.md)
