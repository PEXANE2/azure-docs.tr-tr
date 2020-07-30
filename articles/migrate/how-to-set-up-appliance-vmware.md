---
title: VMware için Azure geçişi gereci ayarlama
description: VMware VM 'lerini değerlendirmek ve geçirmek için bir Azure geçiş gereci ayarlamayı öğrenin.
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: 24ba978d776da375b417fb67823651727836cb22
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386750"
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
- Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi ile kaydedin.

## <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **keşfet**' e tıklayın.
2. Makinelerde **bulunan makinelerde**  >  **makineler sanallaştırılmış mı?**, **VMware vSphere hiper yöneticiyle Evet**' e tıklayın.
3. .OVA şablon dosyasını indirmek için **İndir**’e tıklayın.

  ![OVA dosyasını indirme seçimleri](./media/tutorial-assess-vmware/download-ova.png)

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


## <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın. Gereci bir OVA şablonu yerine bir komut dosyası kullanarak dağıtırsanız, yordamdaki ilk iki adım geçerli değildir.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
4. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Lisans**: lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
        - **Proxy ayarları**' na tıklayın ve proxy adresini ve dinleme bağlantı noktasını, veya biçiminde belirtin http://ProxyIPAddress http://ProxyFQDN .
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Saat doğrulandı. Bulmanın düzgün çalışması için gerecin Internet saatine eşit olması gerekir.
    - **Güncelleştirmeleri yükleme**: Azure geçişi, en son gereç güncelleştirmelerinin yüklü olduğunu denetler.
    - **VDDK 'Yi yükleme**: Azure geçişi VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olduğunu denetler.
        - Azure geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır.
        - VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini gereç üzerindeki belirtilen konuma ayıklayın.

## <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. **Oturum aç**' a tıklayın. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınızla ve parolanızla oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün.
2. Azure geçişi projesinin oluşturulduğu aboneliği seçin. Ardından projeyi seçin.
3. Gereç için bir ad belirtin. Ad 14 karakter veya daha az olmalıdır.
4. **Kaydet**’e tıklayın.


## <a name="start-continuous-discovery-by-providing-vcenter-server-and-vm-credential"></a>vCenter Server ve VM kimlik bilgilerini sağlayarak sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **VCenter Server ayrıntılarını belirtin**bölümünde vCenter Server adı (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **parola**' da, gerecin vCenter sunucusundaki VM 'leri bulması için kullanacağı salt okuma hesabı kimlik bilgilerini belirtin. VCenter hesabına erişimi sınırlayarak bulma kapsamını belirleyebilirsiniz. [Daha fazla bilgi edinin](set-discovery-scope.md).
3. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula** ' ya tıklayın.

### <a name="specify-vm-credentials"></a>VM kimlik bilgilerini belirtin
Uygulama, rol ve özellik bulma ve VM 'lerin bağımlılıklarını görselleştirme için, VMware VM 'lerine erişimi olan bir VM kimlik bilgisi sağlayabilirsiniz. Windows VM 'ler için bir kimlik bilgisi ve Linux sanal makineleri için bir kimlik bilgisi ekleyebilirsiniz. Gerekli erişim ayrıcalıkları hakkında [daha fazla bilgi edinin](./migrate-support-matrix-vmware.md) .

> [!NOTE]
> Bu girdi isteğe bağlıdır ve uygulama bulma ve aracısız bağımlılık görselleştirmesini etkinleştirmek için gereklidir.

1. **VM 'lerde uygulamaları ve bağımlılıkları bul**' da, **kimlik bilgileri ekle**' ye tıklayın.
2. **Işletim sistemini**seçin.
3. Kimlik bilgisi için kolay bir ad sağlayın.
4. **Kullanıcı adı** ve **parola**' da, VM 'lerde en az konuk erişimi olan bir hesap belirtin.
5. **Ekle**'ye tıklayın.

VCenter Server ve VM kimlik bilgilerini (isteğe bağlı) belirledikten sonra, şirket içi ortamı bulmaya başlamak için **Kaydet ve bulmayı Başlat** ' a tıklayın.

Keşfedilen VM 'lerin meta verilerinde portalda görünmesi 15 dakika sürer. Yüklenen uygulamaların, rollerin ve özelliklerin bulunması biraz zaman alabilir, süre bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık 1 saat sürer.

## <a name="next-steps"></a>Sonraki adımlar

[VMware değerlendirmesi](tutorial-assess-vmware.md) ve [aracısız geçiş](tutorial-migrate-vmware.md)öğreticilerini gözden geçirin.
