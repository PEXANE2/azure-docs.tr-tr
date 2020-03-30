---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188172"
---
Bu örnek, bir cep telefonuna SMS mesajları göndermek için [Twilio](https://www.twilio.com/) hizmetini kullanmayı içerir. Azure İşlevleri zaten [Twilio bağlama yoluyla Twilio](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio)için destek vardır ve örnek bu özelliği kullanır.

İhtiyacın olan ilk şey bir Twilio hesabı. Bir ücretsiz https://www.twilio.com/try-twiliooluşturabilirsiniz. Bir hesabınız olduktan sonra, işlev uygulamanıza aşağıdaki üç **uygulama ayarını** ekleyin.

| Uygulama ayar adı | Değer açıklaması |
| - | - |
| **TwilioAccountSid**  | Twilio hesabınız için SID |
| **TwilioAuthToken**   | Twilio hesabınız için Auth belirteci |
| **TwilioPhoneNumber** | Twilio hesabınızla ilişkili telefon numarası. Bu SMS mesajları göndermek için kullanılır. |