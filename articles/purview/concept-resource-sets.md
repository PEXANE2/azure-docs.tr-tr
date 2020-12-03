---
title: Kaynak kümelerini anlama
description: Bu makalede, kaynak kümelerinin ne olduğu ve Azure purview 'ın bunları nasıl oluşturduğu açıklanmaktadır.
author: yaronyg
ms.author: yarong
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/19/2020
ms.openlocfilehash: 55efa9443fd59b66a7677c9c460e473715f201df
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96553983"
---
# <a name="understanding-resource-sets"></a>Kaynak kümelerini anlama

Bu makale, Azure purview 'ın veri varlıklarını mantıksal kaynaklarla eşlemek için kaynak kümelerini nasıl kullandığını anlamanıza yardımcı olur.

## <a name="background-info"></a>Arka plan bilgileri

Ölçekli veri işleme sistemleri genellikle birden çok dosya olarak bir diskte tek bir tabloyu depolar. Bu kavram, kaynak kümeleri kullanılarak Azure purview 'da temsil edilir. Kaynak kümesi, katalogdaki çok sayıda varlığı temsil eden katalogdaki tek bir nesnedir.

Örneğin, Spark kümenizin bir veri çerçevesini ADLS 2. veri kaynağına kalıcı olduğunu varsayalım. Spark 'da tablo tek bir mantıksal kaynak gibi görünse de, her biri toplam veri çerçevesinin içeriğinin bir bölümünü temsil eden binlerce Parquet dosyası vardır. IoT verileri ve Web günlüğü verilerinde aynı zorluk vardır. Günlük dosyalarını birkaç kez veren bir sensör olduğunu düşünün. Bu tek sensörden yüzlerce binlerce günlük dosyası olana kadar uzun sürmez.

Çok sayıda veri varlığını tek bir mantıksal kaynağa eşlemenin zorluğuyla ele almak için Azure purview kaynak kümelerini kullanır.

## <a name="how-azure-purview-detects-resource-sets"></a>Azure purview 'ın kaynak kümelerini nasıl algıladığı

Azure takip görünümü yalnızca Azure Bloblarında, ADLS 1. ve ADLS 2. kaynak kümelerinin algılanmasının kullanılmasını destekler.

Azure purview otomatik kaynak kümesi bulma adlı bir özellik kullanarak kaynak kümelerini otomatik olarak algılar. Bu özellik, tarama yoluyla alınan tüm verilere bakar ve bunları bir dizi tanımlı desenlerle karşılaştırır.

Örneğin, URL 'SI olan bir veri kaynağını taradığınızı varsayalım `https://myaccount.blob.core.windows.net/mycontainer/machinesets/23/foo.parquet` . Azure takip görünümü yol kesimlerine bakar ve yerleşik desenlerle eşleşip eşleşmediğine karar verir. GUID, sayı, tarih biçimleri, yerelleştirme kodları (örneğin, en-US) için yerleşik desenleri vardır. Bu durumda, sayı deseninin *23*' ü eşleştirir. Azure purview, bu dosyanın adlı bir kaynak kümesinin parçası olduğunu varsayar `https://myaccount.blob.core.windows.net/mycontainer/machinesets/{N}/foo.parquet` .

Ya da gibi bir URL için `https://myaccount.blob.core.windows.net/mycontainer/weblogs/en_au/23.json` Azure purview, adlı bir kaynak kümesi üreten yerelleştirme düzeniyle ve sayı düzeniyle eşleşir `https://myaccount.blob.core.windows.net/mycontainer/weblogs/{LOC}/{N}.json` .

Azure purview, bu stratejiyi kullanarak aşağıdaki kaynakları aynı kaynak kümesiyle eşleyebilir `https://myaccount.blob.core.windows.net/mycontainer/weblogs/{LOC}/{N}.json` :

- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/cy_gb/1004.json`
- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/cy_gb/234.json`
- `https://myaccount.blob.core.windows.net/mycontainer/weblogs/de_Ch/23434.json`

> [!Note]
> Azure Data Lake Storage 2. Nesil genel kullanıma sunuldu. Bugün kullanmaya başlamanızı öneririz. Daha fazla bilgi için bkz. [ürün sayfası](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## <a name="file-types-that-azure-purview-will-not-detect-as-resource-sets"></a>Azure purview 'ın kaynak kümesi olarak algılayamayacağı dosya türleri

Kasıtlı olarak, Word, Excel veya PDF gibi belge dosya türlerini kaynak kümeleri olarak sınıflandırmamaya çalışın. Bu, yaygın olarak bölümlenmiş bir dosya biçimi olduğundan, CSV 'lere karşılık gelen özel durumdur.

## <a name="how-azure-purview-scans-resource-sets"></a>Azure purview kaynak kümelerini tarar

Azure purview, bir kaynak kümesinin parçası olduğu kaynakları algıladığında, tam taramadan örnek taramaya geçiş yapar. Örnek bir taramada, yalnızca kaynak kümesinde olduğu bir dosyanın alt kümesini açar. Açık olduğu her dosya için şemasını kullanır ve sınıflandırıcılarından çalıştırılır. Daha sonra Azure purview, açılan kaynaklar arasından en yeni kaynağı bulur ve bu kaynağın şema ve sınıflandırmalarını katalogda tüm kaynak kümesi için girişte kullanır.

## <a name="what-azure-purview-stores-about-resource-sets"></a>Kaynak kümeleri hakkında Azure purview depoları

Tek şema ve sınıflandırmaların yanı sıra Azure purview, kaynak kümeleriyle ilgili olarak aşağıdaki bilgileri depolar:

- Derin taranan en son bölüm kaynağından alınan veriler.
- Kaynak kümesini oluşturan bölüm kaynaklarıyla ilgili bilgileri toplayın.
- Kaç bölüm kaynağı bulunduğunu gösteren bölüm sayısı.
- Derin taradığı örnek kümesinde kaç tane benzersiz şema bulunduğunu gösteren bir şema sayısı. Bu değer 1 – 5 veya 5 ' ten büyük değerler için bir sayıdır.
- Kaynak kümesine birden çok bölüm türü dahil edildiğinde bölüm türlerinin listesi. Örneğin, her ikisi de aynı kaynak kümesinin mantıksal bir parçası olsa da, bir IoT algılayıcısı hem XML hem de JSON dosyalarının çıktısını alabilir.

## <a name="built-in-resource-set-patterns"></a>Yerleşik kaynak kümesi desenleri

Azure takip görünümü aşağıdaki kaynak kümesi düzenlerini destekler. Bu desenler, bir dizinde veya dosya adının bir parçası olarak bir ad olarak görünebilir.

| Model adı | Görünen ad | Açıklama |
|--------------|--------------|-------------|
| GUID         | 'INI       | [RFC 4122](https://tools.ietf.org/html/rfc4122)' de tanımlandığı şekilde, genel olarak benzersiz bir tanımlayıcı. |
| Sayı       | No          | Bir veya daha fazla rakam. |
| Tarih/saat biçimleri | No     | Azure takip görünümü farklı türlerdeki tarih/saat biçimlerini destekler, ancak tümü bir dizi {N} ' ye düşürülür. |
| 4ByteHex     | EŞLENECEK        | Dört basamaklı onaltılık bir sayı. |
| Yerelleştirme | ÇERÇEVE        | [BCP 47](https://tools.ietf.org/html/bcp47)' de tanımlandığı şekilde bir dil etiketi. Azure purview, tire (-) veya alt çizgi (_) içeren etiketleri destekler. Örneğin, en_ca ve en-CA. |

## <a name="issues-with-resource-sets"></a>Kaynak kümeleriyle ilgili sorunlar

Kaynak kümeleri çoğu durumda iyi çalışır, ancak Azure purview ' de aşağıdaki sorunlarla karşılaşabilirsiniz:

- Bir varlığı kaynak kümesi olarak yanlış işaretler
- Bir varlığı yanlış kaynak kümesine koyar
- Bir varlığı, kaynak kümesi olmayan şekilde yanlış işaretler

## <a name="next-steps"></a>Sonraki adımlar

Veri Kataloğu 'Nu kullanmaya başlamak için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
