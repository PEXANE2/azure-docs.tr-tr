---
title: Azure Veri Fabrikası'nda veri yönetimi ağ geçidi yle yüksek kullanılabilirlik
description: Bu makalede, daha fazla düğüm ekleyerek bir veri yönetimi ağ geçidini nasıl ölçeklendirebileceğinizve düğümüzerinde çalıştırılabilen eşzamanlı iş sayısını artırarak nasıl ölçeklendirebileceğiniz açıklanmaktadır.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 4ee89f4bba70bb5e81eef21247d556f65a2a1f16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065203"
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Veri Yönetimi Ağ Geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik (Önizleme)
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [kendi kendine barındırılan tümleştirme çalışma saatini 'de](../create-self-hosted-integration-runtime.md)görün. 


Bu makale, Veri Yönetimi Ağ Geçidi/ Tümleştirme ile yüksek kullanılabilirlik ve ölçeklenebilirlik çözümyapılandırmanıza yardımcı olur.    

> [!NOTE]
> Bu makalede, Tümleştirme Çalışma Süresi (Önceki Veri Yönetimi Ağ Geçidi) temellerini zaten bildiğinizi varsayar. Değilseniz, [Bkz. Veri Yönetimi Ağ Geçidi.](data-factory-data-management-gateway.md)
> 
> **Bu önizleme özelliği resmi olarak Veri Yönetimi Ağ Geçidi sürümü 2.12.xxxx.x ve üzeri desteklenir.** Lütfen sürüm 2.12.xxxx.x veya üzeri kullandığınızdan emin olun. Veri Yönetimi Ağ Geçidi'nin en son sürümünü [buradan](https://www.microsoft.com/download/details.aspx?id=39717)indirebilirsiniz.

## <a name="overview"></a>Genel Bakış
Birden çok şirket içi makinelere yüklenen veri yönetimi ağ geçitlerini portaldan tek bir mantıksal ağ geçidiyle ilişkilendirebilirsiniz. Bu makinelerdüğüm **denir.** Mantıksal bir ağ geçidiyle ilişkili en fazla **dört düğüm** ekleyebilirsiniz. Mantıksal bir ağ geçidi için birden çok düğüme (ağ geçidi yüklü şirket içi makineler) sahip olmanın yararları şunlardır:  

- Şirket içi ve bulut veri depoları arasındaki veri hareketinin performansını artırın.  
- Düğümlerden biri herhangi bir nedenle aşağı iniyorsa, diğer düğümler verileri taşımak için hala kullanılabilir. 
- Düğümlerden birinin bakım için çevrimdışı olarak alınması gerekiyorsa, diğer düğümler verileri taşımak için hala kullanılabilir.

Ayrıca, şirket içi ve bulut veri depoları arasında veri taşıma yeteneğini ölçeklendirmek için düğüm üzerinde çalıştırılabilen **eşzamanlı veri hareketi işlerinin** sayısını da yapılandırabilirsiniz. 

Azure portalını kullanarak, bu düğümlerin durumunu izleyebilirsiniz ve bu da bir düğümü mantıksal ağ geçidinden ekleyip eklememeye veya kaldırmamaya karar vermenize yardımcı olur. 

## <a name="architecture"></a>Mimari 
Aşağıdaki diyagram, Veri Yönetimi Ağ Geçidi'nin ölçeklenebilirlik ve kullanılabilirlik özelliğine mimari genel bakış sağlar: 

![Veri Yönetimi Ağ Geçidi - Yüksek Kullanılabilirlik ve Ölçeklenebilirlik](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

**Mantıksal ağ geçidi,** Azure portalındaki bir veri fabrikasına eklediğiniz ağ geçididir. Daha önce, yalnızca bir şirket içi Windows makinesini mantıksal bir ağ geçidiyle yüklü Veri Yönetimi Ağ Geçidi ile ilişkilendirebilirsiniz. Bu şirket içi ağ geçidi makinesine düğüm denir. Şimdi, en fazla **dört fiziksel düğümü** mantıksal bir ağ geçidiyle ilişkilendirebilirsiniz. Birden çok düğümiçeren mantıksal ağ **geçidine çok düğümlü ağ geçidi**denir.  

Tüm bu düğümler **etkindir.** Hepsi, verileri şirket içi ve bulut veri depoları arasında taşımak için veri hareketi işlerini işleyebilir. Düğümlerden biri hem sevk irsaliyesi hem de işçi olarak hareket eder. Gruplardaki diğer düğümler işçi düğümleridir. Bir **sevk düğümü** veri hareketi görevlerini/işleri bulut hizmetinden çeker ve bunları alt düğümlere (kendisi dahil) gönderir. **Bir işçi** düğümü, verileri şirket içi ve bulut veri depoları arasında taşımak için veri hareketi işlerini yürütür. Bütün düğümler işçidir. Yalnızca bir düğüm hem sevk hem de işçi olabilir.    

Genellikle bir düğümle başlayabilir ve varolan düğüm(ler) veri hareketi yüküyle boğulduğu için daha fazla düğüm eklemek için **ölçeklendirebilirsiniz.** Ayrıca, düğümüzerinde çalışmasına izin verilen eşzamanlı işlerin sayısını artırarak bir ağ geçidi düğümünün veri hareket yeteneğini de ölçeklendirebilirsiniz. **scale up** Bu özellik tek düğümağ geçidiyle de kullanılabilir (ölçeklenebilirlik ve kullanılabilirlik özelliği etkinleştirilmese bile). 

Birden çok düğümiçeren bir ağ geçidi, veri deposu kimlik bilgilerini tüm düğümler arasında eşittutar. Düğümden düğüme bağlantı sorunu varsa, kimlik bilgileri eşitlenmemiş olabilir. Ağ geçidi kullanan bir şirket içi veri deposu için kimlik bilgileri ayarladığınızda, kimlik bilgilerini sevk/işçi düğümüne kaydeder. Sevk düğümü diğer alt düğümlerle eşitler. Bu işlem **kimlik bilgileri eşitleme**olarak bilinir. Düğümler arasındaki iletişim kanalı ortak bir SSL/TLS sertifikası ile **şifrelenebilir.** 

## <a name="set-up-a-multi-node-gateway"></a>Çok düğümlü ağ geçidi ayarlama
Bu bölüm, aşağıdaki iki makaleden geçtiğinizveya bu makalelerdeki kavramlara aşina olduğunuzu varsayar: 

- [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) - ağ geçidine ayrıntılı bir genel bakış sağlar.
- [Şirket içi ve bulut veri depoları arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) - tek bir düğüm ile bir ağ geçidi kullanmak için adım adım yönergeleri içeren bir geçiş içerir.  

> [!NOTE]
> Şirket içi Windows makinesine veri yönetimi ağ geçidi yüklemeden önce ana makalede listelenen ön [koşullara](data-factory-data-management-gateway.md#prerequisites)bakın.

1. Mantıksal bir ağ geçidi [oluştururken, geçiş sırasında](data-factory-move-data-between-onprem-and-cloud.md#create-gateway)Yüksek **Kullanılabilirlik & Ölçeklenebilirlik** özelliğini etkinleştirin. 

    ![Veri Yönetimi Ağ Geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik sağlar](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. **Yapılandırma** sayfasında, ilk düğüme (şirket içi Windows makinesi) ağ geçidi yüklemek için **Açık Kurulum** veya El **Ile Kurulum** bağlantısını kullanın.

    ![Veri Yönetimi Ağ Geçidi - ekspres veya manuel kurulum](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Ekspres kurulum seçeneğini kullanırsanız, düğümden düğüme iletişim şifreleme olmadan yapılır. Düğüm adı makine adı ile aynıdır. Düğüm düğümü iletişiminin şifrelenmiş olması gerekiyorsa veya seçtiğiniz bir düğüm adı belirtmek istiyorsanız el ile kurulumkullanın. Düğüm adları daha sonra düzenlenemez.
3. **Ekspres kurulumu** seçerseniz
    1. Ağ geçidi başarıyla yüklendikten sonra aşağıdaki iletiyi görürsünüz:

        ![Veri Yönetimi Ağ Geçidi - hızlı kurulum başarısı](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. [Bu yönergeleri](data-factory-data-management-gateway.md#configuration-manager)izleyerek ağ geçidi için Veri Yönetimi Yapılandırma Yöneticisi başlatın. Ağ geçidi adını, düğüm adını, durumu vb. görürsünüz.

        ![Veri Yönetimi Ağ Geçidi - kurulum başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. **Manuel kurulum**seçerseniz:
    1. Yükleme paketini Microsoft Download Center'dan indirin, makinenize ağ geçidi yüklemek için çalıştırın.
    2. Ağ geçidini kaydetmek için **Yapılandırma** sayfasındaki **kimlik doğrulama anahtarını** kullanın.
    
        ![Veri Yönetimi Ağ Geçidi - kurulum başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. Yeni **ağ geçidi düğümü** sayfasında, ağ geçidi düğümüne özel bir **ad** sağlayabilirsiniz. Varsayılan olarak, düğüm adı makine adı ile aynıdır.    

        ![Veri Yönetimi Ağ Geçidi - adı belirtin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. Bir sonraki sayfada, **düğümden düğüme iletişim için şifrelemeyi etkinleştirip etkinleştirmeyeceğinizi**seçebilirsiniz. Şifrelemeyi devre dışı katlamak için **Atla'yı** tıklatın (varsayılan).

        ![Veri Yönetimi Ağ Geçidi - şifrelemeyi etkinleştirin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Şifreleme modunun değiştirilmesi yalnızca mantıksal ağ geçidinde tek bir ağ geçidi düğümü olduğunda desteklenir. Bir ağ geçidinde birden çok düğüm olduğunda şifreleme modunu değiştirmek için aşağıdaki adımları yapın: bir düğüm hariç tüm düğümleri silin, şifreleme modunu değiştirin ve düğümleri yeniden ekleyin.
        > 
        > TLS/SSL sertifikası kullanmak için gereksinimlerin listesi için [TLS/SSL sertifika gereksinimleri](#tlsssl-certificate-requirements) bölümüne bakın. 
    5. Ağ geçidi başarıyla yüklendikten sonra, Configuration Manager'ı Başlat'ı tıklatın:
    
        ![Manuel kurulum - başlatma yapılandırma yöneticisi](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. düğümde (şirket içi Windows makinesi) veri yönetimi ağ geçidi yapılandırma yöneticisini görürsünüz, bu da bağlantı durumunu, **ağ geçidi adını**ve düğüm **adını**gösterir.  

        ![Veri Yönetimi Ağ Geçidi - kurulum başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Bir Azure VM'de ağ geçidini uyguluyorsanız, [bu Azure Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-mutiple-vms-with-data-management-gateway)kullanabilirsiniz. Bu komut dosyası mantıksal bir ağ geçidi oluşturur, Veri Yönetimi Ağ Geçidi yazılımı yüklü VM'ler ayarlar ve bunları mantıksal ağ geçidiyle kaydeder. 
6. Azure portalında **Ağ Geçidi** sayfasını başlatın: 
    1. Portaldaki veri fabrikası ana sayfasında **Bağlantılı Hizmetler'i**tıklatın.
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. **Ağ Geçidi** sayfasını görmek için **ağ geçidini** seçin:
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. **Ağ Geçidi** sayfasını görüyorsunuz:   

        ![Tek düğüm görünümüolan ağ geçidi](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Mantıksal ağ geçidine düğüm eklemek için araç çubuğunda **Düğüm Ekle'yi** tıklatın. Ekspres kurulum kullanmayı planlıyorsanız, bu adımı ağ geçidine düğüm olarak eklenecek şirket içi makineden yapın. 

    ![Veri Yönetimi Ağ Geçidi - düğüm menüsü ekle](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Adımlar, ilk düğümü ayarlamaya benzer. Configuration Manager UI, manuel yükleme seçeneğini seçerseniz düğüm adını ayarlamanızı sağlar: 

    ![Configuration Manager - ikinci ağ geçidi ni yükleyin](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Ağ geçidi düğüme başarıyla yüklendikten sonra Configuration Manager aracı aşağıdaki ekranı görüntüler:  

    ![Configuration Manager - ikinci ağ geçidini başarılı bir şekilde yükleyin](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Portaldaki **Ağ Geçidi** sayfasını açarsanız, şimdi iki ağ geçidi düğümü görürsünüz: 

    ![Portalda iki düğüm bulunan ağ geçidi](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. Ağ geçidi düğümünü silmek için, araç çubuğunda **Node'u Sil'i** tıklatın, silmek istediğiniz düğümü seçin ve ardından araç çubuğundan **Sil'i** tıklatın. Bu eylem, seçili düğümü gruptan siler. Bu eylemin, düğümden (şirket içi Windows makinesi) veri yönetimi ağ geçidi yazılımını kaldırmadığını unutmayın. Ağ geçidini kaldırmak için şirket içi Denetim Masası'nda **Programlar Ekle'yi veya kaldır'ı** kullanın. Düğümden ağ geçidini kaldırdığınızda, ağ geçidi portalda otomatik olarak silinir.   

## <a name="upgrade-an-existing-gateway"></a>Varolan bir ağ geçidini yükseltme
Yüksek kullanılabilirlik ve ölçeklenebilirlik özelliğini kullanmak için varolan bir ağ geçidini yükseltebilirsiniz. Bu özellik yalnızca sürüm >= 2.12.xxxx veri yönetimi ağ geçidine sahip düğümlerle çalışır. Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nin **Yardım** sekmesinde bir makineye yüklenen veri yönetimi ağ geçidisürümünü görebilirsiniz. 

1. [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=39717)bir MSI kurulum paketi indirip çalıştırarak şirket içi makinedeki ağ geçidini en son sürüme güncelleştirin. Ayrıntılar için [yükleme](data-factory-data-management-gateway.md#installation) bölümüne bakın.  
2. Azure portalına gidin. Veri **fabrikanız** için Veri Fabrikası sayfasını başlatın. **Bağlantılı hizmetler sayfasını**başlatmak için Bağlantılı hizmetler döşemesini tıklatın. **Ağ geçidi sayfasını**başlatmak için ağ geçidini seçin. Aşağıdaki resimde gösterildiği gibi **Önizleme Özelliği'ni** tıklatın ve etkinleştirin: 

    ![Veri Yönetimi Ağ Geçidi - önizleme özelliğini etkinleştirin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Portalda önizleme özelliği etkinleştirildikten sonra tüm sayfaları kapatın. Yeni önizleme kullanıcı arabirimini (UI) görmek için **ağ geçidi sayfasını** yeniden açın.
 
    ![Veri Yönetimi Ağ Geçidi - önizleme özelliği başarısını etkinleştirin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Veri Yönetimi Ağ Geçidi - önizleme Web Bilgisi](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Yükseltme sırasında, ilk düğümün adı makinenin adıdır. 
3. Şimdi, bir düğüm ekle. Ağ **Geçidi** sayfasında **Düğüm Ekle'yi**tıklatın.  

    ![Veri Yönetimi Ağ Geçidi - düğüm menüsü ekle](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Düğümü ayarlamak için önceki bölümdeki yönergeleri izleyin. 

### <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler

- Ağ geçidi için ana makinedeki güç planını, makinenin hazırda beklemede olmaması için yapılandırın. Ana bilgisayar makinesi hazırda bekletme de, ağ geçidi veri isteklerine yanıt vermez.
- Ağ geçidiyle ilişkili sertifikayı yedekle.
- İdeal performans için tüm düğümlerin benzer konfigürasyona (önerilir) olduğundan emin olun. 
- Yüksek kullanılabilirlik sağlamak için en az iki düğüm ekleyin.  

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifika gereksinimleri
Tümleştirme çalışma zamanı düğümleri arasındaki iletişimi sağlamak için kullanılan TLS/SSL sertifikası için gereksinimler şunlardır:

- Sertifika, herkese açık bir X509 v3 sertifikası olmalıdır. Halka açık (üçüncü taraf) bir sertifika yetkilisi (CA) tarafından verilen sertifikaları kullanmanızı öneririz.
- Her tümleştirme çalışma zamanı düğümü, kimlik bilgisi yöneticisi uygulamasını çalıştıran istemci makinenin yanı sıra bu sertifikaya da güvenmelidir. 
  > [!NOTE]
  > Kimlik bilgisi yöneticisi uygulaması, Kopyalama Sihirbazı/ Azure Portalı'ndan kimlik bilgisi güvenli bir şekilde ayarlanırken kullanılır. Ve bu şirket içi / özel veri deposu ile aynı ağ içinde herhangi bir makineden ateşlenebilir.
- Joker kart sertifikaları desteklenir. FQDN adınız **node1.domain.contoso.com**ise ***.domain.contoso.com'ı** sertifikanın özadı olarak kullanabilirsiniz.
- San sertifikaları önerilmez, çünkü Konu Alternatif Adları'nın yalnızca son öğesi kullanılacak ve diğerleri geçerli sınırlama nedeniyle yoksayılacaktır. Örneğin SAN **node1.domain.contoso.com** ve **node2.domain.contoso.com**olan bir SAN sertifikanız var, bu sertifikayı sadece FQDN'si **node2.domain.contoso.com**olan makinede kullanabilirsiniz.
- TLS/SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen tüm anahtar boyutunu destekler.
- CNG anahtarlarını kullanan sertifika desteklenmez.

#### <a name="faq-when-would-i-not-enable-this-encryption"></a>SSS: Bu şifrelemeyi ne zaman etkinleştirmem?
Şifrelemeyi etkinleştirmek altyapınıza belirli bir maliyet ekleyebilir (ortak sertifika sahibi olmak) dolayısıyla aşağıdaki durumlarda şifrelemeyi etkinleştirmeyi atlayabilirsiniz:
- Tümleştirme çalışma süresi güvenilir bir ağda veya IP/SEC gibi saydam şifrelemeye sahip bir ağda çalışırken. Bu kanal iletişimi yalnızca güvenilen ağınızda sınırlı olduğundan, ek şifrelemeye gerek olmayabilir.
- Tümleştirme çalışma süresi üretim ortamında çalışmadığında. Bu, TLS/SSL sertifika maliyetini azaltmaya yardımcı olabilir.


## <a name="monitor-a-multi-node-gateway"></a>Çok düğümlü ağ geçidini izleme
### <a name="multi-node-gateway-monitoring"></a>Çok düğümlü ağ geçidi izleme
Azure portalında, ağ geçidi düğümlerinin durumlarıyla birlikte her düğümde kaynak kullanımının (CPU, bellek, ağ(giriş/çıkış) neredeyse gerçek zamanlı anlık görüntüsünü görüntüleyebilirsiniz. 

![Veri Yönetimi Ağ Geçidi - çoklu düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Ağ **Geçidi** sayfasında **Gelişmiş Ayarlar'ı** etkinleştirerek **Ağ**(giriş/çıkış), Rol & Kimlik Bilgisi Durumu , ağ geçidi sorunlarının hata ayıklanmasında yardımcı olan rol **ve**performans ayarı sırasında buna göre değiştirilebilen/değiştirilebilen **Eşzamanlı İşler** (Çalıştırma/ Sınır) gibi gelişmiş ölçümleri görebilirsiniz. Aşağıdaki tablo, **Ağ Geçidi Düğümleri** listesindeki sütunların açıklamalarını sağlar:  

İzleme Özelliği | Açıklama
:------------------ | :---------- 
Adı | Ağ geçidiyle ilişkili mantıksal ağ geçidinin ve düğümlerin adı.  
Durum | Mantıksal ağ geçidi ve ağ geçidi düğümlerinin durumu. Örnek: Çevrimiçi/Çevrimdışı/Sınırlı/vb. Bu durumlar hakkında bilgi için [Ağ Geçidi durum](#gateway-status) bölümüne bakın. 
Sürüm | Mantıksal ağ geçidinin ve her ağ geçidi düğümünün sürümünü gösterir. Mantıksal ağ geçidinin sürümü, gruptaki düğümlerin çoğunluğunun sürümüne göre belirlenir. Mantıksal ağ geçidi kurulumunda farklı sürümlere sahip düğümler varsa, yalnızca mantıksal ağ geçidi işleviyle aynı sürüm numarasına sahip düğümler. Diğerleri sınırlı moddadır ve el ile güncelleştirilmelidir (yalnızca otomatik güncelleştirme başarısız olursa). 
Kullanılabilir bellek | Ağ geçidi düğümünde kullanılabilir bellek. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. 
CPU kullanımı | Bir ağ geçidi düğümünün CPU kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. 
Ağ (Dahili/Çıkış) | Ağ geçidi düğümünün ağ kullanımı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. 
Eşzamanlı İşler (Çalışan / Limit) | Her düğümde çalışan iş veya görev sayısı. Bu değer neredeyse gerçek zamanlı anlık görüntüdür. Sınır, her düğüm için en büyük eşzamanlı işleri belirtir. Bu değer makine boyutuna göre tanımlanır. CPU/ bellek/ ağ az kullanıldığı, ancak etkinliklerin zamanlandığı gelişmiş senaryolarda eşzamanlı iş yürütmeyi ölçeklendirme sınırını artırabilirsiniz. Bu özellik tek düğümağ geçidiyle de kullanılabilir (ölçeklenebilirlik ve kullanılabilirlik özelliği etkinleştirilmese bile). Daha fazla bilgi için [ölçek hususları](#scale-considerations) bölümüne bakın. 
Rol | İki tür rol vardır – Sevkiyatçı ve çalışan. Tüm düğümler işçidir, bu da hepsinin işleri yürütmek için kullanılabildiği anlamına gelir. Bulut hizmetlerinden görevleri/işleri çekmek ve bunları farklı işçi düğümlerine (kendisi dahil) göndermek için kullanılan tek bir sevk düğümü vardır. 

![Veri Yönetimi Ağ Geçidi - gelişmiş çoklu düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

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

### <a name="pipeline-activities-monitoring"></a>Boru hattı/ faaliyetlerin izlenmesi
Azure portalı, parçalı düğüm düzeyi ayrıntılarıyla bir boru hattı izleme deneyimi sağlar. Örneğin, hangi etkinliklerin hangi düğüm üzerinde koştu gösterir. Bu bilgiler, ağ azaltma nedeniyle, belirli bir düğüm üzerinde performans sorunları anlamada yararlı olabilir. 

![Veri Yönetimi Ağ Geçidi - boru hatları için birden fazla düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Veri Yönetimi Ağ Geçidi - boru hattı ayrıntıları](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Ölçek hususları

### <a name="scale-out"></a>Ölçeği genişletme
Kullanılabilir **bellek düşük sayılsa** ve **CPU kullanımı yüksekse,** yeni bir düğüm eklemek, yükü makineler arasında ölçeklendirmeye yardımcı olur. Zaman ayarı veya ağ geçidi düğümü çevrimdışı olması nedeniyle etkinlikler başarısız oluyorsa, ağ geçidine bir düğüm eklerseniz yardımcı olur.
 
### <a name="scale-up"></a>Ölçeği artırma
Kullanılabilir bellek ve CPU iyi kullanılmadığında, ancak boşta kapasite 0 olduğunda, düğüm üzerinde çalıştırılabilen eşzamanlı işlerin sayısını artırarak ölçeklendirmeniz gerekir. Ağ geçidi aşırı yüklendiğinden, etkinlikler zamanlandığında da ölçeklendirmek isteyebilirsiniz. Aşağıdaki resimde gösterildiği gibi, bir düğüm için maksimum kapasiteyi artırabilirsiniz. Başlangıç olarak iki katına çıkarmanızı öneririz.  

![Veri Yönetimi Ağ Geçidi - ölçek hususlar](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Bilinen sorunlar/kesme değişiklikleri

- Şu anda, tek bir mantıksal ağ geçidi için en fazla dört fiziksel ağ geçidi düğümü olabilir. Performans nedenleriyle dörtten fazla düğüme ihtiyacınız varsa, [DMGHelp@microsoft.com](mailto:DMGHelp@microsoft.com)e-posta gönderin.
- Geçerli mantıksal ağ geçidinden geçiş yapmak için başka bir mantıksal ağ geçidinden kimlik doğrulama anahtarıyla bir ağ geçidi düğümü yeniden kaydedemezsiniz. Yeniden kaydolmak için, düğümden ağ geçidini kaldırın, ağ geçidini yeniden yükleyin ve diğer mantıksal ağ geçidiiçin kimlik doğrulama anahtarıyla kaydedin. 
- TÜM ağ geçidi düğümleriniz için HTTP proxy'si gerekiyorsa, proxy'yi diahost.exe.config ve diawp.exe.config olarak ayarlayın ve tüm düğümlerin aynı diahost.exe.config ve diawip.exe.config'e sahip olduğundan emin olmak için sunucu yöneticisini kullanın. Ayrıntılar için [proxy ayarlarını yapılandırın](data-factory-data-management-gateway.md#configure-proxy-server-settings) bölümüne bakın. 
- Ağ Geçidi Configuration Manager'da düğümden düğüme iletişim için şifreleme modunu değiştirmek için, biri hariç portaldaki tüm düğümleri silin. Ardından, şifreleme modunu değiştirdikten sonra düğümleri geri ekleyin.
- Düğümden düğüme iletişim kanalını şifrelemeyi seçerseniz resmi bir TLS sertifikası kullanın. Kendi imzalı sertifika, diğer makinelerde sertifika yetkilisi listesinde aynı sertifikaya güvenilmeyebileceğinden bağlantı sorunlarına neden olabilir. 
- Düğüm sürümü mantıksal ağ geçidi sürümünden daha düşük olduğunda, bir ağ geçidi düğümini mantıksal ağ geçidine kaydedemezsiniz. Daha düşük bir sürüm düğümünü (düşürmeyi) kaydedebilmeniz için mantıksal ağ geçidinin tüm düğümlerini portaldan silin. Mantıksal bir ağ geçidinin tüm düğümlerini silerseniz, bu mantıksal ağ geçidine el ile yeni düğümler yükleyin ve kaydedin. Bu durumda express kurulumu desteklenmez.
- Bulut kimlik bilgilerini kullanmaya devam eden varolan bir mantıksal ağ geçidine düğüm yüklemek için açık kurulum kullanamazsınız. Ayarlar sekmesindeki Ağ Geçidi Yapılandırma Yöneticisi'nden kimlik bilgilerinin nerede depolandığına bakabilirsiniz.
- Düğümden düğüme şifreleme etkin olan varolan bir mantıksal ağ geçidine düğüm yüklemek için açık kurulum kullanamazsınız. Şifreleme modunu ayarlamak el ile sertifika eklemeyi gerektirdiğinden, express install artık bir seçenek değildir. 
- Şirket içi ortamdan bir dosya kopyası için, localhost veya yerel sürücü tüm düğümler üzerinden erişilebilir olmayabilir beri artık localhost veya C:\files kullanmamalısınız. \\ Bunun yerine, dosyaların konumunu belirtmek için ServerName\files'ı kullanın. \\


## <a name="rolling-back-from-the-preview"></a>Önizlemeden geri alma 
Önizlemeden geri dönmek için, bir düğüm dışında tüm düğümleri silin. Hangi düğümleri sildiğin önemli değildir, ancak mantıksal ağ geçidinde en az bir düğüm olduğundan emin olun. Makinedeki ağ geçidini açarak veya Azure portalını kullanarak düğümü silebilirsiniz. Azure portalında, **Veri Fabrikası** sayfasında, Bağlantılı hizmetler sayfasını başlatmak için Bağlantılı **hizmetler** sayfasını tıklatın. **Ağ Geçidi** sayfasını başlatmak için ağ geçidini seçin. Ağ Geçidi sayfasında, ağ geçidiyle ilişkili düğümleri görebilirsiniz. Sayfa, ağ geçidinden bir düğümü silmenizi sağlar.
 
Silmeden sonra, aynı Azure portal sayfasındaki **önizleme özelliklerini** tıklatın ve önizleme özelliğini devre dışı bırakın. Bir düğüm GA (genel kullanılabilirlik) ağ geçidiiçin ağ geçidini sırdama.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri gözden geçirin:
- [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) - ağ geçidine ayrıntılı bir genel bakış sağlar.
- [Şirket içi ve bulut veri depoları arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) - tek bir düğüm ile bir ağ geçidi kullanmak için adım adım yönergeleri içeren bir geçiş içerir. 
