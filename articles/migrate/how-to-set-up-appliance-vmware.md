---
title: VMware için Azure geçişi gereci ayarlama
description: VMware VM 'lerini değerlendirmek ve geçirmek için bir Azure geçiş gereci ayarlamayı öğrenin.
ms.topic: article
ms.date: 11/18/2019
ms.openlocfilehash: da451149d0420bc71a355fdf4f4fb122b196d5a0
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029052"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>VMware VM 'Leri için bir gereç ayarlama

Bu makalede, Azure geçişi sunucu taşıma aracı 'nı kullanarak VMware VM 'lerini Azure geçişi sunucu değerlendirmesi aracıyla değerlendiriyorsanız veya VMware VM 'lerini Azure 'a taşıma

VMware VM gereci, aşağıdakileri yapmak için Azure geçişi sunucu değerlendirmesi/geçişi tarafından kullanılan hafif bir gereç:

- Şirket içi VMware VM'lerini bulun.
- Keşfedilen VM 'Ler için meta verileri ve performans verilerini Azure geçişi sunucu değerlendirmesini/geçişini gönderin.

Azure geçişi gereci hakkında [daha fazla bilgi edinin](migrate-appliance.md) .


## <a name="appliance-deployment-steps"></a>Gereç dağıtım adımları

Gereci kurmak için şunları yapın:
- Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi ile kaydedin.

## <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Azure geçişi: Sunucu değerlendirmesi** > **sunucuları** > **geçiş hedeflerde** **bul**' a tıklayın.
2. **Makineleri bul** > **Makineleriniz sanallaştırıldı mı?** bölümünde **Evet, VMware vSphere hiper yöneticisi ile** seçeneğini belirleyin.
3. .OVA şablon dosyasını indirmek için **İndir**’e tıklayın.



### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce OVA dosyasının güvenli olduğundan emin olun.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA 'nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. En son gereç sürümü için, oluşturulan karma bu ayarlarla eşleşmelidir.

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | c06ac2a2c0f870d3b274a0b7a73b78b1
  SHA256 | 4ce4faa3a78189a09a26bfa5b817c7afcf5b555eb46999c2fad9d2ebc808540c


## <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.
2. OVF şablonu Dağıtma Sihirbazı > **kaynak**bölümünde ova dosyasının konumunu belirtin.
3. **Ad** ve **konum**bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
5. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
6. **Depolama**alanında VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç VM 'sinin [Azure URL 'lerine](migrate-appliance.md#url-access)bağlanabildiğinizden emin olun.


## <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
4. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Lisans**: lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
        - **Ara sunucu ayarları**' na tıklayın ve proxy adresini ve dinleme bağlantı noktasını, http://ProxyIPAddress veya http://ProxyFQDN biçiminde belirtin.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Saat doğrulandı. Bulmanın düzgün çalışması için gerecin Internet saatine eşit olması gerekir.
    - **Güncelleştirmeleri yükleme**: Azure geçişi, en son gereç güncelleştirmelerinin yüklü olduğunu denetler.
    - **VDDK 'Yi yükleme**: Azure geçişi, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olduğunu denetler.
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
4. **Kaydol**' a tıklayın.


## <a name="start-continuous-discovery-by-providing-vcenter-server-and-vm-credential"></a>vCenter Server ve VM kimlik bilgilerini sağlayarak sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **VCenter Server ayrıntılarını belirtin**bölümünde vCenter Server adı (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **parola**' da, gerecin vCenter sunucusundaki VM 'leri bulması için kullanacağı salt okuma hesabı kimlik bilgilerini belirtin. VCenter hesabına erişimi uygun şekilde sınırlayarak bulma kapsamını belirleyebilirsiniz; kapsam [bulma hakkında](tutorial-assess-vmware.md#set-the-scope-of-discovery)daha fazla bilgi edinin.
3. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula** ' ya tıklayın.

### <a name="specify-vm-credentials"></a>VM kimlik bilgilerini belirtin
Uygulama, rol ve özellik bulma ve VM 'lerin bağımlılıklarını görselleştirme için, VMware VM 'lerine erişimi olan bir VM kimlik bilgisi sağlayabilirsiniz. Windows VM 'ler için bir kimlik bilgisi ve Linux sanal makineleri için bir kimlik bilgisi ekleyebilirsiniz. Gerekli erişim ayrıcalıkları hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-vcenter-server-permissions) .

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
