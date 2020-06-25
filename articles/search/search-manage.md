---
title: Portalda hizmet yönetimi
titleSuffix: Azure Cognitive Search
description: Azure portal kullanarak Microsoft Azure barındırılan bir bulut arama hizmeti olan Azure Bilişsel Arama hizmetini yönetin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: azure-portal
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/24/2020
ms.openlocfilehash: f1887065ef356717e05814a23ad85c7f0e6c6ab0
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362671"
---
# <a name="service-administration-for-azure-cognitive-search-in-the-azure-portal"></a>Azure portal Azure Bilişsel Arama için hizmet yönetimi

> [!div class="op_single_selector"]
>
> * [PowerShell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Portal](search-manage.md)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Bilişsel Arama, özel uygulamalara zengin arama deneyimi oluşturmak için kullanılan, tam olarak yönetilen, bulut tabanlı bir arama hizmetidir. Bu makalede, önceden sağladığınız bir arama hizmeti için [Azure Portal](https://portal.azure.com) gerçekleştirebileceğiniz hizmet yönetim görevleri ele alınmaktadır. Hizmet yönetimi, tasarım açısından hafif ve aşağıdaki görevlerle sınırlıdır:

* **Genel bakış** giriş sayfasında hizmetinize ve içeriklerinizi hakkında bilgi edinin.
* Orta sayfa **kullanım** bağlantısını kullanarak depolamayı denetleyin.
* Orta sayfa **izleme** bağlantısını kullanarak sorgu birimlerini ve gecikme süresini ve isteklerin kısıtlanıp kısıtlanmadığını denetleyin.
* Sol taraftaki **anahtarlar** sayfasını kullanarak erişimi yönetin.
* **Ölçek** sayfasını sola kullanarak kapasiteyi ayarlayın.

Portalda gerçekleştirilen aynı görevler Ayrıca [Yönetim API 'leri](https://docs.microsoft.com/rest/api/searchmanagement/) ve [az. Search PowerShell modülü](search-manage-powershell.md)aracılığıyla programlı bir şekilde işlenebilir. Yönetim görevleri Portal ve programlı arabirimler arasında tam olarak temsil edilir. Yalnızca tek bir modülüde kullanılabilen belirli bir yönetim görevi yoktur.

Azure Bilişsel Arama, daha derin izleme ve yönetim için diğer Azure hizmetlerinden yararlanır. Tek başına, bir arama hizmeti ile depolanan tek veri, içerik (dizinler, Dizin Oluşturucu ve veri kaynağı tanımları ve diğer nesneler). Portal sayfalarına bildirilen ölçümler, 30 günlük bir döngüde iç günlüklerden çekilir. Kullanıcı denetimli günlük tutma ve ek olaylar için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/)'ye ihtiyacınız olacaktır. 

## <a name="fixed-service-properties"></a>Düzeltilen hizmet özellikleri

Bir arama hizmetinin birkaç yönü, hizmetin sağlandığı ve daha sonra değiştirilemeyeceğini belirler:

* Hizmet adı (bir hizmeti yeniden adlandıramazsınız)
* Maksimum çoğaltma ve bölüm sayısı (katman tarafından belirlenir)

En yüksek bir bölüm ile temel ile başlayıp artık daha fazla bölüme ihtiyacınız varsa, daha yüksek bir katmanda [Yeni bir hizmet oluşturmanız](search-create-service-portal.md) ve içeriğinizi yeni hizmette yeniden oluşturmanız gerekir. 

## <a name="administrator-rights"></a>Yönetici hakları

Hizmetin kendisi için sağlanması veya kullanımdan kaldırılması, bir Azure abonelik Yöneticisi veya ortak yönetici tarafından yapılabilir.

Uç noktaya erişim için, hizmet URL 'sine erişimi olan herkesin ve bir API anahtarı içeriğe erişebilir. Anahtarlar hakkında daha fazla bilgi için bkz. [API anahtarlarını yönetme](search-security-api-keys.md).

* Hizmete salt okuma erişimi, genellikle URL ve sorgu API anahtarı vererek bir istemci uygulamasına verilen sorgu haklardır.
* Okuma-yazma erişimi, API anahtarları, dizinler, Dizin oluşturucular, veri kaynakları ve zamanlamalar dahil sunucu nesnelerini ekleme, silme veya değiştirme olanağı sağlar. URL, yönetici API anahtarı vererek okuma-yazma erişimi verilir.

Rol atamaları aracılığıyla hizmet sağlama apparas haklarına yönelik haklar verilir. [Rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) , Azure kaynaklarının sağlanması için [Azure Resource Manager](../azure-resource-manager/management/overview.md) oluşturulan bir yetkilendirme sistemidir. 

Azure Bilişsel Arama bağlamında, [RBAC rol atamaları](search-security-rbac.md) , [portalı](search-manage.md), [PowerShell](search-manage-powershell.md)'i veya [Yönetim REST API 'lerini](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)kullanıp kullanmadıklarından bağımsız olarak kimin hangi görevleri gerçekleştirebileceğini tespit eder:

* Hizmet oluşturma veya silme
* Hizmeti ölçeklendirme
* API anahtarlarını silme veya yeniden oluşturma
* Tanılama günlüğünü etkinleştirme (hizmet oluşturma)
* Trafik analizini etkinleştir (hizmet oluştur)

> [!TIP]
> Azure genelinde mekanizmalar kullanarak, yönetici haklarına sahip kullanıcılar tarafından, arama hizmetinizin yanlışlıkla veya izinsiz olarak silinmesini engellemek için bir aboneliği veya kaynağı kilitleyebilir. Daha fazla bilgi için bkz. [beklenmeyen silme işlemini engellemek için kaynakları kilitleme](../azure-resource-manager/management/lock-resources.md).

## <a name="logging-and-system-information"></a>Günlüğe kaydetme ve sistem bilgileri

Temel katmanda ve yukarıda, Microsoft, hizmet düzeyi sözleşmeleri (SLA) başına% 99,9 kullanılabilirlik için tüm Azure Bilişsel Arama hizmetlerini izler. Hizmet yavaşsa veya istek işleme SLA eşiklerinin altına düşerse, destek ekipleri, bu dosyaların kullanabildiği günlük dosyalarını gözden geçirir ve sorunu ele geçirebilir.

Azure Bilişsel Arama, dizin oluşturma ve sorgu etkinliğini toplamak ve depolamak için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/) 'yi kullanır. Tek başına bir arama hizmeti, yalnızca içeriğini (dizinler, Dizin Oluşturucu tanımları, veri kaynağı tanımları, Beceri tanımları, eş anlamlı eşlemeler) depolar. Önbelleğe alma ve günlüğe kaydetme bilgileri, genellikle Azure Storage hesabında hizmet dışı olarak depolanır. Dizinleme ve sorgu iş yüklerini günlüğe kaydetme hakkında daha fazla bilgi için bkz. [günlük verilerini toplama ve analiz etme](search-monitor-logs.md).

Yalnızca Azure Bilişsel Arama yerleşik olarak bulunan tesislerin kullanıldığı hizmetiniz hakkındaki genel bilgiler açısından, aşağıdaki yollarla bilgi edinebilirsiniz:

* Hizmet **genel bakış** sayfasını bildirimler, Özellikler ve durum iletileri aracılığıyla kullanma.
* [Hizmet özelliklerini almak](https://docs.microsoft.com/rest/api/searchmanagement/services)için [PowerShell](search-manage-powershell.md) veya [Yönetim REST API](https://docs.microsoft.com/rest/api/searchmanagement/) kullanma. Programlı katmanda yeni bilgi veya işlem sağlanmaz. Arabirimler, komut dosyaları yazmak için mevcuttur.

## <a name="monitor-resource-usage"></a>Kaynak kullanımını izleme

Panoda, kaynak izleme hizmet panosunda gösterilen bilgilerle ve hizmeti sorgulayarak elde ettiğiniz birkaç ölçümle sınırlıdır. Hizmet panosunda, kullanım bölümünde, Bölüm kaynak düzeylerinin uygulamanız için yeterli olup olmadığını hızlı bir şekilde belirleyebilirsiniz. Günlüğe kaydedilen olayları yakalamak ve sürdürmek istiyorsanız, Azure izleme gibi dış kaynakları sağlayabilirsiniz. Daha fazla bilgi için bkz. [Azure bilişsel arama izleme](search-monitor-usage.md).

Arama hizmeti REST API kullanarak belgeler ve dizinlerde program aracılığıyla bir sayı alabilir: 

* [Dizin Istatistiklerini al](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Belge sayısı](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Olağanüstü durum kurtarma ve hizmet kesintileri

Verilerinizi, veri merkezi düzeyinde bir kesinti varsa Azure Bilişsel Arama, hizmetin anında yük devretmesini sağlamaz. Veri merkezinde bir küme başarısız olursa, işlemler ekibi, hizmeti algılar ve geri yükleme işlemini çalışır. Hizmet geri yükleme sırasında kapalı kalma süresi yaşarsınız, ancak [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/)başına hizmet kullanım dışı kalması için hizmet kredileri isteyebilirsiniz. 

Microsoft 'un denetimi dışındaki çok sayıda hata oluşması durumunda sürekli hizmet gerekliyse, farklı bir bölgede [ek bir hizmet](search-create-service-portal.md) sağlayabilir ve dizinlerin tüm hizmetlerde tamamen yedekli olmasını sağlamak için coğrafi çoğaltma stratejisi uygulayabilirsiniz.

Dizinleri doldurmak ve yenilemek için [Dizin oluşturucular](search-indexer-overview.md) kullanan müşteriler, aynı veri kaynağından yararlanan coğrafi olarak özel Dizin oluşturucular aracılığıyla olağanüstü durum kurtarmayı işleyebilir. Her biri Dizin Oluşturucu çalıştıran farklı bölgelerdeki iki hizmet, coğrafi yedeklilik sağlamak için aynı veri kaynağını dizinlede olabilir. Aynı zamanda coğrafi olarak yedekli veri kaynaklarından dizin oluşturuyorsanız, Azure Bilişsel Arama Dizin oluşturucuların birincil çoğaltmalardan yalnızca artımlı dizin oluşturma (yeni, değiştirilen veya silinen belgelerden güncelleştirmeleri birleştirme) gerçekleştirebildiğinizden haberdar olun. Bir yük devretme olayında, Dizin oluşturucuyu yeni birincil çoğaltmaya yeniden işaret ettiğinizden emin olun. 

Dizin oluşturucular kullanmıyorsanız, nesneleri ve verileri farklı arama hizmetlerine paralel olarak göndermek için uygulama kodunuzu kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure bilişsel arama performans ve iyileştirme](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Bilişsel Arama bir birincil veri depolama çözümü olmadığından, self servis yedekleme ve geri yükleme için biçimsel bir mekanizma sağlamayız. Bununla birlikte, Dizin tanımınızı ve anlık görüntüsünü bir dizi JSON dosyasına yedeklemek için bu [Azure bilişsel arama .NET örnek deposu](https://github.com/Azure-Samples/azure-search-dotnet-samples) ' nda **Dizin-yedekleme-geri yükleme** örnek kodunu kullanabilir ve gerekirse dizini geri yüklemek için bu dosyaları kullanabilirsiniz. Bu araç Ayrıca, dizinleri hizmet katmanları arasında taşıyabilir.

Aksi halde, dizin oluşturmak ve doldurmak için kullanılan uygulama kodunuz, yanlışlıkla bir dizini silerseniz geri yükleme ve geri yükleme seçeneğidir. Bir dizini yeniden oluşturmak için (varsa) onu siler, hizmette dizini yeniden oluşturun ve birincil veri deponuzdan verileri alarak yeniden yükleyin.

## <a name="scale-up-or-down"></a>Ölçek büyütme veya küçültme

Her arama hizmeti en az bir çoğaltma ve bir bölüm ile başlar. [Daha fazla kapasiteyi destekleyen bir katmana](search-limits-quotas-capacity.md)kaydolduysanız, kaynak kullanımını ayarlamak için sol gezinti bölmesindeki **Ölçek** ' e tıklayın.

Her iki kaynaktan da kapasite eklediğinizde, hizmet bunları otomatik olarak kullanır. Biraz başka eylem gerekmez, ancak yeni kaynağın etkisinden önce küçük bir gecikme olur. Ek kaynakların sağlanması 15 dakika veya daha uzun sürebilir.

### <a name="add-replicas"></a>Çoğaltmalar ekleme

Saniye başına sorgu (QPS) veya yüksek kullanılabilirlik elde etmek çoğaltmalar eklenerek yapılır. Her çoğaltma bir dizinin kopyasına sahiptir, bu nedenle bir çoğaltma eklemek, hizmet sorgu isteklerini işlemek için kullanılabilir bir dizin daha arar. Yüksek kullanılabilirlik için en az 3 çoğaltma gerekir (bkz. Ayrıntılar için [kapasiteyi ayarlama](search-capacity-planning.md) ).

Daha fazla çoğaltmaya sahip bir arama hizmeti, daha fazla sayıda dizin üzerinde sorgu isteklerinin yükünü dengeleyebilir. Sorgu hacmi düzeyi verildiğinde, isteğin hizmet için kullanılabilir dizinin daha fazla kopyası olduğunda sorgu üretimi daha hızlı olur. Sorgu gecikmesi yaşıyorsanız, ek çoğaltmalar çevrimiçi olduktan sonra performans üzerinde olumlu bir etkisi bekleyebilir.

Yineleme eklerken sorgu işleme yukarı gidebilse de, hizmetinize çoğaltmalar eklerken tam olarak Double veya üçlü değildir. Tüm arama uygulamaları, sorgu performansını engelleyebilecek dış faktörlere tabidir. Karmaşık sorgular ve ağ gecikmesi, sorgu yanıt sürelerindeki çeşitlere katkıda bulunan iki etmendir.

### <a name="add-partitions"></a>Bölüm Ekle

Çoğaltmalar eklemek daha yaygındır, ancak depolama sınırlı olduğunda daha fazla kapasite sağlamak için bölümler ekleyebilirsiniz. Hizmeti sağladığınız katman bölümlerin eklenip eklenemeyeceğini belirler. Temel katman bir bölümde kilitlidir. Standart katmanlar ve yukarıdaki ek bölümler desteklenir.

Bölümler 12 katlarına eklenir (özellikle, 1, 2, 3, 4, 6 veya 12). Bu bir parça yapıtı. Hepsi 1 bölümde depolanabilen veya eşit olarak 2, 3, 4, 6 veya 12 bölüme (bölüm başına bir parça) bölünebilen 12 parçalı bir dizin oluşturulur.

### <a name="remove-replicas"></a>Çoğaltmaları kaldır

Yüksek sorgu birimleri dönemlerinden sonra, arama sorgusu yükleri normalleştirildikten sonra (örneğin, tatil satışları kapatıldıktan sonra) çoğaltmaları azaltmak için kaydırıcıyı kullanabilirsiniz. Sizin bölüminizdeki başka bir adım gerekli değildir. Çoğaltma sayısını azaltmak veri merkezindeki sanal makineleri yeniden oluşturur. Sorgunuz ve veri alma işlemleri artık daha az sayıda VM 'de çalışacak. En düşük gereksinim bir yinelemedir.

### <a name="remove-partitions"></a>Bölümleri kaldır

İş üzerinde fazladan çaba gerektirmeyen çoğaltmaları kaldırmanın aksine, daha fazla depolama kullanıyorsanız daha fazla depolama kullanıyor olabilirsiniz. Örneğin, çözümünüz üç bölüm kullanıyorsa, yeni depolama alanı dizininizi barındırmak için gerekenden küçükse bir veya iki bölüme yeniden boyutlandırma bir hata oluşturur. Tahmin edebileceğiniz gibi seçenekleriniz, alanı boşaltmak için ilişkili bir dizin içindeki dizinleri veya belgeleri siler veya geçerli yapılandırmayı tutacağız.

Belirli bölümlerde hangi dizin parçaları depolandığını belirten bir algılama yöntemi yoktur. Her bölüm, depolama alanı üzerinde yaklaşık 25 GB sağlar, bu nedenle depolama alanını sahip olduğunuz bölüm sayısına göre daha fazla bir boyuta düşürmeniz gerekecektir. Bir bölüme geri dönmek istiyorsanız, tüm 12 parçaların sığması gerekir.

Gelecekteki planlamaya yardımcı olmak için, gerçekten ne kadar kullandığınızı görmek üzere depolamayı ( [Dizin Istatistiklerini al](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)' ı kullanarak) denetlemek isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell](search-manage-powershell.md) ile Otomatikleştir

* [Performans ve iyileştirme](search-performance-optimization.md) tekniklerini gözden geçirin

* İçerik ve işlemleri korumak için [güvenlik özelliklerini](search-security-overview.md) gözden geçirme

* Sorgu ve dizin oluşturma iş yüklerini izlemek için [Tanılama günlüğünü](search-monitor-logs.md) etkinleştirme
