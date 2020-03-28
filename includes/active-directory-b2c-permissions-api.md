---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 71a6654acd436c27bd2370646dede81778113860
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186159"
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
1. **İzin Ekle'yi**seçin. Yönlendirildikçe, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **(kiracı adınız) için Grant yönetici onayı**seçin.
1. Şu anda oturum açmış yönetici hesabınızı seçin veya En azından *Bulut uygulama yöneticisi* rolüne atanmış Olan Azure AD B2C kiracınızda bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile'yi**seçin ve ardından "Verilenler..." her iki kapsam için **durum** altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.