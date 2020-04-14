---
title: FarmBeats için felaket kurtarma
description: Bu makalede, veri kurtarma verilerinizi kaybetmeye karşı nasıl korur açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 04/13/2020
ms.author: v-umha
ms.openlocfilehash: fbda28ce588aad5f6bc0d89de60069c4220fa523
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266181"
---
# <a name="disaster-recovery-for-farmbeats"></a>FarmBeats için felaket kurtarma

Veri kurtarma, Azure bölgesinin çökmesi gibi bir durumda verilerinizi kaybetmenizi sağlar. Böyle bir durumda, başarısız olmaya başlayabilir ve FarmBeats dağıtımınızda depolanan verileri kurtarabilirsiniz.

Veri kurtarma, Azure FarmBeats'te varsayılan bir özellik değildir. Verileri Azure eşleştirilmiş bir bölgede depolamak için FarmBeats tarafından kullanılan gerekli Azure kaynaklarını yapılandırarak bu özelliği el ile yapılandırabilirsiniz. Kurtarmayı etkinleştirmek için Etkin – Pasif yaklaşımını kullanın.

Aşağıdaki bölümler, Azure FarmBeats'te veri kurtarmayı nasıl yapılandırabileceğiniz hakkında bilgi sağlar:

- [Veri artıklığını etkinleştirme](#enable-data-redundancy)
- [Çevrimiçi yedeklemeden hizmeti geri yükleme](#restore-service-from-online-backup)


## <a name="enable-data-redundancy"></a>Veri artıklığını etkinleştirme

FarmBeats, verileri **Azure depolama**, **Cosmos DB** ve **Time Series Insights**olan üç Azure birinci taraf hizmetinde depolar. Bu hizmetler için eşleştirilmiş bir Azure bölgesine veri artıklığını etkinleştirmek için aşağıdaki adımları kullanın:

1.  **Azure Depolama** - FarmBbeats dağıtımınızdaki her depolama hesabı için veri artıklığını etkinleştirmek için bu kılavuzu izleyin.
2.  **Azure Cosmos DB** - FarmBeats dağıtımınız için Cosmos DB hesabı için veri artıklığını etkinleştirmek için bu kılavuzu izleyin.
3.  **Azure Zaman Serisi Öngörüleri (TSI) - TSI** şu anda veri artıklığı sunmuyor. Time Series Insights verilerini kurtarmak için sensör/hava durumu ortağınıza gidin ve verileri FarmBeats dağıtımına yeniden itin.

## <a name="restore-service-from-online-backup"></a>Çevrimiçi yedeklemeden hizmeti geri yükleme

FarmBeats dağıtımınız için yukarıda belirtilen veri depolarının her biri için saklanan failover'ı başlatabilir ve depolanan verileri kurtarabilirsiniz. Azure depolama ve Cosmos DB verilerini kurtardıktan sonra, Azure eşleştirilmiş bölgede başka bir FarmBeats dağıtımı oluşturun ve ardından yeni dağıtımı, geri yüklenen veri depolarından (örneğin Azure Depolama ve Cosmos DB) aşağıdaki adımları kullanarak verileri kullanacak şekilde yapılandırın:

1. [Cosmos DB’yi yapılandırma](#configure-cosmos-db)
2. [Depolama Hesabını Yapılandırma](#configure-storage-account)


### <a name="configure-cosmos-db"></a>Cosmos DB’yi yapılandırma

Geri yüklenen Cosmos DB'nin erişim anahtarını kopyalayın ve yeni FarmBeats Datahub Key Vault'u güncelleyin.


  ![Olağanüstü Durum Kurtarma](./media/disaster-recovery-for-farmbeats/keyvault-secrets.png)

> [!NOTE]
> Geri yüklenen Cosmos DB'nin URL'sini kopyalayın ve yeni FarmBeats Datahub App Service Configuration'da güncelleyin. Artık yeni FarmBeats dağıtımında Cosmos DB hesabını silebilirsiniz.

  ![Olağanüstü Durum Kurtarma](./media/disaster-recovery-for-farmbeats/northeu-ehub-api-configuration.png)

### <a name="configure-storage-account"></a>Depolama Hesabını Yapılandırma

Geri yüklenen depolama hesabının erişim anahtarını kopyalayın ve yeni FarmBeats Datahub Key Vault'ta güncelleyin.

![Olağanüstü Durum Kurtarma](./media/disaster-recovery-for-farmbeats/keyvault-7udqm-secrets.png)

>[!NOTE]
> Yeni FarmBeats Toplu VM config dosyasında Depolama Hesabı adını güncelleştirin.

![Olağanüstü Durum Kurtarma](./media/disaster-recovery-for-farmbeats/batch-prep-files.png)

Benzer şekilde, Hızlandırıcı depolama hesabınız için veri kurtarmayı etkinleştirdiyseniz, yeni FarmBeats örneğinde hızlandırıcı Depolama Hesabı erişim anahtarını ve adını güncelleştirmek için 2 adımını izleyin.
