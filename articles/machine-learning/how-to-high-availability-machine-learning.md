---
title: Dayanıklılık & yüksek kullanılabilirlik
titleSuffix: Azure Machine Learning
description: Yüksek kullanılabilirliğe sahip bir yapılandırma kullanarak Azure Machine Learning kaynaklarınızı kesintileri daha dayanıklı hale getirme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jhirono
author: jhirono
ms.reviewer: larryfr
ms.date: 09/16/2020
ms.openlocfilehash: 7a1a63893e6e2988fc5f21e84f21c74315d856b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "93325488"
---
# <a name="increase-azure-machine-learning-resiliency"></a>Azure Machine Learning dayanıklılığı artırın



Bu makalede, yüksek kullanılabilirliğe sahip yapılandırmaların kullanılmasıyla Microsoft Azure Machine Learning kaynaklarınızın nasıl daha dayanıklı hale alınacağını öğreneceksiniz. Azure Machine Learning yüksek kullanılabilirlik için bağlı olan Azure hizmetlerini yapılandırabilirsiniz. Bu makalede, yüksek kullanılabilirlik için yapılandırabileceğiniz hizmetler ve bu kaynakları yapılandırmayla ilgili ek bilgilere bağlantılar tanımlanmaktadır.

> [!NOTE]
> Azure Machine Learning kendisi bir olağanüstü durum kurtarma seçeneği sunmaz.

## <a name="understand-azure-services-for-azure-machine-learning"></a>Azure Machine Learning için Azure hizmetlerini anlama

Azure Machine Learning birden çok Azure hizmetine bağlıdır ve birkaç katmana sahiptir. Bu hizmetlerden bazıları (müşteri) aboneliğinizde temin edilir. Bu hizmetlerin yüksek kullanılabilirlik yapılandırmasından sorumlu olursunuz. Diğer hizmetler bir Microsoft aboneliğinde oluşturulur ve Microsoft tarafından yönetilir. 

Azure Hizmetleri şunları içerir:

* **Azure Machine Learning altyapısı**: Azure Machine Learning çalışma alanı için Microsoft tarafından yönetilen bir ortam.

* **İlişkili kaynaklar**: Azure Machine Learning çalışma alanı oluşturma sırasında aboneliğinizde sağlanan kaynaklar. Bu kaynaklara Azure depolama, Azure Key Vault, Azure Container Registry ve Application Insights dahildir. Bu kaynaklar için yüksek kullanılabilirlik ayarlarını yapılandırmadan sorumlu olursunuz.
  * Varsayılan depolamada model, eğitim günlüğü verileri ve veri kümesi gibi veriler bulunur.
  * Key Vault Azure depolama, Container Registry ve veri depoları için kimlik bilgileri bulunur.
  * Container Registry eğitim ve ınırel bir ortamlar için Docker görüntüsüne sahiptir.
  * Application Insights izleme Azure Machine Learning içindir.

* **İşlem kaynakları**: çalışma alanı dağıtımından sonra oluşturduğunuz kaynaklar. Örneğin, bir Machine Learning modeli eğitmek için bir işlem örneği veya işlem kümesi oluşturabilirsiniz.
  * İşlem örneği ve işlem kümesi: Microsoft tarafından yönetilen model geliştirme ortamları.
  * Diğer kaynaklar: Azure Kubernetes Service (AKS), Azure Databricks, Azure Container Instances ve Azure HDInsight gibi Azure Machine Learning ekleyebileceğiniz Microsoft Bilgi işlem kaynakları. Bu kaynaklar için yüksek kullanılabilirlik ayarlarını yapılandırmadan sorumlu olursunuz.

* **Ek veri depoları**: Azure Machine Learning Azure depolama, Azure Data Lake Storage ve eğitim verileri IÇIN Azure SQL veritabanı gibi ek veri depoları bağlayabilir.  Bu veri depoları aboneliğiniz dahilinde sağlanır. Yüksek kullanılabilirlik ayarlarını yapılandırmadan sorumlu olursunuz.

Aşağıdaki tabloda, Microsoft tarafından yönetilen ve varsayılan olarak yüksek oranda kullanılabilir olan Azure hizmetleri gösterilmektedir.

| Hizmet | Yöneten | Varsayılan olarak yüksek kullanılabilirlik |
| ----- | ----- | ----- |
| **Azure Machine Learning altyapısı** | Microsoft | |
| **İlişkili kaynaklar** |
| Azure Depolama | Siz | |
| Key Vault | Siz | ✓ |
| Container Registry | Siz | |
| Application Insights | Siz | NA |
| **İşlem kaynakları** |
| İşlem örneği | Microsoft |  |
| İşlem kümesi | Microsoft |  |
| AKS gibi diğer işlem kaynakları <br>Azure Databricks, Container Instances, HDInsight | Siz |  |
| Azure depolama, SQL veritabanı, gibi **ek veri depoları**<br> PostgreSQL için Azure veritabanı, MySQL için Azure veritabanı, <br>Azure Databricks dosya sistemi | Siz | |

Bu makalenin geri kalanında, bu hizmetlerin her birini yüksek oranda kullanılabilir hale getirmek için gerçekleştirmeniz gereken işlemler açıklanmaktadır.

## <a name="associated-resources"></a>İlişkili kaynaklar

> [!IMPORTANT]
> Azure Machine Learning, coğrafi olarak yedekli depolama (GRS), coğrafi bölge yedekli depolama (GZRS), Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) veya Okuma Erişimli Coğrafi bölge yedekli depolama (RA-GZRS) kullanarak varsayılan depolama hesabı yük devretmesini desteklemez.

Aşağıdaki belgelere başvurarak her bir kaynağın yüksek kullanılabilirlik ayarlarını yapılandırmayı unutmayın:

* **Azure depolama**: yüksek kullanılabilirliğe sahip ayarları yapılandırmak için bkz. [Azure depolama artıklığı](../storage/common/storage-redundancy.md).
* **Key Vault**: Key Vault, varsayılan olarak yüksek kullanılabilirlik sağlar ve Kullanıcı eylemi gerektirmez.  Bkz. [Azure Key Vault kullanılabilirliği ve artıklığı](../key-vault/general/disaster-recovery-guidance.md).
* **Container Registry**: coğrafi çoğaltma için Premium kayıt defteri seçeneğini belirleyin. Bkz. [Azure Container Registry coğrafi çoğaltma](../container-registry/container-registry-geo-replication.md).
* **Application Insights**: Application Insights yüksek kullanılabilirlik ayarları sağlamıyor. Veri saklama süresini ve ayrıntılarını ayarlamak için, bkz. [Application Insights veri toplama, bekletme ve depolama](../azure-monitor/app/data-retention-privacy.md#how-long-is-the-data-kept).

## <a name="compute-resources"></a>İşlem kaynakları

Aşağıdaki belgelere başvurarak her bir kaynağın yüksek kullanılabilirlik ayarlarını yapılandırmayı unutmayın:

* **Azure Kubernetes hizmeti**: [Azure Kubernetes Service (aks) ' de iş sürekliliği ve olağanüstü durum kurtarma için en iyi yöntemlere](../aks/operator-best-practices-multi-region.md) bakın ve [kullanılabilirlik alanlarını kullanan bir Azure Kubernetes hizmeti (aks) kümesi oluşturun](../aks/availability-zones.md). AKS kümesi Azure Machine Learning Studio, SDK veya CLı kullanılarak oluşturulduysa, bölgeler arası yüksek kullanılabilirlik desteklenmez.
* **Azure Databricks**: [Azure Databricks kümeler Için bkz. bölgesel olağanüstü durum kurtarma](/azure/databricks/scenarios/howto-regional-disaster-recovery).
* **Container Instances**: bir Orchestrator yük devretmenin sorumluluğundadır. [Azure Container Instances ve kapsayıcı düzenleyicilerine](../container-instances/container-instances-orchestrator-relationship.md)bakın.
* **HDInsight**: [Azure HDInsight tarafından desteklenen yüksek kullanılabilirlik hizmetlerine](../hdinsight/hdinsight-high-availability-components.md)bakın.

## <a name="additional-data-stores"></a>Ek veri depoları

Aşağıdaki belgelere başvurarak her bir kaynağın yüksek kullanılabilirlik ayarlarını yapılandırmayı unutmayın:

* **Azure Blob kapsayıcı/Azure dosyaları/Data Lake Storage 2.**: varsayılan depolama ile aynı.
* **Data Lake Storage 1.**: [Data Lake Storage 1. için yüksek kullanılabilirlik ve olağanüstü durum kurtarma Kılavuzu '](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)na bakın.
* **SQL veritabanı**: bkz. [Azure SQL veritabanı ve SQL yönetilen örneği için yüksek kullanılabilirlik](../azure-sql/database/high-availability-sla.md).
* **PostgreSQL Için Azure veritabanı**: bkz. [PostgreSQL için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları-tek sunucu](../postgresql/concepts-high-availability.md).
* **MySQL Için Azure veritabanı**: [MySQL için Azure veritabanı 'Nda iş sürekliliği anlama](../mysql/concepts-business-continuity.md)bölümüne bakın.
* **Azure Databricks dosya sistemi**: [Azure Databricks kümeler Için bkz. bölgesel olağanüstü durum kurtarma](/azure/databricks/scenarios/howto-regional-disaster-recovery).

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Bir Azure Machine Learning çalışma alanı dağıtmak için müşteri tarafından yönetilen bir anahtarınız sağlarsanız, aboneliğiniz içinde de sağlanmış Azure Cosmos DB. Bu durumda, yüksek kullanılabilirlik ayarlarını yapılandırmadan sorumlu olursunuz. [Azure Cosmos DB Ile yüksek kullanılabilirliğe](../cosmos-db/high-availability.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ayarlarınızla ilişkili kaynaklarla Azure Machine Learning dağıtmak için bir [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced)kullanın.