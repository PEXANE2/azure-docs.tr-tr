---
title: Azure DevTest Labs ortamlar için güvenlik uyarıları
description: Bu makalede, DevTest Labs 'de bir ortam için güvenlik uyarılarını görüntüleme ve uygun bir işlem yapma işlemleri gösterilmektedir.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2020
ms.author: spelluru
ms.openlocfilehash: fbac5a2fab91cdac8ebf626e324f12f209cfade5
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84898405"
---
# <a name="security-alerts-for-environments-in-azure-devtest-labs"></a>Azure DevTest Labs ortamlar için güvenlik uyarıları
Laboratuvar kullanıcısı olarak artık laboratuvar ortamlarınız için Azure Güvenlik Merkezi uyarılarını görüntüleyebilirsiniz. Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir. [Azure Güvenlik Merkezi 'nde güvenlik uyarıları hakkında daha fazla bilgi edinin](../security-center//security-center-alerts-overview.md).  


## <a name="prerequisites"></a>Ön koşullar
Şu anda, güvenlik uyarılarını yalnızca laboratuvarınıza dağıtılan bir hizmet olarak platform (PaaS) ortamları için görüntüleyebilirsiniz. Bu özelliği test etmek veya kullanmak için [laboratuvarınızda bir ortam dağıtın](devtest-lab-create-environment-from-arm.md). 

## <a name="view-security-alerts-for-an-environment"></a>Bir ortam için güvenlik uyarılarını görüntüleme

1. Laboratuvarınızın giriş sayfasında, sol taraftaki menüden **güvenlik uyarıları** ' nı seçin. Güvenlik uyarılarının sayısını (yüksek, orta ve düşük) görmeniz gerekir. [Uyarıların sınıflandırıldığı](../security-center/security-center-alerts-overview.md#how-are-alerts-classified)hakkında daha fazla bilgi edinin.

    ![Güvenlik uyarıları-genel bakış](./media/environment-security-alerts/security-alerts-overview-page.png)
2. Son sütunda üç noktaya (...) sağ tıklayın ve **güvenlik uyarılarını görüntüle**' yi seçin. 

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/view-security-alerts-menu.png)
    
3. Uyarılar ve danışman önerileri hakkında daha fazla ayrıntı görürsünüz. [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](../security-center/security-center-managing-and-responding-alerts.md)hakkında daha fazla bilgi edinin.

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/advisor-recommendations.png)


## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Resource Manager şablonlarıyla çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Genel ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
