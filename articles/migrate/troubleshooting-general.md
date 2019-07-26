---
title: Azure geçişi sorunlarını giderme | Microsoft Docs
description: Azure geçişi hizmetindeki bilinen sorunlara genel bir bakış ve sık karşılaşılan hatalar için sorun giderme ipuçları sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 15d3809b9a028fd2495c504e9bf19251dd051520
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372592"
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

[Azure geçişi](migrate-services-overview.md) , değerlendirme ve geçiş için Microsoft araçları, ayrıca üçüncü taraf bağımsız yazılım SATıCıSı (ISV) teklifleri için bir merkez sağlar. Bu belge Azure geçişi, Azure geçişi ile ilgili sorun giderme hakkında yardım sağlar: Sunucu değerlendirmesi ve Azure geçişi: Sunucu Geçişi'ne bağlar.

## <a name="azure-migrate-project-issues"></a>Azure geçişi proje sorunları

### <a name="i-am-unable-to-find-my-existing-azure-migrate-project"></a>Mevcut Azure geçiş projem bulunamıyor.

Azure geçişi 'nin [iki sürümü](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) vardır. Projeyi oluşturduğunuz sürüme bağlı olarak, projeyi bulmak için aşağıdaki adımları izleyin:

1. Azure geçişi 'nin sürekli sürümü (eski deneyim) ile oluşturulmuş bir proje arıyorsanız, projeyi bulmak için aşağıdaki adımları izleyin.

    1. [Azure Portal](https://portal.azure.com)gidin, **Azure geçişi**' ni arayın.
    2. Azure geçişi panosunda, eski projelere erişme hakkında konuşacak bir başlık görürsünüz. Bu başlığı, yalnızca eski deneyimle bir proje oluşturduysanız görürsünüz. Başlık ' a tıklayın.

    ![Mevcut projelere erişin](./media/troubleshooting-general/access-existing-projects.png)

    3. Şimdi Azure geçişi 'nin önceki sürümüyle oluşturulan mevcut projelerin listesini görürsünüz.

2. Geçerli sürümle (yeni deneyim) oluşturulmuş bir proje arıyorsanız, projeyi bulmak için aşağıdaki adımları izleyin.

    1. [Azure Portal](https://portal.azure.com)gidin, **Azure geçişi**' ni arayın.
    2. Azure geçişi panosunda sol bölmedeki **sunucular** sayfasına gidin ve sağ üst köşedeki **Değiştir** ' i seçin:

    ![Mevcut bir Azure geçişi projesine geç](./media/troubleshooting-general/switch-project.png)

    3. Uygun **aboneliği** seçin ve **projeyi geçirin**.

### <a name="i-am-unable-to-create-a-second-azure-migrate-project"></a>İkinci bir Azure geçişi projesi oluşturamıyorum.

Yeni bir Azure geçişi projesi oluşturmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com)gidin, **Azure geçişi**' ni arayın.
2. Azure geçişi panosunda sol bölmedeki **sunucular** sayfasına gidin ve sağ üst köşedeki **Değiştir** ' i seçin:

   ![Azure geçişi projesini değiştirme](./media/troubleshooting-general/switch-project.png)

3. Yeni bir proje oluşturmak için aşağıdaki ekran görüntüsünde gösterildiği gibi **buraya tıklayın ' ı** seçin:

   ![İkinci bir Azure geçişi projesi oluşturma](./media/troubleshooting-general/create-new-project.png)

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Azure geçişi tarafından hangi Azure coğrafi ve desteklenen?

VMware için listeyi [burada](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) ve [Hyper-V için burada](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects)bulabilirsiniz.

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Azure geçişi projelerini ve ilişkili Log Analytics çalışma alanını silme

Bir Azure geçişi projesini sildiğinizde, geçiş projesini bulunan makineler hakkındaki meta verilerle birlikte siler. Ancak, sunucu değerlendirmesi aracına bir Log Analytics çalışma alanı eklediyseniz, Log Analytics çalışma alanını otomatik olarak silmez. Bunun nedeni, birden çok kullanım durumu için aynı Log Analytics çalışma alanının kullanılması olabilir. Log Analytics çalışma alanını da silmek isterseniz, el ile yapmanız gerekir.

1. Projeye bağlı Log Analytics çalışma alanına gidin.
     1. Henüz geçiş projesini sildiyseniz, temel bileşenler bölümündeki sunucu değerlendirmesi genel bakış sayfasından çalışma alanının bağlantısını bulabilirsiniz.

     ![LA çalışma alanı](./media/troubleshooting-general/loganalytics-workspace.png)

     2. Geçiş projesini zaten sildiyseniz Azure portal sol bölmedeki **kaynak grupları** ' nı seçin. Çalışma alanının oluşturulduğu kaynak grubuna gidin ve ardından buna gidin.
2. Çalışma alanını silmek için [Bu makaledeki](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) yönergeleri izleyin.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Geçiş projesi oluşturma başarısız oldu, hata *istekleri Kullanıcı kimlik üst bilgileri içermelidir*

Bu sorun, kuruluşun Azure Active Directory (Azure AD) kiracısına erişimi olmayan kullanıcılar için ortaya çıkabilir. Bir Kullanıcı Azure AD kiracısına ilk kez eklendiğinde kiracıya katılması için bir e-posta daveti alır. Kullanıcıların e-postaya gitmesi ve kiracıya başarıyla eklenmesi için daveti kabul etmesi gerekir. E-postayı göremiyorsanız, kiracıya zaten erişimi olan bir kullanıcıya ulaşın ve [burada](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users)belirtilen adımları kullanarak daveti size yeniden göndermesini isteyin.

Davet e-postası alındıktan sonra, e-postayı açmanız ve e-postadaki bağlantıya tıklayarak daveti kabul etmeniz gerekir. Bu işlem tamamlandıktan sonra, Azure portal oturumunuzu kapatıp yeniden oturum açmanız gerekir. böylece, tarayıcıyı yenilemek çalışmaz. Ardından geçiş projesini oluşturmayı deneyebilirsiniz.

## <a name="appliance-issues"></a>Gereç sorunları

### <a name="deployment-of-azure-migrate-appliance-for-vmware-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>VMware için Azure geçişi gereci dağıtımı şu hatayla başarısız oldu: Sağlanan bildirim dosyası geçersiz: Geçersiz OVF manifest girdisi.

1. Azure geçişi gereci OVA dosyasının karma değerini denetleyerek doğru şekilde indirilip indirilmediğini doğrulayın. Karma değerini doğrulamak için [bu makaleye](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware) bakın. Karma değeri eşleşme değilse, OVA dosyasını yeniden indirin ve dağıtımı yeniden deneyin.
2. Yine başarısız olursa ve OVF dağıtımı için VMware vSphere Client uygulamasını kullanıyorsanız vSphere Web Client üzerinden dağıtmayı deneyin. Hala başarısız olduysa, farklı bir Web tarayıcısı kullanmayı deneyin.
3. VSphere Web istemcisi kullanıyorsanız ve bunu vCenter Server 6,5 veya 6,7 ' de dağıtmaya çalışıyorsanız, aşağıdaki adımları izleyerek OVA 'yı doğrudan ESXi konağına dağıtmayı deneyin:
   - Web istemcisi (https://<*ana BILGISAYAR IP adresi*>/UI) kullanarak ESXi konağına doğrudan (vCenter Server yerine) bağlanın.
   - **Giriş** > **envanteri**' ne gidin.
   - **Ovf şablonu**' na tıklayın.  >  OVA 'ya gidin ve dağıtımı doldurun.
4. Dağıtım hala başarısız olursa Azure geçiş desteği 'ne başvurun.

### <a name="appliance-is-not-able-to-connect-to-the-internet"></a>Gereç internet 'e bağlanamıyor

Bu durum, kullandığınız makine bir proxy 'nin arkasındaysa meydana gelebilir. Proxy 'nin bir tane ihtiyacı varsa, yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Giden bağlantıyı denetlemek için herhangi bir URL tabanlı güvenlik duvarı proxy 'si kullanıyorsanız, bu gerekli URL 'Leri izin verilenler listesine eklediğinizden emin olun:

Senaryo | URL listesi
--- | ---
VMware için sunucu değerlendirmesi | [Şöyledir](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
Hyper-V için sunucu değerlendirmesi | [Şöyledir](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
VMware için sunucu geçişi (aracısız) | [Şöyledir](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
VMware için sunucu geçişi (aracı tabanlı) | [Şöyledir](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
Hyper-V için sunucu geçişi | [Şöyledir](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Internet 'e bağlanmak için bir kesintiye uğratan ara sunucu kullanıyorsanız, proxy sertifikasını gereç VM 'sine aktarmanız gerekir. [Burada](https://docs.microsoft.com/azure/migrate/concepts-collector)açıklanan adımları kullanarak proxy sertifikasını içeri aktarabilirsiniz.

### <a name="error-802-date-and-time-synchronization-error"></a>Hata 802: Tarih ve saat eşitleme hatası

Sunucu saati, beş dakikadan uzun bir süre boyunca geçerli zaman eşitleme dışı olabilir. Toplayıcı VM 'de saat saatini şu şekilde geçerli saatle eşleşecek şekilde değiştirin:

1. VM 'de bir yönetici komut istemi açın.
2. Saat dilimini denetlemek için W32tm/tzrun komutunu çalıştırın.
3. Saati eşitleme için W32tm/yeniden eşitleniyor ckomutunu çalıştırın.


### <a name="error-unabletoconnecttoserver"></a>Hata UnableToConnectToServer

Şu hata nedeniyle vCenter Server'a bağlanamıyor "Servername.com:9443": İletiyi kabul edebilecek hiçbir uç nokta https://Servername.com:9443/sdk konumunu dinlemiyordu.

Toplayıcı gerecinin en son sürümünü çalıştırıp çalıştırmadığından emin olun, Gereç sürümünü [en son sürüme](https://docs.microsoft.com/azure/migrate/concepts-collector)yükseltin.

Sorun hala en son sürümde gerçekleşirse, bunun nedeni, toplayıcı makinesinin belirtilen vCenter Server adı çözemediği veya belirtilen bağlantı noktasının yanlış olması olabilir. Varsayılan olarak, bağlantı noktası belirtilmemişse, toplayıcı 443 numaralı bağlantı noktasına bağlanmaya çalışır.

1. Servername.com, toplayıcı makinesinden ping işlemi yapmayı deneyin.
2. 1 adım başarısız olursa, IP adresi üzerinden vCenter Server’a bağlanmayı deneyin.
3. vCenter’a bağlanmak için doğru bağlantı noktasını belirleyin.
4. Son olarak vCenter Server’ın çalışır durumda olup olmadığını denetleyin.


### <a name="the-appliance-could-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60052"></a>Gereç, Azure geçişi projesine başarıyla kaydedilemedi (Hata KIMLIĞI: 60052)

Bu hata, gereci kaydetmek için kullanılan Azure hesabındaki izinlerin yetersiz olması nedeniyle gereklidir. Gereci kaydettirmek için kullanılan Azure Kullanıcı hesabının abonelikte en az ' katılımcı ' erişimine sahip olduğundan emin olun. Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .

### <a name="the-appliance-could-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60039"></a>Gereç, Azure geçişi projesine başarıyla kaydedilemedi (Hata KIMLIĞI: 60039)

Gereci kaydettirmek için seçtiğiniz Azure geçişi projesi bulunamadı, kaydın başarısız olmasına neden olur. Azure portal gidin ve projenin kaynak grubunuzda mevcut olup olmadığını denetleyin. Proje yoksa, kaynak grubunuzda yeni bir Azure geçişi projesi oluşturun ve gereci yeniden kaydedin. Yeni bir Azure geçişi projesi oluşturma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) .

### <a name="azure-key-vault-management-operation-failed-error-id-60030-60031"></a>Azure Anahtar Kasası Yönetimi işlemi başarısız oldu (Hata KIMLIĞI: 60030, 60031)

Gereci kaydettirmek için kullanılan Azure Kullanıcı hesabının abonelikte en az ' katılımcı ' erişimine sahip olduğundan emin olun. Ayrıca, hesabın hata iletisinde belirtilen Key Vault erişimi olup olmadığını denetleyin ve işlemi yeniden deneyin. Sorun devam ederse, Microsoft desteği 'ne başvurun. Gerekli Azure rolleri ve izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .

### <a name="discovery-could-not-be-initiated-due-to-the-error-the-operation-failed-for-the-given-list-of-hosts-or-clusters-error-id-60028"></a>Hata nedeniyle bulma başlatılamadı. İşlem, belirtilen konaklar veya kümeler listesi için başarısız oldu (Hata KIMLIĞI: 60028)

VM bilgilerine erişirken veya alırken bir sorun oluştuğundan hatada listelenen konaklarda bulma işlemi başlatılamadı; eklediğiniz ana bilgisayarların geri kalanı başarıyla eklendi. **Ana bilgisayar Ekle** seçeneğini kullanarak hata içindeki Konakları yeniden ekleyin. Doğrulama hatası varsa, hataları onarmak için düzeltme kılavuzunu gözden geçirin ve **bulmayı yeniden kaydetmeyi ve başlatmayı** deneyin.

### <a name="azure-active-directory-aad-operation-failed-the-error-occurred-while-creating-or-updating-the-aad-application-error-id-60025"></a>Azure Active Directory (AAD) işlemi başarısız oldu. AAD uygulaması oluşturulurken veya güncelleştirilirken hata oluştu (Hata KIMLIĞI: 60025)

Gereci kaydetmek için kullanılan Azure Kullanıcı hesabının, hata iletisinde belirtilen AAD uygulamasına erişimi yok. AAD uygulamasının sahibi olup olmadığınızı kontrol edin. AAD uygulama izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) .


## <a name="discovery-issues"></a>Bulma sorunları

### <a name="i-started-discovery-but-i-dont-see-the-discovered-vms-on-azure-portal-server-assessment-and-server-migration-tiles-show-a-status-of-discovery-in-progress"></a>Bulmayı başladım, ancak Azure portal bulunan VM 'Leri görmüyorum. Sunucu değerlendirmesi ve sunucu geçiş kutucukları "bulma devam ediyor" durumunu gösterir
Gereci bulmayı başlattıktan sonra, bulunan makinelerin Azure portal göstermesi için bir süre bekleyin. Bir VMware bulma işlemi için 15 dakika ve bir Hyper-V bulma için eklenen ana bilgisayar başına 2 dakikalık bir süre sürer. Bu süreden sonra bile "bulma devam ediyor" görmeye devam ederseniz, **sunucular** sekmesinde **Yenile** ' ye tıklayın. Bu, sunucu değerlendirmesi ve sunucu geçiş kutucuklarında bulunan sunucu sayısını göstermelidir.


### <a name="i-am-using-the-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Şirket içi ortamımı sürekli bulduğu gereci kullanıyorum, ancak şirket içi ortammda silinen VM 'Ler hala portalda gösteriliyor.

Gereç tarafından toplanan keşif verilerinin portalda yansıtılması 30 dakikaya kadar sürer. 30 dakika sonra bile güncel bilgileri görmüyorsanız, aşağıdaki adımları kullanarak verilerde bir yenileme yayınlayın:

1. Sunucular > Azure geçişi: Sunucu değerlendirmesi, **Genel Bakış ' a**tıklayın.
2. **Yönet**altında **Aracı durumu** ' ye tıklayın.
3. **Aracıyı yenileme**seçeneğine tıklayın. Aşağıda, aracıların listesinin altında bu seçeneği görürsünüz.
4. Yenileme işleminin tamamlanmasını bekleyin. Sanal makinelerinizdeki güncel bilgileri görebildiğinizi doğrulayın.

### <a name="i-dont-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>Portalda şirket içi VM 'Lerle ilgili en son bilgileri bilmiyorum

Gereç tarafından toplanan keşif verilerinin portalda yansıtılması 30 dakikaya kadar sürer. 30 dakika sonra bile güncel bilgileri görmüyorsanız, aşağıdaki adımları kullanarak veri üzerinde bir yenileme yayınlayın:

1. Sunucular > Azure geçişi: Sunucu değerlendirmesi, **Genel Bakış ' a**tıklayın.
2. **Yönet**altında **Aracı durumu** ' ye tıklayın.
3. **Aracıyı yenileme**seçeneğine tıklayın. Bu seçeneği, aracılar listesinin altında görürsünüz.
4. Yenileme işleminin tamamlanmasını bekleyin. Artık sanal makinelerinize yönelik güncel bilgileri görmeniz gerekir.

### <a name="unable-to-connect-to-hosts-or-cluster-as-the-server-name-cannot-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>Sunucu adı çözümlenemediğinden konağa veya kümeye bağlanılamıyor. WinRM hata kodu: 0x803381B9 (Hata KIMLIĞI: 50004)
Bu hata, gereci sunan DNS, verdiğiniz küme veya ana bilgisayar adını çözümleyemediğinde oluşur. Kümede bu hatayı görürseniz, kümenin tam etki alanı adını sağlamayı deneyin.

Bir kümedeki konaklar için de bu hatayı görebilirsiniz. Bu durumda, Gereç kümeye bağlanabilir. Ancak küme, tam etki alanı adları olmayan ana bilgisayar adlarını döndürdü.

Bu hatayı çözmek için, IP adresi ve ana bilgisayar adlarının bir eşlemesini ekleyerek gereç üzerindeki Hosts dosyasını güncelleştirin.
1. Not defteri 'Ni yönetici kullanıcı olarak açın. C:\windows\system32\drivers\etc\hostdosyasını açın.
2. IP adresini ve ana bilgisayar adını bir satıra ekleyin. Bu hatayı gördüğünüz her bir konak veya küme için tekrarlayın.
3. Hosts dosyasını kaydedin ve kapatın.
4. Gerecin gereç Yönetimi uygulamasını kullanarak konaklara bağlanıp bağlanamadığını kontrol edebilirsiniz. 30 dakika sonra, Azure portal bu konaklardaki en son bilgileri görmeniz gerekir.


## <a name="assessment-issues"></a>Değerlendirme sorunları

### <a name="azure-readiness-issues"></a>Azure hazırlık sorunları

Sorun | Düzeltme
--- | ---
Desteklenmeyen önyükleme türü | Azure, EFı önyükleme türü olan VM 'Leri desteklemez. Geçiş çalıştırmadan önce önyükleme türünü BIOS 'a dönüştürmeniz önerilir. <br/><br/>Bu tür sanal makinelerin geçişini yapmak için Azure geçişi sunucu geçişini kullanabilirsiniz. Bu işlem, geçiş sırasında VM 'nin önyükleme türünü BIOS 'a dönüştürecek.
Koşullu olarak desteklenen Windows işletim sistemi | İşletim sistemi destek tarihine geçti ve [Azure 'da destek](https://aka.ms/WSosstatement)için özel bir destek SÖZLEŞMESI (CSA) istiyor ve Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeyi düşünün.
Desteklenmeyen Windows İşletim Sistemi | Azure yalnızca [Seçili Windows işletim sistemi sürümlerini](https://aka.ms/WSosstatement)destekler, Azure 'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeyi düşünün.
Koşullu olarak desteklenen Linux işletim sistemi | Azure yalnızca [Seçili Linux işletim sistemi sürümlerini](../virtual-machines/linux/endorsed-distros.md)onaylıyor, Azure 'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeyi düşünün.
Desteklenmeyen Linux İşletim Sistemi | Makine Azure 'da başlatılabilir, ancak Azure tarafından bir işletim sistemi desteği sağlanmayabilir, Azure 'a geçiş yapmadan önce işletim sistemini [onaylı bir Linux sürümüne](../virtual-machines/linux/endorsed-distros.md) yükseltmeyi düşünün
Bilinmeyen işletim sistemi | Azure geçişi 'nin sanal makinenin Azure hazırlığını tanımlayabileceği için, VM 'nin işletim sistemi vCenter Server ' diğer ' olarak belirtilmiştir. Makineyi geçirmeden önce makine içinde çalışan işletim sisteminin Azure tarafından [desteklendiğinden](https://aka.ms/azureoslist) emin olun.
İşletim sistemi bit genişliği desteklenmiyor | 32 bit işletim sistemine sahip VM 'Ler Azure 'da başlatılabilir, ancak Azure 'a geçiş yapmadan önce VM 'nin işletim sistemini 32 bit 'ten 64 bit sürümüne yükseltmeniz önerilir.
Visual Studio aboneliği gerektirir. | Makinede, yalnızca Visual Studio aboneliğinde desteklenen bir Windows istemci işletim sistemi çalışıyor.
Gerekli depolama performansı için VM bulunamadı. | Makine için gereken depolama performansı (ıOPS/aktarım hızı) Azure VM desteğini aşıyor. Geçişten önce makinenin depolama gereksinimlerini azaltın.
Gerekli ağ performansı için VM bulunamadı. | Makine için gereken ağ performansı (ın/out) Azure VM desteğini aşıyor. Makinenin ağ gereksinimlerini azaltın.
VM belirtilen konumda bulunamadı. | Geçişten önce farklı bir hedef konum kullanın.
Bir veya daha fazla uygun olmayan disk. | VM 'ye bağlı bir veya daha fazla disk, Azure gereksinimlerini karşılamıyor. SANAL makineye bağlı her disk için, diskin boyutunun < 4 TB olduğundan emin olun, aksi takdirde Azure 'a geçirmeden önce disk boyutunu küçültün. Her disk için gereken performansın (ıOPS/aktarım hızı) Azure [yönetilen sanal makine diskleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits)tarafından desteklendiğinden emin olun.   
Bir veya daha fazla uygun olmayan ağ bağdaştırıcısı. | Kullanılmayan ağ bağdaştırıcılarını geçişten önce makineden kaldırın.
Disk sayısı, sınırı aşıyor | Kullanılmayan diskleri geçişten önce makineden kaldırın.
Disk boyutu, sınırı aşıyor | Azure, boyutu 4 TB olan diskleri destekler. Geçiş işleminden önce diskleri 4 TB 'den daha az olacak şekilde daraltın.
Disk, belirtilen konumda kullanılamıyor | Geçirmeden önce diskin hedef konumunda olduğundan emin olun.
Disk, belirtilen çoğaltma için kullanılamıyor | Disk, değerlendirme ayarlarında tanımlanan artıklık depolama türünü kullanmalıdır (varsayılan olarak LRS).
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdeklere ve belleğe sahip VM bulunamadı | Azure uygun bir VM türü bulamadı. Geçirmeden önce şirket içi makinenin bellek ve çekirdek sayısını azaltın.
Bir iç hata nedeniyle VM uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla disk için uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısının uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.

### <a name="i-am-unable-to-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>Değerlendirme özelliklerinde Azure teklifi olarak Kurumsal Anlaşma (EA) belirtmem istemiyorum mi?
Azure Geçişi: Sunucu Değerlendirmesi şu anda Kurumsal Anlaşma (EA) tabanlı fiyatlandırmayı desteklemez. Bu sorunun geçici çözümü Azure teklifi olarak ‘Kullandıkça Öde’ teklifini kullanmak ve ‘İndirim’ özelliğiyle sahip olduğunuz özel indirimleri belirtmektir. [Değerlendirmeyi özelleştirme hakkında daha fazla bilgi edinin](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready-is-there-anything-i-need-to-do-to-fix-this"></a>Sunucu değerlendirmesi neden Linux sanal makinelerimi ' koşullu olarak hazırlanıyor ' olarak işaretler. Bunu çözmem gereken bir şey var mı?
Sunucu Değerlendirmesi'nde şirket içi VM'lere yüklenen Linux işletim sisteminin ikincil sürümünün algılanamamasına yol açan, bilinen bir açık vardır (örneğin RHEL 6.10, için Sunucu Değerlendirmesi şu anda yalnızca işletim sistemi sürümü olarak RHEL 6'yı algılar). Azure Linux'ın yalnızca belirli sürümlerini onayladığından, Linux VM'leri şu anda Sunucu Değerlendirmesi'nde koşullu olarak hazır şeklinde işaretlenir. [Azure Linux destek belgelerini](https://aka.ms/migrate/selfhost/azureendorseddistros)inceleyerek, ŞIRKET içi VM üzerinde çalışan Linux Işletim sisteminin Azure 'da olup olmadığını el ile sağlayabilirsiniz. Desteklenen dağıtımı doğruladıktan sonra bu uyarıyı yoksayabilirsiniz.

### <a name="the-vm-sku-recommended-by-server-assessment-has-more-number-of-cores-and-a-larger-memory-size-than-what-is-allocated-on-premises-why-is-that-so"></a>Sunucu değerlendirmesi tarafından önerilen VM SKU 'SU daha fazla sayıda çekirdeğe ve şirket içinde ayrıldıkdan daha büyük bir bellek boyutuna sahiptir. Bunun nedeni nedir?
Sunucu Değerlendirmesi'nde VM SKU'su önerisi, değerlendirme özelliklerine bağlıdır. Sunucu Değerlendirmesi'nde iki tür değerlendirme ('performans tabanlı' ve 'şirket içi olarak') oluşturabilirsiniz. Performans tabanlı değerlendirmelere yönelik sunucu değerlendirmesi, şirket içi sanal makinelerinize yönelik doğru hedef VM SKU 'sunu belirlemede şirket içi VM 'lerin (CPU, bellek, disk ve ağ kullanımı) kullanım verilerini dikkate alır. Buna ek olarak, performans tabanlı boyutlandırma için etkili kullanım belirlenirken konfor katsayısı hesaba katılır. Şirket içi olarak boyutlandırma için performans verileri dikkate alınmaz ve şirket içinde neyin ayrıldığına bağlı olarak bir hedef SKU önerilir.

Örneğin, 4 çekirdek ve 8 GB bellek içeren ve% 50 CPU kullanımı ve% 50 bellek kullanımı ile bir şirket içi VM olduğunu ve değerlendirmede bir rahat1,3 lığını belirtelim. Değerlendirmede boyutlandırma ölçütü ' Şirket içi olarak ' ise, 4 çekirdeğe sahip bir Azure VM SKU 'SU ve 8 GB bellek kullanılması önerilir; ancak, boyutlandırma ölçütü, etkin CPU ve bellek kullanımına göre (% 50, 4 çekirdek * 1,3 = 2,6 çekirdekleri ve 8 GB% 50) temel alınarak performans tabanlıdır. bellek * 1,3 = 5,3-GB bellek), dört çekirdekli sanal makine SKU 'SU (en yakın desteklenen çekirdek sayısı) ve 8 GB bellek boyutu (en yakın desteklenen bellek boyutu) önerilir. [Sunucu Değerlendirmesi'nin boyutlandırmayı nasıl yaptığı hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

### <a name="the-disk-sku-recommended-by-server-assessment-has-a-bigger-size-than-what-is-allocated-on-premises-why-is-that-so"></a>Sunucu değerlendirmesi tarafından önerilen disk SKU 'SU, şirket içinde ayrıldıkdakinden daha büyük bir boyuta sahiptir. Bunun nedeni nedir?
Sunucu Değerlendirmesi'nde disk boyutlandırma iki değerlendirme özelliğine dayanır: boyutlandırma ölçütü ve depolama türü. Boyutlandırma ölçütü ' performans tabanlı ' ve depolama türü ' otomatik ' olarak ayarlandıysa, diskin ıOPS ve aktarım hızı değerleri, hedef disk türünü (Standart HDD, Standart SSD veya Premium diskler) belirlemek için kabul edilir. Şirket içi diskinin boyut gereksinimleri göz önünde bulundurularak disk türü içinde bir disk SKU'su önerilir. Boyutlandırma ölçütü ' performans tabanlı ' ve depolama türü ' Premium ' ise, şirket içi diskin ıOPS, aktarım hızı ve Boyut gereksinimlerine bağlı olarak Azure 'da bir Premium disk SKU 'SU önerilir. Boyutlandırma ölçütü 'Şirket içi olarak' boyutlandırma ve depolama türü 'Standart HHD', 'Standart SSD' veya 'Premium' olduğunda disk boyutlandırması için aynı mantık kullanılır.

Örneğin, 32 GB bellek içeren bir şirket içi diskiniz varsa, ancak diskin toplanmış okuma ve yazma ıOPS değeri 800 ıOPS ise, sunucu değerlendirmesi bir Premium disk türü önerir (daha yüksek ıOPS gereksinimleri nedeniyle) ve ardından bir disk SKU 'SU önerilir ve bu işlem gerekli ıOPS ve boyut. Bu örnekteki en yakın eşleşme P15 (256 GB, 1100 IOPS) olabilir. Dolayısıyla şirket içi diski için gereken boyut 32 GB olsa bile Sunucu Değerlendirmesi şirket içi diskinin yüksek IOPS gereksinimine bağlı olarak daha büyük boyutlu bir disk önerirdi.

### <a name="why-does-my-assessment-report-say-percentageofcoresutilizedmissing-or-percentageofmemoryutilizedmissing-for-some-vms"></a>Neden değerlendirme raporum bazı VM 'Ler için ' PercentageOfCoresUtilizedMissing ' veya ' PercentageOfMemoryUtilizedMissing ' söylemi?
Yukarıdaki sorunlar, Azure geçişi gereci şirket içi VM 'Ler için performans verilerini toplayamadığınızda listelenir. Bu durum, bir sanal makinenin kapatıldığı zaman bir VM için performans verilerini toplayamadığı için, VM 'Lerin değerlendirme oluşturduğunuz süre (son bir gün/bir hafta/bir ay) için kapalı olması durumunda gerçekleşebilir. Yalnızca bellek sayaçları eksikse ve Hyper-V VM 'lerini değerlendirmeye çalışıyorsanız, bu VM 'lerde Dinamik belleğin etkinleştirilip etkinleştirilmediğini denetleyin. Şu anda Azure geçiş gerecinin, dinamik belleği etkinleştirilmemiş VM 'Ler için bellek kullanımı toplayamadığından dolayı bilinen bir sorun var. Sorunun yalnızca Hyper-V VM 'Leri için olduğunu ve VMware VM 'leri için mevcut olmadığını unutmayın. Performans sayaçlarından herhangi biri eksikse, Azure geçişi: Sunucu değerlendirmesi, ayrılmış çekirdekler/belleğe geri döner ve buna uygun bir VM boyutu önerir.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>Sunucu değerlendirmesi tarafından tahmin edilen Işlem maliyetine dahil edilen VM 'nin işletim sistemi lisans maliyeti midir?
Sunucu Değerlendirmesi şu anda yalnızca Windows makinelerinin işletim sistemi lisans maliyetini hesaba katar; Linux makinelerinin işletim sistemi lisans maliyeti şu anda dikkate alınmaz.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Performans geçmişi ve yüzdebirlik kullanımı, boyut önerilerinde ne etkiler?
Bu özellikler yalnızca 'Performans tabanlı' boyutlandırma için geçerlidir. Sunucu Değerlendirmesi şirket içi makinelerin performans verilerini sürekli toplar ve bunları Azure’da VM SKU'su ile disk SKU'su önermek için kullanır. Aşağıda Sunucu Değerlendirmesi tarafından performans verilerinin nasıl toplandığı gösterilir:
- Azure geçişi gereci, VMware VM 'leri için her 20 saniyede bir ve Hyper-V VM 'Leri için 30 saniyede bir gerçek zamanlı kullanım verileri toplamak üzere şirket içi ortamı sürekli olarak profiller.
- Alet, her 10 dakikada bir tek veri noktası oluşturmak için 20/30 saniyelik örnekler toplar. Tek veri noktasını oluşturmak için alet tüm 20/30 saniyelik örneklerden en yüksek değerleri seçer ve Azure’a gönderir.
- Sunucu Değerlendirmesi'nde bir değerlendirme oluşturduğunuzda, performans süresi ve performans geçmişi yüzdebirlik değerine bağlı olarak temsili kullanım değeri belirlenir. Örneğin, performans geçmişi bir hafta ise ve yüzdebirlik kullanımı 80 ' dir. Azure geçişi, son bir hafta için 10 dakikalık tüm örnek noktalarını artan düzende sıralar ve ardından temsili değeri olarak 95. yüzdebirlik değerini seçer.
95. yüzdebirlik değeri, 99. yüzdebirliği seçtiğinizde eklenebilecek aykırı değerleri görmezden gelmenizi sağlar. Dönemin en yüksek kullanımını seçmek ve aykırı değerleri kaçırmamak istiyorsanız, yüzdebirlik kullanımı olarak 99. yüzdebirliği seçmelisiniz.

## <a name="dependency-visualization-issues"></a>Bağımlılık görselleştirme sorunları

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Azure Kamu projeleri için bağımlılık görselleştirme işlevini bulamıyorum.

Azure geçişi, bağımlılık görselleştirme işlevselliği için Hizmet Eşlemesi bağımlıdır ve Hizmet Eşlemesi Şu anda Azure Kamu 'da kullanılamadığından, bu işlev Azure Kamu 'da kullanılamaz.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Şirket içi sanal makinelerime Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükledim, ancak bağımlılıklar artık Azure geçişi portalında gösteriliyor.

Aracıları yükledikten sonra, Azure geçişi genellikle portalda bağımlılıkları göstermek için 15-30 dakika sürer. 30 dakikadan uzun süre beklediğinizden, aşağıdaki adımları izleyerek MMA aracısının OMS çalışma alanıyla iletişim kurabildiğinden emin olun:

Windows VM için:
1. **Denetim Masası** ' na gidin ve **Microsoft Monitoring Agent**başlatın.
2. MMA özellikleri açılır penceresinde **Azure Log Analytics (OMS)** sekmesine gidin.
3. Çalışma alanı **durumunun** yeşil olduğundan emin olun.
4. Durum yeşil değilse, çalışma alanını kaldırmayı ve onu yeniden eklemeyi deneyin.
        ![MMA durumu](./media/troubleshooting-general/mma-status.png)

Linux VM için, MMA ve bağımlılık aracısının yükleme komutlarının başarılı olduğundan emin olun.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA tarafından desteklenen işletim sistemleri nelerdir?

MMA tarafından desteklenen Windows işletim sistemlerinin listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)verilmiştir.
MMA tarafından desteklenen Linux işletim sistemlerinin listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Bağımlılık Aracısı tarafından desteklenen işletim sistemleri nelerdir?

Bağımlılık Aracısı tarafından desteklenen Windows işletim sistemlerinin listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Bağımlılık Aracısı tarafından desteklenen Linux işletim sistemlerinin listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Azure geçişi 'ndeki bağımlılıkları bir saatlik süre boyunca görselleştirilemiyor musunuz?
Azure geçişi, bağımlılıkları bir saatlik süreye kadar görselleştirmenize imkan tanır. Azure geçişi, geçmişteki en son bir aya kadar belirli bir tarihe kadar geri dönmenize izin veriyor, ancak bağımlılıklarını görselleştirmeniz için en uzun süre 1 saate kadar. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresi işlevini kullanabilirsiniz, ancak yalnızca bir saatlik pencere için görüntüleyebilir. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>10 ' dan fazla VM 'ye sahip gruplar için bağımlılıklar görselleştirilemiyor mu?
10 adede kadar VM 'ye sahip olan [gruplar için bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) , 10 ' dan fazla VM içeren bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve bağımlılıkları görselleştirmeniz önerilir.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Aracıları yükledim ve gruplar oluşturmak için bağımlılık görselleştirmesini kullandınız. Artık yük devretme sonrası, makineler "bağımlılıkları görüntüle" yerine "aracıyı yüklemeyi" gösterir
* Planlı veya planlanmamış yük devretme sonrası, şirket içi makineler kapalıdır ve eşdeğer makineler Azure 'da sona erdirir. Bu makineler farklı bir MAC adresi edintir. Kullanıcının şirket içi IP adresini tutmayı tercih etmeksizin, farklı bir IP adresi elde edebilirler. MAC ve IP adreslerinden her ikisi de farklıysa Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez ve kullanıcıdan bağımlılıkları görüntülemek yerine aracıları yüklemesini ister.
* Yük devretme testi sonrası, şirket içi makineler beklendiği gibi açık kalır. Azure 'da bulunan eşdeğer makineler, farklı bir MAC adresi elde edebilir ve farklı IP adresleri alabilir. Kullanıcı giden Azure Izleyici günlükleri trafiğini bu makinelerden engellediğinde, Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez ve kullanıcıdan bağımlılıkları görüntülemek yerine aracıları yüklemesini ister.

## <a name="collect-azure-portal-logs"></a>Azure portal günlüklerini topla

**Azure portal ağ trafiği günlükleri Nasıl yaparım? toplansın mı?**

1. Tarayıcıyı açın ve [portalda](https://portal.azure.com)oturum açın.
2. Geliştirici Araçları başlatmak için F12 tuşuna basın. Gerekirse, **gezintide girdileri temizle**ayarını temizleyin.
3. **Ağ** sekmesine tıklayın ve ağ trafiğini yakalamaya başlayın:
   - Chrome 'da **günlüğü koru**' yı seçin. Kayıt otomatik olarak başlamalıdır. Kırmızı bir daire trafiğin yakalandığını belirtir. Görünmüyorsa, başlamak için siyah daireye tıklayın.
   - Microsoft Edge/IE 'de kayıt otomatik olarak başlamalıdır. Aksi takdirde, yeşil oynatma düğmesine tıklayın.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kayıt sırasında hata ile karşılaşduktan sonra kaydı durdurun ve kayıtlı etkinliğin bir kopyasını kaydedin:
   - Chrome 'da, sağ tıklayın ve **IÇERIKLE har olarak kaydet**' e tıklayın. Bu, günlükleri bir. har dosyası olarak sıkıştırır ve dışarı aktarır.
   - Microsoft Edge/IE 'de **yakalanan trafiği dışarı aktar** simgesine tıklayın. Bu, günlüğü sıkıştırır ve dışarı aktarır.
6. Herhangi bir uyarı veya hata olup olmadığını denetlemek için **konsol** sekmesine gidin. Konsol günlüğünü kaydetmek için:
   - Chrome 'da konsol günlüğünde herhangi bir yere sağ tıklayın. Günlüğe **Kaydet**, dışarı aktarmak için ve günlüğü posta ' yı seçin.
   - Microsoft Edge/IE 'de hatalara sağ tıklayıp **Tümünü Kopyala**' yı seçin.
7. Geliştirici Araçları kapatın.
