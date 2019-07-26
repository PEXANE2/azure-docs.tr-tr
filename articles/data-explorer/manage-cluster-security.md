---
title: Azure Veri Gezgini kümenizin güvenliğini sağlama
description: Bu makalede, Azure portal içindeki Azure Veri Gezgini kümenizin güvenliğini nasıl güvence altına almak açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: fbc5b18de093b2c91b17fa310c08a5b02b113a22
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406528"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Azure Veri Gezgini kümenizin güvenliğini sağlama

[Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview) , kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korumanıza ve korumaya yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar. Ayrıca, disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](/azure/key-vault/) ile TÜMLEŞIR ve VM disklerindeki tüm verilerin Azure depolama sırasında bekleyen olarak şifrelenmesini sağlar. Küme güvenlik ayarlarınız, kümenizde disk şifrelemeyi etkinleştirmenizi sağlar.
  
## <a name="enable-encryption-at-rest"></a>Bekleyen şifrelemeyi etkinleştir
  
Kümenizdeki [geri kalan şifrelemeyi](/azure/security/azure-security-encryption-atrest) etkinleştirmek, depolanan veriler için veri koruması sağlar (bekleyen). 

1. Azure portal Azure Veri Gezgini küme kaynağına gidin. **Ayarlar** başlığı altında **güvenlik**' i seçin. 

    ![Bekleyen şifrelemeyi aç](media/manage-cluster-security/security-encryption-at-rest.png)

1. **Güvenlik** penceresinde, **disk şifreleme** güvenlik ayarı için **Açık** ' ı seçin. 

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](/azure/data-explorer/check-cluster-health)
