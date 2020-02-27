---
title: Azure Güvenlik Merkezi 'nde güvenlik olaylarını yönetme | Microsoft Docs
description: Bu belge, güvenlik olaylarını yönetmek için Azure Güvenlik Merkezi 'ni kullanmanıza yardımcı olur.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: memildin
ms.openlocfilehash: 1a6dbaeac5355d50edb93a7f215d7f8e88231e98
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615962"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik olaylarını yönetme

Güvenlik uyarılarını önceliklendirme ve araştırmak, en nitelikli Güvenlik analistleri için zaman alabilir ve birçok büyük olasılıkla nereden başlayabileceğinizi bilmelidir. Farklı [güvenlik uyarıları](security-center-detection-capabilities.md) arasındaki bilgileri bağlamak için [Analiz](security-center-managing-and-responding-alerts.md) özelliğini kullanan Güvenlik Merkezi, bir saldırı kampanyasını ve tüm ilgili uyarıları tek bir yerde görmenizi sağlayabilir; saldırganın hangi işlemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca görebilirsiniz.

Bu konu başlığında Güvenlik Merkezi 'ndeki olaylar ve bunların uyarılarını düzeltme kullanımı açıklanmaktadır.

## <a name="what-is-a-security-incident"></a>Güvenlik olayı nedir?

Güvenlik Merkezi'nde bir güvenlik olayı, bir kaynağın [sonlandırma zinciri](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) desenleri ile hizalanan tüm uyarılarının toplamıdır. Olaylar [güvenlik uyarıları](security-center-managing-and-responding-alerts.md) listesinde görünür. Her oluşum hakkında daha fazla bilgi edinmenizi sağlayan ilgili uyarıları görüntülemek için bir olaya tıklayın.

## <a name="managing-security-incidents"></a>Güvenlik olaylarını yönetme

1. Güvenlik Merkezi panosunda **güvenlik uyarıları** kutucuğuna tıklayın. Olaylar ve uyarılar listelenir. Güvenlik olayı açıklamasının diğer uyarılardan farklı bir simgeye sahip olduğuna dikkat edin.

    ![Güvenlik olaylarını görüntüle](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Ayrıntıları görüntülemek için bir olaya tıklayın. **Güvenlik olayı algılanan** dikey pencerede daha fazla ayrıntı görüntülenir. **Genel bilgiler** bölümü, güvenlik uyarısının tetiklendiği konusunda bir öngörü sunabilir. Hedef kaynak, kaynak IP adresi (varsa), uyarı hala etkinse ve nasıl düzeltileceğine ilişkin öneriler gibi bilgileri görüntüler.  

    ![Azure Güvenlik Merkezi 'nde güvenlik olaylarına yanıt verme](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Her uyarı hakkında daha fazla bilgi edinmek için bir uyarıya tıklayın. Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir.

   > [!NOTE]
   > Aynı uyarı, bir olayın parçası olarak ve tek başına bir uyarı olarak görülebilmesi için de bulunabilir.

    ![Uyarı ayrıntıları](./media/security-center-incident/security-center-incident-alert.png)

1. Her uyarı için verilen düzeltme adımlarını izleyin.

Uyarılar hakkında daha fazla bilgi için [güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md).

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
Bu belgede, Güvenlik Merkezi'nde güvenlik olayı özelliğini nasıl kullanacağınız hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi 'Nde güvenlik uyarıları](security-center-alerts-overview.md).
* [Güvenlik uyarılarını yönetme](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
