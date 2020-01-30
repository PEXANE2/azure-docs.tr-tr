---
title: Azure geçişi gereç dağıtımı ve bulma sorunlarını giderme
description: Azure geçişi gereci dağıtma ve makineler bulma konusunda yardım alın.
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 3f3604205d4aedffdda128ec4a6b895786245e56
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772027"
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
   - **Giriş** > **envanterinde** **Dosya** > **ovf şablonu dağıt**' ı seçin. OVA 'ya gidin ve dağıtımı doldurun.
4. Dağıtım hala başarısız olursa Azure geçiş desteği 'ne başvurun.

## <a name="cant-connect-to-the-internet"></a>İnternet 'e bağlanılamıyor

Gereç makinesi bir proxy 'nin arkasındaysa bu durum oluşabilir.

- Proxy 'ye ihtiyaç duyuyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
- Giden bağlantıyı denetlemek için URL tabanlı bir güvenlik duvarı proxy 'si kullanıyorsanız, bu URL 'Leri izin verilenler listesine ekleyin:

    - [VMware değerlendirmesi URL 'Leri](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [Hyper-V değerlendirmesi için URL 'Ler](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v)
    - [VMware aracısız geçişinin URL 'Leri](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [VMware Aracısı tabanlı geçiş için URL 'ler](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware)
    - [Hyper-V geçişi için URL 'Ler](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v)

- İnternet 'e bağlanmak için bir kesintiye uğratan ara sunucu kullanıyorsanız, [Bu adımları](https://docs.microsoft.com/azure/migrate/concepts-collector)kullanarak proxy SERTIFIKASıNı gereç sanal makinesine aktarın.

##  <a name="datetime-synchronization-error"></a>Tarih/saat eşitleme hatası

Tarih ve saat eşitleme (802) ile ilgili bir hata, sunucu saatinin beş dakikadan uzun bir süredir geçerli zamandan eşitlenmemiş olabileceğini gösterir. Toplayıcı VM 'sinin saat saatini Şu anki saatle eşleşecek şekilde değiştirin:

1. VM 'de bir yönetici komut istemi açın.
2. Saat dilimini denetlemek için **w32tm/tz komutunu**çalıştırın.
3. Saati eşitleme için **w32tm/resync**komutunu çalıştırın.


## <a name="unabletoconnecttoserver"></a>"UnableToConnectToServer"

Bu bağlantı hatası alırsanız, vCenter Server *ServerName*. com: 9443 öğesine bağlanamadıysanız. Hata ayrıntıları, https://*ServerName*. com: 9443/SDK ' da iletiyi kabul edebilecek bir uç nokta dinleme olmadığını gösterir.

- En son gereç sürümünü çalıştırıp çalıştırmadığını denetleyin. Değilseniz, gereci [en son sürüme](https://docs.microsoft.com/azure/migrate/concepts-collector)yükseltin.
- Sorun hala en son sürümde gerçekleşirse, Gereç belirtilen vCenter Server adını çözemeyebilir veya belirtilen bağlantı noktası yanlış olabilir. Varsayılan olarak, bağlantı noktası belirtilmemişse, toplayıcı 443 numaralı bağlantı noktası numarasına bağlanmayı dener.

    1. Gereci *ServerName*. com ' a ping yapın.
    2. 1\. adım başarısız olursa, IP adresini kullanarak vCenter Server 'a bağlanmayı deneyin.
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
- Sorun devam ederse, Microsoft desteği 'ne başvurun.
- Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance---vmware) .

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Hata 60028: bulma başlatılamadı

Hata 60028: "bir hata nedeniyle bulma başlatılamadı. Belirtilen konaklar veya kümeler listesi için işlem başarısız oldu "VM bilgilerine erişirken veya alırken bir sorun nedeniyle hatada listelenen konaklarda bulma işleminin başlatılamayacağını gösterir. Ana bilgisayarların geri kalanı başarıyla eklendi.

- Hatada listelenen Konakları, **ana bilgisayar Ekle** seçeneğini kullanarak yeniden ekleyin.
- Doğrulama hatası varsa, hataları onarmak için düzeltme kılavuzunu gözden geçirin ve sonra **bulmayı Kaydet ve Başlat** seçeneğini tekrar deneyin.

## <a name="error-60025-azure-ad-operation-failed"></a>Hata 60025: Azure AD işlemi başarısız oldu 
Hata 60025: "Azure AD işlemi başarısız oldu. Azure AD uygulaması oluşturulurken veya güncelleştirilirken oluşan hata oluştu "bulmayı başlatmak için kullanılan Azure Kullanıcı hesabı gereci kaydetmek için kullanılan hesaptan farklı olduğunda gerçekleşir. Aşağıdakilerden birini yapın:

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
- VCenter hesabının vCenter VM klasör düzeyinde erişimi varsa Azure geçişi, VMware VM 'lerini bulamıyor. Kapsam bulma hakkında [daha fazla bilgi edinin](tutorial-assess-vmware.md#set-the-scope-of-discovery) .

## <a name="vm-data-not-in-portal"></a>VM verileri portalda yok

Bulunan VM 'Ler portalda görünmezse veya VM verileri güncel değilse, birkaç dakika bekleyin. Keşfedilen VM yapılandırma verilerinde yapılan değişikliklerin portalda görünmesi 30 dakika kadar sürer. Uygulama verilerinde değişikliklerin görünmesi birkaç saat sürebilir. Bu süreden sonra veri yoksa yenilemeyi şu şekilde deneyin

1. **Azure sunucu değerlendirmesini geçir** > **sunucularında** **genel bakış**' ı seçin.
2. **Yönet**altında **Aracı durumu**' yi seçin.
3. **Aracıyı Yenile**' yi seçin.
4. Yenileme işleminin tamamlanmasını bekleyin. Şimdi güncel bilgileri görmeniz gerekir.

## <a name="deleted-vms-appear-in-portal"></a>Portalda silinen VM 'Ler görüntülenir

VM 'Leri silerseniz ve portalda hala görünüyorsa 30 dakika bekleyin. Hala görünüyorsa, yukarıda açıklandığı gibi yenileyin.

## <a name="common-app-discovery-errors"></a>Ortak uygulama bulma hataları

Azure geçişi, Azure geçişi: Sunucu değerlendirmesi kullanılarak uygulama, rol ve özellik bulmayı destekler. Uygulama bulma Şu anda yalnızca VMware için destekleniyor. Uygulama bulmayı ayarlamaya yönelik gereksinimler ve adımlar hakkında [daha fazla bilgi edinin](how-to-discover-applications.md) .

Tipik uygulama bulma hataları tabloda özetlenir.

**Hata:** | **Neden** | **Eylem**
--- | --- | --- | ---
10000: "sunucuda yüklü olan uygulamalar bulunamıyor". | Makine işletim sistemi Windows veya Linux değilse bu durum oluşabilir. | Yalnızca Windows/Linux için uygulama bulmayı kullanın.
10001: "sunucu yüklü uygulamalar alınamıyor". | İç hata-gereç içindeki bazı dosyalar eksik. | Microsoft Desteği'ne başvurun.
10002: "sunucu yüklü uygulamalar alınamıyor". | Gereç üzerindeki bulma Aracısı düzgün çalışmıyor olabilir. | Sorun, 24 saat içinde kendiliğinden çözümlenmezse desteğe başvurun.
10003 "sunucu yüklü uygulamalar alınamıyor". | Gereç üzerindeki bulma Aracısı düzgün çalışmıyor olabilir. | Sorun, 24 saat içinde kendiliğinden çözümlenmezse desteğe başvurun.
10004: "< Windows/Linux > makineler için yüklü uygulamalar bulunamıyor." |  < Windows/Linux > makinelere erişim kimlik bilgileri, Gereç içinde sağlanmamış.| < Windows/Linux > makinelerine erişimi olan gereç için bir kimlik bilgisi ekleyin.
10005: "Şirket içi sunucuya erişilemiyor". | Erişim kimlik bilgileri yanlış olabilir. | Gereç kimlik bilgilerini güncelleştirme ilgili makineye bunlarla erişebildiğinizden emin olun. 
10006: "Şirket içi sunucuya erişilemiyor". | Makine işletim sistemi Windows veya Linux değilse bu durum oluşabilir.|  Yalnızca Windows/Linux için uygulama bulmayı kullanın.
9000/9001/9002: "sunucuda yüklü olan uygulamalar bulunamıyor". | VMware araçları yüklenmemiş veya bozuk olabilir. | VMware araçlarını ilgili makineye yükleyin/yeniden yükleyin ve çalıştığını denetleyin.
9003: sunucuda yüklü olan uygulamalar bulunamıyor ". | Makine işletim sistemi Windows veya Linux değilse bu durum oluşabilir. | Yalnızca Windows/Linux için uygulama bulmayı kullanın.
9004: "sunucuda yüklü olan uygulamalar bulunamıyor". | VM kapatılmışsa bu durum oluşabilir. | Bulma için VM 'nin açık olduğundan emin olun.
9005: "sanal makinede yüklü olan uygulamalar bulunamıyor. | Makine işletim sistemi Windows veya Linux değilse bu durum oluşabilir. | Yalnızca Windows/Linux için uygulama bulmayı kullanın.
9006/9007: "sunucu yüklü uygulamalar alınamıyor". | Gereç üzerindeki bulma Aracısı düzgün çalışmıyor olabilir. | Sorun, 24 saat içinde kendiliğinden çözümlenmezse desteğe başvurun.
9008: "sunucu yüklü uygulamalar alınamıyor". | Bir iç hata olabilir.  | TF sorun, 24 saat içinde kendiliğinden çözümlenmiyor, desteğe başvurun.
9009: "sunucu yüklü uygulamalar alınamıyor". | Sunucu üzerindeki Windows Kullanıcı hesabı denetimi (UAC) ayarları kısıtlayıcıysa ve yüklü uygulamaların bulunmasını engelliyorsa, bu durum oluşabilir. | Sunucuda ' Kullanıcı hesabı denetimi ' ayarlarını arayın ve sunucudaki UAC ayarını alt iki düzeyden birine yapılandırın.
9010: "sunucu yüklü uygulamalar alınamıyor". | Bir iç hata olabilir.  | TF sorun, 24 saat içinde kendiliğinden çözümlenmiyor, desteğe başvurun.
8084: "VMware hatası nedeniyle uygulamalar bulunamıyor: <Exception from VMware>" | Azure geçişi gereci, uygulamaları bulmaya yönelik VMware API 'Lerini kullanır. Bu sorun, uygulamalar bulunmaya çalışılırken vCenter Server tarafından bir özel durum oluşturulursa meydana gelebilir. VMware 'den hata iletisi, portalda gösterilen hata iletisinde görüntülenir. | [VMware belgelerindeki](https://pubs.vmware.com/vsphere-51/topic/com.vmware.wssdk.apiref.doc/index-faults.html)iletiyi arayın ve onarmak için adımları izleyin. Bu hatayı düzeltemedi, Microsoft destek 'e başvurun.
9012: "sunucuda yüklü uygulamalar bulunamadı" | Sorun, bir iç hata nedeniyle oluşabilir.  | Sorun, 24 saat içinde kendiliğinden çözümlenmezse desteğe başvurun.
9013: "sunucuda yüklü uygulamalar bulunamadı" | Sanal makinede her oturum açtığınızda yeni bir geçici profil oluşturulur.  | Belirtilen Konuk Kullanıcı için geçici bir profilin oluşturulmadığından emin olun.



## <a name="next-steps"></a>Sonraki adımlar
[VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md)veya [fiziksel sunucular](how-to-set-up-appliance-physical.md)için bir gereç ayarlayın.
