---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 02/20/2020
ms.author: dapine
ms.openlocfilehash: 4dbaaad4b75e6d706d77522f45a2d596c49bea79
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80659267"
---
Bu hızlı başlangıçta, konuşma özellikli bir ürün oluşturmak veya [Konuşma Transkripsiyon](../conversation-transcription-service.md) aygıtı olarak kullanmak için Android için Konuşma Aygıtları SDK'yı nasıl kullanacağınızı öğreneceksiniz.

Bu kılavuz, Konuşma hizmeti kaynağına sahip bir [Azure Bilişsel Hizmetler](../get-started.md) hesabı gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

Örnek uygulamanın kaynak kodu Konuşma Aygıtları SDK'ya dahildir. [GitHub'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK)da mevcuttur.

## <a name="prerequisites"></a>Ön koşullar

Konuşma Aygıtları SDK'yı kullanmaya başlamadan önce şunları yapmanız gerekir:

- Cihazda güç sağlamak için [geliştirme kitiile](../get-speech-devices-sdk.md) birlikte verilen yönergeleri izleyin.

- [Konuşma Aygıtları SDK'nın](https://aka.ms/sdsdk-download)en son sürümünü indirin ve .zip'i çalışma dizininize ayıklayın.

  > [!NOTE]
  > Bu hızlı başlatma, uygulamanın C:\SDSDK\Android-Örnek-Sürüm'e çıkarılan

- [Konuşma hizmeti için Azure abonelik anahtarı](../get-started.md) almak için

- Konuşma Transkripsiyon'u kullanmayı [planlıyorsanız, dairesel](../get-speech-devices-sdk.md) bir mikrofon cihazı kullanmanız gerekir ve bu özellik şu anda yalnızca bölgeler,"centralus" ve "eastasia" bölgelerinde yalnızca "en-US" ve "zh-CN" için kullanılabilir. Konuşma Transkripsiyon'u kullanmak için bu bölgelerden birinde bir konuşma anahtarına sahip olmalısınız.

- Konuşma hizmetini, kullanıcı söylemlerinden gelen niyetleri (veya eylemleri) tanımlamak için kullanmayı planlıyorsanız, bir [Dil Anlama Hizmeti (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) aboneliğine ihtiyacınız vardır. LUIS ve niyet tanıma hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp)

  Basit [bir LUIS modeli oluşturabilir](https://docs.microsoft.com/azure/cognitive-services/luis/) veya luis-example.json örneklerini kullanabilirsiniz. Örnek LUIS modeli Konuşma [Cihazları SDK indirme sitesinden](https://aka.ms/sdsdk-luis)edinilebilir. Modelinizin JSON dosyasını [LUIS portalına](https://www.luis.ai/home)yüklemek için yeni **uygulamayı içe aktar'ı**seçin ve ardından JSON dosyasını seçin.

- [Android Studio](https://developer.android.com/studio/) ve [Vysor'u](https://vysor.io/download/) bilgisayarınıza yükleyin.

## <a name="set-up-the-device"></a>Cihazı ayarlama

1. Vysor'u bilgisayarınızda başlatın.

   ![Vysor](../media/speech-devices-sdk/qsg-3.png)

1. Cihazınız bir aygıt **seçin**altında listelenmelidir. Aygıtın yanındaki **Görünüm** düğmesini seçin.

1. Klasör simgesini seçerek kablosuz ağınıza bağlanın ve ardından **Ayarlar** > **WLAN'ı**seçin.

   ![Vysor WLAN](../media/speech-devices-sdk/qsg-4.png)

   > [!NOTE]
   > Şirketinizin cihazları Wi-Fi sistemine bağlama yla ilgili politikaları varsa, MAC adresini almanız ve şirketinizin Wi-Fi sistemine nasıl bağlayabileceğiniz konusunda BT departmanınızla iletişime geçmeniz gerekir.
   >
   > Dev kitinin MAC adresini bulmak için, dev kitinin masaüstündeki dosya klasörü simgesini seçin.
   >
   > ![Vysor dosya klasörü](../media/speech-devices-sdk/qsg-10.png)
   >
   > **Ayarlar'ı**seçin. "Mac adresi" arama ve ardından **Mac adresini** > **seçin Gelişmiş WLAN**. İletişim kutusunun altında görünen MAC adresini yazın.
   >
   > ![Vysor MAC adresi](../media/speech-devices-sdk/qsg-11.png)
   >
   > Bazı şirketlerin, bir aygıtın Wi-Fi sistemine ne kadar süreyle bağlanabileceği konusunda bir zaman sınırı olabilir. Geliştirme kitinin Wi-Fi sisteminize kaydını belirli bir sayıdan sonra genişletmeniz gerekebilir.

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Geliştirme kiti kurulumunuzu doğrulamak için örnek uygulamayı oluşturun ve kurun:

1. Android Studio'yı başlatın.

1. **Var olan Android Studio projesini aç**'ı seçin.

   ![Android Studio - Mevcut bir projeyi açın](../media/speech-devices-sdk/qsg-5.png)

1. C:\SDSDK\Android-Örnek-Sürüm\örnek gidin. Örnek projeyi açmak için **Tamam'ı** seçin.

1. Konuşma SDK'ya başvurmak için gradle yapılandırın. Aşağıdaki dosyalar Android **Studio'da Gradle Scripts** altında bulunabilir.

    **Build.gradle(Project:example)** güncelleştirin, tüm projeler bloğu maven satırlarını ekleyerek aşağıdaki yle eşleşmelidir.

    ```xml
    allprojects {
        repositories {
            google()
            jcenter()
            mavenCentral()
            maven {
                url 'https://csspeechstorage.blob.core.windows.net/maven/'
            }
        }
    }
    ```

    Bu satırı bağımlılıklar bölümüne ekleyerek **build.gradle(Modül:app)** güncelleştirin. 
    
    ```xml
    implementation'com.microsoft.cognitiveservices.speech:client-sdk:1.11.0'
    ```
    
1. Konuşma aboneliği anahtarınızı kaynak koduna ekleyin. Niyet tanımayı denemek istiyorsanız, [Dil Bilgisi hizmet](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarınızı ve uygulama kimliğinizi de ekleyin.

   Konuşma ve LUIS için bilgileriniz MainActivity.java'ya girer:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   Konuşma transkripsiyonunu kullanıyorsanız, konuşma anahtarınız ve bölge bilgileriniz de conversation.java'da gereklidir:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
   ```

1. Varsayılan anahtar kelime "Bilgisayar"dır. Ayrıca, "Makine" veya "Yardımcı" gibi sağlanan diğer anahtar kelimelerden birini de deneyebilirsiniz. Bu alternatif anahtar kelimelerin kaynak dosyaları Konuşma Aygıtları SDK'da, anahtar kelime klasöründe bulunmaktadır. Örneğin, C:\SDSDK\Android-Örnek-Sürüm\anahtar kelime\Bilgisayar "Bilgisayar" anahtar kelimesi için kullanılan dosyaları içerir.

   > [!TIP]
   > [Ayrıca özel bir anahtar kelime oluşturabilirsiniz.](../speech-devices-sdk-create-kws.md)

   Yeni bir anahtar kelime kullanmak için aşağıdaki `MainActivity.java`iki satırı güncelleştirin ve anahtar kelime paketini uygulamanızda kopyalayın. Örneğin, anahtar kelime paketi kws-machine.zip gelen 'Makine' anahtar sözcüğlerini kullanmak için:

   - Anahtar kelime paketini "C:\SDSDK\Android-Örnek-Yayın\örnek\app\src\main\assets\" klasörüne kopyalayın.
   - Anahtar `MainActivity.java` kelime ve paket adı ile güncelleştirin:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Mikrofon dizisi geometri si ayarlarını içeren aşağıdaki satırları güncelleştirin:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   Bu tablo desteklenen değerleri listeler:

   | Değişken | Anlamı | Kullanılabilir değerler |
   | -------- | ------- | ---------------- |
   | `DeviceGeometry` | Fiziksel mikrofon yapılandırması | Dairesel dev kiti için:`Circular6+1` |
   |          |         | Doğrusal dev kiti için:`Linear4` |
   | `SelectedGeometry` | Yazılım mikrofon yapılandırması | Tüm mikrofonları kullanan dairesel bir dev kiti için:`Circular6+1` |
   |          |         | Dört mikrofon kullanan dairesel bir dev kiti için:`Circular3+1` |
   |          |         | Tüm mikrofonları kullanan doğrusal dev kiti için:`Linear4` |
   |          |         | İki mikrofon kullanan doğrusal dev kiti için:`Linear2` |

1. Uygulamayı oluşturmak için **Çalıştır** menüsünde **Çalıştır 'uygulaması'nı**seçin. **Dağıtım Hedefini Seç** iletişim kutusu görüntülenir.

1. Cihazınızı seçin ve uygulamayı aygıta dağıtmak için **Tamam'ı** seçin.

   ![Dağıtım Hedefi iletişim kutusunu seçin](../media/speech-devices-sdk/qsg-7.png)

1. Konuşma Aygıtları SDK örnek uygulaması başlar ve aşağıdaki seçenekleri görüntüler:

   ![Örnek Konuşma Cihazları SDK örnek uygulama ve seçenekleri](../media/speech-devices-sdk/qsg-8.png)

1. Yeni Konuşma Transkripsiyon demosu deneyin. 'Başlangıç Oturumu' ile yazıya aktarmaya başlayın. Varsayılan olarak herkes bir konuk. Ancak, katılımcının ses imzaları varsa, bunlar aygıttaki `/video/participants.properties` bir dosyaya konulabilir. Ses imzasını oluşturmak [için, Transcribe konuşmalarına (SDK)](../how-to-use-conversation-transcription-service.md)bakın.

   ![Demo Konuşma Transkripsiyon uygulaması](../media/speech-devices-sdk/qsg-15.png)

1. Deney!

## <a name="troubleshooting"></a>Sorun giderme

Konuşma Aygıtına bağlanamıyorsanız. Komut İstemi penceresine aşağıdaki komutu yazın. Aygıtların listesini döndürecek:

```powershell
 adb devices
```

> [!NOTE]
> Bu komut, `adb.exe`Android Studio yüklemesinin bir parçası olan Android Hata Ayıklama Köprüsü'nü kullanır. Bu araç C:\Users\[kullanıcı adı]\AppData\Local\Android\Sdk\platform-tools adresinde bulunur. Bu dizini, çağırmak `adb`için daha uygun hale getirmek için yolunuza ekleyebilirsiniz. Aksi takdirde, çağıran her komutta adb.exe yüklemeniziçin tam `adb`yolu belirtmeniz gerekir.
>
> Bir hata `no devices/emulators found` görürseniz, USB kablonuzun bağlı olup olmadığını kontrol edin ve yüksek kaliteli bir kablo kullanıldığından emin olun.
