---
title: Sorun giderme Azure Geçiş cihazı dağıtımı ve keşfi
description: Azure Geçiş cihazını dağıtma ve makineleri keşfetme konusunda yardım alın.
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 6cb83a87f2e96eb62696e5d92095ef2b8d7c7def
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677334"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Azure Geçiş cihazını ve keşfini sorun giderme

Bu makale, [Azure Geçiş](migrate-services-overview.md) cihazını dağıtırken ve cihazda şirket içi makineleri keşfetmek için kullanırken sorunları gidermenize yardımcı olur.


## <a name="whats-supported"></a>Neler desteklenir?

Cihaz destek gereksinimlerini [gözden geçirin.](migrate-appliance.md)


## <a name="invalid-ovf-manifest-entry"></a>"Geçersiz OVF manifesto girişi"

"Sağlanan bildirim dosyası geçersizdir: Geçersiz OVF bildirim girişi" hatasını alırsanız, aşağıdakileri yapın:

1. Azure Geçiş cihazı OVA dosyasının karma değerini kontrol ederek doğru indirilmesini doğrulayın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Karma değeri eşleşmiyorsa, OVA dosyasını yeniden indirin ve dağıtımı yeniden deneyin.
2. Dağıtım hala başarısız sayılsa ve OVF dosyasını dağıtmak için VMware vSphere istemcisini kullanıyorsanız, vSphere web istemcisi üzerinden dağıtmayı deneyin. Dağıtım hala başarısız olursa, farklı bir web tarayıcısı kullanmayı deneyin.
3. vSphere web istemcisini kullanıyorsanız ve vCenter Server 6.5 veya 6.7'de dağıtmaya çalışıyorsanız, OVA'yı doğrudan ESXi ana bilgisayara dağıtmayı deneyin:
   - WEB istemcisi (https://<*ana bilgisayar IP Adresi*>/ui) ile doğrudan ESXi ana bilgisayara (vCenter Server yerine) bağlanın.
   -  > Ana **Stok'ta,** **Dosya** > **Dağıt OVF şablonunu**seçin.**Inventory** OVA'ya göz atın ve dağıtımı tamamlayın.
4. Dağıtım hala başarısız olursa, Azure Geçiş desteğine başvurun.

## <a name="cant-connect-to-the-internet"></a>Internet'e bağlanamıyorum

Cihaz makinesi bir proxy'nin arkasındaysa bu durum olabilir.

- Proxy'nin ihtiyacı varsa yetki kimlik bilgilerini sağladığından emin olun.
- Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy'si kullanıyorsanız, [bu URL'leri](migrate-appliance.md#url-access) izin verenler listesine ekleyin.
- Internet'e bağlanmak için bir engelleme proxy kullanıyorsanız, [bu adımları](https://docs.microsoft.com/azure/migrate/concepts-collector)kullanarak cihaz VM üzerine proxy sertifikası almak.

## <a name="cant-sign-into-azure-from-the-appliance-web-app"></a>Cihaz web uygulamasından Azure'da oturum açamıyorum

Azure'da oturum açtırmak için yanlış Azure hesabını kullanıyorsanız , "Üzgünüz, ancak oturum açmanızda sorun yaşıyoruz" hatası görüntülenir. Bu hata birkaç nedenden dolayı oluşur:

- Genel bulut için cihaz web uygulamasında oturum açarsanız, Devlet bulut portalı için kullanıcı hesabı kimlik bilgilerini kullanırsınız.
- Özel bulut portalı için kullanıcı hesabı kimlik bilgilerini kullanarak devlet bulutu için cihaz web uygulamasında oturum açarsanız.

Doğru kimlik bilgilerini kullandığınızdan emin olun.

##  <a name="datetime-synchronization-error"></a>Tarih/saat eşitleme hatası

Tarih ve saat eşitleme (802) ile ilgili bir hata, sunucu saatinin geçerli saatle eşitlemenin beş dakikadan fazla olabileceğini gösterir. Toplayıcı VM'deki saat saatini geçerli saatle eşleşecek şekilde değiştirin:

1. VM'de bir yönetici komut istemi açın.
2. Saat dilimini kontrol etmek için **w32tm /tz'yi**çalıştırın.
3. Zamanı eşitlemek için **w32tm /resync çalıştırın.**


## <a name="unabletoconnecttoserver"></a>"Unabletoconnecttoserver"

Bu bağlantı hatası alırsanız, vCenter *Server Servername*.com:9443'e bağlanamayabilirsiniz. Hata ayrıntıları, iletiyi kabul edebilecek `https://\*servername*.com:9443/sdk` bir bitiş noktası olmadığını gösterir.

- Cihazın en son sürümünü çalıştırıp çalıştırmadığınızı kontrol edin. Eğer değilseniz, cihazı [en son sürüme](https://docs.microsoft.com/azure/migrate/concepts-collector)yükseltin.
- Sorun hala en son sürümde oluşuyorsa, cihaz belirtilen vCenter Server adını çözemeyebilir veya belirtilen bağlantı noktası yanlış olabilir. Varsayılan olarak, bağlantı noktası belirtilmemişse, toplayıcı 443 numaralı bağlantı noktasına bağlanmaya çalışır.

    1. Cihazdan Ping *Servername*.com.
    2. Adım 1 başarısız olursa, IP adresini kullanarak vCenter sunucusuna bağlanmayı deneyin.
    3. vCenter Server'a bağlanmak için doğru bağlantı noktası numarasını belirleyin.
    4. vCenter Server'ın çalışır durumda olduğunu doğrulayın.


## <a name="error-6005260039-appliance-might-not-be-registered"></a>Hata 60052/60039: Cihaz kayıtlı olmayabilir

- 60052 hata, "Cihaz Azure Geçiş projesine başarıyla kaydedilmemiş olabilir" hata, cihazı kaydetmek için kullanılan Azure hesabının yeterli izine sahip olmaması durumunda oluşur.
    - Cihazı kaydetmek için kullanılan Azure kullanıcı hesabının abonelikte en az Katılımcı izinlerine sahip olduğundan emin olun.
    - Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)
- Hata 60039, "Cihaz Azure Geçir projesine başarılı bir şekilde kaydedilemeyebilir" kaydı başarısız olursa, kayıt için kullanılan Azure Geçiş projesi bulunamazsa oluşabilir.
    - Azure portalında projenin kaynak grubunda bulunup bulunmadığından kontrol edin.
    - Proje yoksa, kaynak grubunuzda yeni bir Azure Geçiş projesi oluşturun ve cihazı yeniden kaydedin. Yeni bir proje [oluşturmayı öğrenin.](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool)

## <a name="error-6003060031-key-vault-management-operation-failed"></a>Hata 60030/60031: Key Vault yönetim işlemi başarısız oldu

60030 veya 60031 hatasını alırsanız, "Bir Azure Anahtar Kasası yönetimi işlemi başarısız oldu", aşağıdakileri yapın:
- Cihazı kaydetmek için kullanılan Azure kullanıcı hesabının abonelikte en az Katılımcı izinlerine sahip olduğundan emin olun.
- Hesabın hata iletisinde belirtilen anahtar kasasına erişebilmesini sağla ve ardından işlemi yeniden deneyin.
- Sorun devam ederse, Microsoft desteğine başvurun.
- Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Hata 60028: Keşif başlatılamadı

Hata 60028: "Bulma bir hata nedeniyle başlatılamadı. Belirtilen ana bilgisayar veya kümeler listesi için başarısız olan işlem" vm bilgilerine erişim veya alma da bir sorun nedeniyle hatada listelenen ana bilgisayarlarda keşfin başlatılamayacağını gösterir. Diğer ev sahipleri başarıyla eklendi.

- **Ana bilgisayar** ekle seçeneğini kullanarak hatada listelenen ana bilgisayarları yeniden ekleyin.
- Doğrulama hatası varsa, hataları düzeltmek için düzeltme kılavuzunu gözden geçirin ve ardından **Kaydet ve yeniden bulma seçeneğini deneyin.**

## <a name="error-60025-azure-ad-operation-failed"></a>Hata 60025: Azure AD işlemi başarısız oldu 
Hata 60025: "Bir Azure REKLAM işlemi başarısız oldu. Azure AD uygulamasını oluştururken veya güncellerken meydana gelen hata, keşfi başlatmak için kullanılan Azure kullanıcı hesabının cihazı kaydetmek için kullanılan hesaptan farklı olması yla oluşur. Aşağıdakilerden birini yapın:

- Keşfi başlatan kullanıcı hesabının cihazı kaydetmek için kullanılan hesapla aynı olduğundan emin olun.
- Bulma işleminin başarısız olduğu kullanıcı hesabına Azure Active Directory uygulama erişim izinleri sağlayın.
- Azure Geçiş projesi için daha önce oluşturulan kaynak grubunu silin. Yeniden başlamak için başka bir kaynak grubu oluşturun.
- Azure Active Directory uygulama izinleri hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware)


## <a name="error-50004-cant-connect-to-host-or-cluster"></a>Hata 50004: Ana bilgisayara veya kümeye bağlanamıyorum

Hata 50004: "Sunucu adı çözülemediği için ana bilgisayara veya kümeye bağlanamıyorum. WinRM hata kodu: 0x803381B9" cihaz için Azure DNS hizmeti sağladığınız küme veya ana bilgisayar adını çözemezse oluşabilir.

- Kümede bu hatayı görürseniz, FQDN kümesi.
- Kümedeki ana bilgisayarlar için de bu hatayı görebilirsiniz. Bu, cihazın kümeye bağlanabileceğini gösterir, ancak küme FQDN olmayan ana bilgisayar adlarını döndürür. Bu hatayı gidermek için, IP adresi ve ana bilgisayar adlarının eşlemini ekleyerek cihazdaki ana bilgisayar dosyasını güncelleştirin:
    1. Not Defteri'ni yönetici olarak açın.
    2. C:\Windows\System32\Drivers\etc\hosts dosyasını açın.
    3. IP adresini ve ana bilgisayar adını art arda ekleyin. Bu hatayı gördüğünüz her ana bilgisayar veya küme için yineleyin.
    4. Hosts dosyasını kaydedin ve kapatın.
    5. Cihaz yönetimi uygulamasını kullanarak cihazın ana bilgisayarlara bağlanıp bağlanamadığını kontrol edin. 30 dakika sonra, bu ana bilgisayarlar için en son bilgileri Azure portalında görmeniz gerekir.

## <a name="discovered-vms-not-in-portal"></a>Portalda bulunan VM'ler

Bulma durumu "Bulma devam ediyorsa" ancak portaldaki VM'leri henüz göremiyorsa, birkaç dakika bekleyin:
- Bir VMware VM için yaklaşık 15 dakika sürer.
- Hyper-V VM keşfi için eklenen her ana bilgisayar için yaklaşık iki dakika sürer.

Beklerseniz ve durum değişmezse, **Sunucular** sekmesinde **Yenile'yi** seçin. Bu, Azure Geçir: Sunucu Değerlendirmesi ve Azure Geçişi: Sunucu Geçişi'nde keşfedilen sunucuların sayısını göstermelidir.

Bu işe yaramazsa ve VMware sunucularını keşfediyorsanız:

- Belirttiğiniz vCenter hesabının en az bir VM'ye erişim sağlayan izinlerin doğru ayarladığını doğrulayın.
- VCenter hesabı vCenter VM klasör düzeyinde verilen erişime sahipse Azure Geçir VMware VM'leri keşfedemez. Keşif leri kapsama alma hakkında [daha fazla bilgi edinin.](set-discovery-scope.md)

## <a name="vm-data-not-in-portal"></a>VM verileri portalda yok

Keşfedilen VM'ler portalda görünmüyorsa veya VM verileri güncel değilse, birkaç dakika bekleyin. Keşfedilen VM yapılandırma verilerindeki değişikliklerin portalda görünmesi 30 dakika kadar sürer. Uygulama verilerindeki değişikliklerin görünmesi birkaç saat sürebilir. Bu süreden sonra veri yoksa, aşağıdaki gibi yenilemeyi deneyin

1. **Sunucularda** > **Azure Sunucu Değerlendirmesi Geçir**, Genel **Bakış**seçin.
2. **Yönet**altında, **Ajan Sağlık'ı**seçin.
3. **Aracıyı Yenile'yi**seçin.
4. Yenileme işleminin tamamlanmasını bekleyin. Artık güncel bilgileri görmeniz gerekir.

## <a name="deleted-vms-appear-in-portal"></a>Silinen VM'ler portalda görünür

VM'leri silerseniz ve hala portalda görünürlerse, 30 dakika bekleyin. Hala görünüyorlarsa, yukarıda açıklandığı gibi yenileyin.

## <a name="common-app-discovery-errors"></a>Yaygın uygulama bulma hataları

Azure Geçiş, Azure Geçiş: Sunucu Değerlendirmesi'ni kullanarak uygulamaların, rollerin ve özelliklerin bulunmasını destekler. Uygulama bulma şu anda yalnızca VMware için desteklenir. Uygulama bulma ayarlama gereksinimleri ve adımları hakkında [daha fazla bilgi edinin.](how-to-discover-applications.md)

Tipik uygulama bulma hataları tabloda özetlenir. 

**Hata** | **Neden** | **Eylem**
--- | --- | --- | ---
10000: "Sunucuda yüklü uygulamaları keşfedemiyoruz". | Makine işletim sistemi Windows veya Linux değilse bu oluşabilir. | Yalnızca Windows/Linux için uygulama bulma yı kullanın.
10001: "Sunucu yüklü uygulamaları almak mümkün değil". | İç hata - cihazbazı eksik dosyalar. | Microsoft Desteği'ne başvurun.
10002: "Sunucu yüklü uygulamaları almak mümkün değil". | Cihazın üzerindeki keşif aracısı düzgün çalışmıyor olabilir. | Sorun 24 saat içinde kendiliğinden çözülmezse desteğe başvurun.
10003 "Sunucu yüklü uygulamalar alınamıyor". | Cihazın üzerindeki keşif aracısı düzgün çalışmıyor olabilir. | Sorun 24 saat içinde kendiliğinden çözülmezse desteğe başvurun.
10004: "Windows/Linux <makineleri için yüklü uygulamaları keşfedemiyoruz>." |  Cihazda Windows/Linux> makinelerine <erişim kimlik bilgileri verilmedi.| Windows/Linux> makinelerine <erişimi olan cihaza bir kimlik bilgisi ekleyin.
10005: "Şirket içi sunucuya erişilemiyor". | Erişim kimlik bilgileri yanlış olabilir. | Cihaz kimlik bilgilerini güncelleyin, ilgili makineye onlarla birlikte erişebildiğinizden emin olun. 
10006: "Şirket içi sunucuya erişilemiyor". | Makine işletim sistemi Windows veya Linux değilse bu oluşabilir.|  Yalnızca Windows/Linux için uygulama bulma yı kullanın.
10007: "Alınan meta verileri işleyemiyor" | Bu iç hata JSON deserialize çalışırken oluştu | Çözüm için Microsoft Destek'e başvurun
9000/9001/9002: "Sunucuda yüklü uygulamaları keşfedemiyoruz". | VMware araçları yüklenmemiş veya bozuk olabilir. | VMware araçlarını ilgili makineye yükleyin/yeniden yükleyin ve çalıştığını kontrol edin.
9.003: Sunucuda yüklü uygulamaları bulunamaz". | Makine işletim sistemi Windows veya Linux değilse bu oluşabilir. | Yalnızca Windows/Linux için uygulama bulma yı kullanın.
9004: "Sunucuda yüklü uygulamaları keşfedemiyoruz". | VM kapalıysa bu durum olabilir. | Keşif için VM'nin anın a...................................
9.005: "VM yüklü uygulamaları keşfedemiyoruz. | Makine işletim sistemi Windows veya Linux değilse bu oluşabilir. | Yalnızca Windows/Linux için uygulama bulma yı kullanın.
9006/9007: "Sunucu yüklü uygulamaları alamadım". | Cihazın üzerindeki keşif aracısı düzgün çalışmıyor olabilir. | Sorun 24 saat içinde kendiliğinden çözülmezse desteğe başvurun.
9008: "Sunucu yüklü uygulamaları almak mümkün değil". | İç hata olabilir.  | Tf sorunu 24 saat içinde kendi kendine çözülmüyor, desteğe başvurun.
9009: "Sunucu yüklü uygulamaları almak mümkün değil". | Sunucudaki Windows Kullanıcı Hesabı Denetimi (UAC) ayarları kısıtlayıcıysa ve yüklü uygulamaların bulunmasını engelliyorsa oluşabilir. | Sunucuda 'Kullanıcı Hesabı Denetimi' ayarlarını arayın ve sunucudaki UAC ayarını alt iki düzeyden birine yapınız.
9010: "Sunucu yüklü uygulamaları almak mümkün değil". | İç hata olabilir.  | Tf sorunu 24 saat içinde kendi kendine çözülmüyor, desteğe başvurun.
9011: "Konuk VM'de misafirden indirilen dosya bulunamadı" | Sorun bir iç hata nedeniyle oluşabilir. | Sorun otomatik olarak 24 saat içinde çözülmelidir. Sorun devam ediyorsa, lütfen Microsoft Destek'e başvurun.
9012: "Sonuç dosyası içeriği boş." | Sorun bir iç hata nedeniyle oluşabilir. | Sorun otomatik olarak 24 saat içinde çözülmelidir. Sorun devam ediyorsa, lütfen Microsoft Destek'e başvurun.
9013: "VMware VM'ye her giriş için yeni bir geçici profil oluşturulur" | VM'ye her giriş için yeni bir geçici profil oluşturulur | Konuk VM kimlik bilgilerinde sağlanan kullanıcı adının UPN formatında olduğundan emin olun.
9015: "VCenter'daki ayrıcalıkların yetersizliği nedeniyle VMware VM'lere bağlanılamıyor" | konuk İşlemleri rolü vCenter kullanıcı hesabında etkinleştirildi | VCenter kullanıcı hesabında Konuk İşlemleri rolünün etkinleştirildiğinden emin olun.
9016: "Konuk operasyon aracısı veri dışında olduğu için VMware VM'lere bağlanılamıyor" | VMware araçları düzgün yüklü değildir veya güncel değildir. | VMware araçlarının düzgün şekilde yüklendiğinden ve güncel olduğundan emin olun.
9017: "VM'de keşfedilen meta verilerle dosya bulunamadı." | Sorun bir iç hata nedeniyle oluşabilir. | Çözüm için Microsoft Destek'e başvurun.
9018: "PowerShell Konuk VM'lerde yüklü değil." | PowerShell konuk VM'de kullanılamaz. | PowerShell'i konuk VM'ye yükleyin.
9019: "Konuk VM operasyon arızaları nedeniyle keşfedileme" | VM'de VM'de konuk işlemi başarısız oldu. | VM kimlik bilgilerinin geçerli olduğundan ve konuk VM kimlik bilgilerinde sağlanan kullanıcı adının UPN biçiminde olduğundan emin olun.
9020: "Dosya oluşturma izni reddedildi." | Kullanıcı veya grup ilkesiyle ilişkili rol, kullanıcının dosyayı klasörde oluşturmasını kısıtlıyor | Sağlanan konuk kullanıcının klasördeki dosya için izin oluşturmuş olup olmadığını denetleyin. Klasörün adı için Sunucu Değerlendirmesi'ndeki **Bildirimler'e** bakın.
9021: "Dosya oluşturma izni klasörÜ Sistem Temp Path reddedildi." | VM'deki VMware araç sürümü desteklenmez | VMware araç sürümünüzü 10.2.0'ın üzerine yükseltin.
9022: "WMI nesne erişimi alın reddedildi." | Kullanıcı veya grup ilkesiyle ilişkili rol, kullanıcının WMI nesnesine erişmesini kısıtlamaktır. | Lütfen Microsoft Destek'e başvurun.
9023: "SystemRoot ortamı değişken değeri boş." | Bilinmiyor | Lütfen Microsoft Destek'e başvurun.
9024: "TEMP ortamı değişken değeri boş." | Bilinmiyor | Lütfen Microsoft Destek'e başvurun.
9025: "PowerShell Konuk VM'lerde bozuk." | Bilinmiyor | PowerShell'i konuk VM'de yeniden yükleyin ve PowerShell'in konuk VM'de çalıştırılap çalıştırılamayacağından kontrol edin.
8084: "VMware hatası nedeniyle uygulamaları bulunamıyor: <Exception from VMware>" | Azure Geçir cihazı uygulamaları keşfetmek için VMware API'lerini kullanır. Uygulamaları keşfetmeye çalışırken vCenter Server tarafından bir özel durum atılırsa bu sorun ortaya çıkabilir. VMware'den gelen hata iletisi portalda gösterilen hata iletisinde görüntülenir. | [VMware belgelerindeki](https://pubs.vmware.com/vsphere-51/topic/com.vmware.wssdk.apiref.doc/index-faults.html)iletiyi arayın ve düzeltecek adımları izleyin. Düzeltiremiyorsanız, Microsoft desteğine başvurun.



## <a name="next-steps"></a>Sonraki adımlar
[VMware,](how-to-set-up-appliance-vmware.md) [Hyper-V](how-to-set-up-appliance-hyper-v.md)veya [fiziksel sunucular](how-to-set-up-appliance-physical.md)için bir cihaz ayarlayın.
