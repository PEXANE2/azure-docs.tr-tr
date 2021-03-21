---
title: Azure Data Lake Analytics U-SQL çalışma zamanı hatalarında sorun giderme
description: U-SQL çalışma zamanı hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.date: 10/10/2019
ms.openlocfilehash: 1236b83b410057e55015391772e37bd461a448d0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102030622"
---
# <a name="learn-how-to-troubleshoot-u-sql-runtime-failures-due-to-runtime-changes"></a>Çalışma zamanı değişiklikleri nedeniyle U-SQL çalışma zamanı hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin

Derleyici, iyileştirici ve iş Yöneticisi de dahil olmak üzere Azure Data Lake U-SQL çalışma zamanı, U-SQL kodunuzu işler.

## <a name="choosing-your-u-sql-runtime-version"></a>U-SQL çalışma zamanı sürümünüzü seçme

U-SQL işlerini Visual Studio 'dan, ADL SDK veya Azure Data Lake Analytics portalından gönderdiğinizde, işiniz Şu anda kullanılabilir olan varsayılan çalışma zamanını kullanır. U-SQL çalışma zamanının yeni sürümleri düzenli aralıklarla yayımlanır ve hem küçük güncelleştirmeler hem de güvenlik düzeltmelerini içerir.

Ayrıca, özel bir çalışma zamanı sürümü de seçebilirsiniz; Yeni bir güncelleştirmeyi denemek istediğiniz için, bir çalışma zamanının daha eski bir sürümü üzerinde kalmak veya düzenli yeni güncelleştirme için bekleyemez, bildirilen bir sorun için bir düzeltme ile sağlanması gerekir.

> [!CAUTION]
> Varsayılandan farklı bir çalışma zamanı seçilmesi, U-SQL işlerinizi bölmek için olası olur. Bu diğer sürümleri yalnızca test için kullanın.

Nadir durumlarda Microsoft Desteği, çalışma zamanının farklı bir sürümünü hesabınız için varsayılan olarak sabitleyebilir. Lütfen bu PIN 'i mümkün olan en kısa sürede döndürtığınızdan emin olun. Bu sürüme sabitlenmiş olarak kaldıysanız, daha sonraki bir tarihte sona erer.

### <a name="monitoring-your-jobs-u-sql-runtime-version"></a>İşleri izleme U-SQL çalışma zamanı sürümü

Geçmiş işlerinizin, Visual Studio 'nun iş tarayıcısı veya Azure portal iş geçmişi aracılığıyla hesabınızın iş geçmişinde hangi çalışma zamanı sürümünün kullanıldığını görebilirsiniz.

1. Azure portal Data Lake Analytics hesabınıza gidin.
2. **Tüm Işleri görüntüle**' yi seçin. Hesaptaki tüm etkin ve son tamamlanan işlerin bir listesi görüntülenir.
3. İsteğe bağlı olarak, **zaman aralığı**, **Iş adı** ve **Yazar** değerlerine göre işleri bulmanıza yardımcı olması için **filtre** ' ye tıklayın.
4. Tamamlanan işlerde kullanılan çalışma zamanını görebilirsiniz.

![Geçmiş bir işin çalışma zamanı sürümünü görüntüleme](./media/runtime-troubleshoot/prior-job-usql-runtime-version-.png)

Kullanılabilir çalışma zamanı sürümleri zaman içinde değişir. Varsayılan çalışma zamanı her zaman "varsayılan" olarak adlandırılır ve bir süre içinde en az bir önceki çalışma zamanı ve çeşitli nedenlerle özel çalışma zamanları kullanılabilir hale getirir. Açıkça adlandırılmış çalışma zamanları genellikle aşağıdaki biçimi izler (örneğin, değişken parçaları için italik kullanılır ve [] isteğe bağlı bölümleri gösterir):

release_YYYYMMDD_adl_buildno [_modifier]

Örneğin, release_20190318_adl_3394512_2, 18 2019 Mart 'ın çalışma zamanı sürümünün ikinci sürümü olan 3394512 ve release_20190318_adl_3394512_private aynı sürümün özel bir derlemesini gösterir. Not: Tarih, bu yayın için son iade edildiğinde ve resmi yayın tarihi olması halinde ile ilgilidir.


## <a name="troubleshooting-u-sql-runtime-version-issues"></a>U-SQL çalışma zamanı sürüm sorunlarını giderme

Karşılaşabileceğiniz iki olası çalışma zamanı sürümü sorunu vardır:

1. Bir komut dosyası veya bazı Kullanıcı kodu, bir sürümden bir sonrakine kadar davranış değiştiriyor. Bu tür önemli değişiklikler normalde sürüm notları yayınlanmasıyla zaman önce iletilir. Bu tür bir son değişiklik yaşarsanız, bu bölme davranışını raporlamak için Microsoft Desteği başvurun (henüz açıklanmamıştır) ve işlerinizi eski çalışma zamanı sürümüne göre gönderebilirsiniz.

2. Hesabınız için sabitlenmiş ve bu çalışma zamanının bir süre sonra kaldırılmasıyla, varsayılan olmayan bir çalışma zamanını açıkça veya örtük olarak kullanıyorsunuz. Eksik çalışma zamanları ile karşılaşırsanız, betikleri geçerli varsayılan çalışma zamanıyla çalışacak şekilde yükseltin. Ek zamana ihtiyacınız varsa Microsoft Desteği başvurun

## <a name="known-issues"></a>Bilinen sorunlar

1. Bir USQL betiğine dosya sürüm 12.0.3 veya onende Newtonsoft.Jsbaşvurmak, aşağıdaki derleme hatasına neden olur:

    *"Üzgünüz; Data Lake Analytics hesabınızda çalışan işler muhtemelen daha yavaş çalışacak veya tamamlanmayacak. Beklenmeyen bir sorun, bu işlevselliği Azure Data Lake Analytics hesabınıza otomatik olarak geri yüklememizi engellemektedir. Araştırmak için Azure Data Lake mühendislerle iletişim kuruldu. "*  

    Çağrı yığınının nerede içereceği:  
    `System.IndexOutOfRangeException: Index was outside the bounds of the array.`  
    `at Roslyn.Compilers.MetadataReader.PEFile.CustomAttributeTableReader.get_Item(UInt32 rowId)`  
    `...`

    **Çözüm**: lütfen Newtonsoft.Jsv 12.0.2 veya daha düşük bir dosya üzerinde kullanın.
2. Müşteriler mağazalarına geçici dosya ve klasörler görebilirler. Bunlar normal iş yürütmesinin bir parçası olarak üretilir, ancak genellikle müşteriler onları görüntülemeden önce silinir. Nadir ve rastgele olan belirli koşullarda bir süre görünür kalabilirler. Bunlar sonunda silinir ve Kullanıcı depolamanın bir parçası olarak hiçbir zaman sayılmaz veya herhangi bir ücrete tabi olan herhangi bir ücret oluşturmazlar. Müşterilerin iş mantığına bağlı olarak, sorunlara neden olabilir. Örneğin, iş, klasördeki tüm dosyaları numaralandırır ve sonra dosya listelerini karşılaştırırsa, beklenmeyen geçici dosyalar nedeniyle başarısız olabilir. Benzer şekilde, bir aşağı akış işi daha fazla işleme için belirli bir klasörden tüm dosyaları numaralandıruyorsa, geçici dosyaları da numaralandıramıyor olabilir.  

    **Çözüm**: geçici dosyaların, geçerli çıkış klasöründen hesap düzeyinde Temp klasöründe depolanacağı çalışma zamanında bir düzelme belirlenir. Geçici dosyalar bu yeni Temp klasörüne yazılır ve iş yürütmesinin sonunda silinir.  
    Bu düzeltilme müşteri verilerini işlediğinden, bu düzeltilmek çok önemli oldukça önemlidir. Bu düzeltmesinin 2021 yılının ortasında Beta çalışma zamanı olarak kullanılabilmesi ve 2021 yılının ikinci yarısında varsayılan çalışma zamanı olması beklenmektedir. 


## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Analytics genel bakış](data-lake-analytics-overview.md)
- [Azure portal kullanarak Azure Data Lake Analytics yönetme](data-lake-analytics-manage-use-portal.md)
- [Azure Data Lake Analytics Azure portal kullanarak işleri izleme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
