---
title: Microsoft ile eşleme ayarlama önkoşulları
titleSuffix: Azure
description: Microsoft ile eşleme ayarlama önkoşulları
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 3c820a7be561aeef9b7e50fd0ac0cf4dee721af8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775413"
---
# <a name="prerequisites-to-set-up-peering-with-microsoft"></a>Microsoft ile eşleme ayarlama önkoşulları

Yeni bir bakış isteğinde bulunmadan veya azure kaynağına bakan bir eskiyi dönüştürmeden önce aşağıdaki ön koşulların karşılandığından emin olun.

## <a name="azure-related-prerequisites"></a>Azure ile ilgili ön koşullar
* **Microsoft Azure hesabı:** Microsoft Azure hesabınız yoksa, bir Microsoft [Azure hesabı](https://azure.microsoft.com/free)oluşturun. Eşlemeler Azure abonelikleri içindeki kaynaklar olarak modellendirildikçe, eşlemeleri ayarlamak için geçerli ve etkin bir Microsoft Azure aboneliği gerekir. Bu dikkat etmek önemlidir:
    * Eşleme ayarlamak için kullanılan Azure kaynak türleri her zaman ücretsiz Azure ürünleridir, yani bir Azure hesabı oluşturmak veya abonelik oluşturmak veya eşleme ayarlamak **için** Azure kaynaklarına erişmek için Ücretlendirilmez. **PeerAsn** Bu, siz ve Microsoft arasında, eş ekibimizle açıkça tartışılan terimler arasındaki Doğrudan bakış anlaşmasıyla karıştırılmamalıdır. Bu konuda herhangi bir sorunuz varsa [Microsoft'a başvurun.](mailto:peering@microsoft.com)
    * Ücretsiz veya ücretli olabilecek diğer Azure ürünlerine veya bulut hizmetlerine erişmek için aynı Azure aboneliğini kullanabilirsiniz. Ücretli bir ürüne erişdiğinizde ücrete tabi siniz.
    * Yeni bir Azure hesabı ve/veya aboneliği oluşturuyorsanız, Azure Bulut hizmetlerini denemek için kullanabileceğiniz deneme süresi boyunca ücretsiz Azure kredisi almaya hak kazanabilirsiniz. İlgileniyorsanız, daha fazla bilgi için [Microsoft Azure hesabını](https://azure.microsoft.com/free) ziyaret edin.

* **Ortak Eş ASN:** Bakma isteğinde bulunmadan önce, önce ASN ve iletişim bilgilerinizi aboneliğinizle ilişkilendirin. Azure Aboneliği için [Eş Eş ASN'deki](howto-subscription-association-powershell.md)yönergeleri izleyin.

## <a name="other-prerequisites"></a>Diğer ön koşullar
* **PeeringDB profili:** Eşlerin [PeeringDB'de](https://www.peeringdb.com)tam ve güncel bir profile sahip olması beklenir. Bu bilgileri kayıt sistemimizde, noc bilgileri, teknik iletişim bilgileri ve bunların akran tesislerindeki varlığı gibi akranların bilgilerini doğrulamak için kullanırız.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan bir eşleme oluşturun veya değiştirin.](howto-direct-portal.md)
* [Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)
* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini bir Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)