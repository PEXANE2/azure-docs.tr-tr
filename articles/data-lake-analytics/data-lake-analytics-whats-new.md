---
title: Son değişiklikleri Data Lake Analytics
description: Bu makale, Data Lake Analytics yapılan son değişikliklerin sürekli bir listesini sağlar.
services: data-lake-analytics
author: xujiang1
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: overview
ms.author: xujiang1
ms.date: 07/31/2020
ms.openlocfilehash: e78389ffc06f1b4cd4e39c15ac66215d514e9bc1
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87476337"
---
# <a name="whats-new-in-data-lake-analytics"></a>Data Lake Analytics yenilikler nelerdir?

Azure Data Lake Analytics, belirli bileşenler için düzenli aralıklarla güncelleştirilir. En son güncelleştirmeyle güncel kalmak için, bu makalede hakkında bilgi verilmektedir:

- Anahtar bileşen Beta önizlemesi bildirimi
- Önemli bileşen sürümü bilgileri, örneğin: bileşen kullanılabilir sürümlerinin listesi, geçerli varsayılan sürüm vb.


## <a name="notification-of-key-component-beta-preview"></a>Anahtar bileşen Beta önizlemesi bildirimi

Önizleme için kullanılabilecek anahtar bileşen beta sürümü yok. 

## <a name="u-sql-runtime"></a>U-SQL çalışma zamanı

Derleyici, iyileştirici ve iş Yöneticisi de dahil olmak üzere Azure Data Lake U-SQL çalışma zamanı, U-SQL kodunuzu işler.

Azure Data Lake Analytics işini herhangi bir araçlardan gönderdiğinizde işiniz, üretim ortamında Şu anda kullanılabilir olan varsayılan çalışma zamanını kullanacaktır. 

Çalışma zamanı sürümü düzenli olarak güncelleştirilir. Ve önceki çalışma zamanı bir süredir kullanılabilir tutulacak. Yeni bir beta sürümü önizleme için hazır olduğunda, bu sürüm de kullanılabilir olacaktır.

Şu anda kullanılabilir çalışma zamanı sürümleri aşağıda verilmiştir.

- Yayın-20200124live_adl_16283022_2-- **geçerli varsayılan sürümü** >
- release_20200124live_adl_16283022
- release_20200124_adl_14480125
- release_20190904_adl_10236248_1
- release_20190904_adl_10236248
- release_20190904_adl_9225818

U-SQL çalışma zamanı hatalarıyla ilgili sorunları giderme hakkında daha fazla bilgiye ulaşmak için [u-SQL çalışma zamanı hatalarında sorun giderme](runtime-troubleshoot.md)bölümüne bakın.

## <a name="net-framework"></a>.NET Framework

Azure Data Lake Analytics artık **.NET Framework v 4.7.2**kullanıyor. 

Azure Data Lake Analytics U-SQL betik kodunuz özel derlemeler kullanıyorsa ve bu özel derlemeler .NET kitaplıklarını kullanıyorsa, herhangi bir sıralamaya sahip olup olmadığını denetlemek için kodunuzu doğrulayın.

.Net yükseltmesi ile [Ilgili sorun giderme](runtime-troubleshoot.md)kullanarak bir .net yükseltmesinde sorun gidermeyi öğrenmek için.

## <a name="release-note"></a>Yayın notunun

Son Güncelleştirme ayrıntıları için [Azure Data Lake Analytics sürüm notuna](https://github.com/Azure/AzureDataLake/tree/master/docs/Release_Notes)bakın.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal](data-lake-analytics-get-started-portal.md)  |  [Azure PowerShell](data-lake-analytics-get-started-powershell.md)  |  [CLI](data-lake-analytics-get-started-cli.md) kullanarak Data Lake Analytics kullanmaya başlama

