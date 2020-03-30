---
title: Özel teklifler | Azure Marketi
description: Uygulama ve hizmet yayıncıları için Azure Marketi'nde özel teklifler.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/1/2018
ms.author: dsindona
ms.openlocfilehash: 67aba077304117ad357d4e004ce7bdb25ac58352
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285053"
---
# <a name="private-offers"></a>Özel teklifler

[Microsoft Azure Marketi'ndeki](https://azuremarketplace.microsoft.com/) özel teklifler, yayıncıların yalnızca hedeflenen müşteriler tarafından görülebilen STU'lar oluşturmasına olanak tanır.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Özel tekliflerle kurumsal anlaşmaların kilidini açın

Kurumsal müşteriler bulut çözümleri bulmak, denemek ve satın almak için çevrimiçi pazar yerlerini giderek daha fazla kullanıyor. Artık özel tekliflerle yayıncılar, işletmelerin gerektirdiği özelliklere sahip hedeflenmiş müşterilerle özelleştirilmiş çözümleri özel olarak paylaşmak için pazar yerlerini kullanabilir:

- *Anlaşmalı fiyatlandırma,* yayıncıların herkese açık tekliflerdeki indirimleri ve liste dışı fiyatlandırmayı genişletmelerine olanak tanır.
- *Özel hüküm ve koşullar,* yayıncıların hüküm ve koşulları belirli bir müşteriye uyarlamasına olanak tanır.
- *Özel yapılandırmalar,* yayıncıların Sanal Makinelerini, Azure Uygulamalarını ve SaaS Uygulamaları'nı tek bir müşterinin ihtiyaçlarına göre uyarlamasına izin verirler. Bu seçenek, yayıncıların tüm müşterilere daha geniş bir şekilde başlatmadan önce yeni ürün özelliklerine önizleme erişimi sağlamasına da olanak tanır.

Özel teklifler, yayıncıların özel fırsatlar ve yapılandırmalar sunmak için gereken esneklik ve kontrolle, genel bir pazar ın ölçeğinden ve küresel kullanılabilirliğinden yararlanmalarına olanak sağlar. Birlikte, bu özellikler bulut pazarlarının güçlü kurumsal benimsenmesi için kapıyı açar.  İşletmeler artık bekledikleri ve talep ettikleri şekillerde alıp satabiliyorlar.

Artık Virtual Machine, Azure Uygulaması (çözüm şablonları veya yönetilen uygulamalar olarak uygulanır) için özel teklifler mevcuttur ve SaaS Apps teklifleri sunar. Genel teklifler de olduğu gibi, özel teklifler de [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md)üzerinden oluşturulabilir ve yönetilebilir.  Müşterilere dakikalar içinde özel tekliflere erişim izni verilebilir veya erişim iptal edilebilir.

## <a name="creating-private-offers-using-skus-and-plans"></a>SK'ler ve planlar kullanarak özel teklifler oluşturma

*Genel STU'lar veya planlar ile yeni veya mevcut teklifler*için, yayıncılar yeni STU'lar veya planlar oluşturarak ve bunları özel olarak işaretleyerek kolayca yeni, özel varyasyonlar oluşturabilirler.  [Özel SNU'lar](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) ve planlar bir teklifin bileşenleridir ve yalnızca hedeflenen müşteriler tarafından görülebilir ve satın alınabilir. Özel SKU'lar ve planlar temel görüntüleri yeniden kullanabilir ve/veya genel bir SKU veya plan için zaten yayınlanmış meta verileri sunabilir. Bu seçenek, yayıncıların aynı temel resmin birden çok sürümü yayımlanmak ve meta veriler sunmak zorunda kalmadan herkese açık bir teklifin birden çok özel varyasyonu oluşturmasına olanak tanır. Sanal Makine ve Azure uygulaması yalnızca, özel bir SKU ortak Bir SKU ile temel bir görüntü paylaştığında, teklifin temel görüntüsündeki herhangi bir değişiklik, bu temel görüntüyü kullanarak tüm genel ve özel SKU'larda yayılır.

*Yalnızca özel STU'lar veya planlar içeren yeni teklifler*için, yayıncılar tekliflerini başka bir teklif olarak oluşturabilir ve snu'ları veya planları özel olarak işaretleyebilir. Yalnızca özel SUK'lara veya planlara sahip teklifler, [teklifle](https://azuremarketplace.microsoft.com) ilişkili olmayan müşteriler tarafından Azure Marketi veya [Azure portalı](https://azure.microsoft.com/features/azure-portal/) üzerinden bulunamaz veya erişilebilir olmayacaktır.

## <a name="targeting-customers-with-private-offers"></a>Özel tekliflerle müşterileri hedefleme
Hem yeni hem de mevcut özel teklifler için yayıncılar abonelik tanımlayıcılarını kullanarak müşterileri hedefleyebilir. Sanal Makine veya Azure Uygulaması teklifini kullanan yayıncılar, özel bir SKU'nun kullanılabilirliğini tek bir Azure abonelik kimliğiyle sınırlandırabilir veya 20.000'e kadar Azure abonelik kimliği ne csv yükleyebilir. Yayıncılar, SaaS App özel teklifini kullanırken, el kitabı veya CSV yükleme yaklaşımını kullanarak özel bir planın kullanılabilirliğini kısıtlamak için bir Azure abonelik kimliği veya kiracı kimliği ilişkilendirebilir.

Bir teklif onaylandıktan ve yayınlandıktan sonra, müşteriler Eşitleme Özel Abonelikleri özelliğini kullanarak birkaç dakika içinde Güncellenebilir veya SKU veya Plan'dan kaldırılabilir. Bu özellik, yayıncıların özel SKU veya planın sunulduğu müşteri listesini teklifi düzeltmeden veya yeniden yayınlamadan hızlı ve kolay bir şekilde güncelleştirmelerine olanak tanır.

## <a name="deploying-private-offers"></a>Özel tekliflerin dağıtılması

Özel teklifler yalnızca Azure [portalı](https://azure.microsoft.com/features/azure-portal/) üzerinden keşfedilebilir ve [Azure Marketi](https://azuremarketplace.microsoft.com)üzerinden sunulmamaktadır. Azure portalına giriş yaptıktan sonra, müşteriler özel tekliflerine erişmek için Market navigasyon öğesini seçebilir. Özel Teklifler arama sonuçlarında da görünür ve diğer teklifler gibi komut satırı ve Azure Kaynak Yöneticisi şablonları aracılığıyla dağıtılabilir.

![[Özel teklifler]](./media/marketplace-publishers-guide/private-offer.png)

Özel teklifler de arama sonuçlarında görünür. "Özel" rozetine dikkat edin.

>[!Note]
>Özel teklifler, Bulut Çözüm Sağlayıcısı programının (CSP) bir satıcısı aracılığıyla kurulan aboneliklerle desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Özel teklifleri kullanmaya başlamak için [Özel SNU ve Planlar](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) kılavuzundaki adımları izleyin.
