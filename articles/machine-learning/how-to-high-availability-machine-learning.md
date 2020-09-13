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
ms.openlocfilehash: abb24ef1cc6a693acbaf6561a185fd5ae536a66d
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89668787"
---
# <a name="increase-azure-machine-learning-resiliency"></a>Azure Machine Learning dayanıklılığı artırın

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

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
| Azure Storage | Siz | |
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

* **Azure depolama**: yüksek kullanılabilirliğe sahip ayarları yapılandırmak için bkz. [Azure depolama artıklığı](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* **Key Vault**: Key Vault, varsayılan olarak yüksek kullanılabilirlik sağlar ve Kullanıcı eylemi gerektirmez.  Bkz. [Azure Key Vault kullanılabilirliği ve artıklığı](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance).
* **Container Registry**: coğrafi çoğaltma için Premium kayıt defteri seçeneğini belirleyin. Bkz. [Azure Container Registry coğrafi çoğaltma](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
* **Application Insights**: Application Insights yüksek kullanılabilirlik ayarları sağlamıyor. Veri saklama süresini ve ayrıntılarını ayarlamak için, bkz. [Application Insights veri toplama, bekletme ve depolama](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy#how-long-is-the-data-kept).

## <a name="compute-resources"></a>İşlem kaynakları

Aşağıdaki belgelere başvurarak her bir kaynağın yüksek kullanılabilirlik ayarlarını yapılandırmayı unutmayın:

* **Azure Kubernetes hizmeti**: [Azure Kubernetes Service (aks) ' de iş sürekliliği ve olağanüstü durum kurtarma için en iyi yöntemlere](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) bakın ve [kullanılabilirlik alanlarını kullanan bir Azure Kubernetes hizmeti (aks) kümesi oluşturun](https://docs.microsoft.com/azure/aks/availability-zones). AKS kümesi Azure Machine Learning Studio, SDK veya CLı kullanılarak oluşturulduysa, bölgeler arası yüksek kullanılabilirlik desteklenmez.
* **Azure Databricks**: [Azure Databricks kümeler Için bkz. bölgesel olağanüstü durum kurtarma](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).
* **Container Instances**: bir Orchestrator yük devretmenin sorumluluğundadır. [Azure Container Instances ve kapsayıcı düzenleyicilerine](https://docs.microsoft.com/azure/container-instances/container-instances-orchestrator-relationship)bakın.
* **HDInsight**: [Azure HDInsight tarafından desteklenen yüksek kullanılabilirlik hizmetlerine](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-components)bakın.

## <a name="additional-data-stores"></a>Ek veri depoları

Aşağıdaki belgelere başvurarak her bir kaynağın yüksek kullanılabilirlik ayarlarını yapılandırmayı unutmayın:

* **Azure Blob kapsayıcı/Azure dosyaları/Data Lake Storage 2.**: varsayılan depolama ile aynı.
* **Data Lake Storage 1.**: [Data Lake Storage 1. için yüksek kullanılabilirlik ve olağanüstü durum kurtarma Kılavuzu '](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-disaster-recovery-guidance)na bakın.
* **SQL veritabanı**: bkz. [Azure SQL veritabanı ve SQL yönetilen örneği için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability).
* **PostgreSQL Için Azure veritabanı**: bkz. [PostgreSQL için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları-tek sunucu](https://docs.microsoft.com/azure/postgresql/concepts-high-availability).
* **MySQL Için Azure veritabanı**: [MySQL için Azure veritabanı 'Nda iş sürekliliği anlama](https://docs.microsoft.com/azure/mysql/concepts-business-continuity)bölümüne bakın.
* **Azure Databricks dosya sistemi**: [Azure Databricks kümeler Için bkz. bölgesel olağanüstü durum kurtarma](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Bir Azure Machine Learning çalışma alanı dağıtmak için müşteri tarafından yönetilen bir anahtarınız sağlarsanız, aboneliğiniz içinde de sağlanmış Azure Cosmos DB. Bu durumda, yüksek kullanılabilirlik ayarlarını yapılandırmadan sorumlu olursunuz. [Azure Cosmos DB Ile yüksek kullanılabilirliğe](https://docs.microsoft.com/azure/cosmos-db/high-availability)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ayarlarınızla ilişkili kaynaklarla Azure Machine Learning dağıtmak için bir [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced)kullanın.
