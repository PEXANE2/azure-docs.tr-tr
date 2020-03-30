---
title: Azure Dosya Eşitlemi için şirket içi NAS geçişi
description: Dosyaları şirket içi Ağ Bağlı Depolama (NAS) konumundan Azure Dosya Eşitlemi ve Azure dosya paylaşımlarıyla karma bulut dağıtımına nasıl geçirebilirsiniz öğrenin.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 7b0c7a30580d3863a78e85b8b45287a598bbf394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247359"
---
# <a name="migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Ağ Bağlı Depolama 'dan (NAS) Azure Dosya Eşitleme ile karma bulut dağıtımına geçiş

Azure Dosya Eşitlemesi Doğrudan Bağlı Depolama (DAS) konumlarında çalışır ve Ağ Bağlı Depolama (NAS) konumlarına eşitlenmeyi desteklemez.
Bu gerçek, dosyalarınızın geçişini gerekli kılar ve bu makale, böyle bir geçişin planlanması ve yürütülmesi nde size yol gösterecektir.

## <a name="migration-goals"></a>Geçiş hedefleri

Amaç, NAS cihazınızdaki paylaşımları bir Windows Server'a taşımaktır. Ardından, karma bulut dağıtımı için Azure Dosya Eşitleme'yi kullanın. Bu geçiş, geçiş sırasında üretim verilerinin bütünlüğünü ve kullanılabilirliğini garanti eden bir şekilde yapılmalıdır. İkincisi, normal bakım pencerelerine sığabilmesi veya sadece biraz aşabilmesi için kapalı kalma süresini en aza kadar tutmayı gerektirir.

## <a name="migration-overview"></a>Geçişe genel bakış

Azure Dosyaları [geçiş genel bakışı makalesinde](storage-files-migration-overview.md)de belirtildiği gibi, doğru kopyalama aracını ve yaklaşımını kullanmak önemlidir. NAS cihazınız SMB hisselerini doğrudan yerel ağınız üzerinde ifşa ediyor. Yerleşik Windows Server olan RoboCopy, bu geçiş senaryosunda dosyalarınızı taşımanın en iyi yoludur.

- Aşama 1: [Kaç Azure dosyası na ihtiyacınız olduğunu belirleyin](#phase-1-identify-how-many-azure-file-shares-you-need)
- Aşama 2: [Uygun bir Windows Server'ı şirket içinde sağlama](#phase-2-provision-a-suitable-windows-server-on-premises)
- Aşama 3: [Azure Dosya Eşitleme bulut kaynağını dağıtma](#phase-3-deploy-the-azure-file-sync-cloud-resource)
- Aşama 4: [Azure depolama kaynaklarını dağıtma](#phase-4-deploy-azure-storage-resources)
- Aşama 5: [Azure Dosya Eşitleme aracısını dağıtma](#phase-5-deploy-the-azure-file-sync-agent)
- 6. Aşama: [Windows Sunucusunda Azure Dosya Eşitlemeyi'ni Yapılandırma](#phase-6-configure-azure-file-sync-on-the-windows-server)
- Faz 7: [Robocopy](#phase-7-robocopy)
- Faz 8: [Kullanıcı kesme](#phase-8-user-cut-over)

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Aşama 1: Kaç Azure dosyası na ihtiyacınız olduğunu belirleyin

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-provision-a-suitable-windows-server-on-premises"></a>Aşama 2: Uygun bir Windows Server'ı şirket içinde sağlama

* Sanal makine veya fiziksel sunucu olarak en az 2012R2 olarak bir Windows Server 2019 oluşturun. Windows Server hata kümesi de desteklenir.
* Doğrudan Bağlı Depolama (DESTEKLENMEYEn NAS ile karşılaştırıldığında DAS) sağlama veya ekleme.

    Azure Dosya Eşitlemeleri [bulut katmanlama](storage-sync-cloud-tiering.md) özelliğini kullanıyorsanız, sağlama yaptığınız depolama alanı miktarı NAS aygıtınızda şu anda kullandığınızdan daha küçük olabilir.
    Ancak, dosyalarınızı daha sonraki bir aşamada daha büyük NAS alanından daha küçük Windows Server birimine kopyaladiğinizde, toplu olarak çalışmanız gerekir:

    1. Diske sığan bir dosya kümesini taşıma
    2. dosya eşitleme ve bulut katmanlama nın devreye salet
    3. birimde daha fazla boş alan oluşturulduğunda, bir sonraki dosya toplu dosyasına devam edin. 
    
    Dosyalarınızın NAS cihazında kapladığını Windows Server'da eşdeğer alanı sağlayarak bu toplu işlem yaklaşımını önleyebilirsiniz. NAS / Windows'da deduplication düşünün. Bu yüksek miktarda depolama alanını Windows Server'ınıza kalıcı olarak işlemek istemiyorsanız, geçişten sonra ve bulut katmanlama ilkelerini ayarlamadan önce ses boyutunu küçültebilirsiniz. Bu, Azure dosya paylaşımlarınızın şirket içi daha küçük bir önbelleğini oluşturur.

Dağıttığınız Windows Server'ın kaynak yapılandırması (işlem ve RAM) çoğunlukla eşitlediğiniz öğe (dosya ve klasör) sayısına bağlıdır. Herhangi bir endişeniz varsa daha yüksek bir performans yapılandırması ile devam öneririz.

[Bir Windows Server'ı eşitlemeniz gereken öğe (dosya ve klasörler) sayısına göre nasıl boyutlandırabileceğinizi öğrenin.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> Daha önce bağlanan makale, sunucu belleği (RAM) için bir aralık içeren bir tablo sunar. Sunucunuz için daha küçük sayıya yönelebilirsiniz, ancak ilk eşitlemenin önemli ölçüde daha fazla zaman almasını bekleyebilirsiniz.

## <a name="phase-3-deploy-the-azure-file-sync-cloud-resource"></a>Aşama 3: Azure Dosya Eşitleme bulut kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-4-deploy-azure-storage-resources"></a>Aşama 4: Azure depolama kaynaklarını dağıtma

Bu aşamada, Aşama 1'deki eşleme tablosuna başvurun ve bunları doğru sayıda Azure depolama hesabı ve dosya paylaşımı sağlamak için kullanın.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-5-deploy-the-azure-file-sync-agent"></a>Aşama 5: Azure Dosya Eşitleme aracısını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-6-configure-azure-file-sync-on-the-windows-server"></a>6. Aşama: Windows Sunucusunda Azure Dosya Eşitlemeyi'ni Yapılandırma

Kayıtlı şirket içi Windows Server'ınız bu işlem için hazır ve internete bağlı olmalıdır.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Bulut katmanlama, yerel sunucunun bulutta depolanandan daha az depolama kapasitesine sahip olmasına ve ancak tam ad alanına sahip olmasına olanak tanıyan AFS özelliğidir. Yerel olarak ilginç veriler de hızlı erişim performansı için yerel olarak önbelleğe aledilir. Bulut katmanlama, Azure Dosya Eşitleme "sunucu bitiş noktası" başına isteğe bağlı bir özelliktir.

> [!WARNING]
> Windows sunucu hacminizde NAS aygıtında kullandığınız verilerden daha az depolama alanı sağlarsanız, bulut katmanlama zorunludur. Bulut katmanlamayı açmazsanız, sunucunuz tüm dosyaları depolamak için yer açmaz. Geçiş için geçici olarak katmanlama ilkenizi %99 hacimboş alana ayarlayın. Geçiş tamamlandıktan sonra bulut katmanlama ayarlarınıza geri dönüp daha uzun vadeli bir yararlı düzeye ayarladıktan emin olun.

Eşitleme grubu oluşturma adımlarını ve eşitleme için yapılandırılması gereken tüm Azure dosya paylaşımları / sunucu konumları için eşleşen sunucu klasörünün sunucu bitiş noktası olarak eklenmesiadımlarını yineleyin.

Tüm sunucu uç noktaları oluşturulduktan sonra eşitleme çalışır. Bir test dosyası oluşturabilir ve sunucu konumunuzdan bağlı Azure dosya paylaşımına eşitlediğini görebilirsiniz (eşitleme grubundaki bulut bitiş noktası tarafından açıklandığı gibi).

Her iki konum, sunucu klasörleri ve Azure dosya paylaşımları boştur ve her iki konumda da veri bekliyor. Bir sonraki adımda, dosyaları buluta taşımak için Dosyaları Azure Dosya Eşitlemi için Windows Server'a kopyalamaya başlarsınız. Bulut katmanlamayı etkinleştirdiyseniz, yerel birim(ler) üzerinde kapasiteniz biterse, sunucu dosyaları katmanlandırmaya başlar.

## <a name="phase-7-robocopy"></a>Faz 7: Robocopy

Temel geçiş yaklaşımı, NAS aygıtınızdan Windows Server'ınıza bir RoboCopy ve Azure dosya paylaşımlarına Azure Dosya Eşitlemidir.

İlk yerel kopyayı Windows Server hedef klasörünüze çalıştırın:

* NAS cihazınızın ilk konumunu belirleyin.
* Üzerinde Azure Dosya Eşitlemi yapılandırılan Windows Server'daki eşleşen klasörü tanımlayın.
* RoboCopy kullanarak kopyayı başlatın

Aşağıdaki RoboCopy komutu, NAS depolama alanınızdaki dosyaları Windows Server hedef klasörünüze kopyalar. Windows Server bunu Azure dosya paylaşımı(lar) ile senkronize eder. 

Windows Server'ınızda dosyalarınızın NAS aygıtında olduğundan daha az depolama alanı sağlarsanız, bulut katmanlamayı yapılandırmış sınız demektir. Yerel Windows Server birimi dolduğunda, [bulut katmanlama](storage-sync-cloud-tiering.md) devreye girer ve zaten başarıyla eşitlenmiş dosyaları katmanlar. Bulut katmanlama, NAS cihazından kopyayı devam ettirmeye yetecek kadar alan oluşturur. Bulut katmanlama, neyin senkronize edildiğini görmek ve %99 birim boş alana ulaşmak için disk alanını boşaltmak için saatte bir kontrol eder.
RoboCopy'nin dosyaları bulut ve katmanla yerel olarak eşitlediğinizden daha hızlı hareket ettirerek yerel disk alanının tükenmeolasılığı mümkündür. RoboCopy başarısız olur. Bunu engelleyen bir sırayla paylaşımlar üzerinde çalışmanız önerilir. Örneğin, tüm paylaşımlar için RoboCopy işlerini aynı anda başlatmamak veya yalnızca Windows Server'daki geçerli boş alan miktarına uyan paylaşımları birkaç ı setmek için taşımamak.

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arka plan:

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      RoboCopy'nin çok iş parçacığı çalışmasını sağlar. Varsayılan değer 8, en fazla 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:\<dosya adı\>
   :::column-end:::
   :::column span="1":::
      UNICODE olarak LOG dosyasına durum çıktıları (varolan günlüğün üzerine yazar).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /TEE
   :::column-end:::
   :::column span="1":::
      Konsol penceresine çıkan çıktılar. Bir günlük dosyasına çıktı ile birlikte kullanılır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      RoboCopy'i yedek bir uygulamanın kullanacağı modda çalıştırır. RoboCopy'nin geçerli kullanıcının izinleri olmayan dosyaları taşımasına olanak tanır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MIR
   :::column-end:::
   :::column span="1":::
      Bu RoboCopy komutu birkaç kez, sırayla aynı hedef / hedef üzerinde çalıştırmak için izin verir. Daha önce kopyalanan ları tanımlar ve atlar. Yalnızca değişiklikler, eklemeler ve "*siler*" son çalıştırmadan bu yana gerçekleşen işlenir. Komut daha önce çalıştırılmadığı takdirde, hiçbir şey atlanır. */MIR* bayrağı hala etkin olarak kullanılan ve değişen kaynak konumlar için mükemmel bir seçenektir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      dosya kopyasının doğruluğu (varsayılan /COPY:DAT' dır), kopya bayrakları: D=Data, A=Öznitelikler, T=Zaman damgaları, S=Security=NTFS AKLAR, O=Sahip bilgileri, U=aUditing bilgi
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      TÜM dosya bilgilerini KOPYALA (/COPY:DATSOU'ya eşdeğer)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      dizinlerin kopyası için sadakat (varsayılan dır /DCOPY:DA), kopya bayrakları: D=Data, A=Öznitelikler, T=Zaman damgaları
   :::column-end:::
:::row-end:::

## <a name="phase-8-user-cut-over"></a>Faz 8: Kullanıcı kesme

RoboCopy komutunu ilk kez çalıştırdığınızda, kullanıcılarınız ve uygulamalarınız HALA NAS'taki dosyalara erişiyor ve bunları değiştirme potansiyeline sahip. RoboCopy'nin bir dizini işlemesi, bir sonrakine geçmesi ve kaynak konumdaki (NAS) bir kullanıcının bu geçerli RoboCopy çalışmasında işlenmeyecek bir dosya eklemesi, değiştirmesi veya siler. Bu beklenen bir davranıştır.

İlk çalıştırma, azure dosya eşitlemi aracılığıyla verilerin büyük bir kısmını Windows Server'ınıza ve buluta taşımakla ilgilidir. Bu ilk kopya, bağlı olarak uzun zaman alabilir:

* indirme bant genişliğiniz
* yükleme bant genişliği
* yerel ağ hızı ve RoboCopy iş parçacığı sayısının onunla en uygun şekilde eşleştiğinin sayısı
* RoboCopy ve Azure Dosya Eşitlemi tarafından işlenmesi gereken öğelerin (dosya ve klasörler) sayısı

İlk çalıştırma tamamlandıktan sonra, komutu yeniden çalıştırın.

İkinci kez daha hızlı bitecek, çünkü yalnızca son çalıştırmadan bu yana meydana gelen değişiklikleri taşıması gerekiyor. Bu ikinci çalıştırma sırasında, yine de, yeni değişiklikler birikebilir.

Belirli bir konum için RoboCopy'i tamamlamak için gereken sürenin kapalı kalma süresi için kabul edilebilir bir pencere içinde olduğundan emin olana kadar bu işlemi yineleyin.

Kapalı kalma süresini kabul edilebilir gördüğünüzde ve NAS konumunu çevrimdışı almaya hazır olduğunuzda: Kullanıcı erişimini çevrimdışına almak için, kullanıcıların konuma artık erişemeyeceği veya başka uygun adımlar atamayacak şekilde paylaşım kökündeki ALA'ları değiştirme seçeneğiniz vardır NAS'ınızdaki bu klasörde içeriğin değişmesini önler.

Son bir RoboCopy turu çalıştırın. Gözden kaçırılmış olabilecek her türlü değişikliği alacak.
Bu son adımın ne kadar süreceği, RoboCopy tonu nun hızına bağlıdır. Önceki çalıştırmanın ne kadar sürdüğünü ölçerek süreyi (kapalı kalma sürenize eşittir) tahmin edebilirsiniz.

Windows Server klasöründe bir paylaşım oluşturun ve büyük olasılıkla DFS-N dağıtımınızı buna işaret edecek şekilde ayarlayın. NAS SMB paylaşımınızdaki yle aynı hisse düzeyi izinlerini ayarladıklarından emin olun. Kurumsal sınıf bir etki alanı birleştirilmiş NAS varsa, o zaman kullanıcı SID'ler otomatik olarak kullanıcılar Active Directory ve RoboCopy kopyaları dosyaları ve meta verileri tam sadakat olarak var olarak eşleşir. NAS'ınızda yerel kullanıcıları kullandıysanız, bu kullanıcıları Windows Server yerel kullanıcıları olarak yeniden oluşturmanız ve RoboCopy'nin windows server'ınıza taşındığı yeni Windows Server yerel kullanıcılarının SID'lerine taşındığını haritalanız gerekir.

Bir hisse/hisse grubunu ortak bir kök veya hacime dönüştürmeyi bitirdiniz. (Faz 1'deki haritanıza bağlı olarak)

Bu kopyalardan birkaçını paralel olarak çalıştırmayı deneyebilirsiniz. Aynı anda bir Azure dosya paylaşımının kapsamını işleme nizi öneririz.

> [!WARNING]
> Nas'ınızdaki tüm verileri Windows Server'a taşıdıktan ve geçişiniz tamamlandıktan sonra: Azure portalındaki ***tüm*** eşitleme gruplarına dönün ve bulut katmanlama hacmi boş alan yüzdesi değerini önbellek kullanımı için daha uygun bir şeye ayarlayın, örneğin %20. 

Bulut katmanlama birim boş alan ilkesi, birim düzeyinde hareket eder ve bu politikadan eşitleme olarak birden fazla sunucu uç noktası olabilir. Boş alanı tek bir sunucu bitiş noktasında ayarlamayı unutursanız, eşitleme en kısıtlayıcı kuralı uygulamaya devam edecek ve %99 boş disk alanını tutmaya çalışır ve yerel önbelleğin beklediğiniz gibi performans sergilememesini sağlar. Amacınız yalnızca nadiren erişilen, arşiv verileri içeren bir birim için ad alanına sahip olmak olmadığı sürece ve depolama alanının geri kalanını başka bir senaryo için ayırmış sınız.

## <a name="troubleshoot"></a>Sorun giderme

Karşınıza çıkan en olası sorun, RoboCopy komutunun Windows Server tarafında *"Ses tam"* ile başarısız olmasıdır. Bulut katmanlama, senkronize olan yerel Windows Server diskindeki içeriği boşaltmak için her saat bir hareket eder. Amacı hacim üzerinde% 99 boş alana ulaşmaktır.

Eşitleme ilerleme ve bulut katmanlama disk alanı kadar serbest sağlar. Bunu Windows Server'ınızda Dosya Gezgini'nde gözlemleyebilirsiniz.

Windows Server'ınız yeterli kullanılabilir kapasiteye sahipolduğunda, komutu yeniden çalıştırmak sorunu çözecektir. Bu duruma düştüğünde hiçbir şey kırılmaz ve güvenle ilerleyemezsiniz. Komutu tekrar çalıştırmanın sakıncası tek sonuçtur.

Azure Dosya Eşitleme sorunlarını gidermek için aşağıdaki bölümdeki bağlantıyı kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosya paylaşımları ve Azure Dosya Eşitlemi hakkında keşfedilecek daha çok şey var. Aşağıdaki makaleler gelişmiş seçenekleri, en iyi uygulamaları anlamalarına yardımcı olur ve sorun giderme yardımı da içerir. Bu makaleler, azure [dosya paylaşımı belgelerine](storage-files-introduction.md) uygun şekilde bağlantı sağlar.

* [AFS'ye genel bakış](https://aka.ms/AFS)
* [AFS dağıtım kılavuzu](storage-files-deployment-guide.md)
* [AFS sorun giderme](storage-sync-files-troubleshoot.md)
