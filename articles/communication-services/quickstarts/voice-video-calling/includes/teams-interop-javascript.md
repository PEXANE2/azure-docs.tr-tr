---
title: Hızlı başlangıç-bir takım toplantısına katılarak
author: chpalm
ms.author: mikben
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: c72083f205fae77de366125e666cee479fd46805
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691904"
---
## <a name="prerequisites"></a>Önkoşullar

- Uygulamayı çağıran, çalışan bir [Iletişim Hizmetleri](../getting-started-with-calling.md).
- Bir [takımlar dağıtımı](/deployoffice/teams-install).

## <a name="enable-teams-interoperability"></a>Takımların birlikte çalışabilirliğini etkinleştir

Takımlar birlikte çalışabilirliği özelliği şu anda özel önizlemededir. Iletişim Hizmetleri kaynağınız için bu özelliği etkinleştirmek üzere lütfen ile e-posta gönderin [acsfeedback@microsoft.com](mailto:acsfeedback@microsoft.com) :

1. Iletişim Hizmetleri kaynağınızı içeren Azure aboneliğinin abonelik KIMLIĞI.
2. Takımlarınızın kiracı KIMLIĞI. Bunu almanın en kolay yolu, [takımın bir bağlantısını edinmektir ve paylaşmalıdır](https://support.microsoft.com/office/create-a-link-or-a-code-for-joining-a-team-11b0de3b-9288-4cb4-bc49-795e7028296f).

Bu özelliği kullanmak için her iki varlığın sahip olduğu kuruluşun bir üyesi olmanız gerekir.

## <a name="add-the-teams-ui-controls"></a>Takımlar Kullanıcı arabirimi denetimlerini ekleme

index.html içindeki kodu aşağıdaki kod parçacığıyla değiştirin.
Metin kutusu, takımlar toplantı bağlamını girmek için kullanılacaktır ve bu düğme belirtilen toplantıya katılması için kullanılacak:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Communication Client - Calling Sample</title>
</head>
<body>
    <h4>Azure Communication Services</h4>
    <h1>Teams meeting join quickstart</h1>
    <input id="teams-link-input" type="text" placeholder="Teams meeting link"
        style="margin-bottom:1em; width: 300px;" />
        <p>Call state <span style="font-weight: bold" id="call-state">-</span></p>
    <div>
        <button id="join-meeting-button" type="button" disabled="false">
            Join Teams Meeting
        </button>
        <button id="hang-up-button" type="button" disabled="true">
            Hang Up
        </button>
    </div>
    <script src="./bundle.js"></script>
</body>

</html>
```

## <a name="enable-the-teams-ui-controls"></a>Takımlar Kullanıcı arabirimi denetimlerini etkinleştir

client.js dosyanın içeriğini aşağıdaki kod parçacığıyla değiştirin.

```javascript
import { CallClient } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
let callAgent;
const meetingLinkInput = document.getElementById('teams-link-input');
const hangUpButton = document.getElementById('hang-up-button');
const teamsMeetingJoinButton = document.getElementById('join-meeting-button');
const callStateElement = document.getElementById('call-state');

async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'ACS user'});
    teamsMeetingJoinButton.disabled = false;
}
init();

hangUpButton.addEventListener("click", async () => {
    // end the current call
    await call.hangUp();
  
    // toggle button states
    hangUpButton.disabled = true;
    teamsMeetingJoinButton.disabled = false;
    callStateElement.innerText = '-';
  });

teamsMeetingJoinButton.addEventListener("click", () => {    
    // join with meeting link
    call = callAgent.join({meetingLink: meetingLinkInput.value}, {});
    
    call.on('callStateChanged', () => {
        callStateElement.innerText = call.state;
    })
    // toggle button states
    hangUpButton.disabled = false;
    teamsMeetingJoinButton.disabled = true;
});
```

## <a name="get-the-teams-meeting-link"></a>Takımlar toplantı bağlantısını alın

Takımlar toplantı bağlantısı, Graph API 'Leri kullanılarak alınabilir. Bu, [Graph belgelerinde](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta)ayrıntılıdır.
SDK 'Yı çağıran Iletişim Hizmetleri, bir tam takımlar toplantısı bağlantısını kabul eder. Bu bağlantı, kaynağın bir parçası olarak döndürülür `onlineMeeting` , [ `joinWebUrl` özelliğin](/graph/api/resources/onlinemeeting?view=graph-rest-beta) altında erişilebilir, toplantı davetini davet eden ekipteki **Toplantı** URL 'sinden gerekli toplantı bilgilerini de alabilirsiniz.

## <a name="run-the-code"></a>Kodu çalıştırma

Web paketi kullanıcıları `webpack-dev-server` uygulamanızı derlemek ve çalıştırmak için kullanabilir. Uygulama ana bilgisayarınızı yerel bir Web sunucusuna paketetmek için aşağıdaki komutu çalıştırın:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Tarayıcınızı açın ve adresine gidin http://localhost:8080/ . Şunları görmeniz gerekir:

:::image type="content" source="../media/javascript/acs-join-teams-meeting-quickstart.PNG" alt-text="Tamamlanan JavaScript uygulamasının ekran görüntüsü.":::

Iletişim Hizmetleri uygulamanızın içinden takım toplantısına katmak için, metin kutusuna takımlar bağlamını ekleyin ve *takımlar toplantısına* Ekle ' ye basın.
