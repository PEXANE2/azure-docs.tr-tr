---
title: Azure'da Hizmet Haritası çözümlerini kullanma | Microsoft Dokümanlar
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede, Hizmet Eşlemi'ni ortamınızda dağıtmak ve çeşitli senaryolarda kullanmak için ayrıntılar verilmektedir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/24/2019
ms.openlocfilehash: f2f3e84462307f43ffe432fe878476d979f489f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480921"
---
# <a name="using-service-map-solution-in-azure"></a>Azure’da Hizmet Eşlemesi çözümünü kullanma

Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Hizmet Eşlemesi ile, sunucularınızı planladığınız şekilde kullanabilirsiniz: kritik hizmetler sunabilen birbirine bağlı sistemler. Hizmet Eşlemesi, aracının yüklenmesi dışında herhangi bir yapılandırma gerektirmeden sunucular, işlemler, gelen ve giden bağlantıların gecikme süresi ile TCP aracılığıyla bağlı mimarilerdeki bağlantı noktaları arasındaki bağlantıları gösterir.

Bu makalede, onboarding ve Hizmet Haritası kullanarak ayrıntıları açıklanmaktadır. Bu çözüm için ön koşulları yapılandırma hakkında daha fazla bilgi için [vm'lere genel bakış için Azure Monitörünü Etkinleştir'e](vminsights-enable-overview.md#prerequisites)bakın. Özetlemek gerekirse, aşağıdakilere ihtiyacınız var:

* Bu çözümü etkinleştirmek için bir Log Analytics çalışma alanı.

* Windows bilgisayarına veya Linux sunucusuna yüklenen Log Analytics aracısı, çözümü etkinleştirdiğiniz çalışma alanını rapor etmek üzere yapılandırılmıştır.

* Bağımlılık aracısı Windows bilgisayarına veya Linux sunucusuna yüklenir.

>[!NOTE]
>Hizmet Haritası'nı zaten dağıttıysanız, artık haritalarınızı VM'ler için Azure Monitörü'nde de görüntüleyebilirsiniz, bu da VM durumunu ve performansını izlemek için ek özellikler içerir. Daha fazla bilgi edinmek [için VM'lere genel bakış için Azure Monitor'a](../../azure-monitor/insights/vminsights-overview.md)bakın. Hizmet Haritası çözümü ile VM Haritası için Azure Monitörü arasındaki farklar hakkında bilgi edinmek için aşağıdaki [SSS'ye](../faq.md#azure-monitor-for-vms)bakın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="enable-service-map"></a>Hizmet Haritasını Etkinleştir

1. Hizmet Haritası çözümünü [Azure pazar](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ServiceMapOMS?tab=Overview) alanından veya [Çözüm Galerisi'nden İzleme Ekle çözümleri ekle'de](solutions.md)açıklanan işlemi kullanarak etkinleştirin.
1. [Windows'a Bağımlılık aracısını yükleyin](vminsights-enable-hybrid-cloud.md#install-the-dependency-agent-on-windows) veya veri almak istediğiniz her bilgisayara [Linux'taki Bağımlılık aracısını yükleyin.](vminsights-enable-hybrid-cloud.md#install-the-dependency-agent-on-linux) Bağımlılık Aracısı en yakındaki komşularla bağlantıları izleyebildiğinden her bilgisayarda bir aracıya ihtiyacınız olmayabilir.

Azure portalındaki Hizmet Haritası'na Log Analytics çalışma alanınızdan erişin ve sol bölmeden **Seçenek Çözümleri'ni** seçin.<br><br> ![Çalışma alanında](./media/service-map/select-solution-from-workspace.png)Çözümler seçeneğini seçin.<br> Çözümler listesinden **ServiceMap(workspaceName)** seçeneğini belirleyin ve Hizmet Haritası çözümüne genel bakış sayfasında Hizmet Haritası özet döşemesini tıklatın.<br><br> ![Hizmet Haritası özet](./media/service-map/service-map-summary-tile.png)döşeme .

## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Kullanım örnekleri: BT işlemlerinizi bağımlılıkbilincine hale getirin

### <a name="discovery"></a>Bulma

Hizmet Haritası, sunucularınız, süreçleriniz ve üçüncü taraf hizmetleriniz genelinde bağımlılıkların ortak bir başvuru haritasını otomatik olarak oluşturur. Tüm TCP bağımlılıklarını keşfeder ve eşler, sürpriz bağlantıları, bağlı olduğunuz uzak üçüncü taraf sistemleri ve Etkin Dizin gibi ağınızın geleneksel karanlık alanlarına bağımlılıkları tanımlar. Hizmet Haritası, yönetilen sistemlerinizin yapmaya çalıştığı başarısız ağ bağlantılarını keşfederek olası sunucu yapılandırmasını, hizmet kesintisini ve ağ sorunlarını belirlemenize yardımcı olur.

### <a name="incident-management"></a>Olay yönetimi

Hizmet Haritası, sistemlerin nasıl bağdaştırdığını ve birbirini nasıl etkilediğini göstererek sorun yalıtımı tahminini ortadan kaldırmaya yardımcı olur. Başarısız bağlantıları tanımlamaya ek olarak, yanlış yapılandırılmış yük dengeleyicilerinin, kritik hizmetlere şaşırtıcı veya aşırı yük lerin ve üretim sistemleriyle konuşan geliştirici makineleri gibi sahte istemcilerin belirlenmesine yardımcı olur. Değişiklik İzleme ile tümleşik iş akışlarını kullanarak, arka uç taki bir makinedeki veya hizmetteki bir değişiklik olayının bir olayın temel nedenini açıklayıp açıklamadığını da görebilirsiniz.

### <a name="migration-assurance"></a>Göç güvencesi

Hizmet Haritası'nı kullanarak, azure geçişlerini etkili bir şekilde planlayabilir, hızlandırabilir ve doğrulayabilir, bu da geride hiçbir şey kalmamasını ve sürpriz kesintilerin oluşmamasını sağlamaya yardımcı olur. Birlikte geçiş yapmak, sistem yapılandırmasını ve kapasitesini değerlendirmek ve çalışan bir sistemin kullanıcılara hizmet vermeye devam edip etmediğini veya geçiş yerine hizmet dışı bırakma için aday olup olmadığını belirlemek için birbirine bağlı tüm sistemleri keşfedebilirsiniz. Taşıma tamamlandıktan sonra, test sistemlerinin ve müşterilerin bağladığını doğrulamak için istemci yükünü ve kimliğini denetleyebilirsiniz. Alt ağ planlamanız ve güvenlik duvarı tanımlarınızın sorunları varsa, Hizmet Haritası eşlemlerinde başarısız bağlantılar sizi bağlantı gerektiren sistemlere yönlendirir.

### <a name="business-continuity"></a>İş sürekliliği

Azure Site Kurtarma kullanıyorsanız ve uygulama ortamınız için kurtarma sırasını tanımlamaya yardıma ihtiyacınız varsa, Servis Eşlemi, kurtarma planınızın güvenilir olduğundan emin olmak için sistemlerin birbirine nasıl güvendiğini otomatik olarak gösterebilir. Kritik bir sunucu veya grup seçerek ve istemcilerini görüntüleyerek, sunucu geri yüklendikten ve kullanılabilir hale geldikten sonra hangi ön uç sistemlerini kurtaracağını belirleyebilirsiniz. Tam tersine, kritik sunucuların arka uç bağımlılıklarına bakarak, odak sistemleriniz geri yüklenmeden önce hangi sistemlerin kurtarılacağını belirleyebilirsiniz.

### <a name="patch-management"></a>Düzeltme eki yönetimi

Hizmet Haritası, hangi takımların ve sunucuların hizmetinize bağlı olduğunu göstererek Sistem Güncelleştirme Değerlendirmesi'ni kullanımınızı artırır, böylece yama için sistemlerinizi kaldırmadan önce bunları önceden bildirebilirsiniz. Hizmet Haritası ayrıca, hizmetlerinizin kullanılabilir olup olmadığını ve yamalı ve yeniden başlatıldıktan sonra düzgün bir şekilde bağlanıp bağlanmadığınızı göstererek yama yönetimini geliştirir.

## <a name="mapping-overview"></a>Haritalama genel bakış

Hizmet Haritası aracıları, yüklendikleri sunucudaki TCP'ye bağlı tüm işlemler ve her işlem için gelen ve giden bağlantılar hakkında ayrıntılı bilgi toplar.

Sol bölmedeki listeden, belirli bir zaman aralığında bağımlılıklarını görselleştirmek için Hizmet Haritası aracıları olan makineleri veya grupları seçebilirsiniz. Makine bağımlılık haritaları belirli bir makineye odaklanır ve bu makineler, doğrudan TCP istemcileri veya o makinenin sunucuları olan tüm makineleri gösterir.  Machine Group eşlemleri sunucu kümelerini ve bunların bağımlılıklarını gösterir.

![Hizmet Haritasına genel bakış](media/service-map/service-map-overview.png)

Makineler, seçilen zaman aralığında etkin ağ bağlantıları olan çalışan işlem gruplarını ve süreçleri göstermek için haritada genişletilebilir. İşlem ayrıntılarını göstermek için Bir Hizmet Eşlemi aracısı olan uzak bir makine genişletildiğinde, yalnızca odak makinesiyle iletişim sağlayan işlemler gösterilir. Odak makinesine bağlanan aracısız ön uç makinelerin sayısı, bağlandıkları işlemlerin sol tarafında gösterilir. Odak makinesi aracısı olmayan bir arka uç makinesine bağlantı yapıyorsa, arka uç sunucusu aynı bağlantı noktası numarasına diğer bağlantılarla birlikte bir Server Port Group'a dahil edilir.

Varsayılan olarak, Hizmet Eşlemi eşlemleri bağımlılık bilgilerinin son 30 dakikasını gösterir. Sol üstteki zaman denetimlerini kullanarak, bağımlılıkların geçmişte nasıl göründüğünü (örneğin, bir olay sırasında veya değişiklik meydana gelmeden önce) göstermek için bir saate kadar geçmiş zaman aralıkları için haritaları sorgulayabilirsiniz. Hizmet Haritası verileri ücretli çalışma alanlarında 30 gün, ücretsiz çalışma alanlarında ise 7 gün saklanır.

## <a name="status-badges-and-border-coloring"></a>Durum rozetleri ve kenarlık boyama

Haritadaki her sunucunun alt kısmında, sunucu hakkında durum bilgilerini ileten durum rozetlerinin listesi olabilir. Rozetler, çözüm tümleştirmelerinden birinden sunucu için bazı ilgili bilgiler olduğunu gösterir. Rozeti tıklattığınızda, doğrudan sağ bölmedeki durumun ayrıntılarına götürür. Şu anda kullanılabilen durum rozetleri Uyarılar, Servis Masası, Değişiklikler, Güvenlik ve Güncelleştirmeleri içerir.

Durum rozetlerinin önem derecesine bağlı olarak, makine düğümü kenarlıkları kırmızı (kritik), sarı (uyarı) veya mavi (bilgilendirici) renklendirilebilir. Renk, durum rozetlerinden herhangi birinin en ağır durumunu temsil eder. Gri kenarlık, durum göstergesi olmayan bir düğümü gösterir.

![Durum rozetleri](media/service-map/status-badges.png)

## <a name="process-groups"></a>Süreç Grupları

İşlem Grupları, ortak bir ürün veya hizmetle ilişkili işlemleri bir işlem grubunda birleştirir.  Bir makine düğümü genişletildiğinde, işlem gruplarıyla birlikte bağımsız işlemler görüntüler.  Bir işlem grubu içindeki bir işleme gelen ve giden bağlantılar başarısız olduysa, bağlantı tüm işlem grubu için başarısız olarak gösterilir.

## <a name="machine-groups"></a>Makine Grupları

Makine Grupları, çok katmanlı bir uygulamanın veya sunucu kümesinin tüm üyelerini tek bir haritada görebilmeniz için sadece bir sunucu kümesietrafında ortalanmış haritaları görmenizi sağlar.

Kullanıcılar bir gruba hangi sunucuların ait olduğunu birlikte seçer ve grup için bir ad seçer.  Daha sonra grubu tüm süreçleri ve bağlantılarıyla görüntülemeyi seçebilir veya grubu yalnızca grubun diğer üyeleriyle doğrudan ilişkili işlemler ve bağlantılarla görüntüleyebilirsiniz.

![Makine Grubu](media/service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Makine Grubu Oluşturma

Bir grup oluşturmak için, Makineler listesinde istediğiniz makine yi veya makineleri seçin ve **gruba Ekle'yi**tıklatın.

![Grup Oluşturma](media/service-map/machine-groups-create.png)

Burada, yeni **oluştur'u** seçebilir ve gruba bir ad verebilirsiniz.

![Ad Grubu](media/service-map/machine-groups-name.png)

>[!NOTE]
>Makine grupları 10 sunucuyla sınırlıdır.

### <a name="viewing-a-group"></a>Grubu Görüntüleme

Bazı gruplar oluşturduktan sonra, Gruplar sekmesini seçerek bunları görüntüleyebilirsiniz.

![Gruplar sekmesi](media/service-map/machine-groups-tab.png)

Ardından, bu Makine Grubu'nun eşlemini görüntülemek için Grup adını seçin.
![Makine](media/service-map/machine-group.png) Grubu Gruba ait makineler haritada beyaz renkte özetlenmiştir.

Grubu genişletmek, Makine Grubunu oluşturan makineleri listeler.

![Makine Grubu makineleri](media/service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>İşleme göre filtreleme

Gruptaki tüm işlemleri ve bağlantıları göstermekle yalnızca Makine Grubuile doğrudan ilişkili olanları göstermek arasında harita görünümünü geçişyapabilirsiniz.  Varsayılan görünüm tüm işlemleri göstermektir.  Haritanın üzerindeki filtre simgesine tıklayarak görünümü değiştirebilirsiniz.

![Filtre Grubu](media/service-map/machine-groups-filter.png)

**Tüm işlemler** seçildiğinde, harita Gruptaki her makinedeki tüm işlemleri ve bağlantıları içerir.

![Makine Grubu tüm süreçler](media/service-map/machine-groups-all.png)

Görünümü yalnızca gruba bağlı **işlemleri**göstermek için değiştirirseniz, eşlemi yalnızca gruptaki diğer makinelere doğrudan bağlanan işlemlere ve bağlantılara göre daraltılacak ve basitleştirilmiş bir görünüm oluşturulur.

![Makine Grubu filtrelenmiş prosesler](media/service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Gruba makine ekleme

Varolan bir gruba makine eklemek için, istediğiniz makinelerin yanındaki kutuları işaretleyin ve ardından **gruba Ekle'yi**tıklatın.  Ardından, makineleri eklemek istediğiniz grubu seçin.
 
### <a name="removing-machines-from-a-group"></a>Makineleri bir gruptan çıkarma

Gruplar Listesinde, Makine Grubu'ndaki makineleri listelemek için grup adını genişletin.  Ardından kaldırmak istediğiniz makinenin yanındaki elips menüsüne tıklayın ve **Kaldır'ı**seçin.

![Makineyi gruptan çıkarma](media/service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Grubu kaldırma veya yeniden adlandırma

Grup Listesindeki grup adının yanındaki elips menüsüne tıklayın.

![Makine grubu menüsü](media/service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Rol simgeleri

Bazı işlemler makinelerde belirli rollere hizmet eder: web sunucuları, uygulama sunucuları, veritabanı, ve benzeri. Hizmet Haritası, bir işlemin veya sunucunun oynadığı rolü bir bakışta tanımlamaya yardımcı olmak için işlem ve makine kutularını rol simgeleriyle açıklamaolarak tanımlar.

| Rol simgesi | Açıklama |
|:--|:--|
| ![Web sunucusu](media/service-map/role-web-server.png) | Web sunucusu |
| ![Uygulama Sunucusu](media/service-map/role-application-server.png) | Uygulama sunucusu |
| ![Veritabanı adı](media/service-map/role-database.png) | Veritabanı adı |
| ![LDAP sunucusu](media/service-map/role-ldap.png) | LDAP sunucusu |
| ![Kobİ sunucusu](media/service-map/role-smb.png) | Kobİ sunucusu |

![Rol simgeleri](media/service-map/role-icons.png)


## <a name="failed-connections"></a>Başarısız bağlantılar

Başarısız bağlantılar, işlemler ve bilgisayarlar için Hizmet Haritası eşlerinde gösterilir ve istemci sisteminin bir işlem veya bağlantı noktasına ulaşamadığini gösteren kesik li kırmızı bir çizgi bulunur. Başarısız bağlantılar, başarısız bağlantıyı deneyen sistemse, dağıtılmış bir Hizmet Haritası aracısı olan herhangi bir sistemden bildirilir. Hizmet Haritası, bağlantı kuramayan TCP soketlerini gözlemleyerek bu işlemi ölçer. Bu hata, bir güvenlik duvarı, istemci veya sunucuda bir yanlış yapılandırma veya uzak bir hizmetin kullanılamamasından kaynaklanabilir.

![Başarısız bağlantılar](media/service-map/failed-connections.png)

Başarısız bağlantıları anlamak sorun giderme, geçiş doğrulama, güvenlik çözümlemesi ve genel mimari anlayışla yardımcı olabilir. Başarısız bağlantılar bazen zararsızdır, ancak genellikle bir başarısız ortamın aniden erişilemez hale gelmesi veya iki uygulama katmanının bulut geçişinden sonra konuşamaması gibi doğrudan bir soruna işaret eder.

## <a name="client-groups"></a>İstemci Grupları

İstemci Grupları, haritada Bağımlılık Aracıları olmayan istemci makinelerini temsil eden kutulardır. Tek bir İstemci Grubu, tek bir işlem veya makine için istemcileri temsil eder.

![İstemci Grupları](media/service-map/client-groups.png)

İstemci Grubundaki sunucuların IP adreslerini görmek için grubu seçin. Grubun içeriği **İstemci Grubu Özellikleri** bölmesinde listelenir.

![İstemci Grubu özellikleri](media/service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Sunucu Bağlantı Noktası Grupları

Sunucu Bağlantı Noktası Grupları, Bağımlılık Aracıları olmayan sunucularda sunucu bağlantı noktalarını temsil eden kutulardır. Kutu sunucu bağlantı noktasını ve bu bağlantı noktasına bağlantı olan sunucu sayısının sayısını içerir. Tek tek sunucuları ve bağlantıları görmek için kutuyu genişletin. Kutuda yalnızca bir sunucu varsa, ad veya IP adresi listelenir.

![Sunucu Bağlantı Noktası Grupları](media/service-map/server-port-groups.png)

## <a name="context-menu"></a>Bağlam menüsü

Herhangi bir sunucunun sağ üst kısmındaki elipsis 'i (...) tıklattığınızda, bu sunucunun bağlam menüsü görüntülenir.

![Başarısız bağlantılar](media/service-map/context-menu.png)

### <a name="load-server-map"></a>Sunucu haritasını yükleyin

Yükle **Sunucu Haritası'nı** tıklattığınızda, yeni odak makinesi olarak seçilen sunucuyla yeni bir haritaya geçersiniz.

### <a name="show-self-links"></a>Kendi bağlantılarını göster

**Self-Links'i Göster'i** tıklattığınızda, sunucu içindeki işlemlerde başlayıp biten TCP bağlantıları olan öz bağlantılar da dahil olmak üzere sunucu düğümünü yeniden çizer. Kendi kendine bağlantılar gösterilirse, menü komutu **Kendi Kendine Bağlantıları Gizle**olarak değişir, böylece onları kapatabilirsiniz.

## <a name="computer-summary"></a>Bilgisayar özeti

**Makine Özeti** bölmesi, sunucunun işletim sistemine, bağımlılık sayılarına ve diğer çözümlerden gelen verilere genel bir bakış içerir. Bu tür veriler performans ölçümlerini, servis masası biletlerini, değişiklik izlemeyi, güvenliği ve güncelleştirmeleri içerir.

![Makine Özeti bölmesi](media/service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Bilgisayar ve işlem özellikleri

Bir Hizmet Haritası haritasında gezinirken, özellikleri hakkında ek bağlam kazanmak için makineleri ve işlemleri seçebilirsiniz. Makineler DNS adı, IPv4 adresleri, CPU ve bellek kapasitesi, VM türü, işletim sistemi ve sürümü, son yeniden başlatma zamanı ve OMS ve Hizmet Haritası aracılarının kimliklerini hakkında bilgi sağlar.

![Makine Özellikleri bölmesi](media/service-map/machine-properties.png)

İşlem adı, işlem açıklaması, kullanıcı adı ve etki alanı (Windows'ta), şirket adı, ürün adı, ürün sürümü, çalışma dizini, komut satırı ve işlem dahil olmak üzere çalışma süreçleri yle ilgili işletim sistemi meta verilerinden işlem ayrıntılarını toplayabilirsiniz başlangıç zamanı.

![İşlem Özellikleri bölmesi](media/service-map/process-properties.png)

**İşlem Özeti** bölmesi, bağlı bağlantı noktaları, gelen ve giden bağlantılar ve başarısız bağlantılar da dahil olmak üzere işlemin bağlantısı hakkında ek bilgiler sağlar.

![İşlem Özeti bölmesi](media/service-map/process-summary.png)

## <a name="alerts-integration"></a>Uyarıları entegrasyonu

Hizmet Haritası, seçili zaman aralığında seçilen sunucu için uyarıları göstermek için Azure Uyarıları ile tümleşir. Sunucu geçerli uyarılar varsa bir simge görüntüler ve **Makine Uyarıları** bölmesi uyarıları listeler.

![Makine Uyarıları bölmesi](media/service-map/machine-alerts.png)

Hizmet Haritası'nın ilgili uyarıları görüntülemesini sağlamak için, belirli bir bilgisayar için yangın yapan bir uyarı kuralı oluşturun. Uygun uyarılar oluşturmak için:
- Bilgisayara göre gruplandırmak için bir yan tümce ekleyin (örneğin, **Bilgisayar aralığına göre 1 dakika).**
- Metrik ölçüme göre uyarı yapmayı seçin.

## <a name="log-events-integration"></a>Günlük olayları entegrasyonu

Hizmet Haritası, seçilen zaman aralığında seçilen sunucu için kullanılabilir tüm günlük olaylarının sayısını göstermek için Günlük Arama ile tümleşir. Günlük Arama'ya atlamak ve tek tek günlük olaylarını görmek için olay sayımları listesindeki herhangi bir satırı tıklatabilirsiniz.

![Makine Günlüğü Etkinlikleri bölmesi](media/service-map/log-events.png)

## <a name="service-desk-integration"></a>Servis Masası entegrasyonu

Bt Hizmeti Yönetimi Konektörü ile Hizmet Haritası entegrasyonu, her iki çözüm etkinleştirildiğinde ve Log Analytics çalışma alanınızda yapılandırıldığında otomatiktir. Hizmet Haritası'ndaki tümleştirme "Servis Masası" olarak etiketlenir. Daha fazla bilgi için BT [Hizmet Yönetimi Bağlayıcısı'nı kullanarak ITSM iş öğelerini merkezi olarak yönetin.](https://docs.microsoft.com/azure/log-analytics/log-analytics-itsmc-overview)

**Makine Servis Masası** bölmesi, seçili sunucunun seçili zaman aralığındaki tüm BT Hizmet Yönetimi olaylarını listeler. Sunucu, geçerli öğeler varsa bir simge görüntüler ve Makine Servis Masası bölmesi bunları listeler.

![Makine Servis Masası bölmesi](media/service-map/service-desk.png)

Bağlı ITSM çözümünüzdeki öğeyi açmak için **Çalışma Öğesini Görüntüle'yi**tıklatın.

Günlük Arama'da öğenin ayrıntılarını görüntülemek **için, Günlük Arama'da Göster'i**tıklatın.
Bağlantı ölçümleri Log Analytics'teki iki yeni tabloya yazılır 

## <a name="change-tracking-integration"></a>İzleme tümleştirmeyi değiştir

Her iki çözüm etkinleştirildiğinde ve Log Analytics çalışma alanınızda yapılandırıldığında Değişiklik İzleme ile Hizmet Haritası tümleştirmesi otomatiktir.

**Makine Değişikliği İzleme** bölmesi, en son ilk ilerle birlikte, ek ayrıntılar için Günlük Arama'ya ayrıntıya inmek üzere bir bağlantıyla birlikte tüm değişiklikleri listeler.

![Makine Değiştirme İzleme bölmesi](media/service-map/change-tracking.png)

Aşağıdaki resim, **Log Analytics'te Göster'i**seçtikten sonra görebileceğiniz configurationChange olayının ayrıntılı görünümüdür.

![ConfigurationChange olayı](media/service-map/configuration-change-event-01.png)

## <a name="performance-integration"></a>Performans entegrasyonu

**Makine Performansı** bölmesi seçili sunucu için standart performans ölçümlerini görüntüler. Ölçümler CPU kullanımı, bellek kullanımı, gönderilen ve alınan ağ baytları ve gönderilen ve alınan ağ baytları tarafından üst süreçlerin bir listesini içerir.

![Makine Performans bölmesi](media/service-map/machine-performance.png)

Performans verilerini görmek [için, uygun Log Analytics performans sayaçlarını etkinleştirmeniz](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters)gerekebilir.  Etkinleştirmek isteyeceğiniz sayaçlar:

Windows:
- İşlemci(*)\\% İşlemci Süresi
- Bellek\\% Kullanımda Bayt Taahhüt
- Ağ Bağdaştırıcısı(*)\\Gönderilen/sn
- Ağ Bağdaştırıcısı(*)\\Alınan/sn

Linux:
- İşlemci(*)\\% İşlemci Süresi
- Bellek(*)\\% Kullanılan Bellek
- Ağ Bağdaştırıcısı(*)\\Gönderilen/sn
- Ağ Bağdaştırıcısı(*)\\Alınan/sn

Ağ performansı verilerini almak için çalışma alanınızda Tel Verileri 2.0 çözümünüzü de etkinleştirmiş olmalısınız.
 
## <a name="security-integration"></a>Güvenlik entegrasyonu

Her iki çözüm etkinleştirildiğinde ve Log Analytics çalışma alanınızda yapılandırıldığında Güvenlik ve Denetim ile Hizmet Haritası tümleştirmesi otomatiktir.

**Makine Güvenliği** bölmesi, seçili sunucuiçin Güvenlik ve Denetim çözümünden gelen verileri gösterir. Bölme, seçili zaman aralığında sunucu için bekleyen güvenlik sorunlarının bir özetini listeler. Güvenlik sorunlarından herhangi birini tıklattığınızda, bunlarla ilgili ayrıntılar için Bir Günlük Araması'na girebilirsiniz.

![Makine Güvenlik bölmesi](media/service-map/machine-security.png)

## <a name="updates-integration"></a>Tümleştirmeyi güncelleştirir

Her iki çözüm etkinleştirildiğinde ve Log Analytics çalışma alanınızda yapılandırıldığında, Güncelleştirme Yönetimi ile Hizmet Haritası tümleştirmesi otomatiktir.

**Machine Updates** bölmesi, seçili sunucu için Güncelleştirme Yönetimi çözümünden gelen verileri görüntüler. Bölme, seçili zaman aralığında sunucu için eksik güncelleştirmelerin bir özetini listeler.

![Makine Değiştirme İzleme bölmesi](media/service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Servis Haritası bilgisayar ve proses envanteri verileri Log Analytics'te [arama](../../azure-monitor/log-query/log-query-overview.md) için kullanılabilir. Bu verileri geçiş planlaması, kapasite çözümlemesi, bulma ve isteğe bağlı performans sorun giderme gibi senaryolara uygulayabilirsiniz.

Bir işlem veya bilgisayar Hizmet Haritası'na başladığında veya yerleşik olduğunda oluşturulan kayıtlara ek olarak, her benzersiz bilgisayar ve işlem için saat başına bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tablolarda özelliklere sahiptir. ServiceMapComputer_CL olaylardaki alanlar ve değerler, ServiceMap Azure Kaynak Yöneticisi API'deki Makine kaynağının alanlarıyla eşlenin. ServiceMapProcess_CL olaylardaki alanlar ve değerler, ServiceMap Azure Kaynak Yöneticisi API'deki İşlem kaynağının alanlarına eşlenir. ResourceName_s alanı, ilgili Kaynak Yöneticisi kaynağındaki ad alanıyla eşleşir. 

>[!NOTE]
>Hizmet Haritası özellikleri büyüdükçe, bu alanlar değişebilir.

Benzersiz işlemleri ve bilgisayarları tanımlamak için kullanabileceğiniz dahili olarak oluşturulmuş özellikler vardır:

- Bilgisayar: Log Analytics çalışma alanı içindeki bir bilgisayarı benzersiz olarak tanımlamak için *ResourceId* veya *ResourceName_s* kullanın.
- İşlem: Log Analytics çalışma alanındaki bir işlemi benzersiz olarak tanımlamak için *ResourceId'yi* kullanın. *ResourceName_s,* işlemin yürütüldettiği makine bağlamında benzersizdir (MachineResourceName_s) 

Belirli bir işlem ve belirli bir zaman aralığındaki bilgisayar için birden çok kayıt bulunabildiği için, sorgular aynı bilgisayar veya işlem için birden fazla kayıt döndürebilir. Yalnızca en son kaydı eklemek için "| dedup ResourceId" sorgusuna.

### <a name="connections"></a>Bağlantılar

Bağlantı ölçümleri Log Analytics - VMConnection'daki yeni bir tabloya yazılır. Bu tablo, bir makinenin bağlantıları (gelen ve giden) hakkında bilgi sağlar. Bağlantı Ölçümleri, bir zaman penceresi sırasında belirli bir metrik elde etmek için araçlar sağlayan API'lerle de ortaya çıkarır.  Dinleme soketinde kabul etmekten kaynaklanan TCP bağlantıları gelen, belirli bir IP ve bağlantı noktasına bağlanarak oluşturulanlar ise giden. Bir bağlantının yönü, **gelen** veya **giden**olarak ayarlanabilen Yön özelliği yle temsil edilir. 

Bu tablolardaki kayıtlar Bağımlılık aracısı tarafından bildirilen verilerden oluşturulur. Her kayıt bir dakikalık zaman aralığında bir gözlemtemsil eder. TimeGenerated özelliği zaman aralığının başlangıcını gösterir. Her kayıt, ilgili varlığı, yani bağlantı veya bağlantı noktasını tanımlamak için bilgiler ve bu varlıkla ilişkili ölçümleri içerir. Şu anda, yalnızca IPv4 üzerinden TCP kullanılarak oluşan ağ etkinliği bildirilir.

Maliyet ve karmaşıklığı yönetmek için bağlantı kayıtları tek tek fiziksel ağ bağlantılarını temsil etmez. Birden çok fiziksel ağ bağlantısı, daha sonra ilgili tabloya yansıtılan mantıksal bir bağlantı halinde gruplandırılır.  Anlamı, *VMConnection* tablosundaki kayıtlar, gözlemlenen tek tek fiziksel bağlantıları değil, mantıksal bir gruplamayı temsil eder. Belirli bir dakika aralığında aşağıdaki öznitelikler için aynı değeri paylaşan fiziksel ağ bağlantısı, *VMConnection'da*tek bir mantıksal kayda toplanır. 

| Özellik | Açıklama |
|:--|:--|
| `Direction` |Bağlantının yönü, değer *gelen* veya *giden* |
| `Machine` |Bilgisayar FQDN |
| `Process` |Sürecin veya süreç gruplarının kimliği, bağlantıyı başlatma/kabul etme |
| `SourceIp` |Kaynağın IP adresi |
| `DestinationIp` |Hedefin IP adresi |
| `DestinationPort` |Hedefin bağlantı noktası numarası |
| `Protocol` |Bağlantı için kullanılan protokol.  Değerler *tcp'dir.* |

Gruplandırmanın etkisini hesaba katmak için, gruplanmış fiziksel bağlantı sayısı hakkında bilgiler kaydın aşağıdaki özelliklerinde sağlanır:

| Özellik | Açıklama |
|:--|:--|
| `LinksEstablished` |Raporlama süresi penceresi sırasında kurulan fiziksel ağ bağlantısı sayısı |
| `LinksTerminated` |Raporlama süresi sırasında sonlandırılan fiziksel ağ bağlantısı sayısı |
| `LinksFailed` |Raporlama süresi penceresinde başarısız olan fiziksel ağ bağlantısı sayısı. Bu bilgiler şu anda yalnızca giden bağlantılar için kullanılabilir. |
| `LinksLive` |Raporlama süresi penceresinin sonunda açık olan fiziksel ağ bağlantısı sayısı|

#### <a name="metrics"></a>Ölçümler

Bağlantı sayısı ölçümlerine ek olarak, belirli bir mantıksal bağlantı veya ağ bağlantı noktasına gönderilen ve alınan verilerin hacmi hakkındaki bilgiler de kaydın aşağıdaki özelliklerine dahil edilir:

| Özellik | Açıklama |
|:--|:--|
| `BytesSent` |Raporlama süresi sırasında gönderilen toplam bayt sayısı |
| `BytesReceived` |Raporlama süresi sırasında alınan toplam bayt sayısı |
| `Responses` |Raporlama süresi penceresinde gözlenen yanıt ların sayısı. 
| `ResponseTimeMax` |Raporlama süresi sırasında gözlenen en büyük yanıt süresi (milisaniye).  Değer yoksa, özellik boştur.|
| `ResponseTimeMin` |Raporlama süresi sırasında gözlenen en küçük yanıt süresi (milisaniye).  Değer yoksa, özellik boştur.|
| `ResponseTimeSum` |Raporlama süresi penceresinde gözlenen tüm yanıt sürelerinin (milisaniye) toplamı.  Değer yoksa, özellik boş|

Bildirilen üçüncü veri türü yanıt süresidir - bir arayanın bir bağlantı üzerinden gönderilen bir isteği bekleyerek ne kadar zaman harcadığı ve uzak bitiş noktası tarafından yanıtlandığıdır. Bildirilen yanıt süresi, temel uygulama protokolünün gerçek yanıt süresinin tahminidir. Fiziksel ağ bağlantısının kaynak ve hedef sonu arasındaki veri akışının gözlemine dayalı sezgisel kullanılarak hesaplanır. Kavramsal olarak, bir isteğin son baytının gönderenden ayrıldığı saat ile yanıtın son baytının ona geri geldiği saat arasındaki farktır. Bu iki zaman damgası, belirli bir fiziksel bağlantıdaki istek ve yanıt olaylarını ifade etmek için kullanılır. Aralarındaki fark, tek bir isteğin yanıt süresini temsil eder. 

Bu özelliğin bu ilk sürümünde, algoritmamız, belirli bir ağ bağlantısı için kullanılan gerçek uygulama protokolüne bağlı olarak değişen başarı dereceleriyle çalışabilecek bir yaklaşımdır. Örneğin, geçerli yaklaşım HTTP(S) gibi istek yanıtı tabanlı protokoller için iyi çalışır, ancak tek yönlü veya ileti sıra tabanlı iletişim ile çalışmaz.

Göz önünde bulundurulması gereken bazı önemli noktalar şunlardır:

1. Bir işlem aynı IP adresindeki ancak birden çok ağ arabirimi üzerindeki bağlantıları kabul ederse, her arabirim için ayrı bir kayıt bildirilir. 
2. Joker karakter IP'si içeren kayıtlar etkinlik içermez. Bunlar, makinedeki bir bağlantı noktasının gelen trafiğe açık olduğu gerçeğini temsil etmek için dahildir.
3. Ayrıntılı bilgi ve veri hacmini azaltmak için, belirli bir IP adresiyle eşleşen bir kayıt (aynı işlem, bağlantı noktası ve protokol için) olduğunda joker KARAKTER IP'li kayıtlar atlanır. Bir joker IP kaydı atlandığında, belirli IP adresine sahip IsWildcardBind kayıt özelliği, bağlantı noktasının raporlama makinesinin her arabirimi üzerinde açıkta olduğunu belirtmek için "True" olarak ayarlanır.
4. Yalnızca belirli bir arabirimde bağlanan bağlantı noktaları IsWildcardBind "False" olarak ayarlanmıştır.

#### <a name="naming-and-classification"></a>Adlandırma ve Sınıflandırma

Kolaylık sağlamak için, bir bağlantının uzak ucunun IP adresi RemoteIp özelliğine dahildir. Gelen bağlantılar için RemoteIp SourceIp ile aynıdır, giden bağlantılar için ise DestinationIp ile aynıdır. RemoteDnsCanonicalNames özelliği, RemoteIp için makine tarafından bildirilen DNS kanonik adlarını temsil eder. RemoteDnsQuestions ve RemoteClassification özellikleri ileride kullanılmak üzere ayrılmıştır. 

#### <a name="geolocation"></a>Coğrafi Konum

*VMConnection* ayrıca, her bağlantı kaydının uzak ucuiçin aşağıdaki kayıtlardaki coğrafi konum bilgilerini de içerir: 

| Özellik | Açıklama |
|:--|:--|
| `RemoteCountry` |RemoteIp'e ev sahipliği yapan ülke/bölgenin adı.  Örneğin, *Amerika Birleşik Devletleri* |
| `RemoteLatitude` |Coğrafi konum enlemi.  Örneğin, *47,68* |
| `RemoteLongitude` |Coğrafi konum boylam.  Örneğin, *-122,12* |

#### <a name="malicious-ip"></a>Kötü niyetli IP

*VMConnection* tablosundaki her RemoteIp özelliği, bilinen kötü amaçlı etkinlik le ilgili bir ip kümesiyle karşılaştırılır. RemoteIp kötü niyetli olarak tanımlanırsa, aşağıdaki özellikler kaydın aşağıdaki özelliklerinde doldurulur (IP kötü amaçlı olarak kabul edilmezse boşturlar)

| Özellik | Açıklama |
|:--|:--|
| `MaliciousIp` |RemoteIp adresi |
| `IndicatorThreadType` |Tespit edilen tehdit göstergesi aşağıdaki değerlerden biridir, *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos*, *MalwareUrl*, *Malware*, *Phishing*, *Proxy*, *PUA*, *İzleme Listesi*.   |
| `Description` |Gözlenen tehdidin tanımı. |
| `TLPLevel` |Trafik Işık Protokolü (TLP) Düzeyi tanımlanan değerlerden biridir, *Beyaz,* *Yeşil,* *Kehribar,* *Kırmızı*. |
| `Confidence` |Değerler *0 - 100 arasındadır.* |
| `Severity` |Değerler *0 - 5,* *5* en şiddetli ve *0* hiç şiddetli değildir. Varsayılan değer *3*3'dür.  |
| `FirstReportedDateTime` |Sağlayıcı göstergeyi ilk kez rapor etti. |
| `LastReportedDateTime` |Gösterge interflow tarafından en son görüldüğünde. |
| `IsActive` |Göstergelerin *True* veya *False* değeriyle devre dışı bırakıldığını gösterir. |
| `ReportReferenceLink` |Belirli bir gözlemlenebilir ile ilgili raporlara bağlantılar. |
| `AdditionalInformation` |Varsa, gözlenen tehdit hakkında ek bilgi sağlar. |

### <a name="servicemapcomputer_cl-records"></a>ServiceMapComputer_CL kayıtları

*ServiceMapComputer_CL* türüne sahip kayıtlar, Hizmet Haritası aracıları olan sunucular için envanter verilerine sahiptir. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama |
|:--|:--|
| `Type` | *ServiceMapComputer_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| `ResourceName_s` | Çalışma alanı içindeki bir makine için benzersiz tanımlayıcı |
| `ComputerName_s` | Bilgisayar FQDN |
| `Ipv4Addresses_s` | Sunucunun IPv4 adreslerinin listesi |
| `Ipv6Addresses_s` | Sunucunun IPv6 adreslerinin listesi |
| `DnsNames_s` | Bir dizi DNS adı |
| `OperatingSystemFamily_s` | Windows veya Linux |
| `OperatingSystemFullName_s` | İşletim sisteminin tam adı  |
| `Bitness_s` | Makinenin bitliği (32-bit veya 64-bit)  |
| `PhysicalMemory_d` | MB'de fiziksel bellek |
| `Cpus_d` | CPU sayısı |
| `CpuSpeed_d` | MHz'de CPU hızı|
| `VirtualizationState_s` | *bilinmeyen*, *fiziksel*, *sanal*, *hipervizör* |
| `VirtualMachineType_s` | *hyperv*, *vmware*, ve benzeri |
| `VirtualMachineNativeMachineId_g` | Hypervisor tarafından atanan VM Kimliği |
| `VirtualMachineName_s` | VM adı |
| `BootTime_t` | Önyükleme süresi |

### <a name="servicemapprocess_cl-type-records"></a>ServiceMapProcess_CL Türü kayıtları

Bir *ServiceMapProcess_CL* türüne sahip kayıtlar, Hizmet Haritası aracıları olan sunucularda TCP'ye bağlı işlemler için envanter verilerine sahiptir. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama |
|:--|:--|
| `Type` | *ServiceMapProcess_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Çalışma alanı içindeki bir işlem için benzersiz tanımlayıcı |
| `ResourceName_s` | Üzerinde çalışan makine içinde bir işlem için benzersiz tanımlayıcı|
| `MachineResourceName_s` | Makinenin kaynak adı |
| `ExecutableName_s` | Çalıştırılabilen işlemin adı |
| `StartTime_t` | İşlem havuzu başlangıç saati |
| `FirstPid_d` | İşlem havuzundaki ilk PID |
| `Description_s` | İşlem açıklaması |
| `CompanyName_s` | Şirketin adı |
| `InternalName_s` | Dahili ad |
| `ProductName_s` | Ürünün adı |
| `ProductVersion_s` | Ürün sürümü |
| `FileVersion_s` | Dosya sürümü |
| `CommandLine_s` | Komut satırı |
| `ExecutablePath _s` | Yürütülebilir dosyaya giden yol |
| `WorkingDirectory_s` | Çalışma dizini |
| `UserName` | İşlemin yürütüldettiği hesap |
| `UserDomain` | İşlemin yürütüldettiği etki alanı |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

### <a name="list-all-known-machines"></a>Bilinen tüm makineleri listele

ServiceMapComputer_CL | arg_max özetleyin(TimeGenerated, *) tarafından ResourceId

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Yönetilen tüm bilgisayarların fiziksel bellek kapasitesini listele.

ServiceMapComputer_CL | arg_max özetlemek(TimeGenerated, *) tarafından ResourceId | proje PhysicalMemory_d, ComputerName_s

### <a name="list-computer-name-dns-ip-and-os"></a>Bilgisayar adını, DNS, IP ve Işletim Sistemi'ni listele.

ServiceMapComputer_CL | arg_max özetlemek(TimeGenerated, *) tarafından ResourceId | proje ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "sql" olan tüm işlemleri bulun

ServiceMapProcess_CL | nerede CommandLine_s contains_cs "sql" | arg_max özetleyin(TimeGenerated, *) tarafından ResourceId

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Kaynak adına göre bir makine (en son kayıt) bulma

arama (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | arg_max özetleyin(TimeGenerated, *) tarafından ResourceId

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>IP adresine göre bir makine (en son kayıt) bulma

arama (ServiceMapComputer_CL) "10.229.243.232" | arg_max özetleyin(TimeGenerated, *) tarafından ResourceId

### <a name="list-all-known-processes-on-a-specified-machine"></a>Bilinen tüm işlemleri belirli bir makinede listele

ServiceMapProcess_CL | nerede MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | arg_max özetleyin(TimeGenerated, *) tarafından ResourceId

### <a name="list-all-computers-running-sql"></a>SQL çalıştıran tüm bilgisayarları listele

ServiceMapComputer_CL | nerede ResourceName_s ((search in (ServiceMapProcess_CL) "\*sql\*" | farklı MachineResourceName_s)) | farklı ComputerName_s

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Curl'ün tüm benzersiz ürün sürümlerini veri merkezimde listele

ServiceMapProcess_CL | nerede ExecutableName_s == "curl" | farklı ProductVersion_s

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarlardan oluşan bir bilgisayar grubu oluşturma

ServiceMapComputer_CL | nerede OperatingSystemFullName_s contains_cs "CentOS" | farklı ComputerName_s

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Bir grup makineden giden bağlantıları özetle

```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="rest-api"></a>REST API

Hizmet Haritası'ndaki tüm sunucu, işlem ve bağımlılık verilerine [Hizmet Haritası REST API](https://docs.microsoft.com/rest/api/servicemap/)üzerinden ulaşılabilir.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Hizmet Haritası hizmetini kullanımınız yoluyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri Hizmet Haritası hizmetinin kalitesini, güvenliğini ve bütünlüğünü sağlamak ve geliştirmek için kullanır. Doğru ve verimli sorun giderme özellikleri sağlamak için veriler, işletim sistemi ve sürüm, IP adresi, DNS adı ve iş istasyonu adı gibi yazılımınızın yapılandırması hakkında bilgiler içerir. Microsoft adlar, adresler veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Çevrimiçi Hizmetler Gizlilik Bildirimi'ne](https://go.microsoft.com/fwlink/?LinkId=512132)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Service Map tarafından toplanan verileri almak için Log Analytics'teki [günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) hakkında daha fazla bilgi edinin.

## <a name="troubleshooting"></a>Sorun giderme

Hizmet Haritası'nı yüklerken veya çalıştırmakta sorun yaşıyorsanız, bu bölüm size yardımcı olabilir. Sorununuzu hala çözemediyseniz, lütfen Microsoft Destek'e başvurun.

### <a name="dependency-agent-installation-problems"></a>Bağımlılık aracıyükleme sorunları

#### <a name="installer-prompts-for-a-reboot"></a>Yükleyici yeniden başlatma istemi
Bağımlılık aracısı *genellikle* yükleme veya kaldırma üzerine yeniden başlatma gerektirmez. Ancak, bazı nadir durumlarda, Windows Server yüklemeile devam etmek için yeniden başlatma gerektirir. Bu, genellikle Microsoft Visual C++ Yeniden Dağıtılabilir kitaplığı kilitli bir dosya nedeniyle yeniden başlatmayı gerektirdiğinde bir bağımlılık gerçekleşir.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--code_number-appears"></a>"Bağımlılık aracısı yüklenemez: Visual Studio Runtime kitaplıkları yüklenemedi (kod = [code_number])" görüntülenir

Microsoft Bağımlılık aracısı, Microsoft Visual Studio çalışma zamanı kitaplıkları üzerine kuruludur. Kitaplıkların yüklenmesi sırasında bir sorun varsa bir ileti alırsınız. 

Çalışma zamanı kitaplığı yükleyiciler %LOCALAPPDATA%\temp klasöründe günlükler oluşturur. Dosya, `dd_vcredist_arch_yyyymmddhhmmss.log` *kemer* `x86` veya `amd64` *yyyymmddhhmmss* tarih ve saat (24 saat saat) günlük oluşturulduğu yerdir. Günlük, yüklemeyi engelleyen sorun hakkında ayrıntılı bilgi sağlar.

Önce [en son çalışma zamanı kitaplıklarını](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) yüklemek yararlı olabilir.

Aşağıdaki tabloda kod numaraları ve önerilen çözünürlükler listelenir.

| Kod | Açıklama | Çözüm |
|:--|:--|:--|
| 0x17 | Kitaplık yükleyici, yüklenmemiş bir Windows güncelleştirmesi gerektirir. | En son kitaplık yükleyici günlüğüne bakın.<br><br>Bir başvuru `Windows8.1-KB2999226-x64.msu` bir satır `Error 0x80240017: Failed to execute MSU package,` tarafından takip edilirse KB2999226 yüklemek için ön koşullar yok. Windows makalesinde [Evrensel C Runtime'daki](https://support.microsoft.com/kb/2999226) önkoşullar bölümündeki yönergeleri izleyin. Ön koşulları yüklemek için Windows Update'i çalıştırmanız ve birden çok kez yeniden başlatmanız gerekebilir.<br><br>Microsoft Bağımlılık aracısı yükleyicisini yeniden çalıştırın. |

### <a name="post-installation-issues"></a>Kurulum sonrası sorunlar

#### <a name="server-doesnt-appear-in-service-map"></a>Sunucu Hizmet Haritası'nda görünmüyor

Bağımlılık aracısı yüklemeniz başarılı olduysa, ancak makinenizi Hizmet Haritası çözümünde görmüyorsanız:
* Bağımlılık aracısı başarıyla yüklendi mi? Hizmetin yüklü olup olmadığını ve çalıştırılıp çalışmadığınızı denetleyerek bunu doğrulayabilirsiniz.<br><br>
**Windows**: **Microsoft Bağımlılık aracısı**adlı hizmeti arayın.
**Linux**: **Microsoft-dependency-agent**çalışma süreci arayın.

* [Log Analytics ücretsiz katmanında](https://azure.microsoft.com/pricing/details/monitor/)mısın? Ücretsiz plan, en fazla beş benzersiz Servis Haritası makinesine izin verir. Önceki beş makine artık veri göndermese bile, sonraki makineler Hizmet Haritası'nda görünmez.

* Sunucunuz Azure Monitor Günlükleri'ne günlük ve perf veri gönderiyor mu? Azure Monitor\Logs'a gidin ve bilgisayarınız için aşağıdaki sorguyu çalıştırın: 

    ```kusto
    Usage | where Computer == "admdemo-appsvr" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

Sonuçlarda çeşitli etkinlikler aldınız mı? Veriler yeni mi? Bu nedenle, Log Analytics temsilciniz doğru çalışıyor ve çalışma alanıyla iletişim kuruyor. Değilse, makinenizdeki aracıyı kontrol edin: [Windows sorun giderme için Log Analytics aracısı](../platform/agent-windows-troubleshoot.md) veya [Linux sorun giderme için Log Analytics aracısı.](../platform/agent-linux-troubleshoot.md)

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Sunucu Hizmet Haritası'nda görünür, ancak işlem yoktur

Makinenizi Hizmet Haritası'nda görürseniz, ancak işlem veya bağlantı verisi yoksa, bu Bağımlılık aracısının yüklü ve çalıştığını, ancak çekirdek sürücüsünün yüklemediğini gösterir. 

(Windows) veya `C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log file` `/var/opt/microsoft/dependency-agent/log/service.log file` (Linux) kontrol edin. Dosyanın son satırları çekirdeğin neden yüklenmediğini belirtmelidir. Örneğin, çekirdeğinizi güncellediyseniz çekirdek Linux'ta desteklenmeyebilir.

## <a name="feedback"></a>Geri Bildirim

Hizmet Haritası veya bu belgeler hakkında herhangi bir geri bildiriminiz var mı?  Özellikler önerebileceğiniz veya mevcut önerileri oylayabileceğiniz [Kullanıcı Sesi sayfamızı](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map)ziyaret edin.
