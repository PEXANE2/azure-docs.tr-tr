---
title: Azure'da Cloudyn panolarıyla ana ölçümleri görüntüleme
description: Bu makalede Cloudyn panolarıyla ana ölçümleri nasıl görüntüleyebileceğiniz anlatılmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: vitavor
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 892df9a6e0eb4d791f818eed0a78c96a829e25a0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79464232"
---
# <a name="view-key-cost-metrics-with-dashboards"></a>Panolarla ana maliyet ölçümlerini görüntüleme

Cloudyn'deki panolar, raporların üst düzey görünümünü sunar. Panolar, ana maliyet ölçümlerini tek bir yerde görüntülemenizi sağlar. Ayrıca işlerinizle ilgili önemli kararlar vermenize yardımcı olmak için önemli iş eğilimleri sunar.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

Panolar bunun dışında kuruluşunuzda farklı sorumluluklara sahip olan kişiler için görünüm oluşturma amacıyla da kullanılabilir. Bu kişilerden bazıları:

- Finansal denetleyici
- Uygulama veya proje sahibi
- DevOps mühendisi
- Yöneticiler

Panolar, pencere öğelerinden oluşur ve her bir pencere öğesi aslında bir raporun küçük resmidir. Bir pencere öğesine tıklayarak raporunu açabilirsiniz. Raporları özelleştirdiğinizde Raporlarım'a kaydetmiş olursunuz ve bu raporlar panoya eklenir.

Yönetim (MSP), Kurumsal ve Premium Cloudyn kullanıcıları için sunulan pano sürümleri değişiklik gösterir. Farklılıklar, varlık erişim düzeylerine göre belirlenir. Erişim düzeyleri hakkında daha fazla bilgi için bkz. [Varlık erişim düzeyleri](tutorial-user-access.md#entity-access-levels).

Pano kullanılabilirliği, panoları görüntülerken kullanılan bulut hizmeti sağlayıcısı hesabının türüne bağlıdır. Cloudyn tarafından kullanılabilen ve toplanan bilgilerin türü, panolardaki raporları etkiler. Örneğin AWS hesabınız yoksa S3 İzleyicisi panosu görüntülenmez. Benzer şekilde Cloudyn için Azure Resource Manager erişimini etkinleştirmezseniz İyileştirici pano pencere öğelerinde Azure'a özgü bilgiler görmezsiniz.

Önceden oluşturulmuş panolardan herhangi birini kullanabilir veya özelleştirilmiş raporlarla kendi panonuzu oluşturabilirsiniz. Cloudyn raporları hakkında bilgi sahibi değilseniz, bkz. [Cloudyn raporlarını kullanma](use-reports.md).

## <a name="create-a-custom-dashboard"></a>Özel pano oluşturma

Özel bir panoyu hızlı bir şekilde kullanmaya başlamak için mevcut bir panoyu, özelliklerini kullanmak üzere çoğaltabilirsiniz. Ardından bunu ihtiyaçlarınıza uygun şekilde değiştirebilirsiniz. Kopyalamak istediğiniz panoda **Farklı Kaydet**'e tıklayın. Yalnızca özelleştirilmiş panoları çoğaltabilirsiniz. Cloudyn içindeki panoları çoğaltamazsınız.

Özel pano oluşturmak için:

1. Giriş sayfasında **Yeni Ekle +** öğesine tıklayın. Panom sayfası görüntülenir.  
    ![Yeni raporlar ekleyebileceğiniz Panom sayfası](./media/dashboards/my-dashboard.png)
2. **Yeni Rapor Ekle**'ye tıklayın. Rapor Ekle kutusu görüntülenir.
3. Pano pencere öğesine eklemek istediğiniz raporu seçin. Pencere öğesi panoya eklenir.
4. Yukarıdaki adımları pano tamamlanana kadar yineleyin.
5. Panonun adını değiştirmek için Pano giriş sayfasında panonun adına tıklayın ve yeni adı yazın.

## <a name="modify-a-custom-dashboard"></a>Özel panoyu değiştirme

Özel pano oluşturma işleminde olduğu gibi Cloudyn'de bulunan panoları değiştiremezsiniz. Bir özel pano raporunu değiştirmek için:

1. Panoda, değiştirmek istediğiniz raporu bulun ve **Düzenle**' ye tıklayın. Rapor görüntülenir.
2. Raporda istediğiniz değişiklikleri yapın ve **Kaydet**'e tıklayın. Rapor güncellenir ve yaptığınız değişiklikler görüntülenir.

## <a name="share-a-custom-dashboard"></a>Özel panoyu paylaşma

Özel panoyu _Genel_ veya _Varlığım_ ayarıyla başkalarıyla paylaşabilirsiniz. Genel olarak paylaştığınızda tüm kullanıcılar panoyu görüntüleyebilir. Varlığım olarak paylaştığınızda yalnızca geçerli varlığa erişimi olan kullanıcılar panoyu görüntüleyebilir. Özel bir panoyu Genel ve Varlığım ayarlarıyla paylaşma adımları benzerdir.

Özel bir panoyu Genel ayarıyla paylaşmak için:

1. Panoda **Pano Ayarları**'na tıklayın. Pano Ayarları kutusu görüntülenir.  
    ![Özel pano için pano ayarları](./media/dashboards/dashboard-options.png)
2. Pano Ayarları kutusunda ok simgesine ve ardından **Genel**'e tıklayın. Genel Pano onayı iletişim kutusu görüntülenir.
3. **Evet**'e tıklayın. Pano artık başkaları tarafından kullanılabilir.

## <a name="delete-a-custom-dashboard-report"></a>Özel pano raporunu silme

Panodaki özel rapor bileşenlerini silebilirsiniz. Panodan sildiğiniz rapor, raporlar listesinden silinmez. Sildiğiniz rapor yalnızca panodan kaldırılır.

Pano bileşenlerinden birini silmek için pano bileşeninin üzerinde **Sil**'e tıklayın. **Sil**'e tıkladığınızda pano bileşeni hemen silinir.

## <a name="share-a-dashboard-enterprise"></a>Bir panoyu paylaşma (Kurumsal)

Özel panoları kuruluşunuzdaki tüm kullanıcılarla veya geçerli varlığın kullanıcılarıyla paylaşabilirsiniz. Bir panoyu paylaşmak diğerlerine ana performans göstergelerinizin üst düzey bir görünümünü sağlamanın hızlı bir yoludur. Bir panoyu paylaştığınızda, pano otomatik olarak tüm Cloudyn varlıklarınıza/müşterilerinize çoğaltılır. Paylaşılan panodaki değişiklikler otomatik olarak güncelleştirilir.

Alt varlıklar dahil olmak üzere panoyu tüm kullanıcılarla paylaşmak için:

1. Pano giriş sayfasında **Düzenle**'ye tıklayın.
2. **Paylaş**'a tıklayıp **Genel**'i seçin.
3. Global Genel Pano onayı iletişim kutusu görüntülenir.
4. Panoyu global genel bir pano olarak ayarlamak için **Evet**'e tıklayın.

Panoyu geçerli varlığın tüm kullanıcılarıyla paylaşmak için:

1. Pano giriş sayfasında **Düzenle**'ye tıklayın.
2. **Paylaş**'a tıklayıp **Varlığım**'ı seçin.
3. Panoyu genel bir pano olarak ayarlamak için **Evet**'e tıklayın.

## <a name="duplicate-a-custom-dashboard"></a>Özel panoyu çoğaltma

Yeni bir pano oluştururken var olan bir panoyla benzer özellikleri kullanmak isteyebilirsiniz. Panoyu çoğaltarak yeni bir pano oluşturabilirsiniz.

Yalnızca özel panoları çoğaltabilirsiniz. Standart panoları çoğaltamazsınız.

Özel panoyu çoğaltmak (kopyalamak) için:

1. Çoğaltmak istediğiniz panoda **Farklı Kaydet**'e tıklayın. Aynı ada ve numaraya sahip yeni bir pano açılır.
2. Çoğaltılan panoya istediğiniz adı verin ve istediğiniz değişiklikleri yapın.

-Veya-

1. Pano Ayarları'nda çoğaltmak istediğiniz panonun satırında yer alan **Farklı Kaydet**'e tıklayın.
2. Çoğaltılan pano açılır.
3. Panoya istediğiniz adı verin ve istediğiniz değişiklikleri yapın.

## <a name="set-a-default-dashboard"></a>Varsayılan panoyu belirleme

İstediğiniz panoyu varsayılan olarak belirleyebilirsiniz. Varsayılan olarak belirlediğiniz pano, pano sekmesi listesinde en sol sekmede yer alır. Cloudyn portalını açtığınızda varsayılan pano görüntülenir.

- Varsayılan olarak belirlemek istediğiniz pano sekmesine tıklayın ve ardından sağdaki **Varsayılan**'a tıklayın.

-Veya-

1. Kullanılabilir panolar listesini görmek için **Pano Ayarları**'na tıklayın ve varsayılan olarak belirlemek istediğiniz panoyu seçin.  
    ![varsayılan pano için pano seçenekleri](./media/dashboards/dashboard-options.png)
2. Pano satırında **Varsayılan**'a tıklayın. Varsayılan Pano onay kutusu görüntülenir.
3. **Evet**'e tıklayın. Pano varsayılan olarak ayarlanır.

## <a name="management-dashboard"></a>Yönetim panosu
Yönetim panosu (veya MSP kullanıcıları için MSP panosu), ana rapor türlerinin öne çıkan özelliklerini içerir.  
![Farklı raporları gösteren yönetim panosu](./media/dashboards/management-dash.png)

### <a name="cost-entity-summary-enterprise-only"></a>Maliyet Varlığı Özeti (yalnızca Kurumsal)
Bu pencere öğesi, varlıkların sayısı ve hesap sayısı dahil olmak üzere yönetilen maliyet varlıklarını özetler.
- Kuruluş Ayrıntıları raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-over-time"></a>Zaman İçinde Maliyet
Bu pencere öğesi, maliyet eğilimlerini belirlemenize yardımcı olabilir. Son 30 günün eğilimini temel alarak, son günün maliyetini vurgular.
- Ek ayrıntıları görüntülemek ve filtrelemek için Zamana Göre Gerçek Maliyet raporunu açmak üzere pencere öğesine tıklayın.

### <a name="asset-controller"></a>Varlık Denetleyicisi
Bu pencere öğesi, önceki gündeki çalışan örneklerin sayısını, son 30 güne ait kullanım eğilimine göre vurgular.
- Varlık Denetleyicisi panosunu açmak için pencere öğesine tıklayın.

### <a name="unused-ri-detector"></a>Kullanılmayan RI Algılayıcısı
Bu pencere öğesi, kullanılmayan Amazon EC2 rezervasyonlarının sayısını vurgular.
- Şu anda kullanılmayan ve değiştirebileceğiniz rezervasyonları görüntüleyebileceğiniz Kullanılmayan Rezervasyonlar raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-by-service"></a>Hizmete Göre Maliyet
Bu pencere öğesi, son 30 gün içindeki hizmete göre amorti edilen maliyetleri vurgular. Hizmet başına maliyeti görmek için pasta grafiğinin üzerine gelin.
- Gerçek Maliyet Analizi raporunu açmak için pencere öğesine tıklayın.

### <a name="potential-savings"></a>Olası tasarruf
Bu pencere öğesi, Amazon EC2 ve Amazon RDS için örnek türü fiyatlandırma önerilerini gösterir.
- Tasarruf Analizi raporunu açmak için pencere öğesine tıklayın. Maliyetlerinizi, olası tasarruflarla birlikte örnek türlerine göre listeler.

### <a name="compute-instances---daily-trend"></a>İşlem Örnekleri - Günlük Eğilim
Bu pencere öğesi, son 30 gün için türe göre etkin örnekleri görüntüler.
- Son 30 gün içinde çalışan tüm örneklerin dökümünü görüntüleyebileceğiniz Zamana Göre Örnekler raporunu açmak için pencere öğesine tıklayın.

### <a name="storage-by-department"></a>Departmana göre depolama
Bu pencere öğesi, departmanlar tarafından kullanılan depolama hizmetlerini görüntüler. Departmana göre depolama tüketiminizi görmek için pasta grafiğinin üzerine gelin.
- S3 İzleyicisi panosunu açmak için pencere öğesine tıklayın.

## <a name="cost-controller-dashboard"></a>Maliyet Denetleyicisi panosu
Maliyet Denetleyicisi panosu, önceden ayarlanmış maliyet ayırma vurgularını gösterir.  
![Farklı raporları gösteren Maliyet Denetleyicisi panosu](./media/dashboards/cost-controller-dashboard.png)

### <a name="cost-over-time"></a>Zaman İçinde Maliyet
Bu pencere öğesi, maliyet eğilimlerini belirlemenize yardımcı olur. Son 30 günün eğilimini temel alarak, son günün maliyetini vurgular.
- Ek ayrıntıları görüntülemek ve filtrelemek için Zamana Göre Gerçek Maliyet raporunu açmak üzere pencere öğesine tıklayın.

### <a name="monthly-cost-trends"></a>Aylık Maliyet Eğilimleri
Bu pencere öğesi, ayın başlangıcından bu yana öngörülen amorti edilmiş harcamaları ve gerçek harcamalarınızı vurgular.
- Ayın başlangıcından bugüne kadar maliyet özeti sunan Geçerli Ay Öngörülen Maliyet raporunu açmak için pencere öğesine tıklayın.

Bu rapor önceki ayın başından bu yana olan maliyeti, önceki ayın maliyetini ve geçerli ayın öngörülen maliyetini gösterir. Geçerli ay için öngörülen maliyet, güncel aylık maliyet ve öngörülen maliyet toplanarak hesaplanır. Öngörülen maliyet, son 30 gün içinde izlenen maliyeti temel alır.

### <a name="12-month-planner"></a>12 Aylık Planlayıcı
Bu pencere öğesi sonraki 12 ay ve olası tasarruflar için öngörülen maliyetleri vurgular.
- Yıllık Öngörülen Maliyet raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-by-service"></a>Hizmete Göre Maliyet
Bu pencere öğesi, son 30 gün içindeki hizmete göre amorti edilen maliyetleri vurgular.
- Hizmet başına maliyeti görmek için pasta grafiğinin üzerine gelin.
- Gerçek Maliyet Analizi raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-by-account"></a>Hesaba Göre Maliyet
Bu pencere öğesi, son 30 gün içindeki hesaba göre amorti edilen maliyetleri vurgular.
- Hesap başına maliyeti görmek için pasta grafiğinin üzerine gelin.
- Gerçek Maliyet Analizi raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-trend-by-day"></a>Güne Göre Maliyet Eğilimi
Bu pencere öğesi, son 30 gün içindeki harcamayı vurgular.
- Gün başına maliyetleri görmek için çubuk grafiğinin üzerine gelin.
- Zamana Göre Gerçek Maliyet raporunu açmak için pencere öğesine tıklayın.

### <a name="cost-trend-by-month---last-6-months"></a>Aya Göre Maliyet Eğilimi - Son 6 ay

Bu pencere öğesi, son altı ay boyunca yapılan harcamayı vurgular.
- Aylık maliyetleri görmek için çubuk grafiğinin üzerine gelin.
- Zamana Göre Gerçek Maliyet raporunu açmak için pencere öğesine tıklayın.

## <a name="asset-controller-dashboard"></a>Varlık Denetleyicisi panosu

Bu panoda çalışan örnek sayısı, kullanılabilir ve kullanımda olan diskler, örnek türlerinin dağılımı ve depolama bilgileri görüntülenir.  
![Farklı raporları gösteren Varlık Denetleyicisi panosu](./media/dashboards/asset-controller-dashboard.png)

### <a name="compute-instances"></a>İşlem Örnekleri
Bu pencere öğesi, son 30 güne ait kullanım eğilimi temelinde çalışan örneklerin sayısını görüntüler.
- Zamana Göre Örnekler raporunu açmak için pencere öğesine tıklayın.

### <a name="disks"></a>Diskler
Bu pencere öğesi, kullanılmakta olan ve kullanılabilen disklerin toplam sayısını ve hacmini vurgular.
- Etkin Diskler raporunu açmak için pencere öğesine tıklayın.

### <a name="instance-type-distribution"></a>Örnek Türü Dağılımı
Bu pencere öğesi, örnek türlerini pasta grafiği ile vurgular.
- Seçili toplamaya göre etkin örneklerinizin dökümünü sağlayan Örnek Dağıtımı raporunu açmak için pencere öğesine tıklayın.

### <a name="compute-instances---daily-trend"></a>İşlem Örnekleri - Günlük Eğilim
Bu pencere öğesi, son 30 gün için günlük işlem örneklerini (spot, ayrılmış ve isteğe bağlı) vurgular.
- Günlük ve türe göre işlem örneği sayısını görüntülemek için grafiğin üzerine gelin.
- Zamana Göre Örnekler raporunu açmak için pencere öğesine tıklayın.

### <a name="all-buckets-s3"></a>Tüm Demetler (S3)
Bu pencere öğesi, toplam S3 depolama alanını ve depolanan nesne sayısını vurgular.
- S3 İzleyicisi Panosunu açmak için pencere öğesine tıklayın. Pano, geçerli depolama alanı kullanımınızı ve eğilimlerini bulmanıza, çözümlemenize ve görüntülemenize yardımcı olur.

### <a name="sql-db-instances-rds"></a>SQL Veritabanı Örnekleri (RDS)
Bu pencere öğesi, çalışan Amazon RDS örneklerinin sayısını son 30 günün eğilimi temelinde vurgular.
- Zamana Göre RDS Örneği raporunu açmak için pencere öğesine tıklayın.

## <a name="optimizer-dashboard"></a>İyileştirici Panosu
Bu pano, boyut küçültme önerilerini, kullanılmayan kaynakları ve olası tasarrufları görüntüler.  
![Farklı raporları gösteren iyileştirici panosu](./media/dashboards/optimizer-dashboard.png)

### <a name="ri-calculator"></a>RI Hesaplayıcı
Bu pencere öğesi, RI satın alma önerileri sayısını görüntüler ve olası yıllık tasarrufları vurgular.
- İsteğe bağlı ve ayrılmış fiyatlandırma planlarını ne zaman kullanacağınızı belirleyebileceğiniz Ayrılmış Örnek Hesaplayıcı'yı açmak için pencere öğesine tıklayın.

### <a name="sizing"></a>Boyutlandırma
Bu pencere öğesi, uygulanan boyutlandırmayı ve uygulanmakta olan olası tasarrufları vurgular.
- EC2 Uygun Maliyetli Boyutlandırma Önerileri raporunu açmak için pencere öğesine tıklayın.

### <a name="unused-ri-detector"></a>Kullanılmayan RI Algılayıcısı
Bu pencere öğesi, kullanılmayan Amazon EC2 rezervasyonlarının sayısını vurgular.
- Şu anda kullanılmayan ve değiştirebileceğiniz rezervasyonları görüntüleyebileceğiniz Kullanılmayan Rezervasyonlar raporunu açmak için pencere öğesine tıklayın.

###  <a name="available-disks"></a>Kullanılabilir Diskler
Bu pencere öğesi, dağıtımınızdaki eklenmemiş disklerin sayısını vurgular.
- Eklenmemiş Diskler raporunu açmak için pencere öğesine tıklayın.

### <a name="rds-ri-calculator"></a>RDS RI Hesaplayıcı
Bu pencere öğesi, Amazon RDS örnekleriniz için rezervasyon önerisi sayısını ve olası tasarrufları vurgular.
- İsteğe bağlı örnekler yerine, ayrılmış örnekleri kullanmak üzere Cloudyn önerilerini görebileceğiniz RDS RI Satın Alma Önerileri raporunu açmak için pencere öğesine tıklayın.

### <a name="rds-sizing"></a>RDS Boyutlandırma
Bu pencere öğesi, boyutlandırma önerilerinin sayısını ve olası tasarrufları gösterir.
- Ayrıntılı Amazon RDS boyutlandırma önerilerini görüntüleyen RDS Boyutlandırma Önerileri raporunu açmak için pencere öğesine tıklayın.

İyileştirme önerileri, önceki aya ait izlenen kullanım ve performans verilerini temel alır.

## <a name="s3-tracker-dashboard"></a>S3 İzleyici panosu
S3 İzleyici panosu, geçerli depolama alanı kullanımınızı ve eğilimlerini bulmanıza, çözümlemenize ve görüntülemenize yardımcı olur.  
![Farklı raporları gösteren S3 İzleyici panosu](./media/dashboards/s3-tracker-dashboard.png)

### <a name="all-buckets"></a>Tüm Demetler
Bu pencere öğesi tüm demetlerinizin, GB cinsinden toplam boyutunu ve demetlerinizdeki toplam nesne sayısını vurgular.
- S3 Boyutunun Dağılımı raporunu açmak için pencere öğesine tıklayın. Bu rapor S3 boyutunu demet, en üst düzey klasör, depolama sınıfı ve sürüm oluşturma durumuna göre analiz etmenize yardımcı olur.

### <a name="bucket-properties"></a>Demet Özellikleri
Bu pencere öğesi, toplam depolama demetlerinin sayısını vurgular.
- S3 Demet Özellikleri raporunu görüntülemek için pencere öğesine tıklayın.

### <a name="scan-status"></a>Tarama Durumu
Bu pencere öğesi, son S3 taramasının ne zaman yapıldığını ve bir sonrakinin ne zaman başlatılacağını vurgular.
- S3 Tarama Durumu raporunu görüntülemek için pencere öğesine tıklayın.

### <a name="storage-by-bucket"></a>Demete Göre Depolama
Bu pencere öğesi, her bir demet depolama sınıfının kullandığı yüzdeyi vurgular.
- S3 Boyutunun Dağılımı raporunu açmak için pencere öğesine tıklayın. Bu rapor S3 boyutunu demet, en üst düzey klasör, depolama sınıfı ve sürüm oluşturma durumuna göre analiz etmenize yardımcı olur.

### <a name="number-of-objects-by-bucket"></a>Demete Göre Nesne Sayısı
Bu pencere öğesi, gerçek sayı ve yüzde cinsinden demet başına nesne sayısını vurgular. Toplam nesne sayısını görmek için demetin üzerine gelin.
- S3 Boyutunun Dağılımı raporunu (Tarama tabanlı) açmak için pencere öğesine tıklayın.

## <a name="cloud-comparison-dashboard"></a>Bulut Karşılaştırma Panosu
Bulut Karşılaştırma Panosu, farklı bulut sağlayıcılarından alınan maliyetleri fiyatlandırma, CPU türü ve RAM boyutuna göre karşılaştırmanıza yardımcı olur.  
![Farklı raporları gösteren Bulut Karşılaştırma panosu](./media/dashboards/cloud-comparison-dashboard.png)

### <a name="ec2-cost-in-azure-by-instance-type"></a>Örnek Türüne Göre Azure'da EC2 Maliyeti
Bu pencere öğesi, isteğe bağlı fiyatlar üzerinden son 30 günün kullanımını vurgular. Geçerli Amazon EC2 maliyetleriyle Azure'daki olası maliyetleri karşılaştırır.
- Örnek türü başına maliyetleri karşılaştırmak için çubukların üzerine gelin.
- Dağıtımınızı Taşıma - Maliyet Analizi raporunu açmak için pencere öğesine tıklayın.

### <a name="ec2-cost-in-azure"></a>Azure'da EC2 Maliyeti
Bu pencere öğesi, geçerli Amazon EC2 maliyetlerinizi gösterir ve bunları Azure ile karşılaştırır. Karşılaştırma, son 30 günlük kullanım için isteğe bağlı fiyatları temel alır.
- Dağıtımınızı Taşıma - Maliyet Analizi raporunu açmak için pencere öğesine tıklayın.

### <a name="ec2azure-instance-type-mapping"></a>EC2/Azure Örnek Türü Eşleştirme
Bu pencere öğesi, Amazon EC2 ve Azure arasında en iyi elastik işlem birimi eşleşmesini vurgular.
- Örnek Türü Eşleme raporunu açmak için pencere öğesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Raporlar hakkında daha fazla bilgi edinmek için [Cloudyn raporlarını kullanma](use-reports.md) makalesini okuyun.
