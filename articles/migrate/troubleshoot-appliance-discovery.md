---
title: Azure geçişi gereç dağıtımı ve bulma sorunlarını giderme
description: Azure geçişi gereci dağıtma ve makineler bulma konusunda yardım alın.
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 92a8e129188f2790a3e46162b207373b5d6e6ce4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85611365"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Azure geçişi Gereç ve bulma sorunlarını giderme

Bu makale, [Azure geçiş](migrate-services-overview.md) gereci dağıtma ve şirket içi makineleri bulma konusunda gereç kullanma konularında sorunları gidermenize yardımcı olur.


## <a name="whats-supported"></a>Neler desteklenir?

Gereç destek gereksinimlerini [gözden geçirin](migrate-appliance.md) .


## <a name="invalid-ovf-manifest-entry"></a>"Geçersiz OVF manifest girdisi"

"Belirtilen bildirim dosyası geçersiz: geçersiz OVF manifest entry" hatasını alırsanız şunları yapın:

1. Azure geçişi gereç OVA dosyasının karma değerini denetleyerek doğru şekilde indirildiğini doğrulayın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Karma değeri eşleşmiyorsa, OVA dosyasını yeniden indirin ve dağıtımı yeniden deneyin.
2. Dağıtım hala başarısız olursa ve OVF dosyasını dağıtmak için VMware vSphere istemcisini kullanıyorsanız, vSphere Web istemcisi aracılığıyla dağıtmayı deneyin. Dağıtım hala başarısız olursa, farklı bir Web tarayıcısı kullanmayı deneyin.
3. VSphere Web istemcisini kullanıyorsanız ve vCenter Server 6,5 veya 6,7 ' de dağıtmaya çalışıyorsanız, OVA 'yı doğrudan ESXi konağına dağıtmayı deneyin:
   - Web istemcisi (https://<*ana BILGISAYAR IP adresi*>/UI) Ile ESXi konağına doğrudan (vCenter Server yerine) bağlanın.
   - **Ev**  >  **envanterinde**, **File**  >  **ovf şablonu**' nu Dağıt ' ı seçin. OVA 'ya gidin ve dağıtımı doldurun.
4. Dağıtım hala başarısız olursa Azure geçiş desteği 'ne başvurun.

## <a name="cant-connect-to-the-internet"></a>İnternet 'e bağlanılamıyor

Gereç makinesi bir proxy 'nin arkasındaysa bu durum oluşabilir.

- Proxy 'ye ihtiyaç duyuyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
- Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy 'si kullanıyorsanız, [Bu URL 'leri](migrate-appliance.md#url-access) izin verilenler listesine ekleyin.
- İnternet 'e bağlanmak için bir kesintiye uğratan ara sunucu kullanıyorsanız, [Bu adımları](https://docs.microsoft.com/azure/migrate/concepts-collector)kullanarak proxy SERTIFIKASıNı gereç sanal makinesine aktarın.

## <a name="cant-sign-into-azure-from-the-appliance-web-app"></a>Azure 'da gereç Web uygulamasından oturum açılamıyor

Azure 'da oturum açmak için yanlış Azure hesabı kullanıyorsanız, "Üzgünüz, ancak oturumunuzu açarken sorun yaşıyoruz" hatası görüntülenir. Bu hata birkaç nedenden dolayı oluşur:

- Kamu Bulutu için gereç Web uygulamasında oturum açarsanız, kamu bulutu portalının Kullanıcı hesabı kimlik bilgilerini kullanarak.
- Özel bulut portalının Kullanıcı hesabı kimlik bilgilerini kullanarak kamu bulutu için gereç Web uygulamasında oturum açarsanız.

Doğru kimlik bilgilerini kullandığınızdan emin olun.

##  <a name="datetime-synchronization-error"></a>Tarih/saat eşitleme hatası

Tarih ve saat eşitleme (802) ile ilgili bir hata, sunucu saatinin beş dakikadan uzun bir süredir geçerli zamandan eşitlenmemiş olabileceğini gösterir. Toplayıcı VM 'sinin saat saatini Şu anki saatle eşleşecek şekilde değiştirin:

1. VM 'de bir yönetici komut istemi açın.
2. Saat dilimini denetlemek için **w32tm/tz komutunu**çalıştırın.
3. Saati eşitleme için **w32tm/resync**komutunu çalıştırın.


## <a name="unabletoconnecttoserver"></a>"UnableToConnectToServer"

Bu bağlantı hatası alırsanız, vCenter Server *ServerName*. com: 9443 öğesine bağlanamadıysanız. Hata ayrıntıları, `https://\*servername*.com:9443/sdk` iletiyi kabul edebilecek bir uç nokta dinleme olmadığını gösterir.

- En son gereç sürümünü çalıştırıp çalıştırmadığını denetleyin. Değilseniz, gereci [en son sürüme](https://docs.microsoft.com/azure/migrate/concepts-collector)yükseltin.
- Sorun hala en son sürümde gerçekleşirse, Gereç belirtilen vCenter Server adını çözemeyebilir veya belirtilen bağlantı noktası yanlış olabilir. Varsayılan olarak, bağlantı noktası belirtilmemişse, toplayıcı 443 numaralı bağlantı noktası numarasına bağlanmayı dener.

    1. Gereci *ServerName*. com ' a ping yapın.
    2. 1. adım başarısız olursa, IP adresini kullanarak vCenter Server 'a bağlanmayı deneyin.
    3. VCenter Server bağlanmak için doğru bağlantı noktası numarasını belirler.
    4. VCenter Server çalışır olduğunu doğrulayın.


## <a name="error-6005260039-appliance-might-not-be-registered"></a>Hata 60052/60039: gereç kaydettirilmemiş olabilir

- Hata 60052, "gereci kaydetmek için kullanılan Azure hesabının izinleri yetersizse," gereç Azure geçişi projesine başarıyla kaydedilmemiş olabilir "hatası oluşur.
    - Gereci kaydetmek için kullanılan Azure Kullanıcı hesabının abonelikte en az katkıda bulunan izinleri olduğundan emin olun.
    - Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) .
- Hata 60039, "gereç, kayıt başarısız olursa, kayıt başarısız olursa, Gereç kayıt işlemi için kullanılan Azure geçişi projesi bulunamadığı için," gereç Azure geçiş projesi 'ne başarıyla kaydettirilmemiş "olabilir
    - Azure portal ve projenin kaynak grubunda mevcut olup olmadığını kontrol edin.
    - Proje yoksa, kaynak grubunuzda yeni bir Azure geçişi projesi oluşturun ve gereci yeniden kaydedin. Yeni bir proje oluşturmayı [öğrenin](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) .

## <a name="error-6003060031-key-vault-management-operation-failed"></a>Hata 60030/60031: Key Vault Yönetimi işlemi başarısız oldu

60030 veya 60031 hatası alırsanız, "bir Azure Key Vault yönetim işlemi başarısız oldu", şunları yapın:
- Gereci kaydetmek için kullanılan Azure Kullanıcı hesabının abonelikte en az katkıda bulunan izinleri olduğundan emin olun.
- Hesabın hata iletisinde belirtilen anahtar kasasına erişimi olduğundan emin olun ve işlemi yeniden deneyin.
- Sorun devam ederse, Microsoft desteğine başvurun.
- Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) .

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Hata 60028: bulma başlatılamadı

Hata 60028: "bir hata nedeniyle bulma başlatılamadı. Belirtilen konaklar veya kümeler listesi için işlem başarısız oldu "VM bilgilerine erişirken veya alırken bir sorun nedeniyle hatada listelenen konaklarda bulma işleminin başlatılamayacağını gösterir. Ana bilgisayarların geri kalanı başarıyla eklendi.

- Hatada listelenen Konakları, **ana bilgisayar Ekle** seçeneğini kullanarak yeniden ekleyin.
- Doğrulama hatası varsa, hataları onarmak için düzeltme kılavuzunu gözden geçirin ve sonra **bulmayı Kaydet ve Başlat** seçeneğini tekrar deneyin.

## <a name="error-60025-azure-ad-operation-failed"></a>Hata 60025: Azure AD işlemi başarısız oldu 
Hata 60025: "Azure AD işlemi başarısız oldu. Azure AD uygulaması oluşturulurken veya güncelleştirilirken oluşan hata oluştu "bulmayı başlatmak için kullanılan Azure Kullanıcı hesabı gereci kaydetmek için kullanılan hesaptan farklı olduğunda gerçekleşir. Şunlardan birini yapın:

- Keşfi başlatan kullanıcı hesabının gereci kaydetmek için kullanılan ile aynı olduğundan emin olun.
- Bulma işleminin başarısız olduğu Kullanıcı hesabına Azure Active Directory Uygulama erişim izinleri sağlayın.
- Azure geçişi projesi için önceden oluşturulan kaynak grubunu silin. Yeniden başlamak için başka bir kaynak grubu oluşturun.
- Azure Active Directory Uygulama izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) .


## <a name="error-50004-cant-connect-to-host-or-cluster"></a>Hata 50004: konağa veya kümeye bağlanılamıyor

Hata 50004: "sunucu adı çözümlenemediği için bir konağa veya kümeye bağlanılamıyor. WinRM hata kodu: gereç için Azure DNS hizmeti, verdiğiniz küme veya ana bilgisayar adını çözümleyemezse, 0x803381B9 "gerçekleşebilir.

- Kümede bu hatayı görürseniz, küme FQDN 'SI.
- Ayrıca, bir kümedeki konaklar için bu hatayı görebilirsiniz. Bu, gerecin kümeye bağlanabildiğini, ancak kümenin FQDN olmayan ana bilgisayar adlarını döndürdüğünü gösterir. Bu hatayı çözmek için, IP adresi ve ana bilgisayar adlarının eşlemesini ekleyerek gereç üzerindeki Hosts dosyasını güncelleştirin:
    1. Not defteri 'Ni yönetici olarak açın.
    2. C:\Windows\System32\Drivers\etc\hosts dosyasını açın.
    3. IP adresini ve ana bilgisayar adını bir satıra ekleyin. Bu hatayı gördüğünüz her bir konak veya küme için tekrarlayın.
    4. Hosts dosyasını kaydedin ve kapatın.
    5. Gereç Yönetimi uygulamasını kullanarak gerecin konaklara bağlanıp bağlanamayacağını denetleyin. 30 dakika sonra, Azure portal bu konaklar için en son bilgileri görmeniz gerekir.

## <a name="discovered-vms-not-in-portal"></a>Bulunan VM 'Ler portalda yok

Bulma durumu "devam ediyor" ise ancak portalda VM 'Leri görmüyorsanız, birkaç dakika bekleyin:
- VMware VM 'si için 15 dakika sürer.
- Hyper-V VM keşfi için eklenen her konak için iki dakika sürer.

Beklerseniz ve durum değişmezse, **sunucular** sekmesinde **Yenile** ' yi seçin. Bu, Azure geçişi 'nde bulunan sunucu sayısını göstermelidir: Sunucu değerlendirmesi ve Azure geçişi: sunucu geçişi.

Bu işe yaramazsa ve VMware sunucularını keşfederken:

- Belirttiğiniz vCenter hesabının, en az bir VM 'ye erişimi olan izinlerin doğru şekilde ayarlandığını doğrulayın.
- VCenter hesabının vCenter VM klasör düzeyinde erişimi varsa Azure geçişi, VMware VM 'lerini bulamıyor. Kapsam bulma hakkında [daha fazla bilgi edinin](set-discovery-scope.md) .

## <a name="vm-data-not-in-portal"></a>VM verileri portalda yok

Bulunan VM 'Ler portalda görünmezse veya VM verileri güncel değilse, birkaç dakika bekleyin. Keşfedilen VM yapılandırma verilerinde yapılan değişikliklerin portalda görünmesi 30 dakika kadar sürer. Uygulama verilerinde değişikliklerin görünmesi birkaç saat sürebilir. Bu süreden sonra veri yoksa yenilemeyi şu şekilde deneyin

1. **Sunucular**  >  **Azure geçişi sunucu değerlendirmesi**' nde, **genel bakış**' ı seçin.
2. **Yönet**altında **Aracı durumu**' yi seçin.
3. **Aracıyı Yenile**' yi seçin.
4. Yenileme işleminin tamamlanmasını bekleyin. Şimdi güncel bilgileri görmeniz gerekir.

## <a name="deleted-vms-appear-in-portal"></a>Portalda silinen VM 'Ler görüntülenir

VM 'Leri silerseniz ve portalda hala görünüyorsa 30 dakika bekleyin. Hala görünüyorsa, yukarıda açıklandığı gibi yenileyin.

## <a name="i-do-not-see-performance-data-for-some-network-adapters-on-my-physical-servers"></a>Fiziksel sunucularım üzerinde bazı ağ bağdaştırıcılarının performans verilerini görmüyorum

Fiziksel sunucuda Hyper-V Sanallaştırması etkinleştirilmişse bu durum oluşabilir. Bir ürün boşluğu nedeniyle, ağ aktarım hızı bulunan sanal ağ bağdaştırıcılarında yakalanır.

## <a name="error-the-file-uploaded-is-not-in-the-expected-format"></a>Hata: Yüklenen dosya beklenen biçimde değil
Bazı araçların bölgesel ayarları, sınırlayıcı olarak noktalı virgül kullanan CSV dosyaları oluşturur. Ayarların sınırlayıcı olarak virgül kullandığından emin olun.

## <a name="i-imported-a-csv-but-i-see-discovery-is-in-progress"></a>CSV’yi içeri aktardım ancak “Keşif sürüyor” yazısıyla karşılaşıyorum
Bu durum, CSV yüklemeniz doğrulama hatası nedeniyle başarısız olduysa görüntülenir. CSV’yi içeri aktarmayı yeniden deneyin. Önceki karşıya yüklemenin hata raporunu indirebilir ve hataları gidermek için düzeltme kılavuzunu izleyebilirsiniz. Hata raporları ‘Makineleri keşfet’ sayfasındaki ‘Ayrıntıları içeri aktar’ bölümünden indirilebilir.

## <a name="do-not-see-application-details-even-after-updating-guest-credentials"></a>Konuk kimlik bilgilerini güncelleştirdikten sonra bile uygulama ayrıntılarını görmeyin
Uygulama bulma her 24 saatte bir çalışır. Ayrıntıları hemen görmek isterseniz aşağıdaki gibi yenileyin. Hayır öğesine bağlı olarak bu işlem birkaç dakika sürebilir. bulunan VM 'Ler.

1. **Sunucular**  >  **Azure geçişi sunucu değerlendirmesi**' nde, **genel bakış**' ı seçin.
2. **Yönet**altında **Aracı durumu**' yi seçin.
3. **Aracıyı Yenile**' yi seçin.
4. Yenileme işleminin tamamlanmasını bekleyin. Şimdi güncel bilgileri görmeniz gerekir.

## <a name="unable-to-export-application-inventory"></a>Uygulama envanteri dışarı aktarılamıyor
Portalda envanterden indirilen kullanıcının abonelik üzerinde katkıda bulunan ayrıcalıklara sahip olduğundan emin olun.

## <a name="common-app-discovery-errors"></a>Ortak uygulama bulma hataları

Azure geçişi, Azure geçişi: Sunucu değerlendirmesi kullanılarak uygulama, rol ve özellik bulmayı destekler. Uygulama bulma Şu anda yalnızca VMware için destekleniyor. Uygulama bulmayı ayarlamaya yönelik gereksinimler ve adımlar hakkında [daha fazla bilgi edinin](how-to-discover-applications.md) .

Tipik uygulama bulma hataları tabloda özetlenir. 

**Hata** | **Neden** | **Eylem**
--- | --- | ---
9000: VMware araç durumu algılanamıyor.     |   VMWare araçları yüklenmemiş veya bozuk olabilir.    |   VMware araçlarının VM 'de yüklü olduğundan ve çalıştığından emin olun.
9001: VMware araçları yüklü değil.     |   VMWare araçları yüklenmemiş veya bozuk olabilir.    |   VMware araçlarının VM 'de yüklü olduğundan ve çalıştığından emin olun.
9002: VMware araçları çalışmıyor.   |   VMWare araçları yüklenmemiş veya bozuk olabilir.    |   VMware araçlarının VM 'de yüklü olduğundan ve çalıştığından emin olun.
9003: işletim sistemi türü, Konuk VM keşfi için desteklenmiyor.    |   Sunucu üzerinde çalışan işletim sistemi Windows veya Linux değildir.    |   Desteklenen işletim sistemi türleri yalnızca Windows ve Linux. Sunucu gerçekten Windows veya Linux ise vCenter Server ' de belirtilen işletim sistemi türünü denetleyin.
9004: VM çalışmıyor.     |   VM kapalı.  |   VM 'nin açık olduğundan emin olun.
9005: işletim sistemi türü, Konuk VM keşfi için desteklenmiyor.    |   Konuk VM keşfi için işletim sistemi türü desteklenmiyor.     |   Desteklenen işletim sistemi türleri yalnızca Windows ve Linux.
9006: meta veri dosyasının konuğdan indirileceği URL boş.     |   Bu durum, bulma Aracısı beklendiği gibi çalışmıyorsa gerçekleşebilir.    |   Sorun otomatik olarak in24 saat çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
9007: Konuk VM 'de bulma görevini çalıştıran işlem bulunamadı.   |   Keşif Aracısı düzgün çalışmıyorsa bu durum oluşabilir.   |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
9008: Konuk VM işlem durumu alınamıyor.   |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
9009: Windows UAC, sunucuda bulma görevi yürütmeyi engelledi.  |   Sunucu üzerindeki Windows Kullanıcı hesabı denetimi (UAC) ayarları kısıtlayıcıdır ve yüklü uygulamaların keşfedilmesi önlenir.  |   Sunucudaki ' Kullanıcı hesabı denetimi ' ayarları ' nda, UAC ayarını alt iki düzeyden birinde olacak şekilde yapılandırın.
9010: VM kapalı.     |   VM kapalı.  |   VM 'nin açık olduğundan emin olun.
9011: bulunan meta veri dosyası Konuk VM dosya sisteminde bulunamadı.    |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
9012: bulunan meta veri dosyası boş.     |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
9013: her oturum açma işlemi için yeni bir geçici profil oluşturulur.    |   VMware VM 'de her oturum açma işlemi için yeni bir geçici profil oluşturulur.    |   Bir çözüm için Microsoft Desteği başvurun.
9014: Konuk VM dosya sisteminden meta veriler alınamıyor.     |   ESXi konağına bağlantı yok    |   Gerecin VM 'yi çalıştıran ESXi konağında 443 numaralı bağlantı noktasına bağlanabildiğinden emin olun
9015: vCenter Kullanıcı hesabında Konuk Işlemler rolü etkin değil   |   Konuk Işlemler rolü vCenter Kullanıcı hesabında etkin değil.   |   VCenter Kullanıcı hesabında Konuk Işlemleri rolünün etkinleştirildiğinden emin olun.
9016: Konuk işlem Aracısı güncel olmadığından bulunamıyor.   |   VMware araçları düzgün yüklenmemiş veya güncel değil.    |   VMware araçlarının düzgün bir şekilde yüklendiğinden ve güncel olduğundan emin olun.
9017: sanal makinede bulunan meta verileri içeren dosya bulunamadı.  |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Bir çözüm için Microsoft Desteği başvurun.
9018: PowerShell, Konuk VM 'lerde yüklü değil.  |   PowerShell, Konuk VM 'de kullanılamıyor.    |   PowerShell 'i Konuk VM 'ye yükler.
9019: Konuk VM işlem hatalarından dolayı bulunamıyor.     |   VMware Konuk işlemi VM 'de başarısız oldu.    |   VM kimlik bilgilerinin geçerli olduğundan ve konuk VM kimlik bilgilerinde belirtilen kullanıcı adının UPN biçiminde olduğundan emin olun.
9020: dosya oluşturma izni reddedildi.    |   Kullanıcı veya grup ilkesiyle ilişkili rol, kullanıcının klasörde dosya oluşturmasını kısıtlıyor    |   Belirtilen Konuk kullanıcının klasöründeki dosya için oluşturma iznine sahip olup olmadığını denetleyin. Klasör adı için bkz. Sunucu değerlendirmesi içindeki **Bildirimler** .
9021: sistem geçici yolunda dosya oluşturulamıyor.     |   VMware aracı, kullanıcılar geçici yolu yerine sistem geçici yolunu raporlar.    |   VMware aracı sürümünüzü 10287 ' dan (NGC/VI Istemci biçimi) yükseltin.
9022: WMI nesnesine erişim reddedildi.    |   Kullanıcı veya grup ilkesiyle ilişkili rol, kullanıcının WMI nesnesine erişimini kısıtlıyor.  |   Lütfen Microsoft Desteği başvurun.
9023: sistem kök ortam değişkeni değeri boş olduğundan PowerShell çalıştırılamıyor.    |   Konuk VM için SystemRoot ortam değişkeninin değeri boş.     |   Bir çözüm için Microsoft Desteği başvurun.
9024: GEÇICI ortam değişkeni değeri boş olduğu için bulunamıyor.    |   Konuk VM için GEÇICI ortam değişkeninin değeri boş.   |   Lütfen Microsoft Desteği başvurun.
9025: PowerShell, Konuk VM 'lerde bozulmuş.  |   PowerShell, Konuk VM 'de bozuk.    |   PowerShell 'i Konuk VM 'ye yeniden yükleyin ve PowerShell 'in Konuk VM 'de çalıştırılabildiğini doğrulayın.
9026: VM üzerinde Konuk işlemleri çalıştırılamıyor.  |   VM durumu, VM 'de Konuk işlemlerin çalıştırılmasına izin vermiyor.   |   Bir çözüm için Microsoft Desteği başvurun.
9027: Konuk işlem Aracısı VM 'de çalışmıyor.   |   Sanal makine içinde çalışan konuk işlem aracısıyla iletişim kurulamadı.    |   Bir çözüm için Microsoft Desteği başvurun.
9028: VM 'de yetersiz disk depolaması nedeniyle dosya oluşturulamıyor.     |   Diskte yeterli alan yok.   |   VM 'nin disk depolamada yeterli kullanılabilir alan olduğundan emin olun.
9029: belirtilen Konuk VM kimlik bilgilerinde PowerShell 'e erişim yok.   |   PowerShell 'e erişim kullanıcı için kullanılamaz.     |   Gereçte eklenen kullanıcının PowerShell 'e Konuk VM üzerinde erişebildiğinden emin olun.
9030: ESXi konağının bağlantısı kesildiğinden keşfedilen meta veriler toplanamıyor.     |   ESXi ana bilgisayarı bağlantısı kesik durumda.   |   VM 'yi çalıştıran ESXi konağının bağlı olduğundan emin olun.
9031: ESXi Konağı yanıt vermediğinden keşfedilen meta veriler toplanamıyor.   |   Uzak ana bilgisayar geçersiz durumda.    |   VM 'yi çalıştıran ESXi konağının çalıştığından ve bağlı olduğundan emin olun.
9032: bir iç hata nedeniyle bulunamıyor.   |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Bir çözüm için Microsoft Desteği başvurun.
9033: VM Kullanıcı adı geçersiz karakterler içerdiğinden bulunamıyor.     |   Kullanıcı adında geçersiz karakterler algılandı.   |   Geçersiz karakter olmadığından VM kimlik bilgisini yeniden sağlayın.
9034: belirtilen Kullanıcı adı UPN biçiminde değil.    |   Kullanıcı adı UPN biçiminde değil.  |   Kullanıcı adının Kullanıcı asıl adı (UPN) biçiminde olduğundan emin olun.
9035: PowerShell dil modu ' Full Language ' olarak ayarlı olmadığından bulunamıyor.  |   Konuk VM 'de PowerShell için dil modu tam dil olarak ayarlanmadı.   |   PowerShell dil modunun ' Full Language ' olarak ayarlandığından emin olun.
10000: işletim sistemi türü desteklenmiyor.   |   Sunucu üzerinde çalışan işletim sistemi Windows veya Linux değildir.    |   Desteklenen işletim sistemi türleri yalnızca Windows ve Linux.
10001: sunucu bulma betiği gereç üzerinde bulunamadı.    |   Bulma beklendiği gibi çalışmıyor.   |   Bir çözüm için Microsoft Desteği başvurun.
10002: bulma görevi zamanında tamamlanmadı.     |   Bulma Aracısı beklendiği gibi çalışmıyor.     |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse, Microsoft Desteği başvurun.
10003: bulma görevinin yürütüldüğü işleme bir hatayla çıkıldı.    |   Bulma görevinin yürütüldüğü işleme bir hatayla çıkıldı.  |   Sorun, 24 saat içinde otomatik olarak çözümlenmelidir. Sorun devam ederse lütfen Microsoft Desteği başvurun.
10004: Konuk işletim sistemi türü için kimlik bilgisi sağlanmadı.  |   Bu işletim sistemi türündeki makinelere erişim kimlik bilgileri, Azure geçiş gereci 'nda sağlanmadı.    |   Gereç üzerindeki makineler için kimlik bilgileri ekleme
10005: girilen kimlik bilgileri geçerli değil.   |   Gereç için sunucu erişimi için belirtilen kimlik bilgileri yanlış.  |   Gereçte sunulan kimlik bilgilerini güncelleştirin ve kimlik bilgilerini kullanarak sunucuya erişilebilir olduğundan emin olun.
10006: Konuk işletim sistemi türü kimlik bilgisi deposu tarafından desteklenmiyor.  |   Sunucu üzerinde çalışan işletim sistemi Windows veya Linux değildir.    |   Desteklenen işletim sistemi türleri yalnızca Windows ve Linux.
10007: keşfedilen meta veriler işlenemiyor.    |   JSON seri durumdan çıkarılmaya çalışılırken hata oluştu.    |   Bir çözüm için Microsoft Desteği başvurun.
10008: sunucuda bir dosya oluşturulamıyor.    |  Sorun bir iç hata nedeniyle oluşabilir.    |   Bir çözüm için Microsoft Desteği başvurun.
10009: keşfedilen meta veriler sunucudaki bir dosyaya yazılamıyor.  |   Sorun, bir iç hata nedeniyle oluşabilir.   |   Bir çözüm için Microsoft Desteği başvurun.




## <a name="next-steps"></a>Sonraki adımlar
[VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md)veya [fiziksel sunucular](how-to-set-up-appliance-physical.md)için bir gereç ayarlayın.
