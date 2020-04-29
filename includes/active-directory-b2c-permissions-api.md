---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/04/2020
ms.author: mimart
ms.openlocfilehash: ee63c1c0913baf3700d77e6fba1582fe7027ead7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80875713"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulamalar**' ı seçin ve ardından API erişimi olması gereken Web uygulamasını seçin. Örneğin, *WebApp1*.
1. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
1. **API Seç** açılan menüsünde, Web uygulamasına erişim IZNI verilecek API 'yi seçin. Örneğin, *webapi1*.
1. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız kapsamları seçin. Örneğin, *demo. Read* ve *demo. Write*.
1. **Tamam**’ı seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından API 'ye erişimi olması gereken Web uygulamasını seçin. Örneğin, *WebApp1*.
1. **Yönet**altında **API izinleri**' ni seçin.
1. **Yapılandırılan izinler**altında **izin Ekle**' yi seçin.
1. **API 'Lerim** sekmesini seçin.
1. Web uygulamasına erişim verilmesi gereken API 'yi seçin. Örneğin, *webapi1*.
1. **İzin**altında **demo**' i genişletin ve daha önce tanımladığınız kapsamları seçin. Örneğin, *demo. Read* ve *demo. Write*.
1. **Izin Ekle**' yi seçin.
1. **Yönetici onayı ver ' i (kiracı adınız)** seçin.
1. Bir hesap seçmeniz istenirse, şu anda oturum açmış olan yönetici hesabınızı seçin veya Azure AD B2C kiracınızda en azından *bulut uygulama Yöneticisi* rolüne atanan bir hesapla oturum açın.
1. **Evet**' i seçin.
1. **Yenile**' yi seçin ve ardından "verilen..." öğesini doğrulayın Her iki kapsam için **durum** altında görünür.
