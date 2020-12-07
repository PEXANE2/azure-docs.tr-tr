---
title: Azure kanıtlama Iş akışı
description: Azure kanıtlama iş akışı.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 09d793f3d8ed544a386a362677f24be6d18673d7
ms.sourcegitcommit: 003ac3b45abcdb05dc4406661aca067ece84389f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96748745"
---
# <a name="workflow"></a>İş akışı

Microsoft Azure kanıtlama, kuşkulardan gelen kanıtları alır ve Azure Güvenlik temeli ve yapılandırılabilir ilkelerle karşı kanıtları değerlendirir. Doğrulama başarıyla tamamlandığında, Azure kanıtlama, kuşlığının güvenilirliğini onaylamak için bir kanıtlama belirteci oluşturur.

Aşağıdaki aktörler bir Azure kanıtlama iş akışına dahil değildir:

- **Bağlı olan taraf**: kuşatma geçerliliğini doğrulamak Için Azure kanıtlama 'yi temel alan bileşen. 
- **İstemci**: bir kuşdan bilgi toplayan ve Istekleri Azure kanıtlama 'na gönderen bileşen. 
- **Azure kanıtlama**: istemciden şifreleme kanıtlamayı kabul eden bileşen bunu doğrular ve istemciye kanıtlama belirteci döndürür


## <a name="intel-software-guard-extensions-sgx-enclave-validation-work-flow"></a>Intel® Software Guard uzantıları (SGX) şifreleme doğrulama iş akışı

Tipik bir SGX şifreleme iş akışındaki (Azure kanıtlama kullanarak) genel adımlar aşağıda verilmiştir:

1. İstemci bir kuşdan kanıt toplar. Kanıt, kuşatma ortamı ve kuşın içinde çalışan istemci kitaplığı hakkında bilgi.
1. İstemcinin bir Azure kanıtlama örneğine başvuran bir URI 'SI vardır. İstemci Azure kanıtlama için kanıt gönderir. Sağlayıcıya gönderilen tam bilgiler, şifreleme türüne bağlıdır.
1. Azure kanıtlama gönderilen bilgileri doğrular ve yapılandırılan bir ilkeye göre değerlendirir. Doğrulama başarılı olursa, Azure kanıtlama bir kanıtlama belirteci yayınlar ve istemciye döndürür. Bu adım başarısız olursa, Azure kanıtlama istemciye bir hata bildirir. 
1. İstemci, kanıtlama belirtecini bağlı olan tarafa gönderir. Bağlı olan taraf, imzalama sertifikalarını almak için Azure kanıtlama 'nın ortak anahtar meta veri uç noktasını çağırır. Bağlı olan taraf daha sonra kanıtlama belirtecinin imzasını doğrular ve şifreleme güvenilirliğini sağlar. 

![SGX şifreleme doğrulama akışı](./media/sgx-validation-flow.png)

> [!Note]
> [2018-09-01-Preview](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/attestation/data-plane/Microsoft.Attestation/stable/2018-09-01-preview) API sürümünde kanıtlama istekleri gönderdiğinizde, ISTEMCININ Azure AD erişim belirteciyle birlikte Azure kanıtlama 'na kanıt gönderebilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
