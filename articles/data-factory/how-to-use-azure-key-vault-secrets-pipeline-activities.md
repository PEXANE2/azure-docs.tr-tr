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
ms.openlocfilehash: 837d62784a56ad0f17471cca5a660819d4a83e12
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926769"
---
# <a name="use-azure-key-vault-secrets-in-pipeline-activities"></a>İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma

Kimlik bilgilerini veya gizli değerleri bir Azure Key Vault saklayabilir ve etkinliklerinize geçirilecek işlem hattı yürütme sırasında kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, Data Factory tarafından yönetilen kimliği kullanır.  [Data Factory Için yönetilen kimliğin](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) nasıl çalıştığını öğrenin ve Data Factory 'nizin ilişkili bir tane olduğundan emin olun.

## <a name="steps"></a>Adımlar

1. Data Factory 'nizin özelliklerini açın ve yönetilen kimlik uygulama KIMLIĞI değerini kopyalayın.

    ![Yönetilen kimlik değeri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/managedidentity.png)

2. Anahtar Kasası erişim ilkelerini açın ve gizli dizileri almak ve listelemek için yönetilen kimlik izinlerini ekleyin.

    ![Key Vault erişim ilkeleri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies.png)

    ![Key Vault erişim ilkeleri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies-2.png)

    **Ekle**' ye ve ardından **Kaydet**' e tıklayın.

3. Key Vault verilerinize giderek gizli dizi tanımlayıcısını kopyalayın.

    ![Gizli Dizi Tanımlayıcısı](media/how-to-use-azure-key-vault-secrets-pipeline-activities/secretidentifier.png)

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

5. Değeri başka bir etkinlikte kullanmak için aşağıdaki kod ifadesini kullanın **@activity("Web"). Output. Value)** .

    ![Kod ifadesi](media/how-to-use-azure-key-vault-secrets-pipeline-activities/usewebactivity.png)

## <a name="next-steps"></a>Sonraki adımlar

Veri depoları ve hesaplar için kimlik bilgilerini depolamak üzere Azure Key Vault nasıl kullanacağınızı öğrenmek için bkz. [Azure Key Vault kimlik bilgilerini depolama](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
