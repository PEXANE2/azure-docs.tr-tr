---
title: Azure Dosyaları dağıtımı için planlama | Microsoft Dokümanlar
description: Azure Dosyaları dağıtımı planlarken nelere dikkat etmen gerektiğini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 1/3/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 76a96d36387f55889b65f16ea1ca6ec07359c377
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502441"
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama
[Azure Dosyaları](storage-files-introduction.md) iki ana şekilde dağıtılabilir: sunucusuz Azure dosya paylaşımlarını doğrudan monte ederek veya Azure Dosya Eşitlemeyi'ni kullanarak şirket içinde Azure dosya paylaşımlarını önbelleğe alarak. Hangi dağıtım seçeneğini seçtiğiniz, dağıtımınızı planlarken göz önünde bulundurmanız gereken şeyleri değiştirir. 

- **Azure dosya paylaşımının doğrudan bindirilebilirliği**: Azure Dosyaları Kobİ erişimi sağladığından, Windows, macOS ve Linux'ta kullanılabilen standart SMB istemcisini kullanarak Azure dosya paylaşımlarını şirket içinde veya buluta monte edebilirsiniz. Azure dosya paylaşımları sunucusuz olduğundan, üretim senaryoları için dağıtım için bir dosya sunucusu nun veya NAS aygıtının yönetilmesi gerekmez. Bu, yazılım yamaları uygulamak veya fiziksel diskleri değiştirmek zorunda olmadığınız anlamına gelir. 

- **Azure dosya paylaşımını Azure Dosya Eşitleme**: Azure Dosya Eşitlemi ile şirket içi paylaşma, kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmenizi sağlarken, şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu korumanızı sağlar. Azure Dosya Eşitlemi, şirket içi (veya bulut) bir Windows Server'ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. 

Bu makalede, öncelikle bir Azure dosya paylaşımını doğrudan şirket içi veya bulut istemcisi tarafından monte edilecek şekilde dağıtmayla ilgili dağıtım konuları ele alınır. Azure Dosya Eşitleme dağıtımı planlamak [için, Azure Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md)ya da bkz.

## <a name="management-concepts"></a>Yönetim kavramları
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

Azure dosya paylaşımlarını depolama hesaplarına dağıtırken şunları öneririz:

- Azure dosya paylaşımlarını yalnızca diğer Azure dosya paylaşımlarıyla birlikte depolama hesaplarına dağıtma. GPv2 depolama hesapları karışık amaçlı depolama hesaplarına sahip olmaya izin vermesine rağmen, Azure dosya paylaşımları ve blob kapsayıcıları gibi depolama kaynakları depolama hesabının sınırlarını paylaştığından, kaynakları bir araya getirmek sorun gidermenizi zorlaştırabilir daha sonra performans sorunları. 

- Azure dosya paylaşımlarını dağıtırken depolama hesabının IOPS sınırlamalarına dikkat edin. İdeal olarak, dosya paylaşımlarını depolama hesaplarıyla 1:1 eşlersiniz, ancak bu, hem kuruluşunuz hem de Azure'dan çeşitli sınırlar ve kısıtlamalar nedeniyle her zaman mümkün olmayabilir. Bir depolama hesabında yalnızca bir dosya paylaşımının dağıtılması mümkün olmadığında, en büyük dosya paylaşımların aynı depolama hesabına birlikte yerleştirilmediğinden emin olmak için hangi hisselerin son derece etkin olacağını ve hangi hisselerin daha az etkin olacağını düşünün.

- Yalnızca GPv2 ve FileStorage hesaplarını dağıtın ve gpv1 ve klasik depolama hesaplarını ortamınızda bulduğunuzda yükseltin. 

## <a name="identity"></a>Kimlik
Azure dosya paylaşımına erişmek için dosya paylaşımının kullanıcısının kimlik doğrulaması ve paylaşıma erişmek için yetkiye sahip olması gerekir. Bu işlem, dosya paylaşımına erişen kullanıcının kimliğine göre yapılır. Azure Dosyaları üç ana kimlik sağlayıcısıyla tümleşir:
- **Müşteriye ait Active Directory** (önizleme): Azure depolama hesapları, tıpkı windows server dosya sunucusu veya NAS aygıtı gibi müşteriye ait windows server etkin dizininin etki alanına katılabilir. Etkin Dizin Etki Alanı Denetleyiciniz şirket içinde, bir Azure VM'de veya başka bir bulut sağlayıcısında VM olarak dağıtılabilir; Azure Dosyaları, DC'nizin barındırıldığı yere agnostiktir. Bir depolama hesabı etki alanına katıldıktan sonra, son kullanıcı bilgisayarlarında imzaladıkları kullanıcı hesabıyla bir dosya paylaşımı monte edebilir. AD tabanlı kimlik doğrulama, Kerberos kimlik doğrulama protokolünü kullanır.
- **Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS)**: Azure AD DS, Azure kaynakları için kullanılabilecek Microsoft tarafından yönetilen bir Active Directory Etki Alanı Denetleyicisi sağlar. Depolama alanı hesabınızı Azure AD DS'ye katılmak, etki alanının müşteriye ait bir Active Directory'ye katılmasına benzer avantajlar sağlar. Bu dağıtım seçeneği, AD tabanlı izinler gerektiren uygulama kaldırma ve kaydırma senaryoları için en yararlıdır. Azure AD DS AD tabanlı kimlik doğrulaması sağladığından, bu seçenek Kerberos kimlik doğrulama protokolünü de kullanır.
- **Azure depolama hesabı anahtarı**: Azure dosya paylaşımları bir Azure depolama hesabı anahtarıyla da monte edilebilir. Bir dosya paylaşımını bu şekilde takmak için, depolama hesabı adı kullanıcı adı olarak kullanılır ve depolama hesabı anahtarı parola olarak kullanılır. Azure dosya paylaşımını monte etmek için depolama hesabı anahtarını kullanmak etkili bir yönetici işlemidir, çünkü monte edilmiş dosya paylaşımı, ACD'leri olsa bile paylaşımdaki tüm dosya ve klasörlere tam izinverir. SMB'ye monte etmek için depolama hesabı anahtarını kullanırken, NTLMv2 kimlik doğrulama protokolü kullanılır.

Şirket içi dosya sunucularından geçiş yapmak veya Windows dosya sunucuları veya NAS cihazları gibi rol yapmak üzere Azure Dosyalarında yeni dosya paylaşımları oluşturan müşteriler için, depolama alanı hesabınızı **Müşteriye ait Active Directory'ye** katılmak önerilir. Alan adının depolama alanı hesabınıza müşteriye ait bir Active Directory'ye katılması hakkında daha fazla bilgi edinmek için [Azure Dosyaları Etkin Dizini'ne genel bakış](storage-files-active-directory-overview.md)'a bakın.

Azure dosya paylaşımlarınıza erişmek için depolama hesabı anahtarını kullanmayı planlıyorsanız, [Ağ](#networking) bölümünde açıklandığı gibi hizmet bitiş noktalarını kullanmanızı öneririz.

## <a name="networking"></a>Ağ Oluşturma
Azure dosya paylaşımlarına depolama hesabının genel bitiş noktası üzerinden her yerden erişilebilir. Bu, bir kullanıcının oturum açma kimliği tarafından yetkilendirilen istekler gibi kimlik doğrulama isteklerinin Azure'un içinden veya dışından güvenli bir şekilde kaynaklanabileceği anlamına gelir. Birçok müşteri ortamlarında, Azure VM'lerden gelen bağlar başarılı olsa bile, şirket içi iş istasyonunuzdaki Azure dosya paylaşımının ilk yuvası başarısız olur. Bunun nedeni, birçok kuruluş ve internet servis sağlayıcıları (ISS) SMB iletişim kurmak için kullandığı bağlantı noktası, 445 engellemek olduğunu. 445 numaralı bağlantı noktasından erişime izin veren veya erişimi engelleyen ISP'lerin özetini görmek için [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)'e gidin.

Azure dosya paylaşımınızın engelini kaldırmak için iki ana seçeneğiniz var:

- Kuruluşunuzun şirket içi ağı için bağlantı noktası 445'in engelini kaldırın. Azure dosya paylaşımlarına yalnızca SMB 3.0 ve FileREST API gibi internet güvenli protokolleri kullanılarak ortak bitiş noktası üzerinden dışarıdan erişilebilir. Bu, kuruluşunuzun giden bağlantı noktası kurallarını değiştirmenin ötesinde gelişmiş ağ yapılandırması gerektirmediğinden, Azure dosya paylaşımınıza şirket içinde erişmenin en kolay yoludur, ancak SMB'nin eski ve yıkıntılı sürümlerini kaldırmanızı öneririz protokol, yani SMB 1.0. Bunu nasıl yapacağınızı öğrenmek için [Windows/Windows Server'ı güvence altına alma](storage-how-to-use-files-windows.md#securing-windowswindows-server) ve [Linux'u güvence altına alma](storage-how-to-use-files-linux.md#securing-linux)'ya bakın.

- ExpressRoute veya VPN bağlantısı üzerinden Azure dosya paylaşımları erişin. Azure dosya paylaşımınıza bir ağ tüneli üzerinden erişdiğinizde, Kobİ trafiği kuruluş sınırınızdan geçmediği için Azure dosya paylaşımınızı şirket içi dosya paylaşımı gibi monte edebilirsiniz.   

Teknik açıdan bakıldığında Azure dosya paylaşımlarınızı genel bitiş noktası üzerinden monte etmek çok daha kolay olsa da, çoğu müşterinin Azure dosya paylaşımlarını ExpressRoute veya VPN bağlantısı üzerinden takmayı tercih etmesini bekliyoruz. Bunu yapmak için, ortamınız için aşağıdakileri yapılandırmanız gerekir:  

- **ExpressRoute, Site-to-Site veya Point-to-Site VPN kullanarak ağ tünelleme**: 445 bağlantı noktası engellenmiş olsa bile, sanal bir ağa tünel açmak Azure dosya paylaşımlarına şirket içinde erişmesine olanak tanır.
- **Özel uç noktalar**: Özel uç noktalar, depolama hesabınıza sanal ağın adres alanı içinde özel bir IP adresi verir. Bu, şirket içi ağları Azure depolama kümelerinin sahip olduğu TÜM IP adres aralıklarına kadar açmaya gerek kalmadan ağ tünelleme olanağı sağlar. 
- **DNS yönlendirme**: Şirket içi DNS'nizi, depolama hesabınızın adını (yani `storageaccount.file.core.windows.net` genel bulut bölgeleri için) özel uç noktalarınızın IP adresine çözümlemesine göre yapılandırın.

Azure dosya paylaşımıdağıtmayla ilişkili ağ için plan yapmak için [Azure Dosyaları ağ la ilgili hususlara](storage-files-networking-overview.md)bakın.

## <a name="encryption"></a>Şifreleme
Azure Dosyaları iki farklı şifreleme türünü destekler: Azure dosya paylaşımını monte ederken/erişirken kullanılan şifrelemeyle ilgili olan aktarım şifrelemesi ve diskte depolandığında verilerin nasıl şifrelendiğiyle ilgili şifreleme. 

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Varsayılan olarak, tüm Azure depolama hesaplarının aktarım özelliği etkin bir şifrelemeye sahiptir. Bu, bir dosya paylaşımını SMB üzerinden bağladığınızveya FileREST protokolü (Azure portalı, PowerShell/CLI veya Azure SDK'ları gibi) aracılığıyla eriştiğınızda, Azure Dosyaları'nın yalnızca şifreleme veya HTTPS ile SMB 3.0+ ile yapıldığında bağlantıya izin vereceği anlamına gelir. SMB 3.0'ı desteklemeyen istemciler veya SMB 3.0'ı destekleyen ancak Kobİ şifrelemesini desteklemeyen istemciler, aktarımda şifreleme etkinse Azure dosya paylaşımını monte edemez. Hangi işletim sistemlerinin SMB 3.0'ı şifrelemeyle desteklediği hakkında daha fazla bilgi için [Windows,](storage-how-to-use-files-windows.md) [macOS](storage-how-to-use-files-mac.md)ve [Linux](storage-how-to-use-files-linux.md)için ayrıntılı belgelerimize bakın. PowerShell, CLI ve SDK'ların tüm geçerli sürümleri HTTPS'yi destekler.  

Bir Azure depolama hesabı için aktarımda şifrelemeyi devre dışı kullanabilirsiniz. Şifreleme devre dışı bırakıldığında, Azure Dosyaları Ayrıca SMB 2.1, şifreleme olmadan SMB 3.0 ve http üzerinden şifrelenmemiş FileREST API aramaları da izin verir. Aktarımsırasında şifrelemeyi devre dışı bilmelidir, windows server 2008 R2 veya daha eski Linux dağıtımı gibi eski bir işletim sisteminde çalıştırılması gereken eski bir uygulamayı desteklemektir. Azure Dosyaları yalnızca Azure dosya paylaşımıyla aynı Azure bölgesinde sMB 2.1 bağlantılarına izin verir; Azure dosya paylaşımının Azure bölgesi nin dışında bulunan (şirket içi veya farklı bir Azure bölgesi gibi) bir SMB 2.1 istemcisi dosya paylaşımına erişemez.

Aktarım sırasında ki verilerin şifrelemesinin etkin olmasını şiddetle öneririz.

Aktarım sırasında şifreleme hakkında daha fazla bilgi için Azure [depolama alanında güvenli aktarım gerektirmeye](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="storage-tiers"></a>Depolama katmanları
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

Genel olarak, Azure Dosyaları özellikleri ve diğer hizmetlerle birlikte çalışabilirlik, premium dosya paylaşımları ile standart dosya paylaşımları arasında aynıdır, ancak birkaç önemli fark vardır:
- **Faturalama modeli**
    - Premium dosya paylaşımları, sağlanan bir faturalandırma modeli kullanılarak faturalandırılır, bu da gerçekte ne kadar depolama alanı istediğinizyerine ne kadar depolama alanı sağlamanız için ödeme yaptığınız anlamına gelir. 
    - Standart dosya paylaşımları, gerçekte ne kadar depolama alanı tükettiğinize ilişkin temel depolama maliyetini ve ardından payı nasıl kullandığınıza bağlı olarak ek bir işlem maliyeti içeren kullandıkça öde modeli kullanılarak faturalandırılır. Standart dosya paylaşımlarıyla, Azure dosyasını daha fazla kullanırsanız (okuma/yazma/monte edin) faturanız artar.
- **Artıklık seçenekleri**
    - Premium dosya paylaşımları yalnızca yerel olarak yedekli (LRS) ve bölge yedekli (ZRS) depolama için kullanılabilir. 
    - Standart dosya paylaşımları yerel olarak yedekli, bölge yedekli, coğrafi yedekli (GRS) ve coğrafi bölge yedekli (GZRS) depolama için kullanılabilir.
- **Dosya paylaşımının maksimum boyutu**
    - Premium dosya paylaşımları herhangi bir ek çalışma olmaksızın 100 TiB'ye kadar kullanılabilir.
    - Varsayılan olarak, standart dosya paylaşımları yalnızca 5 TiB'ye kadar yayılabilir, ancak paylaşım sınırı *büyük dosya paylaşımı* depolama hesabı özellik bayrağına girerek 100 TiB'ye yükseltilebilir. Standart dosya paylaşımları, yerel olarak yedekveya bölge gereksiz depolama hesapları için yalnızca 100 TiB'ye kadar yayılabilir. Dosya paylaşımı boyutlarını artırma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-files-how-to-create-large-file-share)
- **Bölgesel kullanılabilirlik**
    - Premium dosya paylaşımları her bölgede kullanılamaz ve bölge yedek desteği daha küçük bir bölge alt kümesinde kullanılabilir. Premium dosya paylaşımlarının bölgenizde şu anda kullanılabilir olup olmadığını öğrenmek için Azure için bölge sayfasına [göre kullanılabilen ürünlere](https://azure.microsoft.com/global-infrastructure/services/?products=storage) bakın. Hangi bölgelerin ZRS'yi desteklediğini öğrenmek için [bölgeye göre Azure Kullanılabilirlik Bölgesi desteğine](../../availability-zones/az-overview.md#services-support-by-region)bakın. Yeni bölgelere ve premium katman özelliklerine öncelik vermemize yardımcı olmak için lütfen bu [anketi](https://aka.ms/pfsfeedback)doldurun.
    - Standart dosya paylaşımları her Azure bölgesinde kullanılabilir.
- Azure Kubernetes Service (AKS), sürüm 1.13 ve sonraki sürümdeki premium dosya paylaşımlarını destekler.

Dosya paylaşımı premium veya standart dosya paylaşımı olarak oluşturulduktan sonra, dosyayı otomatik olarak diğer katmana dönüştüremezsiniz. Diğer katmana geçmek istiyorsanız, bu katmanda yeni bir dosya paylaşımı oluşturmanız ve orijinal paylaşımınızdaki verileri oluşturduğunuz yeni paylaşıma el ile kopyalamanız gerekir. Bu kopyayı gerçekleştirmek `rsync` için Windows veya macOS ve Linux için kullanmanızı `robocopy` öneririz.

### <a name="understanding-provisioning-for-premium-file-shares"></a>Premium dosya paylaşımları için sağlamanın anlaşılması
Premium dosya payları sabit Bir GİB/IOPS/iş ortası oranına göre sağlanır. Sağlanan her GİB için, hisse başına maksimum limitlere kadar bir IOPS ve 0,1 MiB/s iş çıkarı verilecektir. İzin verilen minimum sağlama, min IOPS/iş başı olan 100 GiB'dir.

En iyi çaba temelinde, tüm hisseler, payın boyutuna bağlı olarak 60 dakika veya daha uzun süre verilen depolama nın GiB başına üç IOPS'ye kadar patlaması na neden olabilir. Yeni hisseler, sağlanan kapasiteye dayalı tam seri kredi ile başlar.

Hisseler 1 GİB artışlarında sağlanmalıdır. Minimum boyutu 100 GiB, sonraki boyutu 101 GiB ve benzeri.

> [!TIP]
> Temel IOPS = 1 * gib hükmü. (En fazla 100.000 IOPS).
>
> Patlama Limiti = 3 * Temel IOPS. (En fazla 100.000 IOPS).
>
> çıkış oranı = 60 MiB/s + 0,06 * karşılıklı GİB
>
> giriş oranı = 40 MiB/s + 0,04 * verilen GiB

Verilen hisse boyutu hisse kotası ile belirtilir. Hisse kotası herhangi bir zamanda artırılabilir, ancak son artıştan bu yana sadece 24 saat sonra azaltılabilir. Kota artışı olmadan 24 saat bekledikten sonra, hisse kotasını tekrar artırana kadar istediğiniz kadar azaltabilirsiniz. IOPS/İş-İş cetveli değişiklikleri boyut değişikliğinden sonra birkaç dakika içinde etkili olacaktır.

Verilen payınızın boyutunu kullanılmış GİB'inizin altında küçültmek mümkündür. Bunu yaparsanız, veri kaybetmezsiniz, ancak yine de kullanılan boyut için faturalandırılır ve sağlanan payın performansını (temel IOPS, iş çıkışı ve seri IOPS) alırsınız, kullanılan boyutu değil.

Aşağıdaki tablo, sağlanan paylaşım boyutları için bu formüllerin birkaç örneğini göstermektedir:

|Kapasite (GİB) | Temel IOPS | Patlama IOPS | Çıkış (MiB/s) | Giriş (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | 300'e kadar     | 66   | 44   |
|500         | 500     | 1.500'e kadar   | 90   | 60   |
|1,024       | 1,024   | 3.072'ye kadar   | 122   | 81   |
|5,120       | 5,120   | 15.360'a kadar  | 368   | 245   |
|10,240      | 10,240  | 30.720'ye kadar  | 675 | 450   |
|33,792      | 33,792  | 100.000'e kadar | 2,088 | 1,392   |
|51,200      | 51,200  | 100.000'e kadar | 3,132 | 2,088   |
|102,400     | 100.000 | 100.000'e kadar | 6,204 | 4,136   |

> [!NOTE]
> Dosya paylaşımları performansı, diğer birçok etkenin yanı sıra makine ağı sınırlarına, kullanılabilir ağ bant genişliğine, IO boyutlarına, paralelliğe tabidir. Örneğin, 8 KiB okuma / Yazma IO boyutları, tek bir Windows sanal makine, *Standart F16s_v2*ile iç test dayalı, SMB üzerinden prim dosya paylaşımıbağlı 20K iOPS okumak ve 15K IOPS yazmak elde edebilirim. 512 MiB IO boyutu okuma/yazma ile aynı VM 1.1 GiB/s çıkış ve 370 MiB/s giriş girişi elde edebilir. Maksimum performans ölçeği elde etmek için yükü birden çok VM'ye yayıltın. Lütfen bazı yaygın performans sorunları ve geçici geçici çözüm için [sorun giderme kılavuzuna](storage-troubleshooting-files-performance.md) bakın.

#### <a name="bursting"></a>Patlama
Premium dosya paylaşımları, IOPS'lerini üç kata kadar patlayabilir. Bursting otomatiktir ve bir kredi sistemine dayalı çalışır. Patlama en iyi çaba temelinde çalışır ve patlama sınırı bir garanti değildir, dosya paylaşımları sonuna *kadar* patlayabilir.

Dosya paylaşımınız için trafik temel IOPS'nin altında olduğunda krediler bir patlama kovasında birikir. Örneğin, 100 GiB payı 100 temel IOPS vardır. Hisse üzerindeki gerçek trafik belirli bir 1 saniyelik aralık için 40 IOPS ise, o zaman 60 kullanılmayan IOPS bir patlama kova yatırılır. Bu krediler daha sonra operasyonlar temel IOPs aşıldığında kullanılacaktır.

> [!TIP]
> Seri çekim kovasının boyutu = Temel IOPS * 2 * 3600.

Bir pay temel IOPS'yi aştığında ve bir seri kovada kredisi olduğunda patlar. Hisseler krediler kaldığı sürece patlamaya devam edebilir, ancak 50 TiB'den küçük hisseler patlama limitinde yalnızca bir saate kadar kalır. 50 TiB'den büyük hisseler teknik olarak bu bir saat sınırını iki saate kadar aşabilir, ancak bu, tahakkuk eden seri kredi sayısına bağlıdır. Temel IOPS ötesinde her IO bir kredi tüketir ve bir kez tüm krediler tüketilen payı temel IOPS dönecektir.

Hisse kredilerinin üç eyaleti vardır:

- Dosya paylaşımı temel IOPS'den daha az kullanıyorsa tahakkuk etme.
- Dosya paylaşımı patladığında azalıyor.
- Kredi veya temel IOPS olmadığında sabit kalır.

Yeni dosya paylaşımları, seri kovasındaki tam kredi sayısıyla başlar. Hisse IOPS sunucu tarafından daraltma nedeniyle temel IOPS altına düşerse Burst kredileri tahakkuk edilmez.

### <a name="enable-standard-file-shares-to-span-up-to-100-tib"></a>Standart dosya paylaşımlarını 100 TiB'e kadar yayılmaya olanak sağlama
[!INCLUDE [storage-files-tiers-enable-large-shares](../../../includes/storage-files-tiers-enable-large-shares.md)]

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="redundancy"></a>Yedeklilik
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

## <a name="migration"></a>Geçiş
Çoğu durumda, kuruluşunuz için yeni bir net dosya paylaşımı oluşturmaz, bunun yerine varolan bir dosya paylaşımını şirket içi dosya sunucusundan veya NAS aygıtından Azure Dosyaları'na geçirebilirsiniz. Hem Microsoft hem de üçüncü taraflarca sağlanan, dosya paylaşımına geçiş yapmak için birçok araç vardır, ancak bunlar kabaca iki kategoriye ayrılabilir:

- **AK'lar ve zaman damgaları gibi dosya sistemi özniteliklerini koruyan araçlar:**
    - **[Azure Dosya Eşitleme](storage-sync-files-planning.md)**: Azure Dosya Eşitlemi, istenen son dağıtım şirket içi varlığı korumak için olmasa bile, verileri Azure dosya paylaşımına almak için bir yöntem olarak kullanılabilir. Azure Dosya Eşitlemi, mevcut Windows Server 2012 R2, Windows Server 2016 ve Windows Server 2019 dağıtımlarında yerinde yüklenebilir. Azure Dosya Eşitlemi'ni bir yutma mekanizması olarak kullanmanın bir avantajı, son kullanıcıların varolan dosya paylaşımını yerinde kullanmaya devam edebiliyor olmasıdır; tüm veriler arka planda yükleme tamamlandıktan sonra Azure dosya paylaşımına kesme oluşabilir.
    - **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy, Windows ve Windows Server ile birlikte gelen tanınmış bir kopyalama aracıdır. Robocopy, dosya paylaşımını yerel olarak monte ederek ve ardından Robocopy komutundaki hedef olarak monte edilen konumu kullanarak verileri Azure Dosyalarına aktarmak için kullanılabilir.

- **Dosya sistemi özniteliklerini korumayan araçlar:**
    - Veri Kutusu : Veri **Kutusu,** verileri Azure'a aktarmak için çevrimdışı veri aktarım mekanizması sağlar. Bu yöntem, iş bilgililiği artırmak ve bant genişliğinden tasarruf etmek için tasarlanmıştır, ancak şu anda zaman damgaları ve ALA'lar gibi dosya sistemi özniteliklerini desteklemez.
    - **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: AzCopy, Azure Dosyalarına ve Azure Blob depolamasına veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır ve en iyi performansa sahip basit komutları kullanır.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Dosya Eşitleme Dağıtımı Planlama](storage-sync-files-planning.md)
* [Azure Dosyalarını Dağıtma](storage-files-deployment-guide.md)
* [Azure Dosya Eşitleme dağıtma](storage-sync-files-deployment-guide.md)
