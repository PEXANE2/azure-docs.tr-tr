---
title: Azure Dosya Eşitleme için şirket içi NAS geçişi
description: Şirket içi ağ bağlantılı bir depolama (NAS) konumundan Azure Dosya Eşitleme ve Azure dosya paylaşımlarında karma bulut dağıtımına dosya geçirmeyi öğrenin.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: fd8e845734169bcd73fa0e087c30c0f2fd6ef4f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510314"
---
# <a name="migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Ağ bağlantılı depolamadan (NAS) Azure Dosya Eşitleme ile karma bulut dağıtımına geçiş

Azure Dosya Eşitleme doğrudan bağlı depolama (DAS) konumlarında çalışarak ağa bağlı depolama (NAS) konumlarına eşitlemeyi desteklemez.
Bu olgu, dosyalarınızın bir geçişini yapar ve bu makalede böyle bir geçişin planlanması ve yürütülmesi sırasında size kılavuzluk eder.

## <a name="migration-goals"></a>Geçiş hedefleri

Amaç, NAS gerecinizde sahip olduğunuz paylaşımları bir Windows sunucusuna taşımaktır. Ardından karma bulut dağıtımı için Azure Dosya Eşitleme kullanın. Bu geçişin, üretim verilerinin bütünlüğünü ve geçiş sırasında kullanılabilirliğini garanti eden bir şekilde yapılması gerekir. İkinci olarak, kapalı kalma süresinin en az bir süre içinde tutulması gerekir, böylece normal bakım pencereleri içine sığacak veya yalnızca biraz daha fazla olabilir.

## <a name="migration-overview"></a>Geçişe genel bakış

Azure dosyaları [geçişine genel bakış makalesinde](storage-files-migration-overview.md)belirtildiği gibi, doğru kopyalama aracını ve yaklaşımı kullanmak önemlidir. NAS gereç, SMB paylaşımlarını doğrudan yerel ağınızda kullanıma sunmaktadır. Windows Server 'da bulunan RoboCopy, dosyalarınızı bu geçiş senaryosunda taşımanın en iyi yoludur.

- 1. Aşama: [kaç Azure dosya paylaşımına ihtiyacınız olduğunu belirleme](#phase-1-identify-how-many-azure-file-shares-you-need)
- 2. Aşama: [Şirket içinde uygun bir Windows Server sağlama](#phase-2-provision-a-suitable-windows-server-on-premises)
- 3. Aşama: [Azure dosya eşitleme bulut kaynağını dağıtma](#phase-3-deploy-the-azure-file-sync-cloud-resource)
- 4. Aşama: [Azure depolama kaynaklarını dağıtma](#phase-4-deploy-azure-storage-resources)
- 5. Aşama: [Azure dosya eşitleme aracısını dağıtma](#phase-5-deploy-the-azure-file-sync-agent)
- 6. Aşama: [Windows Server 'da Azure dosya eşitleme yapılandırma](#phase-6-configure-azure-file-sync-on-the-windows-server)
- 7. Aşama: [Robocopy](#phase-7-robocopy)
- 8. Aşama: [Kullanıcı tarafından kesilen](#phase-8-user-cut-over)

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>1. Aşama: kaç Azure dosya paylaşımına ihtiyacınız olduğunu belirleme

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-provision-a-suitable-windows-server-on-premises"></a>2. Aşama: şirket içinde uygun bir Windows Server sağlama

* Bir sanal makine veya fiziksel sunucu olarak en az 2012R2 ' de bir Windows Server 2019 oluşturun. Windows Server yük devretme kümesi de desteklenir.
* Doğrudan bağlı depolama alanı sağlayın veya ekleyin (desteklenmeyen bir şekilde, NAS ile karşılaştırıldığında).

    Azure dosya eşitleme [bulut katmanlama](storage-sync-cloud-tiering.md) özelliğini kullanıyorsanız, sağladığınız depolama alanı miktarı, şu anda NAS gerecinizde kullandığınızdan daha küçük olabilir.
    Ancak, daha sonraki bir aşamada, daha büyük bir NAS alanından dosyalarınızı daha küçük bir Windows Server birimine kopyaladığınızda, toplu iş ' de çalışmanız gerekir:

    1. Diske sığan bir dosya kümesini taşıyın
    2. dosya eşitlemeye ve bulut katmanlamasına izin ver
    3. birimde daha fazla boş alan oluşturulduğunda, sonraki dosya toplu iş ile devam edin. 
    
    Dosyalarınızın NAS gereci üzerinde kaplayacağı Windows Server üzerinde eşdeğer alanı sağlayarak bu toplu işleme yaklaşımına engel olabilirsiniz. NAS/Windows üzerinde Yinelenenleri kaldırmayı göz önünde bulundurun. Bu yüksek miktarda depolama alanını Windows sunucunuza kalıcı olarak kaydetmek istemiyorsanız, geçişten sonra ve bulut katmanlama ilkelerini ayarlamadan önce birim boyutunu azaltabilirsiniz. Bu, Azure dosya paylaşımlarınızın daha küçük bir şirket içi önbelleği oluşturur.

Dağıttığınız Windows Server 'ın kaynak yapılandırması (işlem ve RAM), genellikle eşitlendirilecektir öğe sayısına (dosya ve klasör) bağlıdır. Herhangi bir endişeniz varsa daha yüksek performans yapılandırması yapmanızı öneririz.

[Eşitleme yapmanız gereken öğe sayısına (dosya ve klasör) göre bir Windows Server 'ın nasıl boyutlandıralınacağını öğrenin.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> Daha önce bağlanılan makalede, sunucu belleği (RAM) için bir aralığa sahip bir tablo sunulmaktadır. Sunucunuz için daha küçük bir sayıya yaklaşmayı, ancak ilk eşitlemenin çok daha uzun sürmeyeceğini tahmin edebilirsiniz.

## <a name="phase-3-deploy-the-azure-file-sync-cloud-resource"></a>3. Aşama: Azure Dosya Eşitleme bulut kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-4-deploy-azure-storage-resources"></a>4. Aşama: Azure depolama kaynaklarını dağıtma

Bu aşamada, 1. Aşama ' daki eşleme tablosuna başvurun ve bunların içinde doğru sayıda Azure depolama hesabı ve dosya paylaşımı sağlamak için bunu kullanın.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-5-deploy-the-azure-file-sync-agent"></a>5. Aşama: Azure Dosya Eşitleme aracısını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-6-configure-azure-file-sync-on-the-windows-server"></a>6. Aşama: Windows Server 'da Azure Dosya Eşitleme yapılandırma

Kayıtlı şirket içi Windows Server, bu işlem için önceden ve internet 'e bağlı olmalıdır.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Bulut katmanlaması, yerel sunucunun bulutta depolankıyasla daha az depolama kapasitesine sahip olmasına izin veren, ancak tam ad alanı kullanılabilir olan AFS özelliğidir. Yerel olarak ilginç veriler de hızlı erişim performansı için yerel olarak önbelleğe alınır. Bulut katmanlama, Azure Dosya Eşitleme "sunucu uç noktası" başına isteğe bağlı bir özelliktir.

> [!WARNING]
> Windows Server biriminizde, NAS gereci üzerinde kullanılan verilerinize kıyasla daha az depolama alanı sağladıysanız, bulut katmanlaması zorunludur. Bulut katmanlamayı açmazsanız, sunucunuz tüm dosyaları depolamak için alan boşaltmaz. Geçiş için geçici olarak %99 birim boş alana kadar katmanlama ilkenizi ayarlayın. Geçiş tamamlandıktan sonra bulut katmanlama ayarlarınıza döndiğinizden emin olun ve daha uzun süreli bir kullanışlı düzeye ayarlayın.

Eşitleme grubu oluşturma adımlarını ve eşleşen sunucu klasörünün, eşitleme için yapılandırılması gereken tüm Azure dosya paylaşımları/sunucu konumları için sunucu uç noktası olarak eklenmesi ' nı tekrarlayın.

Tüm sunucu uç noktaları oluşturulduktan sonra eşitleme çalışır. Bir test dosyası oluşturabilir ve sunucu konumunuzda bağlı Azure dosya paylaşımında (eşitleme grubundaki bulut uç noktasında açıklandığı gibi) eşitlemeyi görebilirsiniz.

Her iki konum, sunucu klasörleri ve Azure dosya paylaşımları başka bir yerde boş ve her iki konumda da veri bekliyor. Bir sonraki adımda, dosyaları buluta taşımak için Azure Dosya Eşitleme Windows Server 'a kopyalamaya başlayacaksınız. Bulut katmanlamayı etkinleştirdiyseniz, sunucu daha sonra katman dosyalarına başlar, ancak yerel birimde kapasiteniz tükenmelidir.

## <a name="phase-7-robocopy"></a>7. Aşama: RoboCopy

Temel geçiş yaklaşımı, NAS gerecinizden Windows sunucunuza yönelik bir RoboCopy ve Azure dosya paylaşımlarına Azure Dosya Eşitleme.

İlk yerel kopyayı Windows Server hedef klasörünüze çalıştırın:

* NAS gerecinizdeki ilk konumu belirler.
* Windows Server 'da zaten Azure Dosya Eşitleme yapılandırılmış olan eşleşen klasörü belirler.
* RoboCopy kullanarak kopyayı başlatma

Aşağıdaki RoboCopy komutu, dosyaları NAS depolamadan Windows Server hedef klasörünüze kopyalayacaktır. Windows Server onu Azure dosya paylaşımıyla eşitler. 

Windows Server 'da dosyalarınızın NAS gerecinden daha az depolama alanı sağladıysanız, bulut katmanlaması yapılandırmış olursunuz. Yerel Windows Server birimi tam aldığından, [bulut katmanlaması](storage-sync-cloud-tiering.md) , zaten zaten eşitlenmiş olan ve katman dosyalarını açacaktır. Bulut katmanlaması, bir kopyanın NAS gerecinden devam etmesi için yeterli alan oluşturacak. Bulut katmanlaması, %99 birim boş alana ulaşmak için ne eşitlendiğini ve disk alanını boşaltmak için bir saatte bir kez kontrol eder.
Bu, RoboCopy, buluta ve katmana yerel olarak eşitlenebilmeniz ve bu nedenle yerel disk alanı tükendiğinden dosyaları daha hızlı taşıbilirler. RoboCopy başarısız olacak. Paylaşımlar üzerinde bunu önleyen bir sırayla çalışmanız önerilir. Örneğin, aynı anda tüm paylaşımlar için RoboCopy işlerini başlatmaktan veya yalnızca Windows Server 'daki geçerli boş alan miktarına uygun olan paylaşımları birkaç kez bahsetmez.

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arka plan

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      RoboCopy 'nin çoklu iş parçacıklı çalıştırmasına izin verir. Varsayılan 8 ' dir, Max 128 ' dir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:\<file name\>
   :::column-end:::
   :::column span="1":::
      GÜNLÜK dosyasının durumunu UNICODE olarak çıkış (varolan günlüğün üzerine yaz).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /T
   :::column-end:::
   :::column span="1":::
      Konsol penceresine çıkış. Günlük dosyasına çıktılarla birlikte kullanılır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      RoboCopy, bir yedekleme uygulamasının kullanacağı modda çalıştırır. RoboCopy 'nin geçerli kullanıcının izinleri olmayan dosyaları taşımasına izin verir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MıR
   :::column-end:::
   :::column span="1":::
      Aynı hedefte/hedefte sırayla bu RoboCopy komutunu birkaç kez çalıştırmaya izin verir. Ne önce kopyalanmış olduğunu tanımlar ve onu atlar. Yalnızca değişiklikler, ekler ve "*silmeler*" işlenir. Bu, son çalıştırmasından bu yana yapılır. Komut daha önce çalıştırılmadıysa hiçbir şey atlanamaz. */MIR* bayrağı, hala etkin olarak kullanılan ve değişen kaynak konumları için mükemmel bir seçenektir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dosya kopyasının doğruluğu (varsayılan:/COPY: DAT), kopya bayrakları: D = veri, A = öznitelikler, T = zaman damgaları, S = güvenlik = NTFS ACL 'Leri, O = sahip bilgileri, U = denetim bilgileri
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Tüm dosya bilgilerini kopyala (/COPY: DATSOU ile eşdeğer)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dizinlerin kopyasına uygunluk (varsayılan:/DCOPY: DA), kopyalama bayrakları: D = veri, A = öznitelikler, T = zaman damgaları
   :::column-end:::
:::row-end:::

## <a name="phase-8-user-cut-over"></a>8. Aşama: Kullanıcı tarafından kesilen

RoboCopy komutunu ilk kez çalıştırdığınızda, kullanıcılarınız ve uygulamalarınız hala NAS 'daki dosyalara erişiyor ve bu dosyaları değiştirebilir. Bu, RoboCopy bir dizin işledi, bir sonrakine geçer ve ardından kaynak konumdaki (NAS) bir Kullanıcı, bu geçerli RoboCopy çalıştırmasında işlenmeyecek bir dosyayı ekler, değiştirir veya siler. Bu beklenen bir davranıştır.

İlk çalıştırma, Azure Dosya Eşitleme aracılığıyla verilerin toplu olarak Windows sunucunuza ve buluta taşınması ile ilgilidir. Bu ilk kopya, şu değere bağlı olarak uzun zaman alabilir:

* indirme bant genişliğiniz
* karşıya yükleme bant genişliği
* Yerel ağ hızı ve RoboCopy iş parçacıklarının sayısının ne kadar en iyi şekilde eşleştiğini
* RoboCopy ve Azure Dosya Eşitleme tarafından işlenmesi gereken öğe sayısı (dosya ve klasörler)

İlk çalıştırma tamamlandıktan sonra, komutu yeniden çalıştırın.

İkinci kez daha hızlı bitecektir çünkü yalnızca son çalıştırmasından bu yana gerçekleşen değişiklikleri taşıması yeterlidir. Bu ikinci çalıştırma sırasında, yeni değişiklikler birikmeye devam edebilir.

Belirli bir konum için bir RoboCopy için gereken süre miktarının, kapalı kalma süresi için kabul edilebilir bir pencere dahilinde olduğundan memnun olana kadar bu işlemi tekrarlayın.

Kapalı kalma süresini kabul ediyorsanız ve NAS konumunu çevrimdışı duruma getirmek için hazırladınız: Kullanıcı erişimini çevrimdışı duruma getirmek için, paylaşma kökündeki ACL 'Leri değiştirme seçeneğine sahip olursunuz. bu şekilde, kullanıcılar konuma erişemez veya içeriği NAS 'unuzda bu klasörde değiştirilmesini engelleyen herhangi bir uygun adımı alamaz.

Bir son RoboCopy Round çalıştırın. Bu işlem, kaçırılmış olabilecek tüm değişiklikleri seçer.
Bu son adımın ne kadar süreceği, RoboCopy taramasının hızına bağlıdır. Önceki çalıştırmanın ne kadar sürdüğünü ölçerek süreyi tahmin edebilirsiniz (kapalı kalma süresine eşittir).

Windows Server klasöründe bir paylaşma oluşturun ve büyük olasılıkla DFS-N dağıtımınızı bunu işaret etmek üzere ayarlayın. Aynı paylaşma düzeyi izinlerini, NAS SMB paylaşımınızda olduğu gibi ayarladığınızdan emin olun. Kurumsal sınıf etki alanına katılmış bir NAS olsaydıysanız, kullanıcılar Active Directory ' de mevcut olduğu için Kullanıcı SID 'Leri otomatik olarak eşleştirecektir ve RoboCopy dosya ve meta verileri tam uygunlukta kopyalar. NAS 'inizdeki yerel kullanıcıları kullandıysanız, bu kullanıcıları Windows Server yerel kullanıcıları olarak yeniden oluşturmanız ve Windows Server 'a taşınan mevcut SID RoboCopy 'nin yeni, Windows Server yerel kullanıcılarınızın SID 'Lerine eşlenmesi gerekir.

Paylaşımı/paylaşım gruplarını ortak bir köke veya birime geçirmeyi tamamladınız. (1. aşama ile eşleştirmeye bağlı olarak)

Bu kopyaların birkaçını paralel olarak çalıştırmayı deneyebilirsiniz. Tek seferde bir Azure dosya paylaşımının kapsamını işlemeyi öneririz.

> [!WARNING]
> Ortamınızdaki tüm verileri Windows Server 'a taşıdınız ve geçiş işlemi tamamlandıktan sonra: Azure portal ***Tüm*** eşitleme gruplarına geri dönün ve bulut katmanlama birimi boş alan yüzdesi değerini önbellek kullanımına daha uygun bir şekilde ayarlayın, %20 deyin. 

Bulut katmanlama birimi boş alan ilkesi, büyük olasılıkla birden çok sunucu uç noktası ile eşitlenmesi olan bir birim düzeyinde çalışır. Tek bir sunucu uç noktasında boş alanı ayarlamayı unutursanız, eşitleme en kısıtlayıcı kuralı uygulamaya devam eder ve %99 boş disk alanı tutmaya çalışır ve bu da, yerel önbelleğin bekleneceği gibi yapılmadığından bu işlemi gerçekleştirmeyecektir. Amacınız yalnızca nadiren erişilen ve arşiv verileri içeren bir birimin ad alanına sahip olmadığı ve diğer bir senaryo için depolama alanının geri kalanını ayırdığınız durumlar dışında.

## <a name="troubleshoot"></a>Sorun giderme

' De çalıştırabileceğiniz en olası sorun, RoboCopy komutunun Windows Server tarafında *"Volume Full"* ile başarısız olmasına neden olur. Bulut katmanlaması, eşitlenen yerel Windows Server diskinden içerik boşaltmak için saatte bir kez davranır. Amaç, birimdeki %99 boş alana ulaşmaktır.

Eşitleme ilerleme durumu ve bulut katmanlaması için disk alanının serbest olmasına izin verin. Bunu, Windows sunucunuzdaki dosya Gezgini ' nde gözlemleyebilirsiniz.

Windows Server 'da yeterli kullanılabilir kapasite varsa, komutun yeniden çalıştırılması sorunu çözmeyecektir. Bu durumla karşılaşdığınızda hiçbir şey kesilmez ve güvenle ilerlemebilirsiniz. Komutu yeniden çalıştırmanın bir nedeni tek sonucudur.

Sorun giderme Azure Dosya Eşitleme sorunları için aşağıdaki bölümdeki bağlantıyı denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosya paylaşımları ve Azure Dosya Eşitleme hakkında daha fazla bilgi bulabilirsiniz. Aşağıdaki makalelerde gelişmiş seçenekler, en iyi uygulamalar ve ayrıca sorun giderme yardımı da yer almanıza yardımcı olur. Bu makaleler, [Azure dosya paylaşma belgelerini](storage-files-introduction.md) uygun şekilde bağlar.

* [AFS genel bakış](https://aka.ms/AFS)
* [AFS dağıtım kılavuzu](storage-files-deployment-guide.md)
* [AFS sorunlarını giderme](storage-sync-files-troubleshoot.md)
