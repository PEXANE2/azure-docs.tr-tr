---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 9d394b07ff17ab24305b2580efc8bedfc5e7d384
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78183406"
---
#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulamalar**' ı seçin.
1. **Özellikler** sayfasını açmak için *webapi1* uygulamasını seçin.
1. **Yayımlanan kapsamları**seçin. Yayımlanan kapsamlar, bir istemci uygulamasına, Web API 'sine belirli izinler vermek için kullanılabilir.
1. **Kapsam**için girin `demo.read`ve **Açıklama**için girin `Read access to the web API`.
1. **Kapsam**için girin `demo.write`ve **Açıklama**için girin `Write access to the web API`.
1. **Kaydet**’i seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtları (Önizleme)** öğesini seçin.
1. **Genel bakış** sayfasını açmak için *webapi1* uygulamasını seçin.
1. **Yönet**altında **bir API 'yi kullanıma**sunma ' yı seçin.
1. **Uygulama kimliği URI 'sinin**yanındaki **Ayarla** bağlantısını seçin.
1. Varsayılan değeri (bir GUID) ile `api`değiştirin ve ardından **Kaydet**' i seçin. Tam URI gösterilir ve biçiminde `https://your-tenant-name.onmicrosoft.com/api`olmalıdır. Web uygulamanız API için bir erişim belirteci istediğinde, bu URI 'yi API için tanımladığınız her bir kapsamın ön eki olarak eklemesi gerekir.
1. **Bu API tarafından tanımlanan kapsamlar**altında **Kapsam Ekle**' yi seçin.
1. API 'ye okuma erişimini tanımlayan bir kapsam oluşturmak için aşağıdaki değerleri girin, ardından **Kapsam Ekle**' yi seçin:
    1. **Kapsam adı**:`demo.read`
    1. **Yönetici onayı görünen adı**:`Read access to demo API`
    1. **Yönetici onay açıklaması**:`Allows read access to the demo API`
1. **Kapsam Ekle**' yi SEÇIN, API 'ye yazma erişimini tanımlayan bir kapsam eklemek için aşağıdaki değerleri girin ve ardından **Kapsam Ekle**' yi seçin:
    1. **Kapsam adı**:`demo.write`
    1. **Yönetici onayı görünen adı**:`Write access to demo API`
    1. **Yönetici onay açıklaması**:`Allows write access to the demo API`