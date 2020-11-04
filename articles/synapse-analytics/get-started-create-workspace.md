---
title: 'Hızlı başlangıç: Başlarken-SYNAPSE çalışma alanı oluşturma'
description: Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: 6e1eeba99e3ad98aa0fee2e6709bb817ff829ed9
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93304740"
---
# <a name="creating-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, adanmış bir SQL havuzu ve sunucusuz Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticinin tüm adımlarını tamamlayabilmeniz için, **sahip** rolü atadığınız bir kaynak grubuna erişiminizin olması gerekir. Bu kaynak grubunda SYNAPSE çalışma alanını oluşturun.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Azure portal bir Synapse çalışma alanı oluşturun

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE** için arama yapın.
1. Arama sonuçlarında **Hizmetler** altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Çalışma alanı oluşturmak için **Ekle** ' yi seçin.
1. **Temel bilgiler** bölümünde tercih ettiğiniz **abonelik** , **kaynak grubu** , **bölge** girin ve ardından bir çalışma alanı adı seçin. Bu öğreticide, **MyWorkspace** kullanacağız.
1. Bir çalışma alanı oluşturmak için bu hesapta bir ADLSGEN2 hesabı ve bir kapsayıcı gerekir. Yeni bir tane oluşturmak için en basit seçenektir. Mevcut bir işlemi yeniden kullanmak istiyorsanız, bazı ek yapılandırmalar gerçekleştirmeniz gerekir. 
    1. SYNAPSE çalışma alanı, Spark günlüklerini ve Spark tabloları için verileri depolamak için varsayılan konum olarak bu kapsayıcıyı kullanır.
1. 1. seçenek yeni bir ADLSGEN2 hesabı oluşturma 
    1. **Data Lake Storage Gen 2** ' yi seçmek için gidin. 
    1. **Yeni oluştur** ' a tıklayın ve **contosolake** olarak adlandırın.
    1. **Dosya sistemi** ' ne tıklayın ve **kullanıcıları** adlandırın. Bu, **Kullanıcılar** adlı bir kapsayıcı oluşturur
1. 2. seçenek mevcut bir ADLSGEN2 hesabını kullanma. Bu belgenin en altındaki **ADLSGEN2 Storage hesabı Için hazırlama** yönergelerine bakın.
1. Azure SYNAPSE çalışma alanınız, bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolamak için kapsayıcı olarak kullanacaktır. Çalışma alanı, verileri Apache Spark tablolarında depolar. Spark uygulama günlüklerini **/SYNAPSE/WorkspaceName** adlı bir klasörde depolar.
1. **Gözden geçir ve oluştur** > **Oluştur** 'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* [Azure Portal](https://portal.azure.com)SYNAPSE çalışma alanınızı açın. **Genel bakış** bölümünün üst kısmında, **SYNAPSE Studio 'yu Başlat** ' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.

## <a name="create-a-dedicated-sql-pool"></a>Adanmış bir SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **SQL havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**SQL havuzu adı**| **SQLDB1**|
    |**Performans düzeyi**|**DW100C**|
    |||

1. **Gözden geçir ve oluştur** > **Oluştur** 'u seçin. Adanmış SQL havuzunuz birkaç dakika içinde hazırlanacaktır. Adanmış SQL havuzunuz, **SQLDB1** olarak da bilinen ayrılmış bir SQL havuzu veritabanıyla ilişkilidir.

Adanmış bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları kullanır. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

## <a name="create-a-serverless-apache-spark-pool"></a>Sunucusuz Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **Apache Spark havuzlarını** Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**Apache Spark havuzu adı**|**Spark1**
    |**Düğüm boyutu**| **Küçük**|
    |**Düğüm sayısı**| En az 3 ve en fazla 3 olarak ayarlayın|

1. **Gözden geçir ve oluştur** > **Oluştur** 'u seçin. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

Azure SYNAPSE 'te Spark etkinliği gerçekleştirdiğinizde, kullanmak için bir Spark havuzu belirtirsiniz. Havuz, Azure SYNAPSE 'in kaç Spark kaynağı kullandığını söyler. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynakların otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.

> [!NOTE]
> Spark veritabanları Spark havuzlarından bağımsız olarak oluşturulur. Çalışma alanı her zaman **varsayılan** olarak adlandırılan bir Spark veritabanına sahiptir. Ek Spark veritabanları oluşturabilirsiniz.

## <a name="the-serverless-sql-pool"></a>Sunucusuz SQL havuzu

Her çalışma alanı, **yerleşik** olarak adlandırılan önceden oluşturulmuş bir havuz ile gelir. Bu havuz silinemiyor. Sunucusuz SQL havuzu, Azure SYNAPSE 'da sunucusuz bir SQL havuzu oluşturmak veya yönetmek zorunda kalmadan SQL ile çalışmanıza olanak sağlar.


Diğer havuz türlerinin aksine, sunucusuz SQL havuzu için faturalandırma, sorguyu yürütmek için kullanılan kaynak sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

* Sunucusuz SQL havuzu, diğer sunucusuz SQL havuzlarından bağımsız olarak bulunan kendi veritabanlarına sahiptir.
* Çalışma alanı her zaman **yerleşik** olarak adlandırılan bir SUNUCUSUZ SQL havuzu içerir.

## <a name="preparing-a-adlsgen2-storage-account"></a>ADLSGEN2 Storage hesabı hazırlanıyor

### <a name="perform-the-following-steps-before-you-create-your-workspace"></a>Çalışma alanınızı oluşturmadan önce aşağıdaki adımları gerçekleştirin

1. [Azure portalını](https://portal.azure.com) açın.
1. Mevcut depolama hesabınıza gidin
1. Sol bölmede **erişim denetimi (IAM)** seçeneğini belirleyin. 
1. Aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun:
    * Kendinizi **sahip** rolüne atayın.
    * Kendinizi **Depolama Blobu veri sahibi** rolüne atayın.
1. Sol bölmede **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun.
1. Kapsayıcıya bir ad verebilirsiniz. Bu belgede,  **Kullanıcı** adı ' nı kullanırız.
1. Varsayılan ayar olan **genel erişim düzeyini** kabul edin ve **Oluştur** ' u seçin.

### <a name="perform-the-following-steps-after-you-create-your-workspace"></a>Çalışma alanınızı oluşturduktan sonra aşağıdaki adımları gerçekleştirin

Çalışma alanınızdan depolama hesabına erişimi yapılandırın. Azure SYNAPSE çalışma alanınız için Yönetilen kimlikler zaten depolama hesabına erişebilmiş olabilir. Aşağıdakileri sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol bölmeden **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Aşağıdaki rolleri atayın veya zaten atandıklarından emin olun. Çalışma alanı kimliği ve çalışma alanı adı için aynı adı kullanıyoruz.
    * Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolü için çalışma alanı kimliği olarak **MyWorkspace** ' i atayın.
    * Çalışma alanı adı olarak **MyWorkspace** atayın.
1. **Kaydet** ’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Adanmış bir SQL havuzu kullanarak çözümle](get-started-analyze-sql-pool.md)
