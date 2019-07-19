---
title: Azure Güvenlik Merkezi 'nde güvenlik uyarıları | Microsoft Docs
description: Bu konuda, güvenlik uyarıları ve Azure Güvenlik Merkezi 'nde bulunan farklı türler açıklanmaktadır.
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
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 91dd397095718b3b43e41767af422801fd50b7f1
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295692"
---
# <a name="security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik uyarıları

Bu makalede, Azure Güvenlik Merkezi 'nde (ASC) bulunan farklı güvenlik uyarısı türleri sunulmaktadır. Birçok farklı kaynak türü için çeşitli uyarılar vardır. ASC, hem Azure üzerinde dağıtılan kaynaklar hem de şirket içi ve karma bulut ortamlarında dağıtılan kaynaklar için uyarı oluşturur. 

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?

Uyarılar, güvenlik merkezi 'nin kaynaklarınızın tehditleri algıladığında oluşturduğu bildirimlerdir. Sorunu hızlı bir şekilde araştırmak için gereken bilgileri ve uyarıları önceliklendirir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler de sağlar.

## <a name="how-does-security-center-detect-threats"></a>Güvenlik Merkezi tehditleri nasıl algılar?

Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini toplar, çözümler ve tümleştirir. Güvenlik Merkezi, tehditleri belirlemek için genellikle birden çok kaynaktan gelen bilgileri ilişkilendirmek üzere bu bilgileri analiz eder.

ASC, Azure 'da dağıtılan veya diğer şirket içi ve hibrit bulut ortamlarında dağıtılan kaynakları izler. Tehditleri algılama ve yanıtlama hakkında daha fazla bilgi edinmek için bkz. [Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Güvenlik uyarısı türleri

Aşağıdaki konular, kaynak türlerine göre farklı ASC uyarılarında size rehberlik sağlar:

* [IaaS VM 'Leri sunucu uyarıları &](security-center-alerts-iaas.md)
* [Yerel işlem uyarıları](security-center-alerts-compute.md)
* [Veri Hizmetleri uyarıları](security-center-alerts-data-services.md)

Aşağıdaki konularda, Azure üzerinde dağıtılan kaynaklar için ek koruma katmanları uygulamak üzere güvenlik merkezi 'nin Azure altyapısıyla tümleştirerek topladığı farklı telemetrinin nasıl kullanıldığı açıklanmaktadır:

* [Hizmet katmanı uyarıları](security-center-alerts-service-layer.md)
* [Azure Güvenlik ürünleriyle tümleştirme](security-center-alerts-integration.md)

## <a name="what-are-alert-incidents"></a>Uyarı olayları nelerdir?

Güvenlik olayı, her uyarıyı ayrı ayrı listelemek yerine ilgili uyarıların bir koleksiyonudur. Güvenlik Merkezi, farklı uyarıların ve düşük uygunlukta sinyallerin güvenlik olaylarına ilişkilendirilmesi için Fusion kullanır.

Güvenlik Merkezi, olayları kullanarak bir saldırı kampanyasının ve tüm ilgili uyarıların tek bir görünümünü sağlar. Bu görünüm, saldırganın hangi eylemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca anlamanıza olanak sağlar. Daha fazla bilgi için bkz. [bulut Akıllı uyarı bağıntısı](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Uyarıları kullanmaya başlama

ASC tarafından izlenen kaynaklar hakkında daha fazla bilgi edinmek ve ASC tarafından sunulan uyarılara nasıl yanıt verileceğini öğrenmek için aşağıdaki konulara bakın.

* Hangi platformların ve özelliklerin ASC ile korunduğunu görmek için bkz. [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve Özellikler](security-center-os-coverage.md).  
* Güvenlik olaylarını ve ASC 'nin bunlara nasıl yanıt vereceğini anlamak için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik olaylarını işleme](security-center-incident.md). 
* Aldığınız uyarıların nasıl yönetileceğini öğrenmek için bkz. [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md).
* Güvenlik Merkezi 'nin düzgün şekilde yapılandırıldığını doğrulama ve bir test uyarısı alma hakkında bilgi için bkz. [Azure Güvenlik Merkezi 'Nde uyarılar doğrulaması](security-center-alert-validation.md).  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Gelişmiş algılamalar için standart sürümüne yükseltme

Gelişmiş algılamaları ayarlamak için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. 

1. Güvenlik Merkezi menüsünde **güvenlik ilkesi**' ni seçin.
2. Standart katmana taşımak istediğiniz abonelikler için **Ayarları Düzenle**' ye tıklayın. 
3. Ayarlar sayfasında **fiyatlandırma katmanı**' nı seçin. 
   Ayda ücretsiz bir deneme vardır. Daha fazla bilgi edinmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Güvenlik Merkezi 'nde güvenlik uyarılarını ve farklı uyarı türlerini öğrendiniz. Daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Azure Güvenlik MERKEZI SSS](https://docs.microsoft.com/azure/security-center/security-center-faq): Hizmet kullanımı ile ilgili sık sorulan soruları bulun.

