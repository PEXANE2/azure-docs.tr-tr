---
title: Azure Veri Gölü Analizi kodunu yerel olarak hata ayıklama
description: Yerel iş istasyonunuzdaki U-SQL işlerini hata ayıklamak için Visual Studio için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 0827311218202de447e5cf27356e00c4da020e94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61473000"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Azure Veri Gölü Analizi kodunu yerel olarak hata ayıklama

Visual Studio için Azure Veri Gölü Araçlarını kullanarak, azure veri gölü analizi hizmetinde olduğu gibi yerel iş istasyonunuzda Azure Veri Gölü Analizi kodunu çalıştırıp hata ayıklayabilirsiniz.

[Yerel makinenizde U-SQL komut dosyasını nasıl çalıştırılacınız öğrenin.](data-lake-analytics-data-lake-tools-local-run.md)

## <a name="debug-scripts-and-c-assemblies-locally"></a>Betikler ve C# derlemeleri üzerinde yerel olarak hata ayıklama

C# derlemelerini Göndermeden ve Azure Veri Gölü Analizi hizmetine kaydetmeden hata ayıklayabilirsiniz. Hem kod arkası dosyasında hem de başvurulan c# projesinde kesme noktaları ayarlayabilirsiniz.

### <a name="debug-local-code-in-a-code-behind-file"></a>Kod arkası dosyasındaki yerel kodu hata ayıklama

1. Kod arkasındaki dosyada kesme noktaları ayarlayın.
2. Komut dosyasının yerel olarak hata ayıklamasını yapmak için **F5'i** seçin.

> [!NOTE]
   > Aşağıdaki yordam yalnızca Visual Studio 2015'te çalışır. Eski Visual Studio **sürümlerinde, PDB** dosyalarını el ile eklemeniz gerekebilir.  
   >
   >

### <a name="debug-local-code-in-a-referenced-c-project"></a>Başvurulan C# projesinde yerel kodu hata ayıklama

1. Bir C# derleme projesi oluşturun ve çıktı **DLL** dosyasını oluşturmak için oluşturun.
2. U-SQL deyimi kullanarak **DLL** dosyasını kaydedin:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. C# kodunda kesme noktalarını ayarlayın.
4. C# **DLL** dosyasına yerel olarak başvurarak komut dosyasının hatasını ayıklamak için **F5'i** seçin.


## <a name="next-steps"></a>Sonraki adımlar

- Daha karmaşık bir sorgu örneği için [bkz.](data-lake-analytics-analyze-weblogs.md)
- İş ayrıntılarını görüntülemek [için Azure Veri Gölü Analizi işleri için İş Tarayıcısı ve İş Görünümü'nü kullanın'a](data-lake-analytics-data-lake-tools-view-jobs.md)bakın.
- Vertex yürütme görünümünü kullanmak [için](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)bkz.
