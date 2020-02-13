---
title: Azure dosyaları dağıtımını planlama | Microsoft Docs
description: Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 203bf584711fbfcfd0baeee8f5e4c7f70d96823b
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77157235"
---
# <a name="planning-for-an-azure-files-deployment"></a>Azure Dosyaları dağıtımı planlama

[Azure dosyaları](storage-files-introduction.md) , bulutta ENDÜSTRI standardı SMB protokolü aracılığıyla erişilebilen tam olarak yönetilen dosya paylaşımları sunar. Azure dosyaları tam olarak yönetildiğinden, üretim senaryolarında dağıtmak bir dosya sunucusu veya NAS cihazını dağıtmaktan ve yönetmekten çok daha kolaydır. Bu makalede, kuruluşunuzda üretim kullanımı için bir Azure dosya paylaşımının dağıtılmasında dikkate alınması gereken konular ele alınmaktadır.

## <a name="management-concepts"></a>Yönetim kavramları

 Aşağıdaki diyagramda Azure dosya yönetimi yapıları gösterilmektedir:

![Dosya Yapısı](./media/storage-files-introduction/files-concepts.png)

* **Depolama Hesabı**: Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) .

* **Paylaşım**: Dosya Depolama paylaşımı Azure’daki bir SMB dosyası paylaşımıdır. Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Hesap sınırsız sayıda paylaşım içerebilir ve bir paylaşım, dosya paylaşımının toplam kapasitesine kadar sınırsız sayıda dosyayı depolayabilirler. Premium ve standart dosya paylaşımlarının toplam kapasitesi 100 TiB 'dir.

* **Dizin:** Dizinlerin isteğe bağlı hiyerarşisi.

* **Dosya**: Paylaşımdaki bir dosya. Dosya boyutu en fazla 1 TiB olabilir.

* **URL biçimi**: Dosya REST protokolüyle yapılan bir Azure dosya paylaşımıyla ilgili istekler için, dosyalar aşağıdaki URL biçimi kullanılarak adreslenebilir:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>Veri erişim yöntemi

Azure dosyaları, verilerinize erişmek için ayrı olarak veya birbirleriyle birlikte kullanabileceğiniz iki, yerleşik, uygun veri erişim yöntemleri sunar:

1. **Doğrudan bulut erişimi**: herhangi bir Azure dosya paylaşımının, sektör standart sunucu ileti bloğu (SMB) protokolü veya Dosya REST API aracılığıyla [Windows](storage-how-to-use-files-windows.md), [MacOS](storage-how-to-use-files-mac.md)ve/veya [Linux](storage-how-to-use-files-linux.md) tarafından bağlanabilir. SMB, paylaşımdaki dosyalara okuma ve yazma işlemleri doğrudan Azure 'daki dosya paylaşımında yapılır. Azure 'daki bir VM tarafından bağlamak için, işletim sistemindeki SMB istemcisinin en az SMB 2,1 ' i desteklemesi gerekir. Örneğin, bir kullanıcının iş istasyonunda olduğu gibi şirket içi bağlamak için, iş istasyonu tarafından desteklenen SMB istemcisinin en az SMB 3,0 (şifreleme ile) desteklemesi gerekir. SMB 'nin yanı sıra, yeni uygulamalar veya hizmetler, yazılım geliştirme için kolay ve ölçeklenebilir bir uygulama programlama arabirimi sağlayan Dosya REST aracılığıyla dosya paylaşımının doğrudan erişimine sahip olabilir.
2. **Azure dosya eşitleme**: Azure dosya eşitleme ile paylaşımlar, şirket Içinde veya Azure 'Da Windows sunucularına çoğaltılabilir. Kullanıcılarınız, bir SMB veya NFS paylaşımından olduğu gibi, Windows Server aracılığıyla dosya paylaşımıyla erişebilirler. Bu, şube senaryosunda olduğu gibi verilerin bir Azure veri merkezinde erişildiği ve değiştirildiği senaryolar için yararlıdır. Veriler, birden fazla şube ofisi gibi birden çok Windows Server uç noktası arasında çoğaltılabilir. Son olarak, veriler Azure dosyaları ile katmanlanmış olabilir, bu nedenle tüm veriler sunucu aracılığıyla erişilebilir olur ancak sunucu, verilerin tam bir kopyasına sahip değildir. Bunun yerine, verileriniz Kullanıcı tarafından açıldığında sorunsuz bir şekilde geri çekilir.

Aşağıdaki tabloda, kullanıcılarınızın ve uygulamalarınızın Azure dosya paylaşımınıza nasıl erişebileceği gösterilmektedir:

| | Doğrudan bulut erişimi | Azure Dosya Eşitleme |
|------------------------|------------|-----------------|
| Hangi protokollerin kullanılması gerekir? | Azure dosyaları SMB 2,1, SMB 3,0 ve Dosya REST API destekler. | Azure dosya paylaşımınıza Windows Server (SMB, NFS, FTPS, vb.) üzerinde desteklenen herhangi bir protokol aracılığıyla erişin |  
| İş yükünüzü nerede çalıştırıyorsunuz? | **Azure 'da**: Azure dosyaları verilerinize doğrudan erişim sağlar. | **Yavaş ağ ile şirket içi**: Windows, Linux ve MacOS istemcileri, yerel bir şirket Içi Windows dosya paylaşımından Azure dosya paylaşımınızın hızlı bir önbelleği olarak bağlanabilir. |
| Hangi ACL düzeyine ihtiyacınız var? | Paylaşma ve dosya düzeyi. | Paylaşma, dosya ve Kullanıcı düzeyi. |

## <a name="data-security"></a>Veri güvenliği

Azure dosyaları, veri güvenliğini sağlamaya yönelik çeşitli yerleşik seçeneklere sahiptir:

* Her iki hat üzeri protokolde şifreleme desteği: SMB 3,0 şifreleme ve HTTPS üzerinden dosya geri kalanı. Varsayılan olarak: 
    * SMB 3,0 şifrelemesini destekleyen istemciler şifreli bir kanal üzerinden veri gönderme ve alma.
    * Şifreleme ile SMB 3,0 ' i desteklemeyen istemciler, şifreleme olmadan SMB 2,1 veya SMB 3,0 üzerinden veri merkezi olarak iletişim kurabilir. SMB istemcilerinin, şifreleme olmadan, SMB 2,1 veya SMB 3,0 üzerinden veri merkezine iletişim kurmasına izin verilmez.
    * İstemciler, HTTP veya HTTPS ile dosya geri kalanı üzerinden iletişim kurabilir.
* Bekleyen şifreleme ([Azure depolama hizmeti şifrelemesi](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): tüm depolama hesapları için depolama HIZMETI ŞIFRELEMESI (SSE) etkin. Rest verileri, tam olarak yönetilen anahtarlarla şifrelenir. Bekleyen şifreleme, depolama maliyetlerini artırmaz veya performansı düşürür. 
* Geçiş sırasında isteğe bağlı şifreli veri gereksinimi: seçildiğinde, Azure dosyaları şifrelenmemiş kanallar üzerinden verilere erişimi reddeder. Özellikle, şifreleme bağlantılarıyla yalnızca HTTPS ve SMB 3,0 kullanılabilir.

    > [!Important]  
    > Verilerin güvenli aktarılmasını istemek, eski SMB istemcilerinin şifreleme ile SMB 3,0 ile iletişim kurmamasına neden olur. Daha fazla bilgi için bkz. [Windows 'A bağlama](storage-how-to-use-files-windows.md), [Linux 'Ta bağlama](storage-how-to-use-files-linux.md)ve [MacOS 'a bağlama](storage-how-to-use-files-mac.md).

En yüksek güvenlik için, her iki şifrelemeyi de her zaman bekleyen bir şekilde etkinleştirmenizi ve verilerinize erişmek için modern istemciler kullandığınızda aktarım sırasında veri şifrelemeyi etkinleştirmenizi kesinlikle öneririz. Örneğin, yalnızca SMB 2,1 ' yi destekleyen bir Windows Server 2008 R2 sanal makinesine bir paylaşma bağlamanız gerekiyorsa, SMB 2,1 şifrelemeyi desteklemediğinden depolama hesabınıza şifrelenmemiş trafiğe izin vermeniz gerekir.

Azure dosya paylaşımınıza erişmek için Azure Dosya Eşitleme kullanıyorsanız, bekleyen verilerin şifrelenmesini gerektirmeden bağımsız olarak, verilerinizi Windows Server 'ınızla eşitlemek için şifreleme ile her zaman HTTPS ve SMB 3,0 kullanacağız.

## <a name="file-share-performance-tiers"></a>Dosya paylaşımının performans katmanları

Azure dosyaları iki performans katmanı sunar: Standart ve Premium.

### <a name="standard-file-shares"></a>Standart dosya paylaşımları

Standart dosya paylaşımları sabit disk sürücüleri (HDD 'Ler) tarafından desteklenir. Standart dosya paylaşımları, genel amaçlı dosya paylaşımları ve geliştirme/test ortamları gibi performans çeşitliliğine daha az duyarlı olan GÇ iş yükleri için güvenilir performans sağlar. Standart dosya paylaşımları yalnızca Kullandıkça Öde faturalandırma modelinde kullanılabilir.

> [!IMPORTANT]
> 5 TiB 'den büyük dosya paylaşımlarını kullanmak istiyorsanız, ekleme adımları ve bölgesel kullanılabilirlik ve kısıtlamalar için [daha büyük dosya paylaşımlarına (Standart katman)](#onboard-to-larger-file-shares-standard-tier) ekleme bölümüne bakın.

### <a name="premium-file-shares"></a>Premium dosya paylaşımları

Premium dosya paylaşımları, katı hal sürücüleri (SSD 'Ler) tarafından desteklenir. Premium dosya paylaşımları, GÇ yoğun iş yükleri için çoğu GÇ işlemleri için tek basamaklı milisaniye içinde tutarlı yüksek performans ve düşük gecikme sağlar. Bu, veritabanları, Web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygun hale getirir. Premium dosya paylaşımları yalnızca sağlanan faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları standart dosya paylaşımlarından ayrı bir dağıtım modeli kullanır.

Azure Backup Premium dosya paylaşımları için kullanılabilir ve Azure Kubernetes hizmeti sürüm 1,13 ve üzeri sürümlerde Premium dosya paylaşımlarını destekler.

Premium dosya paylaşımının nasıl oluşturulduğunu öğrenmek isterseniz, konudaki makalemize bakın: [Azure Premium dosya depolama hesabı oluşturma](storage-how-to-create-premium-fileshare.md).

Şu anda standart bir dosya paylaşımından ve Premium dosya paylaşımında doğrudan dönüştüremezsiniz. Her iki katmana geçmek istiyorsanız, o katmanda yeni bir dosya paylaşma oluşturmanız ve verileri özgün paylaşımınızdan oluşturduğunuz yeni paylaşıma el ile kopyalamanız gerekir. Bunu, Robocopy veya AzCopy gibi Azure dosyaları tarafından desteklenen kopyalama araçlarından herhangi birini kullanarak yapabilirsiniz.

> [!IMPORTANT]
> Premium dosya paylaşımları, depolama hesapları sunan ve bölgelerin daha küçük bir alt kümesindeki ZRS ile birlikte LRS ile kullanılabilir. Premium dosya paylaşımlarının bölgede şu anda kullanılabilir olup olmadığını öğrenmek için bkz. Azure için [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=storage) sayfası. ZRS 'yi destekleyen bölgeler hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../common/storage-redundancy.md).
>
> Yeni bölgelerin ve Premium katman özelliklerinin önceliklendirmemize yardımcı olmak için lütfen bu [anketi](https://aka.ms/pfsfeedback)doldurun.

#### <a name="provisioned-shares"></a>Sağlanan paylaşımlar

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
|500         | 500     | 1\.500 kadar   | 90   | 60   |
|1,024       | 1,024   | 3\.072 kadar   | 122   | 81   |
|5\.120       | 5\.120   | 15.360 kadar  | 368   | 245   |
|10.240      | 10.240  | 30.720 kadar  | 675 | 450   |
|33.792      | 33.792  | 100.000 kadar | 2\.088 | 1\.392   |
|51.200      | 51.200  | 100.000 kadar | 3\.132 | 2\.088   |
|102.400     | 100.000 | 100.000 kadar | 6\.204 | 4\.136   |

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

## <a name="file-share-redundancy"></a>Dosya paylaşma artıklığı

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) kabul ediyorsanız Azure dosyasının şu anda herhangi bir bölgede Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) desteklemediğini bilmeniz gerekir. RA-GRS depolama hesabındaki dosya paylaşımları, GRS hesaplarında göründükleri gibi çalışır ve GRS fiyatları üzerinden ücretlendirilir.

> [!Warning]  
> Azure dosya paylaşımınızı bir GRS depolama hesabında bulut uç noktası olarak kullanıyorsanız, depolama hesabı yük devretmesini başlatmamanız gerekir. Bunun yapılması eşitlemenin çalışmayı durdurmasına neden olur ve yeni katmanlanmış dosyalar söz konusu olduğunda beklenmedik veri kaybına da yol açabilir. Azure bölgesinin kaybedilmesi durumunda, Microsoft, depolama hesabı yük devretmesini Azure Dosya Eşitleme ile uyumlu bir şekilde tetikleyecektir.

Azure Files Premium paylaşımları hem LRS hem de ZRS 'yi destekler, ZRS Şu anda bölgelerin daha küçük bir alt kümesinde mevcuttur.

## <a name="onboard-to-larger-file-shares-standard-tier"></a>Daha büyük dosya paylaşımlarına ekleme (Standart katman)

Bu bölüm yalnızca standart dosya paylaşımları için geçerlidir. Tüm Premium dosya paylaşımları 100 TiB kapasitesinde kullanılabilir.

### <a name="restrictions"></a>Kısıtlamalar

- Büyük dosya paylaşımları etkin olan herhangi bir depolama hesabı için LRS/ZRS 'den GRS/GZRS hesabı dönüştürme mümkün olmayacaktır.

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

100 TiB kapasite sınırına sahip standart dosya paylaşımları, genel olarak tüm Azure bölgelerinde kullanılabilir.

- LRS: Güney Afrika Kuzey ve Güney Afrika Batı hariç tüm bölgeler.
   - Doğu ABD ve Batı Avrupa: tüm yeni hesaplar desteklenir. Az sayıda mevcut hesap yükseltme işlemini tamamlamamıştı. [Büyük dosya paylaşımlarını etkinleştirmeye](storage-files-how-to-create-large-file-share.md)çalışırken, mevcut depolama hesaplarınızın yükseltme işlemini tamamlayıp tamamlamadığınızı kontrol edebilirsiniz.

- ZRS: Japonya Doğu, Kuzey Avrupa, Güney Afrika Kuzey dışındaki tüm bölgeler.
- GRS/GZRS: desteklenmiyor.

### <a name="enable-and-create-larger-file-shares"></a>Daha büyük dosya paylaşımları etkinleştirin ve oluşturun

Daha büyük dosya paylaşımlarını kullanmaya başlamak için, [büyük dosya paylaşımlarını etkinleştirme ve oluşturma](storage-files-how-to-create-large-file-share.md)makalemize bakın.

## <a name="data-growth-pattern"></a>Veri büyüme kriteri

Bugün, bir Azure dosya paylaşımının en büyük boyutu 100 TiB 'dir. Bu geçerli sınırlama nedeniyle, bir Azure dosya paylaşımından dağıtım yaparken beklenen verilerin büyümesini göz önünde bulundurmanız gerekir.

Birden çok Azure dosya paylaşımını Azure Dosya Eşitleme ile tek bir Windows dosya sunucusu ile eşitlemek mümkündür. Bu, şirket içinde sahip olduğunuz eski, büyük dosya paylaşımlarının Azure Dosya Eşitleme olarak getirilebilir olmasını güvence altına almanıza olanak tanır. Daha fazla bilgi için bkz. [Azure dosya eşitleme dağıtım planlaması](storage-files-planning.md).

## <a name="data-transfer-method"></a>Veri aktarımı yöntemi

Şirket içi dosya paylaşımından mevcut bir dosya paylaşımından verileri Azure dosyalarına toplu olarak aktarmaya yönelik birçok kolay seçenek vardır. Popüler olanlar şunlardır: (ayrıntılı olmayan liste):

* **[Azure dosya eşitleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)** : bir Azure dosya paylaşımıyla ("bulut uç noktası") ve bir Windows Dizin ad alanı ("sunucu uç noktası") arasındaki ilk eşitlemenin bir parçası olarak, Azure dosya eşitleme var olan dosya paylaşımındaki tüm verileri Azure dosyaları 'na çoğaltacaktır.
* **[Azure içeri/dışarı aktarma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : Azure Içeri/dışarı aktarma hizmeti, sabit disk sürücülerinin bir Azure veri merkezine göndererek büyük miktarlardaki verileri bir Azure dosya paylaşımında güvenli bir şekilde aktarmanıza olanak tanır. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)** : Robocopy, Windows ve Windows Server ile birlikte gelen iyi bilinen bir kopyalama aracıdır. Robocopy, dosya paylaşımının yerel olarak bağlanması ve ardından Robocopy komutunda hedef olarak bağlı konumu kullanarak Azure dosyalarına veri aktarmak için kullanılabilir.
* **[AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)** : AzCopy, Azure dosyalarını ve Azure Blob Storage 'a veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır ve en iyi performansla basit komutlar kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Dosya Eşitleme dağıtımı için planlama yapma](storage-sync-files-planning.md)
* [Azure dosyalarını dağıtma](storage-files-deployment-guide.md)
* [Azure Dosya Eşitleme dağıtma](storage-sync-files-deployment-guide.md)
