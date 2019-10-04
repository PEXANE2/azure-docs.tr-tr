---
title: içerme dosyası
description: içerme dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 08/16/2019
ms.custom: include file
ms.openlocfilehash: 690ff402c632bfc28f1a9a218677e9772f5a3200
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949933"
---
1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve ardından **Özellikler** bölmesini açın. **DIZIN kimliğini** geçici bir dosyaya kopyalayın. Bu değeri bir sonraki bölümde örnek bir uygulama yapılandırmak için kullanacaksınız.

    [![Azure Active Directory dizin KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png#lightbox)

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve sonra **uygulama kayıtları (eski)** bölmesini açın. **Yeni uygulama kaydı** düğmesini seçin.

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Uygulama türünü** **Yerel**olarak seçin ve URI 'yi `https://microsoft.com` olarak **yeniden yönlendirin** . **Oluştur**' u seçin.

    [![ pencere oluştur](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)](./media/digital-twins-permissions-legacy/aad-app-reg-create.png#lightbox)

1.  Kayıtlı uygulamayı açın ve **uygulama kimliği** alanının değerini geçici bir dosyaya kopyalayın. Bu değer Azure Active Directory uygulamanızı tanımlar. Uygulama KIMLIĞINI aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için kullanacaksınız.

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png#lightbox)

1. Uygulama kayıt bölmenizi açın. @No__t **ayarları**-1**gerekli izinler**' i seçin ve ardından:

   a. Sol üstteki **Ekle** ' yı seçerek **API erişimi ekle** bölmesini açın.

   b. **BIR API seçin** ' i seçin ve **Azure dijital TWINS**'i arayın. Aramanız API 'YI bulamıyorsa, bunun yerine **Azure akıllı boşluklar** ' ı arayın.

   ,. **Azure dijital TWINS (Azure akıllı boşluklar hizmeti)** seçeneğini belirleyin ve **Seç**' i seçin.

   TID. **Izinleri Seç ' i**seçin. **Okuma/yazma erişimi** temsilcisi izinleri onay kutusunu seçin ve **Seç**' i seçin.

   a. **API erişimi ekle** bölmesinde **bitti** ' yi seçin.

   vadeli. **Gerekli izinler** bölmesinde, **izin ver** düğmesini seçin ve görüntülenen bildirimi kabul edin. Bu API için izin verilmezse yöneticinize başvurun.

      [![Gerekli izinler bölmesi](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png#lightbox)

 
