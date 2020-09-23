---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: danieldoolabh
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 09/03/2020
ms.topic: include
ms.custom: include file
ms.author: dadoolab
ms.openlocfilehash: 3fe27bf31385e0310211b5e1b0b7bbfa636eb19b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948360"
---
SMS mesajları göndermek için Iletişim Hizmetleri Python SMS istemci Kitaplığı ' nı kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

<!--**TODO: update all these reference links as the resources go live**

[API reference documentation](../../../references/overview.md) | [Library source code](#todo-sdk-repo) | [Package (PiPy)](#todo-nuget) | [Samples](#todo-samples)--> 

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Python](https://www.python.org/downloads/) 2,7, 3,5 veya üzeri.
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- SMS etkin telefon numarası. [Telefon numarası alın](../get-phone-number.md).

### <a name="prerequisite-check"></a>Önkoşul denetimi

- Bir Terminal veya komut penceresinde, `python --version` Python 'un yüklü olup olmadığını denetlemek için komutunu çalıştırın.
- Iletişim Hizmetleri kaynağınız ile ilişkili telefon numaralarını görüntülemek için [Azure Portal](https://portal.azure.com/)oturum açın, iletişim hizmetleri kaynağınızı bulun ve sol gezinti bölmesinden **telefon numaraları** sekmesini açın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir sms-quickstart && cd sms-quickstart
```

Proje kök dizininde **Send-SMS.py** adlı bir dosya oluşturmak ve temel özel durum işleme dahil olmak üzere programın yapısını eklemek için bir metin düzenleyicisi kullanın. Bu hızlı başlangıç için tüm kaynak kodu aşağıdaki bölümlerde bu dosyaya ekleyeceksiniz.

```python
import os
from azure.communication.sms import PhoneNumber
from azure.communication.sms import SendSmsOptions
from azure.communication.sms import SmsClient

try:
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-the-package"></a>Paketi yükler

Hala uygulama dizininde, komutunu kullanarak Python paketi için Azure Iletişim Hizmetleri SMS istemci Kitaplığı ' nı da yükleyebilirsiniz `pip install` .

```console
pip install azure-communication-sms
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Python için Azure Iletişim Hizmetleri SMS istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| SmsClient | Bu sınıf tüm SMS işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve SMS mesajları göndermek için kullanabilirsiniz. |
| SendSmsOptions | Bu sınıf, teslim raporlamayı yapılandırmak için seçenekler sağlar. Enable_delivery_report true olarak ayarlanırsa, teslim başarılı olduğunda bir olay yayınlanır |

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Bağlantı dizeniz ile bir **SmsClient** örneği oluşturun. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

```python
# This code demonstrates how to fetch your connection string
# from an environment variable.
connection_string = os.getenv('COMMUNICATION_SERVICES_CONNECTION_STRING')

# Create the SmsClient object which will be used to send SMS messages
sms_client = SmsClient.from_connection_string(connection_string)
```

## <a name="send-an-sms-message"></a>SMS iletisi gönderme

Send yöntemini çağırarak SMS iletisi gönderin. `try` **Send-SMS.py**içindeki blok sonuna bu kodu ekleyin:

```python

# calling send() with sms values
sms_response = sms_client.send(
        from_phone_number=PhoneNumber("<leased-phone-number>"),
        to_phone_numbers=[PhoneNumber("<to-phone-number>")],
        message="Hello World via SMS",
        send_sms_options=SendSmsOptions(enable_delivery_report=True)) # optional property

```

`<leased-phone-number>`İletişim hizmetinize ve `<to-phone-number>` ileti göndermek istediğiniz telefon numarasıyla ILIŞKILI bir SMS etkin telefon numarasıyla değiştirmelisiniz. Tüm telefon numarası parametreleri [E. 164 standardına](../../../concepts/telephony-sms/plan-solution.md#optional-reading-international-public-telecommunication-numbering-plan-e164)bağlı olmalıdır.

`send_sms_options`Parametresi, teslim raporlamayı yapılandırmak için kullanabileceğiniz isteğe bağlı bir parametredir. Bu, SMS iletileri teslim edildiğinde olayları yayma isteyebileceğiniz senaryolar için yararlıdır. SMS iletilerinize yönelik teslim raporlamayı yapılandırmak için [SMS olayları](../handle-sms-events.md) Hızlı Başlangıç Kılavuzu ' na bakın.

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `python` .

```console
python send-sms.py
```
