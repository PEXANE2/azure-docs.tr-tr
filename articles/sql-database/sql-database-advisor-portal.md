---
title: Performans önerilerini uygulama
description: Azure SQL Veritabanınızın performansını en iyi duruma getirebilecek performans önerilerini bulmak için Azure portalını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 12/19/2018
ms.openlocfilehash: b0452d51dc472e100ef52536d8e3814ff395292b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214171"
---
# <a name="find-and-apply-performance-recommendations"></a>Performans önerilerini bulma ve uygulama

Azure portalını, Azure SQL Veritabanınızın performansını optimize edebilecek performans önerilerini bulmak veya iş yüklerinizde tanımlanan bazı sorunu düzeltmek için kullanabilirsiniz. Azure portalındaki **performans önerisi** sayfası, olası etkilerine bağlı olarak en iyi önerileri bulmanıza olanak tanır.

## <a name="viewing-recommendations"></a>Önerileri görüntüleme

Performans önerilerini görüntülemek ve uygulamak için Azure'da doğru [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) izinlerine ihtiyacınız vardır. **Okuyucu**, **SQL DB Katılımcısı** izinleri önerileri görüntülemek için gereklidir ve **Sahibi**, **SQL DB Katılımcı** izinleri herhangi bir eylemi yürütmek için gereklidir; dizinoluşturma veya bırakma ve dizin oluşturmayı iptal etme.

Azure portalında performans önerilerini bulmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Tüm **hizmetler** > **SQL veritabanlarına**gidin ve veritabanınızı seçin.
3. Seçili veritabanı için kullanılabilir önerileri görüntülemek için **Performans önerisine** gidin.

Performans önerileri tabloda aşağıdaki şekilde gösterilene benzer şekilde gösterilir:

![Öneriler](./media/sql-database-advisor-portal/recommendations.png)

Öneriler, performans üzerindeki potansiyel etkilerine göre aşağıdaki kategorilere göre sıralanır:

| Etki | Açıklama |
|:--- |:--- |
| Yüksek |Yüksek etkili öneriler en önemli performans etkisini sağlamalıdır. |
| Orta |Orta etkili öneriler performansı artırmalı, ancak önemli ölçüde değil. |
| Düşük |Düşük etkili öneriler olmadan daha iyi performans sağlamalıdır, ancak iyileştirmeler önemli olmayabilir. |

> [!NOTE]
> Azure SQL Veritabanı'nın bazı önerileri belirlemek için etkinlikleri en az bir gün boyunca izlemesi gerekir. Azure SQL Veritabanı, tutarlı sorgu desenleri için rasgele parçalı etkinlik patlamalarına göre daha kolay optimize edilebilir. Öneriler şu anda kullanılamıyorsa, **Performans önerisi** sayfası nedenini açıklayan bir ileti sağlar.

Ayrıca, geçmiş işlemlerin durumunu da görüntüleyebilirsiniz. Daha fazla bilgi görmek için bir öneri veya durum seçin.

Azure portalında "Dizin oluştur" önerisine bir örnek aşağıda verilmiştir.

![Dizin oluşturma](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Önerilerin uygulanması

Azure SQL Veritabanı, aşağıdaki üç seçenekten herhangi birini kullanarak önerilerin nasıl etkinleştirildigi konusunda tam denetim sağlar:

* Tek tek önerileri teker teker uygulayın.
* Önerileri otomatik olarak uygulamak için Otomatik ayar'ı etkinleştirin.
* Bir öneriyi el ile uygulamak için, önerilen T-SQL komut dosyasını veritabanınıza karşı çalıştırın.

Ayrıntılarını görüntülemek için herhangi bir öneriyi seçin ve ardından önerinin nasıl oluşturulduğuna ilişkin tam ayrıntıları gözden geçirmek için **komut dosyasını** görüntüle'yi tıklatın.

Öneri uygulanırken veritabanı çevrimiçi kalır - performans önerisi veya otomatik ayar kullanarak hiçbir zaman bir veritabanını çevrimdışı duruma almaz.

### <a name="apply-an-individual-recommendation"></a>Bireysel bir öneri uygulama

Önerileri teker teker gözden geçirebilir ve kabul edebilirsiniz.

1. **Öneriler** sayfasında bir öneri seçin.
2. **Ayrıntılar** sayfasında **Uygula** düğmesini tıklatın.

   ![Öneri uygulayın](./media/sql-database-advisor-portal/apply.png)

Seçilen öneri veritabanına uygulanır.

### <a name="removing-recommendations-from-the-list"></a>Önerileri listeden kaldırma

Öneri listeniz listeden kaldırmak istediğiniz öğeleri içeriyorsa, öneriyi atabilirsiniz:

1. Ayrıntıları açmak için **Öneriler** listesinde bir öneri seçin.
2. **Ayrıntılar** sayfasında **At'ı** tıklatın.

İstenirse, atılan öğeleri **Öneriler** listesine geri ekleyebilirsiniz:

1. **Öneriler** **sayfasında, atılanları görüntüle'yi**tıklatın.
2. Ayrıntılarını görüntülemek için listeden atılan bir öğeyi seçin.
3. İsteğe bağlı olarak, dizin **önerileri**ana listesine geri eklemek için **Geri At'ı** tıklatın.

> [!NOTE]
> SQL Veritabanı Otomatik [ayar](sql-database-automatic-tuning.md) ı etkinleştirildiyse ve listeden el ile bir öneri attıysanız, bu öneri hiçbir zaman otomatik olarak uygulanmaz. Bir öneriyi atmak, belirli bir önerinin uygulanmaması gereken durumlarda kullanıcıların Otomatik ayar ı etkinleştirmelerinin kullanışlı bir yoludur.
> Geri Alma seçeneğini seçerek, atılan önerileri Öneriler listesine geri ekleyerek bu davranışı geri alabilirsiniz.

### <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL Veritabanı'nı önerileri otomatik olarak uygulayacak şekilde ayarlayabilirsiniz. Öneriler kullanılabilir hale geldikçe, otomatik olarak uygulanır. Hizmet tarafından yönetilen tüm önerilerde olduğu gibi, performans etkisi negatifse, öneri geri döndürülz.

1. **Öneriler** sayfasında **Otomatikleştir'i**tıklatın:

   ![Danışman ayarları](./media/sql-database-advisor-portal/settings.png)
2. Otomatikleştirmek için eylemleri seçin:

   ![Önerilen Dizinler](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> **DROP_INDEX** seçeneğinin şu anda bölüm değiştirme ve dizin ipuçları nı kullanan uygulamalarla uyumlu olmadığını lütfen unutmayın.

İstediğiniz yapılandırmayı seçtikten sonra Uygula'yı tıklatın.

### <a name="manually-apply-recommendations-through-t-sql"></a>Önerileri T-SQL üzerinden el ile uygulayın

Herhangi bir öneri seçin ve ardından **komut dosyalarını görüntüle'yi**tıklatın. Öneriyi el ile uygulamak için bu komut dosyasını veritabanınıza karşı çalıştırın.

*El ile çalıştırılan dizinler, hizmet tarafından performans etkisi açısından izlenmez ve doğrulanmaz,* bu nedenle performans kazançları sağladıklarını doğrulamak ve gerekirse bunları ayarlamak veya silmek için bu dizinleri oluşturulduktan sonra izlemeniz önerilir. Dizin oluşturma hakkında ayrıntılı bilgi için [CREATE INDEX (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql)bakın. Buna ek olarak, el ile uygulanan öneriler etkin kalır ve 24-48 saat için öneriler listesinde gösterilir. sistem otomatik olarak geri çekmeden önce. Bir öneriyi daha erken kaldırmak isterseniz, el ile atabilirsiniz.

### <a name="canceling-recommendations"></a>Önerileri iptal etme

**Bekleyen,** **Doğrulama**veya **Başarı** durumunda olan öneriler iptal edilebilir. **Yürütme** durumu olan öneriler iptal edilemez.

1. **Öneriler ayrıntıları** sayfasını açmak için **Tuning History** alanında bir öneri seçin.
2. Tavsiyeyi uygulama işlemini iptal etmek için **İptal'i** tıklatın.

## <a name="monitoring-operations"></a>İzleme işlemleri

Bir öneri uygulamak anında gerçekleşmeyebilir. Portal, tavsiye nin durumu yla ilgili ayrıntıları sağlar. Aşağıdakiler, bir dizinin içinde olabileceği olası durumlardır:

| Durum | Açıklama |
|:--- |:--- |
| Beklemede |Uygulama önerisi komutu alındı ve yürütme için zamanlandı. |
| Yürütme |Öneri uygulanıyor. |
| Doğrulama |Öneri başarıyla uygulandı ve hizmet yararları ölçüyor. |
| Başarılı |Öneri başarıyla uygulandı ve faydaları ölçüldü. |
| Hata |Önerinin uygulanması sırasında bir hata oluştu. Bu geçici bir sorun veya tabloya bir şema değişikliği olabilir ve komut dosyası artık geçerli değildir. |
| Dönüştürme |Öneri uygulandı, ancak performans dışı kabul edildi ve otomatik olarak geri alınıyor. |
| Döndürülür |Tavsiye geri alındı. |

Daha fazla bilgi için listeden bir süreç içi önerisini tıklatın:

![Önerilen Dizinler](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Bir öneriyi geri çevirme

Öneriyi uygulamak için performans önerilerini kullandıysanız (yani T-SQL komut dosyasını el ile çalıştırmadın), performans etkisini negatif bulursa değişikliği otomatik olarak geri alır. Herhangi bir nedenle yalnızca bir öneriyi geri almak istiyorsanız, aşağıdakileri yapabilirsiniz:

1. **Tuning geçmişi** alanında başarıyla uygulanan bir öneri seçin.
2. **Öneri ayrıntıları** sayfasında **Geri Dön'e** tıklayın.

![Önerilen Dizinler](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Dizin önerilerinin performans etkisinin izlenmesi

Öneriler başarıyla uygulandıktan sonra (şu anda, dizin işlemleri ve yalnızca sorguları parametrenize etme önerileri), [Sorgu Performansı Öngörülerini](sql-database-query-performance.md) açmak ve en iyi sorgularınızın performans etkisini görmek için öneri ayrıntıları sayfasında Kisiler'i tıklatabilirsiniz. **Query Insights**

![Performans etkisini izleme](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Özet

Azure SQL Veritabanı, SQL veritabanı performansını artırmak için öneriler sağlar. T-SQL komut dosyaları sağlayarak, veritabanınızı en iyi duruma getirerek ve sonuçta sorgu performansını artırmada yardım alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Önerilerinizi izleyin ve performansı iyileştirmek için bunları uygulamaya devam edin. Veritabanı iş yükleri dinamiktir ve sürekli olarak değişir. Azure SQL Veritabanı, veritabanınızın performansını potansiyel olarak artırabilecek önerileri izlemeye ve sağlamaya devam eder.

* Azure SQL Veritabanı'nda otomatik ayar hakkında daha fazla bilgi edinmek için [Otomatik ayar'a](sql-database-automatic-tuning.md) bakın.
* Azure SQL Veritabanı performans önerilerine genel bakış için [Performans önerilerine](sql-database-advisor.md) bakın.
* En iyi sorgularınızın performans etkisini görüntüleme hakkında bilgi edinmek için [Sorgu Performansı Öngörüleri'ne](sql-database-query-performance.md) bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Sorgu Deposu](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rol tabanlı erişim denetimi](../role-based-access-control/overview.md)
