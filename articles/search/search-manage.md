---
title: Portalda servis yönetimi
titleSuffix: Azure Cognitive Search
description: Azure portalını kullanarak Microsoft Azure'da barındırılan bir bulut arama hizmeti olan Azure Bilişsel Arama hizmetini yönetin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: azure-portal
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3abbf2c8e0734d17aabadd2ae5f61cc03889964b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282932"
---
# <a name="service-administration-for-azure-cognitive-search-in-the-azure-portal"></a>Azure portalında Azure Bilişsel Arama için hizmet yönetimi
> [!div class="op_single_selector"]
> * [Powershell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Portal](search-manage.md)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Bilişsel Arama, özel uygulamalarda zengin bir arama deneyimi oluşturmak için kullanılan tam olarak yönetilen bulut tabanlı bir arama hizmetidir. Bu makale, azure [portalında](https://portal.azure.com) zaten vermiş olduğunuz bir arama hizmeti için gerçekleştirebileceğiniz hizmet yönetimi görevlerini kapsar. Servis yönetimi, aşağıdaki görevle sınırlı olmak üzere, tasarım gereği hafiftir:

> [!div class="checklist"]
> * Hizmetinizin okunması veya yazılması için kullanılan *api tuşlarına* erişimi yönetin.
> * Bölümlerin ve yinelemelerin tahsisini değiştirerek hizmet kapasitesini ayarlayın.
> * Hizmet katmanınızın maksimum sınırlarına göre kaynak kullanımını izleyin.

*Yükseltmenin* yönetim görevi olarak listelenmediğini unutmayın. Hizmetler sağlandığında kaynaklar ayrıldıklarından, farklı bir katmana geçmek için yeni bir hizmet gereklidir. Ayrıntılar için [bkz.](search-create-service-portal.md)

Sorgu hacmini ve diğer ölçümleri izleyebilir ve daha hızlı yanıt süreleri için hizmetinizi ayarlamak için bu öngörüleri kullanabilirsiniz. Daha fazla bilgi için bkz: [Monitör kullanımı ve sorgu ölçümleri](search-monitor-usage.md) ile Performans ve [optimizasyon.](search-performance-optimization.md)

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Yönetici hakları
Hizmetin sağlanması veya hizmetin kaldırılması bir Azure abonelik yöneticisi veya yardımcı yöneticisi tarafından yapılabilir.

Bir hizmet içinde, hizmet URL'sine ve yönetici api anahtarına erişimi olan herkes hizmete okuma yazma erişimine sahiptir. Okuma-yazma erişimi, Api tuşları, dizinler, dizinler, veri kaynakları, zamanlamalar ve [RBAC tanımlı roller](search-security-rbac.md)aracılığıyla uygulanan rol atamaları da dahil olmak üzere sunucu nesnelerini ekleme, silme veya değiştirme olanağı sağlar.

Azure Bilişsel Arama ile tüm kullanıcı etkileşimi bu modlardan birine girer: hizmete okuma yazma erişimi (yönetici hakları) veya hizmete salt okunur erişim (sorgu hakları). Daha fazla bilgi için [bkz.](search-security-api-keys.md)

<a id="sys-info"></a>

## <a name="logging-and-system-information"></a>Günlük ve sistem bilgileri
Azure Bilişsel Arama, portal veya programlı arabirimler aracılığıyla tek bir hizmete ait günlük dosyalarını ortaya çıkarmaz. Temel katmanda ve yukarıda, Microsoft tüm Azure Bilişsel Arama hizmetlerini hizmet düzeyi sözleşmeleri (SLA) başına %99,9 kullanılabilirlik için izler. Hizmet yavaşsa veya istek girdisi SLA eşiklerinin altına düşerse, destek ekipleri kendi lerinde bulunan günlük dosyalarını gözden geçirir ve sorunu giderir.

Hizmetiniz hakkında genel bilgiler açısından, aşağıdaki yollarla bilgi edinebilirsiniz:

* Portalda, hizmet panosunda, bildirimler, özellikler ve durum iletileri aracılığıyla.
* [Hizmet özelliklerini](https://docs.microsoft.com/rest/api/searchmanagement/services)veya dizin kaynağı kullanımında durum elde etmek için [PowerShell](search-manage-powershell.md) veya [Management REST API'yi](https://docs.microsoft.com/rest/api/searchmanagement/) kullanma.


<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Kaynak kullanımını izleme
Panoda, kaynak izleme hizmeti panosunda gösterilen bilgiler ve hizmeti sorgulayarak elde edebileceğiniz birkaç ölçümle sınırlıdır. Hizmet panosunda, Kullanım bölümünde, bölüm kaynak düzeylerinin uygulamanız için yeterli olup olmadığını hızlı bir şekilde belirleyebilirsiniz. Günlüğe kaydedilmiş olayları yakalamak ve sürdürmek istiyorsanız, Azure izleme gibi harici kaynaklar sağlayabilirsiniz. Daha fazla bilgi için [bkz.](search-monitor-usage.md)

Arama hizmeti REST API'yi kullanarak, belgelerin ve dizinlerin programlı bir şekilde sayılabilir: 

* [Dizin İstatistiklerini Al](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Belgeleri Say](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Olağanüstü durum kurtarma ve hizmet kesintileri

Verilerinizi kurtarabilsek de, Azure Bilişsel Arama küme veya veri merkezi düzeyinde bir kesinti olduğunda hizmetin anında başarısız olmasına izin vermez. Veri merkezinde bir küme başarısız olursa, işlem ekibi algılar ve hizmeti geri yüklemek için çalışır. Hizmet geri yüklemesi sırasında kesinti yle karşılaşırsınız, ancak [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/)uyarınca hizmet inmemasını telafi etmek için hizmet kredisi talep edebilirsiniz. 

Microsoft'un denetimi dışında yıkıcı hatalar olması durumunda sürekli hizmet gerekiyorsa, farklı bir bölgede [ek bir hizmet sağlayabilir](search-create-service-portal.md) ve dizinlerin tüm hizmetlerde tamamen gereksiz olduğundan emin olmak için bir coğrafi çoğaltma stratejisi uygulayabilirsiniz.

Dizinleri doldurmak ve yenilemek için [dizin işaretleyicileri](search-indexer-overview.md) kullanan müşteriler, aynı veri kaynağını kullanan coğrafi özgü dizinleyiciler aracılığıyla olağanüstü durum kurtarma işlemlerini işleyebilir. Her biri bir dizinleyici çalıştıran farklı bölgelerdeki iki hizmet, coğrafi artıklık elde etmek için aynı veri kaynağını dizine ekleyebilir. Coğrafi gereksiz olan veri kaynaklarından dizin alıyorsanız, Azure Bilişsel Arama dizinleyicilerinin yalnızca birincil yinelemelerden artımlı dizini (yeni, değiştirilmiş veya silinmiş belgelerden güncelleştirmeleri birleştirme) gerçekleştirebileceğini unutmayın. Bir başarısız olay, yeni birincil yineleme için dizinleyici yeniden noktaya emin olun. 

Dizin leyiciler kullanmazsanız, nesneleri ve verileri paralel olarak farklı arama hizmetlerine itmek için uygulama kodunuzu kullanırsınız. Daha fazla bilgi için [Azure Bilişsel Arama'da Performans ve optimizasyon adabına](search-performance-optimization.md)bakın.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Bilişsel Arama birincil veri depolama çözümü olmadığından, self servis yedekleme ve geri yükleme için resmi bir mekanizma sağlamayız. Ancak, dizin tanımınızı ve anlık fotoğrafınızı bir dizi JSON dosyasına yedeklemek için bu [Azure Bilişsel Arama .NET örnek repo'daki](https://github.com/Azure-Samples/azure-search-dotnet-samples) **dizin yedekleme-geri yükleme** örnek kodunu kullanabilir ve gerekirse dizini geri yüklemek için bu dosyaları kullanabilirsiniz. Bu araç, dizinleri hizmet katmanları arasında da taşıyabilir.

Aksi takdirde, bir dizin oluşturmak ve doldurmak için kullanılan uygulama kodunuz, yanlışlıkla bir dizini silerseniz fiili geri yükleme seçeneğidir. Bir dizin oluşturmak için, onu siler (var olduğunu varsayarsak), hizmetteki dizini yeniden oluşturur ve birincil veri deponuzdan veri alarak yeniden yüklersiniz.

<a id="scale"></a>

## <a name="scale-up-or-down"></a>Ölçek büyütme veya küçültme
Her arama hizmeti en az bir yineleme ve bir bölümle başlar. Özel kaynaklar sağlayan bir [katmana](search-limits-quotas-capacity.md)kaydolduysanız, kaynak kullanımını ayarlamak için hizmet panosundaki **SCALE** döşemesini tıklatın.

Her iki kaynak üzerinden kapasite eklediğinizde, hizmet bunları otomatik olarak kullanır. Sizin tarafınızdan başka bir işlem gerekmez, ancak yeni kaynağın etkisi gerçekleşmeden önce hafif bir gecikme vardır. Ek kaynak sağlanması 15 dakika veya daha uzun sürebilir.

 ![][10]

### <a name="add-replicas"></a>Yineleme ekleme
Yinelemeleri saniyede artırma (QPS) veya yüksek kullanılabilirlik elde yinelemeler ekleyerek yapılır. Her yinelemenin bir dizin kopyası vardır, bu nedenle bir yineleme daha eklemek hizmet sorgusu isteklerini işlemek için kullanılabilir bir dizine daha çevirir. Yüksek kullanılabilirlik için en az 3 yineleme gereklidir (ayrıntılar için [Kapasite Planlama'ya](search-capacity-planning.md) bakın).

Daha fazla yinelemeye sahip bir arama hizmeti, bakiye sorgu isteklerini daha fazla dizin sayısıüzerinden yükleyebilir. Sorgu hacmi düzeyi göz önüne alındığında, istek hizmet için dizin daha fazla kopya olduğunda sorgu iş hacmi daha hızlı olacaktır. Sorgu gecikmesi yaşıyorsanız, ek yinelemeler çevrimiçi olduktan sonra performans üzerinde olumlu bir etki bekleyebilirsiniz.

Yinelemeler ekledikçe sorgu çıktısı artsa da, hizmetinize yinelemeler eklediğinizde tam olarak iki veya üç katı değildir. Tüm arama uygulamaları, sorgu performansını engellenebilen dış etkenlere tabidir. Karmaşık sorgular ve ağ gecikmesi, sorgu yanıt süreleri varyasyonlarına katkıda bulunan iki etkendir.

### <a name="add-partitions"></a>Bölüm ekleme
Çoğu hizmet uygulaması, bölümler yerine daha fazla yineleme ye ihtiyaç duyar. Belge sayısının artırılmasının gerekli olduğu durumlarda, Standart hizmete kaydolduysanız bölümler ekleyebilirsiniz. Temel katman ek bölümler sağlamaz.

Standart katmanda, bölümler 12'nin katları olarak eklenir (özellikle 1, 2, 3, 4, 6 veya 12). Bu bir parçalama objesi. Dizin, tümü 1 bölüm üzerinde depolanabilen veya eşit olarak 2, 3, 4, 6 veya 12 bölüme (bölüm başına bir parça) bölünebilen 12 parça halinde oluşturulur.

### <a name="remove-replicas"></a>Yinelemeleri kaldırma
Yüksek sorgu birimlerinin dönemleri sonra, arama sorgusu yükleri normale döndükten sonra (örneğin, tatil satışları bittikten sonra) yinelemeleri azaltmak için kaydırıcıyı kullanabilirsiniz. Sizin tarafınızdan başka adım aranmaz. Yineleme sayısını düşürmek, veri merkezindeki sanal makinelerden feragat eder. Sorgunuz ve veri alma işlemleriniz artık eskisinden daha az VM'de çalışacaktır. Minimum gereksinim bir yinelemedir.

### <a name="remove-partitions"></a>Bölümleri kaldırma
Sizin tarafınızdan ekstra çaba gerektirmeyan yinelemeleri kaldırmanın aksine, azaltılabilecekden daha fazla depolama alanı kullanıyorsanız yapmanız gereken bazı işler olabilir. Örneğin, çözümünüz üç bölüm kullanıyorsa, yeni depolama alanı dizininizi barındırmak için gerekenden daha azsa, bir veya iki bölüme küçülme hata oluşturur. Tahmin edebileceğiniz gibi, seçimleriniz alanı boşaltmak veya geçerli yapılandırmayı korumak için ilişkili bir dizin içindeki dizinleri veya belgeleri silmektir.

Belirli bölümlerde hangi dizin kırıklarının depolandığınızı söyleyen bir algılama yöntemi yoktur. Her bölüm yaklaşık 25 GB depolama alanı sağlar, bu nedenle depolama alanını sahip olduğunuz bölüm sayısına göre barındırılabilen bir boyuta düşürmeniz gerekir. Tek bir bölüme dönmek istiyorsanız, 12 parçanın tümüne sığması gerekir.

Gelecekteki planlamaya yardımcı olmak için, gerçekte ne kadar kullandığınızı görmek için depolama alanını [(Dizini İstatistikleri Al'ı](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)kullanarak) denetlemek isteyebilirsiniz. 

<a id="next-steps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Hizmet yönetiminin arkasındaki kavramları anladıktan sonra, görevleri otomatikleştirmek için [PowerShell'i](search-manage-powershell.md) kullanmayı düşünün.

Ayrıca performans ve [optimizasyon makalesini](search-performance-optimization.md)gözden geçirmenizi öneririz.

<!--Image references-->
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



