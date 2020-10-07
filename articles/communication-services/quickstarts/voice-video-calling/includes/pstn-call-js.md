---
author: nikuklic
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/11/2020
ms.author: nikuklic
ms.openlocfilehash: 009bd57fdb82b8463352da8dc63c9aeebceab09b
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779165"
---
[!INCLUDE [Emergency Calling Notice](../../../includes/emergency-calling-notice-include.md)]
## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- Iletişim Hizmetleri kaynağında alınan telefon numarası. [telefon numarası alma](../../telephony-sms/get-phone-number.md).
- Bir `User Access Token` çağrı istemcisini etkinleştirmek için. [Nasıl yapılır `User Access Token` ](../../access-tokens.md) hakkında daha fazla bilgi edinmek için
- [Uygulamanıza çağrı ekleme ile çalışmaya](../getting-started-with-calling.md) başlama için hızlı başlangıcı doldurun

### <a name="prerequisite-check"></a>Önkoşul denetimi

- Iletişim Hizmetleri kaynağınız ile ilişkili telefon numaralarını görüntülemek için [Azure Portal](https://portal.azure.com/)oturum açın, iletişim hizmetleri kaynağınızı bulun ve sol gezinti bölmesinden **telefon numaraları** sekmesini açın.
- JavaScript için istemci kitaplığı 'nı çağıran Azure Iletişim Hizmetleri ile uygulamanızı oluşturabilir ve çalıştırabilirsiniz:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js
```

## <a name="setting-up"></a>Ayarlanıyor

### <a name="add-pstn-functionality-to-your-app"></a>Uygulamanıza PSTN işlevselliği ekleyin

Telefon arama denetimleriyle düzeninizi genişletin.

Bu kodu `<body />` , etiketlerin önüne **index.html**bölümünün sonuna yerleştirin `<script />` :

```html
<input 
  id="callee-phone-input"
  type="text"
  placeholder="Phone number you would like to dial"
  style="margin-bottom:1em; width: 230px;"
/>
<div>
  <button id="call-phone-button" type="button">
    Start Phone Call
  </button>
  &nbsp;
  <button id="hang-up-phone-button" type="button" disabled="true">
    Hang Up Phone Call
  </button>
</div>
```

Uygulama mantığınızı telefon işlevleriyle genişletin.

Bu kodu **client.js**ekleyin:

```javascript
const calleePhoneInput = document.getElementById("callee-phone-input");
const callPhoneButton = document.getElementById("call-phone-button");
const hangUpPhoneButton = document.getElementById("hang-up-phone-button");
```

## <a name="start-a-call-to-phone"></a>Telefon çağrısı Başlat

Iletişim Hizmetleri kaynağında edindiğiniz telefon numarasını belirtin; Bu, çağrıyı başlatmak için kullanılır:
> [!WARNING]
> Telefon numaralarının E. 164 uluslararası standart biçiminde sağlanması gerektiğini unutmayın. (ör.: + 12223334444)

Tıklandığında verdiğiniz telefon numarası için bir çağrı başlatmak üzere bir olay işleyicisi ekleyin `callPhoneButton` :


```javascript
callPhoneButton.addEventListener("click", () => {
  // start a call to phone
  const phoneToCall = calleePhoneInput.value;
  call = callAgent.call(
    [{phoneNumber: phoneToCall}], { alternateCallerId: {phoneNumber: '+18336528005'}
  });

  // toggle button states
  hangUpPhoneButton.disabled = false;
  callPhoneButton.disabled = true;
});
```

## <a name="end-a-call-to-phone"></a>Telefon çağrısını Sonlandır

Tıklandığında geçerli çağrıyı sonlandırmak için bir olay dinleyicisi ekleyin `hangUpPhoneButton` :

```javascript
hangUpPhoneButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({
    forEveryone: true
  });

  // toggle button states
  hangUpPhoneButton.disabled = true;
  callPhoneButton.disabled = false;
});
```

`forEveryone`Özelliği tüm çağrı katılımcıları için çağrıyı sonlandırır.

## <a name="run-the-code"></a>Kodu çalıştırma

`webpack-dev-server`Uygulamanızı derlemek ve çalıştırmak için kullanın. Uygulama konağını yerel bir Web sunucusuna eklemek için aşağıdaki komutu çalıştırın:


```console
npx webpack-dev-server --entry ./client.js --output bundle.js
```

Tarayıcınızı açın ve adresine gidin `http://localhost:8080/` . Şunları görmeniz gerekir:


![Tamamlanan JavaScript uygulamasının ekran görüntüsü.](../media/javascript/pstn-calling-javascript-app.png)

Eklenen metin alanına bir telefon numarası girerek ve **telefon görüşmesini Başlat** düğmesine tıklayarak gerçek telefon numarasına bir çağrı yerleştirebilirsiniz.

> [!WARNING]
> Telefon numaralarının E. 164 uluslararası standart biçiminde sağlanması gerektiğini unutmayın. (ör.: + 12223334444)
