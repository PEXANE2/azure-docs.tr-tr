---
title: Aracısız VMware VM geçişinde çoğaltma sorunlarını giderme
description: Çoğaltma çevrimi hatalarıyla ilgili yardım alın
author: anvar-ms
ms.manager: bsiva
ms.author: anvar
ms.topic: troubleshooting
ms.date: 08/17/2020
ms.openlocfilehash: a1ef0e9fe3805f1c6d4d1000a9ea70accc64f4d2
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88718705"
---
# <a name="troubleshooting-replication-issues-in-agentless-vmware-vm-migration"></a>Aracısız VMware VM geçişinde çoğaltma sorunlarını giderme

Bu makalede, Azure geçişi: sunucu geçişi aracısız yöntemi kullanarak şirket içi VMware VM 'lerini çoğaltırken karşılaşabileceğiniz bazı yaygın sorunlar ve belirli hatalar açıklanır.

Aracısız çoğaltma yöntemini kullanarak bir VMware sanal makinesini çoğalttığınızda, sanal makine&#39;s diskler (VMDK) verileri Azure aboneliğinizdeki çoğaltma tarafından yönetilen disklere çoğaltılır. Bir VM için çoğaltma başladığında, disklerin tam kopyalarının çoğaltılacağı bir ilk çoğaltma çevrimi meydana gelir. İlk çoğaltma tamamlandıktan sonra, önceki çoğaltma döngüsünden bu yana gerçekleşen tüm değişiklikleri aktarmak için artımlı çoğaltma döngüleri düzenli aralıklarla zamanlanır.

Bazen bir VM için çoğaltma döngüleri başarısız olabilir. Bu arızalar, şirket içi ağ yapılandırmasındaki sorunlardan Azure geçişi bulut hizmeti arka ucunda sorunlara neden olmuş olabilir. Bu makalede şunları göndereceğiz:

 - Çoğaltma durumunu nasıl izleyeceğinizi ve hataları nasıl çözekullanabileceğinizi gösterir.
 - Yaygın olarak oluşan bazı çoğaltma hatalarını listeleyin ve bunları düzeltmek için ek adımlar önerin.

## <a name="monitor-replication-status-using-the-azure-portal"></a>Azure portal kullanarak çoğaltma durumunu izleme

Sanal makineleriniz için çoğaltma durumunu izlemek üzere aşağıdaki adımları kullanın:

  1. Azure portal Azure geçişi ' nde sunucular sayfasına gidin.
  2. Sunucu geçiş kutucuğunda "sunucuları çoğaltma" seçeneğine tıklayarak "makineler çoğaltılıyor" sayfasına gidin.
  3. Durum, sağlık, son eşitleme saati vb. gibi ek bilgilerle birlikte çoğaltma sunucularının bir listesini görürsünüz. Sistem durumu sütunu, sanal makinenin geçerli çoğaltma durumunu gösterir. Sağlık sütunundaki bir ' CriticalHandle ' veya ' Warning ' değeri, genellikle VM için önceki çoğaltma döngüsünün başarısız olduğunu gösterir. Daha fazla ayrıntı edinmek için VM 'ye sağ tıklayın ve "hata ayrıntıları" nı seçin. Hata ayrıntıları sayfası hata hakkındaki bilgileri ve sorun giderme hakkında ek ayrıntıları içerir. Ayrıca, VM için Olaylar sayfasına gitmek üzere kullanılabilecek bir "son olaylar" bağlantısı görürsünüz.
  4. VM için önceki çoğaltma çevrimi başarısızlıklarını görmek üzere "son olaylar" a tıklayın. Olaylar sayfasında, sanal makine için "çoğaltma çevrimi başarısız oldu" veya "disk için çoğaltma çevrimi başarısız oldu" türündeki en son olayı bulun.
  5. Hatanın olası nedenlerini ve önerilen düzeltme adımlarını anlamak için olaya tıklayın. Sorunu gidermek için belirtilen bilgileri kullanın ve hatayı düzeltin.
    
## <a name="common-replication-errors"></a>Ortak çoğaltma hataları

Bu bölümde, yaygın hatalardan bazıları ve bunların nasıl giderebileceğiniz açıklanır.

## <a name="disposeartefactstimedout"></a>DisposeArtefactsTimedOut

**Hata kimliği:** 181008

**Hata iletisi:** VM: VMName. Hata: & ' [' Gateway. Service. StateMachine. SnapshotReplication. SnapshotReplicationEngine + WaitingForArtefactsDisposalPreCycle ' (' WaitingForArtefactsDisposalPreCycle ')] ' durumundaki ' DisposeArtefactsTimeout ' zaman aşımı olayı Ile karşılaşıldı.

**Olası nedenler:**

Verileri Azure 'a çoğaltmaya çalışan bileşen çalışmıyor veya yanıt vermiyor. Olası nedenler şunlardır:

- Azure geçişi gereci 'nda çalışan ağ geçidi hizmeti çalışmıyor.
- Ağ geçidi hizmeti Service Bus/Olay Hub 'ı/gereç depolama hesabına yönelik bağlantı sorunlarıyla karşılaşıyor.

**DisposeArtefactsTimedOut ve ilgili çözümlemenin tam nedenini belirleme:**

1. Azure Geçişi aletinin açık ve çalışır durumda olduğundan emin olun.
2. Gereç üzerinde Ağ Geçidi hizmetinin çalışıp çalışmadığını denetleyin:
   1.  Uzak Masaüstü kullanarak Azure geçiş gereci 'nda oturum açın ve aşağıdakileri yapın.

   2.  Microsoft Hizmetleri MMC ek bileşenini açın (> Services. msc ' yi çalıştırın) ve "Microsoft Azure ağ geçidi hizmeti" nin çalışır durumda olup olmadığını denetleyin. Hizmet durdurulmuşsa veya çalışmıyorsa, hizmeti başlatın. Alternatif olarak, komut istemi veya PowerShell 'i açabilir ve şunları yapabilirsiniz: "net start asrgwy"

3. Azure geçiş gereci ve gereç depolama hesabı arasındaki bağlantı sorunlarını denetleyin: 

    Azure geçişi gereci AzCopy indirdikten sonra aşağıdaki komutu çalıştırın:
    
    _AzCopy tezgahtır https://[hesap]. blob. Core. Windows. net/[Container]? 'LARıNıN_
    
    **Performans kıyaslama testini çalıştırma adımları:**
    
      1. AzCopy [indirin](https://go.microsoft.com/fwlink/?linkid=2138966)
        
      2. Kaynak grubundaki gereç depolama hesabını bulun. Depolama hesabı, migrategwsa öğesine benzeyen bir ada sahiptir \* \* \* \* \* \* \* \* \* \* . Bu, Yukarıdaki komutta [account] parametresinin değeridir.
        
      3. Azure portal depolama hesabınızı arayın. Arama yapmak için kullandığınız aboneliğin, depolama hesabının oluşturulduğu abonelik (hedef abonelik) olduğundan emin olun. Blob hizmeti bölümünde kapsayıcılar ' a gidin. + Kapsayıcı ' ya tıklayın ve bir kapsayıcı oluşturun. Genel erişim düzeyini varsayılan seçili değere bırakın.
        
      4. Ayarlar altında paylaşılan erişim Imzasına gidin. "Izin verilen kaynak türü" içinde kapsayıcı seçin. SAS ve bağlantı dizesi oluştur ' a tıklayın. SAS değerini kopyalayın.
        
      5. Hesap, kapsayıcı, SAS değerlerini sırasıyla 2, 3 ve 4. adımlarda elde edilen değerlerle değiştirerek komut Isteminde yukarıdaki komutu yürütün.
        
      Alternatif olarak, Azure Storage 'ı gerecle [indirin](https://go.microsoft.com/fwlink/?linkid=2138967) ve depolama hesaplarına YAKLAŞıK 64 MB olan 10 blobu yüklemeyi deneyin. Sorun yoksa yükleme başarılı olmalıdır.
        
    **Çözüm:** Bu test başarısız olursa, bir ağ sorunu&#39;. Bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.
    
4.  Azure geçişi gereci ve Service Bus arasında bağlantı sorunları olup olmadığını denetleyin:

    Bu test, Azure geçişi gerecinin Azure geçişi bulut hizmeti arka ucu ile iletişim kurabildiğini denetler. Gereç, Service Bus ve Olay Hub 'ı ileti kuyrukları aracılığıyla hizmet arka ucuyla iletişim kurar. Gerecden Service Bus bağlantıyı doğrulamak için Service Bus Gezginini [indirin](https://go.microsoft.com/fwlink/?linkid=2139104) , Gereç Service Bus bağlanmayı ve ileti gönder/al iletisi gerçekleştirmeyi deneyin. Sorun yoksa, bu başarılı olmalıdır.

    **Testi çalıştırma adımları:**

    1. Bağlantı dizesini geçirme projesinde oluşturulan Service Bus kopyalayın
    2. Service Bus Gezginini açın
    3. Dosyaya git ve Bağlan
    4. Bağlantı dizesini yapıştırın ve Bağlan ' a tıklayın.
    5. Bu, Service Bus ad alanını açar
    6. Konusunda Snapshot Manager ' yi seçin. Snapshot Manager sağ tıklayın, "Iletileri al" seçeneğini belirleyin > "göz at" seçeneğini belirleyip Tamam ' a tıklayın.
    7. Bağlantı başarılı olursa, konsol çıkışında "[x] ileti alındı" iletisini görürsünüz. Bağlantı başarılı olmazsa, bağlantının başarısız olduğunu belirten bir ileti görürsünüz
    
    **Çözüm:** Bu test başarısız olursa bir ağ sorunu vardır. Bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.

5. Azure geçişi gereci ve Azure Key Vault arasındaki bağlantı sorunları:

    Bu test, Azure geçişi gereci ve Azure Key Vault arasındaki bağlantı sorunlarını denetler. Key Vault, çoğaltma için kullanılan depolama hesabı erişimini yönetmek için kullanılır.
    
    **Bağlantıyı denetleme adımları:**
    
    1. Azure geçişi projesine karşılık gelen kaynak grubundaki kaynak listesinden Key Vault URI 'sini getirin.
    
    1. Azure geçiş gereci 'nda PowerShell 'i açın ve aşağıdaki komutu çalıştırın:
    
    _test-NetConnection Key Vault URI-P 443_
    
    Bu komut, bir TCP bağlantısı dener ve bir çıkış döndürür.
    
     - Çıktıda "_Tcptestsucceeded_" alanını denetleyin. Değer "_true_" Ise, Azure geçişi gereci ve Azure Key Vault arasında bir bağlantı sorunu yoktur. Değer "false" ise, bir bağlantı sorunu vardır.
    
    **Çözüm:** Bu test başarısız olursa, Azure geçişi gereci ve Azure Key Vault arasında bir bağlantı sorunu var. Bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.
    
## <a name="diskuploadtimedout"></a>Diskuploadtimebir zaman aşımına uğradı

**Hata kimliği:** 1011

**Hata iletisi:** Disk DiskPath, DiskID, sanal makine için veri yükleme, VMName; VMID beklenen süre içinde tamamlanmadı.

Bu hata genellikle çoğaltmayı gerçekleştiren Azure geçişi gerecinin Azure Cloud Services bağlanamadığını veya çoğaltma döngüsünün zaman aşımına geçmesine neden olan çoğaltmanın yavaş ilerlediğini gösterir.

Olası nedenler şunlardır:

- Azure geçişi gereci çalışmıyor.
- Gereç üzerindeki çoğaltma ağ geçidi hizmeti çalışmıyor.
- Çoğaltma ağ geçidi hizmeti, çoğaltma için kullanılan şu Azure hizmeti bileşenlerinden birine yönelik bağlantı sorunları yaşıyor: Service Bus/Olay Hub 'ı/Azure önbellek depolama hesabı/Azure Key Vault.
- Disk okunmaya çalışılırken ağ geçidi hizmeti vCenter düzeyinde kısıtlanıyor.

**Kök nedeni tanımlama ve sorunu çözme:**

1. Azure Geçişi aletinin açık ve çalışır durumda olduğundan emin olun.
2. Gereç üzerinde Ağ Geçidi hizmetinin çalışıp çalışmadığını denetleyin:
   1.  Uzak Masaüstü kullanarak Azure geçiş gereci 'nda oturum açın ve aşağıdakileri yapın.

   2.  Microsoft Hizmetleri MMC ek bileşenini açın (> Services. msc ' yi çalıştırın) ve "Microsoft Azure ağ geçidi hizmeti" nin çalışır durumda olup olmadığını denetleyin. Hizmet durdurulmuşsa veya çalışmıyorsa, hizmeti başlatın. Alternatif olarak, komut istemi veya PowerShell 'i açabilir ve şunları yapabilirsiniz: "net start asrgwy".


3. **Azure geçişi gereci ile önbellek depolama hesabı arasındaki bağlantı sorunlarını denetleyin:** 

    Azure geçişi gereci AzCopy indirdikten sonra aşağıdaki komutu çalıştırın:
    
    _AzCopy tezgahtır https://[hesap]. blob. Core. Windows. net/[Container]? 'LARıNıN_
    
    **Performans kıyaslama testini çalıştırma adımları:**
    
      1. AzCopy [indirin](https://go.microsoft.com/fwlink/?linkid=2138966)
        
      2. Kaynak grubundaki gereç depolama hesabını bulun. Depolama hesabı, migratelsa benzer bir ada sahiptir \* \* \* \* \* \* \* \* \* \* . Bu, Yukarıdaki komutta [account] parametresinin değeridir.
        
      3. Azure portal depolama hesabınızı arayın. Arama yapmak için kullandığınız aboneliğin, depolama hesabının oluşturulduğu abonelik (hedef abonelik) olduğundan emin olun. Blob hizmeti bölümünde kapsayıcılar ' a gidin. + Kapsayıcı ' ya tıklayın ve bir kapsayıcı oluşturun. Genel erişim düzeyini varsayılan seçili değere bırakın.
        
      4. Ayarlar altında paylaşılan erişim Imzasına gidin. "Izin verilen kaynak türü" içinde kapsayıcı seçin. SAS ve bağlantı dizesi oluştur ' a tıklayın. SAS değerini kopyalayın.
        
      5. Hesap, kapsayıcı, SAS değerlerini sırasıyla 2, 3 ve 4. adımlarda elde edilen değerlerle değiştirerek komut Isteminde yukarıdaki komutu yürütün.
        
      Alternatif olarak, Azure Storage 'ı gerecle [indirin](https://go.microsoft.com/fwlink/?linkid=2138967) ve depolama hesaplarına YAKLAŞıK 64 MB olan 10 blobu yüklemeyi deneyin. Sorun yoksa yükleme başarılı olmalıdır.
        
    **Çözüm:** Bu test başarısız olursa, bir ağ sorunu&#39;. Bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.
                
4.  **Azure geçişi gereci ve Azure Service Bus arasındaki bağlantı sorunları:**

    Bu test, Azure geçişi gerecinin Azure geçişi bulut hizmeti arka ucu ile iletişim kurup kuramayacağını kontrol eder. Gereç, Service Bus ve Olay Hub 'ı ileti kuyrukları aracılığıyla hizmet arka ucuyla iletişim kurar. Gerecden Service Bus bağlantıyı doğrulamak için Service Bus Gezginini [indirin](https://go.microsoft.com/fwlink/?linkid=2139104) , Gereç Service Bus bağlanmayı ve ileti gönder/al iletisi gerçekleştirmeyi deneyin. Sorun yoksa, bu başarılı olmalıdır.

    **Testi çalıştırma adımları:**
    
    1. Bağlantı dizesini Azure geçişi projesine karşılık gelen kaynak grubunda oluşturulan Service Bus kopyalayın
    
    1. Service Bus Gezginini aç
    
    1. Dosya > Bağlan 'a git
    
    1. 1. adımda kopyaladığınız bağlantı dizesini yapıştırın ve Bağlan ' a tıklayın.
    
    1. Bu, Service Bus ad alanını açar.
    
    1. Ad alanındaki konusunun Snapshot Manager seçin. Snapshot Manager sağ tıklayın, "Iletileri al" seçeneğini belirleyin > "göz at" seçeneğini belirleyip Tamam ' a tıklayın.
    
    Bağlantı başarılı olursa, konsol çıkışında "[x] ileti alındı" iletisini görürsünüz. Bağlantı başarılı olmazsa, bağlantının başarısız olduğunu belirten bir ileti görürsünüz.
    
    **Çözüm:** Bu test başarısız olursa, Azure geçişi gereci ve Service Bus arasında bir bağlantı sorunu var. Bu bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.
    
 5. **Azure geçişi gereci ve Azure Key Vault arasındaki bağlantı sorunları:**

    Bu test, Azure geçişi gereci ve Azure Key Vault arasındaki bağlantı sorunlarını denetler. Key Vault, çoğaltma için kullanılan depolama hesabı erişimini yönetmek için kullanılır.
    
    **Bağlantıyı denetleme adımları:**
    
    1. Azure geçişi projesine karşılık gelen kaynak grubundaki kaynak listesinden Key Vault URI 'sini getirin.
    
    1. Azure geçiş gereci 'nda PowerShell 'i açın ve aşağıdaki komutu çalıştırın:
    
    _test-NetConnection Key Vault URI-P 443_
    
    Bu komut, bir TCP bağlantısı dener ve bir çıkış döndürür.
    
    1. Çıktıda "_Tcptestsucceeded_" alanını denetleyin. Değer "_true_" Ise, Azure geçişi gereci ve Azure Key Vault arasında bir bağlantı sorunu yoktur. Değer "false" ise, bir bağlantı sorunu vardır.
    
    **Çözüm:** Bu test başarısız olursa, Azure geçişi gereci ve Azure Key Vault arasında bir bağlantı sorunu var. Bağlantı sorunlarını kontrol etmek için yerel ağ ekibinize katılın. Genellikle, hatalara neden olan bazı güvenlik duvarı ayarları olabilir.
    
## <a name="encountered-an-error-while-trying-to-fetch-changed-blocks"></a>Değiştirilen bloklar getirilmeye çalışılırken bir hatayla karşılaşıldı

Hata Iletisi: ' değişiklik blokları getirilmeye çalışılırken bir hatayla karşılaşıldı '

Aracısız çoğaltma yöntemi, verileri Azure 'a çoğaltmak için VMware 'nin değiştirilmiş blok izleme teknolojisini (CBT) kullanır. CBT, sunucu geçiş aracının yalnızca son çoğaltma döngüsünden bu yana değişmiş olan blokları izlemesini ve çoğaltılmasını sağlar. Çoğaltılan sanal makinede değişen blok izlemesi sıfırlanırsa veya değişen blok izleme dosyası bozulursa bu hata oluştur.

Bu hata aşağıdaki iki şekilde çözülebilir:

- VM çoğaltmasını tetiklendiğinde "Evet" seçeneğini belirleyerek "çoğaltmayı otomatik olarak Onar" seçeneğini tercih ettiyseniz, araç sizin için onarmaya çalışır. VM 'ye sağ tıklayın ve "çoğaltmayı Onar" ı seçin.
- "Çoğaltmayı otomatik olarak Onar" seçeneğini kabul etmediyseniz veya yukarıdaki adım sizin için işe çalışmadıysanız, sanal makine için çoğaltmayı durdurun, sanal makinede [değiştirilen blok izlemeyi sıfırlayın](https://go.microsoft.com/fwlink/?linkid=2139203) ve sonra çoğaltmayı yeniden yapılandırın.

VMware vSphere 5,5 ' de sanal makinenin bir CBT sıfırlamasına neden olabilecek bilinen bir sorun, VMware KB 2048201 ' de açıklanmıştır: vSphere 5. x içindeki bir Depolama vMotion işleminden sonra [değiştirilen blok izleme](https://go.microsoft.com/fwlink/?linkid=2138888) sıfırlanır. VMware vSphere 5.5 kullanıyorsanız bu Bilgi Bankası makalesinde açıklanan güncelleştirmeleri uyguladığınızdan emin olun.

Alternatif olarak, VMware PowerCLI kullanarak bir sanal makinede VMware değiştirilmiş blok izlemeyi sıfırlayabilirsiniz.

## <a name="an-internal-error-occurred"></a>Bir iç hata oluştu

Bazen VMware ortamındaki/API 'deki sorunlar nedeniyle oluşan bir hatayla karşılaşabilirsiniz. VMware ortamıyla ilgili hatalar olarak aşağıdaki hata kümesini belirledik. Bu hataların sabit bir biçimi vardır.

_Hata Iletisi: bir iç hata oluştu. [Hata iletisi]_

Örneğin: hata Iletisi: bir iç hata oluştu. [Geçersiz bir anlık görüntü yapılandırması algılandı].

Aşağıdaki bölümde, yaygın olarak görülen bazı VMware hataları ve bunların nasıl azaltılacağını listelenmektedir.

### <a name="error-message-an-internal-error-occurred-server-refused-connection"></a>Hata Iletisi: bir iç hata oluştu. [Sunucu bağlantıyı reddetti]

Sorun bilinen bir VMware sorunudur ve VDDK 6,7 ' de gerçekleşir. Azure geçişi gereci üzerinde çalışan ağ geçidi hizmetini durdurmanız, [VMware KB 'den bir güncelleştirme indirmeniz](https://go.microsoft.com/fwlink/?linkid=2138889)ve Ağ Geçidi hizmetini yeniden başlatmanız gerekir.

Ağ Geçidi hizmetini durdurma adımları:

1. Windows + R tuşlarına basın, Services. msc ' yi açın. "Microsoft Azure ağ geçidi hizmeti" ne tıklayın ve uygulamayı durdurun.
2. Alternatif olarak, komut istemi veya PowerShell 'i açabilir ve şunları yapabilirsiniz: net stop asrgwy. Hizmetin artık çalışmadığını lütfen beklemediğinizden emin olun.

Ağ Geçidi hizmetini başlatma adımları:

1. Windows + R tuşlarına basın, Services. msc ' yi açın. "Ağ geçidi hizmeti" Microsoft Azure sağ tıklayın ve başlatın.
2. Alternatif olarak, komut istemi veya PowerShell 'i açabilir ve şunları yapabilirsiniz: net start asrgwy.

### <a name="error-message-an-internal-error-occurred-an-invalid-snapshot-configuration-was-detected"></a>Hata Iletisi: bir iç hata oluştu. [' Geçersiz bir anlık görüntü yapılandırması algılandı. ']

Birden çok diske sahip bir sanal makineniz varsa, sanal makineden bir diski kaldırırsanız bu hatayla karşılaşabilirsiniz. Bu sorunu düzeltmek için, [Bu VMware makalesindeki](https://go.microsoft.com/fwlink/?linkid=2138890)adımlara bakın.

### <a name="error-message-an-internal-error-occurred-generate-snapshot-hung"></a>Hata Iletisi: bir iç hata oluştu. [Anlık görüntü askıda üret]

Bu sorun, anlık görüntü oluşturma askıda olduğunda oluşur. Bu sorun oluştuğunda, %95 veya %99 ' de anlık görüntü oluşturma görevi durduruluyor ' u görebilirsiniz. Bu sorunu aşmak için bu [VMware KB](https://go.microsoft.com/fwlink/?linkid=2138969) 'ye başvurun.

### <a name="error-message-an-internal-error-occurred-failed-to-consolidate-the-disks-on-vm-_reasons_"></a>Hata Iletisi: bir iç hata oluştu. [VM 'ler üzerinde diskler birleştirme başarısız oldu _[nedenler]_]

Diskleri çoğaltma döngüsünün sonunda birleştirdiğimiz zaman, işlem başarısız olur. Sorunu çözmek için uygun _nedeni_ seçerek [VMware KB](https://go.microsoft.com/fwlink/?linkid=2138970) ' deki yönergeleri izleyin.

VMware anlık görüntüsüne yönelik işlemler – disk oluşturma, silme veya birleştirme işlemi başarısız olduğunda aşağıdaki hatalar oluşur. Hataları düzeltmek için sonraki bölümdeki yönergeleri izleyin:

### <a name="error-message-an-internal-error-occurred-another-task-is-already-in-progress"></a>Hata Iletisi: bir iç hata oluştu. [Başka bir görev zaten devam ediyor]

Bu sorun, arka planda çalışan çakışan sanal makine görevleri olduğunda veya vCenter Server bir görev zaman aşımına uğrarsa oluşur. Aşağıdaki [VMware KB](https://go.microsoft.com/fwlink/?linkid=2138891)' de belirtilen çözümü izleyin.

### <a name="error-message-an-internal-error-occurred-operation-not-allowed-in-current-state"></a>Hata Iletisi: bir iç hata oluştu. [Geçerli durumda işleme izin verilmiyor]

Bu sorun, vCenter Server Yönetim aracılarının çalışmayı durdurması durumunda oluşur. Bu sorunu çözmek için, aşağıdaki [VMware KB](https://go.microsoft.com/fwlink/?linkid=2138971)' deki çözünürlüğe bakın.

### <a name="error-message-an-internal-error-occurred-snapshot-disk-size-invalid"></a>Hata Iletisi: bir iç hata oluştu. [Anlık görüntü disk boyutu geçersiz]

Bu, anlık görüntü tarafından belirtilen disk boyutunun sıfır olduğu bilinen bir VMware sorunudur. [VMware KB](https://go.microsoft.com/fwlink/?linkid=2138972)'de verilen çözümü izleyin.

### <a name="error-message-an-internal-error-occurred-memory-allocation-failed-out-of-memory"></a>Hata Iletisi: bir iç hata oluştu. [Bellek ayırma başarısız oldu. Bellek yetersiz.]

Bu, NFC ana bilgisayar arabelleğinin belleği tükendiğinde gerçekleşir. Bu sorunu çözmek için VM 'yi (COMPUTE vMotion), ücretsiz kaynakları olan farklı bir konağa taşımanız gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

VM çoğaltmaya devam edin ve [test geçişi](https://go.microsoft.com/fwlink/?linkid=2139333)gerçekleştirin.