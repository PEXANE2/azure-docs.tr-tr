---
title: Azure Data Lake Analytics kodu yerel olarak hata ayıkla
description: Yerel iş istasyonunuzda U-SQL işlerinin hatalarını ayıklamak için Visual Studio için Azure Data Lake Araçları nasıl kullanacağınızı öğrenin.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 07/03/2018
ms.openlocfilehash: 1c7218deac9efba6df6c1284f2578a744e768284
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92221035"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Azure Data Lake Analytics kodu yerel olarak hata ayıkla

Azure Data Lake Analytics hizmetinde yapabildiğiniz gibi, yerel iş istasyonunuzda Azure Data Lake Analytics kodu çalıştırmak ve hatalarını ayıklamak için Visual Studio için Azure Data Lake Araçları kullanabilirsiniz.

[Yerel makinenizde U-SQL betiğini çalıştırmayı](data-lake-analytics-data-lake-tools-local-run.md)öğrenin.

## <a name="debug-scripts-and-c-assemblies-locally"></a>Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

C# derlemelerinin hatalarını Azure Data Lake Analytics hizmetine göndermeden ve kaydetmeksizin hata ayıklaması yapabilirsiniz. Yalnızca arka plan kod dosyasında ve başvurulan bir C# projesinde kesme noktaları ayarlayabilirsiniz.

### <a name="debug-local-code-in-a-code-behind-file"></a>Arka plan kod dosyasında yerel kod hatalarını ayıklama

1. Arka plan kod dosyasında kesme noktaları ayarlayın.
2. Komut dosyasında yerel olarak hata ayıklamak için **F5** ' i seçin.

> [!NOTE]
   > Aşağıdaki yordam yalnızca Visual Studio 2015 ' de geçerlidir. Daha eski Visual Studio sürümlerinde **pdb** dosyalarını el ile eklemeniz gerekebilir.  
   >
   >

### <a name="debug-local-code-in-a-referenced-c-project"></a>Başvurulan bir C# projesinde yerel kodda hata ayıklama

1. Bir C# derleme projesi oluşturun ve çıktı **DLL** dosyasını oluşturmak için oluşturun.
2. Bir U-SQL ifadesini kullanarak **DLL** dosyasını kaydedin:

   ```sql
   CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
   ```
   
3. C# kodunda kesme noktalarını ayarlayın.
4. C# **DLL** dosyasına yerel olarak başvurarak komut dosyasında hata ayıklamak için **F5** ' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

- Daha karmaşık bir sorgu örneği için bkz. [Azure Data Lake Analytics kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- İş ayrıntılarını görüntülemek için bkz. [Azure Data Lake Analytics işleri Için Iş tarayıcısını ve Iş görünümünü kullanma](data-lake-analytics-data-lake-tools-view-jobs.md).
- Köşe yürütme görünümünü kullanmak için bkz. [Visual Studio için Data Lake araçları 'Nda köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
