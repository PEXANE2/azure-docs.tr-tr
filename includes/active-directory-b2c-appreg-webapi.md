---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 9367719b1a1050ce8abf36804b3d0d0d58c7484b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183404"
---
Web API kaynaklarının, erişim jetonu sunan istemci uygulamalarının korumalı kaynak isteklerini kabul edip yanıtlamadan önce kiracınızda kaydedilmesi gerekir.

Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
1. Uygulama için bir ad girin. Örneğin, *webapi1*.
1. **Web Uygulaması / Web API için** **Evet'i**seçin.
1. **Örtük akışa izin vermek**için **Evet'i**seçin.
1. **YanıtURL'si için,** Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Bu öğreticide, örnek yerel olarak çalışır `https://localhost:5000`ve 'de dinler.
1. **App ID URI**için, gösterilen URI'ye bir API uç nokta tanımlayıcısı ekleyin. Bu öğretici için, `api` `https://contosob2c.onmicrosoft.com/api`girin , böylece tam URI benzer .
1. **Oluştur'u**seçin.
1. Uygulama **kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *webapi1*.
1. **Uri'yi Yeniden Yönlendirme**altında **Web'i**seçin ve ardından Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Bu öğreticide, örnek yerel olarak çalışır `http://localhost:5000`ve 'de dinler.
1. **Kaydol**’u seçin.
1. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

Ardından, örtülü hibe akışını etkinleştirin:

1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Örtülü hibe**altında, hem **Erişim belirteçleri** hem de **kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet'i**seçin.