---
title: Kiracılar arasında kimlik doğrulaması
description: Azure Resource Manager kiracılar genelinde kimlik doğrulama isteklerini nasıl işlediğini açıklar.
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 7a13ba6f6cbfc10c52484c45e4011da7a0d8ee4c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75478831"
---
# <a name="authenticate-requests-across-tenants"></a>Kiracılar genelinde isteklerin kimliğini doğrulama

Çok kiracılı bir uygulama oluştururken, farklı kiracılarda bulunan kaynaklar için kimlik doğrulama isteklerini işlemeniz gerekebilir. Bir Kiracıdaki bir sanal makinenin başka bir Kiracıdaki sanal bir ağa katılması gereken yaygın bir senaryo. Azure Resource Manager, farklı kiracılara yönelik isteklerin kimliğini doğrulamak için yardımcı belirteçleri depolamak üzere bir üst bilgi değeri sağlar.

## <a name="header-values-for-authentication"></a>Kimlik doğrulaması için üst bilgi değerleri

İstek aşağıdaki kimlik doğrulama üst bilgisi değerlerine sahiptir:

| Üst bilgi adı | Açıklama | Örnek değer |
| ----------- | ----------- | ------------ |
| Yetkilendirme | Birincil belirteç | Taşıyıcı &lt; birincil belirteç&gt; |
| x-MS-yetkilendirme-yardımcı | Yardımcı belirteçler | Taşıyıcı &lt; yardımcı-token1 &gt; , encryptedtaşıyıcı &lt; yardımcı-token2 &gt; , taşıyıcı &lt; yardımcı-token3&gt; |

Yardımcı üst bilgi en fazla üç yardımcı belirteç tutabilir. 

Çok kiracılı uygulamanızın kodunda, diğer kiracıların kimlik doğrulama belirtecini alın ve bunları yardımcı üstbilgilere depolayın. Tüm belirteçler aynı kullanıcı veya uygulamadan olmalıdır. Kullanıcı veya uygulama diğer kiracılara Konuk olarak davet edilmiş olmalıdır.

## <a name="processing-the-request"></a>İstek işleniyor

Uygulamanız Kaynak Yöneticisi bir istek gönderdiğinde, istek birincil belirteçten kimlik altında çalıştırılır. Birincil belirtecin geçerli olması ve geçerliliği dolmamış olması gerekir. Bu belirteç, aboneliği yönetebileceğini bir kiracıdan olmalıdır.

İstek farklı kiracıdan bir kaynağa başvurduğunda, Kaynak Yöneticisi isteğin işlenip işlenemediğini anlamak için yardımcı belirteçleri denetler. Başlıktaki tüm yardımcı belirteçler geçerli ve geçerliliği geçmemiş olmalıdır. Herhangi bir belirtecin kullanım geçerliliği dolmuşsa Kaynak Yöneticisi, 401 yanıt kodu döndürür. Yanıt, geçerli olmayan belirteçten istemci KIMLIĞINI ve kiracı KIMLIĞINI içerir. Yardımcı üst bilgi kiracı için geçerli bir belirteç içeriyorsa, çapraz kiracı isteği işlenir.

## <a name="next-steps"></a>Sonraki adımlar

* Kimlik doğrulama istekleri hakkında bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](../../active-directory/develop/authentication-flows-app-scenarios.md).
* Belirteçler hakkında daha fazla bilgi için bkz. [Azure Active Directory erişim belirteçleri](../../active-directory/develop/access-tokens.md).
