---
title: Hızlı başlangıç-bir takım toplantısına katılarak
author: chpalm
ms.author: mikben
ms.date: 10/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: dd75e5e97e5dca898ba10e91528782861fb949ec
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114654"
---
## <a name="prerequisites"></a>Ön koşullar

- Uygulamayı çağıran, çalışan bir [Iletişim Hizmetleri](../getting-started-with-calling.md).
- Bir [takımlar dağıtımı](https://docs.microsoft.com/deployoffice/teams-install).

## <a name="enable-teams-interoperability"></a>Takımların birlikte çalışabilirliğini etkinleştir

Takımlar birlikte çalışabilirliği özelliği şu anda özel önizlemededir. Iletişim Hizmetleri kaynağınız için bu özelliği etkinleştirmek üzere lütfen ile e-posta gönderin [acsfeedback@microsoft.com](mailto:acsfeedback@microsoft.com) :

1. Iletişim Hizmetleri kaynağınızı içeren Azure aboneliğinin abonelik KIMLIĞI.
2. Takımlarınızın kiracı KIMLIĞI. Bunu almanın en kolay yolu, [takımın bir bağlantısını edinmektir ve paylaşmalıdır](https://support.microsoft.com/office/create-a-link-or-a-code-for-joining-a-team-11b0de3b-9288-4cb4-bc49-795e7028296f#:~:text=Create%20a%20link%20If%20you%E2%80%99re%20a%20team%20owner%2C,link%20into%20any%20browser%20to%20join%20the%20team).

Bu özelliği kullanmak için her iki varlığın sahip olduğu kuruluşun bir üyesi olmanız gerekir.

## <a name="add-the-teams-ui-controls"></a>Takımlar Kullanıcı arabirimi denetimlerini ekleme

HTML 'niz içine yeni bir metin kutusu ve düğme ekleyin. Metin kutusu, takımlar toplantı bağlamını girmek için kullanılacaktır ve bu düğme belirtilen toplantıya katılması için kullanılacak:

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
    <input 
      id="teams-id-input"
      type="text"
      placeholder="Teams meeting context"
      style="margin-bottom:1em; width: 300px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
         <button id="meeting-button" type="button" disabled="false">
        Join Teams Meeting
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

## <a name="enable-the-teams-ui-controls"></a>Takımlar Kullanıcı arabirimi denetimlerini etkinleştir

Şimdi **takım toplantısına katılma** düğmesini, sunulan takımlar toplantısına birleştiren koda bağlayabiliriz:

```javascript
meetingButton.addEventListener("click", () => {
    
    // join with meeting link
    call = callAgent.join({meetingLink: 'MEETING_LINK'}, {});

     // join with meeting coordinates
     call = callAgent.join({
        threadId: 'CHAT_THREAD_ID',
        organizerId: 'ORGANIZER_ID',
        tenantId: 'TENANT_ID',
        messageId: 'MESSAGE_ID'
    }, {})
    
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
    meetingButton.disabled = true;
});
```

## <a name="get-the-meeting-context"></a>Toplantı bağlamını al

Takımlar bağlamı, Graph API 'Leri kullanılarak alınabilir. Bu, [Graph belgelerinde](https://docs.microsoft.com/graph/api/onlinemeeting-createorget?view=graph-rest-beta&tabs=http)ayrıntılıdır.

Ayrıca, Toplantı daveti ' nda **Toplantı** davet URL 'sinden gerekli toplantı bilgilerini de alabilirsiniz.

## <a name="run-the-code"></a>Kodu çalıştırma

Web paketi kullanıcıları `webpack-dev-server` uygulamanızı derlemek ve çalıştırmak için kullanabilir. Uygulama ana bilgisayarınızı yerel bir Web sunucusuna paketetmek için aşağıdaki komutu çalıştırın:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Tarayıcınızı açın ve adresine gidin http://localhost:8080/ . Şunları görmeniz gerekir:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Tamamlanan JavaScript uygulamasının ekran görüntüsü.":::

Iletişim Hizmetleri uygulamanızın içinden takım toplantısına katmak için, metin kutusuna takımlar bağlamını ekleyin ve *takımlar toplantısına* Ekle ' ye basın.

