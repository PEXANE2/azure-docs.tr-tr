---
title: include dosyası
description: include dosyası
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/16/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: ef6b395aeff18a63f52f58e2477679b48a19b002
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624766"
---
1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve ardından **Özellikler** bölmesini açın. **Dizin Kimliğini** geçici bir dosyaya kopyalayın. Bu değeri bir sonraki bölümde örnek bir uygulama yapılandırmak için kullanacaksınız.

    ![Azure Active Directory Directory KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve sonra **uygulama kayıtları (eski)** bölmesini açın. **Yeni uygulama kaydı** düğmesini seçin.

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Uygulama türünü** **Yerel**olarak seçin ve URI 'yi olarak `https://microsoft.com` **yeniden yönlendirin** . **Oluştur**’u seçin.

    ![Bölme oluştur](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  Kayıtlı uygulamayı açın ve **uygulama kimliği** alanının değerini geçici bir dosyaya kopyalayın. Bu değer Azure Active Directory uygulamanızı tanımlar. Uygulama KIMLIĞINI aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için kullanacaksınız.

    ![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. Uygulama kayıt bölmenizi açın. **Ayarlar** > **gerekli izinler**' i seçin ve ardından:

   a. Sol üstteki **Ekle** ' yı seçerek **API erişimi ekle** bölmesini açın.

   b. **BIR API seçin** ' i seçin ve **Azure dijital TWINS**'i arayın. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın.

   c. **Azure dijital TWINS (Azure akıllı boşluklar hizmeti)** seçeneğini belirleyin ve **Seç**' i seçin.

   d. **Izinleri Seç ' i**seçin. **Okuma/yazma erişimi** temsilcisi izinleri onay kutusunu seçin ve **Seç**' i seçin.

   e. **API erişimi ekle** bölmesinde **bitti** ' yi seçin.

   f. **Gerekli izinler** bölmesinde, **izin ver** düğmesini seçin ve görüntülenen bildirimi kabul edin. Bu API için izin verilmezse yöneticinize başvurun.

      ![Gerekli izinler bölmesi](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 
