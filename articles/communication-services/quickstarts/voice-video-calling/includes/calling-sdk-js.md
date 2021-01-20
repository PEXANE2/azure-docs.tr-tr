---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: c51316b445471c58ae88237b370f5a25fdedf373
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98605308"
---
## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Bir `User Access Token` çağrı istemcisini etkinleştirmek için. [Nasıl yapılır `User Access Token` ](../../access-tokens.md) hakkında daha fazla bilgi edinmek için
- İsteğe bağlı: [uygulamanıza çağrı ekleme ile çalışmaya](../getting-started-with-calling.md) başlama için hızlı başlangıcı doldurun

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`npm install`JavaScript Için Azure Iletişim Hizmetleri 'Ni çağıran ve ortak istemci kitaplıklarını yüklemek için komutunu kullanın.

```console
npm install @azure/communication-common --save

npm install @azure/communication-calling --save

```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler:

| Ad                             | Açıklama                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | CallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.                                                                       |
| CallAgent                        | CallAgent, çağrıları başlatmak ve yönetmek için kullanılır.                                                                                            |
| AzureCommunicationUserCredential | AzureCommunicationUserCredential sınıfı, CallAgent örneğini oluşturmak için kullanılan CommunicationUserCredential arabirimini uygular. |


## <a name="initialize-the-callclient-create-callagent-and-access-devicemanager"></a>CallClient 'ı başlatın, CallAgent oluşturun ve DeviceManager 'a erişin

Yeni bir `CallClient` örnek oluşturun. Bunu, bir günlükçü örneği gibi özel seçeneklerle yapılandırabilirsiniz.
Bir, örneği oluşturulduktan sonra `CallClient` `CallAgent` örnek üzerinde yöntemini çağırarak bir örnek oluşturabilirsiniz `createCallAgent` `CallClient` . Bu, zaman uyumsuz bir `CallAgent` örnek nesnesi döndürüyor.
`createCallAgent`Yöntemi bir `CommunicationUserCredential` [Kullanıcı erişim belirtecini](../../access-tokens.md)kabul eden bir bağımsız değişken olarak alır.
Erişim için `DeviceManager` önce bir callAgent örneğine erişmek gerekir. Daha sonra, `getDeviceManager` `CallClient` devmanager 'ı almak için örnek üzerinde yöntemini kullanabilirsiniz.

```js
const userToken = '<user token>';
callClient = new CallClient(options);
const tokenCredential = new AzureCommunicationUserCredential(userToken);
const callAgent = await callClient.createCallAgent(tokenCredential, { displayName: 'optional ACS user name' });
const deviceManager = await callClient.getDeviceManager()
```

## <a name="place-an-outgoing-call"></a>Giden bir çağrı yerleştir

Bir çağrı oluşturmak ve başlatmak için, CallAgent 'da API 'lerden birini kullanmanız ve Iletişim Hizmetleri Yönetimi istemci kitaplığı aracılığıyla oluşturduğunuz bir Kullanıcı sağlamanız gerekir.

Çağrı oluşturma ve başlatma zaman uyumludur. Çağrı örneği, çağrı olaylarına abone olmanızı sağlar.

## <a name="place-a-11-call-to-a-user-or-a-1n-call-with-users-and-pstn"></a>Kullanıcılara 1:1 çağrısı veya Kullanıcı ve PSTN ile 1: n çağrısı koyun

Başka bir Iletişim Hizmetleri kullanıcısına çağrı yerleştirmek için, `call` yöntemi çağırın `callAgent` ve [iletişim Hizmetleri yönetim kitaplığıyla oluşturduğunuz](../../access-tokens.md)communicationuser öğesini geçirin.

```js
const oneToOneCall = callAgent.call([CommunicationUser]);
```

### <a name="place-a-1n-call-with-users-and-pstn"></a>Kullanıcılar ve PSTN ile 1: n çağrısı yerleştir

Bir kullanıcıya 1: n çağrısı ve bir PSTN numarası eklemek için, bir CommunicationUser ve her iki Cali için telefon numarası belirtmeniz gerekir.

Iletişim Hizmetleri kaynağınız, PSTN çağırmaya izin verecek şekilde yapılandırılmalıdır.
```js

const userCallee = { communicationUserId: <ACS_USER_ID> };
const pstnCallee = { phoneNumber: <PHONE_NUMBER>};
const groupCall = callAgent.call([userCallee, pstnCallee], placeCallOptions);

```

### <a name="place-a-11-call-with-video-camera"></a>Video kamera ile 1:1 çağrısı yerleştirme
> [!WARNING]
> Şu anda birden fazla giden yerel video akışı yok.
Bir video araması yerleştirmek için, deviceManager API 'sini kullanarak yerel kameraları listeleyebilirsiniz `getCameraList` .
İstediğiniz kamerayı seçtikten sonra, bir `LocalVideoStream` örnek oluşturmak ve `videoOptions` dizi içinde yöntemine bir öğe olarak geçirmek için onu kullanın `localVideoStream` `call` .
Çağrınızdan bağlantı kurulduktan sonra, seçili kameradan diğer katılımcılara otomatik olarak bir video akışı göndermeye başlar.

Bu ayrıca Call. Accept () video seçenekleri ve CallAgent. JOIN () video seçenekleri için de geçerlidir.
```js
const deviceManager = await callClient.getDeviceManager();
const videoDeviceInfo = deviceManager.getCameraList()[0];
localVideoStream = new LocalVideoStream(videoDeviceInfo);
const placeCallOptions = {videoOptions: {localVideoStreams:[localVideoStream]}};
const call = callAgent.call(['acsUserId'], placeCallOptions);

```

### <a name="receiving-an-incoming-call"></a>Gelen bir çağrı alınıyor
```js
callAgent.on('callsUpdated', e => {
    e.added.forEach(addedCall => {
        if(addedCall.isIncoming) {
        addedCall.accept();
    }
    });
})
```

### <a name="join-a-group-call"></a>Grup çağrısına katılır
Yeni bir grup çağrısı başlatmak veya devam eden bir grup çağrısına katmak için, ' JOIN ' metodunu kullanın ve bir özelliği olan bir nesne geçirin `groupId` . Değer bir GUID olmalıdır.
```js

const locator = { groupId: <GUID>}
const call = callAgent.join(locator);

```

### <a name="join-a-teams-meeting"></a>Takımlar toplantısına katılarak
Bir ekip toplantısına katmak için ' JOIN ' yöntemini kullanın ve bir toplantı bağlantısı veya bir toplantının koordinatlarını geçirin
```js
// Join using meeting link
const locator = { meetingLink: <meeting link>}
const call = callAgent.join(locator);

// Join using meeting coordinates
const locator = {
    threadId: <thread id>,
    organizerId: <organizer id>,
    tenantId: <tenant id>,
    messageId: <message id>
}
const call = callAgent.join(locator);
```

## <a name="call-management"></a>Arama Yönetimi

Arama özelliklerine erişebilir ve video ve sesle ilgili ayarları yönetme çağrısı sırasında çeşitli işlemler gerçekleştirebilirsiniz.

### <a name="call-properties"></a>Çağrı özellikleri
* Bu çağrının benzersiz KIMLIĞINI (dize) alın.
```js

const callId: string = call.id;

```

* Çağrısındaki diğer katılımcılar hakkında bilgi edinmek için, `remoteParticipant` örneği üzerinde koleksiyonu inceleyin `call` . Dizi, liste `RemoteParticipant` nesneleri içeriyor
```js
const remoteParticipants = call.remoteParticipants;
```

* Çağrı gelen, çağıran kimliği. Kimlik `Identifier` türlerden biri
```js

const callerIdentity = call.callerIdentity;

```

* Çağrının durumunu alın.
```js

const callState = call.state;

```
Bu, çağrının geçerli durumunu temsil eden bir dize döndürür:
* ' None '-ilk çağrı durumu
* ' Gelen '-bir çağrının geliş olduğunu, kabul edildiğini veya reddedildiğini belirtir
* ' Bağlanıyor '-çağrı yerleştirildiğinde veya kabul edildiğinde ilk geçiş durumu
* ' Çalıyor '-giden bir çağrı için-uzak katılımcılar için bir çağrının çaldırdığını, bu kişinin tarafında ' gelen ' olduğunu belirtir
* ' EarlyMedia '-çağrı bağlanmadan önce bir duyurunun yürütüldüğü durumu belirtir
* ' Connected '-çağrı bağlı
* ' Hold '-çağrı beklemeye koyuyor, Yerel uç nokta ve uzak katılımcı arasında medya akışı yok
* Çağrının ' bağlantısı kesildi ' durumuna geçmeden önce ' bağlantısı kesiliyor '-geçiş durumu
* ' Bağlantısı kesildi '-son çağrı durumu.
   * Ağ bağlantısı kaybolursa, durum yaklaşık 2 dakika sonra ' bağlantısı kesildi ' durumuna geçer.


* Belirli bir çağrının neden bitmediğini görmek için, `callEndReason` özelliği inceleyin.
```js

const callEndReason = call.callEndReason;
// callEndReason.code (number) code associated with the reason
// callEndReason.subCode (number) subCode associated with the reason
```

* Geçerli çağrının gelen bir çağrı olup olmadığını öğrenmek için, özelliğini inceleyin ve `isIncoming` döndürür `Boolean` .
```js
const isIncoming = call.isIncoming;
```

* Çağrının kaydedilip kaydedilmediğini denetlemek için, özelliğini inceleyin ve `isRecordingActive` döndürür `Boolean` .
```js
const isResordingActive = call.isRecordingActive;
```

*  Geçerli mikrofonun kapalı olup olmadığını denetlemek için, özelliğini inceleyin ve `muted` döndürür `Boolean` .
```js

const muted = call.isMicrophoneMuted;

```

* Ekran paylaşım akışının belirli bir uç noktadan gönderilip gönderilmediğini görmek için, özelliği iade edin `isScreenSharingOn` `Boolean` .
```js

const isScreenSharingOn = call.isScreenSharingOn;

```

* Etkin video akışlarını denetlemek için, koleksiyonu, `localVideoStreams` `LocalVideoStream` nesneleri içerir
```js

const localVideoStreams = call.localVideoStreams;

```

### <a name="mute-and-unmute"></a>Sessiz ve aç

Yerel uç noktanın sesini kapatmak veya sesini açmak için `mute` ve `unmute` zaman uyumsuz API 'leri kullanabilirsiniz:

```js

//mute local device 
await call.mute();

//unmute local device 
await call.unmute();

```

### <a name="start-and-stop-sending-local-video"></a>Yerel video göndermeyi başlatma ve durdurma


Bir videoyu başlatmak için nesne üzerindeki metodunu kullanarak kameraları numaralandırabilirsiniz gerekir `getCameraList` `deviceManager` . Ardından `LocalVideoStream` istenen kamerayı `startVideo` bir bağımsız değişken olarak yönteme geçirmenin yeni bir örneğini oluşturun:


```js
const localVideoStream = new LocalVideoStream(videoDeviceInfo);
await call.startVideo(localVideoStream);

```

Videoyu göndermeye başarıyla başladıktan sonra, bir `LocalVideoStream` `localVideoStreams` çağrı örneği üzerinde koleksiyona bir örnek eklenecektir.

```js

call.localVideoStreams[0] === localVideoStream;

```

Yerel videoyu durdurmak için, `localVideoStream` örneği koleksiyonda kullanılabilir olarak geçirin `localVideoStreams` :

```js

await call.stopVideo(localVideoStream);

```

Bir örneği çağırarak video gönderilirken farklı bir kamera cihazına geçiş yapabilirsiniz `switchSource` `localVideoStream` :

```js
const source callClient.getDeviceManager().getCameraList()[1];
localVideoStream.switchSource(source);

```
### <a name="faq"></a>SSS
 * Ağ bağlantısı kaybolursa, çağrı durumu ' bağlantısız ' olarak değişir mi?
    * Evet, ağ bağlantısı 2 dakikadan fazla kaybolursa, çağrı bağlantısı kesik duruma geçer ve çağrı sona bırakılır.

## <a name="remote-participants-management"></a>Uzak katılımcılar yönetimi

Tüm uzak katılımcılar, tür ile temsil edilir `RemoteParticipant` ve `remoteParticipants` bir çağrı örneği üzerinde koleksiyon aracılığıyla kullanılabilir.

### <a name="list-participants-in-a-call"></a>Bir çağrıda katılımcıları listeleme
`remoteParticipants`Koleksiyon verilen çağrıda uzak katılımcılar listesini döndürür:

```js

call.remoteParticipants; // [remoteParticipant, remoteParticipant....]

```

### <a name="remote-participant-properties"></a>Uzak katılımcı özellikleri
Uzak katılımcının ilişkili özellikler ve koleksiyonlar kümesi vardır

* Bu uzak katılımcının tanımlayıcısını alın.
Kimlik ' Identifier ' türlerinden biridir:
```js
const identifier = remoteParticipant.identifier;
//It can be one of:
// { communicationUserId: '<ACS_USER_ID'> } - object representing ACS User
// { phoneNumber: '<E.164>' } - object representing phone number in E.164 format
```

* Bu uzak katılımcının durumunu alın.
```js

const state = remoteParticipant.state;
```
Durum aşağıdakilerden biri olabilir
* ' Boşta '-ilk durum
* Katılımcı çağrıya bağlanırken ' bağlanıyor '-geçiş durumu
* ' Connected '-katılımcı çağrıya bağlı
* ' Hold '-katılımcı beklemeye açık
* ' EarlyMedia '-katılımcı çağrıya bağlanmadan önce duyuru yürütülür
* ' Bağlantısı kesildi '-son durum-katılımcının çağrı bağlantısı kesildi.
   * Uzak katılımcı ağ bağlantılarını kaybederse, uzak katılımcı durumu yaklaşık 2 dakika sonra ' bağlantısı kesildi ' durumuna geçer.

Katılımcının çağrıyı neden bıraktı olduğunu öğrenmek için, `callEndReason` özelliği inceleyin:
```js

const callEndReason = remoteParticipant.callEndReason;
// callEndReason.code (number) code associated with the reason
// callEndReason.subCode (number) subCode associated with the reason
```

* Bu uzak katılımcının kapalı olup olmadığını denetlemek için, `isMuted` özelliği incele, döndürüyor `Boolean`
```js
const isMuted = remoteParticipant.isMuted;
```

* Bu uzak katılımcının konuşuyor olup olmadığını denetlemek için, `isSpeaking` döndürdüğü özelliği inceleyin `Boolean`
```js

const isSpeaking = remoteParticipant.isSpeaking;

```

* Belirli bir katılımcının Bu çağrıda gönderdiği tüm video akışlarını denetlemek için, `videoStreams` koleksiyon, `RemoteVideoStream` nesneleri içerir
```js

const videoStreams = remoteParticipant.videoStreams; // [RemoteVideoStream, ...]

```


### <a name="add-a-participant-to-a-call"></a>Çağrıya katılımcı ekleme

Çağrıya (bir kullanıcı veya telefon numarası) bir katılımcı eklemek için, çağırabilirsiniz `addParticipant` .
' Identifier ' türlerinden birini sağlayın.
Bu, uzak katılımcı örneğini zaman uyumlu olarak döndürür.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>};
const remoteParticipant = call.addParticipant(userIdentifier);
const remoteParticipant = call.addParticipant(pstnIdentifier);
```

### <a name="remove-participant-from-a-call"></a>Katılımcıyı bir çağrıdan kaldır

Bir katılımcıyı bir çağrıdan (Kullanıcı veya telefon numarası) kaldırmak için, çağırabilirsiniz `removeParticipant` .
' Tanımlayıcı ' türlerinden birini geçirmeniz gerekir, katılımcı çağrıdan kaldırıldıktan sonra bu, zaman uyumsuz olarak çözümlenir.
Katılımcı de `remoteParticipants` Koleksiyondan kaldırılacak.

```js
const userIdentifier = { communicationUserId: <ACS_USER_ID> };
const pstnIdentifier = { phoneNumber: <PHONE_NUMBER>};
await call.removeParticipant(userIdentifier);
await call.removeParticipant(pstnIdentifier);
```

## <a name="render-remote-participant-video-streams"></a>Uzak katılımcı video akışlarını işle

Uzak katılımcıların video akışlarını ve ekran paylaşım akışlarını listelemek için, `videoStreams` koleksiyonları inceleyin:

```js
const remoteVideoStream: RemoteVideoStream = call.remoteParticipants[0].videoStreams[0];
const streamType: MediaStreamType = remoteVideoStream.type;
```
 
A 'yı işlemek için `RemoteVideoStream` bir olaya abone olmanız gerekir `isAvailableChanged` .
`isAvailable`Özelliği olarak değişirse `true` , uzak katılımcı bir akış gönderiyor.
Bu durumda, yeni bir örneğini oluşturun `Renderer` ve ardından `RendererView` zaman uyumsuz yöntemi kullanarak yeni bir örnek oluşturun `createView` .  Daha sonra `view.target` herhangi bir kullanıcı arabirimi öğesine iliştirebilirsiniz.
Bir uzak akış değişikliklerinin kullanılabilirliği her kullanıldığında, tüm oluşturucuyu yok edebilir veya tek tek seçebilirsiniz `RendererView` , ancak bu, boş video çerçevesinin görüntülenmesine neden olur.

```js
let renderer: Renderer;
const displayVideo = () => {
    renderer = new Renderer(remoteParticipantStream);
    const view = await renderer.createView();
    htmlElement.appendChild(view.target);
}
remoteParticipantStream.on('availabilityChanged', async () => {
    if (remoteParticipantStream.isAvailable) {
        displayVideo();
    } else {
        renderer.dispose();
    }
});
if (remoteParticipantStream.isAvailable) {
    displayVideo();
}
```

### <a name="remote-video-stream-properties"></a>Uzak video akışı özellikleri
Uzak video akışları aşağıdaki özelliklere sahiptir:

* `Id` -Uzak video akışının KIMLIĞI
```js
const id: number = remoteVideoStream.id;
```

* `StreamSize` -uzak video akışının boyutu (genişlik/yükseklik)
```js
const size: {width: number; height: number} = remoteVideoStream.size;
```

* `MediaStreamType` -' video ' veya ' ScreenSharing ' olabilir
```js
const type: MediaStreamType = remoteVideoStream.type;
```
* `isAvailable` -Uzak katılımcı uç noktasının akışı etkin bir şekilde gönderiyor olduğunu belirtir
```js
const type: boolean = remoteVideoStream.isAvailable;
```

### <a name="renderer-methods-and-properties"></a>İşleyici yöntemleri ve özellikleri

* `RendererView`Uzak video akışını işlemek için uygulama kullanıcı arabirimine daha sonra eklenebilecek bir örnek oluşturun.
```js
renderer.createView()
```

* Oluşturucuyu ve tüm ilişkili `RendererView` örnekleri atın.
```js
renderer.dispose()
```


### <a name="rendererview-methods-and-properties"></a>RendererView yöntemleri ve özellikleri
Bir oluştururken `RendererView` , `scalingMode` ve `mirrored` özelliklerini belirtebilirsiniz.
Ölçeklendirme modu ' Esnetme ', ' Kırp ' veya ' fit ' olabilir `Mirrored` . belirtilirse, işlenen akış dikey olarak çevrilcektir.

```js
const rendererView: RendererView = renderer.createView({ scalingMode, mirrored });
```
Verilen herhangi `RendererView` bir örnek, `target` işleme yüzeyini temsil eden bir özelliğe sahiptir. Bu, uygulama kullanıcı arabirimine eklenmelidir:
```js
document.body.appendChild(rendererView.target);
```

Daha sonra yöntemini çağırarak ölçekleme modunu güncelleştirebilirsiniz `updateScalingMode` .
```js
view.updateScalingMode('Crop')
```
### <a name="faq"></a>SSS
* Uzak bir katılımcı ağ bağlantılarını kaybederse, durumları ' bağlantısı kesildi ' olarak değişir mi?
    * Evet, uzak bir katılımcı ağ bağlantılarını 2 dakikadan uzun bir süre kaybederse, bu durum kesilmeden geçiş yapılır ve çağrıdan kaldırılır.
## <a name="device-management"></a>Cihaz yönetimi

`DeviceManager` ses/video akışlarınızı iletmek için bir çağrıda kullanılabilecek yerel cihazları listelemenizi sağlar. Ayrıca, yerel tarayıcı API 'sini kullanarak bir kullanıcıdan mikrofona ve kameraya erişim izni talep etmenizi sağlar.

`deviceManager`Çağırarak, çağıran `callClient.getDeviceManager()` yöntemine erişebilirsiniz.
> [!WARNING]
> Şu anda `callAgent` DeviceManager 'a erişim kazanmak için önce bir nesne örneği oluşturulmalıdır

```js

const deviceManager = await callClient.getDeviceManager();

```

### <a name="enumerate-local-devices"></a>Yerel cihazları listeleme

Yerel cihazlara erişmek için Aygıt Yöneticisi numaralandırma yöntemlerini kullanabilirsiniz. Sabit listesi, zaman uyumlu bir işlemdir.

```js

//  Get a list of available video devices for use.
const localCameras = deviceManager.getCameraList(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
const localMicrophones = deviceManager.getMicrophoneList(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
const localSpeakers = deviceManager.getSpeakerList(); // [AudioDeviceInfo, AudioDeviceInfo...]

```

### <a name="set-default-microphonespeaker"></a>Varsayılan mikrofonu/konuşmacıyı ayarla

Aygıt Yöneticisi, bir çağrı başlatılırken kullanılacak varsayılan bir cihaz ayarlamanıza olanak sağlar.
İstemci Varsayılanları ayarlanmamışsa, Iletişim Hizmetleri işletim sistemi varsayılanlarına geri döner.

```js

// Get the microphone device that is being used.
const defaultMicrophone = deviceManager.getMicrophone();

// Set the microphone device to use.
await deviceMicrophone.setMicrophone(AudioDeviceInfo);

// Get the speaker device that is being used.
const defaultSpeaker = deviceManager.getSpeaker();

// Set the speaker device to use.
await deviceManager.setSpeaker(AudioDeviceInfo);

```

### <a name="local-camera-preview"></a>Yerel kamera önizlemesi

`DeviceManager` `Renderer` Yerel kameranızdan akışları işlemeye başlamak için ve kullanabilirsiniz. Bu akış diğer katılımcılara gönderilmez; Bu, yerel bir önizleme akışımıza sahiptir. Bu, zaman uyumsuz bir eylemdir.

```js
const localVideoDevice = deviceManager().getCameraList()[0];
const localCameraStream = new LocalVideoStream(localVideoDevice);
const renderer = new Renderer(localCameraStream);
const view = await renderer.createView();
document.body.appendChild(view.target);

```

### <a name="request-permission-to-cameramicrophone"></a>Kamera/mikrofona izin iste

Kullanıcıdan kameraya kamera/mikrofon izinleri vermesini iste:

```js
const result = await deviceManager.askDevicePermission(audio: true, video: true);
```
Bu, `audio` ve izinlerinin verildiğini belirten bir nesne ile zaman uyumsuz olarak çözümlenir `video` :
```js
console.log(result.audio);
console.log(result.video);
```

Şunu çağırarak, belirli bir tür için geçerli izin durumunu inceleyebilirsiniz `getPermissionState` :

```js

const result = deviceManager.getPermissionState('Microphone'); // for microphone permission state
const result = deviceManager.getPermissionState('Camera'); // for camera permission state

console.log(result); // 'Granted' | 'Denied' | 'Prompt' | 'Unknown';

```

## <a name="eventing-model"></a>Olay modeli

Değerler değiştiğinde bildirilmesi için özelliklerin ve koleksiyonların çoğuna abone olabilirsiniz.

### <a name="properties"></a>Özellikler
Olaylara abone olmak için `property changed` :

```js

const eventHandler = () => {
    // check current value of a property, value is not passed to callback
    console.log(object.property);
};
object.on('propertyNameChanged',eventHandler);

// To unsubscribe:

object.off('propertyNameChanged',eventHandler);

```

### <a name="collections"></a>Koleksiyonlar
Olaylara abone olmak için `collection updated` :

```js

const eventHandler = (e) => {
    // check added elements
    console.log(e.added);
    // check removed elements
    console.log(e.removed);
};
object.on('collectionNameUpdated',eventHandler);

// To unsubscribe:

object.off('collectionNameUpdated',eventHandler);

```
