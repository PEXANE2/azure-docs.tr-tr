---
title: Azure geçişi sorunlarını giderme | Microsoft Docs
description: Azure geçişi hizmetindeki bilinen sorunlara genel bir bakış ve sık karşılaşılan hatalara yönelik sorun giderme ipuçları sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: raynew
ms.openlocfilehash: fa1e7fcf89ccc06e429831191ba5dfce3cf33797
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828322"
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

[Azure geçişi](migrate-services-overview.md) , değerlendirme ve geçiş için bir araç merkezi ve üçüncü taraf bağımsız yazılım SATıCıSı (ISV) teklifleri sunar. Bu makale, Azure geçişi, Azure geçişi sunucu değerlendirmesi ve Azure geçişi sunucu geçişi ile ilgili hataları gidermenize yardımcı olur.

## <a name="azure-migrate-project-issues"></a>Azure geçişi proje sorunları

### <a name="i-cant-find-my-existing-azure-migrate-project"></a>Mevcut Azure geçiş projem 'ı bulamıyorum.

Azure geçişi 'nin [iki sürümü](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) vardır. Projeyi oluşturduğunuz sürüme bağlı olarak, aşağıdaki yöntemlerden birini kullanın:

* Azure geçişi 'nin önceki sürümü (eski deneyim) ile oluşturulmuş bir proje arıyorsanız, projeyi bulmak için şu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com)gidin ve **Azure geçişi**' ni arayın.
    2. Azure geçişi panosunda, eski projelere erişen bir başlık görürsünüz. Bu başlığı yalnızca eski deneyimle bir proje oluşturduysanız görürsünüz. Başlığı seçin.

       ![Mevcut projelere erişin](./media/troubleshooting-general/access-existing-projects.png)

    3. Şimdi Azure geçişi 'nin önceki sürümüyle oluşturulmuş mevcut projelerin listesini görürsünüz.

* Geçerli sürümle (yeni deneyim) oluşturulmuş bir proje arıyorsanız, projeyi bulmak için şu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com)gidin ve **Azure geçişi**' ni arayın.
    2. Azure geçişi panosu ' nda sol bölmedeki **sunucular** sayfasına gidin ve sağ üst köşedeki **Değiştir** ' i seçin.

       ![Mevcut bir Azure geçişi projesine geç](./media/troubleshooting-general/switch-project.png)

    3. Uygun aboneliği ve Azure geçişi projesini seçin.

### <a name="how-do-i-create-a-second-azure-migrate-project"></a>İkinci bir Azure geçişi projesi oluşturmak Nasıl yaparım??

Yeni bir Azure geçişi projesi oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)gidin ve **Azure geçişi**' ni arayın.
2. Azure geçişi panosu ' nda sol bölmedeki **sunucular** sayfasına gidin ve sağ üst köşedeki **Değiştir** ' i seçin.

   ![Azure geçişi projesini değiştirme](./media/troubleshooting-general/switch-project.png)

3. Yeni bir proje oluşturmak için **buraya tıklayın ' ı**seçin.

   ![İkinci bir Azure geçişi projesi oluşturma](./media/troubleshooting-general/create-new-project.png)

### <a name="which-azure-geographies-does-azure-migrate-support"></a>Azure 'un hangi Azure jeo geçiş desteği

[VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) ve [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects)için listelere bakın.

### <a name="how-do-i-delete-azure-migrate-projects-and-associated-log-analytics-workspaces"></a>Nasıl yaparım? Azure geçişi projelerini ve ilişkili Log Analytics çalışma alanlarını silmek mi istiyorsunuz?

Bir Azure geçişi projesini sildiğinizde, geçiş projesi *ve* bulunan makineler hakkında meta veriler silinir. Ancak, sunucu değerlendirmesi aracına bir Log Analytics çalışma alanı eklediyseniz, Log Analytics çalışma alanı otomatik olarak silinmez. (Aynı Log Analytics çalışma alanı birden çok kullanım durumu için kullanılabilir.) Log Analytics çalışma alanını da silmek istiyorsanız, bunu el ile yapmanız gerekir:

1. Projeye bağlı Log Analytics çalışma alanına gidin.
     * Geçiş projesini henüz silmediyse, temel bileşenler bölümündeki sunucu değerlendirmesi genel bakış sayfasından çalışma alanının bağlantısını bulabilirsiniz.

       ![LA çalışma alanı](./media/troubleshooting-general/loganalytics-workspace.png)

     * Geçiş projesini zaten sildiyseniz, Azure portal sol bölmesinde **kaynak grupları** ' nı seçin. Çalışma alanının oluşturulduğu kaynak grubuna gidin ve ardından buna gidin.
2. [Azure Portal bir Azure Log Analytics çalışma alanı silme](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace)bölümündeki yönergeleri izleyin.

### <a name="migration-project-creation-failed-with-a-requests-must-contain-user-identity-headers-error"></a>Geçiş projesi oluşturma bir "Istekler Kullanıcı kimlik üstbilgileri içermesi gerekir" hatasıyla başarısız oldu.

Bu sorun, kuruluşun Azure Active Directory (Azure AD) kiracısına erişimi olmayan kullanıcılar için ortaya çıkabilir. Bir Kullanıcı Azure AD kiracısına ilk kez eklendiğinde, Kullanıcı kiracıya katılması için bir e-posta daveti alır. Kullanıcıların kiracıya başarıyla eklenmesi için daveti kabul etmesi gerekir. E-postayı göremiyorsanız, kiracıya zaten erişimi olan bir kullanıcıyla iletişim kurun ve ardından [Konuk kullanıcılara yapılan davetleri yeniden gönder](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users)' de belirtilen adımları kullanarak daveti size tekrar göndermesini isteyin.

Davet e-postası alındıktan sonra, e-postayı açmanız ve daveti kabul etmek için bağlantıyı seçmeniz gerekir. Sonra, Azure portal oturumunuzu kapatıp yeniden oturum açmanız gerekir. (Tarayıcıyı yenilemek çalışmaz.) Daha sonra geçiş projesi oluşturmaya başlayabilirsiniz.

## <a name="appliance-issues"></a>Gereç sorunları

### <a name="deployment-of-an-azure-migrate-appliance-for-vmware-failed-with-a-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry-error"></a>VMware için bir Azure geçiş gereci dağıtımı, bir "belirtilen bildirim dosyası geçersiz: Geçersiz OVF manifest girdisi "hatası.

1. Azure geçişi gereç OVA dosyasının karma değerini denetleyerek doğru şekilde indirildiğini doğrulayın. Rehberlik için bkz. [Azure 'a değerlendirme ve geçiş Için VMware VM 'Leri hazırlama](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Karma değeri eşleşmiyorsa, OVA dosyasını yeniden indirin ve dağıtımı yeniden deneyin.
2. Dağıtım hala başarısız olursa ve OVF dosyasını dağıtmak için VMware vSphere istemcisini kullanıyorsanız, vSphere Web istemcisi aracılığıyla dağıtmayı deneyin. Dağıtım hala başarısız olursa, farklı bir Web tarayıcısı kullanmayı deneyin.
3. VSphere Web istemcisini kullanıyorsanız ve bunu vCenter Server 6,5 veya 6,7 ' de dağıtmaya çalışıyorsanız, aşağıdaki adımları izleyerek OVA 'yı doğrudan ESXi konağına dağıtmayı deneyin:
   - Web istemcisi (https://<*ana BILGISAYAR IP adresi*>/UI) kullanarak ESXi konağına doğrudan (vCenter Server yerine) bağlanın.
   - **Giriş** > **envanteri**' ne gidin.
   - **Dosya** > **dağıtımı ovf şablonu**' nu seçin. OVA 'ya gidin ve dağıtımı doldurun.
4. Dağıtım hala başarısız olursa Azure geçiş desteği 'ne başvurun.

### <a name="the-appliance-cant-connect-to-the-internet"></a>Gereç internet 'e bağlanamıyor.

Bu davranış, kullanmakta olduğunuz makine bir proxy 'nin arkasındaysa meydana gelebilir. Proxy 'ye ihtiyaç duyuyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Giden bağlantıyı denetlemek için herhangi bir URL tabanlı güvenlik duvarı proxy 'si kullanıyorsanız, bu gerekli URL 'Leri izin verilenler listesine eklediğinizden emin olun:

- [VMware için sunucu değerlendirmesi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
- [Hyper-V için sunucu değerlendirmesi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
- [VMware için sunucu geçişi (aracısız)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
- [VMware için sunucu geçişi (aracı tabanlı)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
- [Hyper-V için sunucu geçişi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

İnternet 'e bağlanmak için bir kesintiye uğratan Proxy kullanıyorsanız, proxy sertifikasını gereç VM 'sine aktarmanız gerekir. [Azure geçişi](https://docs.microsoft.com/azure/migrate/concepts-collector)gereci 'ndaki adımları izleyerek proxy sertifikasını içeri aktarabilirsiniz.

### <a name="error-802-date-and-time-synchronization-error"></a>Hata 802: Tarih ve saat eşitleme hatası.

Sunucu saati, beş dakikadan uzun bir süre boyunca geçerli zaman eşitlenmemiş olabilir. Toplayıcı VM 'de saat saatini şu şekilde geçerli saatle eşleşecek şekilde değiştirin:

1. VM 'de bir yönetici komut istemi açın.
2. Saat dilimini denetlemek için **w32tm/tz komutunu**çalıştırın.
3. Saati eşitleme için **w32tm/resync**komutunu çalıştırın.


###  <a name="connection-failed-error-unabletoconnecttoserver"></a>Bağlantı başarısız oldu. Hata: UnableToConnectToServer.

VCenter Server *ServerName*. com: 9443 öğesine bağlanamadan kaynaklanabilir. Hata ayrıntıları, https://*ServerName*. com: 9443/SDK ' da iletiyi kabul edebilen bir uç nokta dinleme olmadığını gösterir.

Bu durumda, toplayıcı gerecinin en son sürümünü çalıştırıp çalıştırmadığını kontrol edin. Değilseniz, gereci [en son sürüme](https://docs.microsoft.com/azure/migrate/concepts-collector)yükseltin.

Sorun hala en son sürümde gerçekleşirse, toplayıcı makinesi belirtilen vCenter Server adı çözemeyebilir veya belirtilen bağlantı noktası yanlış olabilir. Varsayılan olarak, bağlantı noktası belirtilmemişse, toplayıcı 443 numaralı bağlantı noktası numarasına bağlanmayı dener.

1. Toplayıcı makinesinden *ServerName*. com ' a ping gönderin.
2. 1\. adım başarısız olursa, IP adresini kullanarak vCenter Server 'a bağlanmayı deneyin.
3. vCenter’a bağlanmak için doğru bağlantı noktasını belirleyin.
4. VCenter sunucusunun çalışır ve çalışır olduğunu doğrulayın.


### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60052"></a>Gereç, Azure geçişi projesine başarıyla kaydedilmemiş olabilir (Hata KIMLIĞI: 60052).

Gereci kaydetmek için kullanılan Azure hesabının izinleri yetersizse bu hata oluşur. Gereci kaydetmek için kullanılan Azure Kullanıcı hesabının abonelikte en az katkıda bulunan izinleri olduğundan emin olun. Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .

### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60039"></a>Gereç, Azure geçişi projesine başarıyla kaydedilmemiş olabilir (Hata KIMLIĞI: 60039).

Gereci kaydettirmek için seçilen Azure geçişi projesi bulunamamış olabilir. Bu durumda, kayıt başarısız olur. Azure portal gidin ve projenin kaynak grubunuzda mevcut olup olmadığını denetleyin. Proje yoksa, kaynak grubunuzda yeni bir Azure geçişi projesi oluşturun ve gereci yeniden kaydedin. Yeni bir Azure geçişi projesi oluşturma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) .

### <a name="an-azure-key-vault-management-operation-failed-error-id-60030-60031"></a>Azure Key Vault Yönetimi işlemi başarısız oldu (Hata KIMLIĞI: 60030, 60031).

Gereci kaydetmek için kullanılan Azure Kullanıcı hesabının abonelikte en az katkıda bulunan izinleri olduğundan emin olun. Ayrıca, hesabın hata iletisinde belirtilen anahtar kasasına erişimi olduğundan emin olun ve işlemi yeniden deneyin. Sorun devam ederse, Microsoft desteği 'ne başvurun. Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .

### <a name="discovery-couldnt-be-initiated-because-of-an-error-the-operation-failed-for-the-specified-list-of-hosts-or-clusters-error-id-60028"></a>Bir hata nedeniyle bulma başlatılamadı. Belirtilen konaklar veya kümeler listesi için işlem başarısız oldu (Hata KIMLIĞI: 60028).

VM bilgilerine erişirken veya alırken bir sorun nedeniyle hatada listelenen konaklarda bulma işlemi başlatılamadı. Ana bilgisayarların geri kalanı başarıyla eklendi. Hata durumunda listelenen Konakları, **konak Ekle** seçeneğini kullanarak yeniden ekleyin. Doğrulama hatası varsa, hataları onarmak için düzeltme kılavuzunu gözden geçirin ve sonra **bulmayı Kaydet ve Başlat** seçeneğini tekrar deneyin.

### <a name="an-azure-ad-operation-failed-the-error-occurred-while-creating-or-updating-the-azure-ad-application-error-id-60025"></a>Azure AD işlemi başarısız oldu. Azure AD uygulaması oluşturulurken veya güncelleştirilirken hata oluştu (Hata KIMLIĞI: 60025).

Bulma işlemini başlatırken kullanılan Azure kullanıcı hesabı aleti kaydederken kullanılan hesaptan farklı olduğunda bu hatayla karşılaşılır. Aşağıdakilerden birini yapabilirsiniz:
1. Keşfi başlatan kullanıcı hesabının gereci kaydetmek için kullanılan ile aynı olduğundan emin olun.
1. Bulma işleminin başarısız olduğu diğer Kullanıcı hesabına AAD uygulama erişim izinleri sağlayın.
1. Daha önce Azure geçişi projesi için oluşturulmuş kaynak grubunu silin ve yeniden başlamak için başka bir kaynak grubu oluşturun.

AAD uygulama izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .

## <a name="discovery-issues"></a>Bulma sorunları

### <a name="i-started-discovery-but-dont-see-the-discovered-vms-on-the-azure-portal-the-server-assessment-and-server-migration-tiles-show-a-status-of-discovery-in-progress"></a>Bulmaya başladım, ancak Azure portal bulunan VM 'Leri görmüyorum. **Sunucu değerlendirmesi** ve **sunucu geçiş** kutucukları "bulma devam ediyor" durumunu gösterir.
Gereci bulmayı başlattıktan sonra, bulunan makinelerin Azure portal göstermesi için bir süre bekleyin. Bir VMware bulma işlemi için 15 dakika ve bir Hyper-V bulma için eklenen ana bilgisayar başına 2 dakikalık bir süre sürer. "Bulma devam ediyor" görmeye devam ederseniz, bu bekleme sürelerinden sonra bile **sunucular** sekmesinde **Yenile** ' yi seçin. Bu, sunucu değerlendirmesi ve sunucu geçiş kutucuklarında bulunan sunucu sayısını göstermelidir.


### <a name="im-using-an-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-shown-in-the-portal"></a>Şirket içi ortamımı sürekli bulduğu bir gereç kullanıyorum, ancak şirket içi ortammda silinen VM 'Ler hala portalda gösteriliyor.

Gereç tarafından toplanan bulgu verilerinin portalda yansıtılması 30 dakikaya kadar sürer. 30 dakika sonra bile güncel bilgileri görmüyorsanız, bu adımları izleyerek verileri yenileyin:

1. **Sunucular** > **Azure geçişi sunucu değerlendirmesi**' ne gidin ve **genel bakış**' ı seçin.
2. **Yönet**' in altında **Aracı durumu** ' yi seçin.
3. **Aracıyı Yenile**' yi seçin. Bu seçeneği aracıların listesinin altında görürsünüz.
4. Yenileme işleminin tamamlanmasını bekleyin. Artık sanal makinelerinize yönelik güncel bilgileri görmeniz gerekir.

### <a name="i-dont-see-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>Portalda şirket içi VM 'lerde en son bilgileri görmüyorum.

Gereç tarafından toplanan bulgu verilerinin portala yansıtılması 30 dakikaya kadar sürer. 30 dakika sonra bile güncel bilgileri görmüyorsanız, bu adımları izleyerek verileri yenileyin:

1. **Sunucular** > **Azure geçişi sunucu değerlendirmesi**' ne gidin ve **genel bakış**' ı seçin.
2. **Yönet**' in altında **Aracı durumu** ' yi seçin.
3. **Aracıyı Yenile**' yi seçin. Bu seçeneği aracıların listesinin altında görürsünüz.
4. Yenileme işleminin tamamlanmasını bekleyin. Artık sanal makinelerinize yönelik güncel bilgileri görmeniz gerekir.

### <a name="cant-connect-to-a-host-or-cluster-because-the-server-name-cant-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>Sunucu adı çözümlenemediğinden konağa veya kümeye bağlanılamıyor. WinRM hata kodu: 0x803381B9 (Hata KIMLIĞI: 50004).
Bu hata, Gereç için Azure DNS hizmeti, verdiğiniz küme veya ana bilgisayar adını çözümleyemediğinde oluşur. Kümede bu hatayı görürseniz, kümenin tam etki alanı adını (FQDN) sağlamayı deneyin.

Ayrıca, bir kümedeki konaklar için bu hatayı görebilirsiniz. Bu durumda, Gereç kümeye bağlanabilir ancak küme, FQDN olmayan ana bilgisayar adlarını döndürür.

Bu hatayı çözmek için, IP adresi ve ana bilgisayar adlarının eşlemesini ekleyerek gereç üzerindeki Hosts dosyasını güncelleştirin:
1. Not defteri 'Ni yönetici olarak açın. Ardından, C:\Windows\System32\Drivers\etc\hosts dosyasını açın.
2. IP adresini ve ana bilgisayar adını bir satıra ekleyin. Bu hatayı gördüğünüz her bir konak veya küme için tekrarlayın.
3. Hosts dosyasını kaydedin ve kapatın.
4. Gerecin, Gereç Yönetimi uygulamasını kullanarak konaklara bağlanıp bağlanamadığını kontrol edebilirsiniz. 30 dakika sonra, Azure portal bu konaklar için en son bilgileri görmeniz gerekir.


## <a name="assessment-issues"></a>Değerlendirme sorunları

### <a name="azure-readiness-issues"></a>Azure hazırlık sorunları

Sorun | Düzeltme
--- | ---
Desteklenmeyen önyükleme türü | Azure, EFı önyükleme türü olan VM 'Leri desteklemez. Geçiş çalıştırmadan önce önyükleme türünü BIOS 'a dönüştürmeniz önerilir. <br/><br/>Bu sanal makinelerin geçişini yönetmek için Azure geçişi sunucu geçişini kullanabilirsiniz. Geçiş sırasında VM 'nin önyükleme türünü BIOS 'a dönüştürür.
Koşullu olarak desteklenen Windows işletim sistemi | İşletim sistemi destek son tarihini geçti ve [Azure 'da destek](https://aka.ms/WSosstatement)için özel bir destek SÖZLEŞMESINE (CSA) ihtiyaç duyuyor. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeyi düşünün.
Desteklenmeyen Windows İşletim Sistemi | Azure yalnızca [Seçili Windows işletim sistemi sürümlerini](https://aka.ms/WSosstatement)destekler. Azure 'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeniz göz önünde bulundurun.
Koşullu olarak desteklenen Linux işletim sistemi | Azure yalnızca [Seçili Linux işletim sistemleri sürümlerini](../virtual-machines/linux/endorsed-distros.md)onaylar. Azure 'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeniz göz önünde bulundurun.
Desteklenmeyen Linux İşletim Sistemi | Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. Azure 'a geçiş yapmadan önce işletim sistemini, [onaylı bir Linux sürümüne](../virtual-machines/linux/endorsed-distros.md) yükseltmeyi düşünün.
Bilinmeyen işletim sistemi | VM 'nin işletim sistemi vCenter Server içinde "Other" olarak belirtilmiştir. Bu davranış, Azure geçişi 'nin VM 'nin Azure 'un hazır olduğunu doğrulamasını engeller. Makineyi geçirmeden önce makinenin işletim sisteminin Azure tarafından [desteklendiğinden](https://aka.ms/azureoslist) emin olun.
İşletim sistemi bit genişliği desteklenmiyor | 32 bitlik bir işletim sistemine sahip VM 'Ler Azure 'da önbaşlatılabilir, ancak Azure 'a geçmeden önce VM 'nin işletim sistemini 64 bit 'e yükseltmenizi öneririz.
Microsoft Visual Studio aboneliği gerektirir | Makine, yalnızca bir Visual Studio aboneliği aracılığıyla desteklenen bir Windows istemci işletim sistemini çalıştırıyor.
Gerekli depolama performansı için VM bulunamadı | Makine için gereken depolama performansı (saniye başına giriş/çıkış işlemi [ıOPS] ve üretilen iş), Azure VM desteğini aşıyor. Geçişten önce makinenin depolama gereksinimlerini azaltın.
Gerekli ağ performansı için VM bulunamadı | Makine için gereken ağ performansı (ın/out) Azure VM desteğini aşıyor. Makinenin ağ gereksinimlerini azaltın.
Belirtilen konumda VM bulunamadı | Geçişten önce farklı bir hedef konum kullanın.
Bir veya daha fazla disk uyumsuz | VM 'ye bağlı bir veya daha fazla disk, Azure gereksinimlerini karşılamıyor. Azure Geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD diskleri desteklemez ve Premium yönetilen diskler (32 TB) için disk sınırlarına göre diskleri değerlendirir.  SANAL makineye bağlı her disk için, diskin boyutunun < 64 TB (Ultra SSD diskler tarafından desteklenir) olduğundan emin olun, Azure 'a geçiş yapmadan önce disk boyutunu küçültün veya Azure 'da birden çok disk kullanın ve daha yüksek depolama sınırları almak için [bunları birlikte toplayın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) . Her disk için gereken performansın (ıOPS ve aktarım hızı) Azure [yönetilen sanal makine diskleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits)tarafından desteklendiğinden emin olun.
Bir veya daha fazla uygun olmayan ağ bağdaştırıcısı. | Kullanılmayan ağ bağdaştırıcılarını geçişten önce makineden kaldırın.
Disk sayısı, sınırı aşıyor | Kullanılmayan diskleri geçişten önce makineden kaldırın.
Disk boyutu, sınırı aşıyor | Azure Geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD diskleri desteklemez ve Premium disk sınırlarına (32 TB) göre diskleri değerlendirir. Ancak Azure, 64 TB 'a kadar olan diskleri destekler (Ultra SSD diskleri tarafından desteklenir). Geçiş yapmadan önce diskleri 64 TB 'den daha az bir değere küçültün veya Azure 'da birden çok disk kullanın ve daha yüksek depolama sınırları almak için [bunları birlikte toplayın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) .
Disk, belirtilen konumda kullanılamıyor | Geçirmeden önce diskin hedef konumunda olduğundan emin olun.
Disk, belirtilen çoğaltma için kullanılamıyor | Disk, değerlendirme ayarlarında tanımlanan artıklık depolama türünü kullanmalıdır (varsayılan olarak LRS).
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdeklere ve belleğe sahip VM bulunamadı | Azure uygun bir VM türü bulamadı. Geçirmeden önce şirket içi makinenin bellek ve çekirdek sayısını azaltın.
Bir iç hata nedeniyle VM uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla disk için uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı için uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.

### <a name="i-cant-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>Değerlendirme özelliklerinde Azure teklifi olarak Kurumsal Anlaşma (EA) belirtemezsiniz.
Azure geçişi sunucu değerlendirmesi Şu anda Kurumsal Anlaşma (EA) tabanlı fiyatlandırmayı desteklememektedir. Bu kısıtlamayı geçici olarak çözmek için Azure teklifi olarak **Kullandıkça Öde** ' yi kullanın ve aldığınız özel bir iskontoyu belirtmek için **indirim** özelliğini kullanın. [Değerlendirmeyi özelleştirme hakkında daha fazla bilgi edinin](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready"></a>Sunucu değerlendirmesi neden Linux sanal makinelerimi "koşullu olarak hazırlanıyor" olarak işaretmidir?
Sunucu değerlendirmesinde, şirket içi VM 'lerde yüklü Linux IŞLETIM sisteminin alt sürümünü algılamasını engelleyen bilinen bir boşluk var (örneğin, RHEL 6,10 için şu anda sunucu değerlendirmesi, işletim sistemi sürümü olarak yalnızca RHEL 6 ' ı algılar). Azure, Linux 'un yalnızca belirli sürümlerini sağladığından, Linux VM 'Leri Şu anda sunucu değerlendirmesi için koşullu olarak kullanılabilir olarak işaretlendi. [Azure Linux destek belgelerini](https://aka.ms/migrate/selfhost/azureendorseddistros)inceleyerek ŞIRKET içi VM 'de çalışan Linux Işletim sisteminin Azure 'da olup olmadığını belirleyebilirsiniz. Doğrulanan dağıtımı doğruladıktan sonra bu uyarıyı yoksayabilirsiniz.

### <a name="why-does-the-vm-sku-recommended-by-server-assessment-have-more-cores-and-more-memory-than-whats-allocated-on-premises"></a>Sunucu değerlendirmesi tarafından önerilen sanal makine SKU 'SU, şirket içi ayrıldıkından daha fazla çekirdeğe ve daha fazla belleğe sahip olabilir mi?
Sunucu Değerlendirmesi'nde VM SKU'su önerisi, değerlendirme özelliklerine bağlıdır. Sunucu değerlendirmesi için iki tür değerlendirme oluşturabilirsiniz: *Performans tabanlı* ve *Şirket içi olarak*. Performans tabanlı değerlendirmelere yönelik sunucu değerlendirmesi, şirket içi sanal makinelerinize yönelik doğru hedef VM SKU 'sunu belirlemede şirket içi VM 'lerin (CPU, bellek, disk ve ağ kullanımı) kullanım verilerini dikkate alır. Bunlara ek olarak, performans tabanlı boyutlandırma için, uygun kullanımı belirlemek için rahatlık faktörü göz önünde bulundurulmakta sayılır. Şirket içi boyutlandırma için performans verileri göz önünde bulundurulmaz ve şirket içinde ayrıldıklarıyla ilgili olarak bir hedef SKU 'SU önerilir.

Örneğin,% 50 CPU kullanımı ve% 50 bellek kullanımı ile 4 çekirdek ve 8 GB bellek içeren bir şirket içi VM 'nin olduğunu ve değerlendirmede bir rahat1,3 lığını belirtelim. Değerlendirmenin boyutlandırma ölçütleri **Şirket içinde**ise, 4 çekirdek ve 8 GIGABAYT (GB) bellek Içeren BIR Azure VM SKU 'su önerilir.

Ancak, boyutlandırma ölçütlerinin performans tabanlı olduğunu varsayalım. Etkin CPU ve bellek kullanımına 50 (4 çekirdek * 1,3 = 2,6 çekirdekler 50 ve 8 GB bellek * 1,3 = 5,3-GB bellek) göre, dört çekirdekli (en yakın desteklenen çekirdek sayısı) ve 8 GB bellek (en yakın desteklenen bellek boyutu), en iyi şekilde kullanılabilecek bir sanal makine SKU 'SU önerilir. [Sunucu Değerlendirmesi'nin boyutlandırmayı nasıl yaptığı hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

### <a name="why-is-the-disk-sku-recommended-by-server-assessment-bigger-than-whats-allocated-on-premises"></a>Sunucu değerlendirmesi tarafından şirket içi ayrılandan daha büyük olan disk SKU 'SU neden önerilir?
Sunucu değerlendirmesinde disk boyutlandırma iki değerlendirme özelliklerine bağlıdır: boyutlandırma ölçütleri ve depolama türü. Boyutlandırma ölçütü **performans tabanlıdır** ve depolama türü **Otomatik**olarak ayarlandıysa, diskin IOPS ve aktarım hızı değerleri, hedef disk türünü (Standart HDD, standart SSD veya Premium diskler) belirlemek için kabul edilir. Daha sonra disk türünden bir disk SKU 'SU önerilir ve bu öneri, şirket içi diskin boyut gereksinimlerini dikkate alır. Boyutlandırma ölçütü **performans tabanlıdır**ve depolama türü **Premium**ise, şirket içi diskin IOPS, verimlilik ve Boyut gereksinimlerine bağlı olarak Azure 'DA BIR Premium disk SKU 'su önerilir. Aynı mantık, boyutlandırma ölçütleri **Şirket içinde** olduğunda ve depolama türü **Standart HDD**, **Standart SSD**veya **Premium**olduğunda disk boyutlandırmayı gerçekleştirmek için kullanılır.

Örneğin, 32 GB bellek içeren bir şirket içi diskiniz varsa, ancak diskin toplanmış okuma ve yazma ıOPS değeri 800 ıOPS ise, sunucu değerlendirmesi bir Premium disk türü önerir (daha yüksek ıOPS gereksinimleri nedeniyle) ve ardından destekleyeceği bir disk SKU 'SU önerilir gerekli ıOPS ve boyut. Bu örnekteki en yakın eşleşme P15 (256 GB, 1100 IOPS) olabilir. Şirket içi diskin gerektirdiği boyut 32 GB olsa da, şirket içi diskin yüksek ıOPS gereksinimi nedeniyle sunucu değerlendirmesi daha büyük bir disk önerir.

### <a name="why-does-my-assessment-report-indicate-percentageofcoresutilizedmissing-or-percentageofmemoryutilizedmissing-for-some-vms"></a>Değerlendirme raporum bazı VM 'Ler için neden "PercentageOfCoresUtilizedMissing" veya "PercentageOfMemoryUtilizedMissing" gösteriyor?
Bu sorunlar, Azure geçişi gereci şirket içi VM 'Ler için performans verilerini toplayamayacak zaman bildirilir. Bu davranış, değerlendirme oluştururken (son bir gün/bir hafta/bir ay) VM 'Ler süre boyunca kapatılırsa meydana gelebilir. Gereç, devre dışı bırakıldığında bir VM için performans verilerini toplayamıyor. Yalnızca bellek sayaçları eksikse ve Hyper-V VM 'lerini değerlendirmeye çalışıyorsanız, bu VM 'lerde etkin bir dinamik bellek olup olmadığını kontrol edin. Azure geçişi gerecinin, dinamik belleği etkinleştirilmemiş VM 'Ler için bellek kullanım verilerini toplayamediği bilinen bir sorun vardır. Bu sorun, VMware VM 'lerini değil yalnızca Hyper-V VM 'lerini etkiler. Performans sayaçlarından herhangi biri eksikse, Azure geçişi sunucu değerlendirmesi ayrılmış çekirdeğe ve belleğe geri döner ve buna karşılık gelen VM boyutunu önerir.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>Sunucu değerlendirmesi tarafından tahmin edilen işlem maliyetine dahil edilen VM 'nin işletim sistemi lisans maliyeti midir?
Sunucu değerlendirmesi Şu anda yalnızca Windows makineler için işletim sistemi lisans maliyetini dikkate alır. Linux makineler için işletim sistemi lisans maliyetleri Şu anda dikkate alınmamaktadır.

### <a name="what-impact-do-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Performans geçmişi ve yüzdebirlik kullanımı ne kadar etkiler boyut önerilerinde?
Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir. Sunucu Değerlendirmesi şirket içi makinelerin performans verilerini sürekli toplar ve bunları Azure’da VM SKU'su ile disk SKU'su önermek için kullanır. Bu performans verileri sunucu değerlendirmesi tarafından aşağıdaki şekilde toplanır:
- Azure geçişi gereci, VMware VM 'leri için her 20 saniyede bir ve Hyper-V VM 'Leri için 30 saniyede bir gerçek zamanlı kullanım verileri toplamak üzere şirket içi ortamı sürekli olarak profiller.
- Gereç, her 10 dakikada tek bir veri noktası oluşturmak için 20 saniyelik ve 30 saniyelik örnek toplar. Tek veri noktasını oluşturmak için, Gereç tüm 20 saniyelik ve 30 saniyelik örneklerden tepe değeri seçer ve ardından bunu Azure 'a gönderir.
- Sunucu Değerlendirmesi'nde bir değerlendirme oluşturduğunuzda, performans süresi ve performans geçmişi yüzdebirlik değerine bağlı olarak temsili kullanım değeri belirlenir. Örneğin, performans geçmişi bir hafta ise ve yüzdebirlik kullanımı 95 ise, Azure geçişi son bir hafta için 10 dakikalık tüm örnek noktalarını artan düzende sıralar ve ardından temsili değeri olarak 95. yüzdebirlik değerini seçer.
95. yüzdebirlik değeri, 99. yüzdebirlik değerini seçerseniz dahil edilen tüm aykırı değerleri yok saydığınızdan emin olmanızı sağlar. Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız, yüzdebirlik kullanımı için 99. yüzdebirlik ' ü seçmeniz gerekir.

## <a name="dependency-visualization-issues"></a>Bağımlılık görselleştirme sorunları

### <a name="i-cant-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Azure Kamu projeleri için bağımlılık görselleştirme işlevini bulamıyorum.

Azure geçişi, bağımlılık görselleştirme işlevselliği için Hizmet Eşlemesi bağımlıdır. Hizmet Eşlemesi Şu anda Azure Kamu 'da kullanılamadığından, bu işlev Azure Kamu 'da kullanılamaz.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Şirket içi sanal makinelerime Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükledim, ancak bağımlılıklar artık Azure geçişi portalında gösteriliyor.

Aracıları yükledikten sonra, Azure geçişi genellikle portalda bağımlılıkları göstermek için 15-30 dakika sürer. 30 dakikadan uzun süre beklemişseniz, bu adımları izleyerek MMA 'nın OMS çalışma alanıyla konuşabildiğinizden emin olun.

Windows VM için:
1. Denetim Masası 'na gidin ve Microsoft Monitoring Agent başlatın.
2. **Microsoft Monitoring Agent özellikleri** Iletişim kutusundaki **Azure Log Analytics (OMS)** sekmesinde, çalışma alanının **durumunun** yeşil olduğundan emin olun.
3. Durum yeşil değilse, çalışma alanını kaldırmayı ve onu yeniden eklemeyi deneyin.

      ![MMA özellikleri iletişim kutusu](./media/troubleshooting-general/mma-status.png)

Linux VM için, MMA ve bağımlılık aracısının yükleme komutlarının başarılı olduğundan emin olun.

### <a name="what-operating-systems-does-mma-support"></a>MMA hangi işletim sistemlerini destekler?

 [MMA tarafından desteklenen Windows işletim sistemlerinin bir listesi aşağıda verilmiştir](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
Ve [MMA tarafından desteklenen Linux işletim sistemlerinin bir listesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-operating-systems-does-the-dependency-agent-support"></a>Bağımlılık Aracısı hangi işletim sistemlerini destekler?

[Bağımlılık Aracısı tarafından desteklenen Windows işletim sistemlerinin bir listesi aşağıda verilmiştir](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems). [Bağımlılık Aracısı tarafından desteklenen Linux işletim sistemlerinin bir listesi aşağıda verilmiştir](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-cant-visualize-dependencies-in-azure-migrate-for-more-than-a-one-hour-duration"></a>Azure geçişi 'ndeki bağımlılıkları bir saatlik süre için görselleştirilemiyor.
Azure geçişi 'nde, en fazla bir saatlik süreye kadar bağımlılıkları görselleştirebilirsiniz. Azure geçişi, son ayın belirli bir tarihine geri dönmenize izin verse de, bağımlılıkları görselleştirebileceğiniz en uzun süre bir saattir.

Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresi işlevini kullanabilirsiniz, ancak bunları yalnızca bir saatlik dönem için görüntüleyebilirsiniz. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

### <a name="i-cant-visualize-dependencies-for-groups-that-have-more-than-10-vms"></a>10 ' dan fazla VM 'ye sahip olan gruplar için bağımlılıkları görselleştirilemiyor.
10 adede kadar VM 'ye sahip olan [gruplar için bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) , 10 ' dan fazla VM içeren bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve bağımlılıkları görselleştirmenizi öneririz.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Aracıları yükledim ve gruplar oluşturmak için bağımlılık görselleştirmesini kullandınız. Şimdi, yük devretme sonrası makineler "bağımlılıkları görüntüle" yerine "aracıyı yükler" eylemini gösterir.
* Planlı veya planlanmamış bir yük devretme işleminden sonra şirket içi makineler kapalıdır ve eşdeğer makineler Azure 'da kapalıdır. Bu makineler farklı bir MAC adresi edintir. Kullanıcının şirket içi IP adresini tutmayı tercih etmeksizin, farklı bir IP adresi elde edebilirler.

  MAC ve IP adreslerinden her ikisi de farklıysa Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez. Bunun yerine, kullanıcıdan bağımlılıkları görüntülemek yerine aracıları yüklemesini ister.
* Test sonrası yük devretme, şirket içi makineler beklendiği gibi açık kalır. Azure 'da bulunan eşdeğer makineler, farklı bir MAC adresi elde edebilir ve farklı IP adresleri alabilir. Kullanıcı, bu makinelerden giden Azure Izleyici günlük trafiğini engellemeden Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez ve kullanıcılardan bağımlılıkları görüntülemek yerine aracıları yüklemesini ister.

## <a name="collect-azure-portal-logs"></a>Azure portal günlüklerini topla

**Azure portal ağ trafiği günlükleri Nasıl yaparım? toplansın mı?**

1. Tarayıcıyı açın, [portala](https://portal.azure.com)gidin ve oturum açın.
2. Geliştirici Araçları başlamak için F12 tuşuna basın. Gerekirse, **Gezinti ayarında girişleri temizle** ' yi temizleyin.
3. **Ağ** sekmesini seçin ve ağ trafiğini yakalamaya başlayın:
   - Chrome 'da **günlüğü koru**' yı seçin. Kayıt otomatik olarak başlamalıdır. Kırmızı bir daire trafiğin yakalandığını gösterir. Kırmızı daire görünmezse başlamak için siyah daireyi seçin.
   - Microsoft Edge ve Internet Explorer 'da kayıt otomatik olarak başlamalıdır. Aksi takdirde, yeşil oynatma düğmesini seçin.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kayıt sırasında hata ile karşılaşduktan sonra kaydı durdurun ve kayıtlı etkinliğin bir kopyasını kaydedin:
   - Chrome 'da, sağ tıklayın ve **IÇERIKLE har olarak kaydet**' i seçin. Bu eylem, günlükleri bir. har dosyası olarak sıkıştırır ve dışarı aktarır.
   - Microsoft Edge veya Internet Explorer 'da **yakalanan trafiği dışarı aktar** seçeneğini belirleyin. Bu eylem günlüğü sıkıştırır ve dışarı aktarır.
6. Herhangi bir uyarı veya hata olup olmadığını denetlemek için **konsol** sekmesini seçin. Konsol günlüğünü kaydetmek için:
   - Chrome 'da konsol günlüğünde herhangi bir yere sağ tıklayın. Günlüğe **Kaydet**, dışarı aktarmak için ve günlüğü posta ' yı seçin.
   - Microsoft Edge veya Internet Explorer 'da hatalara sağ tıklayıp **Tümünü Kopyala**' yı seçin.
7. Geliştirici Araçları kapatın.
