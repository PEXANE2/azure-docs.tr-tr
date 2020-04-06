---
title: 'Quickstart: Bir mikrofon, C# (Xamarin) konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: 'Bu makalede, Bilişsel Hizmetler Konuşma SDK kullanarak Evrensel Windows Platformu (UWP), Android ve iOS için bir çapraz platform C # Xamarin uygulaması oluşturun. Konuşmanızı cihazınızın veya simülatörün mikrofonundan gerçek zamanlı olarak metne çevirirsiniz. Uygulama Speech SDK NuGet Paketi ve Microsoft Visual Studio 2019 ile oluşturulmuştır.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.openlocfilehash: 699bbea9d50c24558f6c9d4671cb6035e4718a43
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671484"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=xamarin&pivots=programming-language-csharp)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

Bunu zaten yaptıysan, harika. Devam edelim.

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Ortak helloworld projesi için örnek kod ekleme

Ortak helloworld projesi, platform ötesi uygulamanız için platformdan bağımsız uygulamalar içerir. Şimdi, uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve uygulamanın arkasına C# kodunu ekleyin.

1. **Çözüm Explorer**, ortak helloworld projesi `MainPage.xaml`altında, açık .

1. Tasarımcının XAML görünümünde, aşağıdaki XAML snippet'i Aşağıdaki `<StackLayout>` Ler `</StackLayout>`arasındaki **Izgara** etiketine ekleyin:

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml)]

1. **Çözüm Gezgini'nde,** kod arkasındaki `MainPage.xaml.cs`kaynak dosyayı açın. Altında `MainPage.xaml`gruplanmış.

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. Kaynak dosyanın `OnRecognitionButtonClicked` işleyicisinde dizeyi `YourSubscriptionKey`bulun ve abonelik anahtarınızla değiştirin.


1. İşleyicide `OnRecognitionButtonClicked` dizeyi `YourServiceregion`bulun ve aboneliğinizle ilişkili [bölgeden](https://aka.ms/speech/sdkregion) Bölge **tanımlayıcısıyla** değiştirin. (Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.)

1. Ardından, UWP, Android ve iOS gibi farklı platform projelerinden mikrofon izinlerini sorgulamak için kullanılan bir [Xamarin Hizmeti](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/)oluşturmanız gerekir. Bunu yapmak için, helloworld projesinin altına *Hizmetler* adlı yeni bir klasör ekleyin ve altında yeni bir C# kaynak dosyası oluşturun. *Hizmetler* klasörünü sağ tıklayabilir ve**Yeni Öğe** > **Kodu Dosyası** **Ekle'yi** > seçebilirsiniz. Dosyayı `IMicrophoneService.cs`yeniden adlandırın ve bu dosyaya aşağıdaki parçacıktan tüm kodu yerleştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

#### <a name="android"></a>[Android](#tab/x-android)
## <a name="add-sample-code-for-the-helloworldandroid-project"></a>`helloworld.Android` Proje için örnek kod ekleme

Şimdi uygulamanın Android'e özgü kısmını tanımlayan C# kodunu ekleyin.

1. **Çözüm Explorer**, helloworld altında. Android projesi, `MainActivity.cs`açık .

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Ardından, helloworld altında `MicrophoneService` yeni klasör *Hizmetleri* oluşturarak Android özel uygulama ekleyin. Android projesi. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı `MicrophoneService.cs`yeniden adlandırın. Aşağıdaki kod parçacıklarını bu dosyaya kopyalayıp yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Bundan sonra, `AndroidManifest.xml` *Özellikler* klasörü altında açın. Mikrofon için aşağıdaki kullanım-izin ayarını `</manifest>`ekleyin: `<manifest>`

   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```
   
#### <a name="ios"></a>[iOS](#tab/ios)
## <a name="add-sample-code-for-the-helloworldios-project"></a>`helloworld.iOS` Proje için örnek kod ekleme

Şimdi uygulamanın iOS'a özgü kısmını tanımlayan C# kodunu ekleyin. Ayrıca helloworld.iOS projesiiçin Apple cihaza özel yapılandırmalar oluşturun.

1. **Solution Explorer'da**, helloworld.iOS `AppDelegate.cs`projesi kapsamında, açık .

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Ardından, helloworld.iO projesi altında `MicrophoneService` yeni klasör *Hizmetleri* oluşturarak iOS'a özgü bir uygulama ekleyin. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı `MicrophoneService.cs`yeniden adlandırın. Aşağıdaki kod parçacıklarını bu dosyaya kopyalayıp yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. Metin `Info.plist` düzenleyicisindeki helloworld.iOS projesi altında açın. Dikte bölümü altına aşağıdaki anahtar değer çiftini ekleyin:

   <key>NSMicrophoneUsageDescription</key>
   <string>Bu örnek uygulama mikrofon erişimi gerektirir</string>

   > [!NOTE]
   > Bir iPhone aygıtı için bina oluşturuyorsanız, cihazınızın sağlama profili uygulaması kimliğiyle eşleştiğinden `Bundle Identifier` emin olun. Aksi takdirde, yapı başarısız olur. iPhoneSimulator ile olduğu gibi bırakabilirsiniz.

1. Bir Windows PC'de bina oluşturuyorsanız, **Araçlar** > **iOS** > Pair üzerinden**Mac'e**oluşturmak için Mac aygıtına bağlantı kurun. Mac aygıtına bağlantıyı etkinleştirmek için Visual Studio tarafından sağlanan yönerge sihirbazını izleyin.

#### <a name="uwp"></a>[UWP](#tab/helloworlduwp)
## <a name="add-sample-code-for-the-helloworlduwp-project"></a>`helloworld.UWP` Proje için örnek kod ekleme

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Helloworld için örnek kod ekleyin. UWP projesi

Şimdi, uygulamanın UWP'ye özgü bölümünü tanımlayan C# kodunu ekleyin.

1. **Çözüm Explorer**, helloworld altında. UWP projesi, `MainPage.xaml.cs`açık .

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Ardından, helloworld altında yeni `MicrophoneService` klasör *Hizmetleri* oluşturarak uwp özgü bir uygulama ekleyin. UWP projesi. Bundan sonra, altında yeni bir C# kaynak dosyası oluşturun. Dosyayı `MicrophoneService.cs`yeniden adlandırın. Aşağıdaki kod parçacıklarını bu dosyaya kopyalayıp yapıştırın:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Ardından, helloworld `Package.appxmanifest` altındaki dosyayı çift tıklatın. Visual Studio içinde UWP projesi. **Özellikler**altında **Mikrofon'un** seçildiğinden emin olun ve dosyayı kaydedin.

1. Sonraki çift `Package.appxmanifest` tıklama `helloworld.UWP` dosyası Visual Studio içinde proje altında ve **Yetenekleri** > **Mikrofon** altında kontrol edilir ve dosya yı kaydedin.
   > Not: Uyarı görürseniz : Sertifika dosyası yok: helloworld. UWP_TemporaryKey.pfx, daha fazla bilgi için [lütfen metin örneğine konuşmayı](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp) kontrol edin.

1. Menü çubuğundan, değişikliklerinizi kaydetmek için**Tümünü Kaydet'i** **seçin.** > 

## <a name="build-and-run-the-uwp-application"></a>UWP uygulamasını oluşturma ve çalıştırma

1. Helloworld'u ayarla. Başlangıç projesi olarak UWP. Helloworld sağ tıklayın. UWP projesi ve uygulamayı oluşturmak için **Yap'ı** seçin.

1. Uygulamayı başlatmak için **Hata** > **Ayıklama Başlatma Hata Ayıklama'yı** (veya **F5'i**seçin) seçin. **Helloworld** penceresi görüntülenir.

   ![C# örnek UWP konuşma tanıma uygulaması - quickstart](../../../../media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. **Mikrofonu Etkinleştir'i**seçin. Erişim izni isteği göründüğünde **Evet'i**seçin.

   ![Mikrofon erişim izni isteği](../../../../media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. **Konuşma tanıyı başlat'ı**seçin ve cihazınızın mikrofonuna İngilizce bir ifade veya cümle konuşun. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

   ![Konuşma tanıma kullanıcı arabirimi](../../../../media/sdk/qs-csharp-xamarin-uwp-ui-result.png)
* * *

## <a name="build-and-run-the-android-and-ios-applications"></a>Android ve iOS uygulamalarını oluşturun ve çalıştırın

Cihazda veya simülatörde Android ve iOS uygulamaları oluşturmak ve çalıştırmak UWP'ye benzer bir şekilde gerçekleşir. Tüm SDK'ların bu makalenin "Önkoşullar" bölümünde gerektiği gibi doğru şekilde yüklendiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](../footer.md)]
