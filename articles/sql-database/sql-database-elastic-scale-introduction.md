---
title: Ölçeği genişletme
description: Hizmet Olarak Yazılım (SaaS) geliştiricileri bu araçları kullanarak bulutta kolayca elastik, ölçeklenebilir veritabanları oluşturabilir
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 44c6147402cbe05e62c091863cb0bd4f9235bfab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061638"
---
# <a name="scaling-out-with-azure-sql-database"></a>Azure SQL Veritabanı ile ölçek genişletme
**Elastik Veritabanı** araçlarını kullanarak Azure SQL veritabanlarını kolayca ölçeklendirebilirsiniz. Bu araçlar ve özellikler, işlem iş yükleri ve özellikle Hizmet Olarak Yazılım (SaaS) uygulamaları için çözümler oluşturmak için **Azure SQL Veritabanı'nın** veritabanı kaynaklarını kullanmanıza izin verir. Elastik Veritabanı özellikleri aşağıdakilerden oluşur:

* [Elastik Veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md): İstemci kitaplığı, kırık veritabanları oluşturmanıza ve korumanıza olanak tanıyan bir özelliktir.  Bkz. [Elastik Veritabanı araçları ile başlayın.](sql-database-elastic-scale-get-started.md)
* [Elastik Veritabanı bölme birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md): verileri kırık veritabanları arasında taşır. Bu araç, verileri çok kiracılı bir veritabanından tek kiracıveritabanına (veya tam tersi) taşımak için yararlıdır. Bkz. [Elastik veritabanı Bölme Birleştirme aracı](sql-database-elastic-scale-configure-deploy-split-and-merge.md)öğreticisi.
* [Esnek Veritabanı işleri](elastic-jobs-overview.md): Çok sayıda Azure SQL veritabanını yönetmek için işleri kullanın. İşleri kullanarak şema değişiklikleri, kimlik bilgileri yönetimi, referans veri güncelleştirmeleri, performans veri toplama veya kiracı (müşteri) telemetri koleksiyonu gibi yönetim işlemlerini kolayca gerçekleştirin.
* [Elastik Veritabanı sorgusu](sql-database-elastic-query-overview.md) (önizleme): Birden çok veritabanını kapsayan bir Transact-SQL sorgusu çalıştırmanızı sağlar. Bu, Excel, Power BI, Tableau, vb. gibi raporlama araçlarına bağlantı sağlar.
* [Esnek işlemler](sql-database-elastic-transactions-overview.md): Bu özellik, Azure SQL Veritabanı'nda çeşitli veritabanlarına yayılan hareketleri çalıştırmanızı sağlar. Elastik veritabanı işlemleri ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve [System.Transaction sınıfları](https://msdn.microsoft.com/library/system.transactions.aspx)kullanarak tanıdık programlama deneyimi ile entegre.

Aşağıdaki grafik, veritabanları koleksiyonuile ilgili **Olarak Elastik Veritabanı özelliklerini** içeren bir mimari gösterir.

Bu grafikte, veritabanının renkleri şemaları temsil eder. Aynı renkteki veritabanları aynı şemayı paylaşır.

1. Bir dizi **Azure SQL veritabanı,** parçalama mimarisi kullanılarak Azure'da barındırılır.
2. **Elastik Veritabanı istemci kitaplığı** bir parça kümesini yönetmek için kullanılır.
3. Veritabanlarının bir alt kümesi elastik bir **havuza**konur. [(Bkz. Havuz nedir?](sql-database-elastic-pool.md)).
4. **Elastik Veritabanı işi,** tüm veritabanlarına karşı zamanlanmış veya geçici T-SQL komut dosyaları çalıştırın.
5. **Bölme birleştirme aracı,** verileri bir parçadan diğerine taşımak için kullanılır.
6. **Elastik Veritabanı sorgusu,** parça kümesindeki tüm veritabanlarını kapsayan bir sorgu yazmanızı sağlar.
7. **Esnek hareketler,** çeşitli veritabanlarına yayılan hareketleri çalıştırmanızı sağlar. 

![Elastik Veritabanı araçları][1]

## <a name="why-use-the-tools"></a>Neden araçları kullanıyorsun?
Bulut uygulamaları için esneklik ve ölçek elde etmek, VM'ler ve blob depolama için kolay olmuştur - sadece birimleri ekleyin veya çıkarın veya gücü artırın. Ancak ilişkisel veritabanlarında durumsal veri işleme için bir meydan okuma kalmıştır. Bu senaryolarda sorunlar ortaya çıktı:

* İş yükünüzün ilişkisel veritabanı bölümü için büyüyen ve küçülen kapasite.
* Meşgul bir son müşteri (kiracı) gibi belirli bir veri alt kümesini etkileyen etkin noktaları yönetme.

Geleneksel olarak, bu gibi senaryolar uygulamayı desteklemek için daha büyük ölçekli veritabanı sunucularına yatırım yaparak ele alınmıştır. Ancak, bu seçenek, tüm işlemlerin önceden tanımlanmış emtia donanımında gerçekleştiği bulutta sınırlıdır. Bunun yerine, veri ve işlemenin birçok özdeş yapılandırılmış veritabanları ("parçalama" olarak bilinen ölçeklendirme deseni) arasında dağıtılması, hem maliyet hem de elastikiyet açısından geleneksel ölçeklendirme yaklaşımlarına alternatif sağlar.

## <a name="horizontal-and-vertical-scaling"></a>Yatay ve dikey ölçekleme
Aşağıdaki şekil, elastik veritabanlarının ölçeklendirilebildiği temel yollar olan ölçeklendirmenin yatay ve dikey boyutlarını gösterir.

![Yatay ve dikey ölçeklendirme][2]

Yatay ölçekleme, kapasiteyi veya genel performansı ayarlamak için veritabanları ekleme veya kaldırma anlamına gelir, buna "ölçekleme dışarı" olarak da adlandırılır. Verilerin aynı yapılandırılmış veritabanları koleksiyonuna bölündebağlı olduğu sharding, yatay ölçekleme uygulamanın yaygın bir yoludur.  

Dikey ölçekleme, "ölçekleme" olarak da bilinen tek bir veritabanının işlem boyutunu artırma veya azaltma anlamına gelir.

Bulut ölçekli veritabanı uygulamalarının çoğu bu iki stratejinin bir birleşimini kullanır. Örneğin, Hizmet Olarak Yazılım uygulaması, yeni son müşteriler sağlamak için yatay ölçekleme ve her son müşterinin veritabanının iş yükünün gerektirdiği şekilde kaynakları büyütmesine veya küçültmesine izin vermek için dikey ölçekleme kullanabilir.

* Yatay ölçekleme [Elastik Veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)kullanılarak yönetilir.
* Dikey ölçekleme, hizmet katmanını değiştirmek için Azure PowerShell cmdlets kullanılarak veya veritabanlarını esnek bir havuza yerleştirerek gerçekleştirilir.

## <a name="sharding"></a>Parçalama
*Parçalama*, aynı yapıya sahip büyük miktarlardaki verilerin bağımsız veritabanlarına dağıtılmasını kapsayan bir tekniktir. Özellikle bulut geliştiricilerin son müşteriler veya işletmeler için Hizmet Olarak Yazılım (SAAS) teklifleri oluşturması yla popülerdir. Bu son müşteriler genellikle "kiracı" olarak adlandırılır. Herhangi bir sayıda nedenden dolayı parçalama gerekebilir:  

* Toplam veri miktarı, tek bir veritabanının kısıtlamalarına sığmayacak kadar büyüktür
* Genel iş yükünün işlem oluşturma işlemi, tek bir veritabanının yeteneklerini aşıyor
* Kiracılar birbirinden fiziksel izolasyon gerektirebilir, bu nedenle her kiracı için ayrı veritabanları gereklidir
* Bir veritabanının farklı bölümlerinin uyumluluk, performans veya jeopolitik nedenlerle farklı coğrafyalarda yer etmesi gerekebilir.

Dağıtılmış aygıtlardan veri alma gibi diğer senaryolarda, parçalama zamansal olarak düzenlenen veritabanları kümesini doldurmak için kullanılabilir. Örneğin, ayrı bir veritabanı her gün veya hafta adanabilir. Bu durumda, parçalama anahtarı tarihi (parçalanmış tabloların tüm satırlarında bulunan) temsil eden bir tamsayı olabilir ve bir tarih aralığı için bilgi alan sorgular, uygulama tarafından söz konusu aralığı kapsayan veritabanlarıalt kümesine yönlendirilmelidir.

Bir uygulamadaki her işlem bir parçalama anahtarının tek bir değeriyle sınırlandırılabilirse, parçalama en iyi şekilde çalışır. Bu, tüm hareketlerin belirli bir veritabanına yerel olmasını sağlar.

## <a name="multi-tenant-and-single-tenant"></a>Çok kiracılı ve tek kiracılı
Bazı uygulamalar, her kiracı için ayrı bir veritabanı oluşturma nın en basit yaklaşımını kullanır. Bu yaklaşım, kiracının parçalı lıkta yalıtım, yedekleme/geri yükleme yeteneği ve kaynak ölçekleme sağlayan **tek kiracı parçalama desenidir.** Tek kiracı parçalama ile, her veritabanı belirli bir kiracı kimliği değeri (veya müşteri anahtar değeri) ile ilişkilidir, ancak bu anahtar her zaman veri kendisi nde mevcut olması gerekmez. Her isteği uygun veritabanına yönlendirmek uygulamanın sorumluluğundadır ve istemci kitaplığı bunu basitleştirebilir.

![Tek kiracı karşı çok kiracı][4]

Diğer senaryolar, birden çok kiracıyı ayrı veritabanlarına ayırmak yerine veritabanlarında bir araya getirir. Bu desen tipik bir **çok kiracılı parçalama desenidir** ve bir uygulamanın çok sayıda küçük kiracıyı yönetmesi nedeniyle yönlendirilebilir. Çok kiracılı parçalamada, veritabanı tablolarındaki satırların tümü kiracı kimliğini veya parçalama anahtarını tanımlayan bir anahtar taşımak üzere tasarlanmıştır. Yine, uygulama katmanı kiracının isteğini uygun veritabanına yönlendirmekten sorumludur ve bu, elastik veritabanı istemci kitaplığı tarafından desteklenebilir. Buna ek olarak, satır düzeyinde güvenlik, her kiracının erişebileceği satırları filtrelemek için kullanılabilir - ayrıntılar [için, elastik veritabanı araçları ve satır düzeyinde güvenlik içeren çok kiracılı uygulamalara](sql-database-elastic-tools-multi-tenant-row-level-security.md)bakın. Çok kiracılı parçalama deseni ile veri veritabanları arasında yeniden dağıtmak gerekebilir ve elastik veritabanı bölme birleştirme aracı tarafından kolaylaştırılır. Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verileri birden çok kira veritabanlarından tek kiraveritabanlarına taşıma
Bir SaaS uygulaması oluştururken, olası müşterilere yazılımın deneme sürümünü sunmak tipiktir. Bu durumda, veriler için çok kiracılı bir veritabanı kullanmak uygun maliyetlidir. Ancak, bir müşteri müşteri olduğunda, daha iyi performans sağladığından tek kiracılı veritabanı daha iyidir. Müşteri deneme süresi boyunca veri oluşturmuşsa, verileri çok kiracıdan yeni tek kiracı veritabanına taşımak için [bölme birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar
İstemci kitaplığını gösteren örnek bir uygulama için [bkz.](sql-database-elastic-scale-get-started.md)

Araçları kullanmak için varolan veritabanlarını dönüştürmek [için, ölçeklendirmek için varolan veritabanlarını geçir'e](sql-database-elastic-convert-to-use-elastic-tools.md)bakın.

Elastik havuzun özelliklerini görmek [için, elastik havuz için Fiyat ve performans hususlarına](sql-database-elastic-pool.md)bakın veya [elastik havuzlu](sql-database-elastic-pool-manage-portal.md)yeni bir havuz oluşturun.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

