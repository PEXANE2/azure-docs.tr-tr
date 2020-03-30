---
title: Verileri Azure Depolama lekelerinden Veri Gölü Depolama Gen1'e kopyalama
description: Azure Depolama Blobs'undaki verileri Azure Veri Gölü Depolama Gen1'e kopyalamak için AdlCopy aracını kullanın
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: ad408df140be49da2e50ef810285dd850e9da6a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638880"
---
# <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-storage-gen1"></a>Azure Depolama Blobs'tan Azure Veri Gölü Depolama Gen1'e veri kopyalama

> [!div class="op_single_selector"]
> * [DistCp kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Data Lake Storage Gen1, aşağıdaki kaynaklardan verileri kopyalamak için bir komut satırı aracı olan [AdlCopy'i](https://www.microsoft.com/download/details.aspx?id=50358)sağlar:

* Azure Depolama blobs veri gölü depolama gen1 içine. Veri Gölü Depolama Gen1'den Azure Depolama blobs verileri kopyalamak için AdlCopy kullanamazsınız.
* İki Veri Gölü Depolama Gen1 hesabı arasında.

Ayrıca, AdlCopy aracını iki farklı modda kullanabilirsiniz:

* **Tek başına,** aracın görevi gerçekleştirmek için Veri Gölü Depolama Gen1 kaynaklarını kullandığı yer.
* Kopyalama işlemini gerçekleştirmek için Veri **Gölü Analytics**hesabınıza atanan birimlerin kullanıldığı bir Veri Gölü Analizi hesabı nı kullanarak. Kopyalama görevlerini öngörülebilir bir şekilde gerçekleştirmek istediğinizde bu seçeneği kullanmak isteyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Azure Depolama, bazı verilerle birlikte **kapsayıcıyı şişiriyor.**
* **Bir Veri Gölü Depolama Gen1 hesabı**. Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* **Data Lake Analytics hesabı (isteğe bağlı)** - Data Lake Analytics hesabının nasıl oluşturulacağına ilişkin talimatlar için Azure Veri Gölü Analizi'ni görmeye [başlayın.](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* **AdlCopy aracı**. [AdlCopy aracını](https://www.microsoft.com/download/details.aspx?id=50358)yükleyin.

## <a name="syntax-of-the-adlcopy-tool"></a>AdlCopy aracının sözdizimi

AdlCopy aracıyla çalışmak için aşağıdaki sözdizimini kullanın

    AdlCopy /Source <Blob or Data Lake Storage Gen1 source> /Dest <Data Lake Storage Gen1 destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Units <Number of Analytics units> /Pattern

Sözdiziminde parametreler aşağıda açıklanmıştır:

| Seçenek | Açıklama |
| --- | --- |
| Kaynak |Azure depolama blob'undaki kaynak verilerin konumunu belirtir. Kaynak bir blob konteyner, bir blob veya başka bir Veri Gölü Depolama Gen1 hesabı olabilir. |
| Dest |Kopyalanması gereken Veri Gölü Depolama Gen1 hedefini belirtir. |
| Kaynak Anahtarı |Azure depolama blob kaynağının depolama erişim anahtarını belirtir. Bu, yalnızca kaynak bir blob kapsayıcı veya bir blob ise gereklidir. |
| Hesap |**İsteğe bağlı**. Kopyalama işini çalıştırmak için Azure Data Lake Analytics hesabını kullanmak istiyorsanız bunu kullanın. Sözdiziminde /Hesap seçeneğini kullanıyorsanız ancak Bir Veri Gölü Analizi hesabı belirtmezseniz, AdlCopy işi çalıştırmak için varsayılan bir hesap kullanır. Ayrıca, bu seçeneği kullanıyorsanız, Kaynağı (Azure Depolama Blob) ve hedef (Azure Veri Gölü Depolama Gen1) Veri Gölü Analizi hesabınız için veri kaynağı olarak eklemeniz gerekir. |
| Birimler |Kopyalama işi için kullanılacak Data Lake Analytics birimlerinin sayısını belirtir. Veri Gölü Analizi hesabını belirtmek için **/Hesap** seçeneğini kullanıyorsanız bu seçenek zorunludur. |
| Desen |Hangi lekelerin veya dosyaların kopyalanacağı bir regex deseni belirtir. AdlCopy büyük/küçük harf duyarlı eşleştirme kullanır. Desen belirtilmediğinde varsayılan desen tüm öğeleri kopyalamaktır. Birden çok dosya deseninin belirtilmesi desteklenmez. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Azure Depolama blob'undaki verileri kopyalamak için AdlCopy'yi (tek başına) kullanma

1. Bir komut istemi açın ve AdlCopy'nin yüklendiği dizine gidin, genellikle `%HOMEPATH%\Documents\adlcopy`.
1. Kaynak kapsayıcıdan belirli bir blob'u Veri Gölü Depolama Gen1 klasörüne kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Örnek:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[!NOTE]
    >Yukarıdaki sözdizimi, dosyanın Veri Gölü Depolama Gen1 hesabındaki bir klasöre kopyalanmasını belirtir. Belirtilen klasör adı yoksa AdlCopy aracı bir klasör oluşturur.

    Veri Gölü Depolama Gen1 hesabınız altında bulunan Azure aboneliğiiçin kimlik bilgilerini girmeniz istenir. Aşağıdakilere benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Ayrıca, bir kapsayıcıdan veri gölü depolama gen1 hesabına aşağıdaki komutu kullanarak tüm lekelerkopyalayabilirsiniz:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Örnek:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Bir Azure Blob Depolama hesabından kopyalıyorsanız, blob depolama tarafındaki kopya sırasında daraltılmış olabilirsiniz. Bu, kopyalama işinizin performansını düşürür. Azure Blob Depolama'nın sınırları hakkında daha fazla bilgi edinmek için [Azure abonelik ve hizmet sınırlarında](../azure-resource-manager/management/azure-subscription-service-limits.md)Azure Depolama sınırlarına bakın.

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-storage-gen1-account"></a>Başka bir Veri Gölü Depolama Gen1 hesabından veri kopyalamak için AdlCopy'yi (tek başına) kullanın

İki Veri Gölü Depolama Gen1 hesabı arasındaki verileri kopyalamak için AdlCopy'yi de kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy'nin yüklendiği dizine gidin, genellikle `%HOMEPATH%\Documents\adlcopy`.
1. Belirli bir dosyayı bir Veri Gölü Depolama Gen1 hesabından diğerine kopyalamak için aşağıdaki komutu çalıştırın.

        AdlCopy /Source adl://<source_adlsg1_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adlsg1_account>.azuredatalakestore.net/<path>/

    Örnek:

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > Yukarıdaki sözdizimi, dosyanın hedef Veri Gölü Depolama Gen1 hesabındaki bir klasöre kopyalanmasını belirtir. Belirtilen klasör adı yoksa AdlCopy aracı bir klasör oluşturur.
   >
   >

    Veri Gölü Depolama Gen1 hesabınız altında bulunan Azure aboneliğiiçin kimlik bilgilerini girmeniz istenir. Aşağıdakilere benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
1. Aşağıdaki komut, tüm dosyaları kaynak Veri Gölü Depolama Gen1 hesabındaki belirli bir klasörden hedef Veri Gölü Depolama Gen1 hesabındaki bir klasöre kopyalar.

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

AdlCopy tek başına bir araç olarak kullanırken, kopya paylaşılan, Azure tarafından yönetilen kaynaklarda çalıştırılır. Bu ortamda elde edebilirsiniz performans sistem yükü ve kullanılabilir kaynaklara bağlıdır. Bu mod en iyi özel olarak küçük aktarımlar için kullanılır. AdlCopy'yi tek başına bir araç olarak kullanırken parametrelerin ayarlanmasına gerek yoktur.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Verileri kopyalamak için AdlCopy'yi (Data Lake Analytics hesabıyla) kullanın

Azure depolama lekelerinden Veri Gölü Depolama Gen1'e veri kopyalamak için AdlCopy işini çalıştırmak için Data Lake Analytics hesabınızı da kullanabilirsiniz. Taşınacak veriler gigabayt ve terabayt aralığında olduğunda ve daha iyi ve öngörülebilir performans verisi istediğinizde genellikle bu seçeneği kullanırsınız.

Bir Azure Depolama Blob'undan kopyalamak için AdlCopy ile Veri Gölü Analizi hesabınızı kullanmak için, kaynağın (Azure Depolama Blob'u) Veri Gölü Analytics hesabınıza veri kaynağı olarak eklenmesi gerekir. Data Lake Analytics hesabınıza ek veri kaynakları ekleme yle ilgili talimatlar için [bkz.](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources)

> [!NOTE]
> Veri Gölü Analytics hesabını kullanarak kaynak olarak bir Azure Veri Gölü Depolama Gen1 hesabından kopyalıyorsanız, Veri Gölü Depolama Gen1 hesabını Data Lake Analytics hesabıyla ilişkilendirmeniz gerekmez. Kaynak mağazayı Veri Gölü Analizi hesabıyla ilişkilendirme gereksinimi yalnızca kaynak bir Azure Depolama hesabı olduğunda dır.
>
>

Veri Gölü Analytics hesabını kullanarak bir Azure Depolama blob'undan Veri Gölü Depolama Gen1 hesabına kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Units <number_of_data_lake_analytics_units_to_be_used>

Örnek:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Benzer şekilde, veri Veri Gölü Depolama Gen1 hesabındaki belirli bir klasördeki tüm dosyaları veri gölü analizi hesabını kullanarak hedef Veri Gölü Depolama Gen1 hesabındaki bir klasöre kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /Source adl://mysourcedatalakestorage.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastorage.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Terabayt aralığındaki verileri kopyalarken, AdlCopy'yi kendi Azure Veri Gölü Analytics hesabınızla kullanmak daha iyi ve daha öngörülebilir performans sağlar. Ayarlanması gereken parametre, kopyalama işi için kullanılacak Azure Veri Gölü Analiz Birimlerinin sayısıdır. Birim sayısını artırmak kopyalama işinizin performansını artırır. Kopyalanacak her dosya en fazla bir birim kullanabilir. Kopyalanan dosya sayısından daha fazla birim belirtmek performansı artırmaz.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Desen eşleştirmesini kullanarak verileri kopyalamak için AdlCopy'yi kullanın

Bu bölümde, verileri bir kaynaktan kopyalamak için AdlCopy'yi nasıl kullanacağınızı (aşağıdaki örneğimizde Azure Depolama Blob'u kullanıyoruz) desen eşleştirmesini kullanarak hedef Veri Gölü Depolama Gen1 hesabına nasıl kullanacağınızı öğrenirsiniz. Örneğin, kaynak blob'dan hedefe .csv uzantılı tüm dosyaları kopyalamak için aşağıdaki adımları kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy'nin yüklendiği dizine gidin, genellikle `%HOMEPATH%\Documents\adlcopy`.
1. Kaynak kapsayıcıdan veri gölü depolama gen1 klasörüne belirli bir blob *.csv uzantısı ile tüm dosyaları kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Örnek:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Faturalandırma

* AdlCopy aracını tek başına kullanırsanız, kaynak Azure Depolama hesabı Veri Gölü Depolama Gen1 hesabıyla aynı bölgede değilse, veri taşıma için çıkış maliyetleri için faturalandırılırsınız.
* Data Lake Analytics hesabınızla AdlCopy aracını kullanırsanız, standart [Data Lake Analytics faturalandırma oranları](https://azure.microsoft.com/pricing/details/data-lake-analytics/) geçerli olacaktır.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy kullanımı için dikkat edilmesi gerekenler

* AdlCopy (sürüm 1.0.5 için), topluca binlerce dosya ve klasöre sahip kaynaklardan veri kopyalamayı destekler. Ancak, büyük bir veri kümesini kopyalayan sorunlarla karşılaşırsanız, dosyaları/klasörleri farklı alt klasörlere dağıtabilir ve yolu kaynak olarak bu alt klasörlere kullanabilirsiniz.

## <a name="performance-considerations-for-using-adlcopy"></a>AdlCopy kullanmak için performans hususları

AdlCopy, binlerce dosya ve klasör içeren verilerin kopyalanmasına destek sağlar. Ancak, büyük bir veri kümesini kopyalayan sorunlarla karşılaşırsanız, dosyaları/klasörleri daha küçük alt klasörlere dağıtabilirsiniz. AdlCopy geçici kopyalar için oluşturulmuş. Verileri yinelenen olarak kopyalamaya çalışıyorsanız, kopyalama işlemleri etrafında tam yönetim sağlayan [Azure Veri Fabrikası'nı](../data-factory/connector-azure-data-lake-store.md) kullanmayı düşünmelisiniz.

## <a name="release-notes"></a>Sürüm notları

* 1.0.13 - Birden çok adlcopy komutu üzerinden verileri aynı Azure Veri Gölü Depolama Gen1 hesabına kopyalıyorsanız, artık her çalıştırma için kimlik bilgilerinizi yeniden girmeniz gerekmez. Adlcopy artık bu bilgileri birden çok çalıştırmada önbelleğe alacak.

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
