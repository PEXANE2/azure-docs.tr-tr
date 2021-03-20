---
title: Azure kanıtlama Iş akışı
description: Azure kanıtlama iş akışı.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 27a97ceb2ca9a7b58df7200930e4e47d89c9ae89
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98762210"
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

## <a name="trusted-platform-module-tpm-enclave-validation-work-flow"></a>Güvenilir Platform Modülü (TPM) şifreleme doğrulama iş akışı

Tipik bir TPM şifrelemesi kanıtlama iş akışındaki (Azure kanıtlama kullanarak) genel adımlar aşağıda verilmiştir:

1.  Cihaz/platform önyüklemesi üzerinde, TPM tarafından desteklenen ve güvenli bir şekilde saklanan ve (TCG günlüğü) çeşitli önyükleme yükleyicileri ve önyükleme hizmetleri tarafından ölçü olayları işlenir.
2.  İstemci, cihaz ve TPM tırnağından gelen TCG günlüklerini toplar. Bu, kanıtlama kanıtını üstlenir.
3.  İstemcinin bir Azure kanıtlama örneğine başvuran bir URI 'SI vardır. İstemci Azure kanıtlama için kanıt gönderir. Sağlayıcıya gönderilen tam bilgiler platforma bağlıdır.
4.  Azure kanıtlama gönderilen bilgileri doğrular ve yapılandırılan bir ilkeye göre değerlendirir. Doğrulama başarılı olursa, Azure kanıtlama bir kanıtlama belirteci yayınlar ve istemciye döndürür. Bu adım başarısız olursa, Azure kanıtlama istemciye bir hata bildirir. İstemci ve kanıtlama hizmeti arasındaki iletişim Azure kanıtlama TPM protokolü tarafından belirlenir.
5.  İstemci daha sonra kanıtlama belirtecini bağlı olan tarafa gönderir. Bağlı olan taraf, imzalama sertifikalarını almak için Azure kanıtlama 'nın ortak anahtar meta veri uç noktasını çağırır. Bağlı olan taraf daha sonra kanıtlama belirtecinin imzasını doğrular ve platformların güvenilirliğini sağlar.

![TPM doğrulama akışı](./media/tpm-validation-flow.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
