---
title: "Öğretici: Fotoğraf çeken ve Onu Sürükleyici Okuyucu 'da (Swift) başlatan bir iOS uygulaması oluşturun"
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, sıfırdan bir iOS uygulaması oluşturacak ve Resim'i Sürükleyici Okuyucu işlevine ekleyeceksiniz.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 69ff58d6cdabe49000b00afecfc6b4ad1a3f2daa
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841855"
---
# <a name="tutorial-create-an-ios-app-that-launches-the-immersive-reader-with-content-from-a-photo-swift"></a>Öğretici: Bir fotoğraftan (Swift) içerik içeren Sürükleyici Okuyucuyu başlatan bir iOS uygulaması oluşturun

[Immersive Reader,](https://www.onenote.com/learningtools) okuma anlama geliştirmek için kanıtlanmış teknikleri uygulayan kapsayıcı bir şekilde tasarlanmış bir araçtır.

[Bilgisayar Görüşü Bilişsel Hizmetler Okuma API'si,](https://docs.microsoft.com/azure/cognitive-services/computer-vision/concept-recognizing-text) Microsoft'un en son tanıma modellerini kullanarak görüntüdeki metin içeriğini algılar ve tanımlanan metni makine tarafından okunabilir bir karakter akışına dönüştürür.

Bu eğitimde, sıfırdan bir iOS uygulaması oluşturacak ve Immersive Reader SDK'yı kullanarak Okuma API'sini ve Sürükleyici Okuyucu'yu entegre edebilirsiniz. Bu öğretici tam bir çalışma örneği [burada](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/samples/picture-to-immersive-reader-swift)mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Azure Etkin Dizin kimlik doğrulaması için yapılandırılan Sürükleyici Bir Reader kaynağı. Kurulumu için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Örnek proje özelliklerini yapılandırırken burada oluşturulan bazı değerlere ihtiyacınız olacaktır. Oturumunuzun çıktısını ileride başvurmak için bir metin dosyasına kaydedin.
* Bu örneğin kullanımı, Computer Vision Bilişsel Hizmeti için bir Azure aboneliği gerektirir. [Azure portalında Bir Bilgisayar Vizyonu Bilişsel Hizmet kaynağı oluşturun.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'da yeni bir proje oluşturun.

![Yeni Proje](./media/ios/xcode-create-project.png)

**Tek Görünüm Uygulaması**seçin.

![Yeni Tek Görünüm Uygulaması](./media/ios/xcode-single-view-app.png)

## <a name="get-the-sdk-cocoapod"></a>SDK CocoaPod'u alın
Immersive Reader SDK'yı kullanmanın en kolay yolu CocoaPods'dur. Cocoapods üzerinden yüklemek için:
1. [CocoaPods yükleyin](http://guides.cocoapods.org/using/getting-started.html) - Cocoapods yüklemek için başlangıç kılavuzuizleyin.
2. Xcode projenizin `pod init` kök dizininde çalıştırarak bir Podfile oluşturun.
3.  CocoaPod'u Podfile'nıza `pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'`ekleyerek ekleyin. Podfile'niz aşağıdaki gibi görünmelidir ve hedefinizin adı resimden sürükleyici okuyucuya hızlı bir şekilde değiştirilmelidir:
 ```ruby
  platform :ios, '9.0'

  target 'picture-to-immersive-reader-swift' do
  use_frameworks!
  # Pods for picture-to-immersive-reader-swift
  pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'
  end
```
4. Terminalde, Xcode projenizin dizininde, Sürükleyici `pod install` Okuyucu SDK bölmesini yüklemek için komutu çalıştırın.
5. SDK'ya başvurması gereken tüm dosyalara ekleyin. `import immersive_reader_sdk`
6. Dosyayı değil, dosyayı `.xcworkspace` açarak `.xcodeproj` projeyi açtığından emin olun.

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD kimlik doğrulama belirteci edinme

Bu bölüm için yukarıdaki Azure AD kimlik doğrulama yapılandırması önkoşul adımından bazı değerlere ihtiyacınız vardır. O oturumdan kaydettiğiniz metin dosyasına geri bakın.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

ViewController.swift dosyasını içeren ana proje klasöründe Constants.swift adlı bir Swift sınıfı dosya oluşturun. Sınıfı aşağıdaki kodla değiştirerek, uygun olan yerlerde değerlerinize ekleyerek. Bu dosyayı yalnızca makinenizde bulunan yerel bir dosya olarak tutun ve herkese açık hale getirilmemesi gereken sırlar içerdiğinden bu dosyayı kaynak denetimine işlemediğinden emin olun. Uygulamanızda sır saklamamanız önerilir. Bunun yerine, sırların uygulamanın dışında ve cihazın dışında tutulabileceği belirteci elde etmek için bir arka uç hizmeti kullanmanızı öneririz. Arka uç API bitiş noktası, yetkisiz kullanıcıların Sürükleyici Reader hizmetinize ve faturanıza karşı kullanılacak jetonları almasını önlemek için bir tür kimlik doğrulamanın (örneğin, [OAuth)](https://oauth.net/2/)arkasına sabitlenmelidir; bu çalışma bu öğretici kapsamı dışındadır.

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>Uygulamayı bir film şeridi olmadan çalışacak şekilde ayarlama

AppDelegate.swift'i açın ve dosyayı aşağıdaki kodla değiştirin.

## <a name="add-functionality-for-taking-and-uploading-photos"></a>Fotoğraf çekmek ve yüklemek için işlevsellik ekleme

ViewController.swift'i PictureLaunchViewController.swift olarak yeniden adlandırın ve dosyayı aşağıdaki kodla değiştirin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Bir simülatör veya aygıt hedefi seçerek arşiv düzenini Xcode'da ayarlayın.
![Arşiv şeması](./media/ios/xcode-archive-scheme.png)<br/>
![Hedef seçme](./media/ios/xcode-select-target.png)

Xcode'da Ctrl + R tuşuna basın veya projeyi çalıştırmak için oynat düğmesine tıklayın ve uygulamanın belirtilen simülatör veya cihazda başlatılması gerekir.

Uygulamanızda şunları görmeniz gerekir:

![Örnek uygulama](./media/ios/picture-to-immersive-reader-ipad-app.png)

Uygulamanın içinde, 'Fotoğraf Çek' düğmesine veya 'Kütüphaneden Fotoğraf Seç' düğmesine basarak metin fotoğrafı çekin veya yükleyin ve Immersive Reader daha sonra fotoğraftaki metni görüntülemeye başlayacaktır.

![Tam Ekran Okuyucu](./media/ios/picture-to-immersive-reader-ipad.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sürükleyici [Okuyucu SDK Referans](./reference.md) keşfedin
