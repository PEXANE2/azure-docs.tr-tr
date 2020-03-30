---
title: Azure DevTest Labs'daki ortamlar için güvenlik uyarıları
description: Bu makalede, DevTest Labs bir ortam için güvenlik uyarıları görüntülemek ve uygun bir eylem icra nasıl gösterilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588714"
---
# <a name="security-alerts-for-environments-in-azure-devtest-labs"></a>Azure DevTest Labs'daki ortamlar için güvenlik uyarıları
Laboratuvar kullanıcısı olarak artık laboratuvar ortamlarınız için Azure Güvenlik Merkezi uyarılarını görüntüleyebilirsiniz. Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir. [Azure Güvenlik Merkezi'ndeki güvenlik uyarıları hakkında daha fazla bilgi edinin.](../security-center//security-center-alerts-overview.md)  


## <a name="prerequisites"></a>Ön koşullar
Şu anda, güvenlik uyarılarını yalnızca platform için laboratuarınıza dağıtılan bir hizmet (PaaS) ortamları olarak görüntüleyebilirsiniz. Bu özelliği sınamak veya kullanmak [için, bir ortamı laboratuarınıza dağıtın.](devtest-lab-create-environment-from-arm.md) 

## <a name="view-security-alerts-for-an-environment"></a>Ortam için güvenlik uyarılarını görüntüleme

1. Laboratuvarınızın ana sayfasında sol menüde **Güvenlik uyarıları'nı** seçin. Güvenlik uyarılarının (yüksek, orta ve düşük) sayısını görmeniz gerekir. [Uyarıların nasıl sınıflandırış ları](../security-center/security-center-alerts-overview.md#how-are-alerts-classified)hakkında daha fazla bilgi edinin.

    ![Güvenlik uyarıları - genel bakış](./media/environment-security-alerts/security-alerts-overview-page.png)
2. Son sütundaki üç noktanın (...) sağ tıklayın ve **güvenlik uyarılarını görüntüle'yi**seçin. 

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/view-security-alerts-menu.png)
    
3. Uyarılar ve danışman önerileri hakkında daha fazla ayrıntı görürsünüz. [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtverme](../security-center/security-center-managing-and-responding-alerts.md)hakkında daha fazla bilgi edinin.

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/advisor-recommendations.png)


## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Kaynak Yöneticisi şablonları ile çoklu vm ortamları ve PaaS kaynakları oluşturun](devtest-lab-create-environment-from-arm.md)
- [Genel ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
