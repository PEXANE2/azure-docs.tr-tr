---
title: Fiyatlandırma katmanları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki bilgi işlem ve depolama seçenekleri açıklanmaktadır.
author: jasonwhowell
ms.author: jasonh
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 1e4e69b63e51bafe8ca0b032c22ca509f5a7e6a2
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770579"
---
# <a name="pricing-tiers-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı fiyatlandırma katmanları - Tek Sunucu

Temel, Genel Amaç ve Bellek Optimize Edilmiş üç farklı fiyatlandırma katmanından birinde PostgreSQL sunucusu için bir Azure Veritabanı oluşturabilirsiniz. Fiyatlandırma katmanları, vCore'larda sağlanabilir bilgi işlem miktarı, vCore başına bellek ve verileri depolamak için kullanılan depolama teknolojisiyle ayırt edilir. Tüm kaynaklar PostgreSQL sunucu düzeyinde sağlanmaktadır. Bir sunucunun bir veya birkaç veritabanı olabilir.

|    | **Temel** | **Genel Amaç** | **Bellek Optimize** |
|:---|:----------|:--------------------|:---------------------|
| İşlem oluşturma | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| vCore başına bellek | 2 GB | 5 GB | 10 GB |
| Depolama boyutu | 5 GB - 1 TB | 5 GB - 16 TB | 5 GB - 16 TB |
| Veritabanı yedekleme bekletme süresi | 7 ila 35 gün | 7 ila 35 gün | 7 ila 35 gün |

Bir fiyatlandırma katmanı seçmek için, başlangıç noktası olarak aşağıdaki tabloyu kullanın.

| Fiyatlandırma katmanı | Hedef iş yükleri |
|:-------------|:-----------------|
| Temel | Hafif hesaplama ve G/Ç performansı gerektiren iş yükleri. Örnekler geliştirme veya sınama veya seyrek kullanılan küçük ölçekli uygulamalar için kullanılan sunucular içerir. |
| Genel Amaçlı | Ölçeklenebilir G/Ç iş yeri ile dengeli hesaplama ve bellek gerektiren çoğu iş yükü. Örnekler arasında web ve mobil uygulamaları barındırma sunucuları ve diğer kurumsal uygulamalar yer almaktadır.|
| Bellek İçin İyileştirilmiş | Daha hızlı işlem işleme ve daha yüksek eşzamanlılık için bellek içi performans gerektiren yüksek performanslı veritabanı iş yükleri. Örnekler arasında gerçek zamanlı verileri ve yüksek performanslı işlemsel veya analitik uygulamaları işlemek için sunucular verilebilir.|

Bir sunucu oluşturduktan sonra, vCores sayısı, donanım oluşturma ve fiyatlandırma katmanı (ve Temel hariç) saniye içinde yukarı veya aşağı değiştirilebilir. Ayrıca, uygulama kapalı kalma süresi olmadan depolama miktarını ve yedekleme bekletme süresini yukarı veya aşağı bağımsız olarak ayarlayabilirsiniz. Bir sunucu oluşturulduktan sonra yedek depolama türünü değiştiremezsiniz. Daha fazla bilgi [için, Ölçek kaynakları](#scale-resources) bölümüne bakın.

## <a name="compute-generations-and-vcores"></a>Hesaplama nesiller ve vCores

Bilgi işlem kaynakları, temel donanımın mantıksal CPU'sunu temsil eden vCores olarak sağlanır. Çin Doğu 1, Çin Kuzey 1, ABD DoD Central ve ABD DoD Doğu Intel E5-2673 v3 (Haswell) 2.4-GHz işlemciler dayalı Gen 4 mantıksal CPU'lar kullanır. Diğer tüm bölgeler Intel E5-2673 v4 (Broadwell) 2.3-GHz işlemcilere dayanan Gen 5 mantıksal CPU'larını kullanır.

## <a name="storage"></a>Depolama

Sağlama yaptığınız depolama alanı, PostgreSQL sunucusu için Azure Veritabanınızın kullanabileceği depolama kapasitesi miktarıdır. Depolama veritabanı dosyaları, geçici dosyalar, işlem günlükleri ve PostgreSQL sunucu günlükleri için kullanılır. Hükmettiğiniz toplam depolama alanı miktarı, sunucunuz için kullanılabilen G/Ç kapasitesini de tanımlar.

|    | **Temel** | **Genel Amaç** | **Bellek Optimize** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Temel Depolama | Genel Amaçlı Depolama | Genel Amaçlı Depolama |
| Depolama boyutu | 5 GB - 1 TB | 5 GB - 16 TB | 5 GB - 16 TB |
| Depolama artış boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>Min 100 IOPS<br/>Maksimum 20.000 IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>Maksimum 20.000 IOPS |

> [!NOTE]
> 16TB ve 20.000 IOPS'ye kadar depolama aşağıdaki bölgelerde desteklenir: Doğu ABD, Doğu ABD 2, Orta ABD, Batı ABD, Kuzey Orta ABD, Güney Orta ABD, Kuzey Avrupa, Batı Avrupa, İngiltere Güney, İngiltere Batı, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Avustralya Doğu, Avustralya Güney Doğu.
>
> Diğer tüm bölgeler 4 TB'a kadar depolama ve 6000 IOPS'yi destekler.
>

Sunucunun oluşturulması sırasında ve sonrasında ek depolama kapasitesi ekleyebilir ve sistemin iş yükünüzün depolama tüketimine bağlı olarak depolama alanını otomatik olarak büyütmesine izin verebilirsiniz. 

>[!NOTE]
> Depolama yalnızca büyütülebilir, küçültülemez.

Temel katman IOPS garantisi sağlamaz. Genel Amaç ve Bellek Optimize edilmiş fiyatlandırma katmanlarında, IOPS 3:1 oranında sağlanan depolama boyutuyla ölçeklendirilir.

G/Ç tüketiminizi Azure portalında veya Azure CLI komutlarını kullanarak izleyebilirsiniz. İzlenecek ilgili ölçümler [depolama sınırı, depolama yüzdesi, kullanılan depolama yüzdesi ve IO yüzdesidir.](concepts-monitoring.md)

### <a name="reaching-the-storage-limit"></a>Depolama sınırına ulaşma

100 GB'dan daha az tahsis edilmiş depolama alanına sahip sunucular, ücretsiz depolama alanı 512MB'dan az veya sağlanan depolama boyutunun %5'inden azsa salt okunur olarak işaretlenir. Sağlanan depolama alanı 100 GB'tan fazla olan sunucular, boş depolama alanı 5 GB'ın altına düştüğünde salt okunur olarak işaretlenir.

Örneğin, 110 GB depolama alanı sağlamanız varsa ve gerçek kullanım 105 GB'ın üzerine çıktıysa, sunucu salt okunur olarak işaretlenir. Alternatif olarak, 5 GB depolama alanı sağlamışsanız, ücretsiz depolama alanı 512 MB'dan az olduğunda sunucu salt okunur olarak işaretlenir.

Sunucu salt okunur olarak ayarlandığında, varolan tüm oturumların bağlantısı kesilir ve kaydedilmemiş hareketler geri alınır. Sonraki tüm yazma işlemleri ve işlem taahhütleri başarısız oldu. Sonraki tüm okuma sorguları kesintisiz çalışacaktır.  

Sunucunuza sağlanan depolama miktarını artırabilir veya okuma yazma modunda yeni bir oturum başlatabilir ve ücretsiz depolama yı geri almak için veri bırakabilirsiniz. Running, `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` geçerli oturumu yazma modunu okumak için ayarlar. Veri bozulmasını önlemek için, sunucu hala salt okunur durumdayken yazma işlemleri gerçekleştirmeyin.

Depolama otomatik büyümesini açmanızı veya sunucu depolamanız eşiğe yaklaşırken sizi bilgilendirmek için bir uyarı ayarlamanızı öneririz, böylece salt okunur duruma girmekten kaçınabilirsiniz. Daha fazla bilgi için, [bir uyarının nasıl ayarlanabildiğini](howto-alert-on-metric.md)gösteren belgelere bakın.

### <a name="storage-auto-grow"></a>Depolama otomatik büyümek

Depolama otomatik olarak büyür, sunucunuzun depolama alanının tükenmesini ve salt okunur hale gelmesini önler. Depolama otomatik büyümesi etkinse, depolama iş yükünü etkilemeden otomatik olarak büyür. 100 GB'dan daha az depoya sahip sunucular için, ücretsiz depolama alanı 1 GB'ın altında veya sağlanan depolama alanının %10'unun altında olduğu anda sağlanan depolama boyutu 5 GB artırılır. 100 GB'dan fazla depo ya da 100 GB'dan fazla depolama alanına sahip sunucular için, boş depolama alanı 10 GB'ın altında veya sağlanan depolama boyutunun %5'inin altında olduğunda sağlanan depolama boyutu %5 artırılır. Yukarıda belirtildiği gibi maksimum depolama sınırları geçerlidir.

Örneğin, 1000 GB depolama alanı sağlamanız varsa ve gerçek kullanım 950 GB'ın üzerine çıkarsa, sunucu depolama boyutu 1050 GB'a yükseltilir. Alternatif olarak, 10 GB depolama alanı saysanız, 1 GB'dan az depolama alanı olmadığında depolama boyutu 15 GB'a yükseltilir.

Depolamanın küçültülmeyeceğini unutmayın.

## <a name="backup"></a>Backup

Hizmet otomatik olarak sunucunuzun yedeklerini alır. Bir bekletme süresi 7 ila 35 gün aralığından seçebilirsiniz. Genel Amaç ve Bellek Optimize Edilmiş sunucular yedeklemeler için coğrafi yedekli depolama alanına sahip olmayı seçebilir. [Kavramlar makalesinde](concepts-backup.md)yedeklemeler hakkında daha fazla bilgi edinin.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuzu oluşturduktan sonra, vCore'ları, donanım oluşturmayı, fiyatlandırma katmanını (Basic'e ve Temel'den gelene, depolama miktarını ve yedekleme bekletme süresini) bağımsız olarak değiştirebilirsiniz. Bir sunucu oluşturulduktan sonra yedek depolama türünü değiştiremezsiniz. vCores sayısı yukarı veya aşağı ölçeklendirilebilir. Yedekleme bekletme süresi 7 günden 35 güne kadar büyütülebilir veya küçültülmüş olabilir. Depolama boyutu yalnızca artırılabilir. Kaynakların ölçeklemesi portal veya Azure CLI üzerinden yapılabilir. Azure CLI kullanarak ölçeklendirme örneği için [bkz.](scripts/sample-scale-server-up-or-down.md)

> [!NOTE] 
> Depolama boyutu yalnızca artırılabilir. Artıştan sonra daha küçük bir depolama boyutuna geri dönemezsiniz.

VCores, donanım oluşturma veya fiyatlandırma katmanı sayısını değiştirdiğinizde, özgün sunucunun bir kopyası yeni bilgi işlem tahsisi ile oluşturulur. Yeni sunucu çalışır duruma geçtikten sonra, bağlantılar yeni sunucuya geçer. Sistem yeni sunucuya geçerken yeni bağlantı kurulamaz ve tüm işlenmemiş işlemler geri alınır. Bu süre değişir, ancak çoğu durumda bir dakikadan daha kısadır.

Depolama alanını ölçekleme ve yedekleme bekletme süresini değiştirme gerçek çevrimiçi işlemlerdir. Kapalı kalma süresi yoktur ve uygulamanız etkilenmez. IOPS, sağlanan depolama alanının boyutuyla ölçeklendirildikçe, depolama alanını ölçeklendirerek sunucunuzda bulunan IOPS'yi artırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için hizmet fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/PostgreSQL/)bakın. Azure [portalı,](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) istediğiniz yapılandırmanın maliyetini görmek için seçtiğiniz seçeneklere bağlı olarak **Fiyatlandırma katmanı** sekmesinde aylık maliyeti gösterir. Azure aboneliğiniz yoksa, tahmini bir fiyat elde etmek için Azure fiyatlandırma hesaplayıcısını kullanabilirsiniz. Azure [fiyatlandırma hesap makinesi](https://azure.microsoft.com/pricing/calculator/) web **sitesinde, öğeleri ekle'yi**seçin, **Veritabanları** kategorisini genişletin ve seçenekleri özelleştirmek **için PostgreSQL için Azure Veritabanı'nı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Portalda nasıl [bir PostgreSQL sunucusu oluşturabilirsiniz](tutorial-design-database-using-azure-portal.md)öğrenin.
- Hizmet [sınırları](concepts-limits.md)hakkında bilgi edinin. 
- Okuma yinelemeleri ile nasıl [ölçeklendirilen öğrenin.](howto-read-replicas-portal.md)
