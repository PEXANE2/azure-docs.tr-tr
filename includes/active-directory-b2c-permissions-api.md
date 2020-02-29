---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 71a6654acd436c27bd2370646dede81778113860
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78186159"
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
1. **Izin Ekle**' yi seçin. Yönlendirildiğinden, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **Yönetici onayı ver ' i (kiracı adınız)** seçin.
1. Şu anda oturum açmış olan yönetici hesabınızı seçin veya Azure AD B2C kiracınızda, en azından *bulut uygulama Yöneticisi* rolüne atanan bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile**' yi seçin ve ardından "verilen..." öğesini doğrulayın Her iki kapsam için **durum** altında görünür. İzinlerin yayılması birkaç dakika sürebilir.