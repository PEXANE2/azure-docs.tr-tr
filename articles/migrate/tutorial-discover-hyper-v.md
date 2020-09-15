---
title: Azure geçişi sunucu değerlendirmesi ile Hyper-V VM 'lerini bulma
description: Azure geçişi sunucu değerlendirmesi aracı ile şirket içi Hyper-V VM 'lerini bulmayı öğrenin.
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: fdf96b5767b461953fa88923aaa5050aff4613bc
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90064494"
---
# <a name="tutorial-discover-hyper-v-vms-with-server-assessment"></a>Öğretici: Sunucu değerlendirmesi ile Hyper-V VM 'lerini bulma

Azure 'a geçiş sürecinizin bir parçası olarak şirket içi envanterinizi ve iş yüklerinizi keşfedeceksiniz. 

Bu öğreticide, basit bir Azure geçişi gereci kullanarak Azure geçişi: Sunucu değerlendirmesi aracı ile şirket içi Hyper-V sanal makinelerini (VM) nasıl keşfedebileceğiniz gösterilmektedir. Gereci, makine ve performans meta verilerini sürekli olarak bulacak şekilde Hyper-V VM 'si olarak dağıtırsınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure hesabı ayarlama
> * Hyper-V ortamını bulma için hazırlayın.
> * Bir Azure Geçişi projesi oluşturun.
> * Azure geçişi gereci ayarlayın.
> * Sürekli bulmayı başlatın.

> [!NOTE]
> Öğreticiler, bir senaryoyu denemek için en hızlı yolu gösterir ve varsayılan seçenekleri kullanır.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce, bu önkoşulların yerinde olup olmadığını kontrol edin.


**Gereksinim** | **Ayrıntılar**
--- | ---
**Hyper-V konağı** | VM 'Lerin bulunduğu Hyper-V konakları tek başına veya bir kümede olabilir.<br/><br/> Konağın Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2 çalıştırması gerekir.<br/><br/> BT 'nin Genel Bilgi Modeli (CıM) oturumu kullanarak çekme VM meta verileri ve performans verilerine bağlanabilmesi için WinRM bağlantı noktası 5985 ' de (HTTP) gelen bağlantılara izin verildiğini doğrulayın.
**Gereç dağıtımı** | vCenter Server, Gereç için bir VM ayırmak üzere kaynaklara ihtiyaç duyuyor:<br/><br/> - Windows Server 2016<br/><br/> -32 GB RAM<br/><br/> -Sekiz vCPU<br/><br/> -Yaklaşık 80 GB disk depolaması.<br/><br/> -Dış sanal anahtar.<br/><br/> -VM için doğrudan veya bir ara sunucu üzerinden Internet erişimi.
**VM’ler** | VM 'Ler herhangi bir Windows veya Linux işletim sistemi çalıştırıyor olabilir. 

Başlamadan önce, gerecin bulma sırasında topladığı [verileri gözden](migrate-appliance.md#collected-data---hyper-v) geçirebilirsiniz.

## <a name="prepare-an-azure-user-account"></a>Azure Kullanıcı hesabı hazırlama

Azure geçişi projesi oluşturmak ve Azure geçişi gerecini kaydettirmek için, şu bir hesaba sahip olmanız gerekir:
- Azure aboneliğinde katkıda bulunan veya sahip izinleri.
- Azure Active Directory uygulamaları kaydetme izinleri.

Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz. Abonelik sahibi değilseniz, izinleri aşağıdaki şekilde atamak için sahibiyle birlikte çalışın:


1. Azure portal, "abonelikler" araması yapın ve **Hizmetler**altında **abonelikler**' i seçin.

    ![Azure aboneliğini aramak için arama kutusu](./media/tutorial-discover-hyper-v/search-subscription.png)

2. **Abonelikler** sayfasında, Azure geçişi projesi oluşturmak istediğiniz aboneliği seçin. 
3. Abonelikte **erişim denetimi (IAM)**  >  **erişimi denetle**' yi seçin.
4. **Erişimi denetle**' de ilgili Kullanıcı hesabını arayın.
5. **Rol ataması Ekle**' de, **Ekle**' ye tıklayın.

    ![Erişimi denetlemek ve rol atamak için bir kullanıcı hesabı arayın](./media/tutorial-discover-hyper-v/azure-account-access.png)

6. **Rol ataması Ekle**' de, katkıda bulunan veya sahip rolünü seçin ve hesabı (örneğimizde azmigrateuser) seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Hesaba rol atamak için rol ataması Ekle sayfasını açar](./media/tutorial-discover-hyper-v/assign-role.png)

7. Portalda, kullanıcılar için arama yapın ve **Hizmetler**altında **Kullanıcılar**' ı seçin.
8. **Kullanıcı ayarları**' nda, Azure AD kullanıcılarının uygulamaları kaydedebildiğini doğrulayın (varsayılan olarak **Evet** ' e ayarlanır).

    ![Kullanıcıların Active Directory uygulamalar kaydedebildiğini Kullanıcı ayarlarında doğrula](./media/tutorial-discover-hyper-v/register-apps.png)

## <a name="prepare-hyper-v-hosts"></a>Hyper-V konakları hazırlama

Hyper-V konaklarında yönetici erişimi olan bir hesap ayarlayın. Gereç bu hesabı bulma için kullanır.

- Seçenek 1: Hyper-V konak makinesine yönetici erişimi olan bir hesap hazırlayın.
- 2. seçenek: bir yerel yönetici hesabı veya etki alanı yönetici hesabı hazırlayın ve hesabı şu gruplara ekleyin: uzak yönetim kullanıcıları, Hyper-V yöneticileri ve performans Izleyicisi kullanıcıları.


## <a name="set-up-a-project"></a>Proje ayarlama

Yeni bir Azure geçişi projesi kurun.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde **proje oluştur**' u seçin.
5. **Proje oluştur**' da Azure aboneliğinizi ve kaynak grubunuzu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-discover-hyper-v/new-project.png)

7. **Oluştur**’u seçin.
8. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin.

**Azure geçişi: Sunucu değerlendirmesi** Aracı, varsayılan olarak yeni projeye eklenir.

![Varsayılan olarak eklenen sunucu değerlendirmesi aracını gösteren sayfa](./media/tutorial-discover-hyper-v/added-tool.png)


## <a name="set-up-the-appliance"></a>Gereci ayarlama

Bu öğreticide, bir Hyper-V VM 'si üzerindeki gereç ayarlanır.
- Gereç şablonunu indirir ve gereç sanal makinesini oluşturmak için şablonu vCenter Server içeri aktarmalısınız.
- Gereci oluşturduktan sonra ilk kez ayarlayın ve Azure geçişi projesine kaydedin.

> [!NOTE]
> Bir nedenle, bir şablon kullanarak gereci ayarlayamıyorum, bir PowerShell betiği kullanarak bu gereci ayarlayabilirsiniz. [Daha fazla bilgi edinin](deploy-appliance-script.md#set-up-the-appliance-for-hyper-v).


### <a name="download-the-vhd"></a>VHD 'YI indirin

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
3. VHD dosyasını indirmek için **İndir** ' i seçin.

   ![OVA dosyasını indirme seçimleri](./media/tutorial-discover-hyper-v/download-vhd.png)



### <a name="deploy-the-appliance-vm"></a>Gereç sanal makinesini dağıtma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun:

1. Sıkıştırılmış VHD dosyasını, Gereç VM 'sinin yerleştirileceği Hyper-V konağına indirin.
2. Daraltılmış dosyayı ayıklayın.

    - Ayıklanan konumda, dosya AzureMigrateAppliance_VersionNumber adlı bir klasöre UNIP 'leri yok.
    - Bu klasör AzureMigrateAppliance_VersionNumber olarak da adlandırılan bir alt klasör içerir.
    - Bu alt klasör, daha fazla üç alt görüntü, sanal sabit disk ve sanal makine içerir.

3. Hyper-V Yöneticisi'ni açın. **Eylemler**' de **sanal makineyi içeri aktar**' a tıklayın.

    ![VHD 'YI içeri aktarmak için Hyper-V Yöneticisi 'ndeki menü öğesi](./media/tutorial-discover-hyper-v/deploy-vhd.png)

4. Sanal makineyi Içeri aktarma sihirbazında > **başlamadan önce** **İleri**' ye tıklayın.
5. **Klasörü bul**' da, **sanal makineler** klasörünü seçin. Ardından **İleri**'ye tıklayın.
6. **Sanal makine seç**bölümünde **İleri**' ye tıklayın.
7. **Içeri aktarma türünü seçin**bölümünde **sanal makineyi Kopyala (yenı bir benzersiz kimlik oluştur)** seçeneğine tıklayın. Ardından **İleri**'ye tıklayın.
8. **Hedef Seç**bölümünde varsayılan ayarı bırakın. **İleri**’ye tıklayın.
9. **Depolama klasörlerinde**varsayılan ayarı bırakın. **İleri**’ye tıklayın.
10. **Ağ seçin**bölümünde, VM 'nin kullanacağı sanal anahtarı belirtin. Anahtar, verileri Azure 'a göndermek için internet bağlantısı gerektirir. Sanal anahtar oluşturma hakkında [bilgi edinin](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) .
11. **Özet**bölümünde ayarları gözden geçirin. Ardından, **Son**'a tıklayın.
12. Hyper-V Yöneticisi 'nde **sanal makineler**>, VM 'yi başlatın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç erişimini aşağıdaki gibi denetleyin:

1. VM 'nin Azure 'a bağlanıp bağlanamadan emin olun.
    - Ortak bulutta, Gereç makinesi bu [URL 'lere](migrate-appliance.md#public-cloud-urls)bağlanabilmelidir.
    - Kamu bulutunda, Gereç bu [devlet URL 'lerine](migrate-appliance.md#government-cloud-urls)bağlanabilmelidir.
2. Bu bağlantı noktalarının gereç makinesinde açık olduğundan emin olun:

    - TCP bağlantı noktası 3389 üzerinde gelen bağlantılara, Gereç ile Uzak Masaüstü bağlantılarına izin vermek için izin verin.
    - 44368 numaralı bağlantı noktası üzerinde gelen bağlantılara izin ver: https://: 44368 URL 'sini kullanarak gereç Web uygulamasına uzaktan erişin \<appliance-ip-or-name> .
    - Bağlantı noktası 443 ' de (HTTPS) giden bağlantılara izin vermek için Azure geçişi 'ne bulma ve performans meta verileri gönderme.


### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci indirilen VHD yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. Hyper-V Yöneticisi > **sanal makineler**' de, **bağlan**' a > VM ' ye sağ tıklayın.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Lisans**: lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
      - **Proxy ayarları**' na tıklayın ve proxy adresini ve dinleme bağlantı noktasını, veya biçiminde belirtin http://ProxyIPAddress http://ProxyFQDN .
      - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
      - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Saat doğrulandı. VM bulmanın düzgün çalışması için gereç süresi internet saatine eşit olmalıdır.
    - **Güncelleştirmeleri yükleme**: Azure geçişi sunucu değerlendirmesi, gerecin en son güncelleştirmelerin yüklü olduğunu denetler.

### <a name="register-the-appliance"></a>Gereci Kaydet 

1. **Azure geçişi Ile kaydolun**bölümünde **oturum aç**' ı seçin. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.

    ![Gereci kaydetmeye başlamak için oturum aç ' a tıklayın](./media/tutorial-discover-hyper-v/register.png)

1. **Oturum aç** sayfasında, Azure Kullanıcı adı ve parolanızla oturum açın. PIN ile oturum açma desteklenmez.

    ![Gereci kaydetmek için oturum aç düğmesi](./media/tutorial-discover-hyper-v/sign-in.png)
1. Başarıyla oturum açtıktan sonra uygulamaya geri dönün.
1. **Azure geçişi Ile kaydolun**bölümünde, Azure geçişi projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
1. Gereç için bir ad belirtin. Ad 14 karakter veya daha kısa bir harf olmalıdır.
3. **Kaydet**’i seçin. Sonra **devam**' a tıklayın. Bir ileti başarıyla kaydı gösterir.

    ![Abonelik, proje ve gereç adını doldur ve gereci Kaydet](./media/tutorial-discover-hyper-v/success-register.png)


### <a name="delegate-credentials-for-smb-vhds"></a>SMB VHD 'leri için temsilci kimlik bilgileri

SMB 'lerde VHD 'ler çalıştırıyorsanız, Gereç üzerinden Hyper-V konaklarına kimlik bilgileri temsilcisini etkinleştirmeniz gerekir. Bunu yapmak için, her bir konağın gereç için bir temsilci görevi görmesini sağlayabilirsiniz. Öğreticileri sırayla izlediyseniz, değerlendirme ve geçiş için Hyper-V ' yi hazırladığınız zaman önceki öğreticide bunu yaptınız. Konaklar için CredSSP 'yi [el ile](tutorial-prepare-hyper-v.md#enable-credssp-to-delegate-credentials)veya bunu yapan [bir komut dosyası çalıştırarak](tutorial-prepare-hyper-v.md#run-the-script) ayarlamanız gerekir.

Gereçte aşağıdaki şekilde etkinleştirin:

#### <a name="option-1"></a>1\. Seçenek

Gereç sanal makinesinde bu komutu çalıştırın. HyperVHost1/HyperVHost2, örnek ana bilgisayar adlarıdır.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
```

Örnek: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>2\. Seçenek

Alternatif olarak, bunu gereç üzerindeki Yerel Grup İlkesi Düzenleyicisi yapın:

1. **Yerel bilgisayar ilkesi**  >  **bilgisayar yapılandırması**' nda, **Yönetim Şablonları**  >  **sistem**  >  **kimlik bilgileri temsili**' ne tıklayın.
2. **Yeni kimlik bilgileri aktarımına Izin ver**' e çift tıklayın ve **etkin**' i seçin.
3. **Seçenekler**' de **göster**' e tıklayın ve bulmayı Istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.
4. Ardından, **kimlik bilgileri temsilcisi**' nde, **yalnızca NTLM sunucu kimlik doğrulamasıyla yeni kimlik bilgileri aktarmaya izin ver**' e çift tıklayın. Daha sonra, bulmayı istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.



## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Sanal makinelerin, VM 'Leri bulması için Hyper-V konaklarına bağlanması gerekir.


### <a name="connect-to-hyper-v-hosts"></a>Hyper-V konaklarına bağlanma

1. **Kullanıcı adı** ve **parola**' da, gerecin VM 'leri bulması için kullanacağı hesap kimlik bilgilerini belirtin. Kimlik bilgileri için kolay bir ad belirtin ve **ayrıntıları kaydet**' e tıklayın.
2. **Konak Ekle**' ye tıklayın ve Hyper-V konağı/küme ayrıntılarını belirtin.
3. **Doğrula**'ya tıklayın. Doğrulamadan sonra, her bir konakta/kümede keşfedilebilir VM sayısı gösterilir.
    - Bir konakta doğrulama başarısız olursa, **durum** sütunundaki simgenin üzerine gelerek hatayı gözden geçirin. Sorunları giderin ve yeniden doğrulayın.
    - Konakları veya kümeleri kaldırmak için > **Sil**' i seçin.
    - Belirli bir konağı kümeden kaldıramazsınız. Tüm kümeyi yalnızca çıkarabilirsiniz.
    - Kümedeki belirli konaklarla ilgili sorunlar olsa bile bir küme ekleyebilirsiniz.
4. Doğrulamadan sonra, bulma işlemini başlatmak için **Kaydet ve bulmayı Başlat** ' a tıklayın.

Bu, bulmayı başlatır. Bulunan sunucuların meta verilerinin Azure portal görünmesi için ana bilgisayar başına yaklaşık 1,5 dakika sürer.


### <a name="verify-discovered-vms-in-the-portal"></a>Portalda bulunan VM 'Leri doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucuların**sayısını gösteren simgeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerine geçiş için [Hyper-V VM 'Lerini değerlendirin](tutorial-assess-hyper-v.md) .
- Bulma sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---hyper-v) .