---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 10/03/2019
ms.custom: include file
ms.openlocfilehash: 7e0396c032a9f3dc26b82648604624446d6ad191
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71978612"
---
## <a name="azure-active-directory-legacy-registration"></a>Azure Active Directory eski kayıt

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve ardından **Özellikler** bölmesini açın. **Dizin Kimliğini** geçici bir dosyaya kopyalayın. Bu değeri bir sonraki bölümde örnek bir uygulama yapılandırmak için kullanacaksınız.

    [![Azure Active Directory dizin KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png#lightbox)

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve sonra **uygulama kayıtları (eski)** bölmesini açın. **Yeni uygulama kaydı** düğmesini seçin.

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Uygulama türünü** **Yerel**olarak seçin ve URI 'yi `https://microsoft.com` olarak **yeniden yönlendirin** . **Oluştur**'u seçin.

    [![ pencere oluştur](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)](./media/digital-twins-permissions-legacy/aad-app-reg-create.png#lightbox)

1.  Kayıtlı uygulamayı açın ve **uygulama kimliği** alanının değerini geçici bir dosyaya kopyalayın. Bu değer Azure Active Directory uygulamanızı tanımlar. Uygulama KIMLIĞINI aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için kullanacaksınız.

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png#lightbox)

1. Uygulama kayıt bölmenizi açın. @No__t **ayarları**-1**gerekli izinler**' i seçin ve ardından:

   a. Sol üstteki **Ekle** ' yı seçerek **API erişimi ekle** bölmesini açın.

   b. **BIR API seçin** ' i seçin ve **Azure dijital TWINS**'i arayın. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın.

   c. **Azure dijital TWINS (Azure akıllı boşluklar hizmeti)** seçeneğini belirleyin ve **Seç**' i seçin.

   d. **Izinleri Seç ' i**seçin. **Okuma/yazma erişimi** temsilcisi izinleri onay kutusunu seçin ve **Seç**' i seçin.

   e. **API erişimi ekle** bölmesinde **bitti** ' yi seçin.

   f. **Gerekli izinler** bölmesinde, **izin ver** düğmesini seçin ve görüntülenen bildirimi kabul edin. Bu API için izin verilmezse yöneticinize başvurun.

      [![Gerekli izinler bölmesi](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png#lightbox)

 
