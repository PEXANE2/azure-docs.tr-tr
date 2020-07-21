---
title: 'Öğretici: Swift iOS kod örneğini kullanarak modern okuyucu başlatma'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, tam ekran okuyucuyu başlatan örnek bir Swift uygulamasını yapılandırıp çalıştıracaksınız.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.openlocfilehash: d9bb3a5fb41215f2e839efc7989ea7854f254f16
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537944"
---
# <a name="tutorial-start-the-immersive-reader-using-the-swift-ios-code-sample"></a>Öğretici: Swift iOS kod örneğini kullanarak modern okuyucu başlatma

[Genel bakışta](./overview.md), derinlikli okuyucu ne olduğunu ve dil öğrenimi, gelişmekte olan okuyucular ve öğrenme farklılığı olan öğrenciler için okuma kavraışını geliştirmek üzere kendini kanıtlamış tekniklerin nasıl uyguladığını öğrendiniz. Bu öğreticide, tam ekran okuyucuyu Başlatan bir iOS uygulamasının nasıl oluşturulacağı ele alınmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek bir proje kullanarak iOS için bir Swift uygulaması yapılandırın ve çalıştırın.
> * Erişim belirteci alın.
> * Örnek içerikle tam ekran okuyucuyu başlatın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özelliklerini yapılandırırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.
* [MacOS](https://www.apple.com/macos).
* [Git](https://git-scm.com/).
* [Modern Okuyucu SDK 'sı](https://github.com/microsoft/immersive-reader-sdk).
* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="configure-authentication-credentials"></a>Kimlik doğrulama kimlik bilgilerini yapılandırma

1. Xcode ' u açın ve **immersive-Reader-SDK/js/Samples/iOS/QuickStart-Swift/QuickStart-Swift. XCODEPROJ**dosyasını açın.
1. Üstteki menüde **ürün**  >  **düzeni**  >  **düzenleme şeması**' nı seçin.
1. **Çalıştır** görünümünde, **bağımsız değişkenler** sekmesini seçin.
1. **Ortam değişkenleri** bölümünde aşağıdaki adları ve değerleri ekleyin. Tam ekran okuyucuyu oluştururken verilen değerleri sağlayın.

    ```text
    TENANT_ID=<YOUR_TENANT_ID>
    CLIENT_ID=<YOUR_CLIENT_ID>
    CLIENT_SECRET<YOUR_CLIENT_SECRET>
    SUBDOMAIN=<YOUR_SUBDOMAIN>
    ```

Ortak yapılmaması gereken gizli dizileri içerdiğinden bu değişikliği kaynak denetimine oluşturmayın.

## <a name="start-the-immersive-reader-with-sample-content"></a>Örnek içerikle modern okuyucu başlatma

Xcode 'da projeyi çalıştırmak için **CTRL + R** ' yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](./reference.md)bulun.
* [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/)'daki kod örneklerini görüntüleyin.
