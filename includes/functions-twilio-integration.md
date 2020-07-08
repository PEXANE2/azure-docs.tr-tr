---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188172"
---
Bu örnek, bir cep telefonuna SMS iletileri göndermek için [Twilio](https://www.twilio.com/) hizmetinin kullanılmasını içerir. Azure Işlevleri, [Twilio bağlama](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio)aracılığıyla Twilio desteğini zaten içerir ve örnek bu özelliği kullanır.

İhtiyacınız olan ilk şey bir Twilio hesabıdır. Bir ücretsiz oluşturabilirsiniz https://www.twilio.com/try-twilio . Bir hesabınız olduğunda, işlev uygulamanıza aşağıdaki üç **uygulama ayarını** ekleyin.

| Uygulama ayarı adı | Değer açıklaması |
| - | - |
| **TwilioAccountSid**  | Twilio hesabınızın SID 'SI |
| **TwilioAuthToken**   | Twilio hesabınız için kimlik doğrulama belirteci |
| **TwilioPhoneNumber** | Twilio hesabınızla ilişkilendirilen telefon numarası. Bu, SMS mesajları göndermek için kullanılır. |