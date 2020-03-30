---
title: Azure Veri Gölü Analytics U-SQL çalışma zamanı hatalarını giderme
description: U-SQL çalışma zamanı hatalarını nasıl gidereceklerini öğrenin.
services: data-lake-analytics
author: guyhay
ms.author: guyhay
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.workload: big-data
ms.date: 10/10/2019
ms.openlocfilehash: 1e3fb218e6cda5619bfa1a0936e07d6731a9cc93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73648460"
---
# <a name="learn-how-to-troubleshoot-u-sql-runtime-failures-due-to-runtime-changes"></a>Çalışma zamanı değişiklikleri nedeniyle U-SQL çalışma zamanı hatalarını nasıl gidereceklerini öğrenin

Derleyici, optimize edici ve iş yöneticisi de dahil olmak üzere Azure Veri Gölü U-SQL çalışma zamanı, U-SQL kodunuzu işleyen şeydir.

## <a name="choosing-your-u-sql-runtime-version"></a>U-SQL çalışma zamanı sürümünüzü seçme

Visual Studio, ADL SDK veya Azure Veri Gölü Analizi portalından U-SQL işlerini gönderdiğinizde, işiniz geçerli olan varsayılan çalışma süresini kullanır. U-SQL çalışma zamanının yeni sürümleri düzenli olarak yayımlanır ve hem küçük güncelleştirmeleri hem de güvenlik düzeltmelerini içerir.

Ayrıca özel bir çalışma zamanı sürümü de seçebilirsiniz; yeni bir güncelleştirme denemek istediğiniziçin, çalışma zamanının eski bir sürümünde kalmanız veya normal yeni güncelleştirmeyi bekleyemeyeceğiniz bildirilen bir sorun için bir düzeltme sağlanmış olması gerekir.

> [!CAUTION]
> Varsayılandan farklı bir çalışma zamanı seçmek, U-SQL işlerinizi bozma potansiyeline sahiptir. Yalnızca sınama için bu diğer sürümleri kullanın.

Nadir durumlarda, Microsoft Destek çalışma zamanının farklı bir sürümünü hesabınız için varsayılan olarak sabitleyebilir. Lütfen bu pimi mümkün olan en kısa sürede geri aldığınızdan emin olun. Bu sürüme sabitkalırsanız, daha sonraki bir tarihte süresi dolacaktır.

### <a name="monitoring-your-jobs-u-sql-runtime-version"></a>İşlerinizi izleme U-SQL runtime sürümü

Visual Studio'nun iş tarayıcısı veya Azure portalının iş geçmişi aracılığıyla geçmiş işlerinizin hesabınızın iş geçmişinde hangi çalışma zamanı sürümünü kullandığını görebilirsiniz.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Tüm İşleri Görüntüle'yi**seçin. Hesaptaki tüm etkin ve son tamamlanan işlerin listesi görüntülenir.
3. İsteğe bağlı olarak, **Zaman Aralığı,** İş **Adı**ve **Yazar** değerlerine göre işleri bulmanıza yardımcı olmak için **Filtre'yi** tıklatın.
4. Tamamlanan işlerde kullanılan çalışma zamanını görebilirsiniz.

![Önceki bir işin çalışma zamanı sürümünü görüntüleme](./media/runtime-troubleshoot/prior-job-usql-runtime-version-.png)

Kullanılabilir çalışma zamanı sürümleri zaman içinde değişir. Varsayılan çalışma süresi her zaman "varsayılan" olarak adlandırılır ve en azından önceki çalışma zamanını bir süre için kullanılabilir tutar ve çeşitli nedenlerle özel çalışma sürelerini kullanılabilir hale getirebiliriz. Açıkça adlandırılmış çalışma süreleri genellikle aşağıdaki biçimi izleyin (italik değişken parçalar için kullanılır ve [] isteğe bağlı parçaları gösterir):

release_YYYYMMDD_adl_buildno[_modifier]

Örneğin, release_20190318_adl_3394512_2, 18 Mart 2019'daki çalışma zamanı sürümünün build 3394512'sinin ikinci sürümü ve release_20190318_adl_3394512_private'nin de aynı sürümün özel bir yapısı anlamına geldiği anlamına gelir. Not: Tarih, son iadenin bu sürüm için ne zaman yapıldığıyla ilgilidir ve resmi çıkış tarihi ne zaman olması gerekmez.

Aşağıdakiler şu anda kullanılabilir çalışma zamanı sürümleridir.

- release_20190318_adl_3394512
- geçerli varsayılan release_20190318_adl_5832669
- release_20190703_adl_4713356

## <a name="troubleshooting-u-sql-runtime-version-issues"></a>U-SQL çalışma zamanı sürüm sorunlarını giderme

Karşılaşabileceğiniz iki olası çalışma zamanı sürümü sorunu vardır:

1. Komut dosyası veya bazı kullanıcı kodu davranışı bir sürümden diğerine değiştiriyor. Bu tür kırılma değişiklikleri normalde sürüm notlarının yayınlanması ile önceden iletilir. Böyle bir kırılma yla karşılaşırsanız, bu son kesme davranışını bildirmek için lütfen Microsoft Destek'e başvurun (henüz belgelenmemiş olması durumunda) ve işlerinizi eski çalışma zamanı sürümüne karşı gönderin.

2. Hesabınıza sabitlendiğinde ve bir süre sonra çalışma süresi kaldırıldığında varsayılan olmayan bir çalışma süresi kullanıyorsunuz. Eksik çalışma süreleri ile karşılaşırsanız, geçerli varsayılan çalışma zamanı ile çalıştırmak için lütfen komut dosyalarınızı yükseltin. Ek zamana ihtiyacınız varsa, lütfen Microsoft Destek'e başvurun

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Veri Gölü Analizine genel bakış](data-lake-analytics-overview.md)
- [Azure portalLarını kullanarak Azure Veri Gölü Analizini yönetme](data-lake-analytics-manage-use-portal.md)
- [Azure portalını kullanarak Azure Veri Gölü Analitiği'ndeki işleri izleme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
