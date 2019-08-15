---
title: Azure Site Recovery Azure'dan Azure'a çoğaltma sorunlarını ve hataları için sorun giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: asgang
ms.openlocfilehash: 4d8ba44cdd5161a1a5ff108837cb57af4cd98835
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69034777"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure'dan Azure'a VM çoğaltmayla sorunları giderme

Bu makalede Azure Site Recovery çoğaltma ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma giren yaygın sorunların ve bunları nasıl giderebileceğinizden açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz. [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="list-of-errors"></a>Hata listesi
- **[Azure Kaynak kotası sorunları (hata kodu 150097)](#azure-resource-quota-issues-error-code-150097)**
- **[Güvenilen kök sertifikalar (hata kodu 151066)](#trusted-root-certificates-error-code-151066)**
- **[Site Recovery için giden bağlantı (hata kodu 151195)](#issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br)**

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure Kaynak kotası sorunları (hata kodu 150097)
Azure Vm'lerinde olağanüstü durum kurtarma bölgeniz olarak kullanmayı planlıyorsanız hedef bölgede oluşturmak için aboneliğinizi yeniden etkinleştirilmesi gerekir. Ayrıca, aboneliğinizi yeterli kotası belirli boyut sanal makineler oluşturmak için etkinleştirilmiş olması gerekir. Varsayılan olarak, Site Recovery, hedef sanal makine için aynı boyutta VM kaynağı olarak seçer. Eşleşen boyutu kullanılabilir değilse, en yakın olası boyutu otomatik olarak seçilir. Kaynak VM yapılandırması destekleyen hiçbir eşleşen boyutu varsa, bu hata iletisi görüntülenir:

**Hata kodu** | **Olası nedenler** | **Öneri**
--- | --- | ---
150097<br></br>**İleti**: VM VmName sanal makinesi için çoğaltma etkinleştirilemedi. | -Abonelik Kimliğinizi hedef bölge konumda herhangi bir VM oluşturmak için etkinleştirilmemiş olabilir.</br></br>-Abonelik Kimliğinizi etkinleştirilmemiş veya hedef bölge konumunda belirli VM boyutları oluşturmak için yeterli kotası yok.</br></br>-Bir uygun bir hedef kaynak VM NIC sayısı (2) eşleşen bir VM boyutu için abonelik Kimliğini hedef bölge konumunda bulunan değil.| İlgili kişi [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) gereken hedef konum aboneliğinizde VM boyutları için VM oluşturmayı etkinleştirmek için. Etkinleştirildikten sonra başarısız olan işlemi yeniden deneyin.

### <a name="fix-the-problem"></a>Sorunu
Sizinle iletişim [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) hedef konumda gerekli boyutlardaki Vm'leri oluşturmak aboneliğinizi etkinleştirmek için.

Hedef konumu bir kapasite kısıtlaması varsa, çoğaltmayı devre dışı bırakın ve gerekli boyutlardaki Vm'leri oluşturmak için yeterli kotası aboneliğinizin bulunduğu farklı bir konuma etkinleştirin.

## <a name="trusted-root-certificates-error-code-151066"></a>Güvenilen kök sertifikalar (hata kodu 151066)

Sanal makinede mevcut en yeni güvenilen kök sertifikalar mevcut değilse, "çoğaltmayı etkinleştir" işi başarısız olabilir. Sertifikaları olmadan, Site Recovery hizmeti çağrıları VM'den yetkilendirme ve kimlik doğrulaması başarısız. Başarısız "çoğaltmayı etkinleştir" Site kurtarma işi için hata iletisi görüntülenir:

**Hata kodu** | **Olası nedeni** | **Öneriler**
--- | --- | ---
151066<br></br>**İleti**: Site Recovery yapılandırması başarısız oldu. | İstenen makinede mevcut olmayan kimlik doğrulaması ve yetkilendirme için kök sertifikalarını kullanılan güvenilir. | -Windows işletim sistemi çalıştıran bir VM için güvenilen kök sertifikalar makinede mevcut olduğundan emin olun. Bilgi için [yapılandırma Güvenilen Kökleri ve izin verilmeyen sertifikaları](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Linux işletim sistemini çalıştıran bir VM için Linux işletim sistemi sürümü dağıtıcı tarafından yayımlanan güvenilen kök sertifikalar için yönergeleri izleyin.

### <a name="fix-the-problem"></a>Sorunu
**Windows**

Tüm güvenilen kök sertifikalar makinede mevcut olacak şekilde sanal makinede en son Windows güncelleştirmelerini yükleyin. Bağlantısı kesilmiş bir ortamda kullanıyorsanız, kuruluşunuz sertifikaları almak için standart Windows güncelleştirme işlemi izleyin. Gerekli sertifikaları sanal makinede mevcut değilse, Site Recovery hizmetine çağrı güvenlikle ilgili nedenlerle başarısız.

Tipik Windows güncelleştirme yönetimi veya sertifika güncelleştirme yönetimi işlemi sanal makinelere en son kök sertifikalar ve güncelleştirilmiş sertifika iptal listesini almak için kuruluşunuzdaki izleyin.

Sorunun giderilmiş olduğunu doğrulamak için vm'nizde bir tarayıcıdan login.microsoftonline.com için gidin.

**Linux**

Sanal makinede en son güvenilir kök sertifikaları ve en son sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.

SuSE Linux sertifika listesini korumak için çözümlemeyin kullandığından, aşağıdaki adımları izleyin:

1.  Kök kullanıcı olarak oturum açın.

2.  Dizini değiştirmek için bu komutu çalıştırın.

      ``# cd /etc/ssl/certs``

1. Symantec kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

2. Symantec kök CA sertifika bulunmazsa, dosyayı indirmek için aşağıdaki komutu çalıştırın. Hatalar için denetleyin ve ağ hataları için önerilen eylemi izleyin.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

3. Baltimore kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls Baltimore_CyberTrust_Root.pem``

4. Baltimore kök CA sertifika bulunmazsa sertifikayı indirin.  

    ``# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

5. DigiCert_Global_Root_CA cert var olup olmadığını denetleyin.

    ``# ls DigiCert_Global_Root_CA.pem``

6. DigiCert_Global_Root_CA bulunmazsa sertifikayı indirmek için aşağıdaki komutları çalıştırın.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

7. Rehash betiği, bir sertifikayı güncelleştirmek için yeni indirilen sertifika için konu karmaları çalıştırın.

    ``# c_rehash``

8.  Çözümlemeyin sertifikalarını oluşturuldukça konu karıştırır, kontrol edin.

    - Komut

      ``# ls -l | grep Baltimore``

    - Çıktı

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Komut

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Komut

      ``# ls -l | grep DigiCert_Global_Root``

    - Çıktı

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

9.  Filename b204d74a.0 ile VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem dosyanın bir kopyasını oluşturma

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

10. Filename 653b494a.0 ile Baltimore_CyberTrust_Root.pem dosyanın bir kopyasını oluşturma

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Filename 3513523f.0 ile DigiCert_Global_Root_CA.pem dosyanın bir kopyasını oluşturma

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Dosyaları mevcut olup olmadığını denetleyin.  

    - Komut

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery hizmeti URL'lerine veya IP aralıkları (hata kodu 151037 veya 151072) için giden bağlantı

Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa bu sorunlardan biri karşılaşıyor.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Sorun 1: Site Recovery ile Azure sanal makinesi kaydedilemedi (151195) </br>
- **Olası nedeni** </br>
  - DNS çözümleme hatası nedeniyle Site Recovery uç noktalara bağlantı sağlanamadı.
  - Bu daha sık yeniden koruma sırasında sanal makine üzerinde başarısız oldu, ancak DR bölgesindeki DNS sunucusu erişilebilir değil görülür.

- **Çözümleme**
   - Özel DNS kullanıyorsanız sonra emin olun, DNS sunucusu olağanüstü durum kurtarma bölgeden erişilebilir. Sanal Makineye gidin özel bir DNS olup olmadığını denetlemek için > olağanüstü durum kurtarma ağı > DNS sunucuları. DNS sunucusu sanal makineden erişmeyi deneyin. Erişilebilir değilse, ardından erişilebilir DNS sunucusu üzerinde başarısız olan veya DR ağ DNS arasındaki site satırının oluşturma kolaylaştırır.

    ![COM hatası](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Sorun 2: Site Recovery yapılandırması başarısız oldu (151196)
- **Olası nedeni** </br>
  - Office 365 kimlik doğrulaması ve kimlik IP4 uç noktaları için bağlantı kurulamıyor.

- **Çözümleme**
  - Azure Site Recovery, Office 365 IP aralıkları erişimi kimlik doğrulaması için gereklidir.
    VM üzerinde giden ağ bağlantısını denetlemek için Azure ağ güvenlik grubu (NSG) kuralları/güvenlik duvarı proxy'si kullanıyorsanız, O365 aralıkları için iletişime izin vermek emin olun. Oluşturma bir [Azure Active Directory (AAD) hizmet etiketi](../virtual-network/security-overview.md#service-tags) erişimi için AAD karşılık gelen tüm IP adreslerine izin vermek için NSG kural tabanlı
      - Azure Active Directory (AAD) gelecekte yeni adresler eklenir, yeni NSG kuralları oluşturmak gerekir.

> [!NOTE]
> Sanal makineler **Standart** iç yük dengeleyici 'nin arkasındaysa O365 IP 'lerine erişimi olmaz, yani Varsayılan olarak login.microsoftonline.com. **Temel** iç yük dengeleyici türüne değiştirin veya [makalede](https://aka.ms/lboutboundrulescli)belirtildiği gibi, bağlantılı erişim oluşturun.

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Sorun 3: Site Recovery yapılandırması başarısız oldu (151197)
- **Olası nedeni** </br>
  - Azure Site Recovery Hizmeti uç noktalarına bağlantı kurulamıyor.

- **Çözümleme**
  - Azure Site Recovery gerekli erişim [Site kurtarma IP aralıkları](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) bölgeye bağlı olarak. Bu gerekli IP aralıkları sanal makineden erişilebilir olduğundan emin olun.


### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Sorun 4: A2A çoğaltma, ağ trafiği şirket içi ara sunucu üzerinden geçtiğinde başarısız oldu (151072)
- **Olası nedeni** </br>
  - Özel ara sunucu ayarları geçersiz ve Azure Site Recovery Mobility hizmeti Aracısı IE 'den proxy ayarlarını otomatik olarak algılamadı


- **Çözümleme**
  1. Mobility hizmeti aracısı için proxy ayarlarını Windows üzerinde IE ve Linux'ta /etc/environment algılar.
  2. Proxy 'yi yalnızca Azure Site Recovery Mobility hizmeti için ayarlamayı tercih ediyorsanız, şu adreste bulunan ProxyInfo. conf dosyasında proxy ayrıntılarını sağlayabilirsiniz:</br>
     - ``/usr/local/InMage/config/`` üzerinde ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` üzerinde ***Windows***
  3. ProxyInfo.conf proxy ayarlarını aşağıdaki INI biçiminde olmalıdır.</br>
                *[proxy]*</br>
                *Adres =http://1.2.3.4*</br>
                *Bağlantı noktası 567 =*</br>
  4. Azure Site Recovery Mobility hizmeti Aracısı yalnızca ***kimliği doğrulanmamış proxy 'leri***destekler.


### <a name="fix-the-problem"></a>Sorunu
[Gerekli URL 'lere](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıklarına](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)izin vermek için [Ağ Kılavuzu belgesindeki](site-recovery-azure-to-azure-networking-guidance.md)adımları izleyin.

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk (hata kodu 150039) makinede bulunamadı

VM'ye yeni bir disk başlatılmalıdır.

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150039<br></br>**İleti**: Mantıksal birim numarası (LUN) (LUNValue) olan Azure veri diski (DiskName) (DiskURI), aynı LUN değerine sahip VM içinden bildirilen karşılık gelen bir diskle eşleştirilmedi. | -Yeni veri diski VM'ye bağlı, ancak başlatılmamış değildi.</br></br>-VM içindeki veri diski diski VM'ye bağlı LUN değerini doğru şekilde bildirmiyor.| Veri disklerini başlatılır ve ardından işlemi yeniden deneyin emin olun.</br></br>Windows için: [Yeni bir disk ekleyin ve başlatın](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Linux için: [Linux 'ta yeni bir veri diski başlatın](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Sorunu
Veri disklerinin başlatıldığından ve sonra işlemi yeniden deneyin emin olun:

- Windows için: [Yeni bir disk ekleyin ve başlatın](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Linux: [Linux'ta yeni bir veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Sorun devam ederse desteğe başvurun.

## <a name="one-or-more-disks-are-available-for-protectionerror-code-153039"></a>Bir veya daha fazla disk, koruma için kullanılabilir (hata kodu 153039)
- **Olası nedeni** </br>
  - bir veya daha fazla disk, yakın zamanda sanal makineye korumadan sonra eklendiyse. 
  - bir veya daha fazla disk, sanal makinenin korumasından sonra başlatıldıktan sonra.

### <a name="fix-the-problem"></a>Sorunu
Diskleri korumayı seçebilir ya da sanal makinenin çoğaltma durumunu sağlıklı bir şekilde yeniden yapmak için uyarıyı yoksayabilirsiniz.</br>
1. Diskleri korumak için. Çoğaltılan öğeler > VM > disklere gidin > korumasız diske tıklayın > çoğaltmayı etkinleştirin.
 ![add_disks](./media/azure-to-azure-troubleshoot-errors/add-disk.png)
2. Uyarıyı kapatmak için. Çoğaltılan öğeler > VM 'ye gidin > genel bakış bölümünde uyarıyı Kapat ' a tıklayın.
![dismiss_warning](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)


## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information--error-code-150225"></a>Sanal makineyi kasadan kaldır ile tamamlandı (hata kodu 150225)
Sanal makineyi koruma sırasında Azure Site Recovery kaynak sanal makinede bazı bağlantılar oluşturur. Korumayı kaldırdığınızda veya çoğaltmayı devre dışı bıraktığınızda, temizleme işinin bir parçası olarak bu bağlantıları kaldırın Azure Site Recovery. Sanal makinenin kaynak kilidi varsa, iş bilgilerle tamamlanır. Sanal makinenin kurtarma hizmetleri kasasından kaldırıldığını, ancak eski bağlantıların bazılarının kaynak makineden temizlendiğinden emin olduğunu söyler.

Daha sonra bu sanal makineyi daha sonra korumayı düşünmüyorsanız, bu uyarıyı yoksayabilirsiniz. Ancak, bu sanal makineyi daha sonra korumanız gerekiyorsa aşağıdaki adımlarda bahsedilen bağlantıları temizlemeniz gerekir. 

**Temizleme yapmadıysanız:**

1.  Kurtarma Hizmetleri Kasası aracılığıyla çoğaltmayı etkinleştirme sırasında sanal makine listelenmez. 
2.  VM 'yi **sanal makine > ayarları aracılığıyla korumaya çalışırsanız, olağanüstü durum kurtarma >** , "*VM 'deki mevcut eski kaynak bağlantıları nedeniyle çoğaltma etkinleştirilemiyor*" hatasıyla başarısız olur.


### <a name="fix-the-problem"></a>Sorunu

>[!NOTE]
>
>Azure Site Recovery kaynak sanal makineyi silmez veya aşağıdaki adımları gerçekleştirirken herhangi bir şekilde etkilemez.
>

1. Kilidi VM veya VM kaynak grubundan kaldırın. Örneğin: "MoveDemo" adlı VM adının altında silinmesi gereken kaynak kilidi vardır.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Betiği indir [eski Azure Site Recovery yapılandırmasını kaldır](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. *Cleanup-Stale-ASR-config-Azure-VM. ps1*betiğini yürütün.
4. Abonelik KIMLIĞI, VM kaynak grubu ve VM adını bir parametre olarak belirtin.
5. Azure kimlik bilgileri sorulursa lütfen bunu sağlayın ve betiğin herhangi bir hatadan dolayı yürütüldüğünü kontrol edin. 


## <a name="replication-cannot-be-enabled-because-of-the-existing-stale-resource-links-on-the-vm-error-code-150226"></a>VM 'deki mevcut eski kaynak bağlantıları nedeniyle çoğaltma etkinleştirilemiyor (hata kodu 150226)

**Neden: Sanal makinede eski yapılandırma, önceki Site Recovery korumadan kalmadı**

Eski yapılandırmayı aşağıdaki durumlarda bir Azure sanal makinesinde kalmış olabilir:

- Site Recovery kullanarak Azure VM için çoğaltmayı etkinleştirdiniz ve sonra çoğaltmayı devre dışı bıraktığınızda, ancak **kaynak VM 'nin bir kaynak kilidi vardı**.
- Site Recovery kullanarak Azure VM için çoğaltma etkin ve ardından Site Recovery kasası açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan silinir.
- Site Recovery kullanarak Azure VM için çoğaltma etkin ve sonra açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan Site Recovery kasasını içeren kaynak grubu silindi.

### <a name="fix-the-problem"></a>Sorunu

>[!NOTE]
>
>Azure Site Recovery kaynak sanal makineyi silmez veya aşağıdaki adımları gerçekleştirirken herhangi bir şekilde etkilemez.


1. Varsa, VM veya VM kaynak grubundan kilidi kaldırın. *Örneğin:* "MoveDemo" adlı VM adının altında silinmesi gereken kaynak kilidi vardır.
   
   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Betiği indir [eski Azure Site Recovery yapılandırmasını kaldır](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. *Cleanup-Stale-ASR-config-Azure-VM. ps1*betiğini yürütün.
4. Abonelik KIMLIĞI, VM kaynak grubu ve VM adını bir parametre olarak belirtin.
5. Azure kimlik bilgileri sorulursa lütfen bunu sağlayın ve betiğin herhangi bir hatadan dolayı yürütüldüğünü kontrol edin.  

## <a name="unable-to-see-the-azure-vm-or-resource-group--for-selection-in-enable-replication"></a>"Çoğaltmayı etkinleştir" bölümünde seçim için Azure VM veya kaynak grubu görüntülenemiyor

 **Neden 1:  Kaynak grubu ve kaynak sanal makine farklı bir konumda**
 
Kaynak bölgesinin kaynak grubu ve sanal makinelerin aynı konumda olması gereken şu anda Azure Site Recovery. Böyle bir durum söz konusu değilse, koruma sırasında sanal makineyi veya kaynak grubunu bulamamanız mümkün değildir. 

**Geçici bir çözüm olarak**, kurtarma hizmetleri KASASı yerine VM 'Den çoğaltmayı etkinleştirebilirsiniz. Kaynak VM > Özellikler > olağanüstü durum kurtarma ' ya gidin ve çoğaltmayı etkinleştirin.

**Neden 2: Kaynak grubu, seçili aboneliğin bir parçası değil**

Belirtilen abonelik bir parçası değilse, kaynak grubunu koruma süresi bulmak mümkün olmayabilir. Kaynak grubu kullanılıyor aboneliğe ait olduğundan emin olun.

 **Neden 3: Eski yapılandırma**
 
VM için çoğaltmayı etkinleştirmek istediğiniz görmüyorsanız, Azure sanal makinesinde eski bir Site Recovery yapılandırması nedeniyle kalmayabilir. Eski yapılandırmayı aşağıdaki durumlarda bir Azure sanal makinesinde kalmış olabilir:

- Site Recovery kullanarak Azure VM için çoğaltma etkin ve ardından Site Recovery kasası açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan silinir.
- Site Recovery kullanarak Azure VM için çoğaltma etkin ve sonra açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan Site Recovery kasasını içeren kaynak grubu silindi.

- Site Recovery kullanarak Azure VM için çoğaltmayı etkinleştirdiniz ve sonra çoğaltmayı devre dışı bıraktığınızda, ancak kaynak VM 'nin bir kaynak kilidi vardı.

### <a name="fix-the-problem"></a>Sorunu

> [!NOTE]
>
> Aşağıdaki betiği kullanmadan önce "" Azurere. resources "" modülünü güncelleştirdiğinizden emin olun. Azure Site Recovery kaynak sanal makineyi silmez veya aşağıdaki adımları gerçekleştirirken herhangi bir şekilde etkilemez.
>

1. Varsa, VM veya VM kaynak grubundan kilidi kaldırın. *Örneğin:* "MoveDemo" adlı VM adının altında silinmesi gereken kaynak kilidi vardır.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Betiği indir [eski yapılandırmayı kaldır](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
3. *Cleanup-Stale-ASR-config-Azure-VM. ps1*betiğini yürütün.
4. Abonelik KIMLIĞI, VM kaynak grubu ve VM adını bir parametre olarak belirtin.
5. Azure kimlik bilgileri sorulursa lütfen bunu sağlayın ve betiğin herhangi bir hatadan dolayı yürütüldüğünü kontrol edin.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Sanal makine koruma için işaretleyin yapılamıyor
 **Neden 1:  Sanal makinede bir uzantı başarısız veya yanıt vermeyen bir durumda yüklü** <br>
 Sanal makineler gidin > ayarı > Uzantılar ve başarısız durumda herhangi bir uzantısı var olup olmadığını denetleyin. Başarısız uzantının yüklemesini kaldırmak ve sanal makine korumayı yeniden deneyin.<br>
 **Neden 2:  [VM 'nin sağlama durumu geçerli değil](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>VM'in sağlama durumu geçerli değil (hata kodu 150019)

VM üzerinde çoğaltmayı etkinleştirmek için sağlama durumu olmalıdır **başarılı**. Aşağıdaki adımları izleyerek VM durumunu kontrol edebilirsiniz.

1.  Seçin **kaynak Gezgini** gelen **tüm hizmetleri** Azure portalında.
2.  Genişletin **abonelikleri** listelemek ve aboneliğinizi seçin.
3.  Genişletin **ResourceGroups** listeleyebilir ve sanal makinenin kaynak grubunu seçin.
4.  Genişletin **kaynakları** listesinde ve sanal makinenizi seçin
5.  Denetleme **provisioningState** örneği görünümünde, sağ taraftaki alan.

### <a name="fix-the-problem"></a>Sorunu

- Varsa **provisioningState** olduğu **başarısız**, sorun giderme ayrıntıları ile Destek ekibiyle iletişime geçin.
- Varsa **provisioningState** olduğu **güncelleştirme**, başka bir uzantı dağıtılabilecek. Bekleme başarısız Site kurtarma işlemini yeniden deneyin ve bunlar için VM üzerinde devam eden herhangi bir işlem olup olmadığını kontrol **çoğaltmayı etkinleştir** işi.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Sanal ağ - ağ seçimi sekmesi gri renkte hedefi seçmek yüklenemiyor.

**Neden 1: VM 'niz zaten bir ' hedef ağ ' ile eşlenmiş bir ağa eklenmişse.**
- Kaynak VM sanal ağ bir parçasıdır ve hedef kaynak grubunda bir ağ ile aynı sanal ağdaki başka bir VM'den zaten eşlenmiş, ardından tarafından varsayılan ağ seçimi açılan menüsü devre dışı bırakılır.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Neden 2: Daha önce Azure Site Recovery kullanarak VM 'yi koruduysanız ve çoğaltmayı devre dışı bırakmış olursunuz.**
 - Sanal Makinenin çoğaltmasını devre dışı bırakma ağ eşlemesi silmez. Burada VM korunan kurtarma Hizmetleri kasası silinmesi gerekir. </br>
 Kurtarma hizmeti Kasası'na gidin > Site Recovery altyapısı > Ağ eşlemesi. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Olağanüstü durum kurtarma kurulumu sırasında yapılandırılan hedef ağı, VM korunmaya başladıktan sonra ilk sonra değiştirilebilir. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Bu belirli ağ eşlemesi kullanan VM'ler korumalı ağ eşlemesini değiştirmek tüm etkilediğini unutmayın.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ Birim Gölge Kopyası Hizmeti hatası (hata kodu 151025)

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151025<br></br>**İleti**: Site Recovery uzantısı yüklenemedi | -'COM + Sistem uygulaması' hizmeti devre dışı.</br></br>-'Birim gölge kopyası' hizmeti devre dışı bırakıldı.| 'COM + Sistem uygulaması' ve 'Birim gölge kopyası' hizmetlerini otomatik veya el ile başlatma moduna ayarlayın.

### <a name="fix-the-problem"></a>Sorunu

'Hizmetler' konsolunu açın ve 'COM + Sistem uygulaması' emin olun ve 'Birim gölge kopyası', 'Başlangıç türü' 'Devre dışı' olarak ayarlanmamış.
  ![COM hatası](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Desteklenmeyen yönetilen disk boyutu (hata kodu 150172)


**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150172<br></br>**İleti**: Desteklenen en düşük boyut olan 1024 MB 'tan küçük olan sanal makine (DiskName) boyutu (DiskSize) olduğundan, koruma etkinleştirilemedi. | -Disk desteklenen boyut olan 1024 MB 'tan küçük| Disk boyutlarının desteklenen boyut aralığında olduğundan emin olun ve işlemi yeniden deneyin.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>Koruma etkinleştirme, (hata kodu 151126), GRUB yapılandırmasında belirtilen cihaz adı olarak başarısız oldu.

**Olası neden:** </br>
GRUB yapılandırma dosyaları ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/Boot/GRUB2/grub.cfg" veya "/etc/default/grub"), parametre **kökünün** değerini IÇEREBILIR ve UUID yerine gerçek cihaz adları olarak **sürdürülür** . VM 'nin yeniden başlatılması sırasında cihaz adı değişebilir, çünkü yük devretme sırasında sorun ortaya çıkan aynı ada sahip sanal makine aynı ada sahip olmayabilir. Site Recovery Örneğin: </br>


- Aşağıdaki satır, GRUB dosyası **/Boot/GRUB2/grub.cfg**. <br>
  *Linux/Boot/vmlinuz-3.12.49-11-Default **root =/dev/sda2** $ {extra_cmdline} **özgeçmişi =/dev/sda1** Splash = sessiz sessiz showopts*


- GRUB dosyasıdır aşağıdaki satırı **/boot/grub/menu.lst**
  *çekirdek /boot/vmlinuz-3.0.101-63-default **kök = / dev/sda2** **= / dev/sda1 Sürdür** splash sessiz crashkernel = 256M-:128M showopts vga = 0x314 =*

Yukarıdaki kalın dizeyi gözlemlerseniz, GRUB 'nin UUID yerine "root" ve "özgeçmişi" parametrelerinin gerçek cihaz adları vardır.

**Nasıl düzeltilir:**<br>
Cihaz adları karşılık gelen UUID ile değiştirilmelidir.<br>


1. "Blkıd \<cihaz adı >" komutunu yürüterek cihazın UUID 'sini bulun. Örneğin:<br>
   ```
   blkid /dev/sda1
   ```<br>
   ```/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap" ```<br>
   ```blkid /dev/sda2```<br>
   ```/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```<br>
   ```



1. Şimdi, cihaz adını "root = UUID =\<UUID >" biçimindeki biçimde UUID ile değiştirin. Örneğin, "/Boot/GRUB2/grub.cfg", "/Boot/GRUB2/grub.cfg" veya "/etc/default/grub" dosyalarında yukarıda bahsedilen root ve özgeçmişi parametresi için, cihaz adlarını UUID ile değiştirdiğinizden, dosyalardaki satırlar benzer şekilde görünür. <br>
   *çekirdek/Boot/vmlinuz-3.0.101-63-Default **root = UUID = 62927e85-f7ba-40bc-9993-cc1feeb191e4** **özgeçmişi = UUID = 6f614b44-433b-431b-9ca1-4dd2f6f74f6b** tanıtım = sessiz crashkernel = 256D-: 128mshowopts VGA = 0x314*
1. Korumayı yeniden başlatın

## <a name="enable-protection-failed-as-device-mentioned-in-the-grub-configuration-doesnt-existerror-code-151124"></a>GRUB yapılandırmasında belirtilen cihaz mevcut olmadığından korumayı etkinleştirme başarısız oldu (hata kodu 151124)
**Olası neden:** </br>
GRUB yapılandırma dosyaları ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/Boot/GRUB2/grub.cfg" veya "/etc/default/grub"), önyükleme sırasında bulunması gereken LVM cihazını göstermek için "rd.lvm.lv" veya "rd_LVM_LV" parametrelerini içerebilir. Bu LVM cihazları yoksa, korunan sistem önyükleme sürecine önyükleme yapmaz ve takılmaz. Aynı bile yük devretme VM 'si ile de izlenir. Aşağıda birkaç örnek verilmiştir:

Birkaç örnek: </br>

1. Aşağıdaki satır, RHEL7 üzerinde **"/Boot/GRUB2/grub.cfg"** grub dosyasından aşağıda verilmiştir. </br>
   *linux16/vmlinuz-3.10.0-957.EL7.x86_64 root =/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel = 128gb\@64D **RD. LVM. lv = rootvg/root Rd. LVM. lv = rootvg/takas** rhgb quiet lang = en_US. UTF-8*</br>
   Burada vurgulanan bölüm, GRUB 'nin "rootvg" birim grubundan **"root"** ve **"swap"** adlarına sahip iki LVM cihazını algılaması gerektiğini gösterir.
1. Aşağıdaki satır, RHEL7 üzerindeki **"/etc/default/grub"** adlı grub dosyasından </br>
   *GRUB_CMDLINE_LINUX = "crashkernel = Auto **RD. LVM. lv = rootvg/root Rd. LVM. lv = rootvg/takas** rhgb quiet"*</br>
   Burada vurgulanan bölüm, GRUB 'nin "rootvg" birim grubundan **"root"** ve **"swap"** adlarına sahip iki LVM cihazını algılaması gerektiğini gösterir.
1. Aşağıdaki satır, RHEL6 üzerinde **"/boot/grub/menu.lst"** grub dosyasından </br>
   *çekirdek/vmlinuz-2.6.32-754.el6.x86_64 ro root = UUID = 36dd8b45-E90D-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG = en_US. UTF-8 rd_NO_MD SYSFONT = latarcyrheb-sun16 crashkernel = Auto rd_LVM_LV = rootvg/lv_root KEYBOARDTYPE = PC KEYTABLE = US rd_LVM_LV = rootvg/lv_swap rd_NO_DM rhgb quiet* </br>
   Burada vurgulanan bölüm, GRUB 'nin "rootvg" birim grubundan **"root"** ve **"swap"** adlarına sahip iki LVM cihazını algılaması gerektiğini gösterir.<br>

**Nasıl düzeltilir:**<br>

LVM cihazı yoksa, oluşturarak ya da GRUB yapılandırma dosyalarından aynı için parametreyi kaldırıp, ardından korumayı etkinleştir ' i yeniden deneyin. </br>

## <a name="site-recovery-mobility-service-update-completed-with-warnings--error-code-151083"></a>Site Recovery Mobility hizmeti güncelleştirmesi uyarılarla tamamlandı (hata kodu 151083)
Site Recovery Mobility hizmetinde, biri filtre sürücüsü olarak adlandırılan birçok bileşen vardır. Filtre sürücüsü sistem belleğine yalnızca sistem yeniden başlatma sırasında yüklenir. Filtre sürücüsü değişiklikleri olan Site Recovery Mobility hizmeti güncelleştirmesi olduğunda, makineyi güncelleştiririz, ancak yine de bazı düzeltmelerin yeniden başlatma gerektirdiğini belirten bir uyarı verir. Bu, filtre sürücüsü düzeltmelerinin yalnızca sistemin yeniden başlatılması sırasında gerçekleşebilecek yeni bir filtre sürücüsü yüklendiğinde gerçekleştirilme anlamına gelir.<br>
**Lütfen** bu yalnızca bir uyarı olduğunu ve yeni aracı güncelleştirmesinden sonra bile devam eden çoğaltmanın çalıştığını unutmayın. Yeni filtre sürücüsünün avantajlarından yararlanmak istediğiniz zaman yeniden başlatmayı seçebilirsiniz, ancak yeniden başlatma sonrasında eski filtre sürücüsü çalışmaya devam eder. Filtre sürücüsünden ayrı olarak, **Mobility hizmetindeki diğer geliştirmelerin ve düzeltmelerin avantajları, aracı güncelleştirildiği zaman herhangi bir yeniden başlatma olmadan gerçekleştirilir.**  


## <a name="protection-couldnt-be-enabled-as-replica-managed-disk-diskname-replica-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>' Diskname-Replica ' çoğaltma yönetilen diski hedef kaynak grubunda beklenen Etiketler olmadan zaten var olduğundan koruma etkinleştirilemedi (hata kodu 150161

**Neden**: Sanal makine geçmişte daha önce korunmuşsa ve çoğaltmayı devre dışı bırakırken bu durum oluşabilir. çoğaltma diski bir nedenden dolayı temizlenmez.</br>
**Nasıl düzeltilir:** Hata iletisindeki belirtilen çoğaltma diskini silin ve başarısız koruma işini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure sanal makinelerini çoğaltma](site-recovery-replicate-azure-to-azure.md)
