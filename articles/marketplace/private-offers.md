---
title: Özel teklifler | Azure Marketi
description: Uygulama ve hizmet yayıncıları için Azure Marketi'nde özel teklifler.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/19/2020
ms.author: dsindona
ms.openlocfilehash: efda9fb66a65f1f2cd40de8bf32a8a3fa99aea7f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681425"
---
# <a name="private-offers"></a>Özel teklifler

[Microsoft Azure Marketi'ndeki](https://azuremarketplace.microsoft.com/) özel teklifler, yayıncıların yalnızca hedeflenen müşteriler tarafından görülebilen STU'lar oluşturmasına olanak tanır.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Özel tekliflerle kurumsal anlaşmaların kilidini açın

Kurumsal müşteriler bulut çözümleri bulmak, denemek ve satın almak için çevrimiçi pazar yerlerini giderek daha fazla kullanıyor. Artık özel tekliflerle yayıncılar, işletmelerin gerektirdiği özelliklere sahip hedeflenmiş müşterilerle özelleştirilmiş çözümleri özel olarak paylaşmak için pazar yerlerini kullanabilir:

- *Anlaşmalı fiyatlandırma,* yayıncıların herkese açık tekliflerdeki indirimleri ve liste dışı fiyatlandırmayı genişletmelerine olanak tanır.
- *Özel hüküm ve koşullar,* yayıncıların hüküm ve koşulları belirli bir müşteriye uyarlamasına olanak tanır.
- *Özel yapılandırmalar,* yayıncıların Sanal Makinelerini, Azure Uygulamalarını ve SaaS Uygulamaları'nı tek bir müşterinin ihtiyaçlarına göre uyarlamasına izin verirler. Bu seçenek, yayıncıların tüm müşterilere daha geniş bir şekilde başlatmadan önce yeni ürün özelliklerine önizleme erişimi sağlamasına da olanak tanır.

Özel teklifler, yayıncıların özel fırsatlar ve yapılandırmalar sunmak için gereken esneklik ve kontrolle, genel bir pazar ın ölçeğinden ve küresel kullanılabilirliğinden yararlanmalarına olanak sağlar. Birlikte, bu özellikler bulut pazarlarının güçlü kurumsal benimsenmesi için kapıyı açar. İşletmeler artık bekledikleri ve talep ettikleri şekillerde alıp satabiliyorlar.

Artık Virtual Machine, Azure Uygulaması (çözüm şablonları veya yönetilen uygulamalar olarak uygulanır) için özel teklifler mevcuttur ve SaaS Apps teklifleri sunar. 

<!--- Like public offers, private offers can be created and managed via the [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).  Customers can be granted or revoked access to private offers in minutes.
--->

## <a name="creating-private-offers-using-skus-and-plans"></a>SK'ler ve planlar kullanarak özel teklifler oluşturma

*Genel STU'lar veya planlar ile yeni veya mevcut teklifler*için, yayıncılar yeni STU'lar veya planlar oluşturarak ve bunları özel olarak işaretleyerek kolayca yeni, özel varyasyonlar oluşturabilirler.  

<!--- [Private SKUs](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) ---> 

Özel SNU'lar ve planlar bir teklifin bileşenleridir ve yalnızca hedeflenen müşteriler tarafından görülebilir ve satın alınabilir. Özel SKU'lar ve planlar temel görüntüleri yeniden kullanabilir ve/veya genel bir SKU veya plan için zaten yayınlanmış meta verileri sunabilir. Bu seçenek, yayıncıların aynı temel resmin birden çok sürümü yayımlanmak ve meta veriler sunmak zorunda kalmadan herkese açık bir teklifin birden çok özel varyasyonu oluşturmasına olanak tanır. Sanal Makine ve Azure uygulaması yalnızca, özel bir SKU ortak Bir SKU ile temel bir görüntü paylaştığında, teklifin temel görüntüsündeki herhangi bir değişiklik, bu temel görüntüyü kullanarak tüm genel ve özel SKU'larda yayılır.

*Yalnızca özel STU'lar veya planlar içeren yeni teklifler*için, yayıncılar tekliflerini başka bir teklif olarak oluşturabilir ve snu'ları veya planları özel olarak işaretleyebilir. Yalnızca özel SUK'lara veya planlara sahip teklifler, teklifle ilişkili olmayan müşteriler tarafından Microsoft ticari pazarı veya [Azure portalı](https://azure.microsoft.com/features/azure-portal/) aracılığıyla bulunamaz veya erişilebilir olmayacaktır.

## <a name="targeting-customers-with-private-offers"></a>Özel tekliflerle müşterileri hedefleme

Hem yeni hem de mevcut özel teklifler için yayıncılar abonelik tanımlayıcılarını kullanarak müşterileri hedefleyebilir. Sanal Makine veya Azure Uygulaması teklifini kullanan yayıncılar, özel bir SKU'nun kullanılabilirliğini tek bir Azure abonelik kimliğiyle sınırlandırabilir veya 20.000'e kadar Azure abonelik kimliği ne csv yükleyebilir. Yayıncılar, SaaS App özel teklifini kullanırken, el kitabı veya CSV yükleme yaklaşımını kullanarak özel bir planın kullanılabilirliğini kısıtlamak için kiracı kimliğini ilişkilendirebilir.

Bir teklif onaylandıktan ve yayınlandıktan sonra, müşteriler Eşitleme Özel Abonelikleri özelliğini kullanarak birkaç dakika içinde Güncellenebilir veya SKU veya Plan'dan kaldırılabilir. Bu özellik, yayıncıların özel SKU veya planın sunulduğu müşteri listesini teklifi onaylamadan veya yeniden yayımlamadan hızlı ve kolay bir şekilde güncelleştirmelerine olanak tanır.

## <a name="deploying-private-offers"></a>Özel tekliflerin dağıtılması

Özel teklifler yalnızca Azure [portalı](https://azure.microsoft.com/features/azure-portal/) üzerinden keşfedilebilir ve [Microsoft AppSource](https://appsource.microsoft.com/) veya [Azure Marketplace'te](https://azuremarketplace.microsoft.com)gösterilmez. Farklı ticari pazar vitrinlerinde yayımlama hakkında daha fazla bilgi edinmek için [bkz.](./determine-your-listing-type.md)

Azure portalına giriş yaptıktan sonra, müşteriler özel tekliflerine erişmek için Market navigasyon öğesini seçebilir. Özel teklifler arama sonuçlarında da görünür ve diğer teklifler gibi komut satırı ve Azure Kaynak Yöneticisi şablonları aracılığıyla dağıtılabilir.

![[Özel teklifler]](./media/marketplace-publishers-guide/private-offer.png)

Özel teklifler de arama sonuçlarında görünür. "Özel" rozetini ara.

>[!Note]
>Özel teklifler, Bulut Çözüm Sağlayıcısı programının (CSP) bir satıcısı aracılığıyla kurulan aboneliklerle desteklenmez.

<!---
## Next steps

To start using private offers, follow the steps in the [Private SKUs and Plans](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) guide.

--->