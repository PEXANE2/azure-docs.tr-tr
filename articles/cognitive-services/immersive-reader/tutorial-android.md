---
title: 'Öğretici: Android kod örneklerini kullanarak modern okuyucu başlatma'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, tam ekran okuyucuyu başlatan örnek bir Android uygulamasını yapılandırıp çalıştıracaksınız.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.openlocfilehash: d847b4ab9f3c06634390e1f67dfed36df938c3ad
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050175"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-android-java-code-sample"></a>Öğretici: Android Java kod örneğini kullanarak modern okuyucu başlatma

[Genel bakışta](./overview.md), derinlikli okuyucu ne olduğunu ve dil öğrenimi, gelişmekte olan okuyucular ve öğrenme farklılığı olan öğrenciler için okuma kavraışını geliştirmek üzere kendini kanıtlamış tekniklerin nasıl uyguladığını öğrendiniz. Bu öğreticide, tam ekran okuyucuyu Başlatan bir Android uygulamasının nasıl oluşturulacağı ele alınmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek proje kullanarak Android için uygulama yapılandırma ve çalıştırma
> * Erişim belirteci alma
> * Örnek içerikle modern okuyucu başlatma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özellikleri yapılandırılırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.
* [Git](https://git-scm.com/)
* [Modern Okuyucu SDK 'Sı](https://github.com/microsoft/immersive-reader-sdk)
* [Android Studio](https://developer.android.com/studio)

## <a name="configure-authentication-credentials"></a>Kimlik doğrulama kimlik bilgilerini yapılandırma

1. Android Studio başlatın ve projeyi **tam ekran-okuyucu-SDK/js/örnekler/hızlı başlangıç-Java-Android** dizininden (Java) veya **modern-Reader-SDK/js/Samples/QuickStart-Kotlin** dizininden (Kotlin) açın.

2. **/Varlıklar** klasörüne **env** adlı bir dosya oluşturun ve aşağıdaki değerleri uygun şekilde ekleyin. Ortak olmaması gereken gizli dizileri içerdiğinden, bu dosyayı kaynak denetimine yürütmemeyi unutmayın.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET=<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle modern okuyucu başlatma

1. AVD yöneticisinden bir cihaz öykünücüsü seçin ve projeyi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet
* [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/) 'daki kod örneklerini görüntüle