---
title: VMware için Azure geçişi gereci ayarlama
description: VMware VM 'lerini değerlendirmek ve geçirmek için bir Azure geçiş gereci ayarlamayı öğrenin.
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: c9a9f1567f984fc5770b47d3998610cb69643360
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88923643"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>VMware VM 'Leri için bir gereç ayarlama

Azure geçişi [: sunucu değerlendirme](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracı ve Azure geçişi [: sunucu geçiş](migrate-services-overview.md#azure-migrate-server-migration-tool) Aracı kullanılarak aracısız geçiş için Azure geçişi gereci ayarlamak için bu makaleyi izleyin.

[Azure geçişi](migrate-appliance.md) gereci, Azure geçişi tarafından kullanılan bir basit Gereç: Şirket Içi VMware VM 'lerini öğrenmek, Azure 'a VM meta verileri/performans verileri göndermek ve aracısız geçiş sırasında VMware VM 'lerinin çoğaltılması Için Sunucu değerlendirmesi ve sunucu geçişi.

Gereci birkaç yöntem kullanarak dağıtabilirsiniz:

- İndirilen bir OVA şablonu kullanarak bir VMware VM 'de ayarlayın. Bu, bu makalede açıklanan yöntemdir.
- Bir VMware VM 'de veya bir PowerShell yükleyici betiği ile fiziksel makinede ayarlama yapın. [Bu yöntem](deploy-appliance-script.md) , BIR ova şablonu kullanarak bir VM ayarlayamıyorum veya Azure Kamu kullanıyorsanız kullanılmalıdır.

Gereci oluşturduktan sonra Azure geçişi 'ne bağlanıp bağlanamadıktan sonra sunucu değerlendirmesi yapın, ilk kez yapılandırın ve Azure geçişi projesine kaydedin.


## <a name="appliance-deployment-ova"></a>Gereç dağıtımı (OVA)

Bir OVA şablonunu kullanarak gereci ayarlamak için:
- Portalda bir gereç adı sağlayın ve bir Azure geçişi proje anahtarı oluşturun
- Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi anahtarını kullanarak Azure geçişi projesi ile kaydedin.

### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makinelerde**  >  **makineler sanallaştırılmış mı?**, **VMware vSphere hiper yöneticiyle Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluştur**' da, Azure geçiş gereci VMware VM 'leri bulmak için ayarlayacaksınız. ad, 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-ova-template"></a>OVA şablonunu indirin
**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. OVA dosyası ve **İndir**' e tıklayın. 


   ![Bulma makineleri için seçimler](./media/tutorial-assess-vmware/servers-discover.png)


   ![Anahtar oluştur seçimleri](./media/tutorial-assess-vmware/generate-key-vmware.png)

### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce OVA dosyasının güvenli olduğundan emin olun.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA 'nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. En son gereç sürümü için, oluşturulan karma bu [ayarlarla](./tutorial-assess-vmware.md#verify-security)eşleşmelidir.



## <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. VSphere istemci konsolunda, **File**  >  **ovf şablonu dosya dağıt**' a tıklayın.
![OVF şablonu dağıtmak için menü komutu](./media/tutorial-assess-vmware/deploy-ovf.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**bölümünde ova dosyasının konumunu belirtin.
3. **Ad** ve **konum**bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
5. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
6. **Depolama**alanında VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


## <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.


### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci, indirilen OVA yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. VSphere Istemci konsolunda VM 'ye sağ tıklayın ve ardından **Konsolu Aç**' ı seçin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayolunu seçerek uygulamayı gereç masaüstünden açabilirsiniz.
1. **Lisans koşullarını**kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
   - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
     - Proxy adresini belirtmek için **proxy ayarla** ' ya tıklayın (formdaki http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında).
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy’si desteklenir.
     - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
   - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
   - **Güncelleştirmeleri yükleme**: gereç en son güncelleştirmelerin yüklü olmasını sağlar. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.
   - **VDDK 'Yi yükleme**: gereç, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olduğunu denetler. Yüklü değilse, VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini **yükleme yönergelerindeki**belirtilen konuma ayıklayın.

     Azure geçişi sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır. 
1. İsterseniz, gerecin tüm önkoşulları karşılayıp karşılamadığını denetlemek için gereç yapılandırması sırasında dilediğiniz zaman **Önkoşulları yeniden çalıştırabilirsiniz** .

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. **Oturum**aç ' a tıklayın. Yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açar. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün. 
4. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru [izinleri](tutorial-prepare-vmware.md#prepare-azure) varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

1. **1. Adım: vCenter Server kimlik**bilgilerini belirtin bölümünde kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, gerecin vCenter Server örneğindeki VM 'leri bulması için kullanacağı vCenter Server hesabı Için **Kullanıcı adı** ve **parola** ekleyin.
    - [Önceki öğreticide](tutorial-prepare-vmware.md#set-up-permissions-for-assessment)gerekli izinlere sahip bir hesap ayarlamış olmanız gerekir.
    - Bulma işlemini belirli VMware nesneleri (vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, bir konaklar klasörü veya ayrı VM 'Ler) olarak atamak istiyorsanız, Azure geçişi tarafından kullanılan hesabı kısıtlamak için [Bu makaledeki](set-discovery-scope.md) yönergeleri gözden geçirin.
1. **2. Adım: vCenter Server ayrıntıları belirtin**bölümünde, açılan listeden kimlik bilgileri için kolay ad seçmek üzere **bulma kaynağı Ekle** ' ye tıklayın, vCenter Server ÖRNEĞININ **IP adresini/FQDN** 'sini belirtin. **Bağlantı noktasını** varsayılan (443) olarak bırakabilir veya vCenter Server dinlediği ve **Kaydet**' e tıklayarak özel bir bağlantı noktası belirtebilirsiniz.
1. Kaydet 'e tıklanınca, Gereç vCenter Server bağlantısını doğrulamayı dener ve tablodaki **doğrulama durumunu** vCenter Server IP ADRESINE/FQDN 'ye göre gösterir.
1. Bulmayı başlatmadan önce vCenter Server bağlantıyı **yeniden doğrulayabilirsiniz** .
1. **3. Adım: yüklü uygulamaları bulmaya ve aracısız bağımlılık eşlemesi gerçekleştirmeye YÖNELIK VM kimlik bilgilerini sağlayın**, kimlik bilgileri **Ekle**' ye tıklayın ve kimlik bilgilerinin sağlandığı işletim sistemini, kimlik bilgileri Için kolay adı ve **Kullanıcı** adını ve **parolayı**belirtin. Ardından **Kaydet**' e tıklayın.

    - [Uygulama bulma özelliği](how-to-discover-applications.md)veya [aracısız bağımlılık Analizi özelliği](how-to-create-group-machine-dependencies-agentless.md)için kullanmak üzere bir hesap oluşturduysanız, isteğe bağlı olarak burada kimlik bilgileri eklersiniz.
    - Bu özellikleri kullanmak istemiyorsanız, adımı atlamak için kaydırıcıya tıklayabilirsiniz. Amacınızı daha sonra dilediğiniz zaman tersine çevirebilirsiniz.
    - [Uygulama bulma](migrate-support-matrix-vmware.md#application-discovery-requirements)veya [aracısız bağımlılık Analizi](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)için gereken kimlik bilgilerini gözden geçirin.

5. VM bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki vCenter Server IP adresine/FQDN 'ye karşı bulma durumunu kontrol edebilirsiniz.

Bulma işlemi aşağıdaki gibi çalışmaktadır:
- Keşfedilen VM meta verilerinin portalda görünmesi 15 dakika içinde sürer.
- Yüklenen uygulamaların, rollerin ve özelliklerin bulunması biraz zaman alır. Süre, bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık bir saat sürer.

## <a name="next-steps"></a>Sonraki adımlar

[VMware değerlendirmesi](tutorial-assess-vmware.md) ve [aracısız geçiş](tutorial-migrate-vmware.md)öğreticilerini gözden geçirin.
