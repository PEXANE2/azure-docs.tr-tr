---
title: SSS-Azure SYNAPSE Analytics (çalışma alanları Önizleme)
description: Azure SYNAPSE Analytics hakkında SSS (çalışma alanları Önizleme)
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: ba6f79fffe5287be7574d422f026489d4da2795e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287501"
---
# <a name="azure-synapse-analytics-workspaces-preview-frequently-asked-questions"></a>Azure SYNAPSE Analytics (çalışma alanları önizlemesi) sık sorulan sorular

Bu kılavuzda, SYNAPSE Analytics için en sık sorulan soruları bulacaksınız.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="general"></a>Genel

### <a name="q-what-is-azure-synapse-analytics"></a>S: Azure SYNAPSE Analytics nedir?

Y: Azure SYNAPSE, BI, AI ve sürekli zeka için tümleşik bir veri platformudur. SQL ve Spark gibi çeşitli analiz çalışma zamanlarını, için birleştirilmiş bir yol sağlayan tek bir platformda bağlar:

- Ağ dahil olmak üzere analiz kaynaklarınızı güvenli hale getirme, havuza, verilere ve geliştirme yapılarına çoklu oturum açma erişimini yönetme.
- Dilediğiniz katmandaki çalışma alanı etkinliklerinizde oluşan olayları kolayca Izleyip hızlı bir şekilde iyileştirin, tepki verin ve hatalarını ayıklayın.
- Altyapılarınızın tamamında meta verilerinizi yönetin. Spark tablosu oluşturun ve Azure SYNAPSE veritabanlarında otomatik olarak kullanılabilir.
- Birleştirilmiş bir kullanıcı deneyimi aracılığıyla verilerle etkileşime geçin. SYNAPSE Studio, büyük veri geliştiricileri, veri mühendisleri, DBAs, veri analistleri ve veri bilimcilerini aynı platforma taşır.

Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics nedir?](https://docs.microsoft.com/azure/synapse-analytics/overview-what-is).

### <a name="q-what-are-the-main-components-of-azure-synapse-analytics"></a>S: Azure SYNAPSE Analytics 'in ana bileşenleri nelerdir?

Y: Azure SYNAPSE aşağıdaki yeteneklere sahiptir:

- Analiz özellikleri, SQL havuzu veya isteğe bağlı SQL (Önizleme) (sunucusuz) aracılığıyla sunulur.
- Scala, Python, Mini SQL ve C için tam desteğe sahip Apache Spark Havuzu (Önizleme) #
- Kod içermeyen büyük veri dönüştürme deneyimi sunan veri akışı
- Verilerinizi tümleştirmek ve tüm kod geliştirmenizi sağlamak için veri tümleştirme & düzenleme
- Bu olanaklarla tek bir Web Kullanıcı arabirimi aracılığıyla erişmek için Studio

### <a name="q-how-does-azure-synapse-analytics-relate-to-azure-sql-data-warehouse"></a>S: Azure SYNAPSE Analytics, Azure SQL veri ambarı ile nasıl ilişkilidir

Y: Azure SYNAPSE Analytics, Azure SQL veri ambarı 'nın veri ambarı çözümü olarak SQL havuzu içeren bir analiz platformuna gelişme hızıdır. Bu platform veri araştırma, alma, dönüştürme, hazırlama ve bir hizmet Analizi katmanını birleştirir.

## <a name="use-cases"></a>Uygulama alanları

### <a name="q-how-do-i-rename-a-published-artifact-dataset-notebook-sql-script-and-so-on-in-azure-synapse"></a>S: Azure SYNAPSE 'de yayımlanmış bir yapıtı (veri kümesi, Not defteri, SQL betiği vb.) yeniden adlandırma Nasıl yaparım??

Y: yayımlanmış bir yapıt dosyasını yeniden adlandırmak Için, önce dosyayı kopyalayın ve yeni dosyayı tercih ettiğiniz adla yeniden adlandırın. Yapıtın tüm başvurularını yeni dosya adına el ile güncelleştirmeniz ve eski olanı silmeniz gerekir.

### <a name="q-what-is-a-good-use-case-for-synapse-sql-pool"></a>S: SYNAPSE SQL havuzu için iyi bir kullanım durumu nedir?

Y: SQL havuzu, veri ambarı gereksinimlerinizin kalbidir. [Fiyat/performans](https://azure.microsoft.com/services/sql-data-warehouse/compare/)' daki önde gelen veri ambarı çözümüdür. SQL havuzu, sektörde önde gelen bulut veri ambarı çözümüdür:

- yüksek eşzamanlılık ve iş yükü yalıtımı sayesinde performansı etkilemeden büyük ve karışık çeşitli iş yükleri sunar
- Ağ güvenliği ile ince erişime kadar olan gelişmiş özellikler sayesinde verilerinizin güvenliğini kolayca sağlayın
- çok çeşitli ekonomik sistem avantajlarından yararlanın

### <a name="q-what-is-a-good-use-case-for-spark-in-synapse"></a>S: SYNAPSE içinde Spark için iyi bir kullanım durumu nedir?

Y: ilk hedefiniz, toplu iş veya akışta Gölü verileri dönüştürmek için harika bir veri Mühendisliği deneyimi sağlamaktır. Veri düzenleme konusunda sıkı ve basit tümleştirme, geliştirme çalışmalarınızın kullanım durumunu basit hale getirir.

Spark için başka bir kullanım durumu şu şekilde bir veri Bilimcı içindir:

- bir özelliği Ayıkla
- verileri keşfet
- Model eğitme

### <a name="q-what-is-a-good-use-case-for-sql-on-demand-in-synapse"></a>S: SYNAPSE 'de SQL isteğe bağlı SQL için iyi kullanım durumu nedir?

Y: isteğe bağlı SQL, Data Lake 'inizdeki veriler üzerinde bir sorgu hizmetidir. Verileri özel bir depoya kopyalamak veya yüklemeye gerek kalmadan, verileri yerinde sorgulamak üzere tanıdık bir T-SQL sözdizimi sağlayarak tüm verilerinize erişmenizi herkese olanak sağlar.

Kullanım örneği örnekleri şunları içerir:

- temel bulma ve araştırma-veri analistleri, gelişmekte olan veri bilimcileri ve veri mühendislerini, Veri Gölü, okuma T-SQL sorguları hakkında şema ile ilk bakış için kolay bir yol sağlar.
- mantıksal veri ambarı-veri analistleri, Azure depolama 'da bulunan verileri doğrudan sorgulamak ve analiz etmek ve tanıdık bı araçlarını (ör. Azure çözümlemeler Hizmetleri, Power BI Premium vb.) kullanarak, Starlight sorgu sorgularını yeniden çalıştırarak panoları yenilemek için tam ifade eden T-SQL dili çalıştırabilir
- "tek sorgu" ETL-veri mühendislerinin Azure Storage tabanlı verileri bir biçimden diğerine, filtrelemeye, toplamaya, vb. dönüştürmelerine olanak tanır, Azure depolama 'ya sorgu sonuçlarını kalıcı hale getirir ve daha sonra Starlight sorgusunda veya diğer ilgi hizmetlerinde daha fazla işleme için hemen kullanılabilir hale getirin

### <a name="q-what-is-a-good-use-case-for-data-flow-in-synapse"></a>S: SYNAPSE içinde veri akışı için iyi bir kullanım durumu nedir?

Y: veri akışı, veri mühendislerinin kod yazmadan grafik veri dönüştürme mantığı geliştirmesini sağlar. Elde edilen veri akışları, veri tümleştirme & düzenleme içinde etkinlik olarak yürütülür. Veri akışı etkinlikleri, mevcut zamanlama, denetim, akış ve izleme özellikleri aracılığıyla çalıştırılabilir.

## <a name="security-and-access"></a>Güvenlik ve erişim

Y: uçtan uca çoklu oturum açma deneyimi, SYNAPSE Analytics 'teki önemli bir kimlik doğrulama işlemidir. Tam bir AAD tümleştirmesiyle aracılığıyla kimlik yönetimi ve geçirme bir olmalıdır.

### <a name="q-how-do-i-get-access-to-files-and-folders-in-the-adls-gen2"></a>S: Nasıl yaparım? ADLS 2. dosyalara ve klasörlere erişim sağlayın

Y: dosya ve klasörlere erişim şu anda ADLS 2. aracılığıyla yönetilmektedir. Daha fazla bilgi için bkz. [Data Lake Storage Access Control](../storage/blobs/data-lake-storage-access-control.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="q-can-i-use-third-party-business-intelligence-tools-to-access-azure-synapse-analytics"></a>S: Azure SYNAPSE Analytics 'e erişmek için üçüncü taraf iş zekası araçlarını kullanabilir miyim?

Y: Evet, SQL Pool 'a ve isteğe bağlı SQL 'e bağlanmak için Tableau ve Power BI gibi üçüncü taraf iş uygulamalarınızı kullanabilirsiniz. Spark, IntelliJ 'yi destekler.

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanı oluşturma](quickstart-create-workspace.md)
- [SYNAPSE Studio 'Yu kullanma](quickstart-synapse-studio.md)
- [SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md)
- [İsteğe bağlı SQL kullanma](quickstart-sql-on-demand.md)
- [Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md) 
