---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme | Microsoft Docs
description: Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/27/2019
ms.author: memildin
ms.openlocfilehash: 5c199d074a6655ad14a0c66925e4302f70424970
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615979"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama

Bu konu başlığı altında, kaynaklarınızı korumak için aldığınız uyarıların nasıl görüntüleneceği ve işlenmesi gösterilmektedir. 

* Farklı Uyarı türleri hakkında bilgi edinmek için bkz. [güvenlik uyarısı türleri](security-center-alerts-overview.md#security-alert-types).
* Güvenlik Merkezi 'nin nasıl uyarı üretdiğine ilişkin genel bakış için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md#detect-threats).

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](tutorial-security-policy.md)’nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

> [!NOTE]
> Güvenlik Merkezi algılama yeteneklerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md#detect-threats).

## <a name="manage-your-security-alerts"></a>Güvenlik uyarılarınızı yönetme

1. Güvenlik Merkezi panosunda, uyarıları görüntülemek ve genel bakış için **tehdit koruması** kutucuğuna bakın.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. Uyarılar hakkında daha fazla ayrıntı görmek için kutucuğa tıklayın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Gösterilen uyarıları filtrelemek için **filtre**' ye tıklayın ve açılan **filtre** dikey penceresinde, uygulamak istediğiniz filtre seçeneklerini belirleyin. Liste seçili filtreye göre güncelleştirilir. Filtreleme çok faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama

1. **Güvenlik uyarıları** listesinden bir güvenlik uyarısı ' na tıklayın. Bir saldırının düzeltilmesi için gerçekleştirmeniz gereken kaynaklar ve ilgili adımlar gösterilir.

    ![Güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. Bilgileri inceledikten sonra saldırıya uğrayan bir kaynağa tıklayın.

    ![Güvenlik uyarıları hakkında ne yapacaklarıyla ilgili öneriler](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    **Genel bilgiler** bölümü, güvenlik uyarısının tetiklendiği konusunda bir öngörü sunabilir. Hedef kaynak, kaynak IP adresi (varsa), uyarı hala etkinse ve nasıl düzeltileceğine ilişkin öneriler gibi bilgileri görüntüler.  

    > [!NOTE]
    >Bazı örneklerde, kaynak IP adresi kullanılamaz, bazı Windows güvenlik olayları günlükleri IP adresini içermez.

1. Güvenlik Merkezi tarafından önerilen düzeltme adımları, güvenlik uyarısına göre farklılık gösterir. Bunları her uyarı için izleyin. 

    Bazı durumlarda, tehdit algılama uyarısını azaltmak için, önerilen düzeltmeyi uygulamak üzere diğer Azure denetimlerini veya hizmetlerini kullanmanız gerekebilir. 

    Aşağıdaki konular, kaynak türlerine göre farklı uyarılarda size rehberlik sağlar:
    
    * [IaaS Windows makineleri için uyarılar](threat-protection.md#windows-machines)
    * [IaaS Linux makineleri için uyarılar](threat-protection.md#linux-machines)
    * [Azure App Service için uyarılar](threat-protection.md#app-services)
    * [Azure kapsayıcıları için uyarılar](threat-protection.md#azure-containers)
    * [SQL veritabanı ve SQL veri ambarı için uyarılar](threat-protection.md#data-sql)
    * [Azure Storage uyarıları](threat-protection.md#azure-storage)
    * [Cosmos DB için uyarılar](threat-protection.md#cosmos-db)

    Aşağıdaki konularda, Azure üzerinde dağıtılan kaynaklar için ek koruma katmanları uygulamak amacıyla, güvenlik merkezi 'nin Azure altyapısıyla tümleştirerek topladığı farklı Telemetriyi nasıl kullandığı açıklanmaktadır:
    
    * [Azure Yönetim Katmanı (Azure Resource Manager) uyarıları (Önizleme)](threat-protection.md#management-layer)
    * [Azure Key Vault için uyarılar (Önizleme)](threat-protection.md#azure-keyvault)
    * [Azure ağ katmanı uyarıları](threat-protection.md#network-layer)
    * [Diğer hizmetlerden gelen uyarılar](threat-protection.md#alerts-other)    

## <a name="see-also"></a>Ayrıca bkz.

Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi 'Nde güvenlik uyarıları](security-center-alerts-overview.md).
* [Güvenlik olaylarını işleme](security-center-incident.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)