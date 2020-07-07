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
ms.openlocfilehash: a9259026747102dd65be3bb8da853735098667db
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050179"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-swift-ios-code-sample"></a>Öğretici: Swift iOS kod örneğini kullanarak modern okuyucu başlatma

[Genel bakışta](./overview.md), derinlikli okuyucu ne olduğunu ve dil öğrenimi, gelişmekte olan okuyucular ve öğrenme farklılığı olan öğrenciler için okuma kavraışını geliştirmek üzere kendini kanıtlamış tekniklerin nasıl uyguladığını öğrendiniz. Bu öğreticide, tam ekran okuyucuyu Başlatan bir iOS uygulamasının nasıl oluşturulacağı ele alınmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek bir proje kullanarak iOS için bir Swift uygulaması yapılandırma ve çalıştırma
> * Erişim belirteci alma
> * Örnek içerikle modern okuyucu başlatma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özellikleri yapılandırılırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.
* [macOS](https://www.apple.com/macos)
* [Git](https://git-scm.com/)
* [Modern Okuyucu SDK 'Sı](https://github.com/microsoft/immersive-reader-sdk)
* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

## <a name="configure-authentication-credentials"></a>Kimlik doğrulama kimlik bilgilerini yapılandırma

1. Xcode 'u açın ve **immersive-Reader-SDK/js/Samples/iOS/QuickStart-Swift/QuickStart-Swift. XCODEPROJ**dosyasını açın.
2. Üstteki menüde **ürün > düzeni ' ne tıklayın > düzeni Düzenle...**
3. **Çalıştır** görünümünde, **bağımsız değişkenler** sekmesine tıklayın.
4. **Ortam değişkenleri** bölümünde, tam ekran okuyucuyu oluştururken verilen değerleri sağlayarak aşağıdaki adları ve değerleri ekleyin.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

Ortak yapılmamalıdır gizli dizileri içerdiğinden, yukarıdaki değişikliği kaynak denetimine yürütmemeyi unutmayın.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle modern okuyucu başlatma

1. Xcode 'da, projeyi çalıştırmak için **CTRL-R** tuşlarına basın.

## <a name="next-steps"></a>Sonraki adımlar

* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet
* [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/) 'daki kod örneklerini görüntüle
