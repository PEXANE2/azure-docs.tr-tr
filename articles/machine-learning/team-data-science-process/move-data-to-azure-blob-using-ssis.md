---
title: Blob depolama verilerini SSIS konektörleriyle taşıma - Ekip Veri Bilimi Süreci
description: Azure için SQL Server Tümleştirme Hizmetleri Özellik Paketi'ni kullanarak Verileri Azure Blob Depolama'ya veya Azure'dan nasıl taşıyabileceğinizi öğrenin.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 77bfd9d5bcae7bedd673354e32464d5f59bdc9b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720881"
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>SSIS konektörlerini kullanarak verileri Azure Blob Depolama'ya veya Azure Blob Depolama'dan taşıma
[Azure için SQL Server Tümleştirme Hizmetleri Özellik Paketi,](https://msdn.microsoft.com/library/mt146770.aspx) Azure'a bağlanmak, Azure ve şirket içi veri kaynakları arasında veri aktarımı ve Azure'da depolanan verileri işlemek için bileşenler sağlar.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Müşteriler şirket içi verileri buluta taşıdıktan sonra, Azure teknolojileri paketinin tüm gücünden yararlanmak için verilerine herhangi bir Azure hizmetinden erişebilirler. Veriler daha sonra, örneğin Azure Machine Learning'de veya bir HDInsight kümesinde kullanılabilir.

Bu Azure kaynaklarını kullanmaya yönelik örnekler [SQL](sql-walkthrough.md) ve [HDInsight](hive-walkthrough.md) yollarındadır.

Karma veri tümleştirme senaryolarında yaygın iş gereksinimlerini karşılamak için SSIS'i kullanan kanonik senaryoların tartışılması [için](https://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) bkz.

> [!NOTE]
> Azure blob depolamasına tam bir giriş için [Azure Blob Temelleri'ne](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob Hizmeti'ne](https://msdn.microsoft.com/library/azure/dd179376.aspx)bakın.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede açıklanan görevleri gerçekleştirmek için bir Azure aboneliğiniz ve bir Azure Depolama hesabınız olması gerekir. Verileri yüklemek veya indirmek için Azure Depolama hesap adı ve hesap anahtarına ihtiyacınız var.

* **Azure aboneliği**ayarlamak için [ücretsiz bir aylık deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın.
* Depolama **hesabı** oluşturma ve hesap ve önemli bilgileri alma yla ilgili talimatlar için Azure [Depolama hesapları hakkında'ya](../../storage/common/storage-create-storage-account.md)bakın.

**SSIS konektörlerini**kullanmak için şunları indirmeniz gerekir:

* **SQL Server 2014 veya 2016 Standardı (veya üzeri)**: Install, SQL Server Entegrasyon Hizmetlerini içerir.
* **Azure için Microsoft SQL Server 2014 veya 2016 Entegrasyon Hizmetleri Özellik Paketi**: Bu bağlayıcılar [sırasıyla SQL Server 2014 Entegrasyon Hizmetleri](https://www.microsoft.com/download/details.aspx?id=47366) ve SQL Server [2016 Entegrasyon Hizmetleri](https://www.microsoft.com/download/details.aspx?id=49492) sayfalarından indirilebilir.

> [!NOTE]
> SSIS SQL Server ile yüklü, ancak Express sürümüne dahil değildir. SQL Server'ın çeşitli sürümlerinde hangi uygulamaların yer aldığı hakkında bilgi için [SQL Server Sürümleri'ne](https://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/) bakın
> 
> 

SSIS'teki eğitim materyalleri için Bkz. [SSIS için Hands On Training](https://www.microsoft.com/sql-server/training-certification)

Basit çıkarma, dönüştürme ve yükleme (ETL) paketleri oluşturmak için SISS'i kullanarak nasıl çalışır hale geleceğihakkında bilgi için [Bkz. SSIS Tutorial: Basit Bir ETL Paketi Oluşturma](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>NYC Taksi veri setini indirin
Burada açıklanan örnekte, herkese açık bir veri kümesi [(NYC Taksi Gezileri](https://www.andresmh.com/nyctaxitrips/) veri seti) kullanılır. Dataset 2013 yılında NYC yaklaşık 173 milyon taksi sürmek oluşur. İki tür veri vardır: gezi ayrıntıları verileri ve ücret verileri. Her ay için bir dosya olduğu gibi, her biri yaklaşık 2 GB sıkıştırılmamış 24 dosya var.

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolamasına veri yükleme
SSIS özellik paketini kullanarak verileri şirket içi azure blob depolamasına taşımak için, burada gösterilen [**Azure Blob Yükleme Görevi'nin**](https://msdn.microsoft.com/library/mt146776.aspx)bir örneğini kullanırız:

![yapılandırma-veri-bilim-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

Görevin kullandığı parametreler burada açıklanmıştır:

| Alan | Açıklama |
| --- | --- |
| **AzureStorageConnection** |Varolan bir Azure Depolama Bağlantı Yöneticisi belirtir veya blob dosyalarının barındırıldığı yeri gösteren bir Azure Depolama hesabına atıfta bulunan yeni bir hesap oluşturur. |
| **BlobContainer** |Yüklenen dosyaları blob olarak tutan blob kapsayıcısının adını belirtir. |
| **BlobDirectory** |Yüklenen dosyanın blok blob olarak depolandığı blob dizinini belirtir. Blob dizini sanal hiyerarşik bir yapıdır. Leke zaten varsa, ia değiştirildi. |
| **LocalDirectory** |Yüklenecek dosyaları içeren yerel dizinini belirtir. |
| **Dosyaadı** |Belirtilen ad deseni olan dosyaları seçmek için bir ad filtresi belirtir. Örneğin, MySheet\*.xls\* MySheet001.xls ve MySheetABC.xlsx gibi dosyaları içerir |
| **Zaman Aralığından/Zaman Aralığı** |Bir zaman aralığı filtresi belirtir. *Zaman Aralığı'ndan* sonra ve *TimeRangeTo'dan* önce değiştirilen dosyalar dahildir. |

> [!NOTE]
> **AzureStorageConnection** kimlik bilgilerinin doğru olması ve aktarım girişiminde bulunulmadan önce **BlobContainer'ın** bulunması gerekir.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Azure blob depolamasından veri indirme
Azure blob depolamadan SSIS ile şirket içi depolamaya veri indirmek için [Azure Blob İndirme Görevi'nin](https://msdn.microsoft.com/library/mt146779.aspx)bir örneğini kullanın.

## <a name="more-advanced-ssis-azure-scenarios"></a>Daha gelişmiş SSIS-Azure senaryoları
SSIS özellik paketi, paketleme görevleri tarafından daha karmaşık akışların birlikte işlenmesine olanak tanır. Örneğin, blob verileri doğrudan bir HDInsight kümesine beslenebilir, çıktısı bir blob'a ve daha sonra şirket içi depolamaya indirilebilir. SSIS, ek SSIS konektörlerini kullanarak bir HDInsight kümesinde Kovan ve Domuz işlerini çalıştırabilir:

* SSIS ile Azure HDInsight kümesinde Hive komut dosyası çalıştırmak için [Azure HDInsight Hive Task'ı](https://msdn.microsoft.com/library/mt146771.aspx)kullanın.
* SSIS içeren bir Azure HDInsight kümesinde Pig komut dosyası çalıştırmak için [Azure HDInsight Pig Task'ı](https://msdn.microsoft.com/library/mt146781.aspx)kullanın.

