---
title: Azure Güvenlik Merkezi'ndeki güvenlik olaylarını yönetme | Microsoft Dokümanlar
description: Bu belge, güvenlik olaylarını yönetmek için Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 98fc339e473ffb2bf54e7119634e93046cca1ef3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415650"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik olaylarını yönetme

Güvenlik uyarılarını triyaj lamak ve araştırmak en yetenekli güvenlik analistleri için bile zaman alıcı olabilir ve çoğu için nereden başlayacağını bilmek bile zordur. Farklı [güvenlik uyarıları](security-center-managing-and-responding-alerts.md) arasındaki bilgileri bağlamak için [Analiz](security-center-detection-capabilities.md) özelliğini kullanan Güvenlik Merkezi, bir saldırı kampanyasını ve tüm ilgili uyarıları tek bir yerde görmenizi sağlayabilir; saldırganın hangi işlemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca görebilirsiniz.

Bu konu, Güvenlik Merkezi'ndeki olaylar ve uyarılarını nasıl düzelteceklerini açıklar.

## <a name="what-is-a-security-incident"></a>Güvenlik olayı nedir?

Güvenlik Merkezi'nde bir güvenlik olayı, bir kaynağın [sonlandırma zinciri](alerts-reference.md#intentions) desenleri ile hizalanan tüm uyarılarının toplamıdır. Olaylar Güvenlik [Uyarıları](security-center-managing-and-responding-alerts.md) listesinde görünür. Her olay hakkında daha fazla bilgi edinmenizi sağlayan ilgili uyarıları görüntülemek için bir olayı tıklatın.

## <a name="managing-security-incidents"></a>Güvenlik olaylarını yönetme

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** döşemesini tıklatın. Olaylar ve uyarılar listelenir. Güvenlik olayı açıklamasının diğer uyarılardan farklı bir simgeye sahip olduğuna dikkat edin.

    ![Güvenlik olaylarını görüntüleme](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Ayrıntıları görüntülemek için bir olayı tıklatın. **Güvenlik olayı algılanan** bıçak daha fazla ayrıntı görüntüler. **Genel Bilgiler** bölümü, güvenlik uyarısını neyin tetiklediğine dair bir fikir verebilir. Uyarı hala etkinse hedef kaynak, kaynak IP adresi (varsa) gibi bilgileri ve nasıl düzeltileceklerine ilişkin önerileri görüntüler.  

    ![Azure Güvenlik Merkezi'ndeki güvenlik olaylarına yanıt verme](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Her uyarı hakkında daha fazla bilgi edinmek için bir uyarıyı tıklatın. Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir.

   > [!NOTE]
   > Aynı uyarı, bir olayın parçası olarak var olabileceği gibi, tek başına bir uyarı olarak da görülebilir.

    ![Uyarı ayrıntıları](./media/security-center-incident/security-center-incident-alert.png)

1. Her uyarı için verilen düzeltme adımlarını izleyin.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik olayı özelliğini nasıl kullanacağınız hakkında bilgi edindiniz. İlgili bilgiler için aşağıdakileri görebilirsiniz:

* [Azure Güvenlik Merkezi'nde tehdit koruması](threat-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
* [Güvenlik uyarılarını yönetme](security-center-managing-and-responding-alerts.md)