---
title: Azure Geçiş'in önceki sürümüyle çalışma
description: Azure Geçiş'in önceki sürümüyle nasıl çalışılabildiğini açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c9cea52e04a991e6e3ac64426f0443939f8aaa3a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77914390"
---
# <a name="work-with-the-previous-version-of-azure-migrate"></a>Azure Geçiş'in önceki sürümüyle çalışma

Bu makalede, Azure Geçiş önceki sürümüyle çalışma hakkında bilgi verilmektedir.


Azure Geçiş hizmetinin iki sürümü vardır:

- **Geçerli sürüm**: Azure Geçir projeleri oluşturmak, şirket içi makineleri keşfetmek ve değerlendirmeleri ve geçişleri düzenlemek için bu sürümü kullanın. Bu sürümdeki yenilikler hakkında [daha fazla bilgi edinin.](whats-new.md)
- **Önceki sürüm**: Azure Geçiş önceki sürümünü kullanıyorsanız (yalnızca şirket içi VMware VM'lerin değerlendirmesi desteklendi), artık geçerli sürümü kullanmanız gerekir. Önceki sürümde oluşturulan Azure Geçiş projelerini hala kullanmanız gerekiyorsa, şunları yapabilirsiniz ve yapamazsınız:
    - Artık geçiş projeleri oluşturamazsınız.
    - Yeni keşifler yapmamanızı öneririz.
    - Varolan projelere erişebilirsiniz.
    - Hala değerlendirmeleri çalıştırabilirsiniz.
    

## <a name="upgrade-between-versions"></a>Sürümler arasında yükseltme

Önceki sürümdeki projeleri veya bileşenleri yeni sürüme yükseltemezsiniz. [Yeni bir Azure Geçiş projesi oluşturmanız](how-to-add-tool-first-time.md)ve değerlendirme ve geçiş araçları eklemeniz gerekir.

## <a name="find-projects-from-previous-version"></a>Önceki sürümdeki projeleri bulma

Önceki sürümdeki projeleri aşağıdaki gibi bulun:

1. Azure portalında tüm **hizmetleri**>, **Azure Geçiş'i**arayın ve seçin. 
2. Azure Geçir panosunda, eski Azure Geçir projelerine erişmek için bir bildirim ve bağlantı vardır.
3. v1 projelerini açmak için bağlantıyı tıklatın.


## <a name="create-an-assessment"></a>Değerlendirme oluşturma

Portalda VM'ler bulunduktan sonra, bunları gruplandırın ve değerlendirmeler oluşturursunuz.

- Portalda VM'ler bulunduktan hemen sonra şirket içi değerlendirmeler oluşturabilirsiniz.
- Performansa dayalı değerlendirmeler için, güvenilir boyut önerileri almak için performansa dayalı bir değerlendirme oluşturmadan önce en az bir gün beklemenizi öneririz.

Aşağıdaki gibi bir değerlendirme oluşturun:

1. Projenin **Genel Bakış** sayfasında **+Değerlendirme oluştur**’a tıklayın.
2. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.
3. Grubu oluşturun ve bir grup adı belirtin.
4. Gruba eklemek istediğiniz makineleri seçin.
5. Grubu ve değerlendirmeyi oluşturmak için **Değerlendirme Oluştur**’a tıklayın.
6. Değerlendirme oluşturulduktan sonra, **Genel Bakış** > **Panosu'nda**görüntüleyin.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.

Var olan bir değerlendirmeyi en son performans verileriyle güncelleştirmek isterseniz, değerlendirmeyi güncelleştirmek için **Yeniden Hesapla** komutunu kullanabilirsiniz.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme 

Bir değerlendirmenin üç aşaması vardır:

- Değerlendirme, makinelerin Azure'da uyumlu olup olmadığını anlamak için uygunluk analiziyle başlar.
- Boyutlandırma tahminleri.
- Aylık maliyet tahmini.

Bir makine yalnızca bir öncekinden geçerse daha sonraki bir aşamaya geçer. Örneğin, bir makine uygunluk denetiminde başarısız olursa, Azure için uygun olmayan olarak işaretlenir ve boyutlandırma ve maliyetlendirme yapılmaz.


### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

Değerlendirmedeki Azure için hazır olma görünümü, her bir sanal makinenin hazır olma durumunu gösterir.

**Hazırlık** | **Durum** | **Şey**
--- | --- | ---
Azure için hazır | Uyumluluk sorunu yok. Makine Azure'a olduğu gibi geçirilebilir ve tam Azure desteğiyle Azure'da önyükleme yapabilir. | Azure Geçişi, hazır olan VM’ler için Azure’da bir VM boyutu önerir.
Azure için koşullu olarak hazır | Makine Azure'da önyükleme yapabilir, ancak tam Azure desteğine sahip olmayabilir. Örneğin, Azure'da desteklenmeyen windows sunucusunun eski bir sürümüne sahip bir makine. | Azure Geçir, hazırlık sorunlarını açıklar ve düzeltme adımları sağlar.
Azure için hazır değil |  VM Azure'da önyükleme yapmaz. Örneğin, bir VM'de 4 TB'dan fazla bir disk varsa, Azure'da barındırılamaz. | Azure Geçir, hazırlık sorunlarını açıklar ve düzeltme adımları sağlar.
Hazır olma durumu bilinmiyor | Azure Geçir, genellikle veri kullanılamadığı için Azure hazır durumunu belirleyemez.


#### <a name="azure-vm-properties"></a>Azure VM özellikleri
Hazır olma, VM'nin Azure'da çalışıp çalışmayacağını belirlemek için bir dizi VM özelliğini dikkate alır.


**Özellik** | **Şey** | **Hazırlık**
--- | --- | ---
**Önyükleme türü** | BIOS desteklendi. UEFI desteklenmez. | Önyükleme türü UEFI ise koşullu olarak hazır.
**Çekirdekler** | Machines core <= Azure VM için desteklenen maksimum çekirdek sayısı (128).<br/><br/> Performans geçmişi varsa, Azure Geçir kullanılan çekirdekleri dikkate alır.<br/>Değerlendirme ayarlarında bir konfor faktörü belirtilirse, kullanılan çekirdek sayısı konfor faktörü ile çarpılır.<br/><br/> Performans geçmişi yoksa, Azure Geçir, konfor faktörlerini uygulamadan ayrılan çekirdekleri kullanır. | Limitlerden daha az veya eşitse hazır olun.
**Bellek** | Makine bellek boyutu <= Azure VM için maksimum bellek&nbsp;(Azure M serisinde 3892 GB Standard_M128m<sup>2)</sup>olur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure Geçir bellek kullanılan dikkate alır.<br/><br/>Bir konfor faktörü belirtilirse, kullanılan bellek konfor faktörü ile çarpılır.<br/><br/> Geçmiş yoksa, ayrılan bellek konfor faktörü uygulanmadan kullanılır.<br/><br/> | Sınırlar içinde hazır.
**Depolama diski** | Bir diskin ayrılan boyutu 4 TB (4096 GB) veya daha az olmalıdır.<br/><br/> Makineye iliştirilen disk sayısı işletim sistemi de dahil olmak üzere 65 veya daha az olmalıdır. | Sınırlar içinde hazır.
**Ağ Oluşturma** | Bir makinenin 32 veya daha az NIC'si olmalıdır. | Sınırlar içinde hazır.

#### <a name="guest-operating-system"></a>Konuk işletim sistemi

Azure Geçir, VM özelliklerinin yanı sıra, VM'nin Azure'da çalıştırılap çalışmayabileceğini belirlemek için şirket içi VM'nin konuk işletim sistemi de inceler.

- Azure Geçir, vCenter Server'da belirtilen işletim sistemi dikkate alır.
- Azure Geçir tarafından yapılan keşif cihaz tabanlı olduğundan, VM içinde çalışan işletim sistemi vCenter Server'da belirtilenişletim sistemiyle aynı olup olmadığını doğrulamanın bir yolu yoktur.

Aşağıdaki mantık kullanılır.

**İşletim Sistemi** | **Şey** | **Hazırlık**
--- | --- | ---
Windows Server 2016 ve tüm SP'ler | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 R2 ve tüm SP'ler | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 ve tüm SPs | Azure tam destek sağlar. | Azure için hazır
Windows Server 2008 R2 ve tüm SP'ler | Azure tam destek sağlar.| Azure için hazır
Windows Server 2008 (32 bit ve 64 bit) | Azure tam destek sağlar. | Azure için hazır
Windows Server 2003, 2003 R2 | Destek dışı ve Azure'da destek için özel [destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyacınız vardır. | Azure için koşullu olarak hazır, Azure'a geçmeden önce işletim sistemi yükseltmeyi düşünün.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Desteksiz. Makine Azure'da önyükleme yapabilir, ancak Azure tarafından işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak hazır olduğunda, Azure'a geçmeden önce işletim sistemi yükseltmeniz önerilir.
Windows İstemci 7, 8 ve 10 | Azure yalnızca [Visual Studio aboneliği](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) yle destek sağlar. | Azure için koşullu olarak hazır
Windows 10 Pro Masaüstü | Azure, [Multitenant Barındırma Hakları](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) ile destek sağlar. | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Desteksiz. Makine Azure'da önyükleme yapabilir, ancak Azure tarafından işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak hazır olduğunda, Azure'a geçmeden önce işletim sistemi yükseltmeniz önerilir.
Linux | Azure bu [Linux işletim sistemlerini](../virtual-machines/linux/endorsed-distros.md)onaylar. Diğer Linux işletim sistemleri Azure'da önyükleme yapabilir, ancak Azure'a geçmeden önce işletim sisteminin onaylı bir sürüme yükseltilmesi önerilir. | Sürüm onaylanırsa Azure için hazır olun.<br/><br/>Sürüm onaylanmazsa koşullu olarak hazır.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris, Apple Mac OS vb, FreeBSD, vb. | Azure bu işletim sistemlerini onaylamaz. Makine Azure'da önyükleme yapabilir, ancak Azure tarafından işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak hazır, Azure'a geçiş yapmadan önce desteklenen bir işletim sistemi yüklemeniz önerilir.  
vCenter Server'da **Diğer** olarak belirtilen işletim sistemi | Azure Geçir bu durumda işletim sistemi tanımlayamaz. | Bilinmeyen hazırlık. VM içinde çalışan işletim sistemi Azure'da desteklenir emin olun.
32 bit işletim sistemleri | Makine Azure'da önyükleme yapabilir, ancak Azure tam destek sağlamayabilir. | Azure için koşullu olarak hazır, Azure'a geçmeden önce makinenin işletim çekimini 32 bit işletim sistemi'nden 64 bit işletim sistemi'ne yükseltmeyi düşünün.


### <a name="review-sizing"></a>Boyutlandırmayı gözden geçirme

 Azure Geçir boyutu önerisi, değerlendirme özelliklerinde belirtilen boyutlandırma ölçütüne bağlıdır.

- Boyutlandırma performans ayarı tabanlıysa, boyut önerisi VM'lerin (CPU ve bellek) ve disklerin (IOPS ve iş bölümü) performans geçmişini dikkate alır.
- Boyutlandırma ölçütü 'şirket içi olarak' ise, Azure'daki boyut önerisi şirket içi VM'nin boyutuna bağlıdır. Disk boyutlandırma, değerlendirme özelliklerinde belirtilen Depolama türüne dayanır (varsayılan değer premium disklertir). Azure Geçir, VM ve disklerin performans verilerini dikkate almaz.

### <a name="review-cost-estimates"></a>Tahmini maliyetleri gözden geçirme

Maliyet tahminleri, Her makinenin ayrıntılarıyla birlikte Azure'da VM'leri çalıştırmanın toplam bilgi işlem ve depolama maliyetini gösterir.

- Maliyet tahminleri, bir VM makinesi nin boyut önerisi, diskleri ve değerlendirme özellikleri kullanılarak hesaplanır.
- İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir.
- Maliyet tahmini, şirket içi VM'yi hizmet (IaaS) VM'leri olarak Azure Altyapısı olarak çalıştırmak içindir. Azure Geçiş, Platform'u hizmet (PaaS) veya Hizmet (SaaS) maliyeti olarak kabul etmez.

### <a name="review-confidence-rating-performance-based-assessment"></a>Güven derecelendirmesini gözden geçirme (performansa dayalı değerlendirme)

Her performansa dayalı değerlendirme bir güven derecelendirmesi ile ilişkilidir.

- Güven derecelendirmesi bir yıldızdan beş yıldıza kadar değişir (bir yıldız en düşük ve beş yıldızlı en yüksek yıldızdır).
- Güven derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlı olarak bir değerlendirmeye atanır.
- Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Şirket içi değerlendirmeler için güven derecelendirmesi mevcut değildir.

Performanstabanlı boyutlandırma için Azure Geçiş'in aşağıdakilere ihtiyacı vardır:
- CPU için kullanım verileri.
- VM bellek verileri.
- VM'ye bağlı her disk için disk IOPS ve iş verme verilerine ihtiyaç duyar.
- Bir VM'ye bağlı her ağ bağdaştırıcısı için Azure Geçir'in ağ girişine/çıkışına ihtiyacı vardır.
- Yukarıdakilerden herhangi biri kullanılamıyorsa, boyut önerileri (ve dolayısıyla güven derecelendirmeleri) güvenilir olmayabilir.


Kullanılabilir veri noktalarının yüzdeye bağlı olarak, olası güven derecelendirmeleri tabloda özetlenir.

**Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız


#### <a name="assessment-issues-affecting-confidence-ratings"></a>Güven derecelendirmelerini etkileyen değerlendirme sorunları

Bir değerlendirme, bir dizi nedenden dolayı tüm veri noktalarına sahip olmayabilir:

- Değerlendirme süresince çevrenizin profilini çıkmadınız. Örneğin, performans süresi bir güne ayarlanmış bir değerlendirme oluşturursanız, keşfe başladıktan sonra en az bir gün veya toplanacak tüm veri noktaları için beklemeniz gerekir.
- Bazı VM'ler değerlendirmenin hesaplandığı dönemde kapatıldı. Belirli bir süre boyunca herhangi bir Sanal Bilgisayar kapatıldıysa, Azure Geçir o dönem için performans verileri toplayamaz.
- Değerlendirme hesaplama döneminde arasında bazı VM'ler oluşturuldu. Örneğin, geçen ayın performans geçmişini kullanarak bir değerlendirme oluşturur, ancak bir hafta önce ortamda bir dizi VM oluşturursanız, yeni VM'lerin performans geçmişi tüm süre boyunca olmayacaktır.

> [!NOTE]
> Herhangi bir değerlendirmenin güven derecesi beş yıldızın altındaysa, cihazın çevreprofilini niçin en az bir gün bekleyin ve değerlendirmeyi yeniden hesaplayın. Performans tabanlı boyutlandırma yapmazsanız güvenilir olmayabilir. Yeniden hesaplamak istemiyorsanız, değerlendirme özelliklerini değiştirerek şirket içi boyutlandırmaya geçmenizi tavsiye ettik.



## <a name="create-groups-using-dependency-visualization"></a>Bağımlılık görselleştirmekullanarak gruplar oluşturma

Grupları el ile oluşturmaya ek olarak, bağımlılık görselleştirmesini kullanarak gruplar oluşturabilirsiniz.
- Bir değerlendirme çalıştırmadan önce, makine bağımlılıklarını çapraz denetleyerek daha yüksek güven düzeylerine sahip grupları değerlendirmek istediğinizde genellikle bu yöntemi kullanırsınız.
- Bağımlılık görselleştirmesi, Azure'a geçişinizi etkili bir şekilde planlamanıza yardımcı olabilir. Geride hiçbir şey kalmamasını ve Azure'a geçiş yaparken sürpriz kesintilerin oluşmadığından emin olur.
- Birlikte geçiş yapmak için gereken tüm birbirine bağlı sistemleri keşfedebilir ve çalışan bir sistemin kullanıcılara hizmet vermeye devam edip etmediğini veya geçiş yerine hizmet dışı bırakma için aday olup olmadığını belirleyebilirsiniz.
- Azure Geçir, bağımlılık görselleştirmesini etkinleştirmek için Azure Monitor'da Hizmet Haritası çözümünü kullanır.

> [!NOTE]
> Bağımlılık görselleştirmesi Azure Kamu'da kullanılamaz.

Bağımlılık görselleştirmesini ayarlamak için, bir Log Analytics çalışma alanını bir Azure Geçiş projesiyle ilişkilendirin, bağımlılıkları görselleştirmek istediğiniz makinelere aracılar yükleyin ve bağımlılık bilgilerini kullanarak gruplar oluşturursunuz. 



### <a name="associate-a-log-analytics-workspace"></a>Günlük Analitiği çalışma alanını ilişkilendirme

Bağımlılık görselleştirmesini kullanmak için, bir Geçiş Projesi ile Log Analytics çalışma alanını ilişkilendirin. Yalnızca geçiş projesinin oluşturulduğu aynı abonelikte bir çalışma alanı oluşturabilir veya ekleyebilirsiniz.

1. Bir Projeye Log Analytics çalışma alanı eklemek **için, Genel Bakış**, > **Essentials**, **yapılandırma gerektirir'ı**tıklatın.
2. Yeni bir çalışma alanı oluşturabilir veya varolan bir çalışma alanı ekleyebilirsiniz:
  - Yeni bir çalışma alanı oluşturmak için bir ad belirtin. Çalışma alanı, geçiş projesiyle aynı [Azure coğrafyasındaki](https://azure.microsoft.com/global-infrastructure/geographies/) bir bölgede oluşturulur.
  - Varolan bir çalışma alanı eklediğinizde, geçiş projesiyle aynı abonelikteki kullanılabilir tüm çalışma alanlarından seçim yapabilirsiniz. Yalnızca [desteklenen Hizmet Haritası bölgesinde](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)oluşturulan çalışma alanları listelenir. Çalışma alanı eklemek için, çalışma alanına 'Reader' erişimi olduğundan emin olun.

> [!NOTE]
> Geçiş projesiyle ilişkili çalışma alanını değiştiremezsiniz.

### <a name="download-and-install-vm-agents"></a>VM aracılarını indirin ve kurun

Bir çalışma alanını yapılandırıldıktan sonra, değerlendirmek istediğiniz her şirket içi makineye aracıları karşıdan yükler ve yüklersiniz. Ayrıca, internet bağlantısı olmayan makineleriniz varsa, [Log Analytics ağ geçidini](../azure-monitor/platform/gateway.md) indirmeniz ve yüklemeniz gerekir.

1. **Genel Bakış'ta**Makineleri **Yönet'i** > **Machines**tıklatın ve gerekli makineyi seçin.
2. **Bağımlılıklar** sütununda, **aracıları yükle'yi**tıklatın.
3. **Bağımlılıklar** sayfasında, değerlendirmek istediğiniz her VM'deki Microsoft İzleme Aracısını (MMA) ve Bağımlılık aracısını indirin ve yükleyin.
4. Çalışma alanı kimliğini ve anahtarını kopyalayın. MMA'yı şirket içi makineye yüklerken bunlara ihtiyacınız vardır.

> [!NOTE]
> Aracıların yüklenmesini otomatikleştirmek için Configuration Manager gibi bir dağıtım aracını veya Azure Geçiş için aracı dağıtım çözümü sağlayan [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration)gibi bir iş ortağı aracını kullanabilirsiniz.


#### <a name="install-the-mma-agent-on-a-windows-machine"></a>MMA aracısını Windows makinesine yükleme

Aracıyı Bir Windows makinesine yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. **Hedef Klasör'de,** Varsayılan yükleme klasörünü > **Sonraki'de**tutun veya değiştirin.
4. **Aracı Kurulum Seçenekleri'nde,****Sonraki**Azure **Günlük Analizi'ni** > seçin.
5. Yeni bir Log Analytics çalışma alanı eklemek için **Ekle'yi** tıklatın. Portaldan kopyaladığınız çalışma alanı kimliğine ve anahtarına yapıştırın. **İleri**'ye tıklayın.

Aracıyı komut satırından veya Configuration Manager gibi otomatik bir yöntem kullanarak yükleyebilirsiniz. MMA aracısını yüklemek için bu yöntemleri kullanma hakkında [daha fazla bilgi edinin.](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)

#### <a name="install-the-mma-agent-on-a-linux-machine"></a>MMA aracısını bir Linux makinesine yükleme

Aracıyı bir Linux makinesine yüklemek için:

1. Scp/sftp kullanarak uygun paketi (x86 veya x64) Linux bilgisayarınıza aktarın.
2. --install bağımsız değişkenini kullanarak paketi yükleyin.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

MMA'nın Linux işletim sistemleri desteği listesi hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)

### <a name="install-the-mma-agent-on-a-machine-monitored-by-operations-manager"></a>Operasyon Yöneticisi tarafından izlenen bir makineye MMA aracısını yükleme

System Center Operations Manager 2012 R2 veya üzeriyle izlenen makineler için MMA aracısını yüklemek gerekmiyor. Hizmet Haritası, gerekli bağımlılık verilerini toplamak için Operasyon Yöneticisi MMA ile tümleştirir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Bağımlılık aracısının yüklenmesi gerekir.

### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme

1. Bağımlılık aracısını bir Windows makinesine yüklemek için kurulum dosyasını çift tıklatın ve sihirbazı izleyin.
2. Bağımlılık aracısını bir Linux makinesine yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

- Windows ve Linux işletim sistemleri için [Bağımlılık aracısı desteği](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) hakkında daha fazla bilgi edinin.
- Bağımlılık aracısını yüklemek için komut dosyalarını nasıl kullanabileceğiniz hakkında [daha fazla bilgi edinin.](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples)

>[!NOTE]
> Sistem ön koşulları ve Bağımlılık aracısını dağıtma yöntemlerine genel bir bakış sağlamak için başvurulan Azure VM'ler makalesi, Hizmet Haritası çözümü için de geçerlidir.

### <a name="create-a-group-with-dependency-mapping"></a>Bağımlılık eşleme içeren bir grup oluşturma

1. Aracıları yükledikten sonra portala gidin ve**Makineleri** **Yönet'e** > tıklayın.
2. Aracıları yüklediğiniz makineyi arayın.
3. Makinenin **Bağımlılıklar** sütunu artık **Görünüm Bağımlılıkları**olarak gösterilmelidir. Makinenin bağımlılıklarını görüntülemek için sütunu tıklatın.
4. Makinenin bağımlılık haritası aşağıdaki ayrıntıları gösterir:
    - Gelen (İstemler) ve giden (Sunucular) Makineye/makineden TCP bağlantıları
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarına göre gruplandırılır.
        - MMA ve bağımlılık aracısı yüklü olan bağımlı makineler ayrı kutular olarak gösterilir.
    - Makine içinde çalışan süreçler, süreçleri görüntülemek için her makine kutusunu genişletebilirsiniz
    - FQDN, işletim sistemi, MAC adresi de dahil olmak üzere makine özellikleri gösterilir. Ayrıntıları görüntülemek için her makine kutusuna tıklayabilirsiniz.

4. Zaman aralığı etiketindeki zaman süresini tıklatarak farklı zaman süreleri için bağımlılıkları görüntüleyebilirsiniz. Varsayılan olarak aralık bir saattir. Zaman aralığını değiştirebilir veya başlangıç ve bitiş tarihlerini ve süresini belirtebilirsiniz.

   > [!NOTE]
   >    Bir saate kadar bir zaman aralığı desteklenir. Bağımlılık verilerini daha uzun bir süre boyunca [sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için Azure Monitor günlüklerini kullanın.

5. Gruplandırmak istediğiniz bağımlı makineleri belirledikten sonra, haritada birden fazla makine seçmek için Ctrl+Click'i kullanın ve **Makineleri Grupla'yı**tıklatın.
6. Bir grup adı belirtin. Bağımlı makinelerin Azure Geçiş tarafından keşfedildiğini doğrulayın.

    > [!NOTE]
    > Bağımlı bir makine Azure Geçiş tarafından keşfedilmezse, gruba ekemezsiniz. Bu tür makineleri gruba eklemek için, bulma işlemini vCenter Server'da doğru kapsamda yeniden çalıştırmanız ve makinenin Azure Geçiş tarafından bulunduğundan emin olmanız gerekir.  

7. Bu grup için bir değerlendirme oluşturmak istiyorsanız, grup için yeni bir değerlendirme oluşturmak için onay kutusunu seçin.
8. Grubu kaydetmek için **Tamam'ı** tıklatın.

Grup oluşturulduktan sonra, grubun tüm makinelerine aracılar yüklemeniz ve tüm grubun bağımlılığını görselleştirerek grubu hassaslaştırmaları önerilir.

## <a name="query-dependency-data-from-azure-monitor-logs"></a>Azure Monitor günlüklerinden bağımlılık verilerini sorgula

Hizmet Haritası tarafından yakalanan bağımlılık verileri, Azure Geçiş projenizle ilişkili Günlük Analizi çalışma alanında sorgu yapmak için kullanılabilir. Azure Monitor günlüklerinde sorgulanacak Hizmet Haritası veri tabloları hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) 

Kusto sorgularını çalıştırmak için:

1. Aracıları yükledikten sonra portala gidin ve **Genel Bakış'ı**tıklatın.
2. **Genel**Bakış'ta, projenin **Temel Bilgiler** bölümüne gidin ve **OMS Çalışma Alanı'nın**yanında sağlanan çalışma alanı adını tıklatın.
3. Günlük Analitiği çalışma alanı sayfasında **Genel** > **Günlükler'i**tıklatın.
4. Azure Monitor günlüklerini kullanarak bağımlılık verileri toplamak için sorgunuzu yazın. Sonraki bölümde örnek sorguları bulun.
5. Sorgunuzu Çalıştır'a tıklayarak çalıştırın. 

Kusto sorgularını nasıl yazacağım hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) 

### <a name="sample-azure-monitor-logs-queries"></a>Örnek Azure Monitor günlük sorguları

Aşağıda bağımlılık verilerini ayıklamak için kullanabileceğiniz örnek sorgular verebilirsiniz. Tercih ettiğiniz veri noktalarını ayıklamak için sorguları değiştirebilirsiniz. Bağımlılık veri kayıtlarındaki alanların ayrıntılı bir listesini [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records)bulabilirsiniz. [Burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches)daha fazla örnek sorgu bulabilirsiniz.

#### <a name="summarize-inbound-connections-on-a-set-of-machines"></a>Bir makine kümesindeki gelen bağlantıları özetle

Bağlantı ölçümleri için tablodaki kayıtlar, VMConnection, tek tek fiziksel ağ bağlantılarını temsil etmez. Birden çok fiziksel ağ bağlantısı mantıksal bir bağlantı halinde gruplandırılır. VMConnection'da fiziksel ağ bağlantısı verilerinin tek bir mantıksal kayıtta nasıl toplanmış olduğu hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) 

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="summarize-volume-of-data-sent-and-received-on-inbound-connections-between-a-set-of-machines"></a>Bir makine kümesi arasındaki gelen bağlantılarda gönderilen ve alınan verilerin hacmini özetle

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```


## <a name="next-steps"></a>Sonraki adımlar
Azure Geçiş'in en son sürümü [hakkında bilgi edinin.](migrate-services-overview.md)
