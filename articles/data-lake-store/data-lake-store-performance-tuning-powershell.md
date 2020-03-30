---
title: Azure Veri Gölü Depolama Gen1 performans ayarı - PowerShell
description: Azure Veri Gölü Depolama Gen1 ile Azure PowerShell'i kullanırken performansı nasıl artıreceğiniz hakkında ipuçları.
author: stewu
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: stewu
ms.openlocfilehash: c975af1799d427651b76bb9fde5ff765afed3f86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73904571"
---
# <a name="performance-tuning-guidance-for-using-powershell-with-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1 ile PowerShell'i kullanmak için performans alamı kılavuzu

Bu makalede, Data Lake Storage Gen1 ile çalışmak için PowerShell kullanırken daha iyi performans elde etmek için ayarlayabileceğiniz özellikler açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="performance-related-properties"></a>Performansla ilgili özellikler

| Özellik            | Varsayılan | Açıklama |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Bu parametre, her bir dosya karşıya yüklenirken veya indirilirken kaç paralel iş parçacığı kullanılacağını seçmenize olanak tanır. Bu sayı, dosya başına ayrılabilen en büyük iş parçacıklarını temsil eder, ancak senaryonuza bağlı olarak daha az iş parçacığı elde edebilirsiniz (örneğin, 1-KB dosyası yüklüyorsanız, 20 iş parçacığı için sorsanız bile bir iş parçacığı elde elabilirsiniz).  |
| ConcurrentFileCount | 10      | Bu parametre özellikle klasörlerin karşıya yüklenmesi ve indirilmesi içindir. Bu parametre, karşıya yüklenebilecek veya indirilebilecek eş zamanlı dosya sayısını belirler. Bu sayı, aynı anda yüklenebilen veya indirilebilen en fazla eşzamanlı dosya sayısını temsil eder, ancak senaryonuza bağlı olarak daha az eşzamanlılık elde edebilirsiniz (örneğin, iki dosya yüklüyorsanız, sorsanız bile iki eşzamanlı dosya yüklemesi alırsınız için 15). |

**Örnek:**

Bu komut, dosya başına 20 iş parçacığı ve 100 eşzamanlı dosya kullanarak Verileri Göl Depolama Gen1'den kullanıcının yerel sürücüsüne dosya indirir.

```PowerShell
Export-AzDataLakeStoreItem -AccountName "Data Lake Storage Gen1 account name" `
    -PerFileThreadCount 20 `
    -ConcurrentFileCount 100 `
    -Path /Powershell/100GB `
    -Destination C:\Performance\ `
    -Force `
    -Recurse
```

## <a name="how-to-determine-property-values"></a>Özellik değerleri nasıl belirlenir?

Bir sonraki soru, performansla ilgili özellikleri sağlamak için hangi değeri belirleyebileceğinizi belirlemektir. Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **Adım 1: Toplam iş parçacığı sayısını belirleyin** - Kullanılacak toplam iş parçacığı sayısını hesaplayarak başlayın. Genel bir kılavuz olarak, her fiziksel çekirdek için altı iş parçacığı kullanmalısınız.

    `Total thread count = total physical cores * 6`

    **Örnek:**

    PowerShell komutlarını 16 çekirdekli bir D14 VM’den çalıştırdığınız varsayılmıştır

    `Total thread count = 16 cores * 6 = 96 threads`

* **Adım 2: PerFileThreadCount hesaplayın** - Biz dosyaların boyutuna göre PerFileThreadCount hesaplayın. 2,5 GB'dan küçük dosyalar için, 10 varsayılanı yeterli olduğundan bu parametreyi değiştirmeye gerek yoktur. 2,5 GB'dan büyük dosyalar için, ilk 2,5 GB için temel olarak 10 iş parçacığı kullanmalı ve dosya boyutundaki her 256 MB'lık artış için 1 iş parçacığı eklemelisiniz. Birçok farklı boyutta dosya içeren bir klasörü kopyalıyorsanız dosyaları benzer dosya boyutları halinde gruplandırmayı göz önünde bulundurun. Dosya boyutlarının benzer olmaması performansın düşmesine neden olabilir. Benzer boyutlu dosyaları gruplandırmanız mümkün değilse PerFileThreadCount değerini en büyük dosya boyutuna göre ayarlamanız gerekir.

    `PerFileThreadCount = 10 threads for the first 2.5 GB + 1 thread for each additional 256 MB increase in file size`

    **Örnek:**

    1 GB ile 10 GB arasında değişen 100 dosyanız olduğunu varsayarsak, 10 GB'ı denklem için en büyük dosya boyutu olarak kullanırız ve bu dosya aşağıdaki gibi okunur.

    `PerFileThreadCount = 10 + ((10 GB - 2.5 GB) / 256 MB) = 40 threads`

* **Adım 3: Eşzamanlı Dosya Sayısını Hesapla** - Aşağıdaki denklemi temel alan EşzamanlıDosya Sayımı'nı hesaplamak için toplam iş parçacığı sayısını ve PerFileThreadCount'ı kullanın:

    `Total thread count = PerFileThreadCount * ConcurrentFileCount`

    **Örnek:**

    Kullandığımız örnek değerler temel alınmıştır

    `96 = 40 * ConcurrentFileCount`

    Sonuç olarak **ConcurrentFileCount** değeri **2,4** ve bunu **2**’ye yuvarlayabiliriz.

## <a name="further-tuning"></a>Daha fazla ayar

Üzerinde çalışılan dosyaların boyutu çeşitlilik gösterdiğinden daha fazla ayar yapmanız gerekebilir. Dosyaların tümü veya çoğu daha büyük ve 10 GB aralığına daha yakınsa, önceki hesaplama iyi çalışır. Bunun yerine çoğu dosya küçük olacak şekilde birçok farklı dosya boyutu varsa PerFileThreadCount değerini azaltabilirsiniz. PerFileThreadCount değerini azaltarak ConcurrentFileCount değerini artırabiliriz. Bu nedenle, dosyalarımızın çoğunun 5 GB aralığında daha küçük olduğunu varsayarsak, hesaplamamızı yeniden yapabiliriz:

`PerFileThreadCount = 10 + ((5 GB - 2.5 GB) / 256 MB) = 20`

Yani, **EşzamanlıFileCount** 4,8 olan 96/20 olur, **4**yuvarlatılır .

Dosya boyutlarınızın dağılımına göre **PerFileThreadCount** değerini artırıp azaltarak bu ayarları değiştirmeye devam edebilirsiniz.

### <a name="limitation"></a>Sınırlama

* **Dosya sayısı ConcurrentFileCount değerinden az**: Karşıya yüklemekte olduğunuz dosyaların sayısı hesapladığınız **ConcurrentFileCount** değerinden azsa **ConcurrentFileCount** değerini dosya sayısına eşit olacak şekilde azaltmanız gerekir. **PerFileThreadCount** değerini artırmak için kalan iş parçacıklarından dilediğinizi kullanabilirsiniz.

* **Çok fazla iş parçacığı**: Kümenizin boyutunu artırmadan iş parçacığı sayısını çok fazla artırırsanız düşük performans riskiyle karşılaşabilirsiniz. CPU’da bağlam değişimi sırasında çekişme sorunları olabilir.

* **Yetersiz eşzamanlılık**: Eşzamanlılık yeterli değilse kümeniz çok küçük olabilir. Kümenizdeki düğüm sayısını artırabilirsiniz, bu da size daha fazla eşzamanlılık sağlar.

* **Azaltma hataları**: Eşzamanlılığınız çok yüksekse azaltma hataları görebilirsiniz. Azaltma hataları görüyorsanız eşzamanlılığı azaltmanız veya bize ulaşmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)

