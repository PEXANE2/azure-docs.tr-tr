---
title: Azure Veri Gezgini kümenizin güvenliğini sağlama
description: Bu makalede, Azure portal içindeki Azure Veri Gezgini kümenizin güvenliğini nasıl güvence altına almak açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: ad08bb19f96aadad42e973eebb8adce6875e07b1
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876588"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Azure Veri Gezgini kümenizin güvenliğini sağlama

[Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview) , kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar. Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](/azure/key-vault/) ile TÜMLEŞIR ve VM disklerindeki tüm verilerin Azure depolama sırasında bekleyen olarak şifrelenmesini sağlar. Küme güvenlik ayarlarınız, kümenizde disk şifrelemeyi etkinleştirmenizi sağlar.
  
## <a name="enable-encryption-at-rest"></a>Bekleyen şifrelemeyi etkinleştir
  
Kümenizdeki [geri kalan şifrelemeyi](/azure/security/fundamentals/encryption-atrest) etkinleştirmek, depolanan veriler için veri koruması sağlar (bekleyen). 

1. Azure portal Azure Veri Gezgini küme kaynağına gidin. **Ayarlar** başlığı altında **güvenlik**' i seçin. 

    ![Bekleyen şifrelemeyi aç](media/manage-cluster-security/security-encryption-at-rest.png)

1. **Güvenlik** penceresinde, **disk şifreleme** güvenlik ayarı için **Açık** ' ı seçin. 

1. **Kaydet**’i seçin.
 
> [!NOTE]
> Şifreleme etkinleştirildikten sonra devre dışı bırakılabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](/azure/data-explorer/check-cluster-health)
