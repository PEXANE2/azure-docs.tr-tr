---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma, C# (Xamarin)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede bilişsel hizmetler konuşma SDK 'sını kullanarak Evrensel Windows Platformu (UWP), Android ve iOS için platformlar arası C# Xamarin uygulaması oluşturacaksınız. Cihazınızın veya benzeticisinin mikrofonunuzdan konuşmayı gerçek zamanlı olarak metne dönüştürme. Uygulama, konuşma SDK 'Sı NuGet paketiyle oluşturulmuştur ve 2019 Microsoft Visual Studio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.custom: devx-track-csharp
ms.openlocfilehash: 48a80d7367943c2c86fd3822a5c236b0cdf11846
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88926464"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=xamarin&pivots=programming-language-csharp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

Bunu zaten yaptıysanız harika. Şimdi devam edelim.

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Ortak HelloWorld projesi için örnek kod ekleme

Ortak HelloWorld projesi, platformlar arası uygulamanız için platformdan bağımsız uygulamalar içerir. Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve uygulamanın arkasındaki C# kodunu ekleyin.

1. **Çözüm Gezgini**, ortak HelloWorld projesi altında öğesini açın `MainPage.xaml` .

1. Tasarımcı XAML görünümünde, ve arasında **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin `<StackLayout>` `</StackLayout>` :

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` . Bu, altında gruplandırılır `MainPage.xaml` .

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. Kaynak dosyanın `OnRecognitionButtonClicked` işleyicisinde, dizeyi bulun `YourSubscriptionKey` ve abonelik anahtarınızla değiştirin.


1. İşleyicisinde, `OnRecognitionButtonClicked` dizeyi bulun `YourServiceregion` ve aboneliğinizle Ilişkili [bölgeden](https://aka.ms/speech/sdkregion) **bölge tanımlayıcısıyla** değiştirin. 

1. Daha sonra, UWP, Android ve iOS gibi farklı platform projelerinden gelen mikrofon izinlerini sorgulamak için kullanılan bir [Xamarin hizmeti](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/)oluşturmanız gerekir. Bunu yapmak için HelloWorld projesi altında *Hizmetler* adlı yeni bir klasör ekleyin ve bunun altında yeni bir C# kaynak dosyası oluşturun. *Hizmetler* klasörüne sağ tıklayıp **Add**  >  **Yeni öğe**  >  **kodu dosyası**Ekle ' yi seçebilirsiniz. Dosyayı yeniden adlandırın `IMicrophoneService.cs` ve aşağıdaki kod parçacığındaki tüm kodu bu dosyaya yerleştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

#### <a name="android"></a>[Android](#tab/x-android)
## <a name="add-sample-code-for-the-helloworldandroid-project"></a>Proje için örnek kod ekleme `helloworld.Android`

Şimdi uygulamanın Android 'e özgü bölümünü tanımlayan C# kodunu ekleyin.

1. **Çözüm Gezgini**, HelloWorld altında. Android projesi açık `MainActivity.cs` .

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Ardından, `MicrophoneService` HelloWorld altında yeni klasör *hizmetlerini* oluşturarak Android 'e özgü uygulama ekleyin. Android projesi. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı yeniden adlandırın `MicrophoneService.cs` . Aşağıdaki kod parçacığını kopyalayıp bu dosyaya yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Bundan sonra, `AndroidManifest.xml` *Özellikler* klasörünün altında öğesini açın. Ve arasında mikrofon için aşağıdaki kullanımlar-izin ayarını ekleyin `<manifest>` `</manifest>` :

   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```
   
#### <a name="ios"></a>[iOS](#tab/ios)
## <a name="add-sample-code-for-the-helloworldios-project"></a>Proje için örnek kod ekleme `helloworld.iOS`

Şimdi uygulamanın iOS 'a özgü bölümünü tanımlayan C# kodunu ekleyin. Ayrıca HelloWorld. iOS projesinde Apple cihaza özgü yapılandırma oluşturma.

1. **Çözüm Gezgini**, HelloWorld. iOS projesi altında öğesini açın `AppDelegate.cs` .

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Sonra, `MicrophoneService` HelloWorld.io projesi altında yeni klasör *hizmetlerini* oluşturarak için iOS 'a özgü uygulama ekleyin. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı yeniden adlandırın `MicrophoneService.cs` . Aşağıdaki kod parçacığını kopyalayıp bu dosyaya yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. `Info.plist`Metin düzenleyicisinde HelloWorld. iOS projesi altında açın. Aşağıdaki anahtar değer çiftini dict bölümüne ekleyin:

   <key>Nsmikro Phoneusagedescription</key> 
    <string>Bu örnek uygulama, mikrofon erişimi gerektirir</string>

   > [!NOTE]
   > İPhone cihazı için derleme yapıyorsanız `Bundle Identifier` cihazınızın sağlama profili uygulama kimliğiyle eşleştiğinden emin olun. Aksi takdirde, yapı başarısız olur. İPhoneSimulator ile bunu olduğu gibi bırakabilirsiniz.

1. Bir Windows bilgisayarı üzerinde oluşturuyorsanız, **Araçlar**aracılığıyla Mac  >  **iOS**  >  **'e iOS çifti**arasında oluşturma için Mac cihazına bir bağlantı kurun. Mac cihazına bağlantıyı etkinleştirmek için Visual Studio tarafından sunulan yönerge Sihirbazı ' nı izleyin.

#### <a name="uwp"></a>[UWP](#tab/helloworlduwp)
## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Proje için örnek kod ekleme `helloworld.UWP`

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>HelloWorld için örnek kod ekleyin. UWP projesi

Şimdi uygulamanın UWP 'e özgü bölümünü tanımlayan C# kodunu ekleyin.

1. **Çözüm Gezgini**, HelloWorld altında. UWP projesi, açın `MainPage.xaml.cs` .

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Ardından, `MicrophoneService` HelloWorld ' ın altında yeni klasör *HIZMETLERINI* oluşturarak UWP 'e özgü bir uygulama ekleyin. UWP projesi. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı yeniden adlandırın `MicrophoneService.cs` . Aşağıdaki kod parçacığını kopyalayıp bu dosyaya yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Ardından, `Package.appxmanifest` HelloWorld altındaki dosyaya çift tıklayın. Visual Studio içinde UWP projesi. **Yetenekler**altında, **mikrofonun** seçili olduğundan emin olun ve dosyayı kaydedin.

1. Daha sonra `Package.appxmanifest` Visual Studio içindeki proje altındaki dosya ' ya ve yetenekler ' in altında bulunan dosya ' ya sağ tıklayın `helloworld.UWP` **Capabilities**  >  **Microphone** ve dosyayı kaydedin.
   > Not: bir uyarı görmeniz durumunda: sertifika dosyası yok: HelloWorld. Daha fazla bilgi için lütfen [konuşmayı metin](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp) örneğine göz atın. UWP_TemporaryKey

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya**  >  **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-uwp-application"></a>UWP uygulamasını derleme ve çalıştırma

1. HelloWorld ayarlayın. Başlangıç projesi olarak UWP. HelloWorld öğesine sağ tıklayın. UWP projesi ve uygulamayı derlemek için **Oluştur** ' u seçin.

1. **Debug**  >  Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' ı seçin (veya **F5**' i seçin). **HelloWorld** penceresi görüntülenir.

   ![C# ' de örnek UWP konuşma tanıma uygulaması-hızlı başlangıç](../../../../media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. **Mikrofonu etkinleştir**' i seçin. Erişim izni isteği göründüğünde **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](../../../../media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. **Konuşma tanımayı Başlat**' ı seçin ve cihazınızın mikrofonuna bir İngilizce tümcecik veya cümle konuşun. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

   ![Konuşma tanıma kullanıcı arabirimi](../../../../media/sdk/qs-csharp-xamarin-uwp-ui-result.png)
* * *

## <a name="build-and-run-the-android-and-ios-applications"></a>Android ve iOS uygulamalarını derleyin ve çalıştırın

Android ve iOS uygulamalarını cihazda veya simülatörde oluşturmak ve çalıştırmak UWP 'ye benzer bir şekilde gerçekleşir. Tüm SDK 'ların Bu makalenin "Önkoşullar" bölümünde gerektiği şekilde yüklendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
