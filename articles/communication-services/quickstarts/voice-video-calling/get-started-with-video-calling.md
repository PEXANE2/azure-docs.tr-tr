---
title: Hızlı başlangıç-uygulamanıza video çağırma ekleme (JavaScript)
titleSuffix: An Azure Communication Services quickstart
description: Bu hızlı başlangıçta, Azure Iletişim Hizmetleri 'ni kullanarak uygulamanıza nasıl video çağırma özelliği ekleneceğini öğreneceksiniz.
author: xumo-95
ms.author: mikben
ms.date: 07/24/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 00721a5c7763663a17b1e67cf0ac3ae59fd9448a
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102204575"
---
# <a name="quickstart-add-11-video-calling-to-your-app-javascript"></a>Hızlı başlangıç: uygulamanıza 1:1 video çağrısı ekleme (JavaScript)

## <a name="prerequisites"></a>Önkoşullar
- Etkin abonelikle bir Azure hesabı edinin. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/en/) Etkin LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1)
- Etkin bir Iletişim Hizmetleri kaynağı oluşturun. [Iletişim Hizmetleri kaynağı oluşturun](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp).
- Arama istemcisini başlatmak için bir Kullanıcı erişim belirteci oluşturun. [Kullanıcı erişim belirteçleri oluşturmayı ve yönetmeyi öğrenin](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-csharp).

## <a name="setting-up"></a>Ayarlanıyor
### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma
Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.
```console
mkdir calling-quickstart && cd calling-quickstart
```
### <a name="install-the-package"></a>Paketi yükler
`npm install`JavaScript için istemci kitaplığı 'Nı çağıran Azure Iletişim hizmetlerini yüklemek için komutunu kullanın.

Bu hızlı başlangıç, Istemci kitaplığı 'nı çağıran Azure Iletişimini kullandı `1.0.0.beta-6` . 

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```
### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Bu hızlı başlangıç, uygulama varlıklarını paketleyip Web Pack kullanır. `webpack`Ve NPM paketlerini yüklemek için aşağıdaki komutu `webpack-cli` çalıştırın `webpack-dev-server` ve bunları bir geliştirme bağımlılıkları olarak listeleyin `package.json` :

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```
`index.html`Projenizin kök dizininde bir dosya oluşturun. Bu dosyayı, kullanıcının bir 1:1 video araması yerleştirmesini sağlayacak temel bir düzen yapılandırmak için kullanacağız.

Kod şu şekildedir:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - 1:1 Video Calling Sample</title>
</head>

<body>
    <h4>Azure Communication Services</h4>
    <h1>1:1 Video Calling Quickstart</h1>
    <input 
    id="callee-id-input"
    type="text"
    placeholder="Who would you like to call?"
    style="margin-bottom:1em; width: 200px;"
    />

    <div>
    <button id="call-button" type="button" disabled="true">
        start call
    </button>
        &nbsp;
    <button id="hang-up-button" type="button" disabled="true">
        hang up
    </button>
        &nbsp;
    <button id="start-Video" type="button" disabled="true">
        start video
    </button>
        &nbsp;
    <button id="stop-Video" type="button" disabled="true">
        stop video
    </button>     
    </div>

    <div>Local Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;">
      <div id="myVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>     
    </div>
    <div>Remote Video</div>
    <div style="height:200px; width:300px; background-color:black; position:relative;"> 
      <div id="remoteVideo" style="background-color: black; position:absolute; top:50%; transform: translateY(-50%);">
      </div>
    </div>

    <script src="./bundle.js"></script>
</body>
</html>
```

`client.js`Bu hızlı başlangıç için uygulama mantığını içeren adlı projenizin kök dizininde bir dosya oluşturun. Çağıran istemciyi içeri aktarmak ve DOM öğelerine başvuru almak için aşağıdaki kodu ekleyin.

```JavaScript
import { CallClient, CallAgent, Renderer, LocalVideoStream } from "@azure/communication-calling";
import { AzureCommunicationTokenCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
const stopVideoButton = document.getElementById("stop-Video");
const startVideoButton = document.getElementById("start-Video");

let placeCallOptions;
let deviceManager;
let localVideoStream;
let rendererLocal;
let rendererRemote;
```
## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler:

| Ad      | Açıklama | 
| :---        |    :----   |
| CallClient  | CallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.      |
| CallAgent  | CallAgent, çağrıları başlatmak ve yönetmek için kullanılır.        |
| DeviceManager | DeviceManager, medya cihazlarını yönetmek için kullanılır.    |
| AzureCommunicationTokenCredential | AzureCommunicationTokenCredential sınıfı, CallAgent örneğini oluşturmak için kullanılan CommunicationTokenCredential arabirimini uygular.        |

## <a name="authenticate-the-client-and-access-devicemanager"></a>İstemci kimliğini doğrulama ve DeviceManager 'a erişme

<USER_ACCESS_TOKEN>, kaynağınız için geçerli bir Kullanıcı erişim belirteci ile değiştirmeniz gerekir. Kullanılabilir bir belirteciniz yoksa, Kullanıcı erişim belirteci belgelerine başvurun. CallClient 'ı kullanarak bir CallAgent örneğini, çağrı yapıp almamızı sağlayacak bir CommunicationUserCredential ile başlatın. DeviceManager 'a erişmek için önce bir callAgent örneği oluşturulmalıdır. Daha sonra öğesini `getDeviceManager` `CallClient` almak için örneğinde yöntemini kullanabilirsiniz `DeviceManager` .

Aşağıdaki kodu `client.js` dosyasına ekleyin:

```JavaScript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationTokenCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, { displayName: 'optional ACS user name' });
    
    deviceManager = await callClient.getDeviceManager();
    callButton.disabled = false;
}
init();
```
## <a name="place-a-11-outgoing-video-call-to-a-user"></a>Kullanıcıya 1:1 giden bir video çağrısı yerleştir

Tıklandığında bir çağrı başlatmak için bir olay dinleyicisi ekleyin `callButton` :

İlk olarak, deviceManager getCameraList API 'sini kullanarak yerel kameraları numaralandırabilirsiniz. Bu hızlı başlangıçta koleksiyondaki ilk kamerayı kullanıyoruz. İstenen kamera seçildikten sonra, bir LocalVideoStream örneği oluşturulur ve bu, video seçenekleri içinde localVideoStream dizisinin içindeki bir öğe olarak çağrı yöntemine geçirilir. Çağrlarınız bağlandıktan sonra otomatik olarak diğer katılımcıya bir video akışı göndermeye başlar. 

```JavaScript
callButton.addEventListener("click", async () => {
    const videoDevices = await deviceManager.getCameras();
    const videoDeviceInfo = videoDevices[0];
    localVideoStream = new LocalVideoStream(videoDeviceInfo);
    placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};

    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;

    const userToCall = calleeInput.value;
    call = callAgent.startCall(
        [{ communicationUserId: userToCall }],
        placeCallOptions
    );

    subscribeToRemoteParticipantInCall(call);

    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```  
Bir oluşturmak için `LocalVideoStream` Yeni bir örneği oluşturmanız `Renderer` ve ardından zaman uyumsuz yöntemi kullanarak yeni bir RendererView örneği oluşturmanız gerekir `createView` . Daha sonra `view.target` herhangi bir kullanıcı arabirimi öğesine iliştirebilirsiniz. 

```JavaScript
async function localVideoView() {
    rendererLocal = new Renderer(localVideoStream);
    const view = await rendererLocal.createView();
    document.getElementById("myVideo").appendChild(view.target);
}
```
Tüm uzak katılımcılar, `remoteParticipants` bir çağrı örneğindeki koleksiyon aracılığıyla kullanılabilir. Geçerli çağrının uzak katılımcılarına abone olmanız ve `remoteParticipantsUpdated` eklenen uzak katılımcılara abone olmak için olayı dinlemek gerekir.

```JavaScript
function subscribeToRemoteParticipantInCall(callInstance) {
    callInstance.remoteParticipants.forEach( p => {
        subscribeToRemoteParticipant(p);
    })
    callInstance.on('remoteParticipantsUpdated', e => {
        e.added.forEach( p => {
            subscribeToRemoteParticipant(p);
        })
    });   
}
```
`remoteParticipants`Geçerli çağrının koleksiyonuna abone olabilir ve `videoStreams` her katılımcının akışlarını listelemek için koleksiyonları inceleyebilirsiniz. Ayrıca, eklenen uzak katılımcıları işlemek için remoteParticipantsUpdated olayına abone olmanız gerekir. 

```JavaScript
function subscribeToRemoteParticipant(remoteParticipant) {
    remoteParticipant.videoStreams.forEach(v => {
        handleVideoStream(v);
    });
    remoteParticipant.on('videoStreamsUpdated', e => {
        e.added.forEach(v => {
            handleVideoStream(v);
        })
    });
}
```
Öğesini işlemek için bir olaya abone olmanız gerekir `isAvailableChanged` `remoteVideoStream` . `isAvailable`Özelliği olarak değişirse `true` , uzak katılımcı bir akış gönderiyor. Uzak bir akış değişikliklerinin kullanılabilirliği her kullanıldığında, belirli bir veya tek tek yok etme seçeneğini belirleyebilir `Renderer` `RendererView` , ancak bu, boş video çerçevesinin görüntülenmesine neden olur.
```JavaScript
function handleVideoStream(remoteVideoStream) {
    remoteVideoStream.on('availabilityChanged', async () => {
        if (remoteVideoStream.isAvailable) {
            remoteVideoView(remoteVideoStream);
        } else {
            rendererRemote.dispose();
        }
    });
    if (remoteVideoStream.isAvailable) {
        remoteVideoView(remoteVideoStream);
    }
}
```
Bir oluşturmak için `RemoteVideoStream` Yeni bir örneği oluşturmanız `Renderer` ve ardından `RendererView` zaman uyumsuz yöntemi kullanarak yeni bir örnek oluşturmanız gerekir `createView` . Daha sonra `view.target` herhangi bir kullanıcı arabirimi öğesine iliştirebilirsiniz. 

```JavaScript
async function remoteVideoView(remoteVideoStream) {
    rendererRemote = new Renderer(remoteVideoStream);
    const view = await rendererRemote.createView();
    document.getElementById("remoteVideo").appendChild(view.target);
}
```
## <a name="receive-an-incoming-call"></a>Gelen bir çağrı al
Olayını dinlemek için gereken gelen çağrıları işlemek için `incomingCall` `callAgent` . Gelen bir çağrı olduktan sonra, yerel kameraları numaralandırın ve `LocalVideoStream` diğer katılımcıya video akışı göndermek için bir örnek oluşturmanız gerekir. Ayrıca, `remoteParticipants` uzak video akışlarını işlemek için hizmetine abone olmanız gerekir. Örneğin çağrısını kabul edebilir veya reddedebilirsiniz `incomingCall` . 

`init()`Gelen çağrıları işlemek için uygulamayı içine koyun. 

```JavaScript
callAgent.on('incomingCall', async e => {
    const videoDevices = await deviceManager.getCameras();
    const videoDeviceInfo = videoDevices[0];
    localVideoStream = new LocalVideoStream(videoDeviceInfo);
    localVideoView();

    stopVideoButton.disabled = false;
    callButton.disabled = true;
    hangUpButton.disabled = false;

    const addedCall = await e.incomingCall.accept({videoOptions: {localVideoStreams:[localVideoStream]}});
    call = addedCall;

    subscribeToRemoteParticipantInCall(addedCall);   
});
```
## <a name="end-the-current-call"></a>Geçerli çağrıyı Sonlandır
Tıklandığında geçerli çağrıyı sonlandırmak için bir olay dinleyicisi ekleyin `hangUpButton` :
```JavaScript
hangUpButton.addEventListener("click", async () => {
    // dispose of the renderers
    rendererLocal.dispose();
    rendererRemote.dispose();
    // end the current call
    await call.hangUp();
    // toggle button states
    hangUpButton.disabled = true;
    callButton.disabled = false;
    stopVideoButton.disabled = true;
});
```
## <a name="subscribe-to-call-updates"></a>Çağrı güncelleştirmelerine abone olma
Uzak katılımcı, video oluşturucularının atılmasına ve iki durumlu düğme durumlarına yönelik çağrıyı sonlandırıyorsa olaya abone olmanız gerekir. 

Olaya abone olmak için uygulamayı Init () içine koyun `callsUpdated` . 
 ```JavaScript 
callAgent.on('callsUpdated', e => {
    e.removed.forEach(removedCall => {
        // dispose of video renders
        rendererLocal.dispose();
        rendererRemote.dispose();
        // toggle button states
        hangUpButton.disabled = true;
        callButton.disabled = false;
        stopVideoButton.disabled = true;
    })
})
```

## <a name="start-and-end-video-during-the-call"></a>Çağrı sırasında videoyu Başlat ve bitir
Geçerli çağrı sırasında videoyu Durdur düğmesine bir olay dinleyicisi ekleyerek ve işleyicisini atmak için durdurabilirsiniz `localVideoStream` . 
 ```JavaScript       
stopVideoButton.addEventListener("click", async () => {
    await call.stopVideo(localVideoStream);
    rendererLocal.dispose();
    startVideoButton.disabled = false;
    stopVideoButton.disabled = true;
});
```
Videoyu Başlat düğmesine bir olay dinleyicisi ekleyerek, geçerli çağrı sırasında durdurulduğunda videoyu tekrar açabilirsiniz. 
```JavaScript
startVideoButton.addEventListener("click", async () => {
    await call.startVideo(localVideoStream);
    localVideoView();
    stopVideoButton.disabled = false;
    startVideoButton.disabled = true;
});
```
## <a name="run-the-code"></a>Kodu çalıştırma
`webpack-dev-server`Uygulamanızı derlemek ve çalıştırmak için kullanın. ' Deki uygulama konağını yerel bir Web sunucusuna paketetmek için aşağıdaki komutu çalıştırın:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Tarayıcınızı açın ve adresine gidin http://localhost:8080/ . Şunları görmeniz gerekir:

:::image type="content" source="./media/javascript/1-on-1-video-calling.png" alt-text="1 video arama sayfasında 1":::

Metin alanına bir kullanıcı KIMLIĞI sağlayarak ve çağrıya başla düğmesine tıklayarak 1:1 giden bir video araması yapabilirsiniz. 

## <a name="sample-code"></a>Örnek Kod
Örnek uygulamayı [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/Add%201%20on%201%20video%20calling)'dan indirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](https://docs.microsoft.com/azure/communication-services/quickstarts/create-communication-resource?tabs=windows&pivots=platform-azp#clean-up-resources)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- [Web çağırma](https://docs.microsoft.com/azure/communication-services/samples/web-calling-sample) örneğimize göz atın
- [İstemci kitaplığı yeteneklerini çağırma](https://docs.microsoft.com/azure/communication-services/quickstarts/voice-video-calling/calling-client-samples?pivots=platform-web) hakkında bilgi edinin
- [Nasıl çalıştığını çağırma](https://docs.microsoft.com/azure/communication-services/concepts/voice-video-calling/about-call-types) hakkında daha fazla bilgi edinin
