---
author: nikuklic
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/14/2020
ms.author: nikuklic
ms.openlocfilehash: d25f14f7413e0f9263b067f31f9075a126b2c124
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762634"
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

## <a name="setting-up"></a>Ayarlanıyor

### <a name="add-pstn-functionality-your-app"></a>Uygulamanıza PSTN işlevselliği ekleyin

`PhoneNumber` **MainActivity. Java**' yı değiştirerek türü uygulamanıza ekleyin:


```java
import com.azure.android.communication.common.PhoneNumber;
```

<!--
> [!TBD]
> Namespace based on input from Komivi Agbakpem. But it does not correlates with other use namespaces in Calling Quickstart. E.g: "com.azure.communication.calling.CommunicationUser" or "com.azure.communication.common.client.CommunicationUserCredential". Double-chek this.
-->

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Android için istemci Kitaplığı çağıran Azure Iletişim Hizmetleri 'nin bazı önemli özelliklerinden bazılarını işler.

| Ad                                           | Açıklama                                                                                          |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| PhoneNumber | Bu sınıf, telefon işlevleri için kullanmak istediğiniz telefon numarasını başlatmak için gereklidir. |


## <a name="start-a-call-to-phone"></a>Telefon çağrısı Başlat

Iletişim Hizmetleri kaynağınızın içinden aldığınız telefon numarasını belirtin. Bu, çağrıyı başlatmak için kullanılacaktır:

> [!WARNING]
> Telefon numaralarının E. 164 uluslararası standart biçiminde sağlanması gerektiğini unutmayın. (ör.: + 12223334444)

`startCall()` **MainActivity. Java**içindeki olay işleyicisini, telefon çağrılarını işleyecek şekilde değiştirin:

```java
    private void startCall() {
        EditText calleePhoneView = findViewById(R.id.callee_id);
        String calleePhone = calleePhoneView.getText().toString();
        PhoneNumber callerPhone = new PhoneNumber("+12223334444");
        StartCallOptions options = new StartCallOptions();
        options.setAlternateCallerId(callerPhone);
        options.setVideoOptions(new VideoOptions(null));
        call = agent.call(
                getApplicationContext(),
                new PhoneNumber[] {new PhoneNumber(calleePhone)},
                options);
    }
```

## <a name="launch-the-app-and-call-the-echo-bot"></a>Uygulamayı başlatın ve yankı bot 'ı çağırın

Uygulama artık araç çubuğunda "uygulama Çalıştır" düğmesi kullanılarak başlatılabilir (SHIFT + F10). Eklenen metin alanına bir telefon numarası girerek ve **çağrı** düğmesine tıklayarak telefon araması yapabilirsiniz.
> [!WARNING]
> Telefon numaralarının E. 164 uluslararası standart biçiminde sağlanması gerektiğini unutmayın. (ör.: + 12223334444)

![Tamamlanmış uygulamayı gösteren ekran görüntüsü.](../media/android/quickstart-android-call-pstn.png)
