---
title: Azure Güvenlik Merkezi 'nde güvenlik olaylarını yönetme | Microsoft Docs
description: Bu belge, güvenlik olaylarını yönetmek için Azure Güvenlik Merkezi 'ni kullanmanıza yardımcı olur.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 98fc339e473ffb2bf54e7119634e93046cca1ef3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79415650"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik olaylarını yönetme

Güvenlik uyarılarını önceliklendirme ve araştırmak, en nitelikli Güvenlik analistleri için zaman alabilir ve birçok büyük olasılıkla nereden başlayabileceğinizi bilmelidir. Farklı [güvenlik uyarıları](security-center-managing-and-responding-alerts.md) arasındaki bilgileri bağlamak için [Analiz](security-center-detection-capabilities.md) özelliğini kullanan Güvenlik Merkezi, bir saldırı kampanyasını ve tüm ilgili uyarıları tek bir yerde görmenizi sağlayabilir; saldırganın hangi işlemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca görebilirsiniz.

Bu konu başlığında Güvenlik Merkezi 'ndeki olaylar ve bunların uyarılarını düzeltme kullanımı açıklanmaktadır.

## <a name="what-is-a-security-incident"></a>Güvenlik olayı nedir?

Güvenlik Merkezi'nde bir güvenlik olayı, bir kaynağın [sonlandırma zinciri](alerts-reference.md#intentions) desenleri ile hizalanan tüm uyarılarının toplamıdır. Olaylar [güvenlik uyarıları](security-center-managing-and-responding-alerts.md) listesinde görünür. Her oluşum hakkında daha fazla bilgi edinmenizi sağlayan ilgili uyarıları görüntülemek için bir olaya tıklayın.

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


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik olayı özelliğini nasıl kullanacağınız hakkında bilgi edindiniz. İlgili bilgiler için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi’nde tehdit koruması](threat-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
* [Güvenlik uyarılarını yönetme](security-center-managing-and-responding-alerts.md)