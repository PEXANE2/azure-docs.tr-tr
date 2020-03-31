---
title: Veri Fabrikası için Veri Yönetimi Ağ Geçidi
description: Verileri şirket içi ve bulut arasında taşımak için bir veri ağ geçidi ayarlayın. Verilerinizi taşımak için Azure Veri Fabrikası'nda Veri Yönetimi Ağ Geçidi'ni kullanın.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 1340c205477b256e3d96ff7ccacb64e575725c2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065396"
---
# <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [kendi kendine barındırılan tümleştirme çalışma saatini 'de](../create-self-hosted-integration-runtime.md)görün.

> [!NOTE]
> Veri Yönetimi Ağ Geçidi artık Kendi kendine barındırılan Tümleştirme Runtime olarak yeniden markalandı.

Veri yönetimi ağ geçidi, bulut ve şirket içi veri depoları arasındaki verileri kopyalamak için şirket içi ortamınıza yüklemeniz gereken bir istemci aracısıdır. Veri Fabrikası tarafından desteklenen şirket içi veri depoları [Desteklenen veri kaynakları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) bölümünde listelenir.

Bu makale, şirket içi [ve bulut veri depoları](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşı veri si üzerindeki gözden geçirmeyi tamamlar. Geçiş durumunda, verileri şirket içi BIR SQL Server veritabanından Azure blob'una taşımak için ağ geçidini kullanan bir ardışık kaynak oluşturursunuz. Bu makalede, veri yönetimi ağ geçidi hakkında ayrıntılı ayrıntılı bilgi sağlar.

Birden çok şirket içi makineyi ağ geçidiyle ilişkilendirerek bir veri yönetimi ağ geçidini ölçeklendirebilirsiniz. Bir düğüm üzerinde aynı anda çalıştırılabilen veri hareketi işlerinin sayısını artırarak ölçeklendirebilirsiniz. Bu özellik, tek bir düğüme sahip mantıksal bir ağ geçidi için de kullanılabilir. Ayrıntılar için [Azure Veri Fabrikası makalesinde veri yönetimi ağ geçidini ölçekleme](data-factory-data-management-gateway-high-availability-scalability.md) makalesine bakın.

> [!NOTE]
> Şu anda ağ geçidi yalnızca Veri Fabrikası'ndaki kopyalama etkinliğini ve depolanan yordam etkinliğini destekler. Şirket içi veri kaynaklarına erişmek için özel bir etkinlikten ağ geçidini kullanmak mümkün değildir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
### <a name="capabilities-of-data-management-gateway"></a>Veri yönetimi ağ geçidinin yetenekleri
Veri yönetimi ağ geçidi aşağıdaki yetenekleri sağlar:

* Şirket içi veri kaynaklarını ve bulut veri kaynaklarını aynı veri fabrikasında modelleyip verileri taşıyın.
* Veri Fabrikası sayfasından ağ geçidi durumuna görünürlük le izleme ve yönetim için tek bir cam bölmeye sahip ol.
* Şirket içi veri kaynaklarına erişimi güvenli bir şekilde yönetin.
  * Kurumsal güvenlik duvarında değişiklik gerekmez. Ağ geçidi yalnızca interneti açmak için giden HTTP tabanlı bağlantılar yapar.
  * Şirket içi veri depolarınızın kimlik bilgilerini sertifikanızla şifreleyin.
* Verileri verimli bir şekilde taşıyın - veriler otomatik yeniden deneme mantığıyla aralıklı ağ sorunlarına paralel ve esnek olarak aktarılır.

### <a name="command-flow-and-data-flow"></a>Komut akışı ve veri akışı
Şirket içi ve bulut arasında veri kopyalamak için bir kopyalama etkinliği kullandığınızda, etkinlik şirket içi veri kaynağından buluta veri aktarmak için bir ağ geçidi kullanır ve bunun tersi de vardır.

Burada üst düzey veri akışı ve veri ağ geçidi ile ![kopyalama adımları özeti: Ağ geçidi kullanarak veri akışı](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Veri geliştiricisi, [Azure portalı](https://portal.azure.com) veya [PowerShell Cmdlet'i](https://docs.microsoft.com/powershell/module/az.datafactory/)kullanarak Bir Azure Veri Fabrikası için bir ağ geçidi oluşturur.
2. Veri geliştiricisi ağ geçidini belirterek şirket içi bir veri deposu için bağlantılı bir hizmet oluşturur. Bağlı hizmeti ayarlamanın bir parçası olarak, veri geliştiricikimlik doğrulama türlerini ve kimlik bilgilerini belirtmek için Kimlik Bilgilerini Ayarlama uygulamasını kullanır. Kimlik Bilgilerini Ayarlama iletişim kutusu, bağlantıyı sınamak için veri deposuyla ve kimlik bilgilerini kaydetmek için ağ geçidiyle iletişim kurar.
3. Ağ geçidi, kimlik bilgilerini bulutta kaydetmeden önce kimlik bilgilerini ağ geçidiyle ilişkili (veri geliştiricisi tarafından sağlanan) sertifikayla şifreler.
4. Veri Fabrikası hizmeti, ortak bir Azure servis veri hizmeti veri günü kuyruğu kullanan bir denetim kanalı aracılığıyla işlerin & yönetimini planlamak için ağ geçidiyle iletişim kurar. Bir kopyalama etkinliği işinin atılması gerektiğinde, Veri Fabrikası kimlik bilgileriyle birlikte isteği sıralar. Gateway, sırayı yokladıktan sonra işe başladı.
5. Ağ geçidi, kimlik bilgilerinin şifresini aynı sertifikayla çözer ve ardından uygun kimlik doğrulama türü ve kimlik bilgileriyle şirket içi veri deposuna bağlanır.
6. Ağ geçidi, şirket içi mağazadaki verileri bulut depolama alanına kopyalar veya Veri ardışık alanında Kopyalama Etkinliğinin nasıl yapılandırıldığına bağlı olarak tam tersi olur. Bu adım için ağ geçidi, güvenli (HTTPS) bir kanal üzerinden Azure Blob Depolama gibi bulut tabanlı depolama hizmetleriyle doğrudan iletişim kurar.

### <a name="considerations-for-using-gateway"></a>Ağ geçidi ni kullanmak için dikkat edilmesi gerekenler
* Veri yönetimi ağ geçidinin tek bir örneği, birden çok şirket içi veri kaynağı için kullanılabilir. Ancak, **tek bir ağ geçidi örneği yalnızca bir Azure veri fabrikasına bağlıdır** ve başka bir veri fabrikasıyla paylaşılamaz.
* Tek bir makinede **yalnızca bir veri yönetimi ağ geçidi örneği** yüklenebilir. Şirket içi veri kaynaklarına erişmeniz gereken iki veri fabrikanız olduğunu ve iki şirket içi bilgisayara ağ geçitleri yüklemeniz gerektiğini varsayalım. Başka bir deyişle, bir ağ geçidi belirli bir veri fabrikasına bağlıdır
* **Ağ geçidinin veri kaynağıyla aynı makinede olması gerekmez.** Ancak, ağ geçidinin veri kaynağına yakın olması, ağ geçidinin veri kaynağına bağlanma süresini kısaltıyor. Ağ geçidini, şirket içi veri kaynağını barındıran makineden farklı bir makineye yüklemenizi öneririz. Ağ geçidi ve veri kaynağı farklı makinelerde olduğunda, ağ geçidi veri kaynağı olan kaynaklar için rekabet etmez.
* **Aynı şirket içi veri kaynağına bağlanan farklı makinelerde birden çok ağ geçidi**olabilir. Örneğin, iki veri fabrikasına hizmet veren iki ağ geçidiniz olabilir, ancak aynı şirket içi veri kaynağı her iki veri fabrikasında da kayıtlıdır.
* Bilgisayarınızda **Power BI** senaryosu sunan bir ağ geçidi zaten yüklüyse, Azure Veri Fabrikası için ayrı bir **ağ geçidini** başka bir makineye yükleyin.
* **Ağ Geçidi, ExpressRoute'u**kullandığınızda bile kullanılmalıdır.
* **ExpressRoute'u**kullandığınızda bile veri kaynağınızı şirket içi veri kaynağı (güvenlik duvarının arkasında) olarak ele alın. Hizmet ve veri kaynağı arasında bağlantı kurmak için ağ geçidini kullanın.
* Veri deposu bir **Azure IaaS VM'de**bulutta olsa bile **ağ geçidini kullanmanız** gerekir.

## <a name="installation"></a>Yükleme
### <a name="prerequisites"></a>Ön koşullar
* Desteklenen **İşletim Sistemi** sürümleri Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2'dir. Etki alanı denetleyicisi üzerinde veri yönetimi ağ geçidi yükleme şu anda desteklenmez.
* .NET Framework 4.5.1 veya üzeri gereklidir. Bir Windows 7 makinesine ağ geçidi yüklüyorsanız,.NET Framework 4.5 veya daha sonrayı yükleyin. Ayrıntılar için [.NET Framework System Gereksinimlerine](https://msdn.microsoft.com/library/8z6watww.aspx) bakın.
* Ağ geçidi makinesi için önerilen **yapılandırma** en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disktir.
* Ana bilgisayar makinesi hazırda bekletme de, ağ geçidi veri isteklerine yanıt vermez. Bu nedenle, ağ geçidini yüklemeden önce bilgisayarda uygun bir **güç planı** yapılandırın. Makine hazırda bekleme değiştirmek için yürütülecek şekilde yıkılıyorsa, ağ geçidi yüklemesi bir ileti ister.
* Veri yönetimi ağ geçidini başarıyla yüklemek ve yapılandırmak için makinede yönetici olmalısınız. **Veri yönetimi ağ geçidine Kullanıcılar** yerel Windows grubuna ek kullanıcılar ekleyebilirsiniz. Bu grubun üyeleri ağ geçidini yapılandırmak için **Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi** aracını kullanabilir.

Kopyalama etkinliği belirli bir frekansta çalıştığından, makinedeki kaynak kullanımı (CPU, bellek) aynı deseni en yüksek ve boşta olan sürelerle birlikte izler. Kaynak kullanımı da büyük ölçüde taşınan veri miktarına bağlıdır. Birden çok kopyalama işi devam ederken, kaynak kullanımının yoğun zamanlarda artulaştığını görürsünüz.

### <a name="installation-options"></a>Yükleme seçenekleri
Veri yönetimi ağ geçidi aşağıdaki şekilde yüklenebilir:

* [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=39717)bir MSI kurulum paketi indirerek. MSI, tüm ayarları korunmuş olarak varolan veri yönetimi ağ geçidini en son sürüme yükseltmek için de kullanılabilir.
* MANUEL KURULUM altında veri ağ geçidi bağlantısını **indir'e** tıklayarak veya EXPRESS SETUP altında **doğrudan bu bilgisayara yükleyin.** Bkz. Ekspres kurulum kullanma yla ilgili adım adım talimatlar için şirket içi ve bulut makalesi [arasında veri taşı.](data-factory-move-data-between-onprem-and-cloud.md) Manuel adım sizi indirme merkezine götürür. Ağ geçidini indirme ve indirme merkezinden yükleme yönergeleri sonraki bölümde yer almaktadır.

### <a name="installation-best-practices"></a>Yükleme en iyi uygulamaları:
1. Ağ geçidi için ana makinedeki güç planını, makinenin hazırda beklemede olmaması için yapılandırın. Ana bilgisayar makinesi hazırda bekletme de, ağ geçidi veri isteklerine yanıt vermez.
2. Ağ geçidiyle ilişkili sertifikayı yedekle.

### <a name="install-the-gateway-from-download-center"></a>Ağ geçidini indirme merkezinden yükleme
1. Microsoft [Veri Yönetimi Ağ Geçidi indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=39717)gidin.
2. **İndir'i**tıklatın, **64 bit** sürümünü seçin (32 bit artık desteklenmez) ve **İleri'yi**tıklatın.
3. **MSI'ı** doğrudan çalıştırın veya sabit diskinize kaydedin ve çalıştırın.
4. Hoş **Geldiniz** sayfasında, Bir **dil** seçin **İleri'ye**tıklayın.
5. Son Kullanıcı Lisans Sözleşmesini **kabul edin** ve **İleri'yi**tıklatın.
6. Ağ geçidini yüklemek için **klasörü** seçin ve **İleri'yi**tıklatın.
7. **Yüklenmeye hazır** sayfasında **Yükle**'ye tıklayın.
8. Yüklemeyi tamamlamak için **Bitir'i** tıklatın.
9. Anahtarı Azure portalından alın. Adım adım talimatlar için sonraki bölüme bakın.
10. Makinenizde çalışan **Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nin** Kayıt ağ **geçidi** sayfasında aşağıdaki adımları yapın:
    1. Metni yapıştırın.
    2. İsteğe bağlı olarak, anahtar metnini görmek için **ağ geçidi tuşunu göster'i** tıklatın.
    3. **Kaydol'u**tıklatın.

### <a name="register-gateway-using-key"></a>Anahtarı kullanarak ağ geçidine kaydolun
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Portalda daha önce mantıklı bir ağ geçidi oluşturmadıysanız
Portalda bir ağ geçidi oluşturmak ve anahtarı **Yapılandırma** sayfasından almak için, şirket içi ve bulut makalesi [arasında veri taşı'ndaki](data-factory-move-data-between-onprem-and-cloud.md) iz başından gelen adımları izleyin.

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Portalda mantıksal ağ geçidini zaten oluşturduysanız
1. Azure **portalında, Veri Fabrikası** sayfasına gidin ve Bağlantılı **Hizmetler** döşemesini tıklatın.

    ![Veri Fabrikası sayfası](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Bağlantılı **Hizmetler** sayfasında, portalda oluşturduğunuz mantıksal **ağ geçidini** seçin.

    ![mantıksal ağ geçidi](media/data-factory-data-management-gateway/data-factory-select-gateway.png)
3. Veri **Ağ Geçidi** sayfasında, **veri ağ geçidini karşıdan yükleyin ve yükleyin'i**tıklatın.

    ![Portaldaki bağlantıyı indirin](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)
4. **Yapıl'ı yapıla** sayfasında, **yeniden oluşturma anahtarını**tıklatın. Dikkatle okuduktan sonra uyarı iletisinin üzerine Evet'i tıklatın.

    ![Anahtarı yeniden oluşturma](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Anahtarın yanındaki Kopyala düğmesini tıklatın. Anahtar panoya kopyalanır.

    ![Anahtarı kopyalama](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Sistem tepsisi simgeleri/ bildirimleri
Aşağıdaki resimde gördüğünüz tepsi simgelerinden bazıları gösterilmektedir.

![sistem tepsisi simgeleri](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

İmleci sistem tepsisi simgesi/bildirim iletisi üzerinde taşırsanız, açılır pencerede ağ geçidi/güncelleştirme işleminin durumu yla ilgili ayrıntıları görürsünüz.

### <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Göz önünde bulundurmanız gereken iki güvenlik duvarı vardır: kuruluşun merkezi yönlendiricisinde çalışan **kurumsal güvenlik duvarı** ve ağ geçidinin yüklü olduğu yerel makinede daemon olarak yapılandırılan Windows güvenlik **duvarı.**

![Güvenlik duvar -ları](./media/data-factory-data-management-gateway/firewalls2.png)

Kurumsal güvenlik duvarı düzeyinde, aşağıdaki etki alanlarını ve giden bağlantı noktalarını yapılandırmanız gerekir:

| Etki alanı adları | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| *.servicebus.windows.net |443 |Veri Hareketi Hizmeti arka uç ile iletişim için kullanılır |
| *.core.windows.net |443 |Azure Blob kullanarak Aşamalı kopya için kullanılır (yapılandırılırsa)|
| *.frontend.clouddatahub.net |443 |Veri Hareketi Hizmeti arka uç ile iletişim için kullanılır |
| *.servicebus.windows.net |9350-9354, 5671 |Kopyalama Sihirbazı tarafından kullanılan TCP üzerinden isteğe bağlı servis veri günü |

Windows güvenlik duvarı düzeyinde, bu giden bağlantı noktaları normalde etkinleştirilir. Değilse, etki alanlarını ve bağlantı noktalarını ağ geçidi makinesinde buna göre yapılandırabilirsiniz.

> [!NOTE]
> 1. Kaynağınıza/ lavabolarınıza bağlı olarak, şirket/Windows güvenlik duvarınızda ek etki alanlarını ve giden bağlantı noktalarını beyaz listeye almanız gerekebilir.
> 2. Bazı Bulut Veritabanları için (Örneğin: [Azure SQL Veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Veri Gölü](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access), vb.), güvenlik duvarı yapılandırmalarında Ağ Geçidi makinesinin IP adresini beyaz listeye almanız gerekebilir.
>
>

#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Verileri kaynak veri deposundan lavabo veri deposuna kopyalama
Güvenlik duvarı kurallarının şirket güvenlik duvarında, Windows güvenlik duvarı ağ geçidi makinesinde ve veri deposunda düzgün şekilde etkinleştirildiğinden emin olun. Bu kuralları etkinleştirmek ağ geçidinin hem kaynağa hem de batmaya başarılı bir şekilde bağlanmasını sağlar. Kopyalama işleminde yer alan her veri deposu için kuralları etkinleştirin.

Örneğin, **şirket içi veri deposundan Azure SQL Veritabanı lavabosuna veya Azure SQL Veri Ambarı lavabosuna**kopyalamak için aşağıdaki adımları yapın:

* Hem Windows güvenlik duvarı hem de şirket güvenlik duvarı için **1433** bağlantı noktasında giden **TCP** iletişimine izin verin.
* Azure SQL sunucusunun güvenlik duvarı ayarlarını, ağ geçidi makinesinin IP adresini izin verilen IP adresleri listesine eklemek için yapılandırın.

> [!NOTE]
> Güvenlik duvarınız giden bağlantı noktası 1433'e izin vermiyorsa, Ağ Geçidi Azure SQL'e doğrudan erişemez. Bu durumda, Staged [Copy](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) to SQL Azure Database/ SQL Azure DW'yi kullanabilirsiniz. Bu senaryoda, veri hareketi için yalnızca HTTPS (bağlantı noktası 443) gerekir.
>
>

### <a name="proxy-server-considerations"></a>Proxy sunucu hususlar
Kurumsal ağ ortamınız Internet'e erişmek için bir proxy sunucusu kullanıyorsa, uygun proxy ayarlarını kullanmak için veri yönetimi ağ geçidini yapılandırın. Proxy'yi ilk kayıt aşamasında ayarlayabilirsiniz.

![Kayıt sırasında proxy ayarlama](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Ağ Geçidi, bulut hizmetine bağlanmak için proxy sunucusunu kullanır. İlk kurulum sırasında **Değiştir** bağlantısını tıklatın. **Proxy ayarı** iletişim kutusunu görürsünüz.

![Config yöneticisi ni kullanarak proxy ayarlama](media/data-factory-data-management-gateway/SetProxySettings.png)

Üç yapılandırma seçeneği vardır:

* **Proxy kullanmayın**: Ağ geçidi, bulut hizmetlerine bağlanmak için açıkça herhangi bir proxy kullanmaz.
* **Sistem proxy'sini kullanın**: Ağ geçidi, diahost.exe.config ve diawp.exe.config'de yapılandırılan proxy ayarını kullanır. Hiçbir proxy diahost.exe.config ve diawp.exe.config yapılandırılırsa, ağ geçidi doğrudan proxy üzerinden gitmeden bulut hizmetine bağlanır.
* **Özel proxy kullanın**: Diahost.exe.config ve diawp.exe.config'de yapılandırmaları kullanmak yerine, AĞ Geçidi için kullanmak üzere HTTP proxy ayarını yapılandırın. Adres ve Bağlantı Noktası gereklidir. Kullanıcı Adı ve Parola, proxy'nizin kimlik doğrulama ayarına bağlı olarak isteğe bağlıdır. Tüm ayarlar ağ geçidinin kimlik bilgisi sertifikasıyla şifrelenir ve ağ geçidi ana bilgisayar makinesinde yerel olarak depolanır.

Güncelleştirilmiş proxy ayarlarını kaydettikten sonra veri yönetimi ağ geçidi Ana Bilgisayar Hizmeti otomatik olarak yeniden başlatılır.

Ağ geçidi başarıyla kaydedildikten sonra, proxy ayarlarını görüntülemek veya güncelleştirmek istiyorsanız, Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni kullanın.

1. **Başlat Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi**.
2. **Ayarlar** sekmesine geçin.
3. **Set HTTP** **Proxy** iletişim kutusunu başlatmak için HTTP Proxy **bölümündeki Değiştir** bağlantısını tıklatın.
4. **İleri** düğmesini tıklattıktan sonra, proxy ayarını kaydetmek ve Ağ Geçidi Ana Bilgisayar Hizmetini yeniden başlatmak için izninizi isteyen bir uyarı iletişim kutusu görürsünüz.

Configuration Manager aracını kullanarak HTTP proxy'yi görüntüleyebilir ve güncelleyebilirsiniz.

![Config yöneticisi ni kullanarak proxy ayarlama](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> NTLM kimlik doğrulaması olan bir proxy sunucusu ayarlarsanız, Ağ Geçidi Ana Bilgisayar Hizmeti etki alanı hesabı altında çalışır. Etki alanı hesabının parolasını daha sonra değiştirirseniz, hizmetin yapılandırma ayarlarını güncelleştirmeyi ve buna göre yeniden başlatmayı unutmayın. Bu gereksinim nedeniyle, parolayı sık sık güncelleştirmenizi gerektirmeyen proxy sunucusuna erişmek için özel bir etki alanı hesabı kullanmanızı öneririz.
>
>

### <a name="configure-proxy-server-settings"></a>Proxy sunucu ayarlarını yapılandırma
HTTP proxy için **sistem proxy** ayarı kullanır seçerseniz, ağ geçidi diahost.exe.config ve diawp.exe.config proxy ayarı kullanır. Diahost.exe.config ve diawp.exe.config'de proxy belirtilmemişse, ağ geçidi doğrudan proxy'ye geçmeden bulut hizmetine bağlanır. Aşağıdaki yordam, diahost.exe.config dosyasını güncelleştirmek için yönergeler sağlar.

1. Dosya Gezgini'nde, özgün dosyayı yedeklemek için *C:\\\\Program Files\\Microsoft Data Management Gateway\\2.0\\Shared\\diahost.exe.config'in* güvenli bir kopyasını yapın.
2. Başlat Notepad.exe yönetici olarak çalışan ve açık metin dosyası *\\\\C:\\Program\\\\Dosyaları Microsoft Veri Yönetimi Ağ Geçidi\\2.0 Paylaşılan diahost.exe.config*. Aşağıdaki kodda gösterildiği gibi system.net için varsayılan etiketi bulursunuz:

    ```
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```

    Daha sonra aşağıdaki örnekte gösterildiği gibi proxy sunucu ayrıntıları ekleyebilirsiniz:

    ```
    <system.net>
        <defaultProxy enabled="true">
            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Komut dosyasıKonum gibi gerekli ayarları belirtmek için proxy etiketinin içinde ek özelliklere izin verilir. Sözdiziminde [proxy Öğesi'ne (Ağ Ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) bakın.

    ```
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Yapılandırma dosyasını özgün konuma kaydedin ve ardından değişiklikleri alan Veri Yönetimi Ağ Geçidi Ana Bilgisayarı hizmetini yeniden başlatın. Hizmeti yeniden başlatmak için: hizmetleri kontrol panelinden veya **Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nden** > **Hizmeti Durdur** düğmesini tıklatarak **Başlat Hizmetini**tıklatın. Hizmet başlatılamazsa, düzenlenen uygulama yapılandırma dosyasına yanlış bir XML etiket sözdizimi eklenmiştir.

> [!IMPORTANT]
> **Hem** diahost.exe.config ve diawp.exe.config güncellemeyi unutmayın.

Bu noktalara ek olarak, Microsoft Azure'un şirketinizin beyaz listesinde olduğundan da emin olmanız gerekir. Geçerli Microsoft Azure IP adreslerinin listesi Microsoft [İndirme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=41653)indirilebilir.

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Güvenlik duvarı ve proxy sunucusuyla ilgili sorunlar için olası belirtiler
Aşağıdakilere benzer hatalarla karşılaşırsanız, büyük olasılıkla güvenlik duvarının veya proxy sunucusunun yanlış yapılandırmasından kaynaklanmaktadır ve bu da ağ geçidinin Veri Fabrikası'na bağlanmasını engelleyen ve kimliğini nitalsini doğrulamasını engeller. Güvenlik duvarınızın ve proxy sunucunuzun düzgün şekilde yapılandırıldığından emin olmak için önceki bölüme bakın.

1. Ağ geçidini kaydetmeye çalıştığınızda, aşağıdaki hatayı alırsınız: "Ağ geçidi anahtarını kaydettirmediğiniz için başarısız olun. Ağ geçidi anahtarını yeniden kaydetmeye çalışmadan önce, veri yönetimi ağ geçidinin bağlı bir durumda olduğunu ve Veri Yönetimi Ağ Geçidi Ana Bilgisayar Hizmetinin Başlatıldığından onaylayın."
2. Configuration Manager'ı açtığınızda, durumu "Bağlantısı kesildi" veya "Bağlanma" olarak görürsünüz. Windows olay günlüklerini görüntülerken, "Olay Görüntüleyicisi" > "Uygulama ve Hizmet Günlükleri" > "Veri Yönetimi Ağ Geçidi" altında aşağıdaki hata gibi hata iletileri görürsünüz:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Kimlik bilgisi şifrelemesi için 8050 bağlantı noktasını açın
**Kimlik Bilgileri Ayarlama** uygulaması, Azure portalında şirket içi bağlantılı bir hizmet ayarladığınızda kimlik bilgilerini ağ geçidine aktarmak için gelen bağlantı noktası **8050'yi** kullanır. Ağ geçidi kurulumu sırasında, varsayılan olarak, ağ geçidi yüklemeağı yüklemeağı makinesinde açar.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8050'yi el ile açabilirsiniz. Ağ geçidi kurulumu sırasında güvenlik duvarı sorunuyla karşınıza çıkarsa, güvenlik duvarını yapılandırmadan ağ geçidini yüklemek için aşağıdaki komutu kullanmayı deneyebilirsiniz.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Ağ geçidi makinesinde bağlantı noktası 8050'yi açmamayı seçerseniz, veri deposu kimlik bilgilerini yapılandırmak için **Kimlik Bilgilerini Ayarlama** uygulamasını kullanmak dışında mekanizmaları kullanın. Örneğin, [New-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) PowerShell cmdlet kullanabilirsiniz. Bkz. Veri deposu kimlik bilgilerinin nasıl ayarlanabildiği yle ilgili Kimlik Bilgileri ve Güvenlik bölümüne bakın.

## <a name="update"></a>Güncelleştirme
Varsayılan olarak, ağ geçidinin daha yeni bir sürümü kullanılabilir olduğunda veri yönetimi ağ geçidi otomatik olarak güncelleştirilir. Tüm zamanlanan görevler yapılana kadar ağ geçidi güncelleştirilmeyecek. Güncelleştirme işlemi tamamlanana kadar ağ geçidi tarafından başka görev işlenmez. Güncelleştirme başarısız olursa, ağ geçidi eski sürüme geri alınır.

Zamanlanan güncelleştirme saatini aşağıdaki yerlerde görürsünüz:

* Azure portalındaki ağ geçidi özellikleri sayfası.
* Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nin ana sayfası
* Sistem tepsisi bildirim iletisi.

Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nin Giriş sekmesi güncelleştirme zamanlamasını ve ağ geçidinin en son yüklendiğinde/güncelleştirileni görüntüler.

![Güncelleştirmeleri zamanlama](media/data-factory-data-management-gateway/UpdateSection.png)

Güncelleştirmeyi hemen yükleyebilir veya ağ geçidinin zamanlanan saatte otomatik olarak güncellenmesini bekleyebilirsiniz. Örneğin, aşağıdaki resimde, ağ geçidi yapılandırma yöneticisinde gösterilen bildirim iletisini ve hemen yüklemek için tıklatabileceğiniz Güncelleştirme düğmesini gösterir.

![DMG Configuration Manager'da güncelleştirme](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Sistem tepsisindeki bildirim iletisi aşağıdaki resimde gösterildiği gibi görünür:

![Sistem Tepsisi iletisi](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Sistem tepsisinde güncelleştirme işleminin (manuel veya otomatik) durumunu görürsünüz. Ağ Geçidi Yapılandırma Yöneticisi'ni bir daha ki sefere başlattığınızda, bildirim çubuğunda ağ geçidinin [yeni konuya](data-factory-gateway-release-notes.md)giden bir bağlantıyla birlikte güncelleştirildiğini belirten bir ileti görürsünüz.

### <a name="to-disableenable-auto-update-feature"></a>Otomatik güncelleştirme özelliğini devre dışı kılabilir/etkinleştirmek için
Aşağıdaki adımları yaparak otomatik güncelleştirme özelliğini devre dışı kılabilir/etkinleştirebilirsiniz:

[Tek düğüm ağ geçidi için]
1. Windows PowerShell'i ağ geçidi makinesinde başlatın.
2. *C:\\\\\\Program Dosyaları Microsoft Integration Runtime\\\\3.0\\ PowerShellScript* klasörüne geçin.
3. Otomatik güncelleştirme özelliğiNI KAPATMAK (devre dışı) açmak için aşağıdaki komutu çalıştırın.

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off
    ```
4. Tekrar açmak için:

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on
    ```
   [Çok düğümlü yüksek kullanılabilir ve ölçeklenebilir ağ geçidi için](data-factory-data-management-gateway-high-availability-scalability.md)
1. Windows PowerShell'i ağ geçidi makinesinde başlatın.
2. *C:\\\\\\Program Dosyaları Microsoft Integration Runtime\\\\3.0\\ PowerShellScript* klasörüne geçin.
3. Otomatik güncelleştirme özelliğiNI KAPATMAK (devre dışı) açmak için aşağıdaki komutu çalıştırın.

    Yüksek kullanılabilirlik özelliğine sahip ağ geçidi için ekstra bir AuthKey param gereklidir.
    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -off -AuthKey <your auth key>
    ```
4. Tekrar açmak için:

    ```powershell
    .\IntegrationRuntimeAutoUpdateToggle.ps1 -on -AuthKey <your auth key>
    ```

## <a name="configuration-manager"></a>Configuration Manager
Ağ geçidini yükledikten sonra, Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni aşağıdaki yollardan biriyle başlatabilirsiniz:

1. **Arama** penceresinde, bu yardımcı programa erişmek için **Veri Yönetimi Ağ Geçidi** yazın.
2. Yürütülebilir *ConfigManager.exe'yi* klasörde çalıştırın: *\\\\C: Program Dosyaları\\Microsoft Veri Yönetimi Ağ Geçidi\\2.0\\Paylaşılır.*

### <a name="home-page"></a>Giriş sayfası
Giriş sayfası aşağıdaki eylemleri yapmanızı sağlar:

* Ağ geçidinin durumunu görüntüleyin (bulut hizmetine bağlı vb.).
* Portaldan bir anahtar kullanarak **kaydolun.**
* Ağ geçidi makinesinde **Veri Yönetimi Ağ Geçidi Ana Bilgisayarı hizmetini** **durdurun** ve başlatın.
* Güncelleştirmeleri günlerin belirli bir saatinde **zamanlayın.**
* Ağ geçidinin en **son güncelleştirilen**tarihi görüntüleyin.

### <a name="settings-page"></a>Ayarlar sayfası
Ayarlar sayfası aşağıdaki işlemleri yapmanızı sağlar:

* Ağ geçidi tarafından kullanılan **sertifikayı** görüntüleme, değiştirme ve dışa aktarma sertifikası. Bu sertifika, veri kaynağı kimlik bilgilerini şifrelemek için kullanılır.
* Bitiş noktası için **HTTPS bağlantı noktasını** değiştirin. Ağ geçidi, veri kaynağı kimlik bilgilerini ayarlamak için bir bağlantı noktası açar.
* Bitiş noktasının **durumu**
* View **SSL sertifikası,** portal ile ağ geçidi arasındaki TLS/SSL iletişimi için veri kaynakları için kimlik bilgilerini ayarlamak için kullanılır.

### <a name="remote-access-from-intranet"></a>Intranetten uzaktan erişim
Bu işlevsellik gelecekte etkinleştirilecektir. Yaklaşan güncelleştirmelerde (v3.4 veya sonraki) kimlik bilgilerini şifrelemek için PowerShell veya Credentials Manager uygulamasını kullanırken, bugün bağlantı noktası 8050 (yukarıdaki bölüme bakın) kullanarak gerçekleşen herhangi bir uzaktan bağlantıyı etkinleştirmenize/devre dışı bırakmanıza izin veririz.

### <a name="diagnostics-page"></a>Tanılama sayfası
Tanılama sayfası aşağıdaki eylemleri yapmanızı sağlar:

* Ayrıntılı **günlüğe kaydetmeyi,** olay görüntüleyicisindeki günlükleri görüntüleyin ve bir hata varsa günlükleri Microsoft'a gönderin.
* Bir veri kaynağına **bağlantı test** edin.

### <a name="help-page"></a>Yardım sayfası
Yardım sayfası aşağıdaki bilgileri görüntüler:

* Ağ geçidinin kısa açıklaması
* Sürüm numarası
* Çevrimiçi yardım, gizlilik bildirimi ve lisans sözleşmesine bağlantılar.

## <a name="monitor-gateway-in-the-portal"></a>Portaldaki ağ geçidini izleme
Azure portalında, kaynak kullanımının (CPU, bellek, ağ(giriş/çıkış), vb. gerçek zamanlı anlık görüntüsünü bir ağ geçidi makinesinde görüntüleyebilirsiniz.

1. Azure portalında, veri fabrikanızın ana sayfasına gidin ve **Bağlantılı hizmetler** döşemesini tıklatın.

    ![Data factory giriş sayfası](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png)
2. **Bağlantılı hizmetler** sayfasındaki **ağ geçidini** seçin.

    ![Bağlantılı hizmetler sayfası](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. Ağ **Geçidi** sayfasında, ağ geçidinin bellek ve CPU kullanımını görebilirsiniz.

    ![Ağ geçidinin CPU ve bellek kullanımı](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png)
4. Ağ kullanımı gibi daha fazla ayrıntıyı görmek için **Gelişmiş ayarları** etkinleştirin.
    
    ![Ağ geçidinin gelişmiş izlenmesi](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

Aşağıdaki tablo, **Ağ Geçidi Düğümleri** listesindeki sütunların açıklamalarını sağlar:

İzleme Özelliği | Açıklama
:------------------ | :----------
Adı | Ağ geçidiyle ilişkili mantıksal ağ geçidinin ve düğümlerin adı. Düğüm, ağ geçidinin yüklü olduğu şirket içi bir Windows makinesidir. Tek bir mantıksal ağ geçidinde birden fazla düğüme (en fazla dört düğüm) sahip olmak hakkında bilgi için veri yönetimi ağ geçidine bakın [- yüksek kullanılabilirlik ve ölçeklenebilirlik.](data-factory-data-management-gateway-high-availability-scalability.md)
Durum | Mantıksal ağ geçidi ve ağ geçidi düğümlerinin durumu. Örnek: Çevrimiçi/Çevrimdışı/Sınırlı/vb. Bu durumlar hakkında bilgi için [Ağ Geçidi durum](#gateway-status) bölümüne bakın.
Sürüm | Mantıksal ağ geçidinin ve her ağ geçidi düğümünün sürümünü gösterir. Mantıksal ağ geçidinin sürümü, gruptaki düğümlerin çoğunluğunun sürümüne göre belirlenir. Mantıksal ağ geçidi kurulumunda farklı sürümlere sahip düğümler varsa, yalnızca mantıksal ağ geçidi işleviyle aynı sürüm numarasına sahip düğümler. Diğerleri sınırlı moddadır ve el ile güncelleştirilmelidir (yalnızca otomatik güncelleştirme başarısız olursa).
Kullanılabilir bellek | Ağ geçidi düğümünde kullanılabilir bellek. Bu değer neredeyse gerçek zamanlı anlık görüntüdür.
CPU kullanımı | Bir ağ geçidi düğümünün CPU kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür.
Ağ (Dahili/Çıkış) | Ağ geçidi düğümünün ağ kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür.
Eşzamanlı İşler (Çalışan / Limit) | Her düğümde çalışan iş veya görev sayısı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. Sınır, her düğüm için en büyük eşzamanlı işleri belirtir. Bu değer makine boyutuna göre tanımlanır. CPU/bellek/ağ yeterince kullanıldığı, ancak etkinliklerin zamanlandığı gelişmiş senaryolarda eşzamanlı iş yürütmeyi ölçeklendirme sınırını artırabilirsiniz. Bu özellik tek düğümağ geçidiyle de kullanılabilir (ölçeklenebilirlik ve kullanılabilirlik özelliği etkinleştirilmese bile).
Rol | Çok düğümlü ağ geçidinde iki tür rol vardır : Sevkçi ve çalışan. Tüm düğümler işçidir, bu da hepsinin işleri yürütmek için kullanılabildiği anlamına gelir. Bulut hizmetlerinden görevleri/işleri çekmek ve bunları farklı işçi düğümlerine (kendisi dahil) göndermek için kullanılan tek bir sevk düğümü vardır.

Bu sayfada, ağ geçidinde iki veya daha fazla düğüm olduğunda (senaryoyu ölçeklendirin) daha mantıklı bazı ayarlar görürsünüz. Bkz. Veri Yönetimi Ağ Geçidi - çok düğümlü ağ geçidi kurma yla ilgili ayrıntılar için [yüksek kullanılabilirlik ve ölçeklenebilirlik.](data-factory-data-management-gateway-high-availability-scalability.md)

### <a name="gateway-status"></a>Ağ geçidi durumu
Aşağıdaki tablo bir **ağ geçidi düğümü**olası durumları sağlar:

Durum  | Yorumlar / Senaryolar
:------- | :------------------
Çevrimiçi | Veri Fabrikası hizmetine bağlı düğüm.
Çevrimdışı | Düğüm çevrimdışı.
Yükseltme | Düğüm otomatik olarak güncelleniyor.
Sınırlı | Bağlantı sorunu nedeniyle. HTTP port 8050 sorunu, servis veri meskeni bağlantısı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir.
Etkin değil | Düğüm, diğer çoğunluk düğümlerinin yapılandırmasından farklı bir yapılandırmadadır.<br/><br/> Düğüm, diğer düğümlere bağlanamıyorsa etkin olmayabilir.

Aşağıdaki tablo, mantıksal bir **ağ geçidinin**olası durumlarını sağlar. Ağ geçidi durumu ağ geçidi düğümlerinin durumlarına bağlıdır.

Durum | Yorumlar
:----- | :-------
İhtiyaç Kaydı | Bu mantıksal ağ geçidine henüz düğüm kaydedilmemiştir
Çevrimiçi | Ağ Geçidi Düğümleri çevrimiçi
Çevrimdışı | Çevrimiçi durumda düğüm yok.
Sınırlı | Bu ağ geçidindeki tüm düğümler sağlıklı durumda değildir. Bu durum, bazı düğüm aşağı olabilir bir uyarıdır! <br/><br/>Sevk/işçi düğümündeki kimlik bilgisi eşitleme sorunundan kaynaklanıyor olabilir.

## <a name="scale-up-gateway"></a>Ağ geçidini ölçeklendir
Şirket içi ve bulut veri depoları arasında veri taşıma yeteneğini ölçeklendirmek için düğüm üzerinde çalıştırılabilen **eşzamanlı veri hareketi işlerinin** sayısını yapılandırabilirsiniz.

Kullanılabilir bellek ve CPU iyi kullanılmadığında, ancak boşta kapasite 0 olduğunda, düğüm üzerinde çalıştırılabilen eşzamanlı işlerin sayısını artırarak ölçeklendirmeniz gerekir. Ağ geçidi aşırı yüklendiğinden, etkinlikler zamanlandığında da ölçeklendirmek isteyebilirsiniz. Ağ geçidi düğümünün gelişmiş ayarlarında, düğüm için maksimum kapasiteyi artırabilirsiniz.

## <a name="troubleshooting-gateway-issues"></a>Sorun giderme ağ geçidi sorunları
Veri yönetimi ağ geçidini kullanmayla ilgili sorun giderme sorunları yla ilgili bilgi/ipucu için [Sorun Giderme ağ geçidi makalesine](data-factory-troubleshoot-gateway-issues.md) bakın.

## <a name="move-gateway-from-one-machine-to-another"></a>Ağ geçidini bir makineden diğerine taşıma
Bu bölümde, ağ geçidi istemcisini bir makineden başka bir makineye taşımak için adımlar sağlanmaktadır.

1. Portalda, **Veri Fabrikası ana sayfasına**gidin ve Bağlantılı **Hizmetler** döşemesini tıklatın.

    ![Veri Ağ Geçitleri Bağlantısı](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. **Bağlantılı Hizmetler** sayfasının **DATA GATEWAYS** bölümünde ağ geçidinizi seçin.

    ![Ağ geçidi seçili Bağlantılı Hizmetler sayfası](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. Veri **ağ geçidi** sayfasında, **veri ağ geçidini karşıdan yükleyin ve yükleyin'i**tıklatın.

    ![Ağ geçidi bağlantısını indirin](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. **Yapıla** sayfasında, **veri ağ geçidini karşıdan yükleyin ve veri ağ geçidini**yüklemek için yönergeleri izleyin'i tıklatın.

    ![Sayfayı yapılandırma](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Microsoft **Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni** açık tutun.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)
6. Portaldaki **Yapıla** sayfasında, komut çubuğundaki **Anahtarı Yeniden Oluştur'u** tıklatın ve uyarı iletisi için **Evet'i** tıklatın. Pano tuşunu kopyalayan anahtar metninin yanındaki **kopyala düğmesini** tıklatın. Anahtarı yeniden oluşturduğunuz anda eski makinedeki ağ geçidi çalışmayı durdurur.

    ![Anahtarı yeniden oluşturma](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Anahtarı, **key** makinenizdeki Veri Yönetimi Ağ Geçidi **Yapılandırma Yöneticisi'nin** **Kayıt Ağ Geçidi** sayfasındaki metin kutusuna yapıştırın. (isteğe bağlı) Anahtar metnini görmek için **ağ geçidi anahtar** onay kutusunu göster'i tıklatın.

    ![Kopya anahtarı ve Kayıt](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Ağ geçidini bulut hizmetine kaydettirmek için **Kaydol'u** tıklatın.
9. **Ayarlar** sekmesinde, eski ağ geçidiyle birlikte kullanılan sertifikayı seçmek için **Değiştir'i** tıklatın, **parolayı**girin ve **Bitir'i**tıklatın.

   ![Sertifika Belirt](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   Aşağıdaki adımları yaparak eski ağ geçidinden bir sertifika dışa aktarabilirsiniz: Eski makinede Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni başlatın, **Sertifika** sekmesine geçin, **Dışa** Aktar düğmesini tıklatın ve yönergeleri izleyin.
10. Ağ geçidinin başarılı bir şekilde kaydedilmesinden sonra, **Kayıt'ın** **Kayıtlı** olarak ayarlı olduğunu ve **Durum'un** Ağ Geçidi Yapılandırma Yöneticisi'nin Ana sayfasında Başlatılamını görmeniz gerekir. **Started**

## <a name="encrypting-credentials"></a>Kimlik bilgilerini şifreleme
Veri Fabrikası Düzenleyicisi'ndeki kimlik bilgilerini şifrelemek için aşağıdaki adımları yapın:

1. **Ağ geçidi makinesinde**web tarayıcısını başlatın, [Azure portalına](https://portal.azure.com)gidin. Gerekirse veri fabrikanızı arayın, **DATA FACTORY** sayfasında veri fabrikasını açın ve ardından Veri Fabrikası Düzenleyicisi'ni başlatmak için Yazar **& Dağıt'ı** tıklatın.
2. JSON tanımını görmek veya veri yönetimi ağ geçidi (örneğin: SQL Server veya Oracle) gerektiren bağlantılı bir hizmet oluşturmak için ağaç görünümünde varolan bir **bağlantılı hizmeti** tıklatın.
3. JSON düzenleyicisinde, **ağ geçidi Adı** özelliği için ağ geçidinin adını girin.
4. **ConnectionString'e** **Veri Kaynağı** özelliği için sunucu adı girin.
5. **ConnectionString'deki** **Başlangıç Kataloğu** özelliği için veritabanı adı girin.
6. Click-once **Kimlik Yöneticisi** uygulamasını başlatan komut çubuğundaki **Şifrele** düğmesini tıklatın. Kimlik Bilgilerini **Ayarlama** iletişim kutusunu görmeniz gerekir.

    ![Kimlik bilgileri iletişim kutusunu ayarlama](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. Kimlik **Bilgilerini Ayarlama** iletişim kutusunda aşağıdaki adımları yapın:
   1. Veritabanına bağlanmak için Veri Fabrikası hizmetinin kullanmasını istediğiniz **kimlik doğrulamasını** seçin.
   2. **USERNAME** ayarı için veritabanına erişimi olan kullanıcının adını girin.
   3. **PAROLA** ayarı için kullanıcıiçin parola girin.
   4. Kimlik bilgilerini şifrelemek ve iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.
8. Şimdi **connectionString'te** şifrelenmiş Bir **Credential** özelliği görmeniz gerekir.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
   Portala ağ geçidi makinesinden farklı bir makineden erişiyorsanız, Kimlik Bilgileri Yöneticisi uygulamasının ağ geçidi makinesine bağlanabilmesini sağlamanız gerekir. Uygulama ağ geçidi makinesine ulaşamıyorsa, veri kaynağı için kimlik bilgilerini ayarlamanıza ve veri kaynağına bağlantıyı sınamanıza izin vermez.

Kimlik Bilgilerini **Ayarlama** uygulamasını kullandığınızda, portal ağ geçidi makinesinde **Ağ Geçidi Yapılandırma Yöneticisi'nin** **Sertifika** sekmesinde belirtilen sertifikayla kimlik bilgilerini şifreler.

Kimlik bilgilerini şifrelemek için API tabanlı bir yaklaşım arıyorsanız, kimlik bilgilerini şifrelemek için [New-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) PowerShell cmdlet'i kullanabilirsiniz. Cmdlet, kimlik bilgilerini şifrelemek için ağ geçidinin kullanacağı sertifikayı kullanır. JSON'daki **connectionString'in** **EncryptedCredential** öğesine şifrelenmiş kimlik bilgileri eklersiniz. JSON'u [New-AzDataFactoryLinkedService](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet ile veya Veri Fabrikası Düzenleyicisi'nde kullanırsınız.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Veri Fabrikası Düzenleyicisi'ni kullanarak kimlik bilgilerini ayarlamak için bir yaklaşım daha vardır. Düzenleyiciyi kullanarak bir SQL Server bağlantılı hizmet oluşturursanız ve kimlik bilgilerini düz metin olarak girerseniz, kimlik bilgileri Veri Fabrikası hizmetinin sahip olduğu bir sertifika kullanılarak şifrelenir. Ağ geçidinin kullanmak üzere yapılandırıldığı sertifikayı KULLANMAZ. Bu yaklaşım bazı durumlarda biraz daha hızlı olsa da, daha az güvenlidir. Bu nedenle, bu yaklaşımı yalnızca geliştirme/test amacıyla izlemenizi öneririz.

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Bu bölümde, Azure PowerShell cmdlets kullanarak bir ağ geçidinin nasıl oluşturulup kaydedilen anlatılmaktadır.

1. **Azure PowerShell'i** yönetici modunda başlatın.
2. Aşağıdaki komutu çalıştırarak ve Azure kimlik bilgilerinizi girerek Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```
3. Aşağıdaki gibi mantıksal bir ağ geçidi oluşturmak için **New-AzDataFactoryGateway** cmdlet kullanın:

    ```powershell
    $MyDMG = New-AzDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Örnek komut ve çıktı**:

    ```
    PS C:\> $MyDMG = New-AzDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description "gateway for walkthrough"

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. Azure PowerShell'de klasöre geçin: *C:\\\\Program Dosyaları\\Microsoft Tümleştirme Çalışma Süresi\\3.0\\PowerShellScript\\*. Aşağıdaki komutta gösterildiği gibi yerel değişken **$Key** ilişkili *RegisterGateway.ps1* çalıştırın. Bu komut dosyası, makinenize yüklenen istemci aracısını daha önce oluşturduğunuz mantıksal ağ geçidiyle kaydeder.

    ```powershell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    IsRegisterOnRemoteMachine parametresini kullanarak ağ geçidini uzak bir makineye kaydedebilirsiniz. Örnek:

    ```powershell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Veri fabrikanızda Ağ Geçitleri listesini almak için **Get-AzDataFactoryGateway** cmdlet'i kullanabilirsiniz. **Durum** **çevrimiçi**olarak gösterdiğinde, ağ geçidiniz kullanıma hazır demektir.

    ```powershell        
    Get-AzDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
   Set-AzDataFactoryGateway cmdlets kullanarak **Remove-AzDataFactoryGateway** cmdlet **Set-AzDataFactoryGateway** ve bir ağ geçidi için güncelleştirme açıklamasını kullanarak bir ağ geçidi kaldırabilirsiniz. Sözdizimi ve bu cmdlets hakkındaki diğer ayrıntılar için Data Factory Cmdlet Reference'a bakın.  

### <a name="list-gateways-using-powershell"></a>PowerShell kullanarak ağ geçitlerini listele

```powershell
Get-AzDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>PowerShell kullanarak ağ geçidini kaldırma

```powershell
Remove-AzDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```

## <a name="next-steps"></a>Sonraki adımlar
* Bkz. [Verileri şirket içi ve bulut veri depoları arasında taşı](data-factory-move-data-between-onprem-and-cloud.md) makalesi. Geçiş durumunda, verileri şirket içi BIR SQL Server veritabanından Azure blob'una taşımak için ağ geçidini kullanan bir ardışık kaynak oluşturursunuz.
