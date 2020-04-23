---
title: HDInsight'ta Azure Veri Gölü Depolama Gen1'e genel bakış
description: HDInsight'ta Veri Gölü Depolama Gen1'e Genel Bakış.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 15d7f0621ffbf883d267d389fb634a13aa927430
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873361"
---
# <a name="azure-data-lake-storage-gen1-overview-in-hdinsight"></a>HDInsight'ta Azure Veri Gölü Depolama Gen1'e genel bakış

Azure Veri Gölü Depolama Gen1, büyük veri analitik iş yükleri için kurumsal çapında bir hiper ölçekli depodur. Azure Veri Gölü'nü kullanarak, herhangi bir boyuttan, türde ve yutma hızından verileri yakalayabilirsiniz. Ve operasyonel ve araştırmacı analitik için tek bir yerde.

WebHDFS uyumlu REST API'lerini kullanarak Hadoop'tan (HDInsight kümesiyle kullanılabilir) Veri Gölü Depolama Gen1'e erişin. Data Lake Storage Gen1, depolanan veriler üzerinde analiz sağlamak üzere tasarlanmıştır ve veri analizi senaryolarında performans için ayarlanmıştır. Gen1, gerçek dünyadaki kurumsal kullanım durumları için gerekli olan yetenekleri içerir. Bu özellikler arasında güvenlik, yönetilebilirlik, uyarlanabilirlik, güvenilirlik ve kullanılabilirlik yer almaktadır.

Azure Veri Gölü Depolama Gen1 hakkında daha fazla bilgi için, [Azure Veri Gölü Depolama Gen1'e ayrıntılı Genel Bakış'a](../data-lake-store/data-lake-store-overview.md)bakın.

Veri Gölü Depolama Gen1'in temel özellikleri şunlardır.

## <a name="compatibility-with-hadoop"></a>Hadoop ile uyumluluk

Data Lake Storage Gen1, HDFS ve Hadoop ortamı ile uyumlu bir Apache Hadoop dosya sistemidir.  WebHDFS API'sini kullanan HDInsight uygulamaları veya hizmetleri, Veri Gölü Depolama Gen1 ile kolayca tümleşebilir. Data Lake Storage Gen1, uygulamalar için WebHDFS uyumlu BIR REST arabirimini de ortaya çıkarır.

Veri Gölü Depolama Gen1'de depolanan veriler Hadoop analitik çerçeveleri kullanılarak kolayca analiz edilebilir. MapReduce veya Hive gibi çerçeveler. Azure HDInsight kümeleri, Veri Gölü Depolama Gen1'de depolanan verilere doğrudan erişmek için kullanılabilir ve yapılandırılabilir.

## <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake Storage Gen1 sınırsız depolama sağlar ve analitik için farklı veri türlerini depolamak için uygundur. Hesap boyutlarına veya dosya boyutlarına sınırlama getirmez. Veya bir veri gölünde depolanabilecek veri miktarı. Tek tek dosyaların boyutu kilobaytlardan petabaytlara kadar değişir ve bu da Veri Gölü Depolama Gen1'i her türlü veriyi depolamak için mükemmel bir seçim haline getirir. Veriler, birden çok kopya oluşturarak uzun bir şekilde depolanır. Ayrıca, verilerin veri gölünde ne kadar süreyle depolanabileceği konusunda da bir sınır yoktur.

## <a name="performance-tuning-for-big-data-analytics"></a>Büyük veri analitiği için performans alamı

Data Lake Storage Gen1 analitik sistemler için tasarlanmıştır. Büyük miktarda veriyi sorgulamak ve çözümlemek için büyük iş kaynağı gerektiren sistemler. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusuna yayır. Verileri çözümlürken, bu kurulum dosya paralel olarak okunduğunda okuma iş buzunu iyileştirir.

## <a name="readiness-for-enterprise-highly-available-and-secure"></a>Kurumsal kullanıma hazır olma: Yüksek kullanılabilir ve güvenli

Data Lake Storage Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıkları dayanıklı olarak depolanır: gereksiz kopyalar beklenmeyen hatalara karşı korur. İşletmeler, çözümlerinde Veri Gölü Depolama Gen1'i mevcut veri platformlarının önemli bir parçası olarak kullanabilirler.

Data Lake Storage Gen1, depolanan veriler için kurumsal dereceli güvenlik de sağlar. Daha fazla bilgi için bkz: [Azure Veri Gölü Depolama Gen1'de veri güvenliğini sağlama.](#data-security-in-data-lake-storage-gen1)

## <a name="flexible-data-structures"></a>Esnek veri yapıları

Veri Gölü Depolama Gen1, önceki dönüşümlere gerek kalmadan, verileri olduğu gibi yerel biçiminde depolayabilir. Veri Gölü Depolama Gen1, veriler yüklenmeden önce bir şema tanımlanmasını gerektirmez. Bireysel analitik çerçeve verileri yorumlar ve analiz sırasında bir şema tanımlar. Veri Gölü Depolama Gen1 yapılandırılmış verileri işleyebilir. Ve yarı yapılandırılmış ve yapılandırılmamış veriler.

Veri için Veri Gölü Depolama Gen1 kapsayıcıları aslında klasörler ve dosyalardır. SDK'ları, Azure portalını ve Azure PowerShell'i kullanarak depolanan verileri kullanırsınız. Bu arabirimler ve kapsayıcılar ile depoya konulan veriler, herhangi bir veri türünü depolayabilir. Veri Gölü Depolama Gen1 veri türüne göre veri herhangi bir özel işleme yapmaz.

## <a name="data-security-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'de veri güvenliği

Veri Gölü Depolama Gen1, kimlik doğrulaması için Azure Etkin Dizini'ni kullanır ve verilerinize erişimi yönetmek için erişim denetim listelerini (AçS) kullanır.

| **Özellik** | **Açıklama** |
| --- | --- |
| Kimlik Doğrulaması |Veri Gölü Depolama Gen1, Veri Gölü Depolama Gen1'de depolanan tüm veriler için kimlik ve erişim yönetimi için Azure Active Directory (Azure AD) ile tümleşir. Tümleştirme sayesinde, Veri Gölü Depolama Gen1 tüm Azure REKLAM özelliklerinden yararlanır. Bu özellikler şunlardır: çok faktörlü kimlik doğrulama, Koşullu Erişim ve rol tabanlı erişim denetimi. Ayrıca, uygulama kullanım izleme, güvenlik izleme ve uyarı, ve benzeri. Data Lake Storage Gen1, REST arabirimi içinde kimlik doğrulama için OAuth 2.0 protokolünü destekler. [Azure Etkin Dizini kullanarak Azure Veri Gölü Depolama Gen1'deki Kimlik Doğrulama'ya](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md) bakın|
| Erişim denetimi |Data Lake Storage Gen1, WebHDFS protokolü tarafından ortaya çıkarılan POSIX tarzı izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. ACD'lerin Veri Gölü Depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'deki Erişim denetimine](../data-lake-store/data-lake-store-access-control.md)bakın. |
| Şifreleme |Veri Gölü Depolama Gen1 de hesapta depolanan veriler için şifreleme sağlar. Veri Gölü Depolama Gen1 hesabı oluştururken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini seçebilir veya şifreleme yi tercih edebilirsiniz. Daha fazla bilgi için Bkz. [Veri Gölü Depolama Gen1'de Şifreleme.](../data-lake-store/data-lake-store-encryption.md) Şifrelemeyle ilgili yapılandırmanın nasıl sağlayabileceğine ilişkin talimatlar için [bkz.](../data-lake-store/data-lake-store-get-started-portal.md) |

Veri Gölü Depolama Gen1'de veri güvenliğini sağlama hakkında daha fazla bilgi edinmek için [bkz.](../data-lake-store/data-lake-store-secure-data.md)

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1 ile uyumlu uygulamalar

Data Lake Storage Gen1, Hadoop ortamındaki çoğu açık kaynak bileşeniyle uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir.  Data Lake Storage Gen1'in hem açık kaynak bileşenleri hem de diğer Azure hizmetleriyle nasıl kullanAbileceği hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Bkz. [Azure Veri Gölü Depolama Gen1 ile çalışan açık kaynak büyük veri uygulamaları.](../data-lake-store/data-lake-store-compatible-oss-other-applications.md)
* Daha geniş bir senaryo aralığı sağlamak için Veri Gölü Deposu Gen1'in diğer Azure hizmetleriyle nasıl kullanılacağını anlamak için [Azure Veri Gölü Deposu Gen1'i diğer](../data-lake-store/data-lake-store-integrate-with-other-services.md) Azure hizmetleriyle tümleştirme bölümüne bakın.
* Bkz. [Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma.](../data-lake-store/data-lake-store-data-scenarios.md)

## <a name="data-lake-storage-gen1-file-system-adl"></a>Veri Gölü Depolama Gen1 dosya sistemi (adl://)

Hadoop ortamlarında, Yeni dosya sistemi olan AzureDataLakeFilesystem (adl://) aracılığıyla Veri Gölü Depolama Gen1'e erişebilirsiniz. Kullanılan `adl://` uygulamaların ve hizmetlerin performansı, webhdfs'de şu anda bulunmayan şekillerde optimize edilebilir. Sonuç olarak, önerilen adl:// kullanarak en iyi performansı elde etme esnekliğielde elabilirsiniz. Veya WebHDFS API'sini doğrudan kullanmaya devam ederek varolan kodu koruyun. Azure HDInsight, Veri Gölü Depolama Gen1'de en iyi performansı sağlamak için AzureDataLakeFilesystem'den tam olarak yararlanır.

Aşağıdaki URI'yi kullanarak Veri Gölü Depolama Gen1'deki verilerinize erişin:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Veri Gölü Depolama Gen1'deki verilere nasıl erişilisi hakkında daha fazla bilgi [için, depolanan verilerde kullanılabilen Eylemler'e](../data-lake-store/data-lake-store-get-started-portal.md#properties)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
* [Azure Veri Gölü Depolama Gen2 genel bakış](./overview-data-lake-storage-gen2.md)