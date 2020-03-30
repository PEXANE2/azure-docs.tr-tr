---
title: Hyper-V için bir Azure Geçiş cihazı ayarlama
description: Hyper-V VM'leri değerlendirmek ve geçirmek için bir Azure Geçir cihazını nasıl ayarlayamanızı öğrenin.
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: 80db2c1d4f5482604ca1507174b127c150f76044
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336806"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Hyper-V VM'ler için bir cihaz ayarlama

Bu makalede, Azure [Geçir:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla Hyper-V VM'lerin değerlendirilmesi için Azure Geçir cihazının nasıl ayarlanır.

[Azure Geçir cihazı,](migrate-appliance.md) şirket içi Hyper-V V MM'leri keşfetmek ve VM meta veri/performans verilerini Azure'a göndermek için Azure Geçiş:Sunucu Değerlendirmesi/Geçiş tarafından kullanılan hafif bir cihazdır.

İndirdiğiniz Bir VHD şablonunu kullanarak veya PowerShell yükleme komut dosyası kullanarak Hyper-V VM değerlendirmesi için Azure Geçir cihazını ayarlayabilirsiniz. Bu makalede, VHD şablonu kullanılarak cihazın nasıl ayarlanır. Komut dosyasını kullanarak cihazı kurmak istiyorsanız, bu [makaledeki](deploy-appliance-script.md)yönergeleri izleyin.


## <a name="appliance-deployment-vhd"></a>Cihaz dağıtımı (VHD)

Cihazı VHD şablonu kullanarak kurmak için:

- Azure portalından sıkıştırılmış hyper-v vhd indirin.
- Cihazı oluşturun ve Azure Geçiş Sunucusu Değerlendirmesi'ne bağlanıp bağlanabıp bağlanabıp bağlanamayalı kontrol edin.
- Cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydedin.

## <a name="download-the-vhd"></a>VHD'yi indirin

Cihaz için sıkıştırılmış VHD şablonuna indirin.

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Yes, with Hyper-V****makineleriniz sanallaştırıldı mı?**
3. VHD dosyasını indirmek için **İndir'i** tıklatın.

    ![VM'yi İndir](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. VHD için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.vhd SHA256```
3.  Cihaz sürümü 2.19.11.12 için oluşturulan karma bu [ayarlarla](https://docs.microsoft.com/azure/migrate/tutorial-assess-hyper-v#verify-security)eşleşmelidir.




## <a name="create-the-appliance-vm"></a>VM cihazını oluşturun

İndirilen dosyayı içe aktarın ve VM'yi oluşturun.

1. Sıkıştırılmış VHD dosyasını Hyper-V ana bilgisayarda, vm cihazını barındıracak bir klasöre ayıklayın. Üç klasör ayıklanır.
2. Hyper-V Yöneticisi'ni açın. **Eylemlerde,** **Sanal MakineYi İçe Aktar'ı**tıklatın.

    ![VHD'yi dağıt](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

2. Sanal Makine > Alma Sihirbazı'nda **başlamadan önce** **İleri'yi**tıklatın.
3. **Klasörü**Bul'da, çıkarılan VHD'yi içeren klasörü belirtin. Ardından **İleri**’ye tıklayın.
1. **Sanal Makine Seç'te** **İleri'yi**tıklatın.
2. **Alma Türü Seç'te,** sanal **makineyi kopyala'yı tıklatın (yeni bir benzersiz kimlik oluşturun)**. Ardından **İleri**’ye tıklayın.
3. **Hedef Seç'te**varsayılan ayarbırakın. **İleri**'ye tıklayın.
4. **Depolama Klasörleri'nde**varsayılan ayarı bırakın. **İleri**'ye tıklayın.
5. **Ağ**Seç'te, VM'nin kullanacağı sanal anahtarı belirtin. Anahtar, Azure'a veri göndermek için internet bağlantısına ihtiyaç duyar.
6. **Özetle,** ayarları gözden geçirin. Ardından, **Son**'a tıklayın.
7. Sanal **Makineler>** Hyper-V Manager'da VM'yi başlatın.


### <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

VM cihazının [Azure URL'lerine](migrate-appliance.md#url-access)bağlanabileceğinden emin olun.

## <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın. Cihazı VHD yerine komut dosyası kullanarak dağıtirseniz, yordamın ilk iki adımı geçerli değildir.

1. Sanal **Makineler>** Hyper-V Manager'da VM > **Connect'e**sağ tıklayın.
2. Cihazın dilini, saatini ve parolasını sağlayın.
3. VM'ye bağlanabilen herhangi bir makinede tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi*: 44368**.

   Alternatif olarak, uygulama kısayolu tıklayarak cihaz masaüstünden uygulamayı açabilirsiniz.
1. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM'nin internet erişimi ne olduğunu denetler. VM proxy kullanıyorsa:
        - **Proxy ayarlarını**tıklatın ve proxy adresini ve dinleme http://ProxyIPAddress http://ProxyFQDNbağlantı noktasını formda veya .
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. VM keşfinin düzgün çalışması için cihazdaki süre internet süresiyle uyumlu olmalıdır.
    - **Yükleme güncelleştirmeleri**: Azure Geçir Sunucusu Değerlendirmesi, cihazın en son güncelleştirmeleri yüklü olduğunu denetler.

### <a name="register-the-appliance-with-azure-migrate"></a>Cihazı Azure Geçiş ile kaydedin

1. **Giriş Yap'ı**tıklatın. Görünmüyorsa, tarayıcıdaki açılır pencere engelleyicisini devre dışı bıraktığınızdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınız ve şifrenizle oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarılı bir şekilde oturum imzaladıktan sonra web uygulamasına geri dön.
4. Azure Geçiş projesinin oluşturulduğu aboneliği seçin. Sonra projeyi seçin.
5. Cihaz için bir ad belirtin. Ad 14 karakter veya daha az alfanümerik olmalıdır.
6. **Kaydol'u**tıklatın.


### <a name="delegate-credentials-for-smb-vhds"></a>Kobİ VHD'ler için temsilci kimlik bilgileri

SMB'lerde VHD çalıştırıyorsanız, cihazdan Hyper-V ana bilgisayarlarına kimlik bilgilerini delegasyonuna olanak sağlamalısınız. Bunu cihazdan yapmak için:

1. VM cihazında bu komutu çalıştırın. HyperVHost1/HyperVHost2 örnek ana bilgisayar adlarıdır.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Alternatif olarak, cihaz la ilgili Yerel Grup İlkesi Düzenleyicisi'nde bunu yapın:
    - **Yerel Bilgisayar İlkesi** > **Bilgisayar Yapılandırmasında,** **Yönetim Şablonları** > **Sistem** > **Kimlik Bilgileri Delegasyonu'na**tıklayın.
    - **Yeni kimlik bilgilerini atamasını İzin ver'e**çift tıklatın ve **Etkin'i**seçin.
    - **Seçenekler'de,** **Wsman/** önek olarak keşfetmek istediğiniz her Hyper-V ana bilgisayarını **göster'i**tıklatın ve ekleyin.
    - **Kimlik Bilgileri Delegasyonu'nda,** **ntlm yalnızca sunucu kimlik doğrulaması ile yeni kimlik bilgilerini ataya izin**ver'i çift tıklatın. Yine, **wsman/** önek olarak, listeye keşfetmek istediğiniz her Hyper-V ana bilgisayar ekleyin.

## <a name="start-continuous-discovery"></a>Sürekli keşfi başlatma

Cihazdan Hyper-V ana bilgisayarlarına veya kümelerine bağlanın ve VM keşfini başlatın.

1. **Kullanıcı adı** ve **Parola'da,** cihazın VM'leri keşfetmek için kullanacağı hesap kimlik bilgilerini belirtin. Kimlik bilgileri için uygun bir ad belirtin ve **ayrıntıları kaydet'i**tıklatın.
2. **Ana bilgisayar ekle'yi**tıklatın ve Hyper-V ana bilgisayar/küme ayrıntılarını belirtin.
3. **Doğrula**'ya tıklayın. Doğrulamadan sonra, her ana bilgisayar/kümede bulunabilecek VM sayısı gösterilir.
    - Doğrulama bir ana bilgisayar için başarısız olursa, **Durum** sütunundaki simgenin üzerine geçerek hatayı gözden geçirin. Sorunları düzeltin ve yeniden doğrulayın.
    - Ana bilgisayarları veya kümeleri kaldırmak için **Sil'>** seçin.
    - Belirli bir ana bilgisayarı kümeden kaldıramazsınız. Yalnızca tüm kümeyi kaldırabilirsiniz.
    - Kümedeki belirli ana bilgisayarlarla ilgili sorunlar olsa bile, bir küme ekleyebilirsiniz.
4. Doğrulamadan sonra **Kaydet'i** tıklatın ve bulma işlemini başlatmak için keşfi başlatın.

Bu keşif başlar. Keşfedilen Sanal M'lerin meta verilerinin Azure portalında görünmesi yaklaşık 15 dakika sürer.

## <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Keşif bittikten sonra, VM'lerin portalda göründüğünü doğrulayabilirsiniz.

1. Azure Geçir panosunu açın.
2. **Azure Geçir - Sunucular** > Azure**Geçir: Sunucu Değerlendirme** **sayfasında, Keşfedilen sunucuların**sayısını görüntüleyen simgeyi tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Geçir Sunucu Değerlendirmesi ile [Hyper-V değerlendirmesini](tutorial-assess-hyper-v.md) deneyin.
