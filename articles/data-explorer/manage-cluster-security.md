---
title: Azure Veri Gezgini'nde kümenizi güvenli hale
description: Bu makalede, Azure portalı içinde Azure Veri Gezgini'nde kümenizin nasıl güvenli hale verilebildiğini açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 0f935999b68a7283c032d43c42d688b273d5c450
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75720353"
---
# <a name="secure-your-cluster-in-azure-data-explorer---azure-portal"></a>Azure Veri Gezgini'nde kümenizi güvenli hale - Azure portalı

[Azure Disk Şifreleme,](/azure/security/azure-security-disk-encryption-overview) kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamak için verilerinizin korunmasına ve korunmasına yardımcı olur. Küme sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifreleme sağlar. Ayrıca, disk şifreleme anahtarlarını ve sırlarını kontrol etmemizi ve yönetmemizi ve VM disklerüzerindeki tüm verilerin şifrelenmesini sağlamamızı sağlayan [Azure Key Vault](/azure/key-vault/)ile de entegre olur. 
  
## <a name="enable-encryption-at-rest-in-the-azure-portal"></a>Azure portalında şifrelemeyi dinlenme de etkinleştirme
  
Küme güvenliği ayarlarınız, kümenizde disk şifrelemesini etkinleştirmenize olanak tanır. Kümenizde [şifrelemeyi](/azure/security/fundamentals/encryption-atrest) etkinleştirmek, depolanan veriler için veri koruması sağlar (istirahatte). 

1. Azure portalında Azure Veri Gezgini küme kaynağınıza gidin. **Ayarlar** başlığı altında **Güvenlik'i**seçin. 

    ![Şifrelemeyi istirahatte açma](media/manage-cluster-security/security-encryption-at-rest.png)

1. **Güvenlik** penceresinde, **Disk şifreleme** güvenlik ayarı için **Aç'ı** seçin. 

1. **Kaydet'i**seçin.
 
> [!NOTE]
> Etkinleştirildikten sonra şifrelemeyi devre dışı kakmak için **Kapalı'yı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Küme durumunu denetleme](/azure/data-explorer/check-cluster-health)
