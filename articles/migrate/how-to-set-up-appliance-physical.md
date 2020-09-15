---
title: Fiziksel sunucular için Azure geçişi gereci ayarlama
description: Fiziksel sunucu değerlendirmesi için bir Azure geçişi gereci ayarlamayı öğrenin.
ms.service: azure-migrate
ms.topic: article
ms.date: 04/15/2020
ms.openlocfilehash: bace4ab03051a9febfe14cc30143bd4a5cfa7c84
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085953"
---
# <a name="set-up-an-appliance-for-physical-servers"></a>Fiziksel sunucular için bir gereç ayarlama

Bu makalede, Azure geçişi: Sunucu değerlendirmesi aracı ile fiziksel sunucuları değerlendirmek için Azure geçişi gerecinin nasıl ayarlanacağı açıklanır.

Azure geçişi gereci, aşağıdakileri yapmak için Azure geçişi sunucu değerlendirmesi tarafından kullanılan hafif bir gereç.

- Şirket içi sunucuları bulun.
- Bulunan sunucular için meta verileri ve performans verilerini Azure geçişi sunucu değerlendirmesi ' ne gönderin.

Azure geçişi gereci hakkında [daha fazla bilgi edinin](migrate-appliance.md) .


## <a name="appliance-deployment-steps"></a>Gereç dağıtım adımları

Gereci kurmak için şunları yapın:
- Portal 'da bir gereç adı sağlayın ve bir Azure geçişi proje anahtarı oluşturun.
- Azure geçişi yükleyicisi komut dosyasıyla Azure portal sıkıştırılmış bir dosyayı indirin.
- Sıkıştırılmış dosyadan içerikleri ayıklayın. Yönetim ayrıcalıklarıyla PowerShell konsolunu başlatın.
- Gereç Web uygulamasını başlatmak için PowerShell betiğini yürütün.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi anahtarını kullanarak Azure geçişi projesi ile kaydedin.

### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **fiziksel veya diğer (AWS, GCP, Xen, vb.)** öğesini seçin.
3. **1: Azure geçişi proje anahtarı oluşturma**' da, Azure geçiş gereci için fiziksel veya sanal sunucu keşfi için ayarladığınız bir ad sağlayın. Ad 14 karakter veya daha kısa bir harf olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-installer-script"></a>Yükleyici betiğini indir

**2: Azure geçişi yükleme gereci indirin**ve **İndir**' e tıklayın.

   ![Bulma makineleri için seçimler](./media/tutorial-assess-physical/servers-discover.png)


   ![Anahtar oluştur seçimleri](./media/tutorial-assess-physical/generate-key-physical.png)

### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Daraltılmış dosyanın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Genel bulut için örnek kullanım: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public.zip SHA256 ```
    - Kamu Bulutu için örnek kullanım: ```  C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip MD5 ```
3.  En son gereç sürümünü ve karma değerleri ayarlarını doğrulayın.
 

## <a name="run-the-azure-migrate-installer-script"></a>Azure geçişi yükleyici betiğini çalıştırma
Yükleyici betiği şunları yapar:

- Fiziksel sunucu keşfi ve değerlendirmesi için aracıları ve bir Web uygulamasını kurar.
- Windows etkinleştirme hizmeti, IIS ve PowerShell ıSE dahil Windows rollerini yükler.
- Bir IIS yeniden yazılabilir modülünü indirip yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure geçişi için kalıcı ayar ayrıntılarıyla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Yolun altında aşağıdaki dosyaları oluşturur:
    - **Yapılandırma dosyaları**:%ProgramData%\Microsoft Azure\Config
    - **Günlük dosyaları**:%ProgramData%\Microsoft Azure\Logs

Betiği aşağıdaki gibi çalıştırın:

1. Sıkıştırılmış dosyayı, Gereç barındıracak sunucuda bir klasöre ayıklayın.  Betiği mevcut bir Azure geçişi gereci üzerinde bir makinede çalıştırmayın emin olun.
2. Yönetim (yükseltilmiş) ayrıcalığıyla yukarıdaki sunucuda PowerShell 'i başlatın.
3. PowerShell dizinini, indirilen sıkıştırılmış dosyadan içeriğin ayıklandığı klasör olarak değiştirin.
4. Aşağıdaki komutu çalıştırarak **AzureMigrateInstaller.ps1** adlı betiği çalıştırın:

    - Genel bulut için: 
    
        ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-Public> .\AzureMigrateInstaller.ps1 ```
    - Azure Kamu için: 
    
        ``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>.\AzureMigrateInstaller.ps1 ```

    Betik, başarıyla tamamlandığında gereç Web uygulamasını başlatacaktır.

Herhangi bir sorun yaşıyorsanız, sorun giderme için C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log dosyasına komut dosyası günlüklerine erişebilirsiniz.



### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

1. Gereç ile bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı masaüstünden açabilirsiniz.
2. **Lisans koşullarını**kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Bağlantı**: uygulama, sunucunun internet erişimi olup olmadığını denetler. Sunucu bir proxy kullanıyorsa:
        - Proxy 'yi **Ayarla** ' ya tıklayın ve proxy adresini (form http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında) belirtin.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
        - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
    - **Zaman eşitleme**: Saat doğrulandı. Sunucu bulmanın düzgün çalışması için gereç saatinin internet ile eşitlenmiş olması gerekir.
    - **Güncelleştirmeleri yükleme**: Azure geçişi sunucu değerlendirmesi, gerecin en son güncelleştirmelerin yüklü olduğunu denetler. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. **Oturum**aç ' a tıklayın. Yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açar. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün. 
4. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru [izinleri](tutorial-prepare-physical.md) varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Şimdi, gerecden keşfedilecek fiziksel sunuculara bağlanın ve bulmayı başlatın.

1. **1. Adım: Windows ve Linux fiziksel veya sanal sunucularının bulunması için kimlik bilgilerini sağlayın**, kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, bir Windows veya Linux sunucusu Için **Kullanıcı adı** ve **parola** ekleyin. **Kaydet**'e tıklayın.
1. Aynı anda birden çok kimlik bilgisi eklemek istiyorsanız, kaydetmek için **daha fazla Ekle** ' ye tıklayın ve daha fazla kimlik bilgisi ekleyin. Fiziksel sunucular bulma için birden çok kimlik bilgisi desteklenir.
1. **2. Adım: fiziksel veya sanal sunucu ayrıntılarını sağlayın**sayfasında **bulma kaynağı Ekle** ' ye tıklayarak sunucu **IP adresini/FQDN** 'yi ve sunucuya bağlanacak kimlik bilgileri için kolay adı belirtin.
1. **Tek seferde tek bir öğe ekleyebilir** veya tek bir go içinde **birden fazla öğe ekleyebilirsiniz** . Ayrıca, **Içeri aktarma CSV**aracılığıyla sunucu ayrıntılarını sağlamak için bir seçenek de vardır.

    ![Keşif kaynağı ekleme seçimleri](./media/tutorial-assess-physical/add-discovery-source-physical.png)

    - **Tek öğe Ekle**' yi seçerseniz, işletim sistemi türünü seçebilir, kimlik bilgileri için kolay ad belirtebilir, sunucu **IP adresi/FQDN** ekleyebilir ve **Kaydet**' e tıklayabilirsiniz.
    - **Birden çok öğe Ekle**' yi seçerseniz, metin kutusunda kimlik bilgileri için kolay ada sahip sunucu **IP adresi/FQDN** belirterek, bir kerede birden çok kayıt ekleyebilirsiniz. Eklenen kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.
    - **CSV 'Yi Içeri aktar** ' ı _(varsayılan olarak seçilidir)_ seçerseniz, bir CSV şablon dosyası Indirebilir, dosyayı sunucu **IP adresi/FQDN** ve kimlik bilgileri için kolay ad ile doldurabilirsiniz. Sonra dosyayı gereç içine aktarır, dosyadaki kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.

1. Kaydet ' e tıkladığınızda, Gereç eklenen sunucularla bağlantıyı doğrulamayı dener ve tablodaki **doğrulama durumunu** her bir sunucuya göre gösterir.
    - Sunucu için doğrulama başarısız olursa, tablonun durum sütununda **doğrulama** ' ya tıklayarak hatayı gözden geçirin. Sorunu düzeltin ve tekrar doğrulayın.
    - Bir sunucuyu kaldırmak için **Sil**' e tıklayın.
1. Keşfi başlatmadan önce sunucularla bağlantı **yeniden doğrulayabilirsiniz** .
1. Başarıyla doğrulanan sunucuları bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki her bir sunucu için bulma durumunu denetleyebilirsiniz.


Bu, bulmayı başlatır. Sunucu başına, bulunan sunucunun meta verilerinin Azure portal görünmesi yaklaşık 2 dakika sürer.

## <a name="verify-servers-in-the-portal"></a>Portalda sunucuları doğrulama

Bulma işlemi tamamlandıktan sonra, sunucuların portalda göründüğünü doğrulayabilirsiniz.

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeye tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi sunucu değerlendirmesi ile [fiziksel sunucu değerlendirmesi](tutorial-assess-physical.md) yapmayı deneyin.
