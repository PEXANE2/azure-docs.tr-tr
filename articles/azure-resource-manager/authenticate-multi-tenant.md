---
title: Kiracılar genelinde kimlik doğrulama-Azure Resource Manager
description: Azure Resource Manager kiracılar genelinde kimlik doğrulama isteklerini nasıl işlediğini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: tomfitz
ms.openlocfilehash: b85ed32ac333402caeca4901e4d91bbe4d1d112c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72300355"
---
# <a name="authenticate-requests-across-tenants"></a>Kiracılar genelinde isteklerin kimliğini doğrulama

Çok kiracılı bir uygulama oluştururken, farklı kiracılarda bulunan kaynaklar için kimlik doğrulama isteklerini işlemeniz gerekebilir. Bir Kiracıdaki bir sanal makinenin başka bir Kiracıdaki sanal bir ağa katılması gereken yaygın bir senaryo. Azure Resource Manager, farklı kiracılara yönelik isteklerin kimliğini doğrulamak için yardımcı belirteçleri depolamak üzere bir üst bilgi değeri sağlar.

## <a name="header-values-for-authentication"></a>Kimlik doğrulaması için üst bilgi değerleri

İstek aşağıdaki kimlik doğrulama üst bilgisi değerlerine sahiptir:

| Üst bilgi adı | Açıklama | Örnek değer |
| ----------- | ----------- | ------------ |
| Yetkilendirme | Birincil belirteç | Taşıyıcı &lt;Birincil-belirteç @ no__t-1 |
| x-MS-yetkilendirme-yardımcı | Yardımcı belirteçler | Taşıyıcı &lt;yardımcı-token1 @ no__t-1, Encryptedtaşıyıcı &lt;yardımcı-token2 @ no__t-3, taşıyıcı &lt;yardımcı-token3 @ no__t-5 |

Yardımcı üst bilgi en fazla üç yardımcı belirteç tutabilir. 

Çok kiracılı uygulamanızın kodunda, diğer kiracıların kimlik doğrulama belirtecini alın ve bunları yardımcı üstbilgilere depolayın. Tüm belirteçler aynı kullanıcı veya uygulamadan olmalıdır. Kullanıcı veya uygulama diğer kiracılara Konuk olarak davet edilmiş olmalıdır.

## <a name="processing-the-request"></a>İstek işleniyor

Uygulamanız Kaynak Yöneticisi bir istek gönderdiğinde, istek birincil belirteçten kimlik altında çalıştırılır. Birincil belirtecin geçerli olması ve geçerliliği dolmamış olması gerekir. Bu belirteç, aboneliği yönetebileceğini bir kiracıdan olmalıdır.

İstek farklı kiracıdan bir kaynağa başvurduğunda, Kaynak Yöneticisi isteğin işlenip işlenemediğini anlamak için yardımcı belirteçleri denetler. Başlıktaki tüm yardımcı belirteçler geçerli ve geçerliliği geçmemiş olmalıdır. Herhangi bir belirtecin kullanım geçerliliği dolmuşsa Kaynak Yöneticisi, 401 yanıt kodu döndürür. Yanıt, geçerli olmayan belirteçten istemci KIMLIĞINI ve kiracı KIMLIĞINI içerir. Yardımcı üst bilgi kiracı için geçerli bir belirteç içeriyorsa, çapraz kiracı isteği işlenir.

## <a name="next-steps"></a>Sonraki adımlar

* Kimlik doğrulama istekleri hakkında bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](../active-directory/develop/authentication-flows-app-scenarios.md).
* Belirteçler hakkında daha fazla bilgi için bkz. [Azure Active Directory erişim belirteçleri](../active-directory/develop/access-tokens.md).
