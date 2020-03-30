---
title: Microsoft ile eşleme ayarlama
titleSuffix: Azure
description: Akrana genel bakış
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: overview
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 576bc3e37711851acd7d6c7ac811a10e40080710
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75908926"
---
# <a name="internet-peering-overview"></a>Internet'e bakış genel bakış

Peering, Microsoft'un küresel ağı (AS8075) ile ağınız arasında, Microsoft çevrimiçi hizmetlerine ve Microsoft Azure Hizmetleri'ne İnternet trafiği alışverişi yapmak amacıyla ara bağlantıdır. Operatörler veya Hizmet Sağlayıcıları, Edge konumlarımızdan herhangi birinde Microsoft'a bağlanmayı talep edebilir. Her istek, izleme politikamıza uyduğundan emin olmak için Microsoft tarafından gözden geçirilir. Microsoft ağıyla bir eşleme iki şekilde ayarlayabilirsiniz:

* **Doğrudan bakma:**

    Peering, Microsoft Edge'deki Microsoft ağı ile ağınız arasındaki doğrudan fiziksel bağlantılar üzerinden kurulur. BGP oturumları, yönlendirme politikamıza göre ve önceden müzakere edilmiş anlaşma kullanılarak bu bağlantılar arasında yapılandırılır. Bu da PNI olarak adlandırılır.

* **Exchange akran:**

    Bu, Internet Exchanges'teki (IX) standart genel bakış bağlantıları anlamına gelir. Microsoft ağı ile ağınız arasındaki fiziksel bağlantılar IX tarafından işletilen anahtar kumaştan geçer. BGP oturumları IX tarafından sağlanan IP alanı kullanılarak yapılandırılır.

## <a name="benefits-of-peering-with-microsoft"></a>Microsoft ile bakmanın avantajları
* Microsoft ile bakışları kullanarak Microsoft trafiğini sunarak geçiş maliyetlerinizi düşürün.
* Ağ atlamalarını ve Microsoft Edge ağına gecikme süresini azaltarak müşterilerinizin performansını artırın.
* Gereksiz konumlarda Microsoft ile birlikte bakarak müşteri trafiğini ağınızdaki veya geçiş sağlayıcınızın ağındaki hatalara karşı koruyun.
* Bakan bağlantılarınız hakkındaki performans ölçümlerini öğrenin ve ağınızı gidermek için öngörülerden yararlanın.

## <a name="benefits-of-using-azure-to-set-up-peering"></a>Eşleme ayarlamak için Azure'u kullanmanın avantajları

Azure PowerShell veya portalı kullanarak Microsoft ile izleme isteğinde bulunabilirsiniz. Bu şekilde ayarlanan peering bir Azure kaynağı olarak yönetilir ve aşağıdaki avantajları sağlar:
* Microsoft ile bakış kurmayı ve yönetmeyi ayarlamak için basitleştirilmiş ve otomatikleştirilebilir adımlar.
* Tüm bakışaçılarınızı tek bir yerde görüntülemenin ve yönetmenin hızlı ve kolay yolu.
* Tüm bağlantılarınız için durum ve bant genişliği verilerini izleyin.
* Azure Bulut Hizmetleri'nize erişmek için aynı aboneliği kullanabilirsiniz.

Microsoft ile zaten eşlemeler oluşturduysanız, **bunlareski eşlemeler**olarak adlandırılır. Yukarıdaki avantajlardan yararlanmak için Azure kaynağı gibi bakışları yönetmeyi seçebilirsiniz. Yeni bir eşleme isteği göndermek veya eski bir bakış isteğini Azure kaynağına dönüştürmek için aşağıdaki **Sonraki adımlar** bölümündeki bağlantıları izleyin.

## <a name="peering-policy"></a>Eşleme ilkesi
Microsoft'un seçici, ancak genellikle açık bir eşleme ilkesi vardır. Akranlar performansa, kapasiteye ve karşılıklı faydanın olduğu durumlarda belirli teknik, ticari ve yasal gerekliliklere göre seçilirler. Ayrıntılar için, [bkz.](policy.md)

## <a name="faq"></a>SSS
Bakma hakkında sık sorulan sorular için [Internet'e bakma - SSS](faqs.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft ile doğrudan bakış kurma adımları hakkında bilgi edinmek için [Doğrudan bakış izleme](walkthrough-direct-all.md)
* Exchange'in Microsoft ile eşlemeyi ayarlama adımları hakkında bilgi edinmek için [Exchange'in bakış izleme gözden geçirmesini](walkthrough-exchange-all.md) izleyin
* Azure'un diğer başlıca [ağ özelliklerinden](https://docs.microsoft.com/azure/networking/networking-overview) bazıları hakkında bilgi edinin.
