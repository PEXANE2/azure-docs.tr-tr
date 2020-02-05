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
ms.openlocfilehash: 4ca17bece33107de756eb221e14eaab851660a99
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992241"
---
# <a name="security-alerts-for-environments-in-azure-devtest-labs"></a>Azure DevTest Labs ortamlar için güvenlik uyarıları
Bu makalede, Azure DevTest Labs ortamlar için güvenlik uyarılarını nasıl görüntüleyebileceğiniz gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
Şu anda güvenlik uyarılarını yalnızca laboratuvarınıza dağıtılan ortamlar için görüntüleyebilirsiniz. Bu özelliği test etmek veya kullanmak için laboratuvarınızda bir ortam dağıtın. 

## <a name="view-security-alerts-for-an-environment"></a>Bir ortam için güvenlik uyarılarını görüntüleme

1. Laboratuvarınızın giriş sayfasında, sol taraftaki menüden **güvenlik uyarıları** ' nı seçin. Güvenlik uyarılarının sayısını (yüksek, orta ve düşük) görmeniz gerekir.

    ![Güvenlik uyarıları-genel bakış](./media/environment-security-alerts/security-alerts-overview-page.png)
2. Son sütunda üç noktaya (...) sağ tıklayın ve **güvenlik uyarılarını görüntüle**' yi seçin. 

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/view-security-alerts-menu.png)
3. Uyarılar ve danışman önerileri hakkında daha fazla ayrıntı görürsünüz. 

    ![Güvenlik uyarılarını görüntüleme](./media/environment-security-alerts/advisor-recommendations.png)


## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Resource Manager şablonlarıyla çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Ortak ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
