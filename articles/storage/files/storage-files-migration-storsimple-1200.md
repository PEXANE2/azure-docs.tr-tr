---
title: Azure Dosya Eşitlemi için StorSimple 1200 geçişi
description: StorSimple 1200 serisi sanal cihazı Azure Dosya Eşitleme'ye nasıl geçirebilirsiniz öğrenin.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 69225da1506ced879363b10b098d939df93cbfba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502391"
---
# <a name="storsimple-1200-migration-to-azure-file-sync"></a>Azure Dosya Eşitlemi için StorSimple 1200 geçişi

StorSimple 1200 serisi, şirket içi veri merkezinde çalıştırılabilen sanal bir cihazdır. Bu cihazdaki verileri Azure Dosya Eşitleme ortamına geçirmek mümkündür. Azure Dosya Eşitlemi, StorSimple cihazlarının geçirilebileceği varsayılan ve stratejik uzun vadeli Azure hizmetidir.

StorSimple 1200 serisi Aralık 2022'de ömrünü niçin [sona erdirecek.](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%201200%20Series)  Geçişinizi mümkün olan en kısa sürede planlamaya başlamak önemlidir. Bu makalede, Azure Dosya Eşitlemi için başarılı bir geçiş için gerekli arka plan bilgisi ve geçiş adımları sağlar. 

## <a name="azure-file-sync"></a>Azure Dosya Eşitleme

> [!IMPORTANT]
> Microsoft, müşterilerine geçişlerinde yardımcı olmayı taahhüt eder. Özelleştirilmiş bir geçiş planı nın yanı sıra geçiş sırasında yardım için .com adresine e-posta gönderin. AzureFilesMigration@microsoft

Azure Dosya Eşitlemi, iki ana bileşene dayanan bir Microsoft bulut hizmetidir:

* Dosya eşitleme ve bulut katmanlama.
* Dosya paylaşımları Azure'da yerel depolama alanı olarak paylaşArak SMB ve DOSYA REST gibi birden çok protokol üzerinden erişilebilir. Azure dosya paylaşımı, bir Windows Sunucusu'ndaki bir dosya paylaşımıyla karşılaştırılabilir ve bu dosyayı ağ sürücüsü olarak yerel olarak monte edebilirsiniz. Öznitelikler, izinler ve zaman damgaları gibi önemli dosya doğruluğu yönlerini destekler. StorSimple'ın aksine, bulutta depolanan dosya ve klasörleri yorumlamak için uygulama/hizmet gerekmez. Genel amaçlı dosya sunucusu verilerinin yanı sıra bazı uygulama verilerini bulutta depolamak için ideal ve en esnek yaklaşım.

Bu makalede, geçiş adımları üzerinde duruluyor. Azure Dosya Eşitlemi hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makaleleri öneririz:

* [Azure Dosya Eşitleme - genel bakış](https://aka.ms/AFS "Genel Bakış")
* [Azure Dosya Eşitleme - dağıtım kılavuzu](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Geçiş hedefleri

Amaç, üretim verilerinin bütünlüğünü garanti altına almak ve kullanılabilirliği garanti etmektir. İkincisi, normal bakım pencerelerine sığabilmesi veya sadece biraz aşabilmesi için kapalı kalma süresini en aza kadar tutmayı gerektirir.

## <a name="storsimple-1200-migration-path-to-azure-file-sync"></a>Azure Dosya Eşitleme için StorSimple 1200 geçiş yolu

Azure Dosya Eşitleme aracısını çalıştırmak için yerel bir Windows Sunucusu gereklidir. Windows Server en az bir 2012R2 sunucusu olabilir ama ideal bir Windows Server 2019 olduğunu.

Çok sayıda, alternatif geçiş yolları vardır ve bunların hepsini belgelemek ve bu makalede en iyi uygulama olarak önerdiğimiz yol üzerinde neden risk veya dezavantajlar taşıdığını göstermek için çok uzun bir makale oluşturur.

:::image type="content" source="media/storage-files-migration-storsimple-shared/storsimple-1200-migration-overview.png" alt-text="Bu makalede, aşağıdaki adımların geçiş rotasına genel bakış.":::

Önceki resim, bu makaledeki bölümlere karşılık gelen adımları görüntüler.

### <a name="step-1-provision-your-on-premises-windows-server-and-storage"></a>Adım 1: Şirket içi Windows Server ve depolama alanınızı sağlama

1. Sanal makine veya fiziksel sunucu olarak en az 2012R2 olarak bir Windows Server 2019 oluşturun. Windows Server hata kümesi de desteklenir.
2. Doğrudan Bağlı Depolama (DESTEKLENMEYEn NAS ile karşılaştırıldığında DAS) sağlama veya ekleme. Windows Server depolama alanının boyutu, sanal StorSimple 1200 cihazınızın kullanılabilir kapasitesinin boyutuna eşit veya daha büyük olmalıdır.

### <a name="step-2-configure-your-windows-server-storage"></a>Adım 2: Windows Server depolama alanınızı yapılandırın

Bu adımda, StorSimple depolama yapınızı (birimler ve paylaşımlar) Windows Server depolama yapınızla eşlersiniz.
Depolama yapınızda, yani birim sayısında, veri klasörlerinin birimlere ilişkilendirilmesinde veya geçerli SMB/NFS paylaşımlarınızın üstündeki veya altındaki alt klasör yapısında değişiklik yapmayı planlıyorsanız, şimdi bu değişiklikleri dikkate alma zamanıdır.
Azure Dosya Eşitlemi yapılandırıldıktan sonra dosyanızı ve klasör yapınızı değiştirmek hantaldır ve kaçınılmalıdır.
Bu makalede, 1:1 eşleme olduğunu varsayar, bu nedenle bu makalede adımları takip ettiğinizde harita değişiklikleri dikkate almak gerekir.

* Üretim verilerinizin hiçbiri Windows Server sistem hacminde olmamalıdır. Bulut katmanlama sistem birimlerinde desteklenmez. Ancak, bu özellik geçiş için yanı sıra storSimple yerine sürekli işlemler için gereklidir.
* Windows Server'ınızda StorSimple 1200 sanal cihazınızda olduğu kadar çok sayıda birim sağlama.
* İhtiyacınız olan Windows Server rollerini, özelliklerini ve ayarlarını yapılandırın. İşletim sisteminizi güvenli ve güncel tutmak için Windows Server güncelleştirmelerini seçmenizi öneririz. Benzer şekilde, Azure Dosya Eşitleme aracısı da dahil olmak üzere Microsoft uygulamalarını güncel tutmak için Microsoft Update'i seçmenizi öneririz.
* Aşağıdaki adımları okumadan önce herhangi bir klasör veya paylaşım yapılandırmayın.

### <a name="step-3-deploy-the-first-azure-file-sync-cloud-resource"></a>Adım 3: İlk Azure Dosya Eşitleme bulut kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

### <a name="step-4-match-your-local-volume-and-folder-structure-to-azure-file-sync-and-azure-file-share-resources"></a>Adım 4: Yerel birim ve klasör yapınızı Azure Dosya Eşitleme ve Azure dosya paylaşımı kaynaklarıyla eşleştirme

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="step-5-provision-azure-file-shares"></a>Adım 5: Azure dosya paylaşımlarını sağlama

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

### <a name="step-6-configure-windows-server-target-folders"></a>Adım 6: Windows Server hedef klasörlerini yapılandırma

Önceki adımlarda, eşitleme topolojilerinizin bileşenlerini belirleyecek tüm yönleri göz önünde bulundurmuşsunuz. Şimdi, yükleme için dosya almak için sunucu hazırlamak için zamanı.

Her biri kendi Azure dosya paylaşımıyla eşitlenecek **tüm** klasörleri oluşturun.
Daha önce belgelediğiniz klasör yapısını izlemeniz önemlidir. Örneğin, birden çok yerel SMB paylaşımını tek bir Azure dosya paylaşımında eşitleme kararı aldıysanız, bunları birimdeki ortak bir kök klasörün altına yerleştirmeniz gerekir. Bu hedef kök klasörünü şimdi birimde oluşturun.

Oluşturduğunuz Azure dosya paylaşımlarının sayısı, bu adımda oluşturduğunuz klasör sayısı yla + kök düzeyinde eşitlediğiniz birim sayısıyla eşleşmelidir.

### <a name="step-7-deploy-the-azure-file-sync-agent"></a>Adım 7: Azure Dosya Eşitleme aracısını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="step-8-configure-sync"></a>Adım 8: Eşitlemeyi yapılandır

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Bulut katmanlamayı açtıktan emin olun!** Yerel sunucunuz, verilerinizin toplam boyutunu StorSimple bulut depolama alanında depolamak için yeterli alana sahip değilse, bu gereklidir. Geçiş için geçici olarak katmanlama ilkenizi %99 hacimboş alana ayarlayın.

Eşitleme grubu oluşturma adımlarını ve eşitleme için yapılandırılması gereken tüm Azure dosya paylaşımları / sunucu konumları için eşleşen sunucu klasörünün sunucu bitiş noktası olarak eklenmesiadımlarını yineleyin.

### <a name="step-9-copy-your-files"></a>Adım 9: Dosyalarınızı kopyalama

Temel geçiş yaklaşımı, StorSimple sanal cihazınızdan Windows Server'ınıza bir RoboCopy ve Azure dosya paylaşımlarına Azure Dosya Eşitlemidir.

İlk yerel kopyayı Windows Server hedef klasörünüze çalıştırın:

* Sanal StorSimple cihazınızın ilk konumunu belirleyin.
* Üzerinde Azure Dosya Eşitlemi yapılandırılan Windows Server'daki eşleşen klasörü tanımlayın.
* RoboCopy kullanarak kopyayı başlatın

Aşağıdaki RoboCopy komutu, StorSimple Azure depolama alanınızdaki dosyaları yerel StorSimple'ınıza geri çağırAcak ve ardından Windows Server hedef klasörüne taşır. Windows Server bunu Azure dosya paylaşımı(lar) ile senkronize eder. Yerel Windows Server birimi dolduğunda, bulut katmanlama devreye girer ve zaten başarıyla eşitlenmiş dosyaları katmanlar. Bulut katmanlama, StorSimple sanal cihazından kopyayı devam etmek için yeterli alan oluşturur. Bulut katmanlama, neyin senkronize edildiğini görmek ve %99 birim boş alana ulaşmak için disk alanını boşaltmak için saatte bir kontrol eder.

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
      /UNILOG:<file name>
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
      Bu RoboCopy komutu birkaç kez, sırayla aynı hedef / hedef üzerinde çalıştırmak için izin verir. Daha önce kopyalanan ları tanımlar ve atlar. Yalnızca değişiklikler, eklemeler ve "*siler*" son çalıştırmadan bu yana gerçekleşen işlenir. Komut daha önce çalıştırılmadığı takdirde, hiçbir şey atlanır. Bu, hala etkin olarak kullanılan ve değişen kaynak konumlar için mükemmel bir seçenektir.
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

RoboCopy komutunu ilk kez çalıştırdığınızda, kullanıcılarınız ve uygulamalarınız hala StorSimple dosya ve klasörlerine erişiyor ve bu komutu değiştirebilir. RoboCopy'nin bir dizini işlemesi, bir sonrakine geçmesi ve kaynak konumdaki (StorSimple) bir kullanıcının bu geçerli RoboCopy çalışmasında işlenmeyecek bir dosya eklemesi, değiştirmesi veya silmiş olması mümkündür. Sorun değil.

İlk çalıştırma, verilerin büyük bir kısmını şirket içi, Windows Server'ınıza ve yedeklemeyi Azure Dosya Eşitlemi aracılığıyla buluta taşımakla ilgilidir. Bu, bağlı olarak, uzun zaman alabilir:

* indirme bant genişliğiniz
* StorSimple bulut hizmetinin geri çağırma hızı
* yükleme bant genişliği
* her iki hizmet tarafından işlenmesi gereken öğelerin (dosya ve klasörler) sayısı

İlk çalıştırma tamamlandıktan sonra, komutu yeniden çalıştırın.

İkinci kez daha hızlı bitecek, çünkü yalnızca son çalıştırmadan bu yana meydana gelen değişiklikleri taşıması gerekiyor. Bu değişiklikler büyük olasılıkla storsimple için zaten yerel, çünkü yeni. Bu, buluttan geri çağırma gereksinimi azaldığı için zamanı daha da azaltır. Bu ikinci çalıştırma sırasında, yine de, yeni değişiklikler birikebilir.

Tamamlanması gereken sürenin kabul edilebilir bir kapalı kalma süresi olduğundan emin olana kadar bu işlemi tekrarlayın.

Kapalı kalma süresini kabul edilebilir gördüğünüzde ve StorSimple konumunu çevrimdışı almaya hazırolduğunuzda, bunu şimdi yapın: Örneğin, hiçbir kullanıcının klasöre erişemaması veya içeriğin bu nda değişmesini engelleyen başka bir uygun adımı atmaması için Kobİ payını kaldırın StorSimple klasörü.

Son bir RoboCopy turu çalıştırın. Bu, gözden kalınmış olabilecek değişiklikleri alacaktır.
Bu son adımın ne kadar süreceği, RoboCopy tonu nun hızına bağlıdır. Önceki çalıştırmanın ne kadar sürdüğünü ölçerek süreyi (kapalı kalma sürenize eşittir) tahmin edebilirsiniz.

Windows Server klasöründe bir paylaşım oluşturun ve büyük olasılıkla DFS-N dağıtımınızı buna işaret edecek şekilde ayarlayın. StorSimple SMB paylaşımınızdakiyle aynı hisse düzeyi izinlerini ayarladıklarından emin olun.

Bir hisse/hisse grubunu ortak bir kök veya hacime dönüştürmeyi bitirdiniz. (Eşlediğiniz ve aynı Azure dosya paylaşımına girmeniz gerektiğine karar verdiğiniz şeye bağlı olarak.)

Bu kopyalardan birkaçını paralel olarak çalıştırmayı deneyebilirsiniz. Aynı anda bir Azure dosya paylaşımının kapsamını işleme nizi öneririz.

> [!WARNING]
> Tüm verileri StorSimple'ınızdan Windows Server'a taşıdıktan ve geçişiniz tamamlandıktan sonra: Azure portalındaki ***tüm*** eşitleme gruplarına dönün ve bulut katmanlama hacmi boş alan yüzdesi değerini önbellek kullanımına daha uygun bir şeye ayarlayın, örneğin %20. 

Bulut katmanlama birim boş alan ilkesi, birim düzeyinde hareket eder ve bu politikadan eşitleme olarak birden fazla sunucu uç noktası olabilir. Boş alanı tek bir sunucu bitiş noktasında ayarlamayı unutursanız, eşitleme en kısıtlayıcı kuralı uygulamaya devam edecek ve %99 boş disk alanını tutmaya çalışır ve yerel önbelleğin beklediğiniz gibi performans sergilememesini sağlar. Amacınız sadece nadiren erişilen arşiv verilerini içeren bir birim için ad alanına sahip olmak değilse.

## <a name="troubleshoot"></a>Sorun giderme

Karşınıza çıkan en olası sorun, RoboCopy komutunun Windows Server tarafında *"Ses tam"* ile başarısız olmasıdır. Bu durumda, indirme hızınız yükleme hızınızdan daha iyidir. Bulut katmanlama, senkronize olan yerel Windows Server diskindeki içeriği boşaltmak için her saat bir hareket eder.

Eşitleme ilerleme ve bulut katmanlama disk alanı kadar serbest sağlar. Bunu Windows Server'ınızda Dosya Gezgini'nde gözlemleyebilirsiniz.

Windows Server'ınız yeterli kullanılabilir kapasiteye sahipolduğunda, komutu yeniden çalıştırmak sorunu çözecektir. Bu duruma düştüğünde hiçbir şey kırılmaz ve güvenle ilerleyemezsiniz. Komutu tekrar çalıştırmanın sakıncası tek sonuçtur.

Diğer Azure Dosya Eşitleme sorunlarıyla da karşınıza çıkabilir.
Olası olmasa da, bu durumda, LINK Azure **Dosya Eşitleme sorun giderme kılavuzuna**bir göz atın.

## <a name="relevant-links"></a>İlgili bağlantılar

Geçiş içeriği:

* [StorSimple 8000 serisi geçiş kılavuzu](storage-files-migration-storsimple-8000.md)

Azure Dosya Eşitleme içeriği:

* [AFS'ye genel bakış](https://aka.ms/AFS)
* [AFS dağıtım kılavuzu](storage-files-deployment-guide.md)
* [AFS sorun giderme](storage-sync-files-troubleshoot.md)
