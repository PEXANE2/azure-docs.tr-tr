---
title: Microsoft ile eşleme ayarlama
titleSuffix: Azure
description: Eşlemeye genel bakış
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: overview
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 576bc3e37711851acd7d6c7ac811a10e40080710
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75908926"
---
# <a name="internet-peering-overview"></a>Internet eşlemesi genel bakış

Eşleme, Microsoft 'un, Microsoft çevrimiçi hizmetler ve Microsoft Azure hizmetlerinden Internet trafiğini değiştirme amacıyla Microsoft 'un küresel ağı (AS8075) ve ağınız arasındaki iç bağlantıdır. Taşıyıcılar veya hizmet sağlayıcıları, Microsoft ile kenar konumlarımızla bağlantı kurmak isteyebilir. Her istek, eşleme ilkenize bağlı olduğundan emin olmak için Microsoft tarafından incelenir. Microsoft Network ile bir eşlemeyi iki şekilde ayarlayabilirsiniz:

* **Doğrudan eşleme:**

    Eşleme, Microsoft Edge ve ağınız ile Microsoft ağı arasındaki doğrudan fiziksel bağlantılar üzerinden oluşturulur. BGP oturumları, yönlendirme ilkenize göre ve önceden anlaşmaya varılan sözleşme kullanılarak bu bağlantılar genelinde yapılandırılır. Bu, PNı olarak da adlandırılır.

* **Exchange eşlemesi:**

    Bu, Internet alışverişlerde (x) standart genel eşleme bağlantılarına başvurur. Microsoft ağı ile ağınız arasındaki fiziksel bağlantılar, x tarafından işletilen anahtar dokusuna sahiptir. BGP oturumları, x tarafından sunulan IP alanı kullanılarak yapılandırılır.

## <a name="benefits-of-peering-with-microsoft"></a>Microsoft ile eşleme avantajları
* Microsoft ile eşleme kullanarak Microsoft trafiği sunarak transit maliyetlerinizi düşürün.
* Ağ atlamalarını ve Microsoft Edge Network 'e gecikme süresini azaltarak müşterilerinizin performansını artırabilirsiniz.
* Gereksiz konumlarda Microsoft ile eşleme yaparak, müşteri trafiğini ağ veya transit sağlayıcınızın ağındaki hatalara karşı koruyun.
* Eşleme bağlantılarınız hakkında performans ölçümlerini öğrenin ve ağınızı sorun gidermek için öngörüleri kullanın.

## <a name="benefits-of-using-azure-to-set-up-peering"></a>Eşleme ayarlamak için Azure kullanmanın avantajları

Azure PowerShell veya Portal kullanarak Microsoft ile eşleme isteyebilirsiniz. Bu şekilde ayarlanan eşleme bir Azure kaynağı olarak yönetilir ve aşağıdaki avantajları sağlar:
* Microsoft ile eşlemeyi ayarlamak ve yönetmek için Basitleştirilmiş ve haritasıdır adımları.
* Tüm eşlerinizi tek bir yerde görüntülemenin ve yönetmenin hızlı ve kolay yolu.
* Tüm bağlantılarınız için durum ve bant genişliği verilerini izleyin.
* Azure Cloud Services erişmek için aynı aboneliği kullanabilirsiniz.

Zaten Microsoft ile eşlemeler oluşturduysanız, bunlar **eski**eşlemeler olarak adlandırılır. Bu tür eşlemeleri, yukarıdaki avantajlardan yararlanmak için Azure kaynağı olarak yönetmeyi tercih edebilirsiniz. Yeni bir eşleme isteği göndermek veya eski eşlemeyi Azure kaynağına dönüştürmek için aşağıdaki **sonraki adımlar** bölümündeki bağlantıları izleyin.

## <a name="peering-policy"></a>Eşleme ilkesi
Microsoft, seçmeli, ancak genellikle açık eşleme ilkesine sahiptir. Eşler, performans, yetenek ve karşılıklı avantajlara göre seçilir ve belirli teknik, ticari ve yasal gereksinimlere tabidir. Ayrıntılar için bkz. [eşleme ilkesi](policy.md).

## <a name="faq"></a>SSS
Eşleme hakkında sık sorulan sorular için bkz. [Internet eşlemesi-SSS](faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft ile doğrudan eşleme ayarlama adımları hakkında bilgi edinmek için [doğrudan eşleme izlenecek yolu](walkthrough-direct-all.md) izleyin
* Exchange eşlemesini Microsoft ile ayarlama adımları hakkında bilgi edinmek için [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu izleyin.
* Azure'un diğer başlıca [ağ özelliklerinden](https://docs.microsoft.com/azure/networking/networking-overview) bazıları hakkında bilgi edinin.
