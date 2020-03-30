---
title: Fiyatlandırma katmanları - MySQL için Azure Veritabanı
description: Bilgi işlem nesilleri, depolama türleri, depolama boyutu, vCore'lar, bellek ve yedekleme bekletme süreleri de dahil olmak üzere MySQL için Azure Veritabanı için çeşitli fiyatlandırma katmanları hakkında bilgi edinin.
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: cf959112a2a717da1005be062dda3b83163d8d6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269451"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>MySQL fiyatlandırma katmanları için Azure Veritabanı

MySQL sunucusu için üç farklı fiyatlandırma katmanından birinde bir Azure Veritabanı oluşturabilirsiniz: Temel, Genel Amaç ve Bellek Optimize Edilmiştir. Fiyatlandırma katmanları, vCore'larda sağlanabilir bilgi işlem miktarı, vCore başına bellek ve verileri depolamak için kullanılan depolama teknolojisiyle ayırt edilir. Tüm kaynaklar MySQL sunucu düzeyinde sağlanmaktadır. Bir sunucunun bir veya birkaç veritabanı olabilir.

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

Sağlama yaptığınız depolama, MySQL sunucusu için Azure Veritabanınızın kullanabileceği depolama kapasitesi miktarıdır. Depolama veritabanı dosyaları, geçici dosyalar, işlem günlükleri ve MySQL sunucu günlükleri için kullanılır. Hükmettiğiniz toplam depolama alanı miktarı, sunucunuz için kullanılabilen G/Ç kapasitesini de tanımlar.

|    | **Temel** | **Genel Amaç** | **Bellek Optimize** |
|:---|:----------|:--------------------|:---------------------|
| Depolama türü | Temel Depolama | Genel Amaçlı Depolama | Genel Amaçlı Depolama |
| Depolama boyutu | 5 GB - 1 TB | 5 GB - 16 TB | 5 GB - 16 TB |
| Depolama artış boyutu | 1 GB | 1 GB | 1 GB |
| IOPS | Değişken |3 IOPS/GB<br/>Min 100 IOPS<br/>Maksimum 20.000 IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>Maksimum 20.000 IOPS |

> [!NOTE]
> 16TB ve 20.000 IOPS'ye kadar depolama aşağıdaki bölgelerde desteklenir: Doğu ABD, Doğu ABD 2, Orta ABD, Batı ABD, Kuzey Orta ABD, Güney Orta ABD, Kuzey Avrupa, Batı Avrupa, İngiltere Güney, İngiltere Batı, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Batı, Kore Orta , Kore Güney, Avustralya Doğu, Avustralya Güney Doğu.
>
> Diğer tüm bölgeler 4 TB'a kadar depolama ve 6000 IOPS'ye kadar desteği sağlar.
>

Sunucunun oluşturulması sırasında ve sonrasında ek depolama kapasitesi ekleyebilir ve sistemin iş yükünüzün depolama tüketimine bağlı olarak depolama alanını otomatik olarak büyütmesine izin verebilirsiniz. 

>[!NOTE]
> Depolama yalnızca büyütülebilir, küçültülemez.

Temel katman IOPS garantisi sağlamaz. Genel Amaç ve Bellek Optimize edilmiş fiyatlandırma katmanlarında, IOPS 3:1 oranında sağlanan depolama boyutuyla ölçeklendirilir.

G/Ç tüketiminizi Azure portalında veya Azure CLI komutlarını kullanarak izleyebilirsiniz. İzlenecek ilgili ölçümler [depolama sınırı, depolama yüzdesi, kullanılan depolama yüzdesi ve IO yüzdesidir.](concepts-monitoring.md)

### <a name="reaching-the-storage-limit"></a>Depolama sınırına ulaşma

100 GB'dan daha az tahsis edilmiş depolama alanına sahip sunucular, ücretsiz depolama alanı sağlanan depolama boyutunun %5'inden azsa salt okunur olarak işaretlenir. Sağlanan depolama alanı 100 GB'tan fazla olan sunucular, boş depolama alanı 5 GB'ın altına düştüğünde salt okunur olarak işaretlenir.

Örneğin, 110 GB depolama alanı sağlamanız varsa ve gerçek kullanım 105 GB'ın üzerine çıktıysa, sunucu salt okunur olarak işaretlenir. Alternatif olarak, 5 GB depolama alanı sağlamışsanız, ücretsiz depolama alanı 256 MB'dan az olduğunda sunucu salt okunur olarak işaretlenir.

Hizmet sunucuyu salt okunur duruma getirdiğinde tüm yeni yazma işlemi istekleri engellenir ve var olan etkin işlemler yürütülmeye devam eder. Sunucu salt okunur olarak ayarlandığında sonraki tüm yazma girişimleri ve işlemler başarısız olur. Okuma sorguları kesintisiz olarak çalışmaya devam eder. Sağlanan depolama alanını artırdıktan sonra sunucu yazma işlemlerini kabul etmeye hazır hale gelir.

Depolama otomatik büyümesini açmanızı veya sunucu depolamanız eşiğe yaklaşırken sizi bilgilendirmek için bir uyarı ayarlamanızı öneririz, böylece salt okunur duruma girmekten kaçınabilirsiniz. Daha fazla bilgi için, [bir uyarının nasıl ayarlanabildiğini](howto-alert-on-metric.md)gösteren belgelere bakın.

### <a name="storage-auto-grow"></a>Depolama otomatik büyümek

Depolama otomatik olarak büyür, sunucunuzun depolama alanının tükenmesini ve salt okunur hale gelmesini önler. Depolama otomatik büyümesi etkinse, depolama iş yükünü etkilemeden otomatik olarak büyür. 100 GB'dan daha az depoya sahip sunucular için, sağlanan depolama alanı, sağlanan depolama alanının %10'unun altında olduğunda 5 GB artırılır. 100 GB'dan fazla depoya sahip sunucular için, boş depolama alanı sağlanan depolama boyutunun 10 GB'ının altında olduğunda, sağlanan depolama boyutu %5 artırılır. Yukarıda belirtildiği gibi maksimum depolama sınırları geçerlidir.

Örneğin, 1000 GB depolama alanı sağlamanız varsa ve gerçek kullanım 990 GB'ın üzerine çıkarsa, sunucu depolama boyutu 1050 GB'a yükseltilir. Alternatif olarak, 10 GB depolama alanı saysanız, 1 GB'dan az depolama alanı olmadığında depolama boyutu 15 GB'a yükseltilir.

Depolamanın küçültülmeyeceğini unutmayın.

## <a name="backup"></a>Backup

Hizmet otomatik olarak sunucunuzun yedeklerini alır. Bir bekletme süresi 7 ila 35 gün aralığından seçebilirsiniz. Genel Amaç ve Bellek Optimize Edilmiş sunucular yedeklemeler için coğrafi yedekli depolama alanına sahip olmayı seçebilir. [Kavramlar makalesinde](concepts-backup.md)yedeklemeler hakkında daha fazla bilgi edinin.

## <a name="scale-resources"></a>Kaynakları ölçeklendirme

Sunucunuzu oluşturduktan sonra, vCore'ları, donanım oluşturmayı, fiyatlandırma katmanını (Basic'e ve Temel'den gelene, depolama miktarını ve yedekleme bekletme süresini) bağımsız olarak değiştirebilirsiniz. Bir sunucu oluşturulduktan sonra yedek depolama türünü değiştiremezsiniz. vCores sayısı yukarı veya aşağı ölçeklendirilebilir. Yedekleme bekletme süresi 7 günden 35 güne kadar büyütülebilir veya küçültülmüş olabilir. Depolama boyutu yalnızca artırılabilir. Kaynakların ölçeklemesi portal veya Azure CLI üzerinden yapılabilir. Azure CLI kullanarak ölçeklendirme örneği için [bkz.](scripts/sample-scale-server.md)

VCores, donanım oluşturma veya fiyatlandırma katmanı sayısını değiştirdiğinizde, özgün sunucunun bir kopyası yeni bilgi işlem tahsisi ile oluşturulur. Yeni sunucu çalışır duruma geçtikten sonra, bağlantılar yeni sunucuya geçer. Sistem yeni sunucuya geçerken yeni bağlantı kurulamaz ve tüm işlenmemiş işlemler geri alınır. Bu süre değişir, ancak çoğu durumda bir dakikadan daha kısadır.

Depolama alanını ölçekleme ve yedekleme bekletme süresini değiştirme gerçek çevrimiçi işlemlerdir. Kapalı kalma süresi yoktur ve uygulamanız etkilenmez. IOPS, sağlanan depolama alanının boyutuyla ölçeklendirildikçe, depolama alanını ölçeklendirerek sunucunuzda bulunan IOPS'yi artırabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

En güncel fiyatlandırma bilgileri için hizmet fiyatlandırma [sayfasına](https://azure.microsoft.com/pricing/details/mysql/)bakın. Azure [portalı,](https://portal.azure.com/#create/Microsoft.MySQLServer) istediğiniz yapılandırmanın maliyetini görmek için seçtiğiniz seçeneklere bağlı olarak **Fiyatlandırma katmanı** sekmesinde aylık maliyeti gösterir. Azure aboneliğiniz yoksa, tahmini bir fiyat elde etmek için Azure fiyatlandırma hesaplayıcısını kullanabilirsiniz. Azure [fiyatlandırma hesap makinesi](https://azure.microsoft.com/pricing/calculator/) web **sitesinde, öğeleri**ekle'yi, **Veritabanları** kategorisini genişletin ve seçenekleri özelleştirmek **için MySQL için Azure Veritabanı'nı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Portalda nasıl [bir MySQL sunucusu oluşturabilirsiniz](howto-create-manage-server-portal.md)öğrenin.
- Hizmet [sınırları](concepts-limits.md)hakkında bilgi edinin.
- Okuma yinelemeleri ile nasıl [ölçeklendirilen öğrenin.](howto-read-replicas-portal.md)
