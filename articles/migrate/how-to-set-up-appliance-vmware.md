---
title: VMware için bir Azure Geçiş cihazı ayarlama
description: VMware VM'leri değerlendirmek ve geçirmek için bir Azure Geçir cihazını nasıl ayarlayamanızı öğrenin.
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: b32c6a9b703e4d341fe353d6b472ea7a18adadf3
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538265"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>VMware VM'ler için bir cihaz ayarlama

[Azure Geçir:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla değerlendirilmek üzere Azure Geçir cihazını ve Azure [Geçir:Sunucu Geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) aracını kullanarak aracısız geçiş için bu makaleyi izleyin.

[Azure Geçir cihazı,](migrate-appliance.md) şirket içi VMware VM'leri keşfetmek, Azure'a VM meta veri/performans verileri göndermek ve aracısız geçiş sırasında VMware VM'lerinin çoğaltılması için Azure Geçiş:Sunucu Değerlendirmesi ve Sunucu Geçişi tarafından kullanılan hafif bir cihazdır.

Cihazı birkaç yöntem kullanarak dağıtabilirsiniz:

- İndirilen BIR OVA şablonu kullanarak VMware VM'de ayarlayın. Bu, bu makalede açıklanan yöntemdir.
- PowerShell yükleyici komut dosyasına sahip bir VMware VM veya fiziksel makineüzerinde ayarlayın. [Bu yöntem,](deploy-appliance-script.md) BIR OVA şablonu kullanarak bir VM ayarlayamıyorsanız veya Azure'da ysanız kullanılmalıdır.

Cihazı oluşturduktan sonra, Azure Geçiş:Sunucu Değerlendirmesi'ne bağlanıp bağlanabildiğinizi, ilk kez yapılandırıp yapılandırıp kaydedilemediğini kontrol edin ve Azure Geçiş projesine kaydettirebilirsiniz.


## <a name="appliance-deployment-ova"></a>Cihaz dağıtımı (OVA)

Cihazı bir OVA şablonu kullanarak kurmak için:
- BIR OVA şablon dosyası indirin ve vCenter Server'a aktarın.
- Cihazı oluşturun ve Azure Geçiş Sunucusu Değerlendirmesi'ne bağlanıp bağlanabıp bağlanabıp bağlanamayalı kontrol edin.
- Cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydedin.

## <a name="download-the-ova-template"></a>OVA şablonuna karşı yükleme

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Yes, with VMWare vSphere hypervisor****makineleriniz sanallaştırılmış mı?**
3. .OVA şablon dosyasını indirmek için **İndir**’e tıklayın.

  ![OVA dosyasını indirmek için seçmeler](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce OVA dosyasının güvenli olup olmadığını denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA için karma oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. En son cihaz sürümü için, oluşturulan karma bu [ayarları](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware#verify-security)eşleştirmelidir.



## <a name="create-the-appliance-vm"></a>VM cihazını oluşturun

İndirilen dosyayı içe aktarın ve bir VM oluşturun.

1. vSphere Client konsolunda, **OvF** > Şablonu Dosya**dağıt'ı**tıklatın.
![OVF şablonunu dağıtmak için menü komutu](./media/tutorial-assess-vmware/deploy-ovf.png)

2. OvF Şablon Sihirbazı **Source**> Kaynak'ta, OVA dosyasının konumunu belirtin.
3. **Ad** ve **Konum**olarak, VM için uygun bir ad belirtin. VM'nin barındırılan stok nesnesini seçin.
5. **Ana Bilgisayar/Küme'de,** VM'nin çalışacağı ana bilgisayarı veya kümeyi belirtin.
6. **Depolama'da,** VM'nin depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. **Ağ Eşleme'de,** VM'nin bağlanacağı ağı belirtin. Ağ, Azure Geçir Sunucu Değerlendirmesi'ne meta veri göndermek için internet bağlantısına ihtiyaç duyar.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


## <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

VM cihazının [genel](migrate-appliance.md#public-cloud-urls) ve [resmi](migrate-appliance.md#government-cloud-urls) bulutlar için Azure URL'lerine bağlanabileceğinden emin olun.


## <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın. Cihazı OVA şablonu yerine komut dosyası kullanarak dağıtirsanız, yordamDaki ilk iki adım geçerli değildir.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Cihazın dilini, saatini ve parolasını sağlayın.
3. VM'ye bağlanabilen herhangi bir makinede tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi*: 44368**.

   Alternatif olarak, uygulama kısayolu tıklayarak cihaz masaüstünden uygulamayı açabilirsiniz.
4. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM'nin internet erişimi ne olduğunu denetler. VM proxy kullanıyorsa:
        - **Proxy ayarlarını**tıklatın ve proxy adresini ve dinleme http://ProxyIPAddress http://ProxyFQDNbağlantı noktasını formda veya .
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. Cihazın üzerindeki süre, keşfin düzgün çalışması için internet süresiyle uyumlu olmalıdır.
    - **Yükleme güncelleştirmeleri**: Azure Geçir, en son cihaz güncelleştirmelerinin yüklü olup olmadığını denetler.
    - **Install VDDK**: Azure Geçir, VMWare vSphere Sanal Disk Geliştirme Kiti'nin (VDDK) yüklü olup olmadığını denetler.
        - Azure Geçirim, Azure'a geçiş sırasında makineleri çoğaltmak için VDDK'yı kullanır.
        - VMware'den VDDK 6.7'yi indirin ve indirilen zip içeriğini cihazda belirtilen konuma çıkarın.

## <a name="register-the-appliance-with-azure-migrate"></a>Cihazı Azure Geçiş ile kaydedin

1. **Giriş Yap'ı**tıklatın. Görünmüyorsa, tarayıcıdaki açılır pencere engelleyicisini devre dışı bıraktığınızdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınız ve şifrenizle oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarılı bir şekilde oturum imzaladıktan sonra web uygulamasına geri dön.
2. Azure Geçiş projesinin oluşturulduğu aboneliği seçin. Sonra projeyi seçin.
3. Cihaz için bir ad belirtin. Ad 14 karakter veya daha az alfanümerik olmalıdır.
4. **Kaydol'u**tıklatın.


## <a name="start-continuous-discovery-by-providing-vcenter-server-and-vm-credential"></a>vCenter Server ve VM kimlik bilgilerini sağlayarak sürekli keşfetmeye başlayın

Cihazın VM'lerin yapılandırma ve performans verilerini keşfetmek için vCenter Server'a bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **vCenter Server ayrıntılarını belirtin,** vCenter Server'ın adını (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server'ınızın dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **Parola'da,** cihazın vCenter sunucusunda VM'leri bulmak için kullanacağı salt okunur hesap kimlik bilgilerini belirtin. vCenter hesabına erişimi sınırlayarak keşfi kapsamınıza göre yapabilirsiniz. [Daha fazla bilgi edinin](set-discovery-scope.md).
3. Cihazın vCenter Server'a bağlanabilmesini sağlamak için **bağlantıyı doğrula'yı** tıklatın.

### <a name="specify-vm-credentials"></a>VM kimlik bilgilerini belirtin
Uygulamaların, rollerin ve özelliklerin keşfi ve VM'lerin bağımlılıklarını görselleştirmek için VMware VM'lere erişimi olan bir VM kimlik bilgisi sağlayabilirsiniz. Windows VM'ler için bir kimlik bilgisi ve Linux VM'leri için bir kimlik bilgisi ekleyebilirsiniz. Gereken erişim ayrıcalıkları hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)

> [!NOTE]
> Bu giriş isteğe bağlıdır ve uygulama bulma ve aracısız bağımlılık görselleştirme etkinleştirmek için gereklidir.

1. **VM'lere yönelik uygulamaları ve bağımlılıkları keşfedin'** de **kimlik bilgileri ekle'yi**tıklatın.
2. **İşletim Sistemini**seçin.
3. Kimlik bilgisi için dostça bir ad sağlayın.
4. **Kullanıcı Adı** ve **Parola'da,** VM'lerde en az konuk erişimi olan bir hesap belirtin.
5. **Ekle**'ye tıklayın.

vCenter Server ve VM kimlik bilgilerini (isteğe bağlı) belirttikten sonra, **Kaydet'i** tıklatın ve şirket içi ortamın keşfini başlatmak için keşfetmeye başlayın.

Keşfedilen VM'lerin meta verilerinin portalda görünmesi yaklaşık 15 dakika sürer. Yüklü uygulamaların, rollerin ve özelliklerin keşfi biraz zaman alır, süre keşfedilen VM sayısına bağlıdır. 500 VM için uygulama envanterinin Azure Geçiş portalında görünmesi yaklaşık 1 saat sürer.

## <a name="next-steps"></a>Sonraki adımlar

[VMware değerlendirmesi](tutorial-assess-vmware.md) ve [aracısız geçiş](tutorial-migrate-vmware.md)için öğreticiler gözden geçirin.
