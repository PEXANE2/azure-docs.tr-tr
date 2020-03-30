---
title: Toplu uygulama yükseltmeleri
description: Bulut uygulamanızın çevrimiçi yükseltmelerini desteklemek için Azure SQL Veritabanı coğrafi çoğaltmasını nasıl kullanacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 02/13/2019
ms.openlocfilehash: 9c627c3e597fdcd3859ce02ea208fc7a8b5d612b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822869"
---
# <a name="manage-rolling-upgrades-of-cloud-applications-by-using-sql-database-active-geo-replication"></a>SQL Database etkin coğrafi çoğaltma kullanarak bulut uygulamalarının yuvarlanma yükseltmelerini yönetme

Bulut uygulamanızın yuvarlanma yükseltmelerini etkinleştirmek için Azure SQL Veritabanı'nda [etkin coğrafi çoğaltmayı](sql-database-auto-failover-group.md) nasıl kullanacağınızı öğrenin. Yükseltmeler yıkıcı işlemler olduğundan, iş sürekliliği planlamanızın ve tasarımınızın bir parçası olmalıdır. Bu makalede, yükseltme işlemini düzenlemenin iki farklı yöntemine bakıyoruz ve her seçeneğin avantajlarını ve dengelerini tartışıyoruz. Bu makalenin amaçları doğrultusunda, veri katmanı olarak tek bir veritabanına bağlı bir web sitesinden oluşan bir uygulamaya başvururuz. Amacımız, kullanıcı deneyimi üzerinde önemli bir etkisi olmadan uygulamanın sürüm 1 (V1) sürümünü sürüm 2 'ye (V2) yükseltmektir.

Yükseltme seçeneklerini değerlendirirken şu faktörleri göz önünde bulundurun:

* Uygulama işlevlerinin ne kadar süreyle sınırlandırılabileceği veya bozulabileceği gibi yükseltmeler sırasında uygulama kullanılabilirliği üzerindeki etkisi.
* Yükseltme başarısız olursa geri alma yeteneği.
* Yükseltme sırasında ilgisiz, felaket bir hata oluşursa uygulamanın güvenlik açığı.
* Toplam dolar maliyeti. Bu faktör, yükseltme işlemi tarafından kullanılan geçici bileşenlerin ek veritabanı artıklığı ve artımlı maliyetlerini içerir.

## <a name="upgrade-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Olağanüstü durum kurtarma için veritabanı yedeklemelerine dayanan yükseltme uygulamaları

Uygulamanız otomatik veritabanı yedeklemelerine dayanıyorsa ve olağanüstü durum kurtarma için coğrafi geri yükleme kullanıyorsa, tek bir Azure bölgesine dağıtılır. Kullanıcı kesintisini en aza indirmek için, yükseltmede yer alan tüm uygulama bileşenleriyle birlikte o bölgede bir hazırlama ortamı oluşturun. İlk diyagram, yükseltme işleminden önceki çalışma ortamını gösterir. Bitiş noktası, `contoso.azurewebsites.net` web uygulamasının üretim ortamını temsil eder. Yükseltmeyi geri alabilmek için, veritabanının tam olarak eşitlenmiş bir kopyasını içeren bir hazırlama ortamı oluşturmanız gerekir. Yükseltme için bir hazırlama ortamı oluşturmak için aşağıdaki adımları izleyin:

1. Aynı Azure bölgesinde ikincil bir veritabanı oluşturun. Tohumlama işleminin tamamlanmamış olup olmadığını görmek için ikincil izleyin (1).
2. Web uygulamanız için yeni bir ortam oluşturun ve buna 'Evreleme' deyin. URL (2) ile Azure DNS'de `contoso-staging.azurewebsites.net` kayıtlı olacaktır.

> [!NOTE]
> Bu hazırlık adımları, tam erişim modunda çalışabilen üretim ortamını etkilemez.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Hazırlama adımları tamamlandığında, uygulama gerçek yükseltme için hazırdır. Sonraki diyagramyükseltme işleminde yer alan adımları gösterir:

1. Birincil veritabanını salt okunur moduna ayarlayın (3). Bu mod, web uygulamasının (V1) üretim ortamının yükseltme sırasında salt okunur kalmasını garanti eder ve böylece V1 ve V2 veritabanı örnekleri arasındaki veri farklılaşmasını önler.
2. Planlanan sonlandırma modunu kullanarak ikincil veritabanını kesin (4). Bu eylem, birincil veritabanının tam olarak eşitlenmiş, bağımsız bir kopyasını oluşturur. Bu veritabanı yükseltilecek.
3. İkincil veritabanını okuma yazma moduna dönüştürün ve yükseltme komut dosyasını çalıştırın (5).

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Yükseltme başarıyla tamamlanırsa, artık kullanıcıları üretim ortamına dönüşen yükseltilmiş kopya uygulamasına değiştirmeye hazırsınız. Anahtarlama, bir sonraki diyagramda gösterildiği gibi birkaç adım daha içerir:

1. Web uygulamasının üretim ve hazırlama ortamları arasında bir takas işlemini etkinleştirin (6). Bu işlem, iki ortamın URL'lerini değiştirir. Şimdi `contoso.azurewebsites.net` web sitesi ve veritabanı (üretim ortamı) V2 sürümüne işaret eder. 
2. Takastan sonra bir evreleme kopyası haline gelen V1 sürümüne artık ihtiyacınız yoksa, evreleme ortamını devre dışı kullanabilirsiniz (7).

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Yükseltme işlemi başarısız olursa (örneğin, yükseltme komut dosyasındaki bir hata nedeniyle), hazırlama ortamının tehlikeye atılmasını göz önünde bulundurun. Uygulamayı yükseltme öncesi duruma geri almak için, üretim ortamındaki uygulamayı tam erişime geri döndürün. Sonraki diyagram, geri dönüş adımlarını gösterir:

1. Veritabanı kopyasını okuma yazma moduna ayarlayın (8). Bu eylem, üretim kopyasının tam V1 işlevselliğini geri yükler.
2. Kök neden çözümlemesi gerçekleştirin ve evreleme ortamını devre dışı bırakmak (9).

Bu noktada, uygulama tamamen işlevseldir ve yükseltme adımlarını yineleyebilirsiniz.

> [!NOTE]
> Henüz bir takas işlemi gerçekleştirmediğiniz için geri alma DNS değişiklikleri gerektirmez.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Bu seçeneğin en önemli avantajı, basit adımlar kümesini izleyerek tek bir bölgedeki bir uygulamayı yükseltebiliyor olmaktır. Yükseltme dolar maliyeti nispeten düşüktür. 

Ana denge, yükseltme sırasında bir felaket hatası oluşursa, ön yükseltme durumuna kurtarma farklı bir bölgede uygulama yeniden dağıtma ve coğrafi geri yükleme kullanarak yedek veritabanı geri içerir. Bu işlem önemli kapalı kalma süresi ile sonuçlanır.

## <a name="upgrade-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için veritabanı coğrafi çoğaltma güveniyor yükseltme uygulamaları

Uygulamanız iş sürekliliği için etkin coğrafi çoğaltma veya otomatik arıza grupları kullanıyorsa, en az iki farklı bölgeye dağıtılır. Birincil bölgede etkin, birincil veritabanı ve yedekleme bölgesinde salt okunur, ikincil veritabanı vardır. Bu makalenin başında belirtilen faktörlerin yanı sıra, yükseltme işlemi de garanti etmelidir:

* Uygulama, yükseltme işlemi sırasında her zaman yıkıcı hatalara karşı korunur.
* Uygulamanın coğrafi yedekbileşenleri etkin bileşenlere paralel olarak yükseltilir.

Bu hedeflere ulaşmak için, Web Uygulamaları ortamlarını kullanmanın yanı sıra, tek bir etkin bitiş noktası ve bir yedek bitiş noktası içeren bir hata üst üste profili kullanarak Azure Trafik Yöneticisi'nden yararlanırsınız. Sonraki diyagram, yükseltme işleminden önceki çalışma ortamını gösterir. Web siteleri `contoso-1.azurewebsites.net` `contoso-dr.azurewebsites.net` ve tam coğrafi artıklık ile uygulamanın bir üretim ortamı temsil eder. Üretim ortamı aşağıdaki bileşenleri içerir:

* Birincil bölgede web uygulamasının `contoso-1.azurewebsites.net` üretim ortamı (1)
* Birincil bölgedeki birincil veritabanı (2)
* Yedekleme bölgesinde web uygulamasının bekleme örneği (3)
* Yedekleme bölgesinde coğrafi çoğaltılmış ikincil veritabanı (4)
* Çevrimiçi bitiş noktası nın çağrıldığı `contoso-1.azurewebsites.net` ve çevrimdışı bitiş noktası nın çağrıldığı bir Trafik Yöneticisi performans profili`contoso-dr.azurewebsites.net`

Yükseltmeyi geri almak için, uygulamanın tam senkronize edilmiş bir kopyasını içeren bir hazırlama ortamı oluşturmanız gerekir. Yükseltme işlemi sırasında bir felaket hatası olması durumunda uygulamanın hızla kurtarılabilmesini sağlamanız gerektiğinden, evreleme ortamı da coğrafi yedekli olmalıdır. Yükseltme için bir hazırlama ortamı oluşturmak için aşağıdaki adımlar gereklidir:

1. Birincil bölgede web uygulamasının bir hazırlama ortamı nı dağıtın (6).
2. Birincil Azure bölgesinde ikincil bir veritabanı oluşturun (7). Web uygulamasının evreleme ortamını ona bağlanacak şekilde yapılandırın. 
3. Birincil bölgedeki ikincil veritabanını çoğaltarak yedekleme bölgesinde başka bir coğrafi yedekli, ikincil veritabanı oluşturun. (Bu *yöntemzincirli coğrafi çoğaltma*olarak adlandırılır.) (8).
4. Yedekleme bölgesinde web uygulaması örneğinin bir evreleme ortamını dağıtın (9) ve (8) oluşturulan coğrafi yedekli ikincil veritabanını bağlayacak şekilde yapılandırın.

> [!NOTE]
> Bu hazırlık adımları, üretim ortamındaki uygulamayı etkilemez. Okuma yazma modunda tamamen işlevsel olmaya devam edecektir.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Hazırlama adımları tamamlandığında, hazırlama ortamı yükseltme için hazırdır. Sonraki diyagramda bu yükseltme adımları gösteriş:

1. Üretim ortamındaki birincil veritabanını salt okunur moduna ayarlayın (10). Bu mod, yükseltme sırasında üretim veritabanının (V1) değişmeyeceğini garanti eder ve böylece V1 ve V2 veritabanı örnekleri arasındaki veri farklılaşmasını önler.

```sql
-- Set the production database to read-only mode
ALTER DATABASE <Prod_DB>
SET (ALLOW_CONNECTIONS = NO)
```

2. İkincil bağlantıyı keserek coğrafi çoğaltmayı sonlandırın (11). Bu eylem, üretim veritabanının bağımsız ancak tam olarak eşitlenmiş bir kopyasını oluşturur. Bu veritabanı yükseltilecek. Aşağıdaki örnekte Transact-SQL kullanır, ancak [PowerShell](/powershell/module/az.sql/remove-azsqldatabasesecondary?view=azps-1.5.0) de kullanılabilir. 

```sql
-- Disconnect the secondary, terminating geo-replication
ALTER DATABASE <Prod_DB>
REMOVE SECONDARY ON SERVER <Partner-Server>
```

3. Yükseltme komut dosyasını `contoso-dr-staging.azurewebsites.net`, ve evreleme birincil veritabanına (12) karşı `contoso-1-staging.azurewebsites.net`çalıştırın. Veritabanı değişiklikleri otomatik olarak ikincil evreleme çoğaltılır.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Yükseltme başarıyla biterse, artık kullanıcıları uygulamanın V2 sürümüne değiştirmeye hazırsınız. Sonraki diyagramda ilgili adımlar gösteriş:

1. Birincil bölgedeki (13) ve yedekleme bölgesindeki web uygulamasının üretim ve hazırlama ortamları arasında bir takas işlemini etkinleştirin (14). Uygulamanın V2'si artık yedekleme bölgesinde gereksiz bir kopyayla bir üretim ortamı haline gelir.
2. Artık V1 uygulamasına (15 ve 16) ihtiyacınız yoksa, hazırlama ortamını devre dışı kullanabilirsiniz.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Yükseltme işlemi başarısız olursa (örneğin, yükseltme komut dosyasındaki bir hata nedeniyle), hazırlama ortamının tutarsız bir durumda olduğunu düşünün. Uygulamayı yükseltme öncesi duruma geri almak için, üretim ortamında uygulamanın V1'ini kullanmaya geri dön. Gerekli adımlar sonraki diyagramda gösterilir:

1. Üretim ortamındaki birincil veritabanı kopyasını okuma yazma moduna ayarlayın (17). Bu eylem, üretim ortamında tam V1 işlevselliğini geri yükler.
2. Kök neden çözümlemesi yapın ve evreleme ortamını onarın veya kaldırın (18 ve 19).

Bu noktada, uygulama tamamen işlevseldir ve yükseltme adımlarını yineleyebilirsiniz.

> [!NOTE]
> Bir takas işlemi gerçekleştirmediğiniz için geri alma işlemi DNS değişiklikleri gerektirmez.

![Bulut felaketkurtarma için SQL Veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Bu seçeneğin en önemli avantajı, yükseltme sırasında iş sürekliliğinizden ödün vermeden hem uygulamayı hem de coğrafi yedekli kopyasını paralel olarak yükseltebilmenizdir.

Ana tradeoff her uygulama bileşeni nin çift artıklık gerektirir ve bu nedenle daha yüksek dolar maliyeti tabi olmasıdır. Ayrıca daha karmaşık bir iş akışı içerir.

## <a name="summary"></a>Özet

Makalede açıklanan iki yükseltme yöntemi karmaşıklık ve dolar maliyeti açısından farklılık gösterir, ancak her ikisi de kullanıcının salt okunur işlemlerle sınırlı ne kadar süreyle sınırlı olduğunu en aza indirmeye odaklanır. Bu süre doğrudan yükseltme komut dosyasının süresiile tanımlanır. Veritabanı boyutuna, seçtiğiniz hizmet katmanına, web sitesi yapılandırmasına veya kolayca kontrol edilemediğiniz diğer etkenlere bağlı değildir. Tüm hazırlık adımları yükseltme adımlarından ayrışılır ve üretim uygulamasını etkilemez. Yükseltme komut dosyasının verimliliği, yükseltmeler sırasında kullanıcı deneyimini belirleyen önemli bir faktördür. Bu nedenle, bu deneyimi geliştirmenin en iyi yolu, yükseltme komut dosyasını mümkün olduğunca verimli hale getirmek için çabalarınızı odaklamaktır.

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliğine genel bakış ve senaryolar için [İş sürekliliğine genel bakış](sql-database-business-continuity.md)alabakın.
* Azure SQL Veritabanı etkin coğrafi çoğaltma hakkında bilgi edinmek için [bkz.](sql-database-active-geo-replication.md)
* Azure SQL Veritabanı otomatik hata grupları hakkında bilgi edinmek için, [birden çok veritabanının saydam ve eşgüdümlü başarısız](sql-database-auto-failover-group.md)olmasını sağlamak için otomatik hata grupları kullan'a bakın.
* Azure Uygulama Hizmeti'ndeki hazırlama ortamları hakkında bilgi edinmek için Azure [Uygulama Hizmeti'nde hazırlama ortamları ayarlama'ya](../app-service/deploy-staging-slots.md)bakın.
* Azure Trafik Yöneticisi profilleri hakkında bilgi edinmek için azure [trafik yöneticisi profilini yönet'e](../traffic-manager/traffic-manager-manage-profiles.md)bakın.
