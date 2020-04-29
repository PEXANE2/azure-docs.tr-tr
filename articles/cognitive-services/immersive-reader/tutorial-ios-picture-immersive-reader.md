---
title: 'Öğretici: fotoğrafı alıp tam ekran okuyucu (Swift) ile Başlatan bir iOS uygulaması oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, sıfırdan bir iOS uygulaması oluşturacaksınız ve resmi tam ekran okuyucu işlevselliğine ekleyeceksiniz.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 69ff58d6cdabe49000b00afecfc6b4ad1a3f2daa
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76841855"
---
# <a name="tutorial-create-an-ios-app-that-launches-the-immersive-reader-with-content-from-a-photo-swift"></a>Öğretici: bir fotoğraftan içerikle modern okuyucu Başlatan bir iOS uygulaması oluşturma (Swift)

[Tam ekran okuyucu](https://www.onenote.com/learningtools) , okuma kavramasını geliştirmek için kendini kanıtlamış teknikler uygulayan, ve dahil tasarlanmış bir araçtır.

Görüntü İşleme bilişsel [Hizmetler okuma API 'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/concept-recognizing-text) , Microsoft 'un en son tanıma modellerini kullanarak bir görüntüdeki metin içeriğini algılar ve tanımlanan metni makine tarafından okunabilen bir karakter akışına dönüştürür.

Bu öğreticide, sıfırdan bir iOS uygulaması oluşturacaksınız ve okuma API 'SI ile modern okuyucu kullanarak tam ekran okuyucu SDK 'sını tümleştirirsiniz. Bu öğreticinin tam bir çalışma örneğine [buradan](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/samples/picture-to-immersive-reader-swift)ulaşabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Örnek proje özellikleri yapılandırılırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.
* Bu örneğin kullanımı, Görüntü İşleme bilişsel hizmet hizmetine bir Azure aboneliği gerektirir. [Azure Portal bir görüntü işleme bilişsel hizmet kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode 'da yeni bir proje oluşturun.

![Yeni Proje](./media/ios/xcode-create-project.png)

**Tek görünüm uygulaması**' nı seçin.

![Yeni tek görünüm uygulaması](./media/ios/xcode-single-view-app.png)

## <a name="get-the-sdk-cocoapod"></a>SDK CocoaPod edinme
Tam ekran okuyucu SDK 'sını kullanmanın en kolay yolu CocoaPods aracılığıyla yapılır. Cocoapods aracılığıyla yüklemek için:
1. [Install Cocoapods](http://guides.cocoapods.org/using/getting-started.html) -Cocoapods yüklemek için Başlarken Kılavuzunu izleyin.
2. Xcode projenizin kök dizininde çalıştırarak `pod init` bir pod dosyası oluşturun.
3.  CocoaPod öğesini ekleyerek `pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'`Pod dosyanıza ekleyin. Pod dosyanız, hedefin adı ile tam ekran-------Swift arasında değişmeli şekilde aşağıdaki gibi görünmelidir:
 ```ruby
  platform :ios, '9.0'

  target 'picture-to-immersive-reader-swift' do
  use_frameworks!
  # Pods for picture-to-immersive-reader-swift
  pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'
  end
```
4. Terminalde, Xcode projenizin dizininde, modern Okuyucu SDK Pod 'u yüklemek için komutunu `pod install` çalıştırın.
5. SDK `import immersive_reader_sdk` 'ya başvurması gereken tüm dosyalara ekleyin.
6. Dosya değil `.xcworkspace` `.xcodeproj` , dosyayı açarak projeyi açmanız emin olun.

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD kimlik doğrulaması belirteci alma

Bu bölüm için yukarıdaki Azure AD kimlik doğrulama yapılandırması önkoşul adımından bazı değerlere ihtiyacınız vardır. Bu oturumu kaydettiğiniz metin dosyasına geri bakın.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

ViewController. Swift dosyasını içeren ana proje klasöründe, sabitler. Swift adlı bir Swift sınıfı dosyası oluşturun. Sınıfı aşağıdaki kodla değiştirin ve uygun olduğunda değerlerinize ekleyin. Bu dosyayı yalnızca makinenizde bulunan yerel bir dosya olarak tutun ve bu dosyayı kaynak denetimine kaydetmediğinden emin olun. bu dosya, genel hale getirilmemelidir. Uygulamanızda gizli dizileri tutmanız önerilir. Bunun yerine, belirteci almak için bir arka uç hizmeti kullanmanızı öneririz; burada gizli dizi, uygulamanın dışında ve cihazın dışında tutulabilir. Yetkisiz kullanıcıların, bir dizi kimlik doğrulama (örneğin, [OAuth](https://oauth.net/2/)) arkasında güvenli hale getirilmesi gerekir, bu da yetkisiz kullanıcıların, tam ekran okuyucusu hizmetinize ve faturalandırmaya yönelik belirteçleri kullanmasını önler; Bu iş, Bu öğreticinin kapsamı dışındadır.

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>Uygulamayı görsel taslak olmadan çalışacak şekilde ayarlama

AppDelegate. Swift dosyasını açın ve dosyayı aşağıdaki kodla değiştirin.

## <a name="add-functionality-for-taking-and-uploading-photos"></a>Fotoğraf alma ve karşıya yükleme işlevlerini ekleme

ViewController. Swift ' i PictureLaunchViewController. Swift olarak yeniden adlandırıp dosyayı aşağıdaki kodla değiştirin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Bir simülatör veya cihaz hedefi seçerek Xcode 'da arşiv şemasını ayarlayın.
![Arşiv şeması](./media/ios/xcode-archive-scheme.png)<br/>
![Hedef seçme](./media/ios/xcode-select-target.png)

Xcode 'da, CTRL + R tuşlarına basın veya projeyi çalıştırmak için Yürüt düğmesine tıklayın ve uygulamanın belirtilen simülatör veya cihazda başlatılması gerekir.

Uygulamanızda şunları görmeniz gerekir:

![Örnek uygulama](./media/ios/picture-to-immersive-reader-ipad-app.png)

Uygulamanın içinde, "fotoğrafı al" düğmesine veya ' kitaplıktan fotoğraf seç ' düğmesine basarak bir metin fotoğrafı alın veya karşıya yükleyin. böylece, tam ekran okuyucu daha sonra fotoğraftan metni görüntülemeye başlar.

![Tam Ekran Okuyucu](./media/ios/picture-to-immersive-reader-ipad.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet
