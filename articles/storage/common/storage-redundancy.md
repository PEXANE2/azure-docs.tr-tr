---
title: Veri yedekliği
titleSuffix: Azure Storage
description: Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır. Artıklık yapılandırması yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS), Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS), coğrafi bölge yedekli depolama (GZRS) (Önizleme) ve okuma erişimi içerir coğrafi bölge yedekli depolama (RA-GZRS) (Önizleme).
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 6b36694c2fe1bf264c876944b054d39371db616c
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674282"
---
# <a name="azure-storage-redundancy"></a>Azure depolama artıklığı

Azure depolama, geçici donanım arızaları, ağ veya güç kesintileri ve çok büyük doğal felaketler de dahil olmak üzere planlı ve planlanmamış olaylardan korunabilmesi için verilerinizin birden çok kopyasını her zaman depolar. Yedeklilik, depolama hesabınızın, başarısızlık durumunda bile [Azure depolama Için hizmet düzeyi sözleşmesini (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) karşıladığından emin olmanızı sağlar.

Senaryonuz için en uygun artıklık seçeneğinin hangisi olduğuna karar verirken, düşük maliyetler ve daha yüksek kullanılabilirlik ve dayanıklılık arasındaki avantajları göz önünde bulundurun. Hangi artıklık seçeneğini belirlemenizi gerektiğine yardımcı olan faktörler şunlardır:  

- Verileriniz birincil bölgede nasıl çoğaltılır
- Verilerinizin birincil bölgeye coğrafi olarak uzaktaki ikinci bir konuma çoğaltılıp çoğaltılmayacağı, bölgesel olağanüstü durumlara karşı koruma
- Birincil bölge herhangi bir nedenle kullanılamaz duruma gelirse, uygulamanızın ikincil bölgedeki çoğaltılan verilere okuma erişimi gerektirip gerektirmediğini belirtir

## <a name="redundancy-in-the-primary-region"></a>Birincil bölgedeki artıklık

Azure depolama hesabındaki veriler her zaman birincil bölgede üç kez çoğaltılır. Azure depolama, verilerinizin birincil bölgede nasıl çoğaltılacağı konusunda iki seçenek sunar:

- **Yerel olarak yedekli depolama (LRS)** , verilerinizi, birincil bölgedeki tek bir fiziksel konum içinde zaman uyumlu olarak üç kez kopyalar. LRS, en az maliyetli çoğaltma seçeneğidir, ancak yüksek kullanılabilirlik gerektiren uygulamalar için önerilmez.
- Bölgesel olarak **yedekli depolama (ZRS)** , verilerinizi, birincil bölgedeki üç Azure kullanılabilirlik bölgesi üzerinden eşzamanlı olarak kopyalar. Microsoft, yüksek kullanılabilirlik gerektiren uygulamalar için birincil bölgede ZRS kullanımını ve ayrıca bir ikincil bölgeye çoğaltmayı önerir.

### <a name="locally-redundant-storage"></a>Yerel olarak yedekli depolama

Yerel olarak yedekli depolama (LRS), verilerinizi birincil bölgedeki tek bir fiziksel konum içinde üç kez çoğaltır. LRS, belirli bir yıl boyunca nesnelerin en az% 99,999999999 (11 nines) dayanıklılığı sağlar.

LRS, en düşük maliyetli artıklık seçeneğidir ve diğer seçeneklere kıyasla en az dayanıklılık sağlar. LRS, verilerinizi sunucu rafı ve sürücü hatalarıyla karşı korur. Ancak, veri merkezinde yangın ya da taşması gibi bir olağanüstü durum oluşursa, LRS kullanan bir depolama hesabının tüm çoğaltmaları kaybolabilir veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft, bölgesel olarak [yedekli depolama (](#zone-redundant-storage) ZRS), coğrafi olarak [yedekli depolama](#geo-redundant-storage) (GRS) veya [coğrafi bölge-yedekli depolama (Önizleme)](#geo-zone-redundant-storage-preview) (GZRS) kullanmanızı önerir.

LRS kullanan bir depolama hesabına yazma isteği eşzamanlı olarak gerçekleştirilir. Yazma işlemi, yalnızca veriler üç kopyaya yazıldıktan sonra başarıyla döndürülür.

LRS aşağıdaki senaryolar için iyi bir seçimdir:

- Uygulamanız veri kaybı oluşursa kolayca yeniden oluşturabileceğiniz verileri depoluyorsa LRS 'yi kabul edebilirsiniz.
- Uygulamanız veri idare gereksinimleri nedeniyle yalnızca bir ülke veya bölge içinde veri çoğaltmaya sınırlı olursa LRS 'yi kabul edebilirsiniz. Bazı durumlarda, verilerin coğrafi olarak çoğaltılacağı eşleştirilmiş bölgeler başka bir ülkede veya bölgede olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).

### <a name="zone-redundant-storage"></a>Alanlar arası yedekli depolama

Bölgesel olarak yedekli depolama (ZRS), Azure depolama verilerinizi birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak çoğaltır. Her kullanılabilirlik alanı, bağımsız güç, soğutma ve ağ ile ayrı bir fiziksel konumdur. ZRS, belirli bir yıl boyunca en az% 99,9999999999 (12 9 ' a) Azure depolama veri nesneleri için dayanıklılık sağlar.

ZRS ile, bir bölge kullanılamaz hale gelirse bile verilerinize hem okuma hem de yazma işlemleri için erişilebilir. Bir bölge kullanılamaz duruma gelirse Azure, DNS yeniden işaretleme gibi ağ güncelleştirmelerini alır. Güncelleştirmeler tamamlanmadan önce verilere erişmeniz durumunda bu güncelleştirmeler uygulamanızı etkileyebilir. ZRS için uygulama tasarlarken, üstel geri alma ile yeniden deneme ilkeleri uygulama da dahil olmak üzere geçici hata işleme için uygulamaları izleyin.

ZRS kullanan bir depolama hesabına yazma isteği eşzamanlı olarak gerçekleştirilir. Yazma işlemi, veriler yalnızca üç kullanılabilirlik alanı genelinde tüm yinelemelere yazıldıktan sonra başarıyla döndürülür.

Microsoft, birincil bölgede, tutarlılık, dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için ZRS kullanılmasını önerir. ZRS, geçici olarak kullanılamaz duruma gelirse verileriniz için mükemmel performans, düşük gecikme süresi ve dayanıklılık sağlar. Ancak, ZRS, verileri birden çok bölgenin kalıcı olarak etkilediği bölgesel bir olağanüstü duruma karşı koruyamayabilir. Bölgesel felate karşı koruma için, Microsoft birincil bölgede ZRS kullanan ve ayrıca verilerinizi ikincil bir bölgeye çoğaltan [coğrafi bölge yedekli depolama](#geo-zone-redundant-storage-preview) (GZRS) kullanılmasını önerir.

Aşağıdaki tabloda hangi depolama hesabı türlerinin hangi bölgelerde ZRS destekledikleri gösterilmektedir:

|    Depolama hesabı türü    |    Desteklenen bölgeler    |    Desteklenen hizmetler    |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
|    Genel amaçlı v2<sup>1</sup>    | Güneydoğu Asya<br /> Kuzey Avrupa<br />  Batı Avrupa<br /> Orta Fransa<br /> Doğu Japonya<br /> Güney Birleşik Krallık<br /> ABD Orta<br /> ABD Doğu<br /> ABD Doğu 2<br /> ABD Batı 2    |    Blok blobları<br /> Sayfa Blobları<sup>2</sup><br /> Dosya paylaşımları (Standart)<br /> Tablolar<br /> Kuyruklar<br /> |
|    BlockBlobStorage<sup>1</sup>    | Batı Avrupa<br /> ABD Doğu    |    Yalnızca blok Blobları    |
|    Dosya depolama    | Batı Avrupa<br /> ABD Doğu    |    Yalnızca Azure dosyaları    |

<sup>1</sup> arşiv katmanı Şu anda ZRS hesapları için desteklenmiyor.<br />
<sup>2</sup> sanal makineler için Azure yönetilen diskleri içeren depolama hesapları her zaman LRS kullanır. Azure yönetilmeyen diskler de LRS kullanmalıdır. GRS kullanan Azure yönetilmeyen diskler için bir depolama hesabı oluşturmak mümkündür, ancak zaman uyumsuz coğrafi çoğaltma üzerinde tutarlılık nedeniyle olası sorunlar nedeniyle bu önerilmez. Yönetilen veya yönetilmeyen diskler ZRS veya GZRS 'yi desteklemez. Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).

ZRS 'yi destekleyen bölgeler hakkında daha fazla bilgi için, [Azure kullanılabilirlik alanları nedir?](../../availability-zones/az-overview.md)bölümünde **bölgeye göre hizmetlere göre destek** bölümüne bakın.

## <a name="redundancy-in-a-secondary-region"></a>İkincil bölgede artıklık

Yüksek kullanılabilirlik gerektiren uygulamalar için, Depolama hesabınızdaki verileri, birincil bölgeden yüzlerce mil uzakta olan ikincil bir bölgeye kopyalamayı tercih edebilirsiniz. Depolama Hesabınız ikincil bir bölgeye kopyalanırsa, tüm bölgesel bir kesinti veya birincil bölgenin kurtarılamaz bir olağanüstü durum durumunda bile verileriniz dayanıklı olur.

Bir depolama hesabı oluşturduğunuzda, hesabın birincil bölgesini seçersiniz. Eşleştirilmiş ikincil bölge, birincil bölgeye göre belirlenir ve değiştirilemez. Azure tarafından desteklenen bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/).

Azure depolama, verilerinizi ikincil bir bölgeye kopyalamak için iki seçenek sunar:

- **Coğrafi olarak yedekli depolama (GRS)** , LRS kullanarak, birincil bölgedeki tek bir fiziksel konum içinde verilerinizi eşzamanlı olarak üç kez kopyalar. Daha sonra verilerinizi zaman uyumsuz olarak ikincil bölgedeki tek bir fiziksel konuma kopyalar.
- **Coğrafi bölge yedekli depolama (GZRS)** (Önizleme), ZRS kullanarak birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında verilerinizi eşzamanlı olarak kopyalar. Daha sonra verilerinizi zaman uyumsuz olarak ikincil bölgedeki tek bir fiziksel konuma kopyalar.

GRS ve GZRS arasındaki birincil fark, verilerin birincil bölgede nasıl çoğaltıladır. İkincil konum içinde, veriler her zaman eş zamanlı olarak LRS kullanılarak çoğaltılır.

GRS veya GZRS ile ikincil konumdaki veriler, ikincil bölgede bir yük devretme işlemi olmadıkça okuma veya yazma erişimi için kullanılamaz. İkincil konuma okuma erişimi için, depolama hesabınızı Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) veya Okuma Erişimli Coğrafi bölge-yedekli depolama (RA-GZRS) kullanacak şekilde yapılandırın. Daha fazla bilgi için bkz. [İkincil bölgedeki verilere okuma erişimi](#read-access-to-data-in-the-secondary-region).

Birincil bölge kullanılamaz duruma gelirse ikincil bölgeye yük devretme (Önizleme) seçeneğini belirleyebilirsiniz. Yük devretme tamamlandıktan sonra, ikincil bölge birincil bölge olur ve verileri okuyabilir ve yazabilirsiniz. Olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek ve ikincil bölgeye yük devretmeyi öğrenmek için bkz. [olağanüstü durum kurtarma ve hesap yük devretme (Önizleme)](storage-disaster-recovery-guidance.md).

> [!IMPORTANT]
> Veriler ikincil bölgeye zaman uyumsuz olarak çoğaltıldığından, birincil bölgeyi etkileyen bir hata, birincil bölge kurtarılamazsa veri kaybına neden olabilir. Birincil bölgeye en son yazma işlemleri ile ikincil bölgeye son yazma arasındaki Aralık, kurtarma noktası hedefi (RPO) olarak bilinir. RPO, verilerin kurtarılabileceği noktayı gösterir. Azure depolama, genellikle 15 dakikadan daha kısa bir RPO 'ya sahiptir, ancak şu anda verilerin ikincil bölgeye çoğaltılmasının ne kadar sürdüğü konusunda bir SLA yoktur.

### <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Coğrafi olarak yedekli depolama (GRS), LRS kullanarak, birincil bölgedeki tek bir fiziksel konum içinde verilerinizi eşzamanlı olarak üç kez kopyalar. Daha sonra verilerinizi zaman uyumsuz bir şekilde birincil bölgeden yüzlerce mil olan ikincil bölgedeki tek bir fiziksel konuma kopyalar. GRS, belirli bir yıl boyunca en az% 99.99999999999999 (16 9) Azure depolama veri nesneleri için dayanıklılık sağlar.

İlk olarak birincil konuma bir yazma işlemi kaydedilir ve LRS kullanılarak çoğaltılır. Güncelleştirme daha sonra ikincil bölgeye zaman uyumsuz olarak çoğaltılır. Veriler ikincil konuma yazıldığında, LRS kullanarak bu konumda da çoğaltılır.

### <a name="geo-zone-redundant-storage-preview"></a>Coğrafi bölge yedekli depolama (Önizleme)

Coğrafi bölge yedekli depolama (GZRS) (Önizleme), coğrafi çoğaltma tarafından sağlanmış olan bölgesel kesintilerden korunarak kullanılabilirlik alanları genelinde artıklık tarafından sağlanmış yüksek kullanılabilirliği birleştirir. Bir GZRS Storage hesabındaki veriler, birincil bölgedeki üç [Azure kullanılabilirlik bölgesine](../../availability-zones/az-overview.md) kopyalanır ve ayrıca bölgesel felaketlerden koruma için ikincil bir coğrafi bölgeye çoğaltılır. Microsoft, olağanüstü durum kurtarma için maksimum tutarlılık, dayanıklılık ve kullanılabilirlik, mükemmel performans ve esnekliği gerektiren uygulamalar için GZRS kullanılmasını önerir.

Bir GZRS depolama hesabı ile bir kullanılabilirlik alanı kullanılamaz hale gelirse veya kurtarılamaz durumdaysa verileri okumaya ve yazmaya devam edebilirsiniz. Ayrıca, tüm bölgesel bir kesinti veya birincil bölgenin kurtarılamaz bir olağanüstü durum durumunda verileriniz de dayanıklı olur. GZRS belirli bir yıl boyunca nesnelerin en az% 99.99999999999999 (16 9) oranında dayanıklılığını sağlamak üzere tasarlanmıştır.

GZRS ve RA-GZRS desteği yalnızca genel amaçlı v2 depolama hesaplarıdır. Depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md). GZRS ve RA-GZRS desteği blok Blobları, sayfa Blobları (VHD diskleri hariç), dosyalar, tablolar ve kuyruklar.

GZRS ve RA-GZRS Şu anda şu bölgelerde önizleme için kullanılabilir:

- Güneydoğu Asya
- Kuzey Avrupa
- Batı Avrupa
- Güney Birleşik Krallık
- ABD Doğu
- ABD Doğu 2
- ABD Orta

Microsoft, ek Azure bölgelerinde GZRS ve RA-GZRS etkinleştirmesine devam etmektedir. Desteklenen bölgeler hakkında bilgi için [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/) sayfasını düzenli olarak denetleyin.

Önizleme fiyatlandırması hakkında bilgi için, [Bloblar](https://azure.microsoft.com/pricing/details/storage/blobs), [dosyalar](https://azure.microsoft.com/pricing/details/storage/files/), [Kuyruklar](https://azure.microsoft.com/pricing/details/storage/queues/)ve [Tablolar](https://azure.microsoft.com/pricing/details/storage/tables/)için GZRS Preview fiyatlandırması bölümüne bakın.

> [!IMPORTANT]
> Microsoft, üretim iş yükleri için Önizleme özelliklerinin kullanılmasını önerir.

## <a name="read-access-to-data-in-the-secondary-region"></a>İkincil bölgedeki verilere yönelik okuma erişimi

Coğrafi olarak yedekli depolama (GRS veya GZRS ile), bölgesel kesintilere karşı koruma sağlamak için verilerinizi ikincil bölgedeki başka bir fiziksel konuma çoğaltır. Ancak, bu veriler yalnızca müşteri veya Microsoft birincili ikincil bölgeye yük devretme işlemi başlattığında okunabilir. İkincil bölgeye okuma erişimini etkinleştirdiğinizde, birincil bölge kullanılamaz hale gelirse verileriniz okunabilir. İkincil bölgeye okuma erişimi için Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) veya Okuma Erişimli Coğrafi bölge-yedekli depolamayı (RA-GZRS) etkinleştirin.

### <a name="design-your-applications-for-read-access-to-the-secondary"></a>Uygulamalarınızı ikincil için okuma erişimi için tasarlama

Depolama Hesabınız ikincil bölgeye okuma erişimi için yapılandırılmışsa, birincil bölge herhangi bir nedenden dolayı kullanılamaz hale gelirse, uygulamalarınızı ikincil bölgeden verileri okumak için sorunsuz bir şekilde kaydırma yapmak üzere tasarlayabilirsiniz. İkincil bölge, her zaman okuma erişimi için kullanılabilir olduğundan, bir kesinti durumunda ikincinden okunmasını sağlamak için uygulamanızı test edebilirsiniz. Uygulamalarınızı yüksek kullanılabilirliğe göre tasarlamak hakkında daha fazla bilgi için bkz. [Okuma Erişimli Coğrafi olarak yedekli depolama kullanarak yüksek oranda kullanılabilir uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md).

İkinciye okuma erişimi etkinleştirildiğinde, verileriniz ikincil uç noktadan ve depolama hesabınızın birincil uç noktasından okunabilir. İkincil uç nokta son eki *–* hesap adına ekler. Örneğin, BLOB depolama için birincil uç noktanız `myaccount.blob.core.windows.net`, ikincil uç nokta `myaccount-secondary.blob.core.windows.net`. Depolama hesabınızın hesap erişim anahtarları, hem birincil hem de ikincil uç noktalar için aynıdır.

### <a name="check-the-last-sync-time-property"></a>Son Eşitleme Zamanı özelliğini denetleme

Veriler ikincil bölgeye zaman uyumsuz olarak çoğaltıldığından, ikincil bölge genellikle birincil bölgenin arkasında olur. Birincil bölgede bir hata oluşursa, birinciye yazma işlemleri henüz ikinciye çoğaltılmayacak.

Hangi yazma işlemlerinin ikincil bölgeye çoğaltıldığını öğrenmek için, uygulamanız depolama hesabınızın **son eşitleme zamanı** özelliğini denetleyebilir. Birincil bölgeye son eşitleme zamanından önce yazılan tüm yazma işlemleri, ikincil bölgeye başarıyla çoğaltılmıştı ve bu, ikincilden okunmak üzere kullanılabilir. Son eşitleme zamanından sonra birincil bölgeye yazılan yazma işlemleri, ikincil bölgeye çoğaltılmayabilir veya bu, okuma işlemleri için uygun olmayabilir.

**Son eşitleme zamanı** özelliğinin değerini Azure PowerShell, Azure CLI veya Azure Storage istemci kitaplıklarından birini kullanarak sorgulayabilirsiniz. **Son eşitleme zamanı** ÖZELLIĞI bir GMT Tarih/saat değeri. Daha fazla bilgi için bkz. [depolama hesabı Için Son eşitleme zamanı özelliğini denetleme](last-sync-time-get.md).

## <a name="summary-of-redundancy-options"></a>Artıklık seçeneklerinin Özeti

Aşağıdaki tabloda, depolama hesabınızda hangi artıklık türünün geçerli olduğuna bağlı olarak verilerinizin belirli bir senaryoda ne kadar dayanıklı ve kullanılabilir olduğunu gösterilmektedir:

| Senaryo                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS (Önizleme)                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Veri Merkezi içindeki bir düğüm kullanılamaz duruma gelir                                                                 | Yes                             | Yes                              | Yes                                  | Yes                                  |
| Tüm veri merkezi (zonal veya ZGen olmayan) kullanılamaz hale gelir                                           | Hayır                              | Yes                              | Yes                                  | Yes                                  |
| Bölge genelinde kesinti meydana gelir                                                                                     | Hayır                              | Hayır                               | Yes                                  | Yes                                  |
| Birincil bölge kullanılamaz duruma gelirse ikincil bölgedeki verilere yönelik okuma erişimi | Hayır                              | Hayır                               | Evet (RA-GRS ile)                                   | Evet (RA-GZRS ile)                                 |
| Belirli<sup>bir yıl boyunca</sup> nesnelerin yüzde dayanıklılığı yüzdesi                                          | en az% 99,999999999 (11 9) | en az% 99,9999999999 (12 9) | en az% 99.99999999999999 (16 9) | en az% 99.99999999999999 (16 9) |
| Desteklenen depolama hesabı türleri<sup>2</sup>                                                                   | GPv2, GPv1, BlockBlobStorage, BlobStorage, FileStorage                | GPv2, BlockBlobStorage, FileStorage                             | GPv2, GPv1, BlobStorage                     | GPv2                     |
| Okuma istekleri için kullanılabilirlik SLA 'Sı<sup>1</sup>  | En az% 99,9 (Seyrek Erişimli Katman için %99) | En az% 99,9 (Seyrek Erişimli Katman için %99) | GRS için en az% 99,9 (Seyrek Erişimli Katman için %99)<br /><br />RA-GRS için en az% 99,99 (Seyrek Erişimli Katman için% 99,9) | GZRS için en az% 99,9 (Seyrek Erişimli Katman için %99)<br /><br />RA-GZRS için en az% 99,99 (Seyrek Erişimli Katman için% 99,9) |
| Yazma istekleri için kullanılabilirlik SLA 'Sı<sup>1</sup>  | En az% 99,9 (Seyrek Erişimli Katman için %99) | En az% 99,9 (Seyrek Erişimli Katman için %99) | En az% 99,9 (Seyrek Erişimli Katman için %99) | En az% 99,9 (Seyrek Erişimli Katman için %99) |

<sup>1</sup> dayanıklılık ve kullanılabilirlik Için Azure depolama garantisi hakkında daha fazla bilgi için bkz. [Azure Storage SLA](https://azure.microsoft.com/support/legal/sla/storage/).

<sup>2</sup> depolama hesabı türleri hakkında bilgi için bkz. [depolama hesabına genel bakış](storage-account-overview.md).

Tüm depolama hesabı türleri için tüm veriler, depolama hesabının artıklık seçeneğine göre kopyalanır. Blok Blobları, ekleme Blobları, sayfa Blobları, kuyruklar, tablolar ve dosyalar da dahil olmak üzere nesneler kopyalanır.

Her artıklık seçeneği için fiyatlandırma bilgileri için bkz. [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

> [!NOTE]
> Azure Premium Disk Depolama şu anda yalnızca yerel olarak yedekli depolamayı (LRS) desteklemektedir. Blok Blob depolama hesapları, belirli bölgelerde yerel olarak yedekli depolamayı (LRS) ve bölge yedekli depolamayı (ZRS) destekler.

## <a name="data-integrity"></a>Veri bütünlüğü

Azure depolama, Döngüsel artıklık denetimleri (CRCs) kullanılarak depolanan verilerin bütünlüğünü düzenli olarak doğrular. Veri bozulması algılanırsa, bu, gereksiz veriler kullanılarak onarılır. Azure Storage Ayrıca veri depolarken veya alırken veri paketlerinin bozulmasını algılamak için tüm ağ trafiğinde sağlama toplamlarını hesaplar.

## <a name="see-also"></a>Ayrıca bkz.

- [Bir depolama hesabı için son eşitleme zamanı özelliğini denetleyin](last-sync-time-get.md)
- [Depolama hesabı için artıklık seçeneğini değiştirme](redundancy-migration.md)
- [RA-GRS depolama kullanarak yüksek oranda kullanılabilir uygulamalar tasarlama](../storage-designing-ha-apps-with-ragrs.md)
- [Olağanüstü durum kurtarma ve hesap yük devretme (Önizleme)](storage-disaster-recovery-guidance.md)
