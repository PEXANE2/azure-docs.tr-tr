---
title: Azure Veri Fabrikası'nda bir veri fabrikasını kopyalama veya klonlama
description: Azure Veri Fabrikası'nda bir veri fabrikasını kopyalama veya klonlama hakkında bilgi edinin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 01/09/2019
ms.openlocfilehash: 5e44bda8648fbf26487b04cf36a8fd0ec085c411
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414101"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>Azure Veri Fabrikası'nda bir veri fabrikasını kopyalama veya klonlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Veri Fabrikası'ndaki bir veri fabrikasının nasıl kopyalanır veya klonlanır.

## <a name="use-cases-for-cloning-a-data-factory"></a>Veri fabrikasını klonlamak için servis taleplerini kullanma

Bir veri fabrikasını kopyalamayı veya klonlamayı yararlı bulabileceğiniz durumlardan bazıları şunlardır:

-   **Kaynakları yeniden adlandırma.** Azure, kaynakları yeniden adlandırmayı desteklemez. Bir veri fabrikasını yeniden adlandırmak istiyorsanız, veri fabrikasını farklı bir adla klonlayabilir ve varolan ını silebilirsiniz.

-   Hata ayıklama özellikleri yeterli olmadığında hata **ayıklama değişiklikleri.** Bazen değişikliklerinizi test etmek için, değişikliklerinizi ana fabrikanıza uygulamadan önce farklı bir fabrikada test etmek isteyebilirsiniz. Çoğu senaryoda Hata Ayıklama'yı kullanabilirsiniz. Ancak tetikleyicilerde, tetikleyici otomatik olarak çağrıldığınızda veya bir zaman penceresinde değişikliklerinizin nasıl hareket edilebileceği gibi değişiklikler, teslim olmadan kolayca sınanamaz. Bu gibi durumlarda, fabrika klonlama ve orada değişiklikleri uygulayarak çok mantıklı. Azure Veri Fabrikası öncelikle çalıştırma sayısına göre ücretlendirme yaptığından, ikinci fabrika ek ücretlendirmeye yol açmaz.

## <a name="how-to-clone-a-data-factory"></a>Bir veri fabrikasını klonlama

1. Azure portalındaki Veri Fabrikası UI,, fabrikanızı klonlarken değiştirmek istediğiniz değerleri değiştirmenize olanak tanıyan bir parametre dosyasıyla birlikte veri fabrikanızın tüm yükünü Bir Kaynak Yöneticisi şablonuna aktarmanızı sağlar.

1. Ön koşul olarak, Azure portalından hedef veri fabrikanızı oluşturmanız gerekir.

1. Kaynak fabrikanızda SelfHosted IntegrationRuntime varsa, hedef fabrikada aynı adla oluşturmanız gerekir. SelfHosted IRs'leri farklı fabrikalar arasında paylaşmak istiyorsanız, [burada](source-control.md#best-practices-for-git-integration)yayınlanan deseni kullanabilirsiniz.

1. GIT modundaysanız, portaldan her yayımladığınızda, fabrikanın Kaynak Yöneticisi şablonu deponun adf_publish dalında GIT'e kaydedilir.

1. Diğer senaryolar için Kaynak Yöneticisi şablonu portaldaki **Dışa Aktarma Kaynak Yöneticisi şablon** düğmesine tıklayarak indirilebilir.

1. Kaynak Yöneticisi şablonu'nu indirdikten sonra, standart Kaynak Yöneticisi şablondağıtım yöntemleri yle dağıtabilirsiniz.

1. Güvenlik nedenleriyle, oluşturulan Kaynak Yöneticisi şablonu bağlantılı hizmetlerin parolaları gibi gizli bilgiler içermez. Sonuç olarak, bu parolaları dağıtım parametreleri olarak sağlamanız gerekir. Parametreleri sağlamak istenmiyorsa, bağlı hizmetlerin bağlantı dizelerini ve parolalarını Azure Key Vault'tan almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Veri Fabrikası Web'si'ni kullanarak veri fabrikası oluşturma'daki](quickstart-create-data-factory-portal.md)Azure portalında bir veri fabrikası oluşturma kılavuzunu gözden geçirin.
