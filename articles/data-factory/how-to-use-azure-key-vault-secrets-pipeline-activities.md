---
title: İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma
description: Azure Anahtar Kasası 'nda depolanan kimlik bilgilerini getirme ve bunları Data Factory işlem hattı çalıştırmaları sırasında kullanma hakkında bilgi edinin.
services: data-factory
author: ChrisLound
manager: anandsub
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: chlound
ms.openlocfilehash: 1766705e73afab5d15cdb5aa2c5bb1487ad3d7c5
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92634292"
---
# <a name="use-azure-key-vault-secrets-in-pipeline-activities"></a>İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Kimlik bilgilerini veya gizli değerleri bir Azure Key Vault saklayabilir ve etkinliklerinize geçirilecek işlem hattı yürütme sırasında kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu özellik, Data Factory tarafından yönetilen kimliği kullanır.  [Data Factory Için yönetilen kimliğin](./data-factory-service-identity.md) nasıl çalıştığını öğrenin ve Data Factory 'nizin ilişkili bir tane olduğundan emin olun.

## <a name="steps"></a>Adımlar

1. Data Factory 'nizin özelliklerini açın ve yönetilen kimlik uygulama KIMLIĞI değerini kopyalayın.

    ![Yönetilen kimlik değeri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/managedidentity.png)

2. Anahtar Kasası erişim ilkelerini açın ve gizli dizileri almak ve listelemek için yönetilen kimlik izinlerini ekleyin.

    !["Erişim Ilkesi Ekle" eylemi vurgulanmış "erişim ilkeleri" sayfasını gösteren ekran görüntüsü.](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies.png)

    ![Key Vault erişim ilkeleri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies-2.png)

    **Ekle** ' ye ve ardından **Kaydet** ' e tıklayın.

3. Key Vault verilerinize giderek gizli dizi tanımlayıcısını kopyalayın.

    ![Gizli dizi tanımlayıcısı](media/how-to-use-azure-key-vault-secrets-pipeline-activities/secretidentifier.png)

    Data Factory işlem hattı çalıştırma sırasında almak istediğiniz gizli URI 'nizi bir yere göz önünde bir şekilde oluşturun.

4. Data Factory işlem hattınızda yeni bir Web etkinliği ekleyin ve bunu aşağıdaki şekilde yapılandırın.  

    |Özellik  |Değer  |
    |---------|---------|
    |Güvenli çıkış     |Doğru         |
    |URL     |[Gizli URI değeri]? api-version = 7.0         |
    |Yöntem     |GET         |
    |Kimlik Doğrulaması     |MSI         |
    |Kaynak        |https://vault.azure.net       |

    ![Web etkinliği](media/how-to-use-azure-key-vault-secrets-pipeline-activities/webactivity.png)

    > [!IMPORTANT]
    > Gizli URI 'nizin sonuna **? api-version = 7.0** eklemeniz gerekir.  

    > [!CAUTION]
    > Gizli çıkışın düz metin olarak kaydedilmesini engellemek için güvenli çıkış seçeneğini true olarak ayarlayın.  Bu değeri kullanan diğer etkinliklerin, güvenli giriş seçeneğinin true olarak ayarlanmış olması gerekir.

5. Değeri başka bir etkinlikte kullanmak için şu kod ifadesini kullanın **@activity (' Web1 '). Output. Value** .

    ![Kod ifadesi](media/how-to-use-azure-key-vault-secrets-pipeline-activities/usewebactivity.png)

## <a name="next-steps"></a>Sonraki adımlar

Veri depoları ve hesaplar için kimlik bilgilerini depolamak üzere Azure Key Vault nasıl kullanacağınızı öğrenmek için bkz. [Azure Key Vault kimlik bilgilerini depolama](./store-credentials-in-key-vault.md)