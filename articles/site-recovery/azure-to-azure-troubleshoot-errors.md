---
title: Azure Site Kurtarma'da Azure VM çoğaltma sorunu
description: Olağanüstü durum kurtarma için Azure sanal makinelerini çoğaltırken hataları giderin.
author: rochakm
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: rochakm
ms.openlocfilehash: c131609a5622061e2ea49db422bc4e9da96666d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80276691"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-errors"></a>Azure'dan Azure'a VM çoğaltma hatalarını giderme

Bu makalede, Azure sanal makinelerinin (VM) bir bölgeden diğerine çoğaltılması ve kurtarılması sırasında Azure Site Kurtarma'daki sık karşılaşılan hataların nasıl giderilenolduğu açıklanmaktadır. Desteklenen yapılandırmalar hakkında daha fazla bilgi için [Azure VM'lerini çoğaltmak için destek matrisine](site-recovery-support-matrix-azure-to-azure.md)bakın.

## <a name="azure-resource-quota-issues-error-code-150097"></a><a name="azure-resource-quota-issues-error-code-150097"></a>Azure kaynak kotası sorunları (hata kodu 150097)

Olağanüstü durum kurtarma bölgeniz olarak kullanmayı planladığınız hedef bölgede Azure VM'leri oluşturmak için aboneliğinizin etkinleştirildiğinden emin olun. Ayrıca, aboneliğinizin gerekli boyutlardaki VM'leri oluşturmak için yeterli kotaya sahip olduğundan emin olun. Varsayılan olarak, Site Kurtarma kaynak VM boyutuyla aynı olan hedef VM boyutunu seçer. Eşleşen boyut kullanılamıyorsa, Site Kurtarma otomatik olarak kullanılabilir en yakın boyutu seçer.

Kaynak VM yapılandırmasını destekleyen bir boyut yoksa, aşağıdaki ileti görüntülenir:

> "Sanal makine *VmName*için çoğaltma etkinleştirilemezdi."

### <a name="possible-causes"></a>Olası nedenler

- Hedef bölge konumunda herhangi bir VM oluşturmak için abonelik kimliğiniz etkinleştirilir.
- Abonelik kimliğiniz, hedef bölge konumunda belirli VM boyutları oluşturmak için etkinleştirildi veya yeterli kotaya sahip değil.
- Hedef bölge konumundaki abonelik kimliği için kaynak VM ağ arabirim kartı (NIC) sayısıyla (2) eşleşecek uygun hedef VM boyutu bulunamaz.

### <a name="fix-the-problem"></a>Sorunu çözme

Aboneliğinizin hedef konumda gerekli boyutlarda NM'ler oluşturmasını sağlamak için [Azure faturalandırma desteğine](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) başvurun. Ardından, başarısız işlemi yeniden deneyin.

Hedef konumda bir kapasite kısıtlaması varsa, çoğaltmayı devre dışı kındırın. Ardından, aboneliğinizin gerekli boyutlarda KiS'ler oluşturmak için yeterli kotaya sahip olduğu farklı bir konuma çoğaltmayı etkinleştirin.

## <a name="trusted-root-certificates-error-code-151066"></a><a name="trusted-root-certificates-error-code-151066"></a>Güvenilen kök sertifikalar (hata kodu 151066)

VM'de en son güvenilen kök sertifikaların tümü yoksa, "Çoğaltmayı etkinleştir" Site Kurtarma işiniz başarısız olabilir. VM'den Gelen Site Kurtarma hizmet çağrılarının kimlik doğrulaması ve yetkilendirmesi bu sertifikalar olmadan başarısız olur.

"Çoğaltmayı etkinleştirme" işi başarısız olursa, aşağıdaki ileti görüntülenir:

> "Site Kurtarma yapılandırması başarısız oldu."

### <a name="possible-cause"></a>Olası nedeni

Yetkilendirme ve kimlik doğrulama için gereken güvenilir kök sertifikaları sanal makinede bulunmaz.

### <a name="fix-the-problem"></a>Sorunu çözme

#### <a name="windows"></a>Windows

Windows işletim sistemini çalıştıran bir VM için, tüm güvenilir kök sertifikaların makinede bulunması için VM'deki en son Windows güncelleştirmelerini yükleyin. VM'lerde en son kök sertifikaları ve güncelleştirilmiş sertifika iptal listesini almak için kuruluşunuzdaki tipik Windows güncelleştirme yönetimi veya sertifika güncelleştirme yönetimi işlemini izleyin.

Bağlantınız kesilmiş bir ortamdaysanız, sertifikaları almak için kuruluşunuzdaki standart Windows güncelleştirme işlemini izleyin. VM'de gerekli sertifikalar yoksa, Site Kurtarma hizmetine yapılan çağrılar güvenlik nedenleriyle başarısız olur.

Sorunun çözüldüğünü doğrulamak için VM'nizdeki bir tarayıcıdan login.microsoftonline.com gidin.

Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/dn265983.aspx)

#### <a name="linux"></a>Linux

En son güvenilir kök sertifikalarını ve VM'deki en son sertifika iptal listesini almak için Linux işletim sistemi sürümünüzün dağıtıcısı tarafından sağlanan kılavuzu izleyin.

SUSE Linux sertifika listesini korumak için sembolik bağlantılar (veya *symlinks)* kullandığından aşağıdaki adımları izleyin:

1. Kök kullanıcı olarak oturum açın.

1. Dizini değiştirmek için bu komutu çalıştırın:

    **# cd /etc/ssl/certs**

1. Symantec kök CA sertifikasının mevcut olup olmadığını kontrol edin:

    **# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Symantec kök CA sertifikası bulunamazsa, dosyayı indirmek için aşağıdaki komutu çalıştırın. Hataları denetleyin ve ağ hataları için önerilen eylemleri izleyin.

    **# Wget https://docs.broadcom.com/docs-and-downloads/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Baltimore kök CA sertifikasının mevcut olup olmadığını kontrol edin:

    **# ls Baltimore_CyberTrust_Root.pem**

1. Baltimore kök CA sertifikası bulunamazsa, sertifikayı indirmek için bu komutu çalıştırın:

    **# Wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem**

1. DigiCert_Global_Root_CA sertifikasının mevcut olup olmadığını kontrol edin:

    **# ls DigiCert_Global_Root_CA.pem**

1. DigiCert_Global_Root_CA bulunamazsa, sertifikayı indirmek için aşağıdaki komutları çalıştırın:

    **# Wgethttp://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt**

    **# açarsl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem**

1. Yeni indirilen sertifikalar için sertifika özne karmalarını güncelleştirmek için rehash komut dosyasını çalıştırın:

    **# c_rehash**

1. Sertifikalar için konu nun söbül olarak mı oluşturulduğunu denetlemek için bu komutları çalıştırın:

    - Komut:

        **# ls -l | grep Baltimore**

    - Çıktı:

        `lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem`

        `-rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem`

    - Komut:

        **# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5**

    - Çıktı:

        `-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

        `lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

    - Komut:

        **# ls -l | grep DigiCert_Global_Root**

    - Çıktı:

        `lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem`

        `-rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem`

1. dosya VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem dosya adı b204d74a.0 ile bir kopyasını oluşturun:

    **# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0**

1. Dosya adı 653b494a.0 ile dosya Baltimore_CyberTrust_Root.pem bir kopyasını oluşturun:

    **# cp Baltimore_CyberTrust_Root.pem 653b494a.0**

1. Dosya DigiCert_Global_Root_CA.pem dosya adı 3513523f.0 ile bir kopyasını oluşturun:

    **# cp DigiCert_Global_Root_CA.pem 3513523f.0**

1. Dosyaların mevcut olup olmadığını denetleyin:

    - Komut:

        **# ls -l 653b494a.0 b204d74a.0 3513523f.0**

    - Çıktı

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0`

        `-rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0`

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0`

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Kurtarma URL'leri veya IP aralıkları için giden bağlantı (hata kodu 151037 veya 151072)

Site Kurtarma çoğaltmaçalışması için, VM'den belirli URL'lere giden bağlantı gereklidir. VM'niz bir güvenlik duvarının arkasındaysa veya giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kurallarını kullanıyorsa, bu sorunlardan biriyle karşılaşabilirsiniz. URL'ler üzerinden giden erişimi desteklemeye devam ederken, IP aralıklarının izin listesini kullanmanın artık desteklenmediğini lütfen unutmayın.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a><a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Sorun 1: Azure sanal makinesini Site Kurtarma (151195) ile kaydedileme </br>
- **Olası nedeni** </br>
  - DNS çözüm hatası nedeniyle Site Kurtarma uç noktalarına bağlantı kurulamaz.
  - Sanal makine yükünü devretmenize rağmen DNS sunucusuna DR bölgesinden erişilemediğinde yeniden koruma sırasında bu durum daha sık görülür.

- **Çözünürlük**
   - Özel DNS kullanıyorsanız, DNS sunucusuna Olağanüstü Durum Kurtarma bölgesinden erişilebildiğinden emin olun. Özel bir DNS'niz olup olmadığını kontrol etmek için VM> Olağanüstü Durum Kurtarma ağına> DNS sunucularına gidin. DNS sunucusuna sanal makineden erişmeyi deneyin. Erişilemezse, dns sunucusu üzerinden başarısız olarak veya DR ağı ile DNS arasındaki site hattını oluşturarak erişilebilir hale getirin.

    ![com hatası](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Sorun 2: Site Kurtarma yapılandırması başarısız oldu (151196)
- **Olası nedeni** </br>
  - Office 365 kimlik doğrulama ve kimlik IP4 uç noktalarına bağlantı kurulamaz.

- **Çözünürlük**
  - Azure Site Kurtarma kimlik doğrulaması için Office 365 IP aralıklarına erişim gerekir.
    VM'de giden ağ bağlantısını denetlemek için Azure Ağ Güvenlik Grubu (NSG) kurallarını/güvenlik duvarı proxy'sini kullanıyorsanız, lütfen AAD'ye erişime izin vermek için [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) tabanlı NSG kuralını kullandığınızdan emin olun. Artık IP adresine dayalı NSG kurallarını destekliyoruz.


### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Sorun 3: Site Kurtarma yapılandırması başarısız oldu (151197)
- **Olası nedeni** </br>
  - Azure Site Kurtarma hizmeti uç noktalarına bağlantı kurulamaz.

- **Çözünürlük**
  - VM'de giden ağ bağlantısını denetlemek için Azure Ağ Güvenlik Grubu (NSG) kurallarını/güvenlik duvarı proxy'sini kullanıyorsanız, lütfen hizmet etiketlerini kullandığınızdan emin olun. Azure Site Kurtarma (ASR) için NSG'ler aracılığıyla IP adreslerinin izin listesini kullanmayı artık desteklemeyiz.


### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premise-proxy-server-151072"></a>Sorun 4: Ağ trafiği şirket içi proxy sunucusundan geçtiğinde A2A çoğaltma başarısız oldu (151072)
 - **Olası nedeni** </br>
   - Özel proxy ayarları geçersizdir ve ASR Mobility Service aracısı IE'den proxy ayarlarını otomatik olarak algılamadı


 - **Çözünürlük**
   1.    Mobilite Hizmeti aracısı, Windows'ta IE'den proxy ayarlarını algılar ve Linux'ta /etc/environment.
   2.  Yalnızca ASR Mobilite Hizmeti için proxy ayarlamayı tercih ederseniz, proxyInfo.conf'ta bulunan proxy ayrıntılarını şu adreste sağlayabilirsiniz:</br>
       - ``/usr/local/InMage/config/``***Linux*** üzerinde
       - ``C:\ProgramData\Microsoft Azure Site Recovery\Config``***Windows'da***
   3.    ProxyInfo.conf dosyasında ara sunucu ayarları şu INI biçiminde bulunmalıdır.</br>
                   *[proxy]*</br>
                   *Adres=http://1.2.3.4*</br>
                   *Bağlantı noktası=567*</br>
   4. ASR Mobility Service aracısı yalnızca ***kimlik doğrulamamış vekilleri***destekler.


### <a name="fix-the-problem"></a>Sorunu çözme

İzin [listesine gerekli URL'leri](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) eklemek için ağ [kılavuzu belgesindeki](site-recovery-azure-to-azure-networking-guidance.md)adımları izleyin.


### <a name="more-information"></a>Daha fazla bilgi

Gerekli [URL'leri](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıklarını](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)belirtmek [için, Azure'da Ağ Hakkında'dan Azure çoğaltmasına](site-recovery-azure-to-azure-networking-guidance.md)ilişkin kılavuzu izleyin.

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk makinede bulunamadı (hata kodu 150039)

VM'ye bağlı yeni bir disk baş harfe getirilmelidir. Disk bulunamazsa, aşağıdaki ileti görüntülenir:

> "Mantıksal birim numarası *LUN* *LUNValue* ile Azure veri diski *DiskName* *DiskURI* aynı LUN değerine sahip VM içinden bildirilen ilgili bir disk eşlenen değildi.

### <a name="possible-causes"></a>Olası nedenler

- VM'ye yeni bir veri diski iliştirildi ancak başharflere geçirilmiş.
- VM içindeki veri diski, diskin VM'ye bağlı olduğu mantıksal birim numarası (LUN) değerini doğru şekilde bildirmiyor.

### <a name="fix-the-problem"></a>Sorunu çözme

Veri disklerinin baş harfe sahip olduğundan emin olun ve işlemi yeniden deneyin.

- **Windows**: [Yeni bir disk iliştirin ve baş harfe bağla.](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal)

- **Linux**: [Linux'ta yeni bir veri diskini başlangıç](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Sorun devam ederse desteğe başvurun.

## <a name="one-or-more-disks-are-available-for-protection-error-code-153039"></a>Bir veya daha fazla disk koruma için kullanılabilir (hata kodu 153039)

### <a name="possible-causes"></a>Olası nedenler

- Koruma dan sonra sanal makineye yakın zamanda bir veya daha fazla disk eklendi.
- Sanal makinenin korunmasından sonra bir veya daha fazla disk başharfe bünyeydi.

### <a name="fix-the-problem"></a>Sorunu çözme

VM çoğaltma durumunu yeniden sağlıklı hale getirmek için, diskleri korumayı veya uyarıyı reddetmeyi seçebilirsiniz.

#### <a name="to-protect-the-disks"></a>Diskleri korumak için

1. **Çoğaltılan Öğeler** > *VM adı* > **Diskleri'ne**gidin.
1. Korumasız diski seçin ve ardından **çoğaltmayı etkinleştir'i**seçin:

    ![VM disklerde çoğaltmayı etkinleştirme](./media/azure-to-azure-troubleshoot-errors/add-disk.png)

#### <a name="to-dismiss-the-warning"></a>Uyarıyı kapatmak için

1. **Çoğaltılmış öğelere** > git*VM adı.*
1. **Genel Bakış** bölümündeuyarıyı seçin ve ardından **Tamam'ı**seçin.

    ![Yeni disk uyarısını reddetme](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)

## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information-error-code-150225"></a>Bilgi ile tamamlanan kasadan sanal makineyi çıkarın (hata kodu 150225)

Sanal makineyi koruduğunda, Site Kurtarma kaynak sanal makinede bazı bağlantılar oluşturur. Korumayı kaldırdığınızda veya çoğaltmayı devre dışı kaldırdığınızda, Site Kurtarma temizleme işinin bir parçası olarak bu bağlantıları kaldırır. Sanal makinede kaynak kilidi varsa, bilgiyle birlikte temizleme işi tamamlanır. Bilgiler, sanal makinenin Kurtarma Hizmetleri kasasından kaldırıldığını, ancak bazı eski bağlantıların kaynak makinede temizlenemediğini söylüyor.

Bu sanal makineyi bir daha korumak niyetinde değilseniz bu uyarıyı yok sayabilirsiniz. Ancak bu sanal makineyi daha sonra korumanız gerekiyorsa, bağlantıları temizlemek için "Sorunu düzeltin" altındaki adımları izleyin.

> [!WARNING]
> Temizleme yapmazsanız:
>
> - Kurtarma Hizmetleri kasası ile çoğaltmayı etkinleştirdiğinizde, sanal makine listelenmez.
> - **Sanal makine** > **Ayarları** > **Olağanüstü Durum Kurtarma'yı**kullanarak VM'yi korumaya çalışırsanız, "VM'deki varolan bayat kaynak bağlantıları nedeniyle çoğaltma etkinleştirilemez" iletisiyle işlem başarısız olur.

### <a name="fix-the-problem"></a>Sorunu çözme

> [!NOTE]
> Site Kurtarma, siz bu adımları gerçekleştirirken kaynak sanal makineyi silmez veya herhangi bir şekilde etkilemez.

1. VM veya VM kaynak grubundan kilidi kaldırın. Örneğin, aşağıdaki resimde, "MoveDemo" adlı VM'deki kaynak kilidi silinmelidir:

    ![VM'deki kilidi kaldırma](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Eski bir [Site Kurtarma yapılandırmasını kaldırmak](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1)için komut dosyasını indirin.
1. Cleanup-bayat-asr-config-Azure-VM.ps1 olarak adlandırılan komut dosyasını çalıştırın. Abonelik kimliğini, VM Kaynak Grubu'nu ve VM adını parametre olarak tedarik edin.
1. Azure kimlik bilgileri istenirse, bunları sağlayın. Ardından, komut dosyasının herhangi bir hata olmadan çalıştığını doğrulayın.

## <a name="replication-cant-be-enabled-because-of-stale-resource-links-on-the-vm-error-code-150226"></a>VM'deki eski kaynak bağlantıları nedeniyle çoğaltma etkinleştirilemiyor (hata kodu 150226)

### <a name="possible-cause"></a>Olası nedeni

Sanal makine, önceki Site Kurtarma korumasından eski bir yapılandırmaya sahiptir.

Site Kurtarma'yı kullanarak Azure VM için çoğaltmayı etkinleştirdiyseniz, Azure VM'de eski bir yapılandırma oluşabilir ve ardından:

- Çoğaltmadevre dışı bıraktınız, ancak kaynak VM'nin kaynak kilidi vardı.
- VM'deki çoğaltmayı açıkça devre dışı bırakmadan Site Kurtarma kasasını sildiniz.
- VM'de çoğaltmayı açıkça devre dışı bırakmadan Site Kurtarma kasasını içeren kaynak grubunu sildiniz.

### <a name="fix-the-problem"></a>Sorunu çözme

> [!NOTE]
> Site Kurtarma, siz bu adımları gerçekleştirirken kaynak sanal makineyi silmez veya herhangi bir şekilde etkilemez.

1. VM veya VM kaynak grubundan kilidi kaldırın. Örneğin, aşağıdaki resimde, "MoveDemo" adlı VM'deki kaynak kilidi silinmelidir:

    ![VM'deki kilidi kaldırma](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Eski bir [Site Kurtarma yapılandırmasını kaldırmak](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1)için komut dosyasını indirin.
1. Cleanup-bayat-asr-config-Azure-VM.ps1 olarak adlandırılan komut dosyasını çalıştırın. Abonelik kimliğini, VM Kaynak Grubu'nu ve VM adını parametre olarak tedarik edin.
1. Azure kimlik bilgileri istenirse, bunları sağlayın. Ardından, komut dosyasının herhangi bir hata olmadan çalıştığını doğrulayın.

## <a name="unable-to-see-the-azure-vm-or-resource-group-for-the-selection-in-the-enable-replication-job"></a>"Çoğaltmayı etkinleştirme" işinde seçim için Azure VM'sini veya kaynak grubunu göremiyor

### <a name="cause-1-the-resource-group-and-source-virtual-machine-are-in-different-locations"></a>Neden 1: Kaynak grubu ve kaynak sanal makine farklı konumlarda

Site Kurtarma şu anda kaynak bölge kaynak grubu ve sanal makinelerin aynı konumda olmasını gerektirir. Değilse, koruma uygulamaya çalıştığınızda sanal makineyi veya kaynak grubunu bulamazsınız.

Geçici çözüm olarak, Kurtarma Hizmetleri kasası yerine VM'den çoğaltmayı etkinleştirebilirsiniz. Kaynak **VM** > **Özellikleri** > **Olağanüstü Durum Kurtarma'ya** gidin ve çoğaltmayı etkinleştirin.

### <a name="cause-2-the-resource-group-is-not-part-of-the-selected-subscription"></a>Neden 2: Kaynak grubu seçili aboneliğin bir parçası değildir

Kaynak grubu seçili aboneliğin bir parçası değilse, koruma sırasında kaynak grubunu bulamayabilirsiniz. Kaynak grubunun kullanmakta olduğunuz aboneye ait olduğundan emin olun.

### <a name="cause-3-stale-configuration"></a>Neden 3: Bayat yapılandırma

Azure VM'de eski bir Site Kurtarma yapılandırması bırakılmışsa çoğaltma için etkinleştirmek istediğiniz VM'yi göremeyebilirsiniz. Bu durum, Azure VM için Site Kurtarma'yı kullanarak çoğaltmayı etkinleştirdiyseniz oluşabilir ve ardından:

- VM'deki çoğaltmayı açıkça devre dışı bırakmadan Site Kurtarma kasasını sildiniz.
- VM'de çoğaltmayı açıkça devre dışı bırakmadan Site Kurtarma kasasını içeren kaynak grubunu sildiniz.
- Çoğaltmadevre dışı bıraktınız, ancak kaynak VM'nin kaynak kilidi vardı.

### <a name="fix-the-problem"></a>Sorunu çözme

> [!NOTE]
> Bu bölümde belirtilen komut dosyasını kullanmadan önce "AzureRM.Resources" modüllerini güncelleştirmiş olduğundan emin olun.  Site Kurtarma, siz bu adımları gerçekleştirirken kaynak sanal makineyi silmez veya herhangi bir şekilde etkilemez.

1. Varsa kilidi VM veya VM kaynak grubundan çıkarın. Örneğin, aşağıdaki resimde, "MoveDemo" adlı VM'deki kaynak kilidi silinmelidir:

    ![VM'deki kilidi kaldırma](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Eski bir [Site Kurtarma yapılandırmasını kaldırmak](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1)için komut dosyasını indirin.
1. Cleanup-bayat-asr-config-Azure-VM.ps1 olarak adlandırılan komut dosyasını çalıştırın. Abonelik kimliğini, VM Kaynak Grubu'nu ve VM adını parametre olarak tedarik edin.
1. Azure kimlik bilgileri istenirse, bunları sağlayın. Ardından, komut dosyasının herhangi bir hata olmadan çalıştığını doğrulayın.

## <a name="unable-to-select-a-virtual-machine-for-protection"></a>Koruma için sanal bir makine seçileme

### <a name="cause-1-the-virtual-machine-has-an-extension-installed-in-a-failed-or-unresponsive-state"></a>Neden 1: Sanal makinenin bir uzantısı başarısız veya yanıt vermeyen bir durumda yüklenir

Sanal **makineler** > **Ayarları** > **Uzantıları** gidin ve başarısız bir durumda herhangi bir uzantıları için kontrol edin. Başarısız olan uzantıyı kaldırın ve sanal makineyi korumak için yeniden deneyin.

### <a name="cause-2-the-vms-provisioning-state-is-not-valid"></a>Neden 2: VM'nin sağlama durumu geçerli değildir

[VM'nin sağlama durumundaki](#the-vms-provisioning-state-is-not-valid-error-code-150019)sorun giderme adımlarını görmek geçerli değildir ( daha sonra bu makalede.

## <a name="the-vms-provisioning-state-is-not-valid-error-code-150019"></a>VM'nin sağlama durumu geçerli değildir (hata kodu 150019)

VM üzerinde çoğaltmayı etkinleştirmek için, onun sağlama durumu **Başarılı**olmalıdır. Sağlama durumunu denetlemek için aşağıdaki adımları izleyin:

1. Azure portalında, **Tüm Hizmetler'den** **Kaynak Gezgini'ni** seçin.
1. **Abonelikler** listesini genişletin ve aboneliğinizi seçin.
1. Kaynak **Grupları** listesini genişletin ve VM'nin kaynak grubunu seçin.
1. **Kaynaklar** listesini genişletin ve VM'nizi seçin.
1. Sağ tarafta Örnek görünümünde **provisioningState** alanını kontrol edin.

### <a name="fix-the-problem"></a>Sorunu çözme

- **ProvisioningState** **Başarısız olduysa,** sorun gidermek için ayrıntılarla birlikte desteğe başvurun.
- **ProvisioningState** **Güncelleniyorsa,** başka bir uzantı dağıtılıyor olabilir. VM'de devam eden işlemler olup olmadığını kontrol edin, bunların tamamlanmasını bekleyin ve ardından başarısız Site Kurtarma "Çoğaltmayı etkinleştir" işini yeniden deneyin.

## <a name="unable-to-select-target-vm-network-selection-tab-is-unavailable"></a>Hedef VM seçilemiş (ağ seçimi sekmesi kullanılamıyor)

### <a name="cause-1-your-vm-is-attached-to-a-network-thats-already-mapped-to-a-target-network"></a>Neden 1: VM'niz hedef ağa zaten eşlenmiş bir ağa bağlı

Kaynak VM sanal bir ağın parçasıysa ve aynı sanal ağdan başka bir VM zaten hedef kaynak grubunda bir ağla eşlenmişse, ağ seçimi açılır liste kutusu varsayılan olarak kullanılamaz (soluk görünür).

![Ağ seçim listesi kullanılamıyor](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

### <a name="cause-2-you-previously-protected-the-vm-by-using-site-recovery-and-then-you-disabled-the-replication"></a>Neden 2: Daha önce Site Kurtarma'yı kullanarak VM'yi korudunuz ve ardından çoğaltmayı devre dışı bıraktınız

VM'nin çoğaltılması devre dışı bırakış, ağ eşlemesini silmez. Eşleme, VM'nin korunduğu Kurtarma Hizmetleri kasasından silinmelidir. Kurtarma *Hizmetleri vault* > **Site Kurtarma Altyapı** > **Ağı Haritalama**gidin.

![Ağ eşlemelerini silme](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)

Olağanüstü durum kurtarma kurulumu sırasında yapılandırılan hedef ağ, VM korunduktan sonra ilk kurulumdan sonra değiştirilebilir:

![Ağ eşlemesini değiştirme](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)

Ağ eşlemi nin değiştirilmesinin, aynı ağ eşlemi kullanan tüm korumalı VM'leri etkilediğini unutmayın.

## <a name="com-or-volume-shadow-copy-service-error-error-code-151025"></a>COM+ veya Birim Gölge Kopyalama hizmet hatası (hata kodu 151025)

Bu hata oluştuğunda, aşağıdaki ileti görüntülenir:

> "Site Kurtarma uzantısı yüklenemedi"

### <a name="possible-causes"></a>Olası nedenler

- COM+ Sistem Uygulaması hizmeti devre dışı bırakılmıştır.
- Birim Gölge Kopya hizmeti devre dışı bırakılır.

### <a name="fix-the-problem"></a>Sorunu çözme

COM+ Sistem Uygulaması ve Birim Gölge Kopyalama hizmetlerini otomatik veya manuel başlatma moduna ayarlayın.

1. Windows'da Hizmetler konsolu'nu açın.
1. COM+ Sistem Uygulaması ve Birim Gölge Kopyalama hizmetlerinin Başlangıç **Türü**olarak **Devre Dışı Bırakılmadığından** emin olun.

    ![COM+ Sistem Uygulaması ve Birim Gölge Kopyalama hizmetlerinin başlangıç türünü kontrol edin](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Desteklenmeyen yönetilen disk boyutu (hata kodu 150172)

Bu hata oluştuğunda, aşağıdaki ileti görüntülenir:

> "Minimum desteklenen boyutu 1024 MB daha az boyutu *DiskName*)* ile *DiskName* olduğu gibi sanal makine için koruma etkinleştirilemezdi."

### <a name="possible-cause"></a>Olası nedeni

Disk, desteklenen 1024 MB boyutundan daha küçüktür.

### <a name="fix-the-problem"></a>Sorunu çözme

Disk boyutunun desteklenen boyut aralığında olduğundan emin olun ve ardından işlemi yeniden deneyin.

## <a name="protection-was-not-enabled-because-the-grub-configuration-includes-the-device-name-instead-of-the-uuid-error-code-151126"></a><a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>GRUB yapılandırması UUID yerine aygıt adını içerdiğinden koruma etkinleştirildi (hata kodu 151126)

### <a name="possible-cause"></a>Olası nedeni

Linux GRUB yapılandırma dosyaları (/boot/grub/menu.lst", /boot/grub/grub.cfg, /boot/grub2/grub.cfg veya /etc/default/grub) *kök* ve *özgeçmiş* parametreleri için UUID değerleri yerine gerçek aygıt adlarını belirtebilir. Aygıt adları değişebileceğinden Site Kurtarma kullanıcı larını gerektirir. Yeniden başlatılanın ardından, bir VM başarısız lıkta aynı adı vermeyebilir ve bu da sorunlara yol açabilir.

Aşağıdaki örnekler, gerekli Kullanıcı Arabirimi yerine aygıt adlarının (kalın olarak gösterildiği) GRUB dosyalarından gelen satırlardır:

- Dosya /boot/grub2/grub.cfg

  > linux /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2** ${extra_cmdline} **resume=/dev/sda1** splash=silent silent showopts

- Dosya: /boot/grub/menu.lst

  > çekirdek /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314


### <a name="fix-the-problem"></a>Sorunu çözme

Her aygıt adını ilgili UUID ile değiştirin:

1. Komut **blkid** ***aygıt adını***çalıştırarak aygıtın UUID'sini bulun. Örnek:

    ```
    blkid /dev/sda1
    /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
    blkid /dev/sda2
    /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Aygıt adını **root=UUID**=*UUID* ve **resume=UUID UUID**=*UUID*biçimlerinde UUID ile değiştirin. Örneğin, değiştirildikten sonra ,/boot/grub/menu.lst (daha önce tartışılan) satırı aşağıdaki gibi görünür:

    > çekirdek /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4 resume=UUID=UUID=62927e85** **6f614b44-433b-431b-9ca1-4dd2f6f74f6b** sıçrama=sessiz crashkernel=256M-:128M showopts vga=0x314

1. Korumayı yeniden deneyin.

## <a name="enable-protection-failed-because-the-device-mentioned-in-the-grub-configuration-doesnt-exist-error-code-151124"></a>GRUB yapılandırmasında belirtilen aygıt yoksa korumayı etkinleştirme (hata kodu 151124)

### <a name="possible-cause"></a>Olası nedeni

GRUB yapılandırma dosyaları (/boot/grub/menu.lst, /boot/grub/grub.cfg, /boot/grub2/grub2/grub.cfg, veya /etc/default/grub) *parametreleri rd.lvm.lv* veya *rd_LVM_LV*içerebilir. Bu parametreler, önyükleme zamanında keşfedilecek Mantıksal Birim Yöneticisi (LVM) aygıtlarını tanımlar. Bu LVM aygıtları yoksa, korumalı sistemin kendisi önyükleme yapmaz ve önyükleme işleminde sıkışıp kalır. Aynı sorun da failover VM ile görülecektir. Aşağıda birkaç örnek verilmiştir:

- Dosya: /boot/grub2/grub.cfg üzerinde RHEL7:

    > linux16 /vmlinuz-3.10.0-957.el7.x86_64 kök=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M\@64M **rd.lvm.lv=rootvg/rootrd.lvm.lv=rootvg/swap** rhgb sessiz LANG=en_US. UTF-8

- Dosya: /etc/default/grub üzerinde RHEL7:

    > GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb sessiz"

- Dosya: /boot/grub/menu.lst üzerinde RHEL6:

    > çekirdek /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US. UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto **rd_LVM_LV=rootvg/lv_root** KEYBOARDTYPE=pc KEYTABLE=us **rd_LVM_LV=rootvg/lv_swap** rd_NO_DM rhgb sessiz

Her örnekte, kalın bölüm GRUB'un "root" ve "swap" adlı iki LVM aygıtını "rootvg" ses grubundan algılaması gerektiğini gösterir.

### <a name="fix-the-problem"></a>Sorunu çözme

LVM aygıtı yoksa, aygıtı oluşturun veya ilgili parametreleri GRUB yapılandırma dosyalarından kaldırın. Ardından, korumayı etkinleştirmek için yeniden deneyin.

## <a name="a-site-recovery-mobility-service-update-finished-with-warnings-error-code-151083"></a>Bir Site Kurtarma Mobilite Hizmeti güncelleştirmesi uyarılarla tamamlandı (hata kodu 151083)

Site Kurtarma Mobilite Hizmeti, biri filtre sürücüsü olarak adlandırılan birçok bileşene sahiptir. Filtre sürücüsü yalnızca sistem yeniden başlatma sırasında sistem bellene yüklenir. Mobilite Hizmeti güncelleştirmesi filtre sürücüsü değişikliklerini içerdiğinde, makine güncelleştirilir, ancak bazı düzeltmelerin yeniden başlatılması gerektiğine dair bir uyarı görürsünüz. Filtre sürücüsü düzeltmeleri yalnızca yeni filtre sürücüsü yüklendiğinde etkili olabileceğinden ve bu uyarı yalnızca yeniden başlatma sırasında gerçekleşebilir.

> [!NOTE]
> Bu sadece bir uyarı. Varolan çoğaltma, yeni aracı güncelleştirmeden sonra bile çalışmaya devam ediyor. Yeni filtre sürücüsünün avantajlarını istediğinizzaman yeniden başlatmayı seçebilirsiniz, ancak yeniden başlatmazsanız eski filtre sürücüsü çalışmaya devam eder.
>
> Filtre sürücüsünün dışında, Mobilite Hizmeti güncelleştirmesindeki diğer geliştirmelerin ve düzeltmelerin avantajları yeniden başlatma gerektirmeden etkili olur.

## <a name="protection-couldnt-be-enabled-because-the-replica-managed-disk-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>Yineleme yönetilen disk, beklenen etiketler olmadan hedef kaynak grubunda zaten var olduğundan koruma etkinleştirilememiş (hata kodu 150161)

### <a name="possible-cause"></a>Olası nedeni

Sanal makine daha önce korunuyorsa ve çoğaltma devre dışı bırakıldığında çoğaltma diski temizlenmediyse, bu sorun oluşabilir.

### <a name="fix-the-problem"></a>Sorunu çözme

Hata iletisinde tanımlanan yineleme diskini silin ve başarısız koruma işini yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure sanal makinelerini çoğaltma](site-recovery-replicate-azure-to-azure.md)
