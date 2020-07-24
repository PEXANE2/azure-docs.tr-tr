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
ms.date: 07/20/2020
ms.openlocfilehash: b4d48dcc8f09ae8e2ec3bb198f8864de1c945682
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101862"
---
# <a name="create-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

Bu öğreticide, bir Synapse çalışma alanı, bir SQL havuzu ve bir Apache Spark havuzu oluşturmayı öğreneceksiniz. 

## <a name="prepare-a-storage-account"></a>Depolama hesabı hazırlama

1. [Azure portalını](https://portal.azure.com) açın.
1. Aşağıdaki ayarlara sahip yeni bir depolama hesabı oluşturun:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel bilgiler|**Depolama hesabı adı**| Herhangi bir ad seçin.| Bu belgede **contosolake**adını kullanacağız.|
    |Temel bilgiler|**Hesap türü**| **StorageV2** ||
    |Temel bilgiler|**Konum**|Herhangi bir konum seçin.| Azure SYNAPSE Analytics çalışma alanınızı ve Azure Data Lake Storage 2. hesabınızın aynı bölgede olmasını öneririz.|
    |Gelişmiş|**Data Lake Storage 2. Nesil**|**Etkin**| Azure SYNAPSE, yalnızca bu ayarın etkinleştirildiği depolama hesaplarıyla birlikte kullanılabilir.|
    |||||

1. Depolama hesabını oluşturduktan sonra sol bölmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun:
    * Kendinizi **sahip** rolüne atayın.
    * Kendinizi **Depolama Blobu veri sahibi** rolüne atayın.
1. Sol bölmede **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun.
1. Kapsayıcıya bir ad verebilirsiniz. Bu belgede kapsayıcı **kullanıcılarını**adı vereceğiz.
1. Varsayılan ayar olan **genel erişim düzeyini**kabul edin ve **Oluştur**' u seçin.

Aşağıdaki adımda, Azure SYNAPSE çalışma alanınızı, bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolayacak kapsayıcı olarak kullanacak şekilde yapılandıracaksınız. Çalışma alanı, verileri Apache Spark tablolarında depolar. Spark uygulama günlüklerini **/SYNAPSE/WorkspaceName**adlı bir klasörde depolar.

## <a name="create-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE**için arama yapın.
1. Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Bu ayarları kullanarak bir çalışma alanı oluşturmak için **Ekle** ' yi seçin:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel bilgiler|**Çalışma alanı adı**|Bunu herhangi bir şekilde adlandırın.| Bu belgede, **MyWorkspace**kullanacağız.|
    |Temel bilgiler|**Bölge**|Depolama hesabının bölgesiyle eşleştirin.|

1. **Data Lake Storage Gen 2**' yi seçin altında, daha önce oluşturduğunuz hesabı ve kapsayıcıyı seçin.
1. **Gözden geçir ve oluştur** > **Oluştur**'u seçin. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

## <a name="verify-access-to-the-storage-account"></a>Depolama hesabına erişimi doğrulama

Azure SYNAPSE çalışma alanınız için Yönetilen kimlikler zaten depolama hesabına erişebilmiş olabilir. Aşağıdakileri sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol bölmeden **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Aşağıdaki rolleri atayın veya zaten atandıklarından emin olun. Çalışma alanı kimliği ve çalışma alanı adı için aynı adı kullanıyoruz.
    * Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolü için çalışma alanı kimliği olarak **MyWorkspace** ' i atayın.
    * Çalışma alanı adı olarak **MyWorkspace** atayın.

1. **Kaydet**'i seçin.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL havuzu kullanarak çözümleme](get-started-analyze-sql-pool.md)
