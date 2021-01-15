---
title: "Öğretici: Azure 'da bilişsel hizmetler için önkoşulları SYNAPSE"
description: Azure 'da bilişsel hizmetler 'i kullanmaya ilişkin önkoşulları yapılandırma öğreticisi SYNAPSE
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: eef65db05ab94b5b8de5ff82c2c51dba0730f170
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98222182"
---
# <a name="tutorial-pre-requisites-for-using-cognitive-services-in-azure-synapse"></a>Öğretici: Azure 'da bilişsel hizmetler 'i kullanmaya yönelik önkoşulların önkoşulları SYNAPSE

Bu öğreticide, Azure SYNAPSE 'de bilişsel hizmetler 'i güvenli bir şekilde kullanmak için önkoşulları nasıl ayarlayacağınızı öğreneceksiniz.

Bu öğreticinin içindekiler:
> [!div class="checklist"]
> - Bilişsel hizmetler kaynakları oluşturun. Örneğin Metin Analizi veya anomali algılayıcısı.
> - Hizmet kaynaklarına Azure Anahtar Kasası 'nda gizli dizi olarak kimlik doğrulama anahtarını depolayın ve Azure SYNAPSE çalışma alanı için erişimi yapılandırın.
> - Azure SYNAPSE Analytics çalışma alanınızda Azure Anahtar Kasası bağlı hizmetini oluşturun.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir ADLS 2. depolama hesabıyla [Azure SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız ADLS 2. FileSystem 'ın **Depolama Blobu veri katılımcısı** olması gerekir.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="create-a-cognitive-services-resource"></a>Bilişsel hizmetler kaynağı oluşturma

[Azure](../../cognitive-services/index.yml) bilişsel hizmetler birçok farklı hizmet türü içerir. Aşağıda, SYNAPSE öğreticilerinde kullanılan bazı örnekler verilmiştir.

### <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma
Azure portal bir [anomali algılayıcısı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) oluşturun.

![Anomali algılayıcısı oluşturma](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00a.png)

### <a name="create-a-text-analytics-resource"></a>Metin Analizi kaynağı oluşturma
Azure portal [metin analizi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) bir kaynak oluşturun.

![Metin analizi oluşturma](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00b.png)

## <a name="create-key-vault-and-configure-secrets-and-access"></a>Key Vault oluşturma ve parolaları ve erişimi yapılandırma

1. Azure portal [Key Vault](https://ms.portal.azure.com/#create/Microsoft.KeyVault) oluşturun.
2. **Key Vault > erişim ilkelerine** gidin ve Azure Key Vault gizli dizileri okumak Için [Azure SYNAPSE çalışma alanı MSI](../security/synapse-workspace-managed-identity.md) izinleri verin.

>İlke değişikliklerinin kaydedildiğinden emin olun. Bu adımın kaçırılması kolaydır.

![Erişim İlkesi Ekle](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00c.png)

3. Bilişsel hizmet kaynağına gidin, örneğin **anomali algılayıcı-> anahtarlar ve uç nokta**, iki anahtardan birini Pano 'ya kopyalayın.

4. Yeni bir gizli dizi oluşturmak için **Key Vault > gizli** sayfasına gidin. Gizli anahtar adını belirtin ve ardından önceki adımdan "değer" alanına yapıştırın. Son olarak, **Oluştur**'a tıklayın.

![Gizli dizi oluştur](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00d.png)

> Bu gizli adı anımsadığınızdan veya aklınızda olduğunuzdan emin olun! Daha sonra, Azure SYNAPSE Studio 'dan bilişsel hizmetlere bağlandığınızda kullanacaksınız.

## <a name="create-azure-keyvault-linked-service-in-azure-synapse"></a>Azure 'da Azure Anahtar Kasası bağlı hizmeti oluşturma SYNAPSE

1. Azure SYNAPSE Studio 'da çalışma alanınızı açın. **Yönet-> bağlı hizmetleri**' ne gidin. Yeni oluşturduğumuz Key Vault işaret eden AB "Azure Key Vault" bağlı hizmeti oluşturun. Sonra, "Bağlantıyı Sına" düğmesine tıklayıp yeşil olup olmadığını kontrol ederek bağlantıyı doğrulayın. Herhangi bir şey düzgün çalışıyorsa, önce "Oluştur" düğmesine tıklayın ve ardından "tümünü Yayımla" ' ya tıklayarak değişikliğinizi kaydedin.
![Bağlı hizmet](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00e.png)

Artık Azure SYNAPSE Studio 'da Azure bilişsel hizmetler deneyimini kullanmaya yönelik öğreticilerden biriyle devam etmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure bilişsel hizmetler ile yaklaşım Analizi](tutorial-cognitive-services-sentiment.md)
- [Öğretici: Azure bilişsel hizmetler ile anomali algılama](tutorial-cognitive-services-sentiment.md)
- [Öğretici: Azure SYNAPSE ADANMıŞ SQL havuzlarında makine öğrenimi modeli Puanlama](tutorial-sql-pool-model-scoring-wizard.md).
- [Azure SYNAPSE Analytics 'teki Machine Learning özellikleri](what-is-machine-learning.md)