---
title: Kiracılar arasında kimlik doğrulaması
description: Azure Kaynak Yöneticisi'nin kiracılar arasında kimlik doğrulama isteklerini nasıl işleyeceğini açıklar.
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 7a13ba6f6cbfc10c52484c45e4011da7a0d8ee4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75478831"
---
# <a name="authenticate-requests-across-tenants"></a>Kiracılar arasında istekleri doğrulama

Çok kiracılı bir uygulama oluştururken, farklı kiracılarda bulunan kaynaklar için kimlik doğrulama isteklerini işlemeniz gerekebilir. Sık karşılaşılan bir senaryo, bir kiracıdaki sanal bir makinenin başka bir kiracıda sanal ağa katılması gerektiğidir. Azure Kaynak Yöneticisi, istekleri farklı kiracılara doğrulamak için yardımcı belirteçleri depolamak için bir üstbilgi değeri sağlar.

## <a name="header-values-for-authentication"></a>Kimlik doğrulaması için üstbilgi değerleri

İstek aşağıdaki kimlik doğrulama üstbilgi değerlerine sahiptir:

| Üst bilgi adı | Açıklama | Örnek değer |
| ----------- | ----------- | ------------ |
| Yetkilendirme | Birincil belirteç | Taşıyıcı &lt;birincil belirteç&gt; |
| x-ms-yetkilendirme-yardımcı | Yardımcı belirteçler | Taşıyıcı &lt;&gt;yardımcı jeton1 , Şifreli Taşıyıcı &lt;yardımcı-belirteç2&gt;, &lt;Taşıyıcı yardımcı-belirteç3&gt; |

Yardımcı başlık en fazla üç yardımcı belirteç tutabilir. 

Çok kiracılı uygulamanızın kodunda, diğer kiracılar Için kimlik doğrulama belirteci alın ve bunları yardımcı üstbilgide saklayın. Tüm belirteçler aynı kullanıcıdan veya uygulamadan olmalıdır. Kullanıcı veya uygulama diğer kiracılara konuk olarak davet edilmiş olmalıdır.

## <a name="processing-the-request"></a>İsteğin işlenmesi

Uygulamanız Kaynak Yöneticisi'ne bir istek gönderdiğinde, istek birincil belirteçteki kimlik altında çalıştırılır. Birincil belirteç geçerli ve süresi dolmamış olmalıdır. Bu belirteç, aboneliği yönetebilen bir kiracıdan olmalıdır.

İstek farklı kiracıdan bir kaynağa başvurur, Kaynak Yöneticisi isteğin işlenebilir olup olmadığını belirlemek için yardımcı belirteçleri denetler. Üstbilgideki tüm yardımcı belirteçler geçerli ve süresi dolmamış olmalıdır. Herhangi bir belirteç süresi dolmuşsa, Kaynak Yöneticisi bir 401 yanıt kodu döndürür. Yanıt, geçerli olmayan belirteçteki istemci kimliğini ve kiracı kimliğini içerir. Yardımcı üstbilgi kiracı için geçerli bir belirteç içeriyorsa, çapraz kiracı isteği işlenir.

## <a name="next-steps"></a>Sonraki adımlar

* Kimlik doğrulama istekleri hakkında bilgi edinmek için [kimlik doğrulama akışları ve uygulama senaryolarına](../../active-directory/develop/authentication-flows-app-scenarios.md)bakın.
* Belirteçler hakkında daha fazla bilgi için [Azure Etkin Dizin erişim belirteçleri'ne](../../active-directory/develop/access-tokens.md)bakın.
