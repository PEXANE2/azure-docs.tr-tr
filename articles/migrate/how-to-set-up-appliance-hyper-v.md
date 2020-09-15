---
title: Hyper-V için Azure geçişi gereci ayarlama
description: Hyper-V VM 'lerini değerlendirmek ve geçirmek için bir Azure geçiş gereci ayarlamayı öğrenin.
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: c53f82268bd1a5d94659a8b749a14fd026f91ce1
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90087159"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Hyper-V VM 'Leri için gereç ayarlama

[Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracı ile Hyper-V VM 'lerinin değerlendirmesi Için Azure geçişi gereci ayarlamak için bu makaleyi izleyin.

Azure [geçişi](migrate-appliance.md)  gereci, Azure geçişi tarafından kullanılan hafif bir gereçdir: şirket içi Hyper-V VM 'lerini keşfetme ve VM meta verileri/performans verilerini Azure 'a göndermek Için Sunucu değerlendirmesi/geçiş.

Gereci birkaç yöntem kullanarak dağıtabilirsiniz:

- İndirilen bir VHD 'yi kullanarak bir Hyper-V sanal makinesine ayarlayın. Bu, bu makalede açıklanan yöntemdir.
- Bir PowerShell yükleyici betiği ile bir Hyper-V VM 'sinde veya fiziksel makinede ayarlama yapın. [Bu yöntem](deploy-appliance-script.md) , bir VHD 'yi kullanarak bir VM ayarlayamıyorum veya Azure Kamu kullanıyorsanız kullanılmalıdır.

Gereci oluşturduktan sonra Azure geçişi 'ne bağlanıp bağlanamadıktan sonra sunucu değerlendirmesi yapın, ilk kez yapılandırın ve Azure geçişi projesine kaydedin.

## <a name="appliance-deployment-vhd"></a>Gereç dağıtımı (VHD)

Bir VHD şablonu kullanarak gereci ayarlamak için:

- Portal 'da bir gereç adı sağlayın ve bir Azure geçişi proje anahtarı oluşturun.
- Azure portal sıkıştırılmış bir Hyper-V VHD 'sini indirin.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi anahtarını kullanarak Azure geçişi projesi ile kaydedin.

### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluşturma**' da, Hyper-V VM 'lerini bulmak için ayarlayabileceğiniz Azure geçiş gereci için bir ad sağlayın. ad 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-vhd"></a>VHD 'YI indirin

**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. VHD dosyası ve **İndir**' e tıklayın. 

   ![Bulma makineleri için seçimler](./media/tutorial-assess-hyper-v/servers-discover.png)


   ![Anahtar oluştur seçimleri](./media/tutorial-assess-hyper-v/generate-key-hyperv.png)


### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. VHD için karma oluşturmak üzere aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.vhd SHA256```





## <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve VM 'yi oluşturun.

1. Sıkıştırılmış VHD dosyasını Hyper-V konağında gereç sanal makinesini barındıracak bir klasöre ayıklayın. Üç klasör ayıklanır.
2. Hyper-V Yöneticisi'ni açın. **Eylemler**' de **sanal makineyi içeri aktar**' a tıklayın.

    ![VHD dağıtma](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

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

    ![Keşif kaynağı ekleme seçimleri](./media/tutorial-assess-hyper-v/add-discovery-source-hyperv.png)

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

Azure geçişi sunucu değerlendirmesi ile [Hyper-V değerlendirmesi](tutorial-assess-hyper-v.md) yapmayı deneyin.
