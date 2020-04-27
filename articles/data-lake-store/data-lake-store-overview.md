---
title: Azure Data Lake Storage 1. nedir? | Microsoft Belgeleri
description: Data Lake Storage 1. (daha önce Azure Data Lake Store olarak bilinirdi) ve diğer veri depoları üzerinde sağladığı değeri genel bakış
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 99384374226fd89cfd672c6b4f851a1743db0764
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67118797"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. nedir?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage 1., büyük veri analizi iş yükleri için kurumsal çapta bir hiper ölçek deposudur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

Data Lake Storage 1., Webileuyumlu REST API 'Leri kullanılarak Hadoop 'tan (HDInsight kümesiyle kullanılabilir) erişilebilir. Depolanan verilerde analizler etkinleştirmek ve veri analizi senaryoları için performans için ayarlanmıştır. Data Lake Storage 1. tüm kurumsal sınıf özellikleri içerir: güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Temel işlevler

Data Lake Storage 1. temel özellikleri şunlardır.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir

Data Lake Storage 1., Hadoop Dağıtılmış Dosya Sistemi (bir) ile uyumlu ve Hadoop ekosistemi ile birlikte çalışarak bir Apache Hadoop dosya sistemidir. Webbir API kullanan mevcut HDInsight uygulamalarınız veya hizmetleriniz Data Lake Storage 1. kolayca tümleştirilebilir. Data Lake Storage 1. Ayrıca, uygulamalar için bir Web, uyumlu REST arabirimi sunar.

MapReduce veya Hive gibi Hadoop analitik çerçeveleri kullanarak Data Lake Storage 1. depolanan verileri kolayca çözümleyebilirsiniz. Azure HDInsight kümelerini temin edebilir ve bunları Data Lake Storage 1. ' de depolanan verilere doğrudan erişecek şekilde yapılandırabilirsiniz.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake Storage 1., sınırsız depolama sağlar ve analiz için çeşitli verileri saklayabilir. Hesap boyutları, dosya boyutları veya bir veri Gölü içinde depolanabilecek veri miktarı için herhangi bir sınır yoktur. Tek dosyalar kilobayt ile petabaytlarca arasında değişebilir. Veriler birden çok kopya yapılarak durarak saklanır. Verilerin Veri Gölü içinde saklanabileceği süre için bir sınır yoktur.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır

Data Lake Storage 1., büyük miktarlarda veriyi sorgulamak ve analiz etmek için büyük ölçekli verimlilik gerektiren büyük ölçekli analitik sistemleri çalıştırmak için oluşturulmuştur. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal kullanıma hazır: yüksek oranda kullanılabilir ve güvenli

Data Lake Storage 1. sektör standardı kullanılabilirliği ve güvenilirliği sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır.

Data Lake Storage 1. Ayrıca, depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için bkz. [Azure Data Lake Storage 1. verileri güvenli hale getirme](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tüm veriler

Data Lake Storage 1., tüm verileri yerel biçiminde, önceki dönüşümlere gerek kalmadan depolayabilirler. Data Lake Storage 1., veriler yüklenmeden önce bir şemanın tanımlanmasını gerektirmez, verileri yorumlamak ve analiz sırasında bir şema tanımlamak için onu ayrı analitik çerçeveye bırakın. Rastgele boyut ve biçimlerin dosyalarını depolayabilme özelliği, Data Lake Storage 1. yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemesini mümkün hale getirir.

Veriler için Data Lake Storage 1. kapsayıcılar temelde klasörler ve dosyalardır. SDK 'Ları, Azure portal ve Azure PowerShell 'i kullanarak depolanan veriler üzerinde işlem yapabilirsiniz. Bu arabirimleri kullanarak ve uygun kapsayıcıları kullanarak verilerinizi mağazaya yerleştirirseniz, herhangi bir veri türünü saklayabilirsiniz. Data Lake Storage 1., depoladığı verilerin türüne göre hiçbir özel veri işleme gerçekleştirmez.

## <a name="securing-data"></a><a name="DataLakeStoreSecurity"></a>Verilerin güvenliğini sağlama

Data Lake Storage 1., kimlik doğrulaması için Azure Active Directory (Azure AD) ve verilerinize erişimi yönetmek için erişim denetim listeleri (ACL 'Ler) kullanır.

| Özellik | Açıklama |
| --- | --- |
| Kimlik doğrulaması |Data Lake Storage 1., Data Lake Storage 1. depolanan tüm veriler için kimlik ve erişim yönetimi için Azure AD ile tümleşir. Tümleştirme nedeniyle, çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımı izleme, güvenlik izleme ve uyarı gibi tüm Azure AD özelliklerinden faydaların Data Lake Storage 1.. Data Lake Storage 1., REST arabirimi içinde kimlik doğrulaması için OAuth 2,0 protokolünü destekler. [Data Lake Storage 1. kimlik doğrulaması](data-lakes-store-authentication-using-azure-active-directory.md)' na bakın.|
| Erişim denetimi |Data Lake Storage 1., Webdapterprotocol tarafından sunulan POSIX stili izinleri destekleyerek erişim denetimi sağlar. ACL 'Leri kök klasörde, alt klasörlerde ve tek tek dosyalarda etkinleştirebilirsiniz. ACL 'Lerin Data Lake Storage 1. bağlamında nasıl çalıştığı hakkında daha fazla bilgi için, bkz. [Data Lake Storage 1. Access Control](data-lake-store-access-control.md). |
| Şifreleme |Data Lake Storage 1., hesapta depolanan veriler için de şifreleme sağlar. Data Lake Storage 1. bir hesap oluştururken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini veya şifrelemeyi kabul etmesinin tercih edilebilir. Daha fazla bilgi için bkz. [Data Lake Storage 1. şifreleme](data-lake-store-encryption.md). Şifrelemeyle ilgili yapılandırma sağlama hakkında yönergeler için, bkz. [Azure Portal kullanarak Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md). |

Data Lake Storage 1. verileri nasıl koruyacağınız hakkında yönergeler için bkz. [Azure Data Lake Storage 1. verileri güvenli hale getirme](data-lake-store-secure-data.md).

## <a name="application-compatibility"></a>Uygulama uyumluluğu

Data Lake Storage 1., Hadoop ekosistemindeki çoğu açık kaynaklı bileşenlerle uyumludur. Ayrıca, diğer Azure hizmetleriyle de iyi tümleşir. Açık kaynaklı bileşenler ve diğer Azure hizmetleriyle Data Lake Storage 1. nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

- Data Lake Storage 1. ile birlikte çalışabilen açık kaynaklı uygulamaların bir listesi için bkz. [Azure Data Lake Storage 1. ile uyumlu uygulamalar ve hizmetler](data-lake-store-compatible-oss-other-applications.md) .
- Daha geniş bir senaryoyu etkinleştirmek için diğer Azure hizmetleriyle Data Lake Storage 1. nasıl kullanacağınızı anlamak için bkz. [diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md) .
- Verileri alma, verileri işleme, verileri indirme ve verileri görselleştirme gibi senaryolarda Data Lake Storage 1. kullanmayı öğrenmek için [Data Lake Storage 1. kullanma senaryolarına](data-lake-store-data-scenarios.md) bakın.

## <a name="data-lake-storage-gen1-file-system"></a>Data Lake Storage 1. dosya sistemi

Data Lake Storage 1., Hadoop ortamlarında (HDInsight kümesiyle kullanılabilir) FileSystem AzureDataLakeFilesystem (adl://) aracılığıyla erişilebilir. Adl://kullanan uygulamalar ve hizmetler, şu anda Webbir 'da kullanılamayan daha fazla performans iyileştirmelerinden yararlanabilir. Sonuç olarak, Data Lake Storage 1., Webbir API 'yi doğrudan kullanmaya devam ederek adl://kullanma veya mevcut kodu sürdürme seçeneklerinin önerilen seçeneğiyle en iyi performansı kullanma esnekliği sağlar. Azure HDInsight, Data Lake Storage 1. en iyi performansı sağlamak için AzureDataLakeFilesystem 'i tamamen kullanır.

Kullanarak `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`Data Lake Storage 1. verilerinize erişebilirsiniz. Data Lake Storage 1. verilere erişme hakkında daha fazla bilgi için bkz. [depolanan verilerin özelliklerini görüntüleme](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md)
- [.NET SDK kullanarak Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-net-sdk.md)
- [Azure HDInsight 'ı Data Lake Storage 1. ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)