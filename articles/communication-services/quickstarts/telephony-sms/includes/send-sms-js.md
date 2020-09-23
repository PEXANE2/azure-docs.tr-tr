---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: dademath
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: dademath
ms.openlocfilehash: cdd4988f9a23904c0771852c4539aa9bce2ee683
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948348"
---
SMS mesajları göndermek için Iletişim Hizmetleri JavaScript SMS istemci Kitaplığı ' nı kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

<!--**TODO: update all these reference links as the resources go live**

[API reference documentation](../../../references/overview.md) | [Library source code](https://github.com/Azure/azure-sdk-for-js-pr/tree/feature/communication/sdk/communication/communication-sms) | [Package (NPM)](https://www.npmjs.com/package/@azure/communication-sms) | [Samples](#todo-samples)-->

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/) Etkin LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- SMS etkin telefon numarası. [Telefon numarası alın](../get-phone-number.md).

### <a name="prerequisite-check"></a>Önkoşul denetimi

- Node.js yüklendiğini denetlemek için bir Terminal veya komut penceresinde komutunu çalıştırın `node --version` .
- Iletişim Hizmetleri kaynağınız ile ilişkili telefon numaralarını görüntülemek için [Azure Portal](https://portal.azure.com/)oturum açın, iletişim hizmetleri kaynağınızı bulun ve sol gezinti bölmesinden **telefon numaraları** sekmesini açın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

İlk olarak, Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir sms-quickstart && cd sms-quickstart
```

`npm init -y`Dosya **üzerinde** varsayılan ayarlarla birpackage.jsoluşturmak için öğesini çalıştırın.

```console
npm init -y
```

Proje kök dizininde **send-sms.js** adlı bir dosya oluşturmak için bir metin düzenleyici kullanın. Bu hızlı başlangıç için tüm kaynak kodu aşağıdaki bölümlerde bu dosyaya ekleyeceksiniz.

### <a name="install-the-package"></a>Paketi yükler

`npm install`JavaScript Için Azure Iletişim HIZMETLERI SMS istemci kitaplığı 'nı yüklemek için komutunu kullanın.

```console
npm install @azure/communication-sms --save
```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Node.js için Azure Communication Services SMS istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| SmsClient | Bu sınıf tüm SMS işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve SMS mesajları göndermek için kullanabilirsiniz. |
| SendSmsOptions | Bu arabirim, teslim raporlamayı yapılandırmak için seçenekler sağlar. `enable_delivery_report`Olarak ayarlanırsa `true` , teslim başarılı olduğunda bir olay yayınlanır. |
| SendMessageRequest | Bu arabirim, SMS isteği oluşturmaya yönelik modeldir (örn. telefon numaralarını ve SMS içeriğini yapılandırın. |

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

**SmsClient** istemci kitaplığından içeri aktarın ve bağlantı dizeniz ile örneğini oluşturun. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

Aşağıdaki kodu **send-sms.js**ekleyin:

```javascript
const { SmsClient } = require('@azure/communication-sms');

// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
```

## <a name="send-an-sms-message"></a>SMS iletisi gönderme

Yöntemini çağırarak SMS iletisi gönderin `send` . Bu kodu **send-sms.js**sonuna ekleyin:

```javascript
await smsClient.send({
  from: "<leased-phone-number>",
  to: ["<to-phone-number>"],
  message: "Hello World 👋🏻 via Sms"
}, {
  enableDeliveryReport: true //Optional parameter
});
```

`<leased-phone-number>`Iletişim Hizmetleri kaynağınız ile ILIŞKILI SMS özellikli telefon numarasıyla ve `<to-phone-number>` ileti göndermek istediğiniz telefon numarasıyla değiştirmelisiniz. Tüm telefon numarası parametreleri [E. 164 standardına](../../../concepts/telephony-sms/plan-solution.md#optional-reading-international-public-telecommunication-numbering-plan-e164)bağlı olmalıdır.

`enableDeliveryReport`Parametresi, teslim raporlamayı yapılandırmak için kullanabileceğiniz isteğe bağlı bir parametredir. Bu, SMS iletileri teslim edildiğinde olayları yayma isteyebileceğiniz senaryolar için yararlıdır. SMS iletilerinize yönelik teslim raporlamayı yapılandırmak için [SMS olayları](../handle-sms-events.md) Hızlı Başlangıç Kılavuzu ' na bakın.

## <a name="run-the-code"></a>Kodu çalıştırma

`node` **send-sms.js** dosyasına eklediğiniz kodu çalıştırmak için komutunu kullanın.

```console

node ./send-sms.js

```