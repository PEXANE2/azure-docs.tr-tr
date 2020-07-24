---
title: Dayanıklılığı artırma
titleSuffix: Azure Machine Learning
description: Yüksek oranda kullanılabilir bir yapılandırma kullanarak Azure Machine Learning ilişkili kaynaklarınızı kesintileri daha dayanıklı hale getirme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jhirono
author: jhirono
ms.reviewer: larryfr
ms.date: 07/16/2020
ms.openlocfilehash: 4f2bf239e1157f5c927c4449857ad5f7793ccb49
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100170"
---
# <a name="increase-the-resiliency-of-azure-machine-learning"></a>Azure Machine Learning esnekliğini artırın

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Yüksek kullanılabilirlik konfigürasyonları kullanarak Azure Machine Learning ilgili kaynaklarınızı daha dayanıklı hale getirme hakkında bilgi edinin. Azure Machine Learning bağlı olan Azure Hizmetleri, yüksek kullanılabilirlik için yapılandırılabilir. Bu makale, yüksek kullanılabilirlik için hangi hizmetlerin yapılandırılabileceğini ve bu kaynakları yapılandırmayla ilgili bilgilere bağlantılar sağlar.

> [!NOTE]
> Azure Machine Learning kendisi bir olağanüstü durum kurtarma seçeneği sunmaz.

## <a name="understand-azure-services-for-azure-machine-learning"></a>Azure Machine Learning için Azure hizmetlerini anlama

Azure Machine Learning birden çok Azure hizmetine bağlıdır ve birkaç katmana sahiptir. Bazıları (müşteri) aboneliğinizde sağlanırlar. Bu hizmetlerin yüksek kullanılabilirlik yapılandırmasından siz sorumlusunuz. Bazıları bir Microsoft aboneliğinde oluşturulur ve Microsoft tarafından yönetilir.

* **Azure Machine Learning altyapısı**: Azure Machine Learning çalışma alanı için Microsoft tarafından yönetilen ortam.

* **Ilişkili kaynaklar**: Azure Machine Learning çalışma alanı oluşturma sırasında aboneliğinizde sağlanan kaynaklar. Azure depolama, Azure Key Vault, Azure Container Registry (ACR) ve uygulama öngörülerini içerir. Bu kaynaklar için yüksek kullanılabilirlik ayarlarından siz sorumlusunuz.
  * Varsayılan depolamada model, eğitim günlüğü verileri ve veri kümesi gibi veriler bulunur.
  * Key Vault depolama, ACR, veri depoları için kimlik bilgilerine sahiptir.
  * ACR, eğitim ve ınktans ortamı için Docker görüntüsüne sahiptir.
  * Uygulama öngörüleri izleme Azure Machine Learning içindir.

* **Işlem kaynakları**: çalışma alanı dağıtımından sonra oluşturduğunuz kaynaklar. Örneğin, bir makine öğrenimi modelini eğitmek için bir işlem örneği veya işlem kümesi oluşturabilirsiniz.
  * İşlem örneği ve Işlem kümesi: Microsoft tarafından yönetilen model geliştirme ortamı.
  * Diğer kaynaklar: Azure Kubernetes hizmeti (AKS), Azure Databricks, Azure Container Instances (ACI) ve HDInsight gibi Azure Machine Learning bilgi işlem kaynakları iliştirilebilir. Yüksek kullanılabilirlik ayarından sorumlusunuz.

* **Ek veri depoları**: Azure Machine Learning Azure depolama, Azure Data Lake Storage ve eğitim verileri IÇIN Azure SQL veritabanı gibi ek veri depoları bağlayabilir.  Bunlar aboneliğiniz içindedir ve yüksek kullanılabilirlik ayarından sorumludur.

Aşağıdaki tabloda, Microsoft tarafından yönetilen ve varsayılan olarak yüksek oranda kullanılabilir olan hizmetler gösterilmektedir:

| Hizmet | Yöneten | Varsayılan olarak HA |
| ----- | ----- | ----- |
| **Azure Machine Learning altyapısı** | Microsoft | |
| **İlişkili kaynaklar** |
| Azure Storage | Siz | |
| Azure Key Vault | Siz | ✓ |
| Azure Container Registry | Siz | |
| Application Insights | Siz | NA |
| **İşlem kaynakları** |
| İşlem örneği | Microsoft |  |
| İşlem kümesi | Microsoft |  |
| Azure Kubernetes hizmeti gibi diğer kaynaklar <br>Azure Databricks, Azure Container Instance, Azure HDInsight | Siz |  |
| Azure depolama, Azure SQL veritabanı, gibi **ek veri depoları**<br> PostgreSQL için Azure veritabanı, MySQL için Azure veritabanı, <br>Azure Databricks dosya sistemi | Siz | |

Bu hizmetlerden her birini yüksek oranda kullanılabilir hale getirmek için gereken eylemleri öğrenmek için bu belgenin geri kalanında bulunan bilgileri kullanın.

## <a name="associated-resources"></a>İlişkili kaynaklar

> [!IMPORTANT]
> Azure Machine Learning, coğrafi olarak yedekli depolama (GRS) veya coğrafi bölge yedekli depolama (GZRS) veya Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) veya Okuma Erişimli Coğrafi olarak yedekli depolama (ra-GZRS) kullanarak varsayılan depolama hesabı yük devretmesini desteklemez.

Aşağıdaki bilgileri içeren her bir kaynağın yüksek kullanılabilirlik ayarını yaptığınızdan emin olun.

* **Azure depolama**: yüksek kullanılabilirlik ayarını yapılandırmak için bkz. [Azure depolama artıklığı](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* **Azure Key Vault**: varsayılan yüksek kullanılabilirlik hizmeti sağlar ve Kullanıcı eylemi gerekli değildir.  Bkz. [Azure Key Vault kullanılabilirliği ve artıklığı](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance).
* **Azure Container Registry**: coğrafi çoğaltma IÇIN Premium SKU seçin. Bkz. [Azure Container Registry coğrafi çoğaltma](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
* **Application Insights**: yüksek kullanılabilirlik ayarı sağlamıyor. [Application Insights veri toplama, bekletme ve depolama](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy#how-long-is-the-data-kept)ile veri saklama süresi ve ayrıntılarını ince ayar sağlayabilirsiniz.

## <a name="compute-resources"></a>İşlem kaynakları

Aşağıdaki belgelerle her bir kaynağın yüksek kullanılabilirlik ayarını yaptığınızdan emin olun.

* **Azure Kubernetes hizmeti**: [Azure Kubernetes Service (aks) ' de iş sürekliliği ve olağanüstü durum kurtarma için en iyi yöntemlere](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) bakın ve [kullanılabilirlik alanlarını kullanan bir Azure Kubernetes hizmeti (aks) kümesi oluşturun](https://docs.microsoft.com/azure/aks/availability-zones). AKS kümesi Azure Machine Learning tarafından oluşturulduysa (Studio, SDK veya ML CLı kullanılarak), çapraz bölge yüksek kullanılabilirlik desteklenmez.
* **Azure Databricks**: [Azure Databricks kümeler Için bkz. bölgesel olağanüstü durum kurtarma](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).
* **Azure Container Instance**: acı Orchestrator, yük devretmeden sorumludur. [Azure Container Instances ve kapsayıcı düzenleyicilerine](https://docs.microsoft.com/azure/container-instances/container-instances-orchestrator-relationship)bakın.
* **Azure HDInsight**: bkz. [Azure HDInsight tarafından desteklenen yüksek kullanılabilirlik Hizmetleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-components).

## <a name="additional-data-stores"></a>Ek veri depoları

Aşağıdaki belgelerle her bir kaynağın yüksek kullanılabilirlik ayarını yaptığınızdan emin olun.

* **Azure Blob kapsayıcı/Azure dosya paylaşma/Azure Data Lake Gen2**: varsayılan depolama ile aynı.
* **Azure Data Lake Gen1**:[Azure Data Lake Storage 1. veri için olağanüstü durum kurtarma Kılavuzu '](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-disaster-recovery-guidance)na bakın.
* **Azure SQL veritabanı**: bkz. [yüksek KULLANıLABILIRLIK ve Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability).
* **PostgreSQL Için Azure veritabanı**: bkz. [PostgreSQL için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları-tek sunucu](https://docs.microsoft.com/azure/postgresql/concepts-high-availability).
* **MySQL Için Azure veritabanı**: [MySQL için Azure veritabanı 'Nda iş sürekliliği anlama](https://docs.microsoft.com/azure/mysql/concepts-business-continuity)bölümüne bakın.
* **Databricks dosya sistemi**: bkz. [Azure Databricks kümeleri için bölgesel olağanüstü durum kurtarma](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery).

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning çalışma alanı dağıtmak için kendi anahtarınızı (müşteri tarafından yönetilen anahtar) sağlarsanız, Cosmos DB da aboneliğiniz dahilinde sağlanır. Bu durumda, yüksek kullanılabilirliğinden sorumlu olursunuz. [Azure Cosmos DB Ile yüksek kullanılabilirliğe](https://docs.microsoft.com/azure/cosmos-db/high-availability) bakın

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ayarlarınızla ilişkili kaynaklarla Azure Machine Learning dağıtmak için bir [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced)kullanın.