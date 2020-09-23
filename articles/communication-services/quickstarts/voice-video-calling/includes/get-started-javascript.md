---
title: Hızlı başlangıç-Azure Iletişim hizmetlerini kullanarak bir Web uygulamasına çağıran VOıP ekleme
description: Bu öğreticide, JavaScript için istemci kitaplığı 'nı çağıran Azure Iletişim hizmetlerini kullanmayı öğreneceksiniz
author: ddematheu
ms.author: nimag
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: d58b4d86936c56a08f27bef59edc1d3cc4ce4617
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948345"
---
Bu hızlı başlangıçta, JavaScript için istemci kitaplığı 'nı çağıran Azure Iletişim hizmetlerini kullanarak nasıl çağrı başlayacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/) Etkin LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
- Etkin bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Arama istemcisini başlatmak için bir Kullanıcı erişim belirteci. [Kullanıcı erişim belirteçleri oluşturmayı ve yönetmeyi](../../access-tokens.md)öğrenin.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

```console
mkdir calling-quickstart && cd calling-quickstart
```

`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

### <a name="install-the-package"></a>Paketi yükler

`npm install`JavaScript için istemci kitaplığı 'Nı çağıran Azure Iletişim hizmetlerini yüklemek için komutunu kullanın.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Bu hızlı başlangıç, uygulama varlıklarını paketleyip Web Pack kullanır. WebPack, WebPack-CLI ve WebPack-dev-Server NPM paketlerini yüklemek için aşağıdaki komutu çalıştırın ve bunları ** üzerindepackage.js**geliştirme bağımlılıkları olarak listeleyin:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Projenizin kök dizininde bir **index.html** dosyası oluşturun. Bu dosyayı, kullanıcının bir Azure Iletişim bot 'a çağrı yerleştirmesini sağlayacak temel bir düzen yapılandırmak için kullanacağız.

Kod aşağıdaki gibidir:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Bu hızlı başlangıç için uygulama mantığını içeren **client.js** adlı projenizin kök dizininde bir dosya oluşturun. Çağıran istemciyi içeri aktarmak ve iş mantığımızı ekleyebilmemiz için DOM öğelerine başvurular almak için aşağıdaki kodu ekleyin. 

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");

// quickstart code goes here
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler:

| Ad                             | Açıklama                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | CallClient, çağıran istemci kitaplığı için ana giriş noktasıdır.                                                                       |
| CallAgent                        | CallAgent, çağrıları başlatmak ve yönetmek için kullanılır.                                                                                            |
| AzureCommunicationUserCredential | AzureCommunicationUserCredential sınıfı, CallAgent örneğini oluşturmak için kullanılan CommunicationUserCredential arabirimini uygular. |


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`<USER_ACCESS_TOKEN>`Kaynağınız için geçerli bir Kullanıcı erişim belirteci ile değiştirmeniz gerekir. Kullanılabilir bir belirteciniz yoksa, [Kullanıcı erişim belirteci](../../access-tokens.md) belgelerine başvurun. Kullanarak `CallClient` , `CallAgent` bir örneği başlatın, bu, `CommunicationUserCredential` çağrısı yapıp almamızı sağlar. Aşağıdaki kodu **client.js**ekleyin:

```javascript
const callClient = new CallClient();
const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
let callAgent;

callClient.createCallAgent(tokenCredential).then(agent => {
  callAgent = agent;
  callButton.disabled = false;
});
```

## <a name="start-a-call"></a>Bir çağrı başlatın

Tıklandığında bir çağrı başlatmak için bir olay işleyicisi ekleyin `callButton` :

```javascript
callButton.addEventListener("click", () => {
    // start a call
    const userToCall = calleeInput.value;
    call = callAgent.call(
        [{ communicationUserId: userToCall }],
        {}
    );
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```

## <a name="end-a-call"></a>Çağrıyı Sonlandır

Tıklandığında geçerli çağrıyı sonlandırmak için bir olay dinleyicisi ekleyin `hangUpButton` :

```javascript
hangUpButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({ forEveryone: true });

  // toggle button states
  hangUpButton.disabled = true;
  callButton.disabled = false;
});
```

`forEveryone`Özelliği tüm çağrı katılımcıları için çağrıyı sonlandırır.

## <a name="run-the-code"></a>Kodu çalıştırma

`webpack-dev-server`Uygulamanızı derlemek ve çalıştırmak için kullanın. ' Deki uygulama konağını yerel bir Web sunucusuna paketetmek için aşağıdaki komutu çalıştırın:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Tarayıcınızı açın ve adresine gidin http://localhost:8080/ . Şunları görmeniz gerekir:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Tamamlanan JavaScript uygulamasının ekran görüntüsü.":::

Metin alanına bir kullanıcı KIMLIĞI sağlayarak ve **çağrıya başla** düğmesine tıklayarak gıden bir VoIP çağrısı yapabilirsiniz. Çağırmak `8:echo123` sizi bir Echo bot ile bağlar, bu, başlamak ve ses cihazlarınızın çalıştığını doğrulamak için harika bir yoldur.
