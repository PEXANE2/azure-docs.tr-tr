---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 02/20/2020
ms.author: dapine
ms.openlocfilehash: 2ee6b12923bfd0e06343e8f185226cb72280d806
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78383809"
---
Bu hızlı başlangıçta, konuşma özellikli bir ürün oluşturmak veya bir konuşma [dökümü](../conversation-transcription-service.md) cihazı olarak kullanmak için Android Için konuşma cihazları SDK 'sını nasıl kullanacağınızı öğreneceksiniz.

Bu kılavuzda, bir konuşma hizmeti kaynağına sahip bir Azure bilişsel [Hizmetler](../get-started.md) hesabı gerekir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

Örnek uygulama için kaynak kodu konuşma cihaz SDK'sı ile dahil edilir. [GitHub 'da da kullanılabilir](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Konuşma cihazları SDK 'sını kullanmaya başlamadan önce şunları yapmanız gerekir:

- Cihaza güç sağlamak için [geliştirme setiniz](../get-speech-devices-sdk.md) ile birlikte sunulan yönergeleri izleyin.

- [Konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download)en son sürümünü indirin ve. zip dosyasını çalışma dizininize ayıklayın.

  > [!NOTE]
  > Bu hızlı başlangıçta, uygulamanın C:\SDSDK\Android-Sample-Release 'e ayıklanacağı varsayılmaktadır

- [Konuşma hizmetine yönelik bir Azure abonelik anahtarı](../get-started.md) almak için

- Konuşma dökümünü kullanmayı planlıyorsanız, [döngüsel bir mikrofon cihazı](../get-speech-devices-sdk.md) kullanmanız gerekir ve bu özellik şu anda yalnızca "en-US" ve "zh-CN" bölgelerinde, "merkezde ABD" ve "eastasıya" olarak kullanılabilir. Konuşma dökümünü kullanmak için bu bölgelerden birinde bir konuşma anahtarınız olmalıdır.

- Kullanıcı dıklarından amaçları (veya eylemleri) belirlemek için konuşma hizmetini kullanmayı planlıyorsanız, bir [Language Understanding hizmeti (Luu)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) aboneliğine sahip olmanız gerekir. Lua ve amaç tanıma hakkında daha fazla bilgi edinmek için bkz. [lusıs C#Ile konuşma amaçlarını tanıma ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp).

  [Basit BIR luya modeli oluşturabilir](https://docs.microsoft.com/azure/cognitive-services/luis/) veya örnek Luo MODELI, Luu-example. JSON kullanabilirsiniz. Örnek LUSıS modeli, [konuşma cihazları SDK 'sı indirme sitesinden](https://aka.ms/sdsdk-luis)edinilebilir. Modelinizin JSON dosyasını [Lue portalına](https://www.luis.ai/home)yüklemek için **Yeni uygulama al**' ı seçin ve ardından json dosyasını seçin.

- [Android Studio](https://developer.android.com/studio/) ve [vysor](https://vysor.io/download/) 'u bilgisayarınıza yükleyip.

## <a name="set-up-the-device"></a>Cihazı ayarlama

1. Bilgisayarınızda Vysor başlatın.

   ![Vysor](../media/speech-devices-sdk/qsg-3.png)

1. Cihazınızın **cihaz Seç**altında listelenmesi gerekir. Cihazın yanındaki **Görünüm** düğmesini seçin.

1. Klasör simgesini seçerek kablosuz ağınıza bağlanın ve sonra da **WLAN** > **Ayarlar** ' ı seçin.

   ![Vysor WLAN](../media/speech-devices-sdk/qsg-4.png)

   > [!NOTE]
   > Şirketiniz kendi Wi-Fi sisteme cihazlar bağlama hakkında ilkeler varsa, MAC adresini alın ve şirketinizin Wi-Fi'a bağlayın konusunda BT departmanınıza başvurun gerekir.
   >
   > Dev Seti MAC adresini bulmak için dev Seti masaüstünde dosya klasör simgesini seçin.
   >
   > ![Vysor dosya klasörü](../media/speech-devices-sdk/qsg-10.png)
   >
   > **Ayarlar**' ı seçin. "MAC adresi" araması yapın ve ardından **GELIŞMIŞ WLAN** > **Mac adresi** ' ni seçin. İletişim kutusunun altına görünen MAC adresi yazın.
   >
   > ![Vysor MAC adresi](../media/speech-devices-sdk/qsg-11.png)
   >
   > Bazı şirketler, cihazın ne kadar kalabileceği üzerinde bir zaman sınırı olabilir, Wi-Fi sisteme bağlı. Wi-Fi sisteminizle dev Seti'nın kayıt belirli bir gün sayısından sonra genişletmek gerekebilir.

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Geliştirme Seti kurulumunuzu doğrulamak için örnek uygulamayı derleyin ve kurun:

1. Android Studio'yu başlatın.

1. **Var olan Android Studio projesini aç**'ı seçin.

   ![Android Studio - mevcut bir projeyi Aç](../media/speech-devices-sdk/qsg-5.png)

1. İçin C:\SDSDK\Android-Sample-Release\example gidin. Örnek projeyi açmak için **Tamam ' ı** seçin.

1. Gradle 'i konuşma SDK 'sına başvuracak şekilde yapılandırın. Aşağıdaki dosyalar Android Studio içindeki **Gradle betikler** altında bulunabilir.

    **Build. Gradle (proje: example)** öğesini güncelleştirin, Maven satırlarını ekleyerek allprojects bloğunun aşağıda eşleşmesi gerekir.

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

    Bu satırı bağımlılıklar bölümüne ekleyerek **Build. Gradle (Module: App)** öğesini güncelleştirin. 
    
    ```xml
    implementation'com.microsoft.cognitiveservices.speech:client-sdk:1.10.0'
    ```
    
1. Konuşma abonelik anahtarınızı kaynak koda ekleyin. Amaç tanımayı denemek istiyorsanız, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarınızı ve uygulama kimliğinizi da ekleyin.

   Konuşma ve LUSıS için, bilgileriniz MainActivity. Java içine gider:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   Konuşma dökümünü kullanıyorsanız konuşma anahtarınız ve bölge bilgileriniz de konuşmada de gereklidir. Java:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
   ```

1. Varsayılan anahtar sözcüğü "bilgisayar" dır. "Makine" veya "yardımcı" gibi, belirtilen diğer anahtar sözcüklerden birini de deneyebilirsiniz. Bu alternatif anahtar sözcüklerin kaynak dosyaları, anahtar sözcük klasöründe konuşma cihazları SDK 'snda bulunur. Örneğin, C:\SDSDK\Android-Sample-Release\keyword\Computer "bilgisayar" anahtar sözcüğü için kullanılan dosyaları içerir.

   > [!TIP]
   > Ayrıca, [özel bir anahtar sözcük oluşturabilirsiniz](../speech-devices-sdk-create-kws.md).

   Yeni bir anahtar sözcük kullanmak için `MainActivity.java`aşağıdaki iki satırı güncelleştirin ve anahtar sözcük paketini uygulamanıza kopyalayın. Örneğin, KWS-Machine. zip anahtar sözcük paketinden ' Machine ' anahtar sözcüğünü kullanmak için:

   - Anahtar sözcük paketini "C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\" klasörüne kopyalayın.
   - `MainActivity.java` anahtar sözcüğüyle ve paket adıyla güncelleştirin:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Mikrofon dizi geometri ayarları içeren aşağıdaki satırları güncelleştirin:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   Bu tabloda desteklenen değerler listelenmiştir:

   | Değişken | Anlamı | Kullanılabilir değerler |
   | -------- | ------- | ---------------- |
   | `DeviceGeometry` | Fiziksel MIC yapılandırma | Döngüsel bir geliştirici seti için: `Circular6+1` |
   |          |         | Doğrusal bir geliştirici seti için: `Linear4` |
   | `SelectedGeometry` | Yazılım MIC yapılandırması | Tüm mikler kullanan bir döngüsel geliştirici seti için: `Circular6+1` |
   |          |         | Dört miks kullanan dairesel bir geliştirici seti için: `Circular3+1` |
   |          |         | Tüm mikler kullanan doğrusal bir geliştirici seti için: `Linear4` |
   |          |         | İki mikü kullanan doğrusal bir geliştirici seti için: `Linear2` |

1. Uygulamayı derlemek için, **Çalıştır** menüsünde **' uygulama ' Çalıştır '** ı seçin. **Dağıtım hedefi seç** iletişim kutusu görüntülenir.

1. Cihazınızı seçin ve ardından uygulamayı cihaza dağıtmak için **Tamam** ' ı seçin.

   ![Dağıtım hedefi iletişim kutusunu seçin](../media/speech-devices-sdk/qsg-7.png)

1. Konuşma cihaz SDK'sı örnek bir uygulama başlar ve şu seçeneklerini gösterir:

   ![Örnek konuşma cihaz SDK'sı örnek uygulama ve seçenekleri](../media/speech-devices-sdk/qsg-8.png)

1. Yeni konuşma dökümü tanıtımı ' nı deneyin. ' Start Session ' ile bir kopyasını başlatın. Varsayılan olarak, herkes bir konudır. Ancak, katılımcının ses imzaları varsa, cihazdaki bir dosyaya `/video/participants.properties` yerleştirebilirsiniz. Ses imzasını oluşturmak için, [konuşmalar (SDK)](../how-to-use-conversation-transcription-service.md)konusuna bakın.

   ![Demo konuşma dökümü uygulaması](../media/speech-devices-sdk/qsg-15.png)

1. Deneyin!

## <a name="troubleshooting"></a>Sorun giderme

Konuşma cihazına bağlanamıyorsanız. Komut Istemi penceresinde aşağıdaki komutu yazın. Cihazların bir listesini döndürür:

```powershell
 adb devices
```

> [!NOTE]
> Bu komut Android Studio yüklemesinin parçası olan `adb.exe`Android Debug Bridge kullanır. Bu araç, C:\Users\[Kullanıcı adı] \AppData\Local\Android\Sdk\platform-tools. içinde bulunur Bu dizini, `adb`çağrılmasını daha kolay hale getirmek için yolunuza ekleyebilirsiniz. Aksi takdirde, `adb`çağıran her komutta ADB. exe yüklemenizin tam yolunu belirtmeniz gerekir.
>
> `no devices/emulators found` bir hata görürseniz, USB kablonuz bağlı olduğundan ve yüksek kaliteli bir kablonun kullanıldığından emin olun.
