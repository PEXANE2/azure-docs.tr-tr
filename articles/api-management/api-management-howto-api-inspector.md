---
title: Azure API Yönetimi’nde istek izlemeyi kullanarak API’lerinizdeki hataları ayıklama | Microsoft Docs
description: Azure API Yönetimi’nde istek işleme adımlarını inceleme hakkında bilgi edinmek için bu öğreticinin adımlarını izleyin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: fc5e8c7a7aa0d4693d96c3405ec0e180a6d13f8e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75768545"
---
# <a name="debug-your-apis-using-request-tracing"></a>İstek izleme özelliğini kullanarak API’lerinizdeki hataları ayıklama

Bu öğreticide, API’nizdeki hataları ayıklamanıza ve sorunları gidermenize yardımcı olması için istek işlemenin nasıl inceleneceği açıklanmaktadır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Çağrı izleme

![API denetçisi](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ [Azure API Management terminolojisini](api-management-terminology.md) öğrenin.
+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

## <a name="trace-a-call"></a>Çağrı izleme

![API izleme](media/api-management-howto-api-inspector/06-DebugYourAPIs-01-TraceCall.png)

1. **API’ler** seçeneğini belirleyin.
2. API listenizden **Tanıtım Konferansı API’sine** tıklayın.
3. **Test** sekmesine geçin.
4. **GetSpeakers** işlemini seçin.
5. Değer **true** olarak ayarlanmış şekilde, **Ocp-Apim-Trace** adlı bir HTTP üst bilgisini eklediğinizden emin olun.

   > [!NOTE]
   > * Ocp-Apim-Subscription-Key otomatik olarak doldurulmazsa, Geliştirici Portalına gidip profil sayfasında anahtarları görüntüleyerek bu değeri alabilirsiniz.
   > * Ocp-Apim-Trace HTTP üstbilgisi kullanıldığında izleme almak için abonelik anahtarının **izleme** izin ayarının etkinleştirilmesi gerekir. İzin ve **resul asyon** ayarını yapılandırmak için, sol menüdeki **API Yönetimi** altında **Abonelikler'i**seçin.
   >   ![API Yönetim Abonelikleri bölmesinde izleme izni verme](media/api-management-howto-api-inspector/allowtracing.png)

6. API çağrısı yapmak için **Gönder'i** tıklatın. 
7. Çağrının tamamlanmasını bekleyin. 
8. **API konsolu**’nda **İzleme** sekmesine gidin. Ayrıntılı izleme bilgilerine atlamak için aşağıdaki bağlantılardan herhangi birine tıklayın: **gelen**, **arka uç**, **giden**.

    **Gelen** bölümünde, çağırandan alınan özgün istek API Yönetimini ve 2. adımda eklediğimiz hız sınırı ve üst bilgi ayarlama ilkeleri de dahil isteğe uygulanan tüm ilkeleri görürsünüz.

    **Arka uç** bölümünde, API arka ucuna gönderilen isteklerin API Yönetimini ve aldığı yanıtı görürsünüz.

    **Giden** bölümünde, çağırana geri gönderilmeden önce yanıta uygulanan tüm ilkeleri görürsünüz.

    > [!TIP]
    > Her bir adım, isteğin API Yönetimi tarafından alınmasından bu yana geçen süreyi de gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çağrı izleme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Düzeltmeleri kullanma](api-management-get-started-revise-api.md)
