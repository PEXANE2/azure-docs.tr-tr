---
title: Azure Veri Gezgini kümenizin güvenliğini sağlama
description: Bu makalede, Azure portal içindeki Azure Veri Gezgini kümenizin güvenliğini nasıl güvence altına almak açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 0f935999b68a7283c032d43c42d688b273d5c450
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75720353"
---
# <a name="secure-your-cluster-in-azure-data-explorer---azure-portal"></a>Azure Veri Gezgini Azure portal kümenizi güvenli hale getirme

[Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview) , kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar. Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize olanak tanıyan ve VM disklerindeki tüm verilerin şifrelendiğinden emin olmak için [Azure Key Vault](/azure/key-vault/)ile tümleşir. 
  
## <a name="enable-encryption-at-rest-in-the-azure-portal"></a>Azure portal bekleyen şifrelemeyi etkinleştir
  
Küme güvenlik ayarlarınız, kümenizde disk şifrelemeyi etkinleştirmenizi sağlar. Kümenizdeki [geri kalan şifrelemeyi](/azure/security/fundamentals/encryption-atrest) etkinleştirmek, depolanan veriler için veri koruması sağlar (bekleyen). 

1. Azure portal Azure Veri Gezgini küme kaynağına gidin. **Ayarlar** başlığı altında **güvenlik**' i seçin. 

    ![Bekleyen şifrelemeyi aç](media/manage-cluster-security/security-encryption-at-rest.png)

1. **Güvenlik** penceresinde, **disk şifreleme** güvenlik ayarı için **Açık** ' ı seçin. 

1. **Kaydet**’i seçin.
 
> [!NOTE]
> Etkinleştirildikten sonra şifrelemeyi devre dışı bırakmak için **kapalı** ' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](/azure/data-explorer/check-cluster-health)
