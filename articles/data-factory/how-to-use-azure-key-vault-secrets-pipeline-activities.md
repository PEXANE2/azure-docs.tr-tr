---
title: İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma
description: Azure anahtar kasasından depolanan kimlik bilgilerini nasıl getireceğinizi ve veri fabrikası ardışık iş aktarımlarında bunları nasıl kullanacağınızı öğrenin.
services: data-factory
author: ChrisLound
manager: anandsub
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: chlound
ms.openlocfilehash: 09051ad3633ddc720cb34d3d145ccf649fa9cb08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77200121"
---
# <a name="use-azure-key-vault-secrets-in-pipeline-activities"></a>İşlem hattı etkinliklerinde Azure Key Vault gizli dizilerini kullanma

Kimlik bilgilerini veya gizli değerleri Azure Anahtar Kasası'nda saklayabilir ve bunları ardışık hat lar yürütme sırasında bunları etkinliklerinize geçmek için kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu özellik, veri fabrikası yönetilen kimliğine dayanır.  [Veri Fabrikası için Yönetilen kimlikten](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) nasıl çalıştığını öğrenin ve veri fabrikanızın ilişkili olduğundan emin olun.

## <a name="steps"></a>Adımlar

1. Veri fabrikanızın özelliklerini açın ve Yönetilen Kimlik Uygulama Kimliği değerini kopyalayın.

    ![Yönetilen Kimlik Değeri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/managedidentity.png)

2. Anahtar kasa erişim ilkelerini açın ve Sırları Al ve Listelemek için yönetilen kimlik izinlerini ekleyin.

    ![Key Vault erişim ilkeleri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies.png)

    ![Key Vault erişim ilkeleri](media/how-to-use-azure-key-vault-secrets-pipeline-activities/akvaccesspolicies-2.png)

    **Ekle'yi**tıklatın, ardından **Kaydet'i**tıklatın.

3. Key Vault sırrınıza gidin ve Gizli Tanımlayıcı'yı kopyalayın.

    ![Gizli Tanımlayıcı](media/how-to-use-azure-key-vault-secrets-pipeline-activities/secretidentifier.png)

    Veri fabrikası boru hattı çalıştırma sırasında almak istediğiniz gizli URI bir not alın.

4. Veri Fabrikası ardışık alanınızda yeni bir Web etkinliği ekleyin ve aşağıdaki gibi yapılandırın.  

    |Özellik  |Değer  |
    |---------|---------|
    |Güvenli Çıkış     |True         |
    |URL'si     |[Gizli URI değeriniz]?api-version=7.0         |
    |Yöntem     |GET         |
    |Kimlik doğrulaması     |MSI         |
    |Kaynak        |https://vault.azure.net       |

    ![Web etkinliği](media/how-to-use-azure-key-vault-secrets-pipeline-activities/webactivity.png)

    > [!IMPORTANT]
    > Gizli URI'nizin sonuna **?api-version=7.0** eklemeniz gerekir.  

    > [!CAUTION]
    > Gizli değerin düz metinde günlüğe kaydolmasını önlemek için Güvenli Çıktı seçeneğini true olarak ayarlayın.  Bu değeri tüketen diğer etkinliklerin Güvenli Giriş seçeneğinin gerçek olması gerekir.

5. Değeri başka bir etkinlikte kullanmak için aşağıdaki kod ifadesini ** @activity('Web1').output.value**kullanın.

    ![Kod ifadesi](media/how-to-use-azure-key-vault-secrets-pipeline-activities/usewebactivity.png)

## <a name="next-steps"></a>Sonraki adımlar

Veri depoları ve bilgi işlemlerinde kimlik bilgilerini depolamak için Azure Key Vault'u nasıl kullanacağınızı öğrenmek için Azure [Anahtar Kasası'nda Mağaza kimlik bilgilerine](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın
