---
title: 'Hızlı başlangıç: konuşmayı tanıma C# , (Xamarin)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede bilişsel hizmetler konuşma SDK 'sını kullanarak C# Windows UWP, Android ve iOS için platformlar arası bir Xamarin uygulaması oluşturacaksınız. Cihazınızın veya benzeticisinin mikrofonunuzdan konuşmayı gerçek zamanlı olarak metne dönüştürme. Uygulama, konuşma SDK 'Sı NuGet paketiyle oluşturulmuştur ve 2019 Microsoft Visual Studio.
services: cognitive-services
author: jhakulin
manager: robch
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: jhakulin
ms.custom: ''
ms.openlocfilehash: 02031e8e2800329134eaf34f5f99c93a20b99aac
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71804058"
---
# <a name="quickstart-recognize-speech-using-cross-platform-xamarin-app-by-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak, platformlar arası Xamarin uygulaması kullanarak konuşmayı tanıma

Hızlı Başlangıç [özelliği, konuşma](quickstart-csharp-uwp.md)ve [metin okuma](quickstart-text-to-speech-csharp-uwp.md) ve konuşma [çevirisi](quickstart-translate-speech-uwp.md)için de kullanılabilir.

Bu makalede, [konuşma SDK 'sını](speech-sdk.md)kullanarak evrensel WINDOWS platformu (UWP) C# , Android ve iOS için Xamarin kullanarak platformlar arası bir uygulama geliştirireceğiz. Program, cihazınızın mikrofonunuzdan gerçek zamanlı olarak konuşmayı metne dönüştürme. Uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 (herhangi bir sürüm) ile oluşturulmuştur.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* Windows 10 Fall Creators Update ile Windows BILGISAYARı (10,0; Derleme 16299) veya üzeri ve çalışan bir mikrofonla.
* [Visual Studio 'Ya Xamarin yüklemesi](https://docs.microsoft.com/xamarin/get-started/installation/?pivots=windows).
* [Windows 'Da Xamarin Android yüklemesi](https://docs.microsoft.com/xamarin/android/get-started/installation/windows).
* [Windows 'Da Xamarin iOS yüklemesi](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/?pivots=windows).
* Android 'i hedeflemek için: 
   * Bir Android cihazı (ARM32/64, x86; API 23: Android 6,0 Şekerlow veya üzeri) çalışan bir mikrofonla [geliştirme için etkinleştirildi](https://developer.android.com/studio/debug/dev-options) .
* İOS hedeflemek için: bir iOS cihazı (ARM64) veya çalışan bir mikrofonla [geliştirmeye yönelik](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/) bir iOS Simülatörü (x64) etkin.
* Windows ARM64 Build desteği için, [isteğe bağlı derleme araçlarını ve ARM/ARM64 Için Windows 10 SDK](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)'yı yüklemelisiniz.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-xamarin-create-proj.md)]

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Ortak `helloworld` projesi için örnek kod ekleme

Ortak `helloworld` projesi platformlar arası uygulamanız için platformdan bağımsız uygulamalar içerir.
Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve uygulamanın arkasındaki C# kodu ekleyin.

1. **Çözüm Gezgini**, ortak `helloworld` proje altında `MainPage.xaml` ' yi açın.

1. Tasarımcının XAML görünümünde, **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin (`<StackLayout>` ile `</StackLayout>` arasında):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını `MainPage.xaml.cs` ' i açın. (@No__t-0 altında gruplandırılır.)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. Kaynak dosyanın `OnRecognitionButtonClicked` işleyicisine `YourSubscriptionKey` dizesini bulun ve abonelik anahtarınızla değiştirin.

1. @No__t-0 işleyicisine `YourServiceRegion` dizesini bulun ve aboneliğiniz ile ilişkili [bölge](regions.md) ile değiştirin. (Örneğin, ücretsiz deneme aboneliği için `westus` ' ı kullanın.)

1. Daha sonra, farklı platform projelerinden (UWP, Android ve iOS) mikrofon izinlerini sorgulamak için kullanılan bir [Xamarin hizmeti](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/)oluşturmanız gerekir. Bunu yapmak için, `helloworld` projesi altında yeni bir klasör @no__t ve bunun altında yeni C# kaynak dosya oluşturun (`Services` klasörüne sağ tıklayıp  > **Yeni öğe** > **kod dosyası**) **ekleyin**ve tüm kodu `IMicrophoneService.cs` olarak yeniden adlandırın ve Bu dosyada aşağıdaki kod parçacığı: 0 @ no__t-11

## <a name="add-sample-code-for-the-helloworldandroid-project"></a>@No__t-0 projesi için örnek kod ekleme

Şimdi uygulamanın Android C# 'e özgü bölümünü tanımlayan kodu ekleyin.

1. **Çözüm Gezgini**' de, `helloworld.Android` projesi altında `MainActivity.cs` ' yi açın.

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Ardından, `helloworld.Android` projesi altında yeni klasör @no__t `MicrophoneService` ' a yönelik Android 'e özgü uygulama ekleyin. Bundan sonra bu yeni C# kaynak dosyayı oluşturun ve `MicrophoneService.cs` olarak yeniden adlandırın ve aşağıdaki kod parçacığını bu dosyaya yapıştırın.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Bundan sonra, `Properties` klasörü altında `AndroidManifest.xml` ' dır ve `<manifest>` ile `</manifest>` arasında mikrofon için aşağıdaki kullanımlar-izin ayarını ekleyin.
```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## <a name="add-sample-code-for-the-helloworldios-project"></a>@No__t-0 projesi için örnek kod ekleme

Şimdi uygulamanın iOS C# 'a özgü bölümünü tanımlayan kodu ekleyin ve ayrıca HelloWorld. IOS projesine Apple cihaza özgü yapılandırma oluşturun.

1. **Çözüm Gezgini**' de, `helloworld.iOS` projesi altında `AppDelegate.cs` ' yi açın.

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Sonra, `helloworld.iOS` projesi altında yeni klasör @no__t `MicrophoneService` ' a yönelik iOS 'a özgü uygulama ekleyin. Bundan sonra bu yeni C# kaynak dosyayı oluşturun ve `MicrophoneService.cs` olarak yeniden adlandırın ve aşağıdaki kod parçacığını bu dosyaya yapıştırın.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. @No__t-0 projesi altındaki Info. plist öğesini metin düzenleyicisinde açın ve aşağıdaki anahtar değer çiftini dict bölümüne ekleyin <key>Nsmikro Phoneusagedescription</key>
   <string>Bu örnek uygulama, mikrofon erişimi gerektirir</string>
   > Note: iPhone cihazı için derlemeyi hedefliyorsanız, cihazınızın sağlama profili uygulama KIMLIĞIYLE `Bundle Identifier` ile eşleştiğinden emin olun, aksi takdirde derleme başarısız olur. İPhoneSimulator ile bunu olduğu gibi bırakabilirsiniz.

1. Windows BILGISAYARı üzerinde derleme yapıyorsanız, Mac 'e **@no__t-** 1**iOS** >  '**e çift Mac**arasında oluşturma için Mac cihazına bağlantı kurmanız gerekir. Mac cihazına bağlanmayı etkinleştirmek için Visual Studio tarafından sunulan yönerge Sihirbazı ' nı izleyin.

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>@No__t-0 projesi için örnek kod ekleme

Şimdi uygulamanın UWP C# 'e özgü bölümünü tanımlayan kodu ekleyin.

1. **Çözüm Gezgini**' de, `helloworld.UWP` projesi altında `MainPage.xaml.cs` ' yi açın.

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Daha sonra, `helloworld.UWP` projesi altında yeni bir klasör @no__t `Services` için UWP 'e özgü uygulama ekleyin. Bundan sonra bu yeni C# kaynak dosyayı oluşturun ve `MicrophoneService.cs` olarak yeniden adlandırın ve aşağıdaki kod parçacığını bu dosyaya yapıştırın.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Ardından, Visual Studio içindeki `helloworld.UWP` projesi altında `Package.appxmanifest` dosyasına çift tıklayın **ve @no__t-** 3**mikrofon** denetlenir ve dosyayı kaydedin.
   > Not: bir uyarı görmeniz durumunda: sertifika dosyası yok: HelloWorld. Daha fazla bilgi için lütfen [konuşmayı metin](quickstart-csharp-uwp.md) örneğine göz atın.

1. Değişikliklerinizi kaydetmek için menü çubuğundan **dosya** > **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-uwp-application"></a>UWP uygulamasını derleme ve çalıştırma

1. @No__t-0 ' ı başlangıç projesi olarak ayarlayın ve `helloworld.UWP` projesinde fareyi kullanarak sağ tıklayın ve uygulamayı derlemek için **Oluştur** ' u seçin. 

1. Uygulamayı başlatmak için **hata ayıkla** > **hata ayıklamayı Başlat** (veya **F5**tuşuna basın) seçeneğini belirleyin. **HelloWorld** penceresi görüntülenir.

   ![Hızlı başlangıçta örnek UWP konuşma tanıma C# uygulaması](media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. **Konuşma tanımayı Başlat**' ı seçin ve cihazınızın mikrofonuna bir İngilizce tümcecik veya cümle konuşun. Konuşma, konuşma hizmetlerine iletilir ve pencerede görüntülenen metne gönderilir.

   ![Konuşma tanıma kullanıcı arabirimi](media/sdk/qs-csharp-xamarin-uwp-ui-result.png)

## <a name="build-and-run-the-android-and-ios-applications"></a>Android ve iOS uygulamalarını derleyin ve çalıştırın

Android ve iOS uygulamalarını cihazda veya benzeticide oluşturmak ve çalıştırmak UWP ile aynı şekilde gerçekleşir. Önemli, bu belgenin `Prerequisites` bölümünde tüm SDK 'ların doğru yüklendiğinden emin olmak için gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)
