---
title: Azure Veri Gezgini kümenizin güvenliğini sağlama
description: Bu makalede, Azure portal içindeki Azure Veri Gezgini kümenizin güvenliğini nasıl güvence altına almak açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: 86350f21f5c530a00560c92cc0ae2fd58c9a2c57
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780052"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Azure Veri Gezgini kümenizin güvenliğini sağlama

[Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview) , kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar. Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](/azure/key-vault/) ile TÜMLEŞIR ve VM disklerindeki tüm verilerin Azure depolama sırasında bekleyen olarak şifrelenmesini sağlar. Küme güvenlik ayarlarınız, kümenizde disk şifrelemeyi etkinleştirmenizi sağlar.
  
## <a name="enable-encryption-at-rest"></a>Bekleyen şifrelemeyi etkinleştir
  
Kümenizdeki [geri kalan şifrelemeyi](/azure/security/fundamentals/encryption-atrest) etkinleştirmek, depolanan veriler için veri koruması sağlar (bekleyen). 

1. Azure portal Azure Veri Gezgini küme kaynağına gidin. **Ayarlar** başlığı altında **güvenlik**' i seçin. 

    ![Bekleyen şifrelemeyi aç](media/manage-cluster-security/security-encryption-at-rest.png)

1. **Güvenlik** penceresinde, **disk şifreleme** güvenlik ayarı için **Açık** ' ı seçin. 

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](/azure/data-explorer/check-cluster-health)
