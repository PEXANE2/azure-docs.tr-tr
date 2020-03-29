---
title: Klasikten Azure Kaynak Yöneticisi'ne geçiş planlama
description: IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama
services: virtual-machines-windows
documentationcenter: ''
author: tanmaygore
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 62cc33b9cfe1a0dc96f0a6a771b753ff48bfb9f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919558"
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama

> [!IMPORTANT]
> Bugün, IaaS VM'lerinin yaklaşık %90'ı [Azure Kaynak Yöneticisi](https://azure.microsoft.com/features/resource-manager/)kullanıyor. 28 Şubat 2020 itibariyle, klasik VM'ler amortismana kaldırılmıştır ve 1 Mart 2023'te tamamen emekliye ayırılacaktır. Bu amortisman ve sizi [nasıl etkilediği](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)hakkında [daha fazla bilgi edinin.]( https://aka.ms/classicvmretirement)

Azure Kaynak Yöneticisi birçok şaşırtıcı özellik sunsa da, işlerin yolunda gittiğinden emin olmak için geçiş seyahatinizi planlamak çok önemlidir. Planlamaya zaman harcamak, geçiş etkinliklerini yürükarırken sorunlarla karşılaşmamanızı sağlar.

Göç yolculuğunun dört genel aşaması vardır:<br>

![Geçiş aşamaları](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Planlama

### <a name="technical-considerations-and-tradeoffs"></a>Teknik hususlar ve dengeler

Teknik gereksinimlerinize, coğrafyalarınıza ve operasyonel uygulamalarınıza bağlı olarak şunları göz önünde bulundurmak isteyebilirsiniz:

1. Azure Kaynak Yöneticisi kuruluşunuz için neden isteniyor?  Göçün iş nedenleri nelerdir?
2. Azure Kaynak Yöneticisi'nin teknik nedenleri nelerdir?  Hangi (varsa) ek Azure hizmetlerinden yararlanmak istersiniz?
3. Geçişe hangi uygulama (veya sanal makine kümeleri) dahildir?
4. Geçiş API'si ile hangi senaryolar desteklenir?  [Desteklenmeyen özellikleri ve yapılandırmaları gözden geçirin.](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations)
5. Operasyonel ekipleriniz artık Hem Klasik hem de Azure Kaynak Yöneticisi'ndeki uygulamaları/VM'leri destekleyecek mi?
6. Azure Kaynak Yöneticisi VM dağıtım, yönetim, izleme ve raporlama işlemlerinizi nasıl (hiç de olsa) değiştirir?  Dağıtım komut dosyalarınızın güncelleştirilmesi gerekiyor mu?
7. Paydaşları (son kullanıcılar, uygulama sahipleri ve altyapı sahipleri) uyarmak için iletişim planı nedir?
8. Ortamın karmaşıklığına bağlı olarak, uygulamanın son kullanıcılar ve uygulama sahipleri tarafından kullanılamadığı bir bakım dönemi olmalı mı?  Eğer öyleyse, ne kadar?
9. Paydaşların Azure Kaynak Yöneticisi'nde bilgili ve yetkin olmalarını sağlamak için eğitim planı nedir?
10. Geçiş için program yönetimi veya proje yönetim planı nedir?
11. Azure Kaynak Yöneticisi geçişi ve diğer ilgili teknoloji yol haritaları için zaman çizelgeleri nelerdir?  En uygun şekilde hizalanmışlar mı?

### <a name="patterns-of-success"></a>Başarı kalıpları

Başarılı müşteriler, önceki soruların tartışıldığı, belgelendiği ve yönetildiği ayrıntılı planlara sahiptir.  Göç planlarının sponsorlara ve paydaşlara geniş bir şekilde iletildiğinden emin olun.  Geçiş seçenekleriniz hakkında bilgi sahibi olun; aşağıda belirlenen bu geçiş belgesi ni okumanız önerilir.

* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için PowerShell'i kullanın](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için CLI'yi kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne geçişine yardımcı olmak için topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirme yle ilgili en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="pitfalls-to-avoid"></a>Tuzaklar önlemek için

- Plan yapmamak.  Bu göçün teknoloji adımları kanıtlanmıştır ve sonucu tahmin edilebilir.
- Platformun desteklenen geçiş API'sinin tüm senaryoları açıklaşacağı varsayımı. Hangi senaryoların desteklenedildiğini anlamak için [desteklenmeyen özellikleri ve yapılandırmaları](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations) okuyun.
- Son kullanıcılar için olası uygulama kesintisi planlamama.  Son kullanıcıları kullanılabilir olabilecek uygulama süresi konusunda yeterince uyaracak yeterli arabellek planlayın.


## <a name="lab-test"></a>Laboratuvar Testi

**Ortamınızı çoğaltma ve test geçişi yapma**
  > [!NOTE]
  > Varolan ortamınızın tam çoğaltması, Microsoft Desteği tarafından resmi olarak desteklenmeyen topluluk tarafından katkıda bulunulan bir araç kullanılarak gerçekleştirilir. Bu nedenle, **isteğe bağlı** bir adımdır, ancak üretim ortamlarınıza dokunmadan sorunları öğrenmenin en iyi yoludur. Topluluk tarafından katkıda bulunulan bir araç kullanmak bir seçenek değilse, aşağıdaki Kuru Çalıştırmayı Doğrula/Hazırla/İptal et önerisini okuyun.
  >

  Tam senaryonuz (bilgi işlem, ağ ve depolama) için bir laboratuvar testi yapmak, sorunsuz bir geçiş sağlamanın en iyi yoludur. Bu, şunları sağlamaya yardımcı olur:

- Test etmek için tamamen ayrı bir laboratuvar veya mevcut üretim dışı bir ortam. Tekrar tekrar geçirilebilen ve yıkıcı bir şekilde değiştirilebilen tamamen ayrı bir laboratuvar öneririz.  Gerçek aboneliklerden meta verileri toplamak/nemlendirmek için komut dosyaları aşağıda listelenmiştir.
- Laboratuarı ayrı bir abonelikle oluşturmak iyi bir fikirdir. Bunun nedeni, laboratuvarın tekrar tekrar yıkılacak olması ve ayrı, izole bir aboneye sahip olmanın gerçek bir şeyin yanlışlıkla silinme olasılığını azaltacak olmasıdır.

  Bu, AsmMetadataParser aracı kullanılarak gerçekleştirilebilir. [Burada bu araç hakkında daha fazla bilgi edinin](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Başarı kalıpları

Aşağıdaki sorunlar büyük göçlerin çoğunda keşfedilen edildi. Bu kapsamlı bir liste değildir ve daha fazla ayrıntı için [desteklenmeyen özelliklere ve yapılandırmalara](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#unsupported-features-and-configurations) başvurmalısınız.  Bu teknik sorunlarla karşılaşabilirsiniz veya karşılaşmayabilirsiniz, ancak geçiş girişiminden önce bunları çözerseniz daha sorunsuz bir deneyim sağlarsınız.

- **Bir Doğrula/Hazırla/Kuru Çalıştırmayı İptal Et** - Bu, Klasik ten Azure Kaynak Yöneticisi'ne geçiş başarısını sağlamak için belki de en önemli adımdır. Geçiş API'sinin üç ana adımı vardır: Doğrula, Hazırla ve İşle. Doğrulama, klasik ortamınızın durumunu okur ve tüm sorunların sonucunu döndürecektir. Ancak, Azure Kaynak Yöneticisi yığınında bazı sorunlar olabileceğinden, Doğrulama her şeyi yakalamaz. Geçiş sürecinde bir sonraki adım, Hazırlayın bu sorunları ortaya yardımcı olacaktır. Prepare meta verileri Klasik'ten Azure Kaynak Yöneticisi'ne taşır, ancak taşımayı gerçekleştirmez ve Klasik taraftahiçbir şeyi kaldırmaz veya değiştirmez. Kuru çalıştırma, göçün hazırlanmasını, daha sonra göçün hazırlanmasını **(taahhüt etmemeyi)** içerir. Kuru çalıştırmayı doğrulama/hazırlama/iptal etme amacı, Azure Kaynak Yöneticisi yığınındaki tüm meta verileri görmek,*(programlı olarak veya Portal'da)* incelemek ve her şeyin doğru şekilde geçirilmeye çalıştığını doğrulamak ve teknik sorunlarla çalışmaktır.  Ayrıca, buna göre kesinti planı böylece geçiş süresi duygusu verecektir.  Doğrulama/hazırlama/iptal etme herhangi bir kullanıcının kapalı kalma süresine neden olmaz; bu nedenle, uygulama kullanımı için non-kesinti değildir.
  - Aşağıdaki öğelerin kuru çalıştırmadan önce çözülmesi gerekir, ancak kuru bir çalışma testi de kaçırılırsa bu hazırlık adımlarını güvenli bir şekilde dışarı atacaktır. Kurumsal göç sırasında, kuru çalıştırmayı göçe hazır olmasını sağlamanın güvenli ve paha biçilmez bir yolu olarak bulduk.
  - Prepare çalışırken, denetim düzlemi (Azure yönetim işlemleri) tüm sanal ağ için kilitlenir, böylece doğrulama/hazırlama/iptal etme sırasında VM meta verilerinde değişiklik yapılamaz.  Ancak aksi takdirde herhangi bir uygulama işlevi (RD, VM kullanımı, vb) etkilenmez.  VM kullanıcıları kuru çalıştırmanın yürütülmekte olduğunu bilmeyecek.

- **Ekspres Rota Devreleri ve VPN**. Şu anda yetkilendirme bağlantıları olan Express Route Ağ Geçitleri kapalı kalma süresi olmadan geçirilemez. Geçici çözüm için bkz: Geçiş [ExpressRoute devreleri ve ilişkili sanal ağlar klasikten Kaynak Yöneticisi dağıtım modeline.](../../expressroute/expressroute-migration-classic-resource-manager.md)

- **VM Uzantıları** - Sanal Makine uzantıları, çalışan VM'lerin geçişinin en büyük engellerinden biridir. VM Uzantılarının düzeltilmesi 1-2 günden fazla sürebilir, bu nedenle buna göre plan layın.  Çalışan VM'lerin VM Uzantısı durumunu geri bildirmek için çalışan bir Azure aracısı gerekir. Durum çalışan bir VM için kötü olarak geri gelirse, bu geçişi durdurur. Geçişi etkinleştirmek için aracının kendisinin çalışıyor olması gerekmez, ancak VM'de uzantılar varsa, geçişin ilerlemesi için hem çalışan bir aracı hem de giden internet bağlantısı (DNS ile) gerekir.
  - Geçiş sırasında bir DNS sunucusuna bağlantı kaybolursa, BGInfo sürüm 1 dışındaki tüm VM Uzantıları. \* geçiş ler hazırlanmadan önce her VM'den kaldırılması ve daha sonra Azure Kaynak Yöneticisi geçişinden sonra VM'ye yeniden eklenmesi gerekir.  **Bu yalnızca çalışan VM'ler içindir.**  VM'ler devre denilmeye durdurulursa, VM Uzantılarının kaldırılması gerekmez.

  > [!NOTE]
  > Azure tanılama ve güvenlik merkezi izleme gibi birçok uzantı geçişten sonra kendilerini yeniden yükler, bu nedenle bunları kaldırmak sorun değildir.

  - Ayrıca, Ağ Güvenlik Grupları'nın giden internet erişimini kısıtlamadığından emin olun. Bu, bazı Ağ Güvenlik Grupları yapılandırmalarında olabilir. VM Uzantılarının Azure Kaynak Yöneticisi'ne geçirilen için giden internet erişimi (ve DNS) gereklidir.
  - BGInfo uzantısı iki sürümü var ve sürüm 1 ve 2 denir.  

      - VM BGInfo sürüm 1 uzantısı kullanıyorsa, olduğu gibi bu uzantısı bırakabilirsiniz. Geçiş API bu uzantısı atlar. BGInfo uzantısı geçişten sonra eklenebilir.
      - VM JSON tabanlı BGInfo sürüm 2 uzantısıkullanıyorsa, VM Azure portalı kullanılarak oluşturulmuştur. Geçiş API'si, aracının çalışması ve giden internet erişimi (ve DNS) olması koşuluyla Azure Kaynak Yöneticisi'ne geçişte bu uzantıyı içerir.

  - **Düzeltme Seçeneği 1**. VM'lerinizin giden internet erişimi, çalışan bir DNS hizmeti ve VM'lerde çalışan Azure aracıları olmayacağını biliyorsanız, hazırlamadan önce geçişin bir parçası olarak tüm VM uzantılarını kaldırın ve geçişten sonra VM Uzantılarını yeniden yükleyin.
  - **Düzeltme Seçeneği 2**. VM uzantıları çok büyük bir engelse, başka bir seçenek geçişişleminden önce tüm VM'leri kapatma/anlaşma yapmaktır. Ayrılan VM'leri geçirin ve Azure Kaynak Yöneticisi tarafında yeniden başlatın. Buradaki yarar, VM uzantılarının geçiş yapacak olmasıdır. Dezavantajı sanal IP'ler karşı karşıya tüm kamu kaybolur (bu olmayan bir başlangıç olabilir) ve tabii ki VM'ler çalışma uygulamaları üzerinde çok daha büyük bir etkiye neden kapatacaktır.

    > [!NOTE]
    > Bir Azure Güvenlik Merkezi ilkesi, taşınan VM'lere göre yapılandırılırsa, uzantıları kaldırmadan önce güvenlik ilkesinin durdurulması gerekir, aksi takdirde güvenlik izleme uzantısı kaldırdıktan sonra Otomatik olarak VM'de yeniden yüklenir.

- **Kullanılabilirlik Setleri** - Bir sanal ağın (vNet) Azure Kaynak Yöneticisi'ne geçirilebilmek için, Içerdiği Klasik dağıtım (yani bulut hizmeti) VM'lerin tümü tek bir kullanılabilirlik kümesinde olmalıdır veya Sanal M'lerin tümü herhangi bir kullanılabilirlik kümesinde olmamalıdır. Bulut hizmetinde birden fazla kullanılabilirlik kümesiolması Azure Kaynak Yöneticisi ile uyumlu değildir ve geçişi durdurur.  Ayrıca, kullanılabilirlik kümesinde bazı VM'ler ve kullanılabilirlik kümesinde olmayan bazı VM'ler de olamaz. Bunu çözmek için bulut hizmetinizi düzeltmeniz veya değiştirmeniz gerekir.  Bu zaman alıcı olabilir gibi buna göre planlayın.

- **Web/İşçi Rol Dağıtımları** - Web ve çalışan rolleri içeren Bulut Hizmetleri Azure Kaynak Yöneticisi'ne geçirilemez. Web ve çalışan rollerinizin içeriğini geçirmek için kodun kendisini daha yeni PaaS Uygulama Hizmetlerine geçirmeniz gerekir (bu tartışma bu belgenin kapsamı dışındadır). Web/worker rollerini olduğu gibi bırakmak ancak klasik VM'leri Kaynak Yöneticisi dağıtım modeline geçirmek istiyorsanız, geçiş başlamadan önce web/işçi rollerinin önce sanal ağdan kaldırılması gerekir.  Tipik bir çözüm, web/işçi rol örneklerini expressroute devresine de bağlı olan ayrı bir Klasik sanal ağa taşımaktır. Eski yeniden dağıtma durumunda, yeni bir Klasik sanal ağ oluşturun, web/işçi rollerini bu yeni sanal ağa taşıyın/yeniden dağıtın ve ardından dağıtımları taşınan sanal ağdan silin. Kod değişikliği gerekmez. Yeni [Sanal Ağ Eşleme](../../virtual-network/virtual-network-peering-overview.md) özelliği, aynı Azure bölgesindeki web/işçi rollerini ve diğer sanal ağları içeren klasik sanal ağı birlikte eşlemek için kullanılabilir **(öteden sanal ağ geçirilemediği için sanal ağ geçişi tamamlandıktan sonra)** performans kaybı ve gecikme/bant genişliği cezası olmadan aynı yetenekleri sağlar. Sanal Ağ [Eşlemesi](../../virtual-network/virtual-network-peering-overview.md)eklenmesi göz önüne alındığında, web/işçi rol dağıtımları artık kolayca azaltılabilir ve Azure Kaynak Yöneticisi'ne geçişi engellemeyebilir.

- **Azure Kaynak Yöneticisi Kotaları** - Azure bölgeleri, hem Klasik hem de Azure Kaynak Yöneticisi için ayrı kotalara/sınırlara sahiptir. Geçiş senaryosunda yeni donanım tüketilmese de *(mevcut VM'leri Klasik'ten Azure Kaynak Yöneticisi'ne değiştiriyoruz)* rağmen, geçiş başlamadan önce Azure Kaynak Yöneticisi kotalarının yeterli kapasiteyle yerinde olması gerekir. Aşağıda sorunlara neden gördüğümüz ana sınırları vardır.  Sınırları yükseltmek için kota destek bileti açın.

    > [!NOTE]
    > Bu sınırların, geçirilecek mevcut ortamınızla aynı bölgede yükseltilmesi gerekir.
    >

  - Ağ Arabirimleri
  - Yük Dengeleyiciler
  - Genel IP'ler
  - Statik Genel IP'ler
  - Çekirdekler
  - Ağ Güvenlik Grupları
  - Yönlendirme Tabloları

    Azure PowerShell'in en son sürümüyle aşağıdaki komutları kullanarak geçerli Azure Kaynak Yöneticisi kotalarınızı kontrol edebilirsiniz.



    **İşlem** *(Çekirdekler, Kullanılabilirlik Kümeleri)*

    ```powershell
    Get-AzVMUsage -Location <azure-region>
    ```

    **Ağ** *(Sanal Ağlar, Statik Genel IP'ler, Genel IP'ler, Ağ Güvenlik Grupları, Ağ Arabirimleri, Yük Dengeleyicileri, Rota Tabloları)*

    ```powershell
    Get-AzUsage /subscriptions/<subscription-id>/providers/Microsoft.Network/locations/<azure-region> -ApiVersion 2016-03-30 | Format-Table
    ```

    **Depolama** *(Depolama Hesabı)*

    ```powershell
    Get-AzStorageUsage
    ```

- **Azure Kaynak Yöneticisi API azaltma sınırları** - Yeterince büyük bir ortamınız varsa (örneğin. VNET'te 400 VM'>, Azure Kaynak Yöneticisi'ndeki (şu anda) `1200 writes/hour`yazmalar için varsayılan API azaltma sınırlarını vurabilirsiniz. Geçişe başlamadan önce, aboneliğiniz için bu sınırı artırmak için bir destek bileti yükseltmeniz gerekir.


- **Zamanlanmış VM Durumunu Sağlama** - Herhangi bir `provisioning timed out`VM,geçiş öncesi çözülmesi gereken bir durum varsa. Bunu yapmanın tek yolu, VM'yi devre-kapalı tutma/yeniden sağlama (silme, diski saklama ve VM'yi yeniden oluşturma) ile birlikte kesintidir.

- **RoleStateUnknown VM Durumu** - Bir `role state unknown` hata iletisi nedeniyle geçiş durursa, portalı kullanarak VM'yi inceleyin ve çalıştığını sağlayın. Bu hata genellikle kendi başına gidecek (hiçbir düzeltme gerekli) birkaç dakika sonra ve genellikle geçici bir `start` `stop`tür `restart` genellikle Sanal Makine sırasında görülen , işlemleri. **Önerilen uygulama:** birkaç dakika sonra geçişi yeniden deneyin.

- **Kumaş Kümesi yok** - Bazı durumlarda, bazı VM'ler çeşitli garip nedenlerle geçirilemez. Bu bilinen durumlardan biri, VM'nin yakın zamanda (son bir hafta içinde) oluşturulduğu ve Azure Kaynak Yöneticisi iş yükleri için henüz donanıma sahip olmayan bir Azure kümesi nin inmesi durumunda olduğudur.  Bunu söyleyen `fabric cluster does not exist` bir hata alırsınız ve VM geçirilemez. Küme yakında Azure Kaynak Yöneticisi'ni etkinleştireceği için birkaç gün beklemek genellikle bu sorunu çözer. Ancak, hemen bir geçici `stop-deallocate` çözüm VM, sonra geçiş ile devam ve geçiş ten sonra Azure Kaynak Yöneticisi VM geri başlatın.

### <a name="pitfalls-to-avoid"></a>Tuzaklar önlemek için

- Kısayollar almayın ve kuru çalıştırma geçişlerini doğrulama/hazırlama/iptal etme hatalarını atlayın.
- Tüm değilse de, potansiyel sorunlarınızın çoğu doğrulama/hazırlama/iptal adımları sırasında ortaya çıkar.

## <a name="migration"></a>Geçiş

### <a name="technical-considerations-and-tradeoffs"></a>Teknik hususlar ve dengeler

Artık hazırsınız, çünkü çevrenizle ilgili bilinen sorunları çözdünüz.

Gerçek geçişler için şunları göz önünde bulundurmak isteyebilirsiniz:

1. Sanal ağı (en küçük geçiş birimi) artan önceliğe sahip planlayın ve zamanlayın.  Önce basit sanal ağları yapın ve daha karmaşık sanal ağlarla ilerleme kaydedin.
2. Çoğu müşteri üretim dışı ve üretim ortamları olacaktır.  Üretimi en son zamanlayın.
3. **(İsteğE bağlı)** Beklenmeyen sorunlar ortaya çıkması durumunda bol miktarda arabellek içeren bir bakım kapalı kalma süresi zamanlayın.
4. Sorunlar ortaya çıkması durumunda destek ekiplerinizle iletişim kurun ve onlarla hizaya

### <a name="patterns-of-success"></a>Başarı kalıpları

_Laboratuvar Testi_ bölümündeki teknik kılavuz, gerçek bir geçişten önce dikkate alınmalı ve azaltılmalıdır.  Yeterli sınama ile, geçiş aslında olmayan bir olaydır.  Üretim ortamları için, güvenilir bir Microsoft iş ortağı veya Microsoft Premier hizmetleri gibi ek desteğe sahip olmak yararlı olabilir.

### <a name="pitfalls-to-avoid"></a>Tuzaklar önlemek için

Tam olarak sınanmamak geçişte sorunlara ve gecikmeye neden olabilir.  

## <a name="beyond-migration"></a>GeçişIn Ötesinde

### <a name="technical-considerations-and-tradeoffs"></a>Teknik hususlar ve dengeler

Artık Azure Kaynak Yöneticisi'nde olduğunuza göre platformu en üst düzeye çıkarın.  Ek avantajlar hakkında bilgi edinmek için [Azure Kaynak Yöneticisi'nin genel görünümünü](../../azure-resource-manager/management/overview.md) okuyun.

Göz önünde bulundurulması gerekenler:

- Göçü diğer etkinliklerle birleştirme.  Çoğu müşteri bir uygulama bakım penceresi tercih.  Bu nedenle, şifreleme ve Yönetilen Disklere geçiş gibi diğer Azure Kaynak Yöneticisi özelliklerini etkinleştirmek için bu kapalı kalma süresini kullanmak isteyebilirsiniz.
- Azure Kaynak Yöneticisi'nin teknik ve iş nedenlerini yeniden gözden geçirin; yalnızca Azure Kaynak Yöneticisi'nde bulunan ve ortamınız için geçerli olan ek hizmetleri etkinleştirin.
- PaaS hizmetleri ile çevrenizi modernleştirin.

### <a name="patterns-of-success"></a>Başarı kalıpları

Azure Kaynak Yöneticisi'nde etkinleştirmek istediğiniz hizmetleri bilerek etkinleştirin.  Birçok müşteri Azure ortamları için aşağıdakileri ilgi çekici bulur:

- [Role Based Access Control](../../role-based-access-control/overview.md).
- [Azure Kaynak Yöneticisi şablonları daha kolay ve daha kontrollü dağıtım için.](../../azure-resource-manager/templates/overview.md)
- [Etiketler](../../azure-resource-manager/management/tag-resources.md).
- [Etkinlik Kontrolü](../../azure-resource-manager/management/view-activity-logs.md)
- [Azure İlkeleri](../../governance/policy/overview.md)

### <a name="pitfalls-to-avoid"></a>Tuzaklar önlemek için

Bu Klasik -Azure Kaynak Yöneticisi geçiş yolculuğunu neden başlattığınızı hatırlayın.  Orijinal iş nedenleri nelerdi? İş sebebini elde ettin mi?


## <a name="next-steps"></a>Sonraki adımlar

* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için PowerShell'i kullanın](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için CLI'yi kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [KAYNAK Yöneticisi geçişine VPN Ağ Geçidi klasiği](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-classic-resource-manager-migration)
* [ExpressRoute devrelerini ve ilişkili sanal ağları klasikten Kaynak Yöneticisi dağıtım modeline geçirin](https://docs.microsoft.com/azure/expressroute/expressroute-migration-classic-resource-manager)
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne geçişine yardımcı olmak için topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirme yle ilgili en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
