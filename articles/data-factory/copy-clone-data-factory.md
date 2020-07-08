---
title: Azure Data Factory bir veri fabrikası kopyalama veya kopyalama
description: Azure Data Factory ' de bir veri fabrikasını kopyalama veya kopyalama hakkında bilgi edinin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: chez-charlie
ms.author: chez
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 304c39f4b6f7852068d4e72adfad2d41eeefc26c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85552974"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>Azure Data Factory bir veri fabrikası kopyalama veya kopyalama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Data Factory ' de bir veri fabrikasının nasıl kopyalanacağı veya kopyalanacağı açıklanır.

## <a name="use-cases-for-cloning-a-data-factory"></a>Veri fabrikasını kopyalamak için kullanım örnekleri

Veri fabrikasını kopyalamak veya klonlamak için yararlı bulabileceğiniz durumlardan bazıları şunlardır:

- **Data Factory** yeni bir bölgeye taşıyın. Data Factory farklı bir bölgeye taşımak istiyorsanız, en iyi yol hedeflenen bölgede bir kopya oluşturmak ve var olanı silmek.

- **Data Factory yeniden adlandırılıyor**. Azure, kaynakların yeniden adlandırılmasını desteklemez. Bir veri fabrikasını yeniden adlandırmak isterseniz, veri fabrikasını farklı bir adla kopyalayabilir ve var olanı silebilirsiniz.

- Hata ayıklama özellikleri yeterli olmadığında **değişiklikleri hata ayıklama** . Çoğu senaryoda [Hata Ayıkla](iterative-development-debugging.md)' yı kullanabilirsiniz. Diğer bir deyişle, kopyalanmış bir korumalı alan ortamındaki değişiklikleri test etmek daha mantıklı olur. Örneğin, parametreli ETL işlem hatlarınız, bir tetikleme zaman penceresi üzerine geldiğinde bir tetikleyici tetiklendiğinde, tek başına hata ayıklama aracılığıyla kolayca test edilemeyebilir. Bu durumlarda, denemek için bir korumalı alan ortamı klonlamak isteyebilirsiniz. Azure Data Factory, öncelikle çalışma sayısına göre ücretlendirdiğinden, ikinci bir fabrika hiçbir ek ücret vermez.

## <a name="how-to-clone-a-data-factory"></a>Veri fabrikasını kopyalama

1. Önkoşul olarak, ilk olarak Azure portal hedef veri fabrikanızı oluşturmanız gerekir.

1. GIT modundaysanız:
    1. Portaldan her yayımladığınızda, fabrikanızın Kaynak Yöneticisi şablonu ADF yayımlama dalında GIT 'e kaydedilir \_
    1. Yeni fabrikasını _aynı_ depoya bağlayın ve ADF \_ Yayımlama dalından derleyin. İşlem hatları, veri kümeleri ve tetikleyiciler gibi kaynaklar

1. Live moddasınız:
    1. Data Factory UI, veri fabrikanızın tüm yükünü bir Kaynak Yöneticisi şablon dosyasına ve bir parametre dosyasına aktarmanızı sağlar. Bu kişiler, portaldaki **ARM şablonu \ dışarı aktar Kaynak Yöneticisi şablonu** düğmesinden erişilebilir.
    1. Parametre dosyasında uygun değişiklikleri yapabilir ve yeni fabrika için yeni değerlerle takas edebilirsiniz
    1. Ardından, standart Kaynak Yöneticisi şablonu dağıtım yöntemleri aracılığıyla dağıtabilirsiniz.

1. Kaynak fabrikasında SelfHosted bir tümleştirme çalışma zamanı varsa, bu adı hedef fabrikasında aynı adla oluşturmanız gerekir. Şirket içinde barındırılan Integration Runtime farklı fabrikalar arasında paylaştırmak istiyorsanız, [burada](create-shared-self-hosted-integration-runtime-powershell.md) yayımlanan ve kendını barındıran IR 'yi paylaşma sayfasında yayımlanmış olan kalıbı kullanabilirsiniz.

1. Güvenlik nedenleriyle, oluşturulan Kaynak Yöneticisi şablonu herhangi bir gizli bilgi (örneğin, bağlı hizmetler için parolalar) içermez. Bu nedenle, kimlik bilgilerini dağıtım parametreleri olarak sağlamanız gerekir. Ayarlarınız için el ile giriş kimlik bilgisi istenmemişse, lütfen bunun yerine bağlantı dizelerini ve parolaları Azure Key Vault almayı göz önünde bulundurun. [Daha fazla göster](store-credentials-in-key-vault.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Factory Kullanıcı arabirimini kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)içindeki Azure Portal veri fabrikası oluşturma kılavuzunu gözden geçirin.
