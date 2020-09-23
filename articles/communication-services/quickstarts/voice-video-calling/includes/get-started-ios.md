---
title: Hızlı başlangıç-Azure Iletişim hizmetlerini kullanarak bir iOS uygulamasına çağrı ekleme
description: Bu hızlı başlangıçta iOS için istemci kitaplığı 'nı çağıran Azure Iletişim hizmetlerini kullanmayı öğreneceksiniz.
author: matthewrobertson
ms.author: marobert
ms.date: 07/24/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: c67440453e5ca8395464369d75bfac418a564764
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948318"
---
Bu hızlı başlangıçta, iOS için istemci kitaplığı 'nı çağıran Azure Iletişim Hizmetleri 'ni kullanarak nasıl çağrı başlayacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Anahtarınıza yüklenmiş geçerli bir geliştirici sertifikasıyla birlikte [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532)çalıştıran bir Mac.
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Azure Iletişim hizmetiniz için bir [Kullanıcı erişim belirteci](../../access-tokens.md) .

## <a name="setting-up"></a>Ayarlanıyor

### <a name="creating-the-xcode-project"></a>Xcode projesi oluşturma

Xcode 'da yeni bir iOS projesi oluşturun ve **tek görünüm uygulama** şablonunu seçin. Bu öğretici [swiftuı çerçevesini](https://developer.apple.com/xcode/swiftui/)kullanır, bu nedenle **dili** **Swift** ve **Kullanıcı arabirimine** **swiftuı**olarak ayarlamanız gerekir. Bu hızlı başlangıç sırasında birim testleri veya UI testleri oluşturuyoruz. **Birim testlerini ekleme** ve ayrıca **UI testlerini dahil**etme onay işaretini kaldırmayı ücretsiz olarak göz atın.

:::image type="content" source="../media/ios/xcode-new-ios-project.png" alt-text="Xcode 'da yeni yeni proje oluştur penceresini gösteren ekran görüntüsü.":::

### <a name="install-the-package"></a>Paketi yükler

İstemci kitaplığı ve bağımlılıklarını (AzureCore. Framework ve AzureCommunication. Framework) çağıran Azure Iletişim hizmetlerini projenize ekleyin.

> [!NOTE]
> AzureCommunicationCalling SDK sürümü ile bir bash betiği bulacaksınız `BuildAzurePackages.sh` . Çalıştırma sırasında betik, bir `sh ./BuildAzurePackages.sh` sonraki adımda örnek uygulamada içeri aktarılması gereken oluşturulan çerçeve paketlerinin yolunu verecektir. Betiği çalıştırmadan önce bunu yapmadıysanız Xcode komut satırı araçlarını ayarlamanız gerekeceğini unutmayın: Xcode 'u başlatın, "Tercihler-> konumları" öğesini seçin. Komut satırı araçları için Xcode sürümünüzü seçin.

1. İOS için istemci kitaplığı 'nı çağıran Azure Iletişim Hizmetleri 'ni indirin.
2. Xcode 'da proje dosyanıza tıklayın ve proje ayarları düzenleyicisini açmak için derleme hedefini seçin.
3. **Genel** sekmesinde, **çerçeveler, kitaplıklar ve katıştırılmış içerik** bölümüne gidin ve **"+"** simgesine tıklayın.
4. İletişim kutusunun sol alt kısmında **Dosya Ekle**' yi seçin, daraltılmış istemci kitaplığı paketinin **AzureCommunicationCalling. Framework** dizinine gidin.
    1. **Azurecore. Framework** ve **AzureCommunication. Framework**eklemek için son adımı yineleyin.
5. Proje ayarları düzenleyicisinin **derleme ayarları** sekmesini açın ve **arama yolları** bölümüne gidin. **AzureCommunicationCalling. Framework**içeren dizin için yeni bir **çerçeve arama yolları** girişi ekleyin.
    1. Bağımlılıkları içeren klasöre işaret eden başka bir Framework arama yolları girişi ekleyin.

:::image type="content" source="../media/ios/xcode-framework-search-paths.png" alt-text="XCode içindeki çerçeve arama yollarının güncelleştirilmesini gösteren ekran görüntüsü.":::

### <a name="request-access-to-the-microphone"></a>Mikrofona erişim isteyin

Cihazın mikrofonuna erişmek için uygulamanızın bilgi Özellik listesini bir ile güncelleştirmeniz gerekir `NSMicrophoneUsageDescription` . İlişkili değeri, `string` sistemin kullanıcıdan istek erişimi istemek için kullandığı iletişim kutusuna dahil edilecek bir öğesine ayarlarsınız.

`Info.plist`Proje ağacının girişine sağ tıklayın ve kaynak kodu **olarak aç**' ı seçin  >  **Source Code**. Aşağıdaki satırları en üst düzey bölümüne ekleyin `<dict>` ve dosyayı kaydedin.

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for VOIP calling.</string>
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Projenizin **ContentView. Swift** dosyasını açın ve `import` dosyanın en üstüne içeri aktarmak için bir bildirim ekleyin `AzureCommunicationCalling library` . Bunlara ek olarak, `AVFoundation` Bu kodda ses izni isteği için de ihtiyacımız olacak.

```swift
import AzureCommunicationCalling
import AVFoundation
```

Yapının uygulamasını, bir `ContentView` kullanıcının bir çağrıyı başlatmasını ve bitibilmesini sağlayan bazı basıt UI denetimleriyle değiştirin. Bu hızlı başlangıçta bu denetimlere iş mantığı ekleyeceğiz.

```swift
struct ContentView: View {
    @State var callee: String = ""
    @State var callClient: ACSCallClient?
    @State var callAgent: ACSCallAgent?
    @State var call: ACSCall?

    var body: some View {
        NavigationView {
            Form {
                Section {
                    TextField("Who would you like to call?", text: $callee)
                    Button(action: startCall) {
                        Text("Start Call")
                    }.disabled(callAgent == nil)
                    Button(action: endCall) {
                        Text("End Call")
                    }.disabled(call == nil)
                }
            }
            .navigationBarTitle("Calling Quickstart")
        }.onAppear {
            // Initialize call agent
        }
    }

    func startCall() {
        // Ask permissions
        AVAudioSession.sharedInstance().requestRecordPermission { (granted) in
            if granted {
                // Add start call logic
            }
        }
    }

    func endCall() {
        // Add end call logic
    }
}
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler:

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ACSCallClient | CallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.|
| ACSCallAgent | CallAgent, çağrıları başlatmak ve yönetmek için kullanılır. |
| CommunicationUserCredential | CommunicationUserCredential, CallAgent örneğini oluşturmak için belirteç kimlik bilgileri olarak kullanılır.| 
| CommunicationIndentifier | CommunicationIndentifier, aşağıdakilerden biri olabilecek Kullanıcı kimliğini temsil etmek için kullanılır: CommunicationUser/PhoneNumber/Callinguygulaması. |

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Bir `CallAgent` Kullanıcı erişim belirtecine sahip bir örnek başlatın, bu da çağrı yapıp almamızı sağlar. Aşağıdaki kodu `onAppear` **ContentView. Swift**içindeki geri aramaya ekleyin:

```swift
var userCredential: CommunicationUserCredential?
do {
    userCredential = try CommunicationUserCredential(token: "<USER ACCESS TOKEN>")
} catch {
    print("ERROR: It was not possible to create user credential.")
    return
}

self.callClient = ACSCallClient()

// Creates the call agent
self.callClient?.createCallAgent(userCredential) { (agent, error) in
    if error != nil {
        print("ERROR: It was not possible to create a call agent.")
    }

    if let agent = agent {
        self.callAgent = agent
        print("Call agent successfully created.")
    }
}
```

`<USER ACCESS TOKEN>`Kaynağınız için geçerli bir Kullanıcı erişim belirteci ile değiştirmeniz gerekir. Kullanılabilir bir belirteciniz yoksa, [Kullanıcı erişim belirteci](../../access-tokens.md) belgelerine başvurun.

## <a name="start-a-call"></a>Bir çağrı başlatın

`startCall`Yöntemi, *çağrıya başla* düğmesine dokunduğunda gerçekleştirilecek eylem olarak ayarlanır. Şunu içeren bir çağrı başlatmak için uygulamayı güncelleştirin `ASACallAgent` :

```swift
func startCall()
{
    // Ask permissions
    AVAudioSession.sharedInstance().requestRecordPermission { (granted) in
        if granted {
            // start call logic
            let callees:[CommunicationIdentifier] = [CommunicationUser(identifier: self.callee)]
            self.call = self.callAgent?.call(callees, options: ACSStartCallOptions())
        }
    }
}
```

Ayrıca, `ACSStartCallOptions` çağrının başlangıç seçeneklerini ayarlamak için içindeki özellikleri de kullanabilirsiniz (yani, mikrofonun sesini kapalı olarak başlatmaya izin verir).

## <a name="end-a-call"></a>Çağrıyı Sonlandır

`endCall` *Son çağrı* düğmesine dokunduğunda geçerli çağrıyı sonlandırmak için yöntemini uygulayın.

```swift
func endCall()
{    
    self.call!.hangup(ACSHangupOptions()) { (error) in
        if (error != nil) {
            print("ERROR: It was not possible to hangup the call.")
        }
    }
}
```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamanızı iOS simülatörü üzerinde, **ürün**  >  **çalıştırması** ' nı seçerek veya (&#8984;-R) klavye kısayolunu kullanarak oluşturabilirsiniz.

:::image type="content" source="../media/ios/quick-start-make-call.png" alt-text="Hızlı başlangıç uygulamasına yönelik nihai görünüm":::

Metin alanına bir kullanıcı KIMLIĞI sağlayarak ve **çağrıya başla** düğmesine dokunarak gıden bir VoIP çağrısı yapabilirsiniz. Çağırmak `8:echo123` sizi bir Echo bot ile bağlar, bu, başlamak ve ses cihazlarınızın çalıştığını doğrulamak için harika bir yoldur. 

> [!NOTE]
> İlk kez bir çağrı yaptığınızda sistem mikrofona erişim ister. Bir üretim uygulamasında API 'yi kullanarak `AVAudioSession` [izin durumunu denetleyin](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources) ve izin verilmediği zaman uygulamanızın davranışını dikkatlice güncelleştirebilirsiniz.

## <a name="sample"></a>Örnek

Örnek uygulamayı [GitHub](https://github.com/Azure/Communication/tree/master/samples/AzureCommunicationCalling/iOS/Swift) 'dan indirebilirsiniz
