---
title: Azure geçişi 'nde AVS değerlendirmesi hesaplamaları | Microsoft Docs
description: Azure geçişi hizmetindeki AVS değerlendirmesi hesaplamalarına genel bir bakış sağlar.
author: rashi-ms
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/25/2020
ms.author: mahain
ms.openlocfilehash: a4d2e810144e7c3d36545cb1e965aec40980c1d2
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118826"
---
# <a name="avs-assessments-in-azure-migrate-server-assessment"></a>Azure geçişi 'nde AVS değerlendirmeleri: Sunucu değerlendirmesi

[Azure geçişi](migrate-services-overview.md) , şirket içi uygulamalarınızın ve iş yüklerinizin bulmayı, değerlendirmesini ve geçişini izlemek için bir merkezi Merkez sağlar. Ayrıca, özel ve genel bulut örneklerinizi Azure 'da izler. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sunar.

Sunucu değerlendirmesi, Azure 'da değerlendirir şirket içi sunucuların Azure IaaS sanal makinelerine ve Azure VMware çözümüne (AVS) geçiş için geçişini sağlayan bir araçtır. Bu makalede, Azure VMware çözümü (AVS) değerlendirmelerinin nasıl hesaplandığı hakkında bilgi sağlanır.

> [!NOTE]
> Azure VMware çözümü (AVS) değerlendirmesi Şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için oluşturulabilir.

## <a name="types-of-assessments"></a>Değerlendirme türleri

Sunucu değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin bir zaman noktası anlık görüntüsüdür. Azure geçişi: Sunucu değerlendirmesi kullanarak oluşturabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme türü** | **Ayrıntılar**
--- | --- 
**Azure VM** | Şirket içi sunucularınızı Azure sanal makinelerine geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure 'a geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md)'lerinizi, [Hyper-V sanal](how-to-set-up-appliance-hyper-v.md)makinelerinizi ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) değerlendirebilirsiniz. [Daha fazla bilgi](concepts-assessment-calculation.md)
**Azure VMware Çözümü (AVS)** | Şirket içi sunucularınızı [Azure VMware çözümüne (AVS)](../azure-vmware/introduction.md)geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure VMware çözümüne (AVS) geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md) 'lerinizi değerlendirebilirsiniz. [Daha fazla bilgi](concepts-azure-vmware-solution-assessment-calculation.md)

Sunucu değerlendirmesinde Azure VMware çözümü (AVS) değerlendirmesi iki boyutlandırma ölçütü seçeneği sağlar:

**Değerlendirme** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Şirket içi VM 'lerin toplanan performans verilerine dayanan değerlendirmeler. | **Önerilen düğüm boyutu**: değerlendirme için seçtiğiniz düğüm türü, depolama türü ve FTT ayarı Ile birlikte CPU ve bellek kullanım verilerine göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **Önerilen düğüm boyutu**: değerlendirme için seçtiğiniz düğüm türü, depolama türü ve FTT ayarı ile birlikte ŞIRKET içi VM boyutuna göre.

## <a name="how-do-i-run-an-assessment"></a>Nasıl yaparım? bir değerlendirme çalıştırmak istiyor musunuz?

Bir değerlendirme çalıştırmak için birkaç yol vardır.

- Hafif bir Azure geçişi gereci tarafından toplanan sunucu meta verilerini kullanarak makineleri değerlendirin. Gereç, şirket içi makineleri bulur. Daha sonra, Azure geçişi 'ne makine meta verilerini ve performans verilerini gönderir.
- Bir virgülle ayrılmış değerler (CSV) biçiminde içeri aktarılan sunucu meta verilerini kullanarak makineleri değerlendirin.

## <a name="how-do-i-assess-with-the-appliance"></a>Gereç ile Nasıl yaparım? değerlendirin?

Şirket içi sunucuları bulmaya yönelik bir Azure geçiş gereci dağıtıyorsanız, aşağıdaki adımları uygulayın:

1. Azure 'u ve şirket içi ortamınızı sunucu değerlendirmesi ile çalışacak şekilde ayarlayın.
2. İlk değerlendirmenizi için bir Azure projesi oluşturun ve sunucu değerlendirmesi aracını buna ekleyin.
3. Hafif bir Azure geçişi gereci dağıtın. Gereç, şirket içi makineleri sürekli bulur ve Azure geçişi 'ne makine meta verilerini ve performans verilerini gönderir. Gereci bir VM olarak dağıtın. Değerlendirmek istediğiniz makinelere herhangi bir şey yüklemeniz gerekmez.

Gereç makine bulmayı başlattıktan sonra, değerlendirmek istediğiniz makineleri bir gruba toplayıp değerlendirme türü **Azure VMware çözümü (AVS)** olan grup için bir değerlendirme çalıştırabilirsiniz.

[Buradaki](how-to-create-azure-vmware-solution-assessment.md)adımları Izleyerek Ilk Azure VMware çözümünüz (AVS) değerlendirmesini oluşturun.

## <a name="how-do-i-assess-with-imported-data"></a>İçeri aktarılan verilerle Nasıl yaparım? değerlendirin?

Sunucuları bir CSV dosyası kullanarak değerlendiriyorsanız, bir gereç olması gerekmez. Bunun yerine, aşağıdaki adımları uygulayın:

1. Azure 'ı sunucu değerlendirmesi ile çalışacak şekilde ayarlayın.
2. İlk değerlendirmenizi için bir Azure projesi oluşturun ve sunucu değerlendirmesi aracını buna ekleyin.
3. Bir CSV şablonu indirin ve buna sunucu verileri ekleyin.
4. Şablonu sunucu değerlendirmesi içine aktarın.
5. İçeri aktarma ile eklenen sunucuları bulun, bir gruba toplayın ve değerlendirme türü **Azure VMware çözümü (AVS)** olan grup için bir değerlendirme çalıştırın.

## <a name="what-data-does-the-appliance-collect"></a>Gereç hangi verileri toplar?

Değerlendirme için Azure geçişi gereci kullanıyorsanız, [VMware](migrate-appliance.md#collected-data---vmware)için toplanan meta veriler ve performans verileri hakkında bilgi edinin.

## <a name="how-does-the-appliance-calculate-performance-data"></a>Gereç performans verilerini nasıl hesaplar?

Bu gereci bulma işlemi için kullanıyorsanız, aşağıdaki adımlarla işlem ayarları için performans verilerini toplar:

1. Gereç gerçek zamanlı bir örnek noktası toplar.

    - **VMware VM 'leri**: her 20 saniyede bir örnek noktası toplanır.

2. Gereç, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktaları birleştirir. Veri noktasını oluşturmak için gereç tüm örneklerden tepe değerlerini seçer. Daha sonra veri noktasını Azure 'a gönderir.
3. Sunucu değerlendirmesi, geçen ay için 10 dakikalık tüm veri noktalarını depolar.
4. Bir değerlendirme oluşturduğunuzda sunucu değerlendirmesi, doğru bir şekilde kullanmak için kullanılacak uygun veri noktasını tanımlar. Kimlik, *performans geçmişi* ve *yüzdebirlik kullanımı*için yüzdebirlik değerlerini temel alır.

    - Örneğin, performans geçmişi bir hafta ise ve yüzdebirlik kullanımı 95. yüzdebirlik ise, sunucu değerlendirmesi geçen hafta için 10 dakikalık örnek noktaları sıralar. Bunları artan düzende sıralar ve doğru bir şekilde yapmak için 95. yüzdebirlik değerini seçer.
    - 95. yüzdebirlik değeri, 99. yüzdebirlik değerini seçtiğiniz takdirde dahil olabilen tüm aykırı değerleri yok saydığınızdan emin olmanızı sağlar.
    - Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız, yüzdebirlik kullanımı için 99. yüzdebirlik ' ü seçin.

5. Bu değer, gerecin topladığı bu ölçümler için etkili performans kullanım verilerini elde etmek için, rahatlık faktörüyle çarpılır:

    - CPU kullanımı
    - RAM kullanımı
    - Disk ıOPS (okuma ve yazma)
    - Disk aktarım hızı (okuma ve yazma)
    - Ağ aktarım hızı (ın ve out)

Aşağıdaki performans verileri toplanır ancak AVS değerlendirmelerine yönelik boyutlandırma önerilerinde kullanılmaz:
  - SANAL makineye bağlı her diske yönelik disk ıOPS ve aktarım hızı verileri.
  - Bir VM 'ye bağlı her bir ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek üzere ağ g/ç.

## <a name="how-are-avs-assessments-calculated"></a>AVS değerlendirmesi nasıl hesaplanır?

Sunucu değerlendirmesi, değerlendirmeleri hesaplamak için şirket içi makinelerin meta verilerini ve performans verilerini kullanır. Azure geçişi gerecini dağıtırsanız, değerlendirme gereç tarafından toplanan verileri kullanır. Ancak bir CSV dosyası kullanarak içeri aktarılan bir değerlendirmeyi çalıştırırsanız, hesaplama için meta verileri sağlarsınız.

Hesaplamalar şu üç aşamada gerçekleşir:

1. **Azure VMware Solution (AVS) hazırlığını hesapla**: Şirket Içi VM 'Lerin Azure VMware çözümüne (AVS) geçiş için uygun olup olmadığı.
2. **AVS düğümleri ve düğümler arasında kullanım sayısını hesapla**: VM 'leri çalıştırmak için gerekli olan ve tüm DÜĞÜMLERDE tahmini CPU, bellek ve depolama kullanımı IÇIN gereken AVS düğümü sayısı.
3. **Aylık maliyet tahmini**: Şirket Içi VM 'leri çalıştıran tüm Azure VMware çözümü (AVS) düğümlerine yönelik tahmini aylık maliyetler.

Hesaplamalar önceki sıradadır. Bir makine sunucusu, yalnızca öncekini geçerse daha sonraki bir aşamaya geçer. Örneğin, bir sunucu AVS hazırlık aşamasına geçemezse, Azure için uygun değil olarak işaretlenir. Bu sunucu için boyutlandırma ve maliyet hesaplamaları yapılmadı

## <a name="whats-in-an-azure-vmware-solution-avs-assessment"></a>Azure VMware Çözüm (AVS) değerlendirmesi nelerdir?

Sunucu değerlendirmesinde bir AVS değerlendirmesi aşağıda verilmiştir:


| **Özellik** | **Ayrıntılar** 
| - | - 
| **Hedef konum** | Geçirmek istediğiniz AVS özel bulut konumunu belirtir.<br/><br/> Sunucu değerlendirmesinde AVS değerlendirmesi Şu anda şu hedef bölgeleri destekliyor: Doğu ABD, Batı Avrupa Batı ABD. 
| **Depolama türü** | AVS 'de kullanılacak depolama altyapısını belirtir.<br/><br/> AVS değerlendirmeleri yalnızca varsayılan depolama türü olarak vSAN 'ı destekler. 
**Ayrılmış örnekler (RIS)** | Bu özellik, AVS 'de ayrılmış örnekler belirtmenize yardımcı olur. Bu, AVS düğümlerinde Şu anda desteklenmemektedir. 
**Düğüm türü** | Şirket içi VM 'Leri eşlemek için kullanılan [AVS düğüm türünü](../azure-vmware/concepts-private-clouds-clusters.md) belirtir. Varsayılan düğüm türü AV36 ' dir. <br/><br/> Azure geçişi, sanal makinelerin AVS 'ye geçirilmesi için gerekli sayıda düğüm önermenizi önerir. 
**FTT ayarı, RAID düzeyi** | Tolerans ve RAID birleşimlerine yönelik geçerli hatayı belirtir. Şirket içi VM disk gereksinimiyle birlikte bulunan seçili FTT seçeneği, AVS 'de gereken toplam vSAN depolama alanını tespit eder. 
**Boyutlandırma ölçütü** | AVS için sanal makineleri *doğru olarak boyutlandıralmak* üzere kullanılacak ölçütleri ayarlar. Performans geçmişini dikkate almadan *performans tabanlı* boyutlandırmayı veya *Şirket içi olarak* tercih edebilirsiniz. 
**Performans geçmişi** | Makinelerin performans verilerini değerlendirmek için göz önünde bulundurulması gereken süreyi ayarlar. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı*olduğunda geçerlidir. 
**Yüzdebirlik kullanımı** | Sağ boyutlandırmanın kabul edileceği performans örneği kümesinin yüzdebirlik değerini belirtir. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.
**Konfor katsayısı** | Azure geçişi sunucu değerlendirmesi, değerlendirme sırasında bir arabellek (rahatetken) kabul eder. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. 
**Teklif** | Kayıtlı olduğunuz [Azure teklifini](https://azure.microsoft.com/support/legal/offer-details/) görüntüler. Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para birimi** | Hesabınız için faturalandırma para birimini gösterir. 
**İndirim (%)** | Azure teklifinin üzerine aldığınız aboneliğe özgü tüm indirimi listeler. Varsayılan ayar, %0’dır. 
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)uygun olduğunu belirtir. Düğüm tabanlı fiyat nedeniyle Azure VMware Çözüm fiyatlandırması üzerinde hiçbir etkisi olmamasına karşın, müşteriler Azure hibrit avantajlarını kullanarak AVS 'ye şirket içi işletim sistemi lisanslarını (Microsoft tabanlı) uygulayabilir. Diğer yazılım işletim sistemi satıcılarının, RHEL gibi kendi lisans şartlarını sağlaması gerekecektir. 
**vCPU fazla abonelik** | AVS düğümündeki bir fiziksel çekirdeğe bağlı sanal çekirdek sayısının oranını belirtir. Hesaplamalardaki varsayılan değer 4 vCPU: AVS 'de 1 fiziksel çekirdek olur. <br/><br/> API kullanıcıları bu değeri bir tamsayı olarak ayarlayabilir. VCPU fazla aboneliği > 4:1 ' nin, CPU kullanımına bağlı olarak iş yüklerini etkileyebileceğini unutmayın. 


## <a name="azure-vmware-solution-avs-suitability-analysis"></a>Azure VMware çözümü (AVS) uygunluk Analizi

Sunucu değerlendirmelerinde AVS değerlendirmelerinde, makine özelliklerini inceleyerek her şirket içi VM 'yi AVS 'ye uygunluk amacıyla değerlendirin. Ayrıca, her bir değerlendirilen makineyi aşağıdaki uygunluk kategorilerinden birine atar:

- **AVS Için hazırlanma**: makine, Azure 'A (AVS) herhangi bir değişiklik yapılmadan olarak geçirilebilir. Tam AVS desteğiyle AVS 'de başlatılır.
- **Koşullara göre**: VM, geçerli vSphere sürümü ile uyumluluk sorunlarına ve VM 'deki tam işlevselliği AVS 'de elde etmeden önce büyük olasılıkla VMware araçları ve veya diğer ayarları gerektirmek olabilir.
- **AVS için hazırlanma**: VM, AVS 'de başlamaz. Örneğin, şirket içi VMware VM 'sinde CD-ROM gibi bir dış cihaz varsa VMware VMotion işlem başarısız olur (VMware VMotion kullanılıyorsa).
- **Hazır olma durumu bilinmiyor**: Azure geçişi, şirket içi ortamdan toplanan meta verilerin yetersiz olması nedeniyle makinenin hazır olduğunu saptayamadık.

Sunucu değerlendirmesi, şirket içi makinenin Azure 'un hazır olduğunu öğrenmek için makine özelliklerini inceler.

### <a name="machine-properties"></a>Makine özellikleri

Sunucu değerlendirmesi, Azure VMware çözümünde (AVS) çalışıp çalışmadığını öğrenmek için şirket içi sanal makinenin aşağıdaki özelliğini gözden geçirir.


| **Özellik** | **Ayrıntılar** | **AVS hazırlık durumu** 
| - | - | - 
| **Internet Protokolü** | AVS Şu anda IPv6 internet adreslemesini desteklemiyor.<br/><br/> Makineniz IPv6 ile algılanıyorsa düzeltme kılavuzumuzu göstermek için yerel MSFT AVS GBB ekibinize başvurun.| Koşullu olarak Ready Internet Protokolü


### <a name="guest-operating-system"></a>Konuk işletim sistemi

Şu anda, AVS değerlendirmelerinde, uygunluk analizinin bir parçası olarak işletim sistemi incelenmez. Şirket içi VM 'lerde çalışan tüm işletim sistemleri, Azure VMware çözümünde (AVS) çalışıyor olabilir.

Sunucu değerlendirmesi, VM özellikleriyle birlikte makinelerin Konuk işletim sistemine bakar ve bunların Azure üzerinde çalışıp çalışmadığını tespit edebilir.


## <a name="sizing"></a>Boyutlandırma

Bir makine AVS için uygun olarak işaretlendikten sonra, sunucu değerlendirmesi içindeki AVS değerlendirmesi, uygun şirket içi VM gereksinimlerini tanımlamayı ve gereken AVS düğümlerinin toplam sayısını bulmayı içeren düğüm boyutlandırma önerilerini oluşturur. Bu öneriler, belirtilen değerlendirme özelliklerine bağlı olarak farklılık gösterir.

- Değerlendirme *performans tabanlı boyutlandırma*kullanıyorsa, Azure GEÇIŞI, AVS 'nin uygun boyutlandırma önerilerini sağlamak için makinenin performans geçmişini dikkate alır. Bu yöntem, şirket içi VM 'yi aşırı ayırdıysanız ancak kullanım düşükse ve maliyetleri kazanmak için AVS 'de sanal makineyi doğru boyuta eklemek istiyorsanız özellikle yararlıdır. Bu yöntem, geçiş sırasında boyutları iyileştirmenize yardımcı olur.
- VM boyutlandırma için performans verilerini göz önünde bulundurmayın ve şirket içi makineleri, AVS 'ye olduğu gibi almak istiyorsanız, boyutlandırma ölçütünü *Şirket içi olarak*olarak ayarlayabilirsiniz. Daha sonra, sunucu değerlendirmesi, kullanım verilerini dikkate almadan VM 'Leri şirket içi yapılandırmaya göre boyut olarak kullanacaktır. 


### <a name="ftt-sizing-parameters"></a>FTT boyutlandırma parametreleri

AVS 'de kullanılan depolama altyapısı vSAN ' dır. vSAN depolama ilkeleri, sanal makineleriniz için depolama gereksinimlerini tanımlar. Bu ilkeler, depolama biriminin VM 'ye nasıl ayrılacağını tespit ettiğinden VM 'niz için gereken hizmet düzeyini garanti eder. Kullanılabilir FTT-RAID birleşimleri şunlardır: 

**Tolerans sayısı (FTT)** | **RAID yapılandırması** | **Gerekli en düşük konaklar** | **Boyutlandırma değerlendirmesi**
--- | --- | --- | --- 
1 | RAID-1 (yansıtma) | 3 | 100'LIK bir VM 200GB tüketir.
1 | RAID-5 (ERASURE kodlaması) | 4 | 100 GB 'lık bir VM 133.33 GB tüketir
2 | RAID-1 (yansıtma) | 5 | 100 GB 'lık bir VM 300 GB tüketir.
2 | RAID-6 (ERASURE kodlaması) | 6 | 100 GB 'lik bir VM, 150GB tüketir.
3 | RAID-1 (yansıtma) | 7 | 100 GB 'lık bir VM 400 ' ü tüketir.

### <a name="performance-based-sizing"></a>Performans tabanlı boyutlandırma

Performans tabanlı boyutlandırma için sunucu değerlendirmesi, VM 'ye bağlı disklerle başlar ve ardından ağ bağdaştırıcıları gelir. Daha sonra, sanal makine gereksinimlerini AVS için uygun bir düğüm olmadan eşler. Azure geçişi gereci, CPU, bellek, disk ve ağ bağdaştırıcısı için performans verilerini toplamak üzere şirket içi ortamı profiller.

**Performans verileri toplama adımları:**

1. VMware VM 'Leri için Azure geçişi gereci, her 20 saniyelik aralığa göre gerçek zamanlı bir örnek noktası toplar. 
2. Gereç, 10 dakikada bir toplanan örnek noktaların dökümünü yapar ve en son 10 dakikalık değeri sunucu değerlendirmesine gönderir.
3. Sunucu değerlendirmesi son bir ay için 10 dakikalık tüm örnek noktalarını depolar. Daha sonra, *performans geçmişi* ve *yüzdebirlik kullanımı*için belirtilen değerlendirme özelliklerine bağlı olarak, doğru boyutlandırma için kullanılacak uygun veri noktasını tanımlar. Örneğin, performans geçmişi 1 güne ayarlanmışsa ve yüzdebirlik kullanımı 95. yüzdebirlik ise, sunucu değerlendirmesi son bir gün için 10 dakikalık örnek noktalarını kullanır, bunları artan düzende sıralar ve sağ boyutlandırma için 95. yüzdebirlik değerini seçer.
4. Bu değer, her ölçüm için etkili performans kullanım verilerini (CPU kullanımı, bellek kullanımı, disk ıOPS (okuma ve yazma), disk aktarım hızı (okuma ve yazma) ve gerecin topladığı ağ aktarım hızını (ın ve out) almak için rahatlık faktörüyle çarpılır.

Etkin kullanım değeri saptandıktan sonra, depolama, ağ ve bilgi işlem boyutu aşağıdaki şekilde işlenir.

**Depolama boyutlandırma**: Azure geçişi, müşteriler tarafından seçilen FTT ayarının yanı sıra AVS vSAN depolama gereksinimlerini belirlemede bir hesaplama parametresi olarak toplam ŞIRKET içi VM disk alanını kullanır. FTT-tolerans başına en az sayıda düğüm gerektirme ve FTT seçeneği için gereken toplam vSAN depolama alanı, VM disk gereksinimiyle birlikte gerekli olan toplam vSAN depolamayı tespit eder.

**Ağ boyutlandırma**: Sunucu değerlendirmesi Şu anda, AVS değerlendirmelerinde hiçbir ağ ayarı dikkate almaz.

**İşlem boyutlandırma**: depolama gereksinimlerini hesapladıktan sonra sunucu değerlendirmesi, düğüm türünü temel alan AVS için gereken düğüm SAYıSıNı belirlemede CPU ve bellek gereksinimlerini dikkate alır.

- Sunucu değerlendirmesi, boyutlandırma ölçütlerine göre performans tabanlı VM verilerine veya şirket içi VM yapılandırmasına göre görünür. Rahatlık faktörü ayarı, kümenin büyüme faktörünü belirtmenize olanak tanır. Şu anda, hiper iş parçacığı oluşturma özelliği etkinleştirilmiştir ve bu nedenle 36 çekirdek düğümü 72 sanal çekirdeğe sahip olacaktır. fiziksel olarak 5 sanal çekirdek; VMware standardını kullanarak küme başına CPU eşiklerini belirleme, bakım veya hataların küme kullanılabilirliğine ödün vermeden işlenmesine izin vermek için kullanılan %80 kullanımı aşılmıyor. Şu anda, fazla abonelik değerlerini değiştirmek için kullanılabilecek geçersiz kılma yok ve bu, gelecekteki sürümlerde bu durum olabilir.

### <a name="as-on-premises-sizing"></a>Şirket içi boyutlandırma olarak

*Şirket içi boyutlandırma olarak*kullanıyorsanız, sunucu değerlendirmesi VM 'lerin ve disklerin performans geçmişini dikkate almaz. Bunun yerine, şirket içinde ayrılan boyuta göre AVS düğümlerini ayırır. Varsayılan depolama türü, AVS 'deki vSAN ' dır.

## <a name="confidence-ratings"></a>Güven derecelendirmeleri

Azure geçişi 'ndeki her performans tabanlı değerlendirme, bir (en düşük) ile beş yıldızlı (en yüksek) arasında değişen bir güvenirlik derecelendirmesi ile ilişkilendirilir.

- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güven derecelendirmeleri, *Şirket içi* değerlendirmelere uygun değildir.
- Performans tabanlı boyutlandırma için sunucu değerlendirmesinde AVS değerlendirmelerinde CPU ve VM belleği için kullanım verileri gerekir. Aşağıdaki veriler toplanır ancak AVS için boyutlandırma önerilerinde kullanılmaz:
  - SANAL makineye bağlı her diske yönelik disk ıOPS ve aktarım hızı verileri.
  - Bir VM 'ye bağlı her bir ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek üzere ağ g/ç.

  Bu kullanım numaralarının herhangi biri vCenter Server kullanılamazsa, boyut önerisi güvenilir olmayabilir.

Kullanılabilir veri noktalarının yüzdesine bağlı olarak, değerlendirmenin güvenilirlik derecelendirmesi aşağıdaki gibi gider.


| **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi** |
| - | - |
| % 0-20 | 1 yıldız |
| % 21-40 | 2 yıldız |
| % 41-60 | 3 yıldız |
| % 61-80 | 4 yıldız |
| % 81-100 | 5 yıldız |

### <a name="low-confidence-ratings"></a>Düşük güvenilirlikli derecelendirmeler

Bir değerlendirmenin en düşük güvenilirlik derecelendirmesinin neden olmasının birkaç nedeni aşağıda verilmiştir:

- Değerlendirmeyi oluşturduğunuz süre için ortamınızı profildiniz. Örneğin, performans süresi ile bir gün ayarlanmış bir değerlendirme oluşturursanız, toplanan tüm veri noktaları için bulmayı başlattıktan sonra en az bir gün beklemeniz gerekir.
- Bazı sanal makineler, değerlendirmenin hesaplanmakta olduğu süre boyunca kapatıldı. Bir süre için herhangi bir VM kapatılmışsa, sunucu değerlendirmesi söz konusu döneme ait performans verilerini toplayamazlar.
- Değerlendirmenin hesaplandığı süre boyunca bazı sanal makineler oluşturulmuştur. Örneğin, geçen ayın performans geçmişi için bir değerlendirme oluşturduysanız ancak bazı sanal makineler ortamda yalnızca bir hafta önce oluşturulduysa, yeni VM 'lerin performans geçmişi, tamamlanma süresi boyunca mevcut olmayacaktır.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecelendirmesi beş yıldızlı günden azsa, gerecin ortamın profilini oluşturup daha sonra değerlendirmeyi yeniden hesaplayabilmeniz için en az bir gün beklemeniz önerilir. Aksi takdirde, performans tabanlı boyutlandırma güvenilir olmayabilir. Bu durumda, değerlendirmeyi şirket içi boyutlandırılmasına geçmeniz önerilir.

## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra, Azure geçişi, düğüm fiyatının gerektirdiği AVS düğümlerinin sayısını çarparak, AVS 'de şirket içi iş yüklerini çalıştırmanın toplam maliyetini hesaplar. VM başına maliyet maliyeti, toplam maliyeti değerlendirmede bulunan VM sayısına bölerek hesaplanır. 
- Hesaplama, gerekli düğüm sayısını, düğüm türünü ve konumu hesaba göre alır.
- Toplam aylık maliyeti hesaplamak için tüm düğümlerdeki maliyeti toplar.
- Ücretler, değerlendirme ayarlarında belirtilen para biriminde görüntülenir.

Azure VMware çözümü (AVS) fiyatlandırması düğüm başına olduğunda, toplam maliyet işlem maliyeti ve depolama maliyeti dağıtımına sahip değildir. [Daha Fazla Bilgi](../azure-vmware/introduction.md)

Azure VMware çözümü (AVS) önizlemede olduğu gibi, değerlendirmede düğüm fiyatlarının önizleme fiyatları olduğunu unutmayın. Rehberlik için lütfen yerel MSFT AVS GBB ekibine başvurun.

## <a name="migration-tool-guidance"></a>Geçiş Aracı Kılavuzu

Azure VMware çözümü (AVS) değerlendirmesi için Azure hazırlık raporu ' nda, aşağıdaki önerilen araçları görebilirsiniz: 
- VMware **HCX veya Enterprise**: VMware makineleri Için VMware karma bulut uzantısı (HCX) çözümü, şirket içi iş yükünüzü Azure VMware çözümünüz (AVS) özel bulutuna geçirmek için önerilen geçiş aracıdır. [Daha fazla bilgi edinin](../azure-vmware/hybrid-cloud-extension-installation.md).
- **Bilinmiyor**: bir CSV dosyası aracılığıyla içeri aktarılan makineler için varsayılan geçiş aracı bilinmez. Ancak VMware makinelerinde, VMWare karma bulut uzantısı (HCX) çözümünün kullanılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[AVS VMware VM 'leri](how-to-create-azure-vmware-solution-assessment.md)için bir değerlendirme oluşturun.
