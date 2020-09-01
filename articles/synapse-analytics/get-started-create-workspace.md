---
title: 'Öğretici: kullanmaya başlama SYNAPSE çalışma alanı oluşturma'
description: Bu öğreticide, bir Synapse çalışma alanı, bir SQL havuzu ve bir Apache Spark havuzu oluşturmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 08/27/2020
ms.openlocfilehash: 56292d3e8ba4c9ec89d73f10640264c178f8a9a7
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89255027"
---
# <a name="create-a-synapse-workspace"></a>Synapse çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, bir SQL havuzu ve bir Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="create-a-synapse-workspace"></a>Synapse çalışma alanı oluşturma

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE**için arama yapın.
1. Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Bu ayarları kullanarak bir çalışma alanı oluşturmak için **Ekle** ' yi seçin:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel Bilgiler|**Çalışma alanı adı**|Bunu herhangi bir şekilde adlandırın.| Bu belgede, **MyWorkspace**kullanacağız.|
    |Temel Bilgiler|**Bölge**|Depolama hesabının bölgesiyle eşleştirin.|

1. Bir çalışma alanı oluşturmak için bir ADLSGEN2 hesabınızın olması gerekir. Yeni bir tane oluşturmak için en basit seçenektir. Mevcut bir işlemi yeniden kullanmak istiyorsanız, bazı ek yapılandırmalar gerçekleştirmeniz gerekir. 
1. 1. seçenek yeni bir ADLSGEN2 hesabı oluşturma 
    1. **Data Lake Storage Gen 2**' yi seçin altında **Yeni oluştur** ' a tıklayın ve **contosolake**olarak adlandırın.
    1. **Data Lake Storage Gen 2**' yi seçin altında **dosya sistemi** ' ne tıklayın ve ardından **kullanıcıları**adlandırın.
1. 2. seçenek bu belgenin en altındaki **depolama hesabı hazırlama** yönergelerine bakın.
1. Azure SYNAPSE çalışma alanınız, bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolamak için kapsayıcı olarak kullanacaktır. Çalışma alanı, verileri Apache Spark tablolarında depolar. Spark uygulama günlüklerini **/SYNAPSE/WorkspaceName**adlı bir klasörde depolar.
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.


## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* [Azure Portal](https://portal.azure.com)SYNAPSE çalışma alanınızı açın. **Genel bakış** bölümünün üst kısmında, **SYNAPSE Studio 'yu Başlat**' ı seçin.
* Adresine gidin `https://web.azuresynapse.net` ve çalışma alanınızda oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **SQL havuzlarını**Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**SQL havuzu adı**| **SQLDB1**|
    |**Performans düzeyi**|**DW100C**|
    |||

1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. SQL havuzunuz birkaç dakika içinde hazırlanacaktır. SQL havuzunuz, **SQLDB1**olarak da BILINEN bir SQL havuzu veritabanıyla ilişkilendirilir.

Bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları tüketir. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

## <a name="create-an-apache-spark-pool"></a>Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **Apache Spark havuzlarını**Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**Apache Spark havuzu adı**|**Spark1**
    |**Düğüm boyutu**| **Küçük**|
    |**Düğüm sayısı**| En az 3 ve en fazla 3 olarak ayarlayın|

1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

> [!NOTE]
> Ada karşın, bir Apache Spark havuzu SQL havuzu gibi değildir. Azure SYNAPSE çalışma alanına Spark ile nasıl etkileşim kuracağınızı söylemek için kullandığınız bazı temel meta veriler.

Meta veriler olduklarından Spark havuzları başlatılamaz veya durdurulamaz.

Azure SYNAPSE 'te Spark etkinliği gerçekleştirdiğinizde, kullanmak için bir Spark havuzu belirtirsiniz. Havuz, Azure SYNAPSE 'in kaç Spark kaynağı kullandığını söyler. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynakların otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.

> [!NOTE]
> Spark veritabanları Spark havuzlarından bağımsız olarak oluşturulur. Çalışma alanı her zaman **varsayılan**olarak adlandırılan bir Spark veritabanına sahiptir. Ek Spark veritabanları oluşturabilirsiniz.

## <a name="the-sql-on-demand-pool"></a>SQL isteğe bağlı havuzu

Her çalışma alanı, **SQL isteğe**bağlı olarak adlandırılan önceden oluşturulmuş bir havuz ile gelir. Bu havuz silinemiyor. SQL isteğe bağlı havuzu, Azure SYNAPSE 'de bir SQL havuzu oluşturmak veya yönetmek zorunda kalmadan SQL ile çalışmanıza olanak sağlar.

Diğer havuz türlerinden farklı olarak, isteğe bağlı SQL için faturalandırma, sorguyu yürütmek için kullanılan kaynak sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

* İsteğe bağlı SQL, tüm SQL isteğe bağlı havuzlarından bağımsız olarak bulunan kendi SQL isteğe bağlı veritabanlarına sahiptir.
* Bir çalışma alanının her zaman **isteğe bağlı SQL**ADLı bir SQL isteğe bağlı havuzu vardır.

## <a name="prepare-a-storage-account"></a>Depolama hesabı hazırlama

1. [Azure portalını](https://portal.azure.com) açın.
1. Aşağıdaki ayarlara sahip yeni bir depolama hesabı oluşturun:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel Bilgiler|**Depolama hesabı adı**| Herhangi bir ad seçin.| Bu belgede **contosolake**adını kullanacağız.|
    |Temel Bilgiler|**Hesap türü**| **StorageV2** ||
    |Temel Bilgiler|**Konum**|Herhangi bir konum seçin.| Azure SYNAPSE Analytics çalışma alanınızı ve Azure Data Lake Storage 2. hesabınızın aynı bölgede olmasını öneririz.|
    |Gelişmiş|**Data Lake Storage 2. Nesil**|**Etkin**| Azure SYNAPSE, yalnızca bu ayarın etkinleştirildiği depolama hesaplarıyla birlikte kullanılabilir.|
    |||||

1. Depolama hesabını oluşturduktan sonra sol bölmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun:
    * Kendinizi **sahip** rolüne atayın.
    * Kendinizi **Depolama Blobu veri sahibi** rolüne atayın.
1. Sol bölmede **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun.
1. Kapsayıcıya bir ad verebilirsiniz. Bu belgede kapsayıcı **kullanıcılarını**adı vereceğiz.
1. Varsayılan ayar olan **genel erişim düzeyini**kabul edin ve **Oluştur**' u seçin.

### <a name="configure-access-to-the-storage-account-from-your-workspace"></a>Çalışma alanınızdan depolama hesabına erişimi yapılandırma

Azure SYNAPSE çalışma alanınız için Yönetilen kimlikler zaten depolama hesabına erişebilmiş olabilir. Aşağıdakileri sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol bölmeden **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Aşağıdaki rolleri atayın veya zaten atandıklarından emin olun. Çalışma alanı kimliği ve çalışma alanı adı için aynı adı kullanıyoruz.
    * Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolü için çalışma alanı kimliği olarak **MyWorkspace** ' i atayın.
    * Çalışma alanı adı olarak **MyWorkspace** atayın.

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL havuzu kullanarak çözümleme](get-started-analyze-sql-pool.md)
