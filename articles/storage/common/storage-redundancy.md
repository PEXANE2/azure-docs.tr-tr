---
title: Veri yedekliği
titleSuffix: Azure Storage
description: Microsoft Azure Depolama hesabınızdaki veriler dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Artıklık yapılandırmaları arasında yerel olarak yedekdepolama (LRS), bölge yedekli depolama (ZRS), coğrafi yedekli depolama (GRS), okuma-erişim coğrafi yedekli depolama (RA-GRS), coğrafi bölge yedekli depolama (GZRS) (önizleme) ve okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) (önizleme) bulunmaktadır.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 78f7c935e64276e7f4862dad966b99bff6bd246d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481934"
---
# <a name="azure-storage-redundancy"></a>Azure Depolama artıklığı

Azure Depolama, geçici donanım arızaları, ağ veya elektrik kesintileri ve büyük doğal afetler de dahil olmak üzere planlanmış ve planlanmamış olaylara karşı korunacak şekilde verilerinizin birden çok kopyasını her zaman depolar. Artıklık, depolama hesabınızın hatalar karşısında bile [Azure Depolama için Hizmet Düzeyi Sözleşmesi'ni (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) karşılamasını sağlar.

Senaryonuz için hangi artıklık seçeneğinin en iyi olduğuna karar verirken, daha düşük maliyetler ve daha yüksek kullanılabilirlik ve dayanıklılık arasındaki dengeleri göz önünde bulundurun. Hangi artıklık seçeneğini seçmenize yardımcı olan etkenler şunlardır:  

- Verileriniz birincil bölgede nasıl çoğaltılır?
- Bölgesel afetlere karşı korunmak için verilerinizin coğrafi olarak birincil bölgeye uzak ikinci bir konuma çoğaltılıp çoğaltılmadığı
- Birincil bölge herhangi bir nedenle kullanılamıyorsa, uygulamanızın ikincil bölgedeki çoğaltılan verilere okuma erişimi gerektirenden gerek ip olmadığı

## <a name="redundancy-in-the-primary-region"></a>Birincil bölgede fazlalık

Azure Depolama hesabındaki veriler birincil bölgede her zaman üç kez çoğaltılır. Azure Depolama, verilerinizin birincil bölgede nasıl çoğaltıldılabilen iki seçenek sunar:

- **Yerel olarak yedekli depolama (LRS),** verilerinizi birincil bölgedeki tek bir fiziksel konum içinde eşzamanlı olarak üç kez kopyalar. LRS en ucuz çoğaltma seçeneğidir, ancak yüksek kullanılabilirlik gerektiren uygulamalar için önerilmez.
- **Bölge yedekli depolama (ZRS),** verilerinizi birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak kopyalar. Yüksek kullanılabilirlik gerektiren uygulamalar için Microsoft, birincil bölgede ZRS kullanılmasını ve ikincil bir bölgeye çoğaltılmasını önerir.

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama

Yerel olarak yedekli depolama (LRS), verilerinizi birincil bölgedeki tek bir fiziksel konum içinde üç kez çoğaltır. LRS, belirli bir yıl içinde nesnelerin en az %99,999999999999% (11 dokuz) dayanıklılığını sağlar.

LRS en düşük maliyetli artıklık seçeneğidir ve diğer seçeneklere göre en az dayanıklılık sunar. LRS verilerinizi sunucu rafına ve sürücü arızalarına karşı korur. Ancak, veri merkezi içinde yangın veya su baskını gibi bir felaket meydana gelirse, LRS kullanan bir depolama hesabının tüm kopyaları kaybolabilir veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft, [bölge yedekli depolama](#zone-redundant-storage) (ZRS), [coğrafi yedekli depolama](#geo-redundant-storage) (GRS) veya coğrafi bölge [yedekli depolama (önizleme) (GZRS)](#geo-zone-redundant-storage-preview) kullanmanızı önerir.

LRS kullanan bir depolama hesabına yazma isteği eşzamanlı olarak gerçekleşir. Yazma işlemi, yalnızca veriler üç yinelemeye de yazıldıktan sonra başarıyla döndürür.

LRS aşağıdaki senaryolar için iyi bir seçimdir:

- Uygulamanız, veri kaybı oluşursa kolayca yeniden oluşturulabilecek verileri depolarsa, LRS'yi tercih edebilirsiniz.
- Uygulamanız, veri yönetimi gereksinimleri nedeniyle yalnızca bir ülke veya bölge içinde veri çoğaltma ile sınırlıysa, LRS'yi tercih edebilirsiniz. Bazı durumlarda, verilerin coğrafi olarak çoğaltıldığı eşleştirilmiş bölgeler başka bir ülke veya bölgede olabilir. Eşlenen bölgeler hakkında daha fazla bilgi için [Azure bölgelerine](https://azure.microsoft.com/regions/)bakın.

### <a name="zone-redundant-storage"></a>Alanlar arası yedekli depolama

Bölge yedekli depolama (ZRS), Azure Depolama verilerinizi birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak çoğaltır. Her kullanılabilirlik bölgesi, bağımsız güç, soğutma ve ağ ile ayrı bir fiziksel konumdur. ZRS, belirli bir yıl içinde en az %99,99999999999 (12 9's) Azure Depolama veri nesneleri için dayanıklılık sunar.

ZRS ile, bölge kullanılamamış olsa bile verileriniz hem okuma hem de yazma işlemleri için hala erişilebilir. Bir bölge kullanılamaz hale gelirse, Azure DNS yeniden işaretleme gibi ağ güncelleştirmelerini üstlenir. Güncelleştirmeler tamamlanmadan önce verilere erişirseniz, bu güncelleştirmeler uygulamanızı etkileyebilir. ZRS uygulamaları tasarlarken, üstel geri leme ile yeniden deneme ilkelerini uygulamak da dahil olmak üzere geçici hata işleme uygulamalarını izleyin.

ZRS kullanan bir depolama hesabına yazma isteği eşzamanlı olarak gerçekleşir. Yazma işlemi, yalnızca veriler üç kullanılabilirlik bölgesinde ki tüm yinelemelere yazıldıktan sonra başarıyla döndürür.

Microsoft, tutarlılık, dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için birincil bölgede ZRS kullanılmasını önerir. ZRS, geçici olarak kullanılamaması durumunda mükemmel performans, düşük gecikme ve verileriniz için esneklik sağlar. Ancak, ZRS tek başına verilerinizi birden çok bölge kalıcı olarak etkilenen bölgesel bir felakete karşı koruyamaz. Microsoft, bölgesel felaketlere karşı korunmak için, birincil bölgede ZRS kullanan ve verilerinizi ikincil bir bölgeye coğrafi olarak kopyalayan [jeo-bölge yedekli depolama](#geo-zone-redundant-storage-preview) (GZRS) kullanmanızı önerir.

Aşağıdaki tablo, hangi depolama hesabı türlerinin hangi bölgelerde ZRS'yi desteklediğini gösterir:

|    Depolama hesabı türü    |    Desteklenen bölgeler    |    Desteklenen hizmetler    |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
|    Genel amaçlı v2<sup>1</sup>    | Güneydoğu Asya<br /> Doğu Avustralya<br /> Kuzey Avrupa<br />  Batı Avrupa<br /> Orta Fransa<br /> Doğu Japonya<br /> Güney Afrika Kuzey<br /> Güney Birleşik Krallık<br /> ABD Orta<br /> ABD Doğu<br /> ABD Doğu 2<br /> ABD Batı 2    |    Blok blobları<br /> Sayfa lekeleri<sup>2</sup><br /> Dosya paylaşımları (standart)<br /> Tablolar<br /> Kuyruklar<br /> |
|    BlockBlobStorage<sup>1</sup>    | Batı Avrupa<br /> ABD Doğu    |    Yalnızca blobs blok    |
|    Dosya Depolama    | Batı Avrupa<br /> ABD Doğu    |    Yalnızca Azure Dosyaları    |

<sup>1</sup> Arşiv katmanı şu anda ZRS hesapları için desteklenmez.<br />
<sup>2</sup> Sanal makineler için Azure yönetilen diskler içeren depolama hesapları her zaman LRS kullanır. Azure yönetilmeyen diskler de LRS kullanmalıdır. GRS kullanan Azure yönetilmeyen diskler için bir depolama hesabı oluşturmak mümkündür, ancak eşzamanlı coğrafi çoğaltma üzerinde tutarlılık içeren olası sorunlar nedeniyle önerilmez. Yönetilen veya yönetilmeyen diskler ZRS veya GZRS'yi desteklemez. Yönetilen diskler hakkında daha fazla bilgi için Azure [yönetilen diskler için Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/managed-disks/)bakın.

Hangi bölgelerin ZRS'yi desteklediği hakkında bilgi için [Azure Kullanılabilirlik Bölgeleri nelerdir?'de](../../availability-zones/az-overview.md) **bölgelere göre Hizmetler desteğine** bakın.

## <a name="redundancy-in-a-secondary-region"></a>İkincil bir bölgede fazlalık

Yüksek kullanılabilirlik gerektiren uygulamalarda, depolama hesabınızdaki verileri birincil bölgeden yüzlerce mil uzaktaki ikincil bir bölgeye kopyalamayı seçebilirsiniz. Depolama hesabınız ikincil bir bölgeye kopyalanırsa, tam bir bölgesel kesinti veya birincil bölgenin kurtarılamayan bir felaket durumunda bile verileriniz dayanıklıdır.

Bir depolama hesabı oluşturduğunuzda, hesabın birincil bölgesini seçersiniz. Eşleştirilmiş ikincil bölge birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında daha fazla bilgi için Azure [bölgelerine](https://azure.microsoft.com/global-infrastructure/regions/)bakın.

Azure Depolama, verilerinizi ikincil bir bölgeye kopyalamak için iki seçenek sunar:

- **Coğrafi yedekli depolama (GRS),** LRS kullanarak verilerinizi birincil bölgedeki tek bir fiziksel konum içinde eşzamanlı olarak üç kez kopyalar. Ardından verilerinizi eşzamanlı olarak ikincil bölgedeki tek bir fiziksel konuma kopyalar.
- **Coğrafi bölge yedekli depolama (GZRS) (önizleme), ZRS** kullanarak birincil bölgedeki üç Azure kullanılabilirlik bölgesinde verilerinizi eşzamanlı olarak kopyalar. Ardından verilerinizi eşzamanlı olarak ikincil bölgedeki tek bir fiziksel konuma kopyalar.

GRS ve GZRS arasındaki temel fark, verilerin birincil bölgede nasıl çoğaltıldığA başladığıdır. İkincil konum içinde, veriler her zaman LRS kullanılarak eşzamanlı olarak üç kez çoğaltılır.

GRS veya GZRS ile, ikincil konumdaki veriler, ikincil bölgeye bir hata olmadığı sürece okuma veya yazma erişimi için kullanılamaz. İkincil konuma okuma erişimi için, okuma-erişim coğrafi yedekli depolama (RA-GRS) veya okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) kullanmak için depolama hesabınızı yapılandırın. Daha fazla bilgi için bkz: [İkinci bölgedeki verilere erişimi oku.](#read-access-to-data-in-the-secondary-region)

Birincil bölge kullanılamıyorsa, ikincil bölgeye (önizleme) başarısız olmayı seçebilirsiniz. Başarısızlık tamamlandıktan sonra, ikincil bölge birincil bölge olur ve verileri yeniden okuyup yazabilirsiniz. Olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek ve ikincil bölgeye nasıl başarısız olunsağı öğreneceğiz için [bkz.](storage-disaster-recovery-guidance.md)

> [!IMPORTANT]
> Veriler eşzamanlı olarak ikincil bölgeye çoğaltıldığından, birincil bölge kurtarılamazsa birincil bölgeyi etkileyen bir hata veri kaybına neden olabilir. Birincil bölgeye en son yazma ile ikincil bölgeye son yazma arasındaki aralık, kurtarma noktası hedefi (RPO) olarak bilinir. RPO, verilerin kurtarılabildiği noktayı gösterir. Azure Depolama'da genellikle 15 dakikadan az bir RPO vardır, ancak şu anda ikincil bölgeye veri çoğaltmanın ne kadar süreceği konusunda SLA yoktur.

### <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Coğrafi yedekli depolama (GRS), LRS kullanarak verilerinizi birincil bölgedeki tek bir fiziksel konum içinde eşzamanlı olarak üç kez kopyalar. Ardından, verilerinizi birincil bölgeden yüzlerce mil uzaktaki ikincil bir bölgedeki tek bir fiziksel konuma eşit olarak kopyalar. GRS, belirli bir yıl içinde en az %99,99999999999999999999999999999999999999999999999999999'luk Azure Depolama veri nesneleri için dayanıklılık sunar.

Bir yazma işlemi ilk olarak birincil konuma adanır ve LRS kullanılarak çoğaltılır. Güncelleştirme daha sonra ikincil bölgeye eşit olarak çoğaltılır. Veriler ikincil konuma yazıldığında, lrs kullanılarak bu konumda da çoğaltılır.

### <a name="geo-zone-redundant-storage-preview"></a>Coğrafi bölge yedekli depolama (önizleme)

Coğrafi bölge yedekli depolama (GZRS) (önizleme), kullanılabilirlik bölgeleri arasında artıklığın sağladığı yüksek kullanılabilirliği, coğrafi çoğaltma tarafından sağlanan bölgesel kesintilere karşı korumayla birleştirir. GZRS depolama hesabındaki veriler birincil bölgedeki üç [Azure kullanılabilirlik bölgesinde](../../availability-zones/az-overview.md) kopyalanır ve bölgesel felaketlerden korunmak için ikincil bir coğrafi bölgeye kopyalanır. Microsoft, maksimum tutarlılık, dayanıklılık ve kullanılabilirlik, mükemmel performans ve olağanüstü durum kurtarma için esneklik gerektiren uygulamalar için GZRS kullanılmasını önerir.

GZRS depolama hesabıyla, kullanılabilirlik bölgesi kullanılamıyorsa veya kurtarılamazsa verileri okumaya ve yazmaya devam edebilirsiniz. Ayrıca, verileriniz tam bir bölgesel kesinti veya birincil bölgenin kurtarılmadığı bir felaket durumunda da dayanıklıdır. GZRS, belirli bir yıl içinde nesnelerin %99,9999999999999999999999'una (16 9's) dayanıklılık sağlamak üzere tasarlanmıştır.

Yalnızca genel amaçlı v2 depolama hesapları GZRS ve RA-GZRS'yi destekler. Depolama hesabı türleri hakkında daha fazla bilgi için [Azure depolama hesabına genel bakış](storage-account-overview.md)bilgisine bakın. GZRS ve RA-GZRS blok blobs, sayfa lekeleri (VHD diskler hariç), dosyalar, tablolar ve kuyruklar destekler.

GZRS ve RA-GZRS şu anda aşağıdaki bölgelerde önizleme için kullanılabilir:

- Güneydoğu Asya
- Kuzey Avrupa
- Batı Avrupa
- Doğu Japonya
- Güney Birleşik Krallık
- ABD Doğu
- ABD Doğu 2
- ABD Orta
- ABD Batı 2

Microsoft, ek Azure bölgelerinde GZRS ve RA-GZRS'yi etkinleştirmeye devam ediyor. Desteklenen bölgeler hakkında bilgi için [Azure Hizmet Güncelleştirmeleri](https://azure.microsoft.com/updates/) sayfasını düzenli olarak kontrol edin.

Önizleme fiyatlandırması hakkında daha fazla bilgi [için, Blobs,](https://azure.microsoft.com/pricing/details/storage/blobs) [Files,](https://azure.microsoft.com/pricing/details/storage/files/) [Queues](https://azure.microsoft.com/pricing/details/storage/queues/)ve [Tablolar](https://azure.microsoft.com/pricing/details/storage/tables/)için GZRS önizleme fiyatlandırmasına bakın.

> [!IMPORTANT]
> Microsoft, üretim iş yükleri için önizleme özelliklerinin kullanılmasına karşı önerir.

## <a name="read-access-to-data-in-the-secondary-region"></a>İkincil bölgedeki verilere erişimi okuma

Coğrafi yedekli depolama (GRS veya GZRS ile) verilerinizi bölgesel kesintilere karşı korumak için ikincil bölgedeki başka bir fiziksel konuma çoğaltır. Ancak, bu veriler yalnızca müşteri veya Microsoft birincil bölgeden ikincil bölgeye bir hata başlatması durumunda okunabilir. İkincil bölgeye okuma erişimini etkinleştirdiğinizde, birincil bölge kullanılamıyorsa verileriniz okunabilir. İkincil bölgeye okuma erişimi için, okuma-erişim coğrafi yedekli depolama (RA-GRS) veya okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) etkinleştirin.

### <a name="design-your-applications-for-read-access-to-the-secondary"></a>İkincil erişim için uygulamalarınızı tasarla

Depolama hesabınız ikincil bölgeye okuma erişimi için yapılandırılırsa, birincil bölge herhangi bir nedenle kullanılamıyorsa uygulamalarınızı ikincil bölgeden okuma verilerine sorunsuz bir şekilde kayacak şekilde tasarlayabilirsiniz. İkincibölge her zaman okuma erişimi için kullanılabilir, böylece bir kesinti durumunda ikincil den okunacak emin olmak için uygulamanızı test edebilirsiniz. Yüksek kullanılabilirlik için uygulamalarınızı nasıl tasarlarınız hakkında daha fazla bilgi için [bkz.](storage-designing-ha-apps-with-ragrs.md)

İkincil erişim etkinleştirildiğinde, verileriniz ikincil bitiş noktasından ve depolama hesabınızın birincil bitiş noktasından okunabilir. İkincil uç nokta, hesap adına *ikincil olan* sonek ekler. Örneğin, Blob depolama için birincil bitiş `myaccount.blob.core.windows.net`noktası ise, ikincil `myaccount-secondary.blob.core.windows.net`bitiş noktası. Depolama hesabınıziçin hesap erişim anahtarları hem birincil hem de ikincil uç noktalar için aynıdır.

### <a name="check-the-last-sync-time-property"></a>Son Eşitleme Zamanı özelliğini denetleme

Veriler eşzamanlı olarak ikincil bölgeye çoğaltıldığından, ikincil bölge genellikle birincil bölgenin arkasındadır. Birincil bölgede bir hata olursa, birincil e doğru yazdığın tüm yazmaların henüz ikincil bölgeye çoğaltılmış olma olasılığı yüksektir.

Hangi yazma işlemlerinin ikincil bölgeye çoğaltıldığını belirlemek için, uygulamanız depolama hesabınız için **Son Eşitleme Zamanı** özelliğini denetleyebilir. Son eşitleme saatinden önce birincil bölgeye yazılan tüm yazma işlemleri, ikincil bölgeye başarıyla çoğaltıldı, yani ikincil bölgeden okunabilecek şekilde kullanılabilir. Son eşitleme süresinden sonra birincil bölgeye yazılan yazma işlemleri ikincil bölgeye çoğaltılmış olabilir veya çoğaltılmış olabilir, bu da okuma işlemleri için kullanılamayabilecekleri anlamına gelir.

Azure PowerShell, Azure CLI veya Azure Depolama istemci kitaplıklarından birini kullanarak **Son Eşitleme Zamanı** özelliğinin değerini sorgulayabilirsiniz. **Son Eşitleme Zamanı** özelliği BIR GMT tarih/saat değeridir. Daha fazla bilgi için, [bir depolama hesabı için Son Eşitleme Zamanı özelliğini denetleyin'](last-sync-time-get.md)e bakın.

## <a name="summary-of-redundancy-options"></a>Artıklık seçeneklerinin özeti

Aşağıdaki tablo, depolama hesabınız için hangi tür fazlalık ların etkin olduğuna bağlı olarak, verilerinizin belirli bir senaryoda ne kadar dayanıklı ve kullanılabilir olduğunu gösterir:

| Senaryo                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS (önizleme)                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Veri merkezi içindeki düğüm kullanılamaz hale gelir                                                                 | Evet                             | Evet                              | Evet                                  | Evet                                  |
| Tüm veri merkezi (zonal veya non-zonal) kullanılamaz hale gelir                                           | Hayır                              | Evet                              | Evet                                  | Evet                                  |
| Bölge genelinde bir kesinti oluşur                                                                                     | Hayır                              | Hayır                               | Evet                                  | Evet                                  |
| Birincil bölge kullanılamıyorsa ikincil bölgedeki verilere erişimi okuma | Hayır                              | Hayır                               | Evet (RA-GRS ile)                                   | Evet (RA-GZRS ile)                                 |
| Belirli bir yıl daki nesnelerin yüzde dayanıklılığı<sup>1</sup>                                          | en az 99.99999999999% (11 9's) | en az 99.9999999999999% (12 9's) | en az 99.99999999999999999999999% (16 9's) | en az 99.99999999999999999999999% (16 9's) |
| Desteklenen depolama hesabı türleri<sup>2</sup>                                                                   | GPv2, GPv1, BlockBlobStorage, BlobStorage, FileStorage                | GPv2, BlockBlobStorage, FileStorage                             | GPv2, GPv1, BlobStorage                     | GPv2                     |
| Okuma istekleri için kullanılabilirlik SLA<sup>1</sup>  | En az %99,9 (havalı erişim katmanı için %99) | En az %99,9 (havalı erişim katmanı için %99) | GRS için en az %99,9 (serin erişim katmanı için %99)<br /><br />RA-GRS için en az %99,99 (havalı erişim katmanı için %99,9) | GZRS için en az %99,9 (havalı erişim katmanı için %99)<br /><br />RA-GZRS için en az %99,99 (havalı erişim katmanı için %99,9) |
| Yazma istekleri için kullanılabilirlik SLA<sup>1</sup>  | En az %99,9 (havalı erişim katmanı için %99) | En az %99,9 (havalı erişim katmanı için %99) | En az %99,9 (havalı erişim katmanı için %99) | En az %99,9 (havalı erişim katmanı için %99) |

<sup>1</sup> Dayanıklılık ve kullanılabilirlik için Azure Depolama garantileri hakkında bilgi için [Azure Depolama SLA'sına](https://azure.microsoft.com/support/legal/sla/storage/)bakın.

<sup>2</sup> Depolama hesabı türleri hakkında bilgi için [bkz.](storage-account-overview.md)

Tüm depolama hesapları ve [tüm katmanlar (arşiv dahil)](../blobs/storage-blob-storage-tiers.md) için tüm veriler, depolama hesabı için artıklık seçeneğine göre kopyalanır. Blok lekeleri, ek blobs, sayfa blobs, kuyruklar, tablolar ve dosyalar gibi nesneler kopyalanır.

Her artıklık seçeneğiiçin fiyatlandırma bilgileri için [Azure Depolama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.

> [!NOTE]
> Azure Premium Disk Depolama şu anda yalnızca yerel olarak yedekli depolamayı (LRS) destekler. Blob depolama hesaplarını blok, belirli bölgelerde ki yerel gereksiz depolama (LRS) ve bölge yedekdepolama (ZRS) destekler.

## <a name="data-integrity"></a>Veri bütünlüğü

Azure Depolama, döngüsel artıklık denetimleri (CRC'ler) kullanılarak depolanan verilerin bütünlüğünü düzenli olarak doğrular. Veri bozulması algılanırsa, gereksiz veriler kullanılarak onarılır. Azure Depolama, veri depolama veya alma yaparken veri paketlerinin bozulmasını algılamak için tüm ağ trafiğindeki denetim leri de hesaplar.

## <a name="see-also"></a>Ayrıca bkz.

- [Depolama hesabı için Son Eşitleme Zamanı özelliğini denetleme](last-sync-time-get.md)
- [Depolama hesabı için artıklık seçeneğini değiştirme](redundancy-migration.md)
- [RA-GRS Depolama kullanarak yüksek kullanılabilir uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
- [Olağanüstü durum kurtarma ve hesap başarısız (önizleme)](storage-disaster-recovery-guidance.md)
