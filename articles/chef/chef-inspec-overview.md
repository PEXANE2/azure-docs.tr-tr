---
title: Azure altyapınızın uyumluluk otomasyonu için InSpec'i kullanın
description: Azure dağıtımlarınızdaki sorunları algılamak için InSpec'i nasıl kullanacağınızı öğrenin
keywords: azure, şef, devops, sanal makineler, genel bakış, otomatikleştirmek, inspec
ms.date: 03/19/2019
ms.topic: article
ms.openlocfilehash: 2531277eb1aa6048c93240031652e09582409e56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74158232"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Azure altyapınızın uyumluluk otomasyonu için InSpec'i kullanın

[InSpec,](https://www.chef.io/inspec/) yazılım mühendisleri, operasyonlar ve güvenlik mühendisleri arasında paylaşılabilen güvenlik & uyumluluk kurallarını açıklayan Chef'in açık kaynak dilidir. InSpec, altyapınızın gerçek durumunu, okunması kolay ve kolay yazılması kolay InSpec kodunda ifade ettiğiniz istediğiniz durumla karşılaştırarak çalışır. InSpec ihlalleri algılar ve bulguları rapor biçiminde görüntüler, ancak düzeltmenin denetimini siz sağlar.

Sanal makineler, ağ yapılandırmaları, Azure Etkin Dizin ayarları ve daha fazlası dahil olmak üzere abonelikteki kaynakların ve kaynak gruplarının durumunu doğrulamak için InSpec'i kullanabilirsiniz.

Bu makalede, Azure'da güvenliği ve uyumluluğu kolaylaştırmak için InSpec'i kullanmanın yararları açıklanmaktadır.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Uyumluluğu anlamayı ve değerlendirmeyi kolaylaştırın

Elektronik tablolarda veya Word belgelerinde yazılan uyumluluk belgeleri gereksinimleri yoruma açık bırakır. InSpec ile gereksinimlerinizi sürümlenmiş, çalıştırılabilir, insan tarafından okunabilir koda dönüştürürsunuz. Kod, somut testler lehine net bir niyetle nelerin değerlendirilmesi gerektiği ne hakkındaki konuşmaların yerini alır.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Filo çapındaki sorunları tespit edin ve bunların düzeltilmesine öncelik verin

InSpec'in aracısız algılama modu, pozlama seviyenizi hızlı bir şekilde değerlendirmenizi sağlar. Etki/önem dereceleri puanlama için yerleşik meta veriler, düzeltme için hangi alanlara odaklanılabilmek gerektiğini belirlemeye yardımcı olur. Ayrıca, yeni güvenlik açıklarına veya düzenlemelere yanıt olarak kuralları hızlı bir şekilde yazabilir ve bunları hemen dağıtabilirsiniz.

## <a name="audit-azure-virtual-machines-with-policy-guest-configuration"></a>İlke Konuk Yapılandırması ile Azure sanal makinelerini denetleme

Azure, [Azure İlkesi Konuk Yapılandırması](/azure/governance/policy/concepts/guest-configuration)aracılığıyla Azure sanal makinelerini denetlemek için Chef InSpec tanımlarının kullanımını doğrudan destekler. Konuk Yapılandırma, Linux sanal makinesini sağlanan Chef InSpec tanımına göre değerlendirir ve Azure Politikası aracılığıyla uyumluluğu geri bildirir. Bu denetimlerin sonuçları Azure Monitor günlükleri aracılığıyla da bildirilir; uyarıları ve diğer otomasyon senaryolarını etkinleştirme.

## <a name="satisfy-audits"></a>Denetimleri tatmin edin

InSpec ile, denetim sorularına sadece üç aylık veya yıllık gibi önceden belirlenmiş aralıklarla değil, istediğiniz zaman yanıt verebilirsiniz. InSpec testlerini sürekli çalıştırarak, bir denetçinin bulgularına şaşırmaktansa, tam uyumluluk duruşunuzu ve geçmişinizi bilerek bir denetim döngüsüne girersiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure Bulut Uyp'ta InSpec'i deneyin](https://shell.azure.com)