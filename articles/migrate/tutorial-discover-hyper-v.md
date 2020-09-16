---
title: Azure geçişi sunucu değerlendirmesi ile Hyper-V VM 'lerini bulma
description: Azure geçişi sunucu değerlendirmesi aracı ile şirket içi Hyper-V VM 'lerini bulmayı öğrenin.
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: eb17ba9fc1b68f09f60e857cd20a3f0885bfdb05
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90603960"
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

Bu öğreticide, aşağıdaki gibi bir Hyper-V sanal makinesinde gereç ayarlanır:

- Portal 'da bir gereç adı sağlayın ve bir Azure geçişi proje anahtarı oluşturun.
- Azure portal sıkıştırılmış bir Hyper-V VHD 'sini indirin.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi anahtarını kullanarak Azure geçişi projesi ile kaydedin.
> [!NOTE]
> Bir nedenle, bir şablon kullanarak gereci ayarlayamıyorum, bir PowerShell betiği kullanarak bu gereci ayarlayabilirsiniz. [Daha fazla bilgi edinin](deploy-appliance-script.md#set-up-the-appliance-for-hyper-v).


### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluşturma**' da, Hyper-V VM 'lerini bulmak için ayarlayabileceğiniz Azure geçiş gereci için bir ad sağlayın. ad 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-vhd"></a>VHD 'YI indirin

**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. VHD dosyası ve **İndir**' e tıklayın. 


### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. ZIP dosyasının karmasını oluşturmak için aşağıdaki PowerShell komutunu çalıştırın
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  En son gereç sürümlerini ve karma değerlerini doğrulayın:

    - Azure genel bulutu için:

        **Senaryo** | **İndir** | **SHA256**
        --- | --- | ---
        Hyper-V (10,4 GB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140422) |  79c151588de049cc102f61b910d61360402324dc8d8a14f47772da351b46d9127

    - Azure Kamu için:

        **Senaryon*** | **İndir** | **SHA256**
        --- | --- | ---
        Hyper-V (85 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140424) |  0769c5f8df1e8c1ce4f685296f9ee18e1ca63e4a111d9aa4e6982e069df430d7

### <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve VM 'yi oluşturun.

1. Sıkıştırılmış VHD dosyasını Hyper-V konağında gereç sanal makinesini barındıracak bir klasöre ayıklayın. Üç klasör ayıklanır.
2. Hyper-V Yöneticisi'ni açın. **Eylemler**' de **sanal makineyi içeri aktar**' a tıklayın.
2. Sanal makineyi Içeri aktarma sihirbazında > **başlamadan önce** **İleri**' ye tıklayın.
3. **Klasörü bul**' da ayıklanan VHD 'yi içeren klasörü belirtin. Ardından **İleri**'ye tıklayın.
1. **Sanal makine seç**bölümünde **İleri**' ye tıklayın.
2. **Içeri aktarma türünü seçin**bölümünde **sanal makineyi Kopyala (yenı bir benzersiz kimlik oluştur)** seçeneğine tıklayın. Ardından **İleri**'ye tıklayın.
3. **Hedef Seç**bölümünde varsayılan ayarı bırakın. **İleri**’ye tıklayın.
4. **Depolama klasörlerinde**varsayılan ayarı bırakın. **İleri**’ye tıklayın.
5. **Ağ seçin**bölümünde, VM 'nin kullanacağı sanal anahtarı belirtin. Anahtar, verileri Azure 'a göndermek için internet bağlantısı gerektirir.
6. **Özet**bölümünde ayarları gözden geçirin. Ardından, **Son**'a tıklayın.
7. Hyper-V Yöneticisi 'nde **sanal makineler**>, VM 'yi başlatın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci indirilen VHD yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. Hyper-V Yöneticisi > **sanal makineler**' de, **bağlan**' a > VM ' ye sağ tıklayın.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
1. **Lisans koşullarını**kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
      - Proxy 'yi **Ayarla** ' ya tıklayın ve proxy adresini (form http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında) belirtin.
      - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
      - Yalnızca HTTP proxy’si desteklenir.
      - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
    - **Zaman eşitleme**: Saat doğrulandı. VM bulmanın düzgün çalışması için gereç süresi internet saatine eşit olmalıdır.
    - **Güncelleştirmeleri yükleme**: Azure geçişi sunucu değerlendirmesi, gerecin en son güncelleştirmelerin yüklü olduğunu denetler. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. **Oturum**aç ' a tıklayın. Yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açar. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün. 
4. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru [izinleri](tutorial-prepare-hyper-v.md#prepare-azure) varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.



### <a name="delegate-credentials-for-smb-vhds"></a>SMB VHD 'leri için temsilci kimlik bilgileri

SMB 'lerde VHD 'ler çalıştırıyorsanız, Gereç üzerinden Hyper-V konaklarına kimlik bilgileri temsilcisini etkinleştirmeniz gerekir. Bunu gereç içinden yapmak için:

1. Gereç sanal makinesinde bu komutu çalıştırın. HyperVHost1/HyperVHost2, örnek ana bilgisayar adlarıdır.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Alternatif olarak, bunu gereç üzerindeki Yerel Grup İlkesi Düzenleyicisi yapın:
    - **Yerel bilgisayar ilkesi**  >  **bilgisayar yapılandırması**' nda, **Yönetim Şablonları**  >  **sistem**  >  **kimlik bilgileri temsili**' ne tıklayın.
    - **Yeni kimlik bilgileri aktarımına Izin ver**' e çift tıklayın ve **etkin**' i seçin.
    - **Seçenekler**' de **göster**' e tıklayın ve bulmayı Istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.
    - **Kimlik bilgileri temsilcisi**içinde, **yalnızca NTLM sunucu kimlik doğrulamasıyla yeni kimlik bilgileri aktarmaya izin ver**' e çift tıklayın. Daha sonra, bulmayı istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecden Hyper-V konaklarına veya kümelerine bağlanın ve VM bulmayı başlatın.

1. **1. Adım: Hyper-V ana bilgisayar kimlik bilgilerini sağlayın**bölümünde kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, gerecin VM 'leri bulması için kullanacağı bir Hyper-v Konağı/kümesi Için **Kullanıcı adı** ve **parola** ekleyin. **Kaydet**'e tıklayın.
1. Aynı anda birden çok kimlik bilgisi eklemek istiyorsanız, kaydetmek için **daha fazla Ekle** ' ye tıklayın ve daha fazla kimlik bilgisi ekleyin. Hyper-V VM 'Leri bulmak için birden çok kimlik bilgisi desteklenir.
1. **2. Adım: Hyper-v Konağı/kümesi ayrıntılarını sağlama**bölümünde, Hyper-v Konağı/kümesi **IP adresini/FQDN** 'yi ve konağa/kümeye bağlanacak kimlik bilgileri için kolay adı belirtmek üzere **bulma kaynağı Ekle** ' ye tıklayın.
1. **Tek seferde tek bir öğe ekleyebilir** veya tek bir go içinde **birden fazla öğe ekleyebilirsiniz** . Ayrıca Hyper-V konağı/kümesi ayrıntılarını **Içeri aktarma CSV**aracılığıyla sağlama seçeneği de vardır.


    - **Tek öğe Ekle**' yi seçerseniz, kimlik bilgileri ve Hyper-V konağı/kümesi **IP adresi/FQDN** için kolay ad belirtmeniz ve **Kaydet**' e tıklamanız gerekir.
    - **Birden çok öğe Ekle** _(varsayılan olarak seçilidir)_ seçeneğini belirlerseniz, metin kutusunda kimlik bilgileri Için kolay ad ile Hyper-V konağı/KÜMESI **IP adresi/FQDN** belirterek birden çok kayıt ekleyebilirsiniz. Eklenen kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.
    - **CSV 'Yi Içeri aktar**' ı SEÇERSENIZ bir CSV şablon dosyası indirebilir, dosyayı Hyper-V konağı/kümesi **IP adresi/FQDN** ve kimlik bilgileri için kolay ad ile doldurabilirsiniz. Sonra dosyayı gereç içine aktarır, dosyadaki kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.

1. Kaydet 'e tıkladığınızda gereç, eklenen Hyper-V konaklarına/kümelerine bağlantıyı doğrulamaya çalışır ve her konak/küme için tablodaki **doğrulama durumunu** gösterir.
    - Başarıyla doğrulanan konaklar/kümeler için IP adreslerine/FQDN 'ye tıklayarak daha fazla ayrıntı görüntüleyebilirsiniz.
    - Bir konakta doğrulama başarısız olursa, tablonun durum sütununda **doğrulama** ' ya tıklayarak hatayı gözden geçirin. Sorunu düzeltin ve tekrar doğrulayın.
    - Konakları veya kümeleri kaldırmak için **Sil**'e tıklayın.
    - Belirli bir konağı kümeden kaldıramazsınız. Tüm kümeyi yalnızca çıkarabilirsiniz.
    - Kümedeki belirli konaklarla ilgili sorunlar olsa bile bir küme ekleyebilirsiniz.
1. Keşfi başlatmadan önce herhangi bir zamanda konaklara/kümelere bağlantıyı **yeniden doğrulayabilirsiniz** .
1. Başarıyla doğrulanan konaklardan/kümelerdeki VM bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki her bir konağa/kümeye yönelik bulma durumunu denetleyebilirsiniz.

Bu, bulmayı başlatır. Bulunan sunucuların meta verilerinin Azure portal görünmesi için ana bilgisayar başına yaklaşık 2 dakika sürer.

## <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma işlemi tamamlandıktan sonra, VM 'Lerin portalda göründüğünü doğrulayabilirsiniz.

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerine geçiş için [Hyper-V VM 'Lerini değerlendirin](tutorial-assess-hyper-v.md) .
- Bulma sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---hyper-v) .
