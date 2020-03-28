---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 9d394b07ff17ab24305b2580efc8bedfc5e7d384
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183406"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulamaları**seçin.
1. **Özellikler** sayfasını açmak için *webapi1* uygulamasını seçin.
1. **Yayımlanmış kapsamları**seçin. Yayımlanmış kapsamlar, istemci uygulamasına web API'sine belirli izinler vermek için kullanılabilir.
1. **SCOPE**için `demo.read`, girin , `Read access to the web API`ve **AÇıKLAMA**için , girin .
1. **SCOPE**için `demo.write`, girin , `Write access to the web API`ve **AÇıKLAMA**için , girin .
1. **Kaydet'i**seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtlarını seçin (Önizleme)**.
1. **Genel Bakış** sayfasını açmak için *webapi1* uygulamasını seçin.
1. **Yönet**altında, **API'yi ortaya çıkar'ı**seçin.
1. Application **ID URI'nin** **yanında, Ayarla** bağlantısını seçin.
1. Varsayılan değeri (GUID) ile `api`değiştirin ve sonra **Kaydet'i**seçin. Tam URI gösterilir ve biçiminde `https://your-tenant-name.onmicrosoft.com/api`olmalıdır. Web uygulamanız API için bir erişim belirteci istediğinde, bu URI'yi API için tanımladığınız her kapsam için öneki olarak eklemelidir.
1. **Bu API tarafından tanımlanan Kapsamlar**altında, **kapsam ekle'yi**seçin.
1. API'ye okuma erişimini tanımlayan bir kapsam oluşturmak için aşağıdaki değerleri girin ve ardından **kapsam ekle'yi**seçin:
    1. **Kapsam adı**:`demo.read`
    1. **Admin onay ekran adı**:`Read access to demo API`
    1. **Admin rıza açıklaması**:`Allows read access to the demo API`
1. **Kapsam ekle'yi**seçin, API'ye yazma erişimini tanımlayan bir kapsam eklemek için aşağıdaki değerleri girin ve ardından **kapsam ekle'yi**seçin:
    1. **Kapsam adı**:`demo.write`
    1. **Admin onay ekran adı**:`Write access to demo API`
    1. **Admin rıza açıklaması**:`Allows write access to the demo API`