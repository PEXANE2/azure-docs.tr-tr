---
title: Azure dosyaları dağıtımını planlama | Microsoft Docs
description: Azure dosyaları dağıtımının planlanmasını anlayın. Azure dosya paylaşımından doğrudan bağlama yapabilir veya Azure dosya paylaşımından Azure Dosya Eşitleme ile önbelleğe yazabilirsiniz.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 1/3/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 2e8a2030acd4297ab3032e8f1e3bde5b6df66659
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037176"
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama
[Azure dosyaları](storage-files-introduction.md) , iki ana şekilde dağıtılabilir: doğrudan sunucusuz Azure dosya paylaşımlarını bağlayarak veya Azure dosya eşitleme kullanarak şirket içi Azure dosya paylaşımlarını önbelleğe alarak. Seçtiğiniz dağıtım seçeneği, dağıtımınız için planlarken göz önünde bulundurmanız gereken şeyleri değiştirir. 

- **Azure dosya paylaşımının doğrudan bağlanması**: Azure dosyaları SMB erişimi sağladığından, Windows, MacOS ve Linux 'ta bulunan standart SMB istemcisini kullanarak şirket içinde veya bulutta Azure dosya paylaşımlarını bağlayabilirsiniz. Azure dosya paylaşımları sunucusuz olduğundan, üretim senaryolarına yönelik dağıtım, bir dosya sunucusu veya NAS cihazının yönetilmesini gerektirmez. Bu, yazılım düzeltme ekleri uygulamanız veya fiziksel diskleri takas etmeniz gerekmediği anlamına gelir. 

- **Şirket Içi Azure dosya paylaşımını Azure dosya eşitleme Ile önbelleğe alma**: Azure dosya eşitleme, kuruluşunuzun dosya paylaşımlarını Azure dosyalarında merkezileştirirken şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu mümkün tutmaya olanak sağlar. Azure Dosya Eşitleme, şirket içi (veya bulut) Windows Server 'ı Azure dosya paylaşımınızın hızlı önbelleğine dönüştürür. 

Bu makalede öncelikle bir Azure dosya paylaşımının dağıtımı, şirket içi veya bulut istemcisi tarafından doğrudan bağlanması için dağıtım konuları ele alınmaktadır. Azure Dosya Eşitleme dağıtımı planlamak için, bkz. [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md).

## <a name="management-concepts"></a>Yönetim kavramları
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

Azure dosya paylaşımlarını depolama hesaplarına dağıttığınızda şunları öneririz:

- Azure dosya paylaşımlarını yalnızca diğer Azure dosya paylaşımlarına sahip depolama hesaplarına dağıtma. Azure dosya paylaşımları ve BLOB kapsayıcıları gibi depolama kaynakları depolama hesabının sınırlarını paylaştığı için GPv2 depolama hesapları karma amaçlı depolama hesaplarına izin veriyor olsa da, kaynakları birlikte karıştırarak, daha sonra performans sorunlarını gidermek daha zor olabilir. 

- Azure dosya paylaşımları dağıtımında bir depolama hesabının ıOPS kısıtlamalarına dikkat edin. İdeal olarak, 1:1 dosya paylaşımlarını depolama hesaplarıyla eşlersiniz, ancak bu, hem kuruluşunuzdan hem de Azure 'dan çeşitli sınırlar ve kısıtlamalar nedeniyle her zaman mümkün olmayabilir. Tek bir depolama hesabında yalnızca bir dosya paylaşımının dağıtılması mümkün olmadığında, en son hangi Paylaşımların etkin olacağını ve en yoğun dosya paylaşımlarının aynı depolama hesabına birlikte yerleştirmemesini sağlamak için hangi paylaşımların daha az etkin olacağını düşünün.

- Ortamınızda bulduğunuz GPv2 ve FileStorage hesaplarını dağıtın ve GPv1 ve klasik depolama hesaplarını yükseltin. 

## <a name="identity"></a>Kimlik
Bir Azure dosya paylaşımında erişim sağlamak için dosya paylaşımının kullanıcısının kimliği doğrulanmalıdır ve paylaşıma erişim yetkisi vardır. Bu, dosya paylaşımıyla erişen kullanıcının kimliğine göre yapılır. Azure dosyaları üç ana kimlik sağlayıcısıyla tümleştirilir:
- Şirket **içi Active Directory Domain Services (AD DS veya şirket içi AD DS)** (Önizleme): Azure depolama hesapları, bir Windows Server dosya sunucusu ya da NAS cihazında olduğu gibi, müşteriye ait, Active Directory Domain Services etki alanına katılmış olabilir. Bir etki alanı denetleyicisini şirket içinde, bir Azure VM 'de, hatta başka bir bulut sağlayıcısında VM olarak dağıtabilirsiniz; Azure dosyaları, etki alanı denetleyicinizin barındırıldığı yere belirsiz. Bir depolama hesabı etki alanına katıldıktan sonra, Son Kullanıcı BILGISAYARıNDA oturum açtıkları kullanıcı hesabıyla bir dosya paylaşımından bağlanabilir. AD tabanlı kimlik doğrulaması, Kerberos kimlik doğrulama protokolünü kullanır.
- **Azure Active Directory Domain Services (azure AD DS)**: Azure AD DS, Azure kaynakları Için kullanılabilen Microsoft tarafından yönetilen bir etki alanı denetleyicisi sağlar. Depolama hesabınıza Azure AD DS katılma etki alanı, şirkete ait Active Directory etki alanına katılma gibi benzer avantajlar sağlar. Bu dağıtım seçeneği, AD tabanlı izinleri gerektiren uygulama kaldırma ve kaydırma senaryoları için en yararlı seçenektir. Azure AD DS, AD tabanlı kimlik doğrulaması sağladığından, bu seçenek Kerberos kimlik doğrulama protokolünü de kullanır.
- Azure **depolama hesabı anahtarı**: Azure dosya paylaşımları, bir Azure depolama hesabı anahtarıyla de bağlanabilir. Bir dosya payını bu şekilde bağlamak için, depolama hesabı adı Kullanıcı adı olarak kullanılır ve depolama hesabı anahtarı parola olarak kullanılır. Azure dosya paylaşımının bağlanması için depolama hesabı anahtarını kullanmak, bağlı dosya paylaşımının ACL 'Leri olsa bile paylaşımdaki tüm dosya ve klasörler üzerinde tam izinlere sahip olacağı için etkili bir şekilde bir yönetici işlemidir. SMB üzerinden bağlanacak depolama hesabı anahtarını kullanırken, NTLMv2 kimlik doğrulama protokolü kullanılır.

Şirket içi dosya sunucularından geçiş yapmak veya Azure dosyalarında Windows dosya sunucuları veya NAS gereçleri gibi davranmaya yönelik yeni dosya paylaşımları oluşturmak için, depolama hesabınıza **müşterinin sahip Active Directory olduğu** etki alanına katılma, önerilen seçenektir. Depolama hesabınıza şirkete ait bir Active Directory katılma hakkında daha fazla bilgi edinmek için bkz. [Azure dosyaları Active Directory genel bakış](storage-files-active-directory-overview.md).

Azure dosya paylaşımlarınıza erişmek için depolama hesabı anahtarını kullanmayı düşünüyorsanız, hizmet uç noktalarının [ağ](#networking) bölümünde açıklandığı gibi kullanılması önerilir.

## <a name="networking"></a>Ağ
Azure dosya paylaşımlarına, depolama hesabının genel uç noktası aracılığıyla her yerden erişilebilir. Bu, bir kullanıcının oturum açma kimliği tarafından yetkilendirilmiş istekler gibi kimliği doğrulanmış isteklerin Azure içinden veya dışından güvenli bir şekilde kaynaklanabilmesi anlamına gelir. Birçok müşteri ortamında, Azure VM 'lerinden gelen Mount başarılı olsa bile, şirket içi iş istasyonunuzda Azure dosya paylaşımının ilk bağlanması başarısız olur. Bunun nedeni, SMB 'nin iletişim kurmak için kullandığı bağlantı noktasını pek çok kuruluşun ve Internet hizmet sağlayıcısının (ISS) engellemesini, bağlantı noktası 445 ' dir. 445 numaralı bağlantı noktasından erişime izin veren veya erişimi engelleyen ISP'lerin özetini görmek için [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)'e gidin.

Azure dosya paylaşımınıza erişimi engellemeyi kaldırmak için iki ana seçeneğiniz vardır:

- Kuruluşunuzun şirket içi ağı için 445 bağlantı noktasını engellemeyi kaldırın. Azure dosya paylaşımlarına yalnızca, SMB 3,0 ve FileREST API gibi internet güvenli protokolleri kullanılarak genel uç nokta aracılığıyla dışarıdan erişilebilir. Bu, kuruluşunuzun giden bağlantı noktası kurallarını değiştirmenin ötesinde gelişmiş ağ yapılandırması gerektirmediğinden bu yana şirket içi sunucudan Azure dosya paylaşımınıza erişmenin en kolay yoludur, ancak SMB 1,0 gibi eski ve kullanımdan kaldırılmış olan SMB protokolünün sürümlerini kaldırmanızı öneririz. Bunun nasıl yapılacağını öğrenmek için bkz. [Windows/Windows Server 'ın güvenliğini sağlama](storage-how-to-use-files-windows.md#securing-windowswindows-server) ve [Linux güvenliğini sağlama](storage-how-to-use-files-linux.md#securing-linux).

- Azure dosya paylaşımlarına bir ExpressRoute veya VPN bağlantısı üzerinden erişin. Azure dosya paylaşımınıza bir ağ tüneli üzerinden eriştiğinizde, SMB trafiği kuruluş sınırınızda geçiş yaptığından, Azure dosya paylaşımınızı şirket içi dosya paylaşımında bağlayabilirsiniz.   

Technical perspektifinden, Azure dosya paylaşımlarınızı genel uç nokta aracılığıyla bağlamak çok daha kolay olsa da, çoğu müşterinin Azure dosya paylaşımlarını bir ExpressRoute veya VPN bağlantısı üzerinden bağlamaya karar veririz. Bunu yapmak için, ortamınız için aşağıdakileri yapılandırmanız gerekir:  

- **ExpressRoute, siteden siteye veya noktadan sıteye VPN kullanarak ağ tüneli**: bir sanal ağa tünel oluşturma, 445 bağlantı noktası engellense bile şirket içi Azure dosya paylaşımlarına erişim sağlar.
- **Özel uç**noktalar: özel uç noktalar, depolama hesabınıza sanal ağın adres alanından ayrılmış bir IP adresi sağlar. Bu, Azure depolama kümelerinin sahip olduğu tüm IP adresi aralıklarına kadar şirket içi ağları açmaya gerek kalmadan ağ tünelini sağlar. 
- **DNS iletimi**: ŞIRKET içi DNS 'nizi, `storageaccount.file.core.windows.net` Özel uç noktalarınızın IP adresini çözümlemek üzere depolama hesabınızın adını (genel bulut bölgeleri için) çözümlemek üzere yapılandırın.

Azure dosya paylaşımının dağıtımıyla ilişkili ağı planlamak için bkz. [Azure dosyaları ağ iletişimi konuları](storage-files-networking-overview.md).

## <a name="encryption"></a>Şifreleme
Azure dosyaları iki farklı şifreleme türünü destekler: Azure dosya paylaşımında bağlama/erişim sırasında kullanılan şifrelemeyle ve bekleyen şifreleme ile ilgili olarak, diskte depolandığında verilerin nasıl şifrelendiğine ilişkin şifreleme ile ilişkili olan geçişte şifreleme. 

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Varsayılan olarak, tüm Azure depolama hesaplarının geçiş etkin olarak şifrelenmesi vardır. Bu, SMB üzerinden bir dosya paylaşımının bağladığınızda veya dosyayı dosya paylaşımından (Azure portal, PowerShell/CLı veya Azure SDK 'Ları aracılığıyla) eriştiğinizde, Azure dosyaları yalnızca şifreleme veya HTTPS ile SMB 3.0 + ile yapılırsa bağlantıya izin verir. SMB 3,0 ' i veya SMB 3,0 ' i destekleyen ancak SMB 'yi destekleyen istemcileri desteklemeyen istemciler, aktarım sırasında şifreleme etkinse Azure dosya paylaşımının bağlanabilmesi mümkün olmayacaktır. Şifreleme ile SMB 3,0 ' i destekleyen işletim sistemleri hakkında daha fazla bilgi için bkz. [Windows](storage-how-to-use-files-windows.md), [MacOS](storage-how-to-use-files-mac.md)ve [Linux](storage-how-to-use-files-linux.md)için ayrıntılı Belgelerimiz. PowerShell, CLı ve SDK 'ların tüm geçerli sürümleri HTTPS 'yi destekler.  

Azure depolama hesabı için iletim sırasında şifrelemeyi devre dışı bırakabilirsiniz. Şifreleme devre dışı bırakıldığında, Azure dosyaları da SMB 2,1, şifrelemeden SMB 3,0 ve HTTP üzerinden şifrelenmemiş dosya API çağrıları sağlar. Geçiş sırasında şifrelemeyi devre dışı bırakmak için birincil neden, Windows Server 2008 R2 veya daha eski Linux dağıtımı gibi eski bir işletim sisteminde çalıştırılması gereken eski bir uygulamayı desteklemedir. Azure dosyaları yalnızca Azure dosya paylaşımıyla aynı Azure bölgesi içinde SMB 2,1 bağlantılarına izin verir; Azure dosya paylaşımının Azure bölgesinin dışında bir SMB 2,1 istemcisi, örneğin şirket içi veya farklı bir Azure bölgesinde, dosya paylaşımıyla erişemeyecektir.

Geçiş sırasında verilerin şifrelenmesini güvence altına almanız önemle önerilir.

Aktarım sırasında şifreleme hakkında daha fazla bilgi için bkz. [Azure depolama 'da güvenli aktarım gerektirme](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="data-protection"></a>Veri koruma
Azure dosyaları, verilerinizin yedeklenmesini, kurtarılabilir ve güvenlik tehditlerine karşı korunmasını sağlamaya yönelik çok katmanlı bir yaklaşıma sahiptir.

### <a name="soft-delete"></a>Geçici silme
Dosya paylaşımları için geçici silme (Önizleme), yanlışlıkla silindiğinde dosya paylaşımınızı kurtarmanıza olanak sağlayan bir depolama hesabı düzeyi ayarıdır. Bir dosya paylaşımının silindiği zaman kalıcı olarak silinmeyeceği yerine geçici olarak silinen bir duruma geçer. Geçici olarak silinen verilerin kalıcı olarak silinmeden önce kurtarılabilir kalma süresini yapılandırabilir ve bu bekletme döneminde her zaman paylaşımdan geri alma işlemini geri alabilirsiniz. 

Çoğu dosya paylaşımı için geçici silme özelliğini etkinleştirmenizi öneririz. Paylaşma silmenin yaygın olduğu ve beklenildiği bir iş akışınız varsa, çok kısa bir Bekletme süresine sahip olmak veya hiç geçici silme özelliği etkinleştirilmemiş olabilir.

Geçici silme hakkında daha fazla bilgi için bkz. [yanlışlıkla veri silmeyi engelleme](https://docs.microsoft.com/azure/storage/files/storage-files-prevent-file-share-deletion).

### <a name="backup"></a>Backup
Azure dosya paylaşımınızı, paylaşımınızın salt okunurdur, paylaşılan [anlık görüntüleri](https://docs.microsoft.com/azure/storage/files/storage-snapshots-files)aracılığıyla yedekleyebilirsiniz. Anlık görüntüler artımlı olarak değişir, yani yalnızca önceki anlık görüntüden beri değiştiği kadar veri içerirler. Dosya paylaşımında en fazla 200 anlık görüntü alabilir ve bunları 10 yıla kadar koruyabilirsiniz. Bu anlık görüntüleri, PowerShell veya komut satırı arabirimi (CLı) aracılığıyla Azure portal el ile alabilir ya da [Azure Backup](https://docs.microsoft.com/azure/backup/azure-file-share-backup-overview?toc=/azure/storage/files/toc.json)kullanabilirsiniz. Anlık görüntüler dosya paylaşımınızda saklanır, yani dosya paylaşımınızı silerseniz anlık görüntülerinizin de silinmesi gerekir. Anlık görüntü yedeklemelerinizi yanlışlıkla silinmeye karşı korumak için, paylaşımınızda geçici silme özelliğinin etkinleştirildiğinden emin olun.

[Azure dosya paylaşımları için Azure Backup](https://docs.microsoft.com/azure/backup/azure-file-share-backup-overview?toc=/azure/storage/files/toc.json) , anlık görüntülerin zamanlamasını ve bekletilmesini işler. Babalar ve oğul-son (GFS) özellikleri, her biri kendi ayrı saklama süresine sahip günlük, haftalık, aylık ve yıllık anlık görüntüleri gerçekleştirebileceğiniz anlamına gelir. Azure Backup Ayrıca, içindeki herhangi bir dosya paylaşımının yedekleme için yapılandırıldığı anda, geçici silme işlemini ve bir depolama hesabında silme kilidi alır. Son olarak, Azure Backup müşterilerin yedeklemelerinin birleştirilmiş bir görünümüne sahip olmasını sağlayan belirli önemli izleme ve uyarı özellikleri sağlar.

Azure Backup kullanarak Azure portal hem öğe düzeyinde hem de paylaşma düzeyinde geri yüklemeler gerçekleştirebilirsiniz. Yapmanız gereken tek şey, geri yükleme noktasını (belirli bir anlık görüntü), varsa belirli bir dosyayı veya dizini ve ardından geri yüklemeyi istediğiniz konumu (orijinal veya alternatif) seçmeniz gerekir. Yedekleme hizmeti, anlık görüntü verilerinin kopyalanmasını işler ve portalda geri yükleme ilerleme durumunu gösterir.

Yedekleme hakkında daha fazla bilgi için bkz. [Azure dosya paylaşma yedeklemesi hakkında](https://docs.microsoft.com/azure/backup/azure-file-share-backup-overview?toc=/azure/storage/files/toc.json).

### <a name="advanced-threat-protection-for-azure-files-preview"></a>Azure dosyaları için Gelişmiş tehdit koruması (Önizleme)
Azure depolama için Gelişmiş tehdit koruması (ATP), depolama hesabınızda anormal etkinlik algıladığında uyarılar sağlayan ek bir güvenlik zekası katmanı sağlar. Örneğin, depolama hesabına erişmek için olağan dışı girişimler. ATP Ayrıca kötü amaçlı yazılım karması saygınlığı analizini çalıştırır ve bilinen kötü amaçlı yazılımlara uyar. Bir abonelik veya depolama hesabı düzeyinde ATP 'yi Azure Güvenlik Merkezi aracılığıyla yapılandırabilirsiniz. 

Daha fazla bilgi için bkz. [Azure depolama Için Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection).

## <a name="storage-tiers"></a>Depolama katmanları
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

Genel olarak, Azure dosyaları özellikleri ve diğer hizmetlerle birlikte çalışabilirlik, Premium dosya paylaşımları ve standart dosya paylaşımları arasında aynıdır, ancak bazı önemli farklılıklar vardır:
- **Faturalama modeli**
    - Premium dosya paylaşımları, sağlanan bir faturalandırma modeli kullanılarak faturalandırılır, bu da gerçekte sizin için gereken depolama alanı yerine ne kadar depolama alanı temin ettiğiniz anlamına gelir. 
    - Standart dosya paylaşımları, bir Kullandıkça Öde modeli kullanılarak faturalandırılır. Bu, gerçekten ne kadar depolama alanı tükettiğini belirten temel bir depolama maliyeti ve daha sonra paylaşımı nasıl kullandığınızı temel alarak ek bir işlem maliyeti içerir. Standart dosya paylaşımlarında, Azure dosya paylaşımını daha fazla kullandığınızda (okuma/yazma/bağlama) faturanız artar.
- **Artıklık seçenekleri**
    - Premium dosya paylaşımları yalnızca yerel olarak yedekli (LRS) ve bölge yedekli (ZRS) depolama alanı için kullanılabilir.
    - Standart dosya paylaşımları yerel olarak yedekli, bölgesel olarak yedekli, coğrafi olarak yedekli (GRS) ve coğrafi bölge yedekli (GZRS) depolama için kullanılabilir.
- **Dosya paylaşımının en büyük boyutu**
    - Premium dosya paylaşımları, ek bir iş olmadan 100 TiB 'ye kadar sağlanabilir.
    - Varsayılan olarak, standart dosya paylaşımları yalnızca 5 TiB 'ye yayılabilir, ancak paylaşım sınırı *büyük dosya paylaşımı* depolama hesabı özelliği bayrağına göre 100 TİB 'ye artırılabilir. Standart dosya paylaşımları, yerel olarak yedekli veya bölgesel olarak yedekli depolama hesapları için 100 TiB 'ye kadar yayılabilir. Dosya paylaşımı boyutlarını artırma hakkında daha fazla bilgi için bkz. [etkinleştirme ve büyük dosya paylaşımlarını oluşturma](https://docs.microsoft.com/azure/storage/files/storage-files-how-to-create-large-file-share).
- **Bölgesel kullanılabilirlik**
    - Premium dosya paylaşımları her bölgede kullanılabilir değildir ve bölge yedekli desteği, bölgelerin daha küçük bir alt kümesinde kullanılabilir. Premium dosya paylaşımlarının bölgede şu anda kullanılabilir olup olmadığını öğrenmek için bkz. Azure için [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=storage) sayfası. ZRS 'nin desteklediği bölgeleri öğrenmek için bkz. [bölgeye göre Azure kullanılabilirlik bölge desteği](../../availability-zones/az-region.md). Yeni bölgelerin ve Premium katman özelliklerinin önceliklendirmemize yardımcı olmak için lütfen bu [anketi](https://aka.ms/pfsfeedback)doldurun.
    - Standart dosya paylaşımları her Azure bölgesinde kullanılabilir.
- Azure Kubernetes hizmeti (AKS), sürüm 1,13 ve üzeri sürümlerde Premium dosya paylaşımlarını destekler.

Bir dosya paylaşımının Premium veya standart dosya paylaşımında oluşturulması durumunda, otomatik olarak diğer katmana dönüştüremezsiniz. Başka katmana geçmek istiyorsanız, o katmanda yeni bir dosya paylaşma oluşturmanız ve verileri özgün paylaşımınızdan oluşturduğunuz yeni paylaşıma el ile kopyalamanız gerekir. `robocopy` `rsync` Bu kopyayı gerçekleştirmek için Windows veya MacOS ve Linux için kullanmanızı öneririz.

### <a name="understanding-provisioning-for-premium-file-shares"></a>Premium dosya paylaşımları için sağlamayı anlama
Premium dosya paylaşımları, sabit bir GiB/ıOPS/verimlilik oranına göre sağlanır. Sağlanan her GiB için, paylaşıma tek bir ıOPS ve 0,1 MIB/s aktarım hızı, her bir paylaşıma göre en fazla sınırlara verilecek. İzin verilen en düşük sağlama, minimum ıOPS/aktarım hızı ile 100 GiB 'dir.

En iyi çaba temelinde, tüm paylaşımlar, 60 dakika veya daha uzun bir süre için, paylaşımın boyutuna bağlı olarak, sağlanan depolama alanına göre üç ıOPS 'ye kadar veri alabilir. Yeni paylaşımlar, sağlanan kapasiteye göre tam patlama kredisi ile başlar.

Paylaşımlar 1 GiB artışlarla sağlanmalıdır. Minimum boyut 100 GiB, sonraki boyut 101 GiB ve bu şekilde devam eder.

> [!TIP]
> Taban çizgisi ıOPS = 1 * sağlanan GiB. (En fazla 100.000 ıOPS).
>
> Patlama sınırı = 3 * temel ıOPS. (En fazla 100.000 ıOPS).
>
> çıkış oranı = 60 MIB/s + 0,06 * sağlanan GiB
>
> Giriş oranı = 40 MIB/s + 0,04 * sağlanan GiB

Sağlanan paylaşma boyutu, paylaşma kotası ile belirtilir. Paylaşılan kota herhangi bir zamanda artırılabilir, ancak son artışdan bu yana yalnızca 24 saat sonra azaltılabilir. Kota artışı olmadan 24 saat bekledikten sonra, yeniden artırana kadar, paylaşma kotasını istediğiniz kadar azaltabilirsiniz. IOPS/verimlilik ölçeği değişiklikleri, boyut değişikliğinden sonra birkaç dakika içinde geçerli olacaktır.

Kullandığınız paylaşımın boyutunu, kullanılan GiB 'nizin altında azaltmak mümkündür. Bunu yaparsanız, verileri kaybetmezsiniz, ancak kullanılan boyutun performansını (temel ıOPS, aktarım hızı ve veri bloğu ıOPS) elde edersiniz ve bu boyut kullanılır.

Aşağıdaki tabloda sağlanan paylaşma boyutları için bu formüle birkaç örnek gösterilmektedir:

|Kapasite (GiB) | Temel ıOPS | Veri bloğu ıOPS | Çıkış (MIB/s) | Giriş (MIB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | 300 kadar     | 66   | 44   |
|500         | 500     | 1.500 kadar   | 90   | 60   |
|1.024       | 1.024   | 3.072 kadar   | 122   | 81   |
|5.120       | 5.120   | 15.360 kadar  | 368   | 245   |
|10.240      | 10.240  | 30.720 kadar  | 675 | 450   |
|33.792      | 33.792  | 100.000 kadar | 2.088 | 1.392   |
|51.200      | 51.200  | 100.000 kadar | 3.132 | 2.088   |
|102.400     | 100.000 | 100.000 kadar | 6.204 | 4.136   |

> [!NOTE]
> Dosya paylaşımları performansı, diğer birçok etken arasında makine ağ sınırlarına, kullanılabilir ağ bant genişliğine, GÇ boyutlarına ve paralellik özelliklerine tabidir. Örneğin, 8 KiB okuma/yazma GÇ boyutlarına sahip dahili teste bağlı olarak, SMB üzerinden Premium dosya paylaşımıyla bağlantılı tek bir Windows sanal makinesi, *standart F16s_v2*, 20K Okuma IOPS ve 15K Yazma IOPS elde edebilirler. 512 MIB okuma/yazma GÇ boyutları ile aynı VM, 1,1 GiB/sn çıkış ve 370 MIB/s giriş aktarım hızını elde edebilirler. En yüksek performans ölçeğini elde etmek için, yükü birden çok VM arasında yayın. Bazı yaygın performans sorunları ve geçici çözümler için lütfen [sorun giderme kılavuzuna](storage-troubleshooting-files-performance.md) bakın.

#### <a name="bursting"></a>Patlaması
Premium dosya paylaşımları, ıOPS 'yi üç etmene kadar alabilir. Burdıya otomatik ve bir kredi sistemine göre çalışır. Burdıya en iyi çaba temelinde çalışır ve veri bloğu sınırı bir garanti değildir; dosya paylaşımları sınıra *kadar* veri bloğu alabilir.

Krediler, dosya paylaşımınız için trafik temel ıOPS 'nin altında olduğunda bir patlama demetini biriktir. Örneğin, 100 GiB paylaşımında 100 temel ıOPS vardır. Paylaşımdaki gerçek trafik belirli bir 1 saniyelik Aralık için 40 ıOPS ise 60, kullanılmayan ıOPS, bir patlama demetine alacaklandırılır. Bu krediler daha sonra, işlemler temel IOPS 'yi aştığında kullanılacaktır.

> [!TIP]
> Patlama demetini = taban çizgisi ıOPS * 2 * 3600.

Bir paylaşımın temel ıOPS 'yi aşması ve bir patlama demeti içinde krediler olması durumunda, bu, veri bloğu olur. Krediler kaldığı sürece paylaşımlar aşırı denemeye devam edebilir, ancak 50 TiB 'den küçük paylaşımlar yalnızca bir saate kadar olan patlama sınırında kalır. 50 TiB 'den büyük paylaşımlar, bu saat sınırını en fazla iki saate kadar sürebilir, ancak bu, tahakkuk eden patlama kredilerinin sayısına bağlıdır. Taban çizgisi ıOPS 'nin ötesindeki her GÇ bir kredi kullanır ve tüm krediler tüketildikten sonra paylaşımın taban ıOPS değerine geri döneirdi.

Paylaşılan kredilerin üç durumu vardır:

- Dosya paylaşımının taban çizgisi ıOPS 'den az kullanıldığı durumlarda tahakkuk edin.
- Dosya paylaşımının ne zaman olduğu reddediliyor.
- Kalan sabit, hiçbir kredi yoksa veya temel ıOPS kullanımda olduğunda.

Yeni dosya paylaşımları, veri bloğu demetini içinde tam kredi sayısı ile başlar. Sunucu tarafından azaltma nedeniyle, paylaşılan ıOPS, temel ıOPS 'nin altında olursa patlama kredileri tahakkuk ettirilmeyecektir.

### <a name="enable-standard-file-shares-to-span-up-to-100-tib"></a>Standart dosya paylaşımlarının 100 TiB 'ye kadar yayılmasını sağlar
[!INCLUDE [storage-files-tiers-enable-large-shares](../../../includes/storage-files-tiers-enable-large-shares.md)]

#### <a name="limitations"></a>Sınırlamalar
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="redundancy"></a>Yedeklilik
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

## <a name="migration"></a>Geçiş
Çoğu durumda, kuruluşunuz için bir ağ yeni dosya paylaşımının kurulması ve bunun yerine, mevcut bir dosya paylaşımının şirket içi bir dosya sunucusundan veya NAS cihazından Azure dosyalarına geçirilmesi gerekir. Senaryonuza yönelik doğru geçiş stratejisi ve aracının seçilmesi, geçişinizin başarısı için önemlidir. 

[Geçişe genel bakış makalesi](storage-files-migration-overview.md) , temel bilgileri kısaca kapsar ve senaryonuzu kapsayan geçiş kılavuzlarını size yönlendiren bir tablo içerir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Dosya Eşitleme dağıtımı için planlama yapma](storage-sync-files-planning.md)
* [Azure dosyalarını dağıtma](storage-files-deployment-guide.md)
* [Azure Dosya Eşitleme dağıtma](storage-sync-files-deployment-guide.md)
* [Senaryonuza yönelik geçiş kılavuzunu bulmak için geçişe genel bakış makalesine göz atın](storage-files-migration-overview.md)
