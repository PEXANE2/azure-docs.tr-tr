---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: 0ab9302909f86ff4559ece217f8d702710733691
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75368019"
---
Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için, geçerli **uygulamalar** deneyimini veya yeni Birleşik **uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Uygulamalar](#tab/applications/)

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure Active Directory**seçin.
1. **Yönet**altında **uygulama kayıtları (eski)** seçeneğini belirleyin.
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulama için bir ad girin. Örneğin, *managementapp1*.
1. **Uygulama türü**için **Web uygulaması/API**' yi seçin.
1. **Oturum açma URL**'sinde GEÇERLI bir URL girin. Örneğin, `https://localhost`. Uç noktanın erişilebilir olması gerekmez, ancak geçerli bir URL olmalıdır.
1. **Oluştur**’u seçin.
1. **Kayıtlı uygulamaya** Genel Bakış sayfasında görüntülenen **uygulama kimliğini** kaydedin. Bu değeri sonraki bir adımda kullanırsınız.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *managementapp1*.
1. **Yalnızca bu kuruluş dizininde hesaplar '** ı seçin.
1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu temizleyin.
1. **Kaydol**’u seçin.
1. Uygulamaya Genel Bakış sayfasında görüntülenen **uygulama (istemci) kimliğini** kaydedin. Bu değeri sonraki bir adımda kullanırsınız.