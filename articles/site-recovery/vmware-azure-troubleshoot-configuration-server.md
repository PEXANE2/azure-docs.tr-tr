---
title: Azure Site Kurtarma ' yı kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure' a olağanüstü kurtarma sırasında yapılandırma sunucusuyla ilgili sorunları giderme | Microsoft Dokümanlar
description: Bu makalede, Azure Site Kurtarma kullanarak VMware VM'lerin ve fiziksel sunucuların olağanüstü durum kurtarma için yapılandırma sunucusunu Azure'a dağıtması için sorun giderme bilgileri sağlanmaktadır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/13/2019
ms.author: ramamill
ms.openlocfilehash: 0383a512dfb7c2bb1ae2422b9ade1e3c7387a70c
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478314"
---
# <a name="troubleshoot-configuration-server-issues"></a>Yapılandırma sunucusu sorunlarını giderme

Bu makale, [Azure Site Kurtarma](site-recovery-overview.md) yapılandırma sunucusunu dağıtırken ve yönetirken sorunları gidermenize yardımcı olur. Yapılandırma sunucusu bir yönetim sunucusu gibi davranır. Site Kurtarma'yı kullanarak şirket içi VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü durum kurtarmasını ayarlamak için yapılandırma sunucusunu kullanın. Aşağıdaki bölümlerde, yeni bir yapılandırma sunucusu eklediğinizde ve bir yapılandırma sunucusunu yönetirken karşılaşabileceğiniz en yaygın hataları tartışır.

## <a name="registration-failures"></a>Kayıt hataları

Mobilite aracısını yüklediğinizde kaynak makine yapılandırma sunucusuna kaydeder. Bu adımda aşağıdaki yönergeleri izleyerek hataları ayıklayabilirsiniz:

1. C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log dosyasını açın. (ProgramData klasörü gizli bir klasör olabilir. ProgramData klasörünü, Dosya Gezgini'nde, **Görünüm** sekmesinde, **Göster/gizle** bölümünde görmüyorsanız, **Gizli öğeler** onay kutusunu seçin.) Hatalar birden çok sorundan kaynaklanabilir.

2. Geçerli IP Adresi bulunamadı dizesini **arayın.** Dize bulunursa:
   1. İstenen ana bilgisayar kimliğinin kaynak makinenin ana bilgisayar kimliğiyle aynı olduğunu doğrulayın.
   2. Kaynak makinenin fiziksel NIC'e atanmış en az bir IP adresi olduğunu doğrulayın. Yapılandırma sunucusuna aracı kaydının başarılı olabilmesi için kaynak makinenin fiziksel NIC'ye atanmış en az bir geçerli IP v4 adresi olması gerekir.
   3. Kaynak makinenin tüm IP adreslerini almak için kaynak makinede aşağıdaki komutlardan birini çalıştırın:
      - Windows için:`> ipconfig /all`
      - Linux için:`# ifconfig -a`

3. **No Valid IP Adresi bulunan** dize bulunamazsa, String **Reason=>NULL'u**arayın. Kaynak makine yapılandırma sunucusuna kaydolmak için boş bir ana bilgisayar kullanıyorsa bu hata oluşur. Dize bulunursa:
    - Sorunları çözdükten sonra, kaydı el ile yeniden denemek için [kaynak makineyi yapılandırma sunucusuna kaydet](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) yönergelerini izleyin.

4. **String Reason=>NULL** bulunamazsa, kaynak makinede C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log dosyasını açın. (ProgramData klasörü gizli bir klasör olabilir. ProgramData klasörünü, Dosya Gezgini'nde, **Görünüm** sekmesinde, **Göster/gizle** bölümünde görmüyorsanız, **Gizli öğeler** onay kutusunu seçin.) Hatalar birden çok sorundan kaynaklanabilir. 

5. String sonrası **isteği arayın: (7) - Sunucuya bağlanamadı.** Dize bulunursa:
    1. Kaynak makine ve yapılandırma sunucusu arasındaki ağ sorunlarını giderin. Yapılandırma sunucusuna ping, izleme yolu veya web tarayıcısı gibi ağ araçlarını kullanarak kaynak makineden erişilebildiğini doğrulayın. Kaynak makinenin 443 portu üzerinden yapılandırma sunucusuna erişebilmesini sağlayın.
    2. Kaynak makinedeki güvenlik duvarı kurallarının kaynak makine ile yapılandırma sunucusu arasındaki bağlantıyı engelleyip engellemediğini denetleyin. Bağlantı sorunlarını kaldırmak için ağ yöneticilerinizle birlikte çalışın.
    3. Site Kurtarma klasöründe listelenen klasörlerin [virüsten koruma programlarından hariç](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) tutulmasını sağlayın.
    4. Ağ sorunları çözüldüğünde, [kaynak makineyi yapılandırma sunucusuna kaydedin'deki](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server)yönergeleri izleyerek kaydı yeniden deneyin.

6. Dize **sonrası isteği: (7) - Sunucuya bağlanamayan,** aynı günlük dosyasında, dize **isteğine bakın: (60) - Eş sertifikası verilen CA sertifikalarıyla doğrulanamaz.** Yapılandırma sunucusu sertifikasının süresi dolduğunda veya kaynak makine TLS 1.0 veya daha sonraki protokolleri desteklemediği için bu hata oluşabilir. Bir güvenlik duvarı kaynak makine ve yapılandırma sunucusu arasındaki TLS iletişimini engellerse de oluşabilir. Dize bulunursa: 
    1. Çözmek için, kaynak makinede bir web tarayıcısı kullanarak yapılandırma sunucusu IP adresine bağlanın. URI https:\/ \/<yapılandırma sunucusu\>IP adresini kullanın :443/. Kaynak makinenin 443 portu üzerinden yapılandırma sunucusuna erişebilmesini sağlayın.
    2. Kaynak makinenin yapılandırma sunucusuyla konuşması için kaynak makineye güvenlik duvarı kurallarının eklenmesi veya kaldırılması gerekip gerekmediğini denetleyin. Kullanımda olabilecek güvenlik duvarı yazılımı çeşitliliği nedeniyle, gerekli tüm güvenlik duvarı yapılandırmalarını listelayamayız. Bağlantı sorunlarını kaldırmak için ağ yöneticilerinizle birlikte çalışın.
    3. Site Kurtarma klasöründe listelenen klasörlerin [virüsten koruma programlarından hariç](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) tutulmasını sağlayın.  
    4. Sorunları çözdükten sonra, [kaynak makineyi yapılandırma sunucusuna kaydedin'deki](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server)yönergeleri izleyerek kaydı yeniden deneyin.

7. Linux'ta, <INSTALLATION_DIR/etc/drscout.conf'taki\>platformun değeri bozulursa, kayıt başarısız olur. Bu sorunu tanımlamak için /var/log/ua_install.log dosyasını açın. VM_PLATFORM değeri **null veya VmWare / Azure değil gibi**dize İptal yapılandırması arayın. Platform **VmWare** veya **Azure**olarak ayarlanmalıdır. Drscout.conf dosyası bozuksa, [hareket aracısını kaldırmanızı](vmware-physical-manage-mobility-service.md#uninstall-mobility-service) ve ardından hareket aracısını yeniden yüklemenizi öneririz. Yükleme yi kaldırma başarısız olursa, aşağıdaki adımları tamamlayın: a. Installation_Directory/uninstall.sh dosyasını açın ve **Çağrıyı StopServices** işlevine yorum yapın.
    b. Installation_Directory/Vx/bin/uninstall.sh dosyasını açın ve **stop_services** işlevine yapılan çağrıyı yorumla.
    c. Installation_Directory/Fx/uninstall.sh dosyasını açın ve Fx hizmetini durdurmaya çalışan bölümün tamamını yorumla.
    d. Hareket aracısını [kaldırın.](vmware-physical-manage-mobility-service.md#uninstall-mobility-service) Başarılı bir şekilde yüklemeyi kaldırmadıktan sonra sistemi yeniden başlatın ve ardından hareket aracısını yeniden yüklemeyi deneyin.

## <a name="installation-failure-failed-to-load-accounts"></a>Yükleme hatası: Hesapları yüklemek için başarısız oldu

Bu hata, hizmet, hareket aracısını yüklerken ve yapılandırma sunucusuna kaydolurken aktarım bağlantısındaki verileri okuyadığında oluşur. Sorunu çözmek için kaynak makinenizde TLS 1.0'ın etkin olduğundan emin olun.

## <a name="vcenter-discovery-failures"></a>vCenter bulma hataları

vCenter bulma hatalarını gidermek için vCenter sunucusunu byPass listesi proxy ayarlarına ekleyin. 

- Sistem kullanıcı içeriğine erişmek için PsExec aracını [buradan](https://aka.ms/PsExec) indirin.
- Aşağıdaki komut satırı psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe" çalıştırarak sistem kullanıcı içeriğinde Açık Internet Explorer
- IE proxy ayarları ekleyin ve tmanssvc hizmetini yeniden başlatın.
- DRA proxy ayarlarını yapılandırmak için CD C:\Program Files\Microsoft Azure Site Kurtarma Sağlayıcısı'nı çalıştırın
- Sonra, DRCONFIGURATOR çalıştırın. EXE /configure /AddBypassUrls [[Configuration Server dağıtımının](vmware-azure-deploy-configuration-server.md#configure-settings) **vCenter Server/vSphere ESXi sunucu** adımı sırasında sağlanan vCenter Server'ın IP Adresi/FQDN ekle ]

## <a name="change-the-ip-address-of-the-configuration-server"></a>Yapılandırma sunucusunun IP adresini değiştirme

Bir yapılandırma sunucusunun IP adresini değiştirmemenizi öneririz. Yapılandırma sunucusuna atanan tüm IP adreslerinin statik IP adresleri olduğundan emin olun. DHCP IP adreslerini kullanmayın.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML belirteci geçersizdir

Bu hatayı önlemek için, sistem saatinizdeki saatin yerel saatten 15 dakikadan fazla farklı olmadığından emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.

## <a name="failed-to-create-a-certificate"></a>Sertifika oluşturulamadı

Site Kurtarma kimliğini doğrulamak için gereken bir sertifika oluşturulamıyor. Kurulumu yerel yönetici olarak çalıştırdığınızdan emin olduktan sonra yeniden çalıştırın.

## <a name="failure-to-activate-windows-license-from-server-standard-evaluation-to-server-standard"></a>Windows Lisansının Sunucu Standart DEĞERLENDİrMEsİnDEN Server Standardına etkinleştirilmemesi

1. OVF aracılığıyla Yapılandırma sunucusu dağıtımının bir parçası olarak, 180 gün boyunca geçerli olan bir değerlendirme lisansı kullanılır. Bu lisansın süresi dolmadan önce bu Lisansı etkinleştirmeniz gerekir. Aksi takdirde, bu yapılandırma sunucusunun sık sık kapanmasına neden olabilir ve böylece çoğaltma etkinliklerinin engeline neden olabilir.
2. Windows lisansını etkinleştiremiyorsanız, sorunu çözmek için [Windows destek ekibine](https://aka.ms/Windows_Support) ulaşın.

## <a name="register-source-machine-with-configuration-server"></a>Kaynak makineyi yapılandırma sunucusuna kaydettirme

### <a name="if-the-source-machine-runs-windows"></a>Kaynak makine Windows çalıştırıyorsa

Kaynak makinede aşağıdaki komutu çalıştırın:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
```

Ayar | Ayrıntılar
--- | ---
Kullanım | UnifiedAgentConfigurator.exe /CSEndPoint <yapılandırma\> sunucusu IP adresi /PassphraseFilePath <passphrase dosya yolu\>
Aracı yapılandırma günlükleri | %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında bulunur.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parolanın konumu. Geçerli UNC veya yerel dosya yolunu kullanın.

### <a name="if-the-source-machine-runs-linux"></a>Kaynak makine Linux çalıştırAn

Kaynak makinede aşağıdaki komutu çalıştırın:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Ayar | Ayrıntılar
--- | ---
Kullanım | cd /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i <yapılandırma\> sunucusu IP adresi -P <passphrase dosya yolu\>
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
-P |  Zorunlu. Parolanın kaydedildiği dosyanın tam dosya yolu. Geçerli bir klasör kullanın.

## <a name="unable-to-configure-the-configuration-server"></a>Yapılandırma sunucusunu yapılandıramıyor

Sanal makineye yapılandırma sunucusu dışındaki uygulamaları yüklerseniz, ana hedefi yapılandıramayabilirsiniz. 

Yapılandırma sunucusu tek amaçlı bir sunucu olmalıdır ve paylaşılan sunucu olarak kullanmak desteklenmez. 

Daha fazla bilgi [için, bir yapılandırma sunucusu dağıt'ta](vmware-azure-deploy-configuration-server.md#faqs)yapılandırma SSS bakın. 

## <a name="remove-the-stale-entries-for-protected-items-from-the-configuration-server-database"></a>Korumalı öğeler için eski girişleri yapılandırma sunucusu veritabanından kaldırma 

Yapılandırma sunucusundaki eski korumalı makineyi kaldırmak için aşağıdaki adımları kullanın. 
 
1. Bayat girişin kaynak makinesini ve IP adresini belirlemek için: 

    1. MYSQL cmdline'ı yönetici modunda açın. 
    2. Aşağıdaki komutları uygulayın. 
   
        ```
        mysql> use svsdb1;
        mysql> select id as hostid, name, ipaddress, ostype as operatingsystem, from_unixtime(lasthostupdatetime) as heartbeat from hosts where name!='InMageProfiler'\G;
        ```

        Bu, kayıtlı makinelerin listesini IP adresleri ve son kalp atışı ile birlikte döndürür. Eski çoğaltma çiftleri olan ana bilgisayarı bulun.

2. Yükseltilmiş bir komut istemiaçın ve C:\ProgramData\ASR\home\svsystems\bin'e gidin. 
4. Kayıtlı ana bilgisayar ayrıntılarını ve eski giriş bilgilerini yapılandırma sunucusundan kaldırmak için kaynak makineyi ve bayat girişip adresini kullanarak aşağıdaki komutu çalıştırın. 
   
    `Syntax: Unregister-ASRComponent.pl -IPAddress <IP_ADDRESS_OF_MACHINE_TO_UNREGISTER> -Component <Source/ PS / MT>`
 
    10.0.0.4 ip adresine sahip "OnPrem-VM01" kaynağı sunucu girişiniz varsa, bunun yerine aşağıdaki komutu kullanın.
 
    `perl Unregister-ASRComponent.pl -IPAddress 10.0.0.4 -Component Source`
 
5. Yapılandırma sunucusuna yeniden kaydolmak için aşağıdaki hizmetleri kaynak makinede yeniden başlatın. 
 
    - InMage Scout Uygulaması Hizmeti
    - InMage Scout VX Ajan - Sentinel / Karakol

## <a name="upgrade-fails-when-the-services-fail-to-stop"></a>Hizmetler durmazsa yükseltme başarısız olur

Yapılandırma sunucusu yükseltmesi, belirli hizmetler durmayınca başarısız olur. 

Sorunu tanımlamak için yapılandırma sunucusundaki C:\ProgramData\ASRSetupLogs\CX_TP_InstallLogFile gidin. Aşağıdaki hataları bulursanız, sorunu gidermek için aşağıdaki adımları kullanın: 

    2018-06-28 14:28:12.943   Successfully copied php.ini to C:\Temp from C:\thirdparty\php5nts
    2018-06-28 14:28:12.943   svagents service status - SERVICE_RUNNING
    2018-06-28 14:28:12.944   Stopping svagents service.
    2018-06-28 14:31:32.949   Unable to stop svagents service.
    2018-06-28 14:31:32.949   Stopping svagents service.
    2018-06-28 14:34:52.960   Unable to stop svagents service.
    2018-06-28 14:34:52.960   Stopping svagents service.
    2018-06-28 14:38:12.971   Unable to stop svagents service.
    2018-06-28 14:38:12.971   Rolling back the install changes.
    2018-06-28 14:38:12.971   Upgrade has failed.

Sorunu gidermek için:

Aşağıdaki hizmetleri el ile durdurun:

- cxprocessserver
- InMage Scout VX Ajan - Sentinel / Karakol, 
- Microsoft Azure Kurtarma Hizmetleri Aracısı, 
- Microsoft Azure Site Kurtarma Hizmeti, 
- tmansvc
  
Yapılandırma sunucusunu güncelleştirmek [için birleşik kurulumu](service-updates-how-to.md#links-to-currently-supported-update-rollups) yeniden çalıştırın.

## <a name="azure-active-directory-application-creation-failure"></a>Azure Etkin Dizin uygulama oluşturma hatası

[Açık Sanallaştırma Uygulaması (OVA)](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template
) şablonunu kullanarak Azure Etkin Dizini'nde (AAD) bir uygulama oluşturmak için yeterli izniniz yoktur.

Sorunu çözmek için Azure portalında oturum açın ve aşağıdakilerden birini yapın:

- AAD'de Uygulama Geliştiricisi rolünü isteyin. Uygulama Geliştiricisi rolü hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.
- Kullanıcı'nın uygulama bayrağı **oluşturabileceğini** doğrulayın, AAD'de *gerçek olarak* ayarlanmıştır. Daha fazla bilgi için [bkz: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın.](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)

## <a name="process-servermaster-target-are-unable-to-communicate-with-the-configuration-server"></a>İşlem sunucusu/Ana Hedef yapılandırma sunucusuyla iletişim kuramıyor 

İşlem sunucusu (PS) ve Master Target (MT) modülleri yapılandırma sunucusuyla (CS) iletişim kuramıyor ve durumları Azure portalında bağlı değil miş olarak gösteriliyor.

Genellikle, bu bağlantı noktası 443 ile bir hata kaynaklanmaktadır. Bağlantı noktasının engelini kaldırmak ve CS ile yeniden iletişimi etkinleştirmek için aşağıdaki adımları kullanın.

**MARS aracısının Ana Hedef aracı tarafından çağrıldığını doğrulayın**

Ana Hedef Aracı'nın Configuration server IP için bir TCP oturumu oluşturabileceğini doğrulamak için, Ana Hedef aracı günlüklerinde aşağıdakilere benzer bir izleme arayın:

TCP \<Burada CS IP ile IP değiştirin \<>:52739 Burada CS IP ile ip değiştirin>:443 SYN_SENT 

TCP 192.168.1.40:52739 192.168.1.40:443 SYN_SENT // IP'yi CS IP ile değiştirin

MT aracı günlüklerinde aşağıdakilere benzer izler bulursanız, MT Aracısı bağlantı noktası 443'te hataları bildiriyor:

    #~> (11-20-2018 20:31:51):   ERROR  2508 8408 313 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
    #~> (11-20-2018 20:31:54):   ERROR  2508 8408 314 FAILED : PostToSVServer with error [at curlwrapper.cpp:CurlWrapper::processCurlResponse:212]   failed to post request: (7) - Couldn't connect to server
 
Diğer uygulamalar da port 443 kullanırken veya bağlantı noktasını engelleyen bir güvenlik duvarı ayarı nedeniyle bu hatayla karşılaşılabilir.

Sorunu gidermek için:

- 443 bağlantı noktasının güvenlik duvarınız tarafından engellenmediğini doğrulayın.
- Bağlantı noktası, bu bağlantı noktasını kullanan başka bir uygulama nedeniyle erişilenemezse uygulamayı durdurun ve kaldırın.
  - Uygulamayı durdurmak mümkün değilse, yeni bir temiz CS kurun.
- Yapılandırma sunucusunu yeniden başlatın.
- IIS hizmetini yeniden başlatın.

### <a name="configuration-server-is-not-connected-due-to-incorrect-uuid-entries"></a>Yapılandırma sunucusu yanlış UUID girişleri nedeniyle bağlı değil

Bu hata, veritabanında birden çok yapılandırma sunucusu (CS) örneği UUID girişi olduğunda oluşabilir. Sorun genellikle yapılandırma sunucusu VM klonlama oluşur.

Sorunu gidermek için:

1. Eski/eski CS VM'yi vCenter'dan kaldırın. Daha fazla bilgi için [bkz.](site-recovery-manage-registration-and-protection.md)
2. Yapılandırma sunucusu VM'de oturum açın ve MySQL svsdb1 veritabanına bağlanın. 
3. Aşağıdaki sorguyu yürüt:

    > [!IMPORTANT]
    >
    > Klonlanmış yapılandırma sunucusunun UUID ayrıntılarını veya artık sanal makineleri korumak için kullanılmayan yapılandırma sunucusunun eski girişini girdiğinizi doğrulayın. Yanlış bir UUID girmek, varolan tüm korumalı öğelerin bilgilerinin kayba yol açacaktır.
   
    ```
        MySQL> use svsdb1;
        MySQL> delete from infrastructurevms where infrastructurevmid='<Stale CS VM UUID>';
        MySQL> commit; 
    ```
4. Portal sayfasını yenileyin.

## <a name="an-infinite-sign-in-loop-occurs-when-entering-your-credentials"></a>Kimlik bilgilerinizi girerken döngüiçinde sonsuz bir işaret oluşur

Yapılandırma sunucusu OVF'ye doğru kullanıcı adı ve parolayı girdikten sonra, Azure oturum açma doğru kimlik bilgileri için istenmeye devam ediyor.

Sistem zamanı yanlış olduğunda bu sorun oluşabilir.

Sorunu gidermek için:

Bilgisayarda doğru zamanı ayarlayın ve oturum açmayı yeniden deneyin. 
 