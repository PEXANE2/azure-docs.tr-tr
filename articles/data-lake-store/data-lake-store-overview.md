---
title: Azure Veri Gölü Depolama Gen1 nedir? | Microsoft Belgeleri
description: Veri Gölü Depolama Gen1'e (daha önce Azure Veri Gölü Deposu olarak bilinir) ve diğer veri depoları üzerinden sağladığı değere genel bakış
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 99384374226fd89cfd672c6b4f851a1743db0764
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67118797"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1 nedir?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Veri Gölü Depolama Gen1, büyük veri analitik iş yükleri için kurumsal çapında hiper ölçekli bir depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

Data Lake Storage Gen1'e WebHDFS uyumlu REST API'leri kullanılarak Hadoop'tan (HDInsight kümesi ile kullanılabilir) erişilebilir. Depolanan veriler üzerinde analiz sağlamak üzere tasarlanmıştır ve veri analizi senaryoları için performans için ayarlanmıştır. Veri Gölü Depolama Gen1 tüm kurumsal sınıf yetenekleri içerir: güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Temel işlevler

Veri Gölü Depolama Gen1'in bazı önemli yetenekleri şunlardır.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir

Data Lake Storage Gen1, Hadoop Dağıtılmış Dosya Sistemi (HDFS) ile uyumlu ve Hadoop ekosistemi ile çalışan bir Apache Hadoop dosya sistemidir. WebHDFS API'sini kullanan mevcut HDInsight uygulamalarınız veya hizmetleriniz Veri Gölü Depolama Gen1 ile kolayca tümleşebilir. Data Lake Storage Gen1, uygulamalar için WebHDFS uyumlu BIR REST arabirimini de ortaya çıkarır.

MapReduce veya Hive gibi Hadoop analitik çerçevelerini kullanarak Veri Gölü Depolama Gen1'de depolanan verileri kolayca analiz edebilirsiniz. Azure HDInsight kümelerini sağlayabilir ve bunları Veri Gölü Depolama Gen1'de depolanan verilere doğrudan erişmek üzere yapılandırabilirsiniz.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake Storage Gen1 sınırsız depolama sağlar ve analiz için çeşitli verileri saklayabilir. Hesap boyutlarına, dosya boyutlarına veya veri gölünde depolanabilecek veri miktarına herhangi bir sınır koymaz. Tek tek dosyalar kilobayt boyutu petabayt arasında olabilir. Veriler, birden çok kopya oluşturarak uzun bir şekilde depolanır. Verilerin veri gölünde depolanabileceği süre için bir sınır yoktur.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır

Veri Gölü Depolama Gen1, büyük miktarda veriyi sorgulamak ve analiz etmek için büyük iş gücü gerektiren büyük ölçekli analitik sistemleri çalıştırmak için oluşturulmuştür. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal hazır: Yüksek kullanılabilir ve güvenli

Data Lake Storage Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır.

Data Lake Storage Gen1, depolanan veriler için kurumsal dereceli güvenlik de sağlar. Daha fazla bilgi için bkz: [Azure Veri Gölü Depolama Gen1'de veri güvenliğini sağlama.](#DataLakeStoreSecurity)

### <a name="all-data"></a>Tüm veriler

Data Lake Storage Gen1, herhangi bir veriyi önceden dönüşüm gerektirmeden yerel biçiminde depolayabilir. Veri Gölü Depolama Gen1, veriler yüklenmeden önce bir şema tanımlanmasını gerektirmez, bu da verileri yorumlamak ve analiz sırasında bir şema tanımlamak için ayrı analitik çerçeveye bırakır. Rasgele boyutve biçimlerden oluşan dosyaları depolama yeteneği, Veri Gölü Depolama Gen1'in yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemesini mümkün kılar.

Veri için Veri Gölü Depolama Gen1 kapsayıcıları aslında klasörler ve dosyalardır. SDK'lar, Azure portalı ve Azure Powershell'i kullanarak depolanan verileri kullanırsınız. Verilerinizi bu arabirimleri ve uygun kapsayıcıları kullanarak depoya koyarsanız, her türlü veriyi depolayabilirsiniz. Veri Gölü Depolama Gen1, depolanan veri türüne göre herhangi bir özel veri işleme gerçekleştirmez.

## <a name="securing-data"></a><a name="DataLakeStoreSecurity"></a>Verilerin güvenliğini sağlama

Veri Gölü Depolama Gen1, kimlik doğrulaması için Azure Etkin Dizin (Azure AD) ve verilerinize erişimi yönetmek için denetim listelerine (AçL' lar) erişir.

| Özellik | Açıklama |
| --- | --- |
| Kimlik doğrulaması |Veri Gölü Depolama Gen1, Veri Gölü Depolama Gen1'de depolanan tüm veriler için kimlik ve erişim yönetimi için Azure AD ile tümleşir. Tümleştirme sayesinde, Veri Gölü Depolama Gen1, çok faktörlü kimlik doğrulama, Koşullu Erişim, rol tabanlı erişim denetimi, uygulama kullanım izleme, güvenlik izleme ve uyarı gibi tüm Azure AD özelliklerinden yararlanır. Data Lake Storage Gen1, REST arabirimi içinde kimlik doğrulama için OAuth 2.0 protokolünü destekler. Bkz. [Veri Gölü Depolama Gen1 kimlik doğrulaması.](data-lakes-store-authentication-using-azure-active-directory.md)|
| Erişim denetimi |Data Lake Storage Gen1, WebHDFS protokolü tarafından açığa çıkarılan POSIX tarzı izinleri destekleyerek erişim denetimi sağlar. Root klasöründe, alt klasörlerde ve tek tek dosyalarda Aç'ları etkinleştirebilirsiniz. ACD'lerin Veri Gölü Depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'de Erişim denetimine](data-lake-store-access-control.md)bakın. |
| Şifreleme |Veri Gölü Depolama Gen1, hesapta depolanan veriler için şifreleme de sağlar. Veri Gölü Depolama Gen1 hesabı oluştururken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini seçebilir veya şifreleme yi tercih edebilirsiniz. Daha fazla bilgi için Bkz. [Veri Gölü Depolama Gen1'de Şifreleme.](data-lake-store-encryption.md) Şifrelemeyle ilgili yapılandırmanın nasıl sağlayabileceğine ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md) |

Veri Gölü Depolama Gen1'de verilerin nasıl güvenli hale verilecesine ilişkin talimatlar [için](data-lake-store-secure-data.md)bkz.

## <a name="application-compatibility"></a>Uygulama uyumluluğu

Data Lake Storage Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşeniile uyumludur. Ayrıca diğer Azure hizmetleriyle de iyi bir şekilde bütünleşir. Açık kaynak bileşenleri ve diğer Azure hizmetleriyle Data Lake Storage Gen1'i nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

- Data Lake Storage Gen1 ile birlikte çalışabiliyor açık kaynak uygulamaların listesi için [Azure Veri Gölü Depolama Gen1 ile uyumlu uygulamalar](data-lake-store-compatible-oss-other-applications.md) ve hizmetlere bakın.
- Daha geniş bir senaryo aralığı sağlamak için Veri Gölü Depolama Gen1'in diğer Azure hizmetleriyle nasıl kullanılacağını anlamak için [diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md) bölümüne bakın.
- Veri Göl [Depolama Gen1'i kullanarak](data-lake-store-data-scenarios.md) veri alma, veri işleme, veri indirme ve verileri görselleştirme gibi senaryolarda Veri Gölü Depolama Gen1'in nasıl kullanılacağını öğrenmek için senaryolara bakın.

## <a name="data-lake-storage-gen1-file-system"></a>Veri Gölü Depolama Gen1 dosya sistemi

Data Lake Storage Gen1'e Hadoop ortamlarında (HDInsight kümesi ile kullanılabilir) AzureDataLakeFilesystem (adl://) dosya sistemi üzerinden erişilebilir. adl:// kullanan uygulamalar ve hizmetler, şu anda WebHDFS'de bulunmayan diğer performans optimizasyonlarından yararlanabilir. Sonuç olarak, Data Lake Storage Gen1, webhdfs API'sını doğrudan kullanmaya devam ederek önerilen adl:// kullanma seçeneğiyle en iyi performansı kullanma veya varolan kodu koruma esnekliği sağlar. Azure HDInsight, Veri Gölü Depolama Gen1'de en iyi performansı sağlamak için AzureDataLakeFilesystem'den tam olarak yararlanır.

Verilerinize Data Lake Storage Gen1'i kullanarak `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`erişebilirsiniz. Veri Gölü Depolama Gen1'deki verilere nasıl erişilir hakkında daha fazla bilgi [için, depolanan verilerin özelliklerini görüntüleyin.](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-portal.md)
- [.NET SDK kullanarak Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-net-sdk.md)
- [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)