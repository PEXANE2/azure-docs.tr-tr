---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/04/2020
ms.author: mimart
ms.openlocfilehash: ee63c1c0913baf3700d77e6fba1582fe7027ead7
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875713"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulamalar'ı**seçin ve ardından API'ye erişimi olması gereken web uygulamasını seçin. Örneğin, *webapp1*.
1. **API erişimini**seçin ve sonra **Ekle'yi**seçin.
1. SELECT **API** açılır sayfasında, web uygulamasına erişim izni verilmesi gereken API'yi seçin. Örneğin, *webapi1*.
1. **Scopes** açılır günlerini seç'te, daha önce tanımladığınız kapsamları seçin. Örneğin, *demo.read* ve *demo.write*.
1. **Tamam'ı**seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından API'ye erişimi olması gereken web uygulamasını seçin. Örneğin, *webapp1*.
1. **Yönet**altında, **API izinlerini**seçin.
1. **Yapılandırılmış izinler**altında, **İzin ekle'yi**seçin.
1. **API'lerim** sekmesini seçin.
1. Web uygulamasına erişim izni verilmesi gereken API'yi seçin. Örneğin, *webapi1*.
1. **İzin** **altında, demoyu**genişletin ve daha önce tanımladığınız kapsamları seçin. Örneğin, *demo.read* ve *demo.write*.
1. **İzin Ekle'yi**seçin.
1. **(kiracı adınız) için Grant yönetici onayı**seçin.
1. Bir hesap seçmeniz istenirse, şu anda oturum açmış yönetici hesabınızı seçin veya Azure AD B2C kiracınızda en azından *Bulut uygulama yöneticisi* rolüne atanmış bir hesapla oturum açın.
1. **Evet'i**seçin.
1. **Yenile'yi**seçin ve ardından "Verilenler..." her iki kapsam için **durum** altında görüntülenir.
