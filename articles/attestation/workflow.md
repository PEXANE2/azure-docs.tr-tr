---
title: Azure kanıtlama Iş akışı
description: Azure kanıtlama iş akışı.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 240b27f897d8e7a34026701cf7fdc844eb9d4086
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89237379"
---
# <a name="workflow"></a>İş akışı

Microsoft Azure kanıtlama, kuşkudan gelen kanıtları alır ve Azure Güvenlik temeli ve yapılandırılabilir ilkelerle karşı kanıtları değerlendirir. Başarılı bir şekilde, Azure kanıtlama, kuşlığının güvenilirliğini onaylamak için bir kanıtlama belirteci oluşturur.

Aşağıdaki aktörler bir Azure kanıtlama iş akışına dahil değildir:

- **Bağlı olan taraf**: kuşatma geçerliliğini doğrulamak Için Azure kanıtlama 'yi temel alan bileşen. 
- **İstemci**: bir kuşdan bilgi toplayan ve Istekleri Azure kanıtlama 'na gönderen bileşen. 
- **Azure kanıtlama**: istemciden şifreleme kanıtlamayı kabul eden bileşen bunu doğrular ve istemciye kanıtlama belirteci döndürür


## <a name="enclave-validation-work-flow"></a>Şifreleme doğrulama iş akışı

Tipik bir SGX şifreleme iş akışındaki (Azure kanıtlama kullanarak) genel adımlar aşağıda verilmiştir:

1. İstemci bir kuşdan kanıt toplar. Kanıt, kuşatma ortamı ve kuşın içinde çalışan istemci kitaplığı hakkında bilgi.
1. İstemcinin bir Azure kanıtlama örneğine başvuran bir URI 'SI vardır. İstemci Azure AD kimliğini doğrular ve bir erişim belirteci alır.
1. İstemci, erişim belirteciyle birlikte Azure kanıtlama için kanıt gönderir. Sağlayıcıya gönderilen tam bilgiler, şifreleme türüne bağlıdır.
1. Azure kanıtlama gönderilen bilgileri doğrular ve yapılandırılan bir ilkeye göre değerlendirir. Doğrulama başarılı olursa, Azure kanıtlama bir kanıtlama belirteci yayınlar ve istemciye döndürür. Bu adım başarısız olursa, Azure kanıtlama istemciye bir hata bildirir. 
1. İstemci, kanıtlama belirtecini bağlı olan tarafa gönderir. Bağlı olan taraf, imzalama sertifikalarını almak için Azure kanıtlama 'nın ortak anahtar meta veri uç noktasını çağırır. Bağlı olan taraf daha sonra kanıtlama belirtecinin imzasını doğrular ve şifreleme güvenilirliğini sağlar. 

![Şifreleme doğrulama akışı](./media/validation-flow.png)


## <a name="next-steps"></a>Sonraki adımlar
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
