---
title: Azure Güvenlik Merkezi 'nde güvenlik uyarıları | Microsoft Docs
description: Bu konu başlığı altında, güvenlik uyarıları ve Azure Güvenlik Merkezi 'nde bulunan farklı türler açıklanmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/20/2019
ms.author: v-mohabe
ms.openlocfilehash: d905096432c6e05e5386631480e5cc45b5c890c5
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013289"
---
# <a name="security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarıları

Azure Güvenlik Merkezi 'nde birçok farklı kaynak türü için çeşitli uyarılar vardır. Güvenlik Merkezi, Azure 'da dağıtılan ve ayrıca şirket içi ve hibrit bulut ortamlarında dağıtılan kaynaklar için uyarı oluşturur. 

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?

Uyarılar, güvenlik merkezi 'nin kaynaklarınızın tehditleri algıladığında oluşturduğu bildirimlerdir. Sorunu hızlı bir şekilde araştırmak için gereken bilgileri ve uyarıları önceliklendirir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler de sağlar.

## <a name="how-does-security-center-detect-threats"></a>Güvenlik Merkezi tehditleri nasıl algılar?

Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan ve ağınızdan gelen günlük verilerini toplar, çözümler ve tümleştirir. Ayrıca, güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümleri ile de birlikte kullanılabilir. Güvenlik Merkezi, tehditleri belirlemek için genellikle birden çok kaynaktan gelen bilgileri ilişkilendirmek üzere bu bilgileri analiz eder.

Güvenlik Merkezi, kaynakları nerede olabileceğini her türlü ortamda izler. Tehditleri algılama ve yanıtlama hakkında daha fazla bilgi edinmek için bkz. [Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Güvenlik uyarısı türleri

Aşağıdaki konular, kaynak türlerine göre farklı uyarılarda size rehberlik sağlar:

* [IaaS VM 'Leri ve sunucu uyarıları](security-center-alerts-iaas.md)
* [Yerel işlem uyarıları](security-center-alerts-compute.md)
* [Veri Hizmetleri uyarıları](security-center-alerts-data-services.md)

Aşağıdaki konularda, Azure üzerinde dağıtılan kaynaklar için ek koruma katmanları uygulamak amacıyla, güvenlik merkezi 'nin Azure altyapısıyla tümleştirerek topladığı farklı Telemetriyi nasıl kullandığı açıklanmaktadır:

* [Hizmet katmanı uyarıları](security-center-alerts-service-layer.md)
* [Azure Güvenlik ürünleriyle tümleştirme](security-center-alerts-integration.md)

## <a name="what-are-security-incidents"></a>Güvenlik olayları nelerdir?

Güvenlik olayı, her uyarıyı ayrı ayrı listelemek yerine ilgili uyarıların bir koleksiyonudur. Güvenlik Merkezi, farklı uyarıların ve düşük uygunlukta sinyallerin güvenlik olaylarına ilişkilendirilmesi için [bulut Akıllı uyarı bağıntısını](security-center-alerts-cloud-smart.md) kullanır.

Güvenlik Merkezi, olayları kullanarak bir saldırı kampanyasının ve tüm ilgili uyarıların tek bir görünümünü sağlar. Bu görünüm, saldırganın hangi eylemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca anlamanıza olanak sağlar. Daha fazla bilgi için bkz. [bulut Akıllı uyarı bağıntısı](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Uyarıları kullanmaya başlama

Aşağıda, Güvenlik Merkezi tarafından izlenen kaynaklar hakkında daha fazla bilgi anlamanıza yardımcı olan makaleler ve sunulan uyarılara yanıt verme hakkında yönergeler sağlanmaktadır.

* [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve özellikler](security-center-os-coverage.md)  
* [Azure Güvenlik Merkezi 'nde güvenlik olaylarını işleme](security-center-incident.md) 
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi'nde Uyarıları Doğrulama](security-center-alert-validation.md)  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Gelişmiş algılamalar için standart sürümüne yükseltme

Gelişmiş algılamaları ayarlamak için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. 

1. **Güvenlik Merkezi** menüsünde **güvenlik ilkesi**' ni seçin.
2. Standart katmana taşımak istediğiniz abonelikler için **Ayarları Düzenle**' yi seçin. 
3. Ayarlar sayfasında **fiyatlandırma katmanı**' nı seçin. 
   Ayda ücretsiz bir deneme vardır. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenlik merkezi 'nde bulunan farklı Uyarı türleri hakkında bilgi edindiniz. Daha fazla bilgi için bkz.

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Azure Güvenlik Merkezi SSS](https://docs.microsoft.com/azure/security-center/security-center-faq)

