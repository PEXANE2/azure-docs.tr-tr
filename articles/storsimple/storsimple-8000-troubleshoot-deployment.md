---
title: Sorun Giderme StorSimple 8000 serisi dağıtım sorunları | Microsoft Dokümanlar
description: StorSimple'ı ilk dağıtırken oluşan hataların nasıl tanılanıp düzeltileceğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: f2b454e812db1eea686f82e92841163f1129b6c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267631"
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Sorun Giderme StorBasit aygıt dağıtım sorunları
## <a name="overview"></a>Genel Bakış
Bu makale, Microsoft Azure StorBasit dağıtımınız için yararlı sorun giderme kılavuzu sağlar. StorSimple'ı yapılandırırken karşılaşabileceğiniz sorunları çözmenize yardımcı olacak sık karşılaşılan sorunları, olası nedenleri ve önerilen adımları açıklar. 

Bu bilgiler hem StorSimple 8000 serisi fiziksel cihaz hem de StorSimple Cloud Appliance için geçerlidir.

> [!NOTE]
> Aygıtı ilk kez dağıttığınızda karşılaşabileceğiniz aygıt yapılandırması ile ilgili sorunlar oluşabilir veya aygıt çalışır durumdayken daha sonra oluşabilir. Bu makalede, ilk kez dağıtım sorunları sorun giderme üzerinde duruluyor. Bir işletim aygıtının sorun giderme için, [bir operasyonel aygıtı gidermek için Tanılama aracını kullanın.](storsimple-8000-diagnostics.md)

Bu makalede, StorSimple dağıtımları sorun giderme araçları da açıklanır ve adım adım sorun giderme örneği sağlar.

## <a name="first-time-deployment-issues"></a>İlk kez dağıtım sorunları
Cihazınızı ilk kez dağıtırken bir sorunla karşılaştıysanız, aşağıdakileri göz önünde bulundurun:

* Fiziksel bir aygıtı niçin sorun giderirken, donanımın [StorSimple 8100 cihazınızı yükleyin](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızı](storsimple-8600-hardware-installation.md)yükleyin'de açıklandığı şekilde yüklendiğinden ve yapılandırıldığından emin olun.
* Dağıtım için ön koşulları denetleyin. [Dağıtım yapılandırma denetim listesinde](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist)açıklanan tüm bilgilere sahip olduğundan emin olun.
* Sorunun açıklanıp açıklanmamasını görmek için StorSimple Release Notes'u gözden geçirin. Sürüm notları, bilinen yükleme sorunları için geçici geçici geçici çözüm içerir. 

Aygıt dağıtımı sırasında, kullanıcıların karşılaştığı en yaygın sorunlar kurulum sihirbazını çalıştırırken ve aygıtı StorSimple için Windows PowerShell üzerinden kaydettiklerinde oluşur. (StorSimple cihazınızı kaydetmek ve yapılandırmak için StorSimple için Windows PowerShell'i kullanırsınız. Cihaz kaydı hakkında daha fazla bilgi için Bkz. [Adım 3: StorSimple için Windows PowerShell üzerinden cihazınızı yapılandırın ve](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)kaydedin).

Aşağıdaki bölümler, StorSimple aygıtını ilk kez yapılandırdığınızda karşılaştığınız sorunları gidermenize yardımcı olabilir.

## <a name="first-time-setup-wizard-process"></a>İlk kez kurulum sihirbazı işlemi
Aşağıdaki adımlar kurulum sihirbazı işlemini özetler. Ayrıntılı kurulum bilgileri için, [şirket içi StorSimple cihazınızı dağıtın.](storsimple-8000-deployment-walkthrough-u2.md)

1. Kalan adımlarda size rehberlik edecek kurulum sihirbazını başlatmak için [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) cmdlet'i çalıştırın. 
2. Ağı yapılandırın: Kurulum sihirbazı, StorSimple cihazınızdaki DATA 0 ağ arabirimi için ağ ayarlarını yapılandırmanızı sağlar. Bu ayarlar aşağıdakileri içerir:
   * Sanal IP (VIP), alt ağ maskesi ve ağ geçidi – [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet arka planda yürütülür. StorSimple cihazınızdaki DATA 0 ağ arabirimi için IP adresini, alt ağ maskesini ve ağ geçidini yapılandırır.
   * Birincil DNS sunucusu – [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet arka planda yürütülür. StorSimple çözümünüz için DNS ayarlarını yapılandırır.
   * NTP sunucusu – [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet arka planda yürütülür. StorSimple çözümünüz için NTP sunucu ayarlarını yapılandırır.
   * İsteğe bağlı web proxy - [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet arka planda yürütülür. StorSimple çözümünüz için web proxy yapılandırmasını ayarlar ve sağlar.
3. Parolayı ayarlayın: bir sonraki adım aygıt yöneticisi parolasını ayarlamaktır.
   Aygıt yöneticisi parolası cihazınızda oturum açmak için kullanılır. Varsayılan cihaz parolası **Password1**’dir.
        
     > [!IMPORTANT]
     > Parolalar kayıttan önce toplanır, ancak yalnızca aygıtı başarıyla kaydettikten sonra uygulanır. Parola uygulanıp uygulanmadığı varsa, gerekli parolalar (karmaşıklık gereksinimlerini karşılayan) toplanana kadar parolayı yeniden sağlamanız istenir.
     
4. Aygıtı kaydettirin: Son adım, aygıtı Microsoft Azure'da çalışan StorSimple Device Manager hizmetine kaydettirmektir. Kayıt, Azure [portalından hizmet kayıt anahtarını almanızı](storsimple-8000-manage-service.md#get-the-service-registration-key) ve kurulum sihirbazında sağlamanızı gerektirir. **Aygıt başarıyla kaydedildikten sonra, size bir hizmet veri şifreleme anahtarı sağlanır. Sonraki tüm aygıtları hizmete kaydettirmek gerekeceğinden, bu şifreleme anahtarını güvenli bir yerde sakladığından emin olun.**

## <a name="common-errors-during-device-deployment"></a>Cihaz dağıtımı sırasında sık karşılaşılan hatalar
Aşağıdaki tablolarda karşılaşabileceğiniz sık karşılaşılan hataları listeleyin:

* Gerekli ağ ayarlarını yapılandırın.
* İsteğe bağlı web proxy ayarlarını yapılandırın.
* Aygıt yöneticisi parolasını ayarlayın.
* Cihazı kaydedin.

## <a name="errors-during-the-required-network-settings"></a>Gerekli ağ ayarları sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Bu komut yalnızca etkin denetleyicide çalıştırılabilir. |Yapılandırma pasif denetleyici üzerinde gerçekleştiriliyordu. |Bu komutu etkin denetleyiciden çalıştırın. Daha fazla bilgi için [bkz.](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device) |
| 2 |Invoke-HcsSetupWizard: Aygıt hazır değil. |DATA 0'da ağ bağlantısıyla ilgili sorunlar vardır. |DATA 0'daki fiziksel ağ bağlantısını kontrol edin. |
| 3 |Invoke-HcsSetupWizard: Ağdaki başka bir sistemle bir IP adresi çakışması vardır (HRESULT'dan istisna: 0x80070263). |DATA 0 için sağlanan IP zaten başka bir sistem tarafından kullanılıyordu. |Kullanılmayan yeni bir IP sağlayın. |
| 4 |Invoke-HcsSetupWizard: Bir küme kaynağı başarısız oldu. (HRESULT'dan istisna: 0x800713AE). |Yinelenen VIP. Sağlanan IP zaten kullanımda. |Kullanılmayan yeni bir IP sağlayın. |
| 5 |Invoke-HcsSetupWizard: Geçersiz IPv4 adresi. |IP adresi yanlış biçimde sağlanır. |Biçimi kontrol edin ve IP adresinizi yeniden girin. Daha fazla bilgi için [Ipv4 Adresleme'ye][1]bakın. |
| 6 |Invoke-HcsSetupWizard: Geçersiz IPv6 adresi. |IP adresi yanlış biçimde sağlanır. |Biçimi kontrol edin ve IP adresinizi yeniden girin. Daha fazla bilgi için [Bkz. Ipv6 Adresleme][2]. |
| 7 |Invoke-HcsSetupWizard: Uç nokta eşlecisinden kullanılabilir uç nokta yok. (HRESULT'dan İstisna: 0x800706D9) |Küme işlevselliği çalışmıyor. |Sonraki adımlar için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |

## <a name="errors-during-the-optional-web-proxy-settings"></a>İsteğe bağlı web proxy ayarları sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Geçersiz parametre (HRESULT'dan istisna: 0x80070057) |Proxy ayarları için sağlanan parametrelerden biri geçerli değildir. |URI doğru biçimde sağlanmaz. Aşağıdaki biçimi kullanın:*\<http:// IP adresi veya FQDN web proxy sunucusu>: **\<TCP bağlantı noktası numarası>* |
| 2 |Invoke-HcsSetupWizard: RPC sunucusu kullanılamıyor (HRESULT'dan istisna: 0x800706ba) |Temel neden aşağıdakilerden biridir:<ol><li>Küme açık değil.</li><li>Pasif denetleyici etkin denetleyici ile iletişim kuramaz ve komut pasif denetleyiciden çalıştırılır.</li></ol> |Kök nedenine bağlı olarak:<ol><li>Kümenin açık olduğundan emin olmak için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)</li><li>Komutu etkin denetleyiciden çalıştırın. Komutu pasif denetleyiciden çalıştırmak istiyorsanız, pasif denetleyicinin etkin denetleyiciyle iletişim kurabileceğinden emin olmanız gerekir. Bu bağlantı bozuksa [Microsoft Destek'e başvurmanız](storsimple-8000-contact-microsoft-support.md) gerekir.</li></ol> |
| 3 |Invoke-HcsSetupWizard: RPC arama başarısız oldu (HRESULT'dan istisna: 0x800706be) |Küme düştü. |Kümenin açık olduğundan emin olmak için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 4 |Invoke-HcsSetupWizard: Küme kaynağı bulunamadı (HRESULT'dan istisna: 0x8007138f) |Küme kaynağı bulunamadı. Yükleme doğru olmadığında bu gerçekleşebilir. |Aygıtı fabrika varsayılan ayarlarına sıfırlamanız gerekebilir. Küme kaynağı oluşturmak için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 5 |Invoke-HcsSetupWizard: Çevrimiçi olmayan küme kaynağı (HRESULT'dan istisna: 0x8007138c) |Küme kaynakları çevrimiçi değildir. |Sonraki adımlar için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |

## <a name="errors-related-to-device-administrator-password"></a>Aygıt yöneticisi parolası ile ilgili hatalar
Varsayılan aygıt yöneticisi parolası **Password1'dir.** Bu parola ilk oturum açtıktan sonra sona erer; bu nedenle, değiştirmek için kurulum sihirbazı kullanmanız gerekir. Aygıtı ilk kez kaydettirdiğinizde yeni bir aygıt yöneticisi parolası sağlamanız gerekir. 

Parolalarınızın aşağıdaki gereksinimleri karşıladığından emin olun:

* Aygıt yöneticisi parolanız 8 ila 15 karakter uzunluğunda olmalıdır.
* Parolalar aşağıdaki 4 karakter türünden 3'ü içermelidir: küçük harf, büyük harf, sayısal ve özel. 
* Parolanız son 24 parolayla aynı olamaz.

Ayrıca, parolaların her yıl süresinin dolduğunu ve yalnızca aygıtı başarıyla kaydettikten sonra değiştirilebileceğini unutmayın. Kayıt herhangi bir nedenle başarısız olursa, parolalar değiştirilmez.

Aygıt yöneticisi parolası hakkında daha fazla bilgi için [StorSimple parolanızı değiştirmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-change-passwords.md)

Aygıt yöneticisini ve StorSimple Snapshot Manager parolalarını ayarlarken aşağıdaki hatalardan biriyle veya birkaçıyla karşılaşabilirsiniz.

| Hayır. | Hata iletisi | Önerilen eylem |
| --- | --- | --- |
| 1 |Parola maksimum uzunluğu aşıyor. |Aygıt yöneticisi parolanız 8 ila 15 karakter uzunluğunda olmalıdır. |
| 2 |Parola gerekli uzunluğu karşılamıyor. |Aygıt yöneticisi parolanız 8 ila 15 karakter uzunluğunda olmalıdır.|
| 3 |Parola küçük karakterler içermelidir. |Parolalar aşağıdaki 4 karakter türünden 3'ü içermelidir: küçük harf, büyük harf, sayısal ve özel. Parolanızın bu gereksinimleri karşıladığından emin olun. |
| 4 |Parola sayısal karakterler içermelidir. |Parolalar aşağıdaki 4 karakter türünden 3'ü içermelidir: küçük harf, büyük harf, sayısal ve özel. Parolanızın bu gereksinimleri karşıladığından emin olun. |
| 5 |Parola özel karakterler içermelidir. |Parolalar aşağıdaki 4 karakter türünden 3'ü içermelidir: küçük harf, büyük harf, sayısal ve özel. Parolanızın bu gereksinimleri karşıladığından emin olun. |
| 6 |Parola aşağıdaki 4 karakter türünden 3'ü içermelidir: büyük harf, küçük harf, sayısal ve özel. |Parolanız gerekli karakter türlerini içermez. Parolanızın bu gereksinimleri karşıladığından emin olun. |
| 7 |Parametre onayla eşleşmiyor. |Parolanızın tüm gereksinimleri karşıladığından ve parolanızı doğru girdiğinizden emin olun. |
| 8 |Parolanız varsayılanla eşleşemez. |Varsayılan parola *Password1'dir.* Oturum açtıktan sonra bu parolayı değiştirmeniz gerekir. |
| 9 |Girdiğiniz parola aygıt parolasıyla eşleşmiyor. Lütfen parolayı yeniden yazın. |Parolayı kontrol edin ve yeniden yazın. |

Parolalar aygıt kaydedilmeden önce toplanır, ancak yalnızca başarılı bir kayıttan sonra uygulanır. Parola kurtarma iş akışı aygıtın kaydolmasını gerektirir.

> [!IMPORTANT]
> Genel olarak, parola uygulama girişimi başarısız olursa, yazılım başarılı olana kadar parolayı tekrar tekrar toplamaya çalışır. Nadir durumlarda parola uygulanamaz. Bu durumda, aygıtı kaydedebilir ve devam edebilirsiniz, ancak parolalar değiştirilmez. Azure portalından kayıt olduktan sonra aygıt yöneticisi parolasını değiştirebilirsiniz.


Parolayı StorSimple Device Manager hizmeti aracılığıyla Azure portalında sıfırlayabilirsiniz. Daha fazla bilgi için [aygıt yöneticisi parolasını değiştir'e](storsimple-8000-change-passwords.md#change-the-device-administrator-password)gidin.

## <a name="errors-during-device-registration"></a>Cihaz kaydı sırasında karşılaşılan hatalar
Aygıtı kaydetmek için Microsoft Azure'da çalışan StorSimple Device Manager hizmetini kullanırsınız. Cihaz kaydı sırasında aşağıdaki sorunlardan biriyle veya daha fazlası ile karşılaşabilirsiniz.

| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Hata 350027: Aygıtı StorSimple Aygıt Yöneticisine kaydedemedi. | |Birkaç dakika bekleyin ve sonra işlemi yeniden deneyin. Sorun devam ederse, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 2 |Hata 350013: Aygıtı kaydederken bir hata oluştu. Bunun nedeni yanlış hizmet kayıt anahtarı olabilir. | |Lütfen cihazı doğru servis kayıt anahtarıyla yeniden kaydedin. Daha fazla bilgi için [bkz.](storsimple-8000-manage-service.md#get-the-service-registration-key) |
| 3 |Hata 350063: StorSimple Device Manager hizmetine kimlik doğrulama geçti ancak kayıt başarısız oldu. Lütfen bir süre sonra operasyonu yeniden deneyin. |Bu hata, ACS kimlik doğrulamasının geçtiğini, ancak hizmete yapılan kayıt çağrısının başarısız olduğunu gösterir. Bu, düzensiz bir ağ hatasının bir sonucu olabilir. |Sorun devam ederse, lütfen [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 4 |Hata 350049: Hizmet kayıt sırasında ulaşılamadı. |Hizmete çağrı yapıldığında, bir web özel durum alınır. Bazı durumlarda, bu işlem daha sonra yeniden çalışarak düzeltilebilir. |Lütfen IP adresinizi ve DNS adınızı kontrol edin ve işlemi yeniden deneyin. Sorun devam ederse, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 5 |Hata 350031: Aygıt zaten kaydedildi. | |Herhangi bir işlem gerekli değil. |
| 6 |Hata 350016: Aygıt Kaydı başarısız oldu. | |Lütfen kayıt anahtarının doğru olduğundan emin olun. |
| 7 |Invoke-HcsSetupWizard: Cihazınızı kaydederken bir hata oluştu; bu yanlış IP adresi veya DNS adı nedeniyle olabilir. Lütfen ağ ayarlarınızı kontrol edin ve yeniden deneyin. Sorun devam ederse, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) (Hata 350050) |Cihazınızın dış ağping emin olun. Dış ağa bağlantınız yoksa, kayıt bu hatayla başarısız olabilir. Bu hata, aşağıdakilerden birinin veya daha fazlasının bir leşimi olabilir:<ul><li>Yanlış IP</li><li>Yanlış alt ağ</li><li>Yanlış ağ geçidi</li><li>Yanlış DNS ayarları</li></ul> |[Adım adım sorun giderme örneğindeki](#step-by-step-storsimple-troubleshooting-example)adımlara bakın. |
| 8 |Invoke-HcsSetupWizard: Geçerli işlem bir iç hizmet hatası nedeniyle başarısız oldu [0x1FBE2]. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Destek'e başvurun. |Bu, hizmet veya aracıdan gelen tüm kullanıcı görünmez hataları için atılan genel bir hatadır. En yaygın nedeni ACS kimlik doğrulaması başarısız olabilir. Hatanın olası bir nedeni, NTP sunucu yapılandırmasıyla ilgili sorunlar olması ve aygıttaki zamanın doğru ayarlanmamasıdır. |Saati düzeltin (sorun varsa) ve ardından kayıt işlemini yeniden deneyin. Saat dilimini ayarlamak için Set-HcsSystem -Timezone komutunu kullanıyorsanız, saat dilimindeki her sözcüğü büyük harfe leştirin (örneğin "Pasifik Standart Saati").  Bu sorun devam ederse, sonraki adımlar için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 9 |Uyarı: Aygıtı etkinleştiremedi. Aygıt yöneticiniz ve StorSimple Snapshot Manager parolalarınız değiştirilmedi. |Kayıt başarısız olursa, aygıt yöneticisi ve StorSimple Snapshot Manager parolaları değiştirilmez. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>StorSimple dağıtımlarının sorunlarını gidermeye yönelik araçlar
StorSimple, StorSimple çözümünüzü gidermek için kullanabileceğiniz çeşitli araçlar içerir. Bunlar:

* Destek paketleri ve aygıt günlükleri.
* Sorun giderme için özel olarak tasarlanmış cmdletler.

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Sorun giderme için destek paketleri ve aygıt günlükleri kullanılabilir
Destek paketi, Microsoft Destek ekibine sorun giderme aygıtı sorunlarında yardımcı olabilecek tüm ilgili günlükleri içerir. StorSimple için Windows PowerShell'i kullanarak destek personeliyle paylaşabileceğiniz şifreli bir destek paketi oluşturabilirsiniz.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Destek paketinin günlüklerini veya içeriğini görüntülemek için
1. Create'de açıklandığı gibi bir destek paketi oluşturmak [ve bir destek paketini yönetmek](storsimple-8000-create-manage-support-package.md)için StorSimple için Windows PowerShell'i kullanın.
2. Şifre [çözme komut dosyasını](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) istemci bilgisayarınızdan yerel olarak indirin.
3. Destek paketini açmak ve şifresini çözmek için bu [adım adım yordamı](storsimple-8000-create-manage-support-package.md#edit-a-support-package) kullanın.
4. Şifresi çözülmüş destek paketi günlükleri etw/etvx formatındadır. Bu dosyaları Windows Olay Görüntüleyicisi'nde görüntülemek için aşağıdaki adımları gerçekleştirebilirsiniz:
   
   1. Windows istemcinizde **eventvwr** komutunu çalıştırın. Bu, Olay Görüntüleyicisi'ni başlatır.
   2. **Eylemler** bölmesinde, **Kaydedilmiş Günlük'ü Aç'ı** tıklatın ve etvx/etw formatında (destek paketi) günlük dosyalarını işaret edin. Artık dosyayı görüntüleyebilirsiniz. Dosyayı açtıktan sonra, dosyayı sağ tıklayıp metin olarak kaydedebilirsiniz.
      
      > [!IMPORTANT]
      > Bu dosyaları Windows PowerShell'de açmak için **Get-WinEvent** cmdlet'ini de kullanabilirsiniz. Daha fazla bilgi için Windows PowerShell cmdlet başvuru belgelerinde [Get-WinEvent'e](https://technet.microsoft.com/library/hh849682.aspx) bakın.
     
5. Olay Görüntüleyicisi'nde günlükler açıldığında, aygıt yapılandırması ile ilgili sorunları içeren aşağıdaki günlükleri arayın:
   
   * hcs_pfconfig/Operasyonel Günlük
   * hcs_pfconfig/Config
6. Günlük dosyalarında, kurulum sihirbazı tarafından çağrılan cmdlets ile ilgili dizeleri arayın. Bu cmdlets listesi için [İlk kez kurulum sihirbazı işlemine](#first-time-setup-wizard-process) bakın.
7. Sorunun nedenini çözemezseniz, sonraki adımlar için [Microsoft Destek'e başvurabilirsiniz.](storsimple-8000-contact-microsoft-support.md) Yardım için Microsoft Destek'e başvursanız [destek isteği oluştur'daki](storsimple-8000-contact-microsoft-support.md#create-a-support-request) adımları kullanın.

## <a name="cmdlets-available-for-troubleshooting"></a>Sorun giderme için cmdletler kullanılabilir
Bağlantı hatalarını algılamak için aşağıdaki Windows PowerShell cmdlets'i kullanın.

* `Get-NetAdapter`: Ağ arabirimlerinin durumunu algılamak için bu cmdlet'i kullanın.
* `Test-Connection`: Bu cmdlet'i kullanarak ağ içindeki ve dışındaki ağ bağlantısını kontrol edin.
* `Test-HcsmConnection`: Başarılı bir şekilde kaydedilmiş bir cihazın bağlantısını kontrol etmek için bu cmdlet'i kullanın.
* `Sync-HcsTime`: Cihaz saatini görüntülemek ve NTP sunucusuyla zaman eşitlenmesini zorlamak için bu cmdlet'i kullanın.
* `Enable-HcsPing`ve `Disable-HcsPing`: Ana bilgisayarların StorSimple cihazınızdaki ağ arabirimlerini pinglemesine izin vermek için bu cmdletleri kullanın. Varsayılan olarak, StorSimple ağ arabirimleri ping isteklerine yanıt vermez.
* `Trace-HcsRoute`: Bu cmdlet'i rota izleme aracı olarak kullanın. Belirli bir süre içinde son hedefe giden yolda her yönlendiriciye paketler gönderir ve ardından her atlamadan döndürülen paketlere göre sonuçları hesaplar. Belirli `Trace-HcsRoute` bir yönlendirici veya bağlantıdaki paket kaybının derecesini gösterdiğinden, hangi yönlendiricilerin veya bağlantıların ağ sorunlarına neden olabileceğini belirleyebilirsiniz.
* `Get-HcsRoutingTable`: Yerel IP yönlendirme tablosunu görüntülemek için bu cmdlet'i kullanın.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Get-NetAdapter cmdlet ile sorun giderme
Ağ arabirimlerini ilk kez aygıt dağıtımı için yapılandırdığınızda, aygıt hizmete henüz kaydedilmemiş olduğundan donanım durumu StorSimple Device Manager hizmeti kullanıcı arabiriminde kullanılamaz. Ayrıca, **Donanım sağlık** bıçağı, özellikle hizmet eşitlemesini etkileyen sorunlar varsa, aygıtın durumunu her zaman doğru şekilde yansıtmayabilir. Bu gibi durumlarda, ağ `Get-NetAdapter` arabirimlerinizin durumunu ve durumunu belirlemek için cmdlet'i kullanabilirsiniz.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Cihazınızdaki tüm ağ bağdaştırıcılarının listesini görmek için
1. StorSimple için Windows PowerShell'i `Get-NetAdapter`başlatın ve ardından yazın. 
2. Ağ arabiriminizin `Get-NetAdapter` durumunu anlamak için cmdlet çıktısını ve aşağıdaki yönergeleri kullanın.
   
   * Arabirim sağlıklı ve etkinse, **ifIndex** durumu **Yukarı**olarak gösterilir.
   * Arabirim sağlıklıysa ancak fiziksel olarak bağlı değilse (ağ kablosuyla), **ifIndex** **Devre Dışı bırakılmış**olarak gösterilir.
   * Arabirim sağlıklı ysa ancak etkin değilse, **ifIndex** durumu **NotPresent**olarak gösterilir.
   * Arabirim yoksa, bu listede görünmez. StorSimple Device Manager hizmeti Kullanıcı Arabirimi bu arabirimi yine de başarısız bir durumda gösterir.

Bu cmdletin nasıl kullanılacağı hakkında daha fazla bilgi için Windows PowerShell cmdlet referansında [Get-NetAdapter'a](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter?view=win10-ps) gidin.

Aşağıdaki bölümlerde `Get-NetAdapter` cmdlet çıktı örnekleri göster.

 Bu örneklerde denetleyici 0 pasif denetleyiciydi ve aşağıdaki gibi yapılandırıldı:

* VERI 0, DATA 1, DATA 2 ve DATA 3 ağ arabirimleri aygıtta mevcut.
* DATA 4 ve DATA 5 ağ arabirim kartları mevcut değildi; bu nedenle, çıktılarda listelenmez.
* DATA 0 etkindi.

Denetleyici 1 etkin denetleyiciydi ve aşağıdaki gibi yapılandırıldı:

* DATA 0, DATA 1, DATA 2, DATA 3, DATA 4 ve DATA 5 ağ arayüzleri cihazda mevcut.
* DATA 0 etkindi.

**Örnek çıktı – denetleyici 0**

Aşağıdaki denetleyici 0 (pasif denetleyici) çıkışıdır. DATA 1, DATA 2 ve DATA 3 bağlı değildir. DATA 4 ve DATA 5, aygıtta bulunmadığından listelenmez.

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Örnek çıktı – denetleyici 1**

Aşağıdaki denetleyici 1 (etkin denetleyici) çıktıdır. Aygıttaki yalnızca DATA 0 ağ arabirimi yapılandırılır ve çalışır.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Test-Bağlantı cmdlet ile sorun giderme
StorSimple cihazınızın `Test-Connection` dış ağa bağlanıp bağlanamayacağını belirlemek için cmdlet'i kullanabilirsiniz. DNS de dahil olmak üzere tüm ağ parametreleri kurulum sihirbazında doğru şekilde `Test-Connection` yapılandırılırsa, cmdlet'i ağ dışında bilinen bir adresi pinglemek için kullanabilirsiniz, örneğin outlook.com.

Ping devre dışı bırakılırsa, bu cmdlet ile bağlantı sorunlarını gidermek için ping'i etkinleştirmelisiniz.

Cmdlet'ten aşağıdaki `Test-Connection` çıktı örneklerine bakın.

> [!NOTE]
> İlk örnekte, aygıt yanlış bir DNS ile yapılandırılır. İkinci örnekte, DNS doğrudur.

**Örnek çıktı – yanlış DNS**

Aşağıdaki örnekte, DNS'nin çözülmediğini gösteren IPV4 ve IPV6 adresleri için çıktı yoktur. Bu, dış ağa bağlantı olmadığı ve doğru bir DNS sağlanması gerektiği anlamına gelir.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Örnek çıktı – doğru DNS**

Aşağıdaki örnekte, DNS, DNS'nin doğru şekilde yapılandırıldığına işaret eden IPV4 adresini döndürür. Bu, dış ağa bağlantı olduğunu doğrular.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection cmdlet ile sorun giderme
StorSimple `Test-HcsmConnection` Device Manager hizmetinize zaten bağlı ve kayıtlı bir aygıt için cmdlet'i kullanın. Bu cmdlet, kayıtlı bir aygıt la ilgili StorSimple Device Manager hizmeti arasındaki bağlantıyı doğrulamanıza yardımcı olur. Bu komutu StorSimple için Windows PowerShell'de çalıştırabilirsiniz.

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection cmdlet çalıştırmak için
1. Aygıtın kayıtlı olduğundan emin olun.
2. Cihazın durumunu kontrol edin. Aygıt bakım modunda veya çevrimdışı devre dışı bırakılırsa, aşağıdaki hatalardan birini görebilirsiniz:
   
   * ErrorCode.CiSDeviceDecommissioned - bu cihazın devre dışı bırakıldığını gösterir.
   * ErrorCode.DeviceNotReady – bu, aygıtın bakım modunda olduğunu gösterir.
   * ErrorCode.DeviceNotReady – bu, aygıtın çevrimiçi olmadığını gösterir.
3. StorSimple Device Manager hizmetinin çalıştığını doğrulayın [(Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet'ini kullanın). Hizmet çalışmıyorsa, aşağıdaki hataları görebilirsiniz:
   
   * Errorcode.cisapplianceagentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – bu, Get-ClusterResource'ı çalıştırdığınızda bir özel durum olduğunu gösterir.
4. Erişim Kontrol Hizmeti (ACS) belirteci kontrol edin. Bir web özel durum oluşturursa, ağ geçidi sorunu, eksik proxy kimlik doğrulaması, yanlış Bir DNS veya kimlik doğrulama hatası sonucu olabilir. Aşağıdaki hataları görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu bir HttpStatusCode.BadGateway özel durumunu gösterir: ad çözümleyici hizmeti ana bilgisayar adını çözemedi.
   * ErrorCode.CiSApplianceProxy – bu bir HttpStatusCode.ProxyAuthenticationGerekli özel durum (HTTP durum kodu 407) gösterir: istemci proxy sunucusu ile kimlik doğrulaması olamazdı.
   * ErrorCode.CiSApplianceDNSError – bu bir WebExceptionStatus.NameResolutionFailure özel durumu gösterir: ad çözümleyici hizmeti ana bilgisayar adını çözemedi.
   * ErrorCode.CiSApplianceACSError – bu, hizmetin bir kimlik doğrulama hatası döndürüldünü gösterir, ancak bağlantı vardır.
     
     Web özel durum atmazsa, ErrorCode.CiSApplianceFailure olup olmadığını kontrol edin. Bu, cihazın arızalı olduğunu gösterir.
5. Bulut hizmeti bağlantısını kontrol edin. Hizmet bir web özel durum atarsa, aşağıdaki hataları görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu bir HttpStatusCode.BadGateway özel durumunu gösterir: bir ara proxy sunucusu başka bir proxy'den veya orijinal sunucudan kötü bir istek aldı.
   * ErrorCode.CiSApplianceProxy – bu bir HttpStatusCode.ProxyAuthenticationGerekli özel durum (HTTP durum kodu 407) gösterir: istemci proxy sunucusu ile kimlik doğrulaması olamazdı.
   * ErrorCode.CiSApplianceDNSError – bu bir WebExceptionStatus.NameResolutionFailure özel durumu gösterir: ad çözümleyici hizmeti ana bilgisayar adını çözemedi.
   * ErrorCode.CiSApplianceACSError – bu, hizmetin bir kimlik doğrulama hatası döndürüldünü gösterir, ancak bağlantı vardır.
     
     Bir web özel durum atmazsa, ErrorCode.CiSApplianceSaasServiceError için kontrol edin. Bu, StorSimple Device Manager hizmetiyle ilgili bir sorunu gösterir.
6. Azure Hizmet Veri Servisi bağlantısını kontrol edin. ErrorCode.CiSApplianceServiceBusError, aygıtın Servis Veri Servisi'ne bağlanamayacağını gösterir.

Günlük dosyaları CiSCommandletLog0Curr.errlog ve CiSAgentsvc0Curr.errlog özel durum ayrıntıları gibi daha fazla bilgi olacak.

Cmdlet'in nasıl kullanılacağı hakkında daha fazla bilgi için Windows PowerShell başvuru belgelerindeki [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) adresine gidin.

> [!IMPORTANT]
> Bu cmdlet'i hem etkin hem de pasif denetleyici için çalıştırabilirsiniz.

Cmdlet'ten aşağıdaki `Test-HcsmConnection` çıktı örneklerine bakın.

**Örnek çıktı – StorSimple Update 3'u çalıştıran başarılı bir şekilde kayıtlı aygıt**

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device to service  ... This will take a few minutes.

      Checking connectivity from device to service  ... Success

      Checking connectivity from service to device  ... Success

      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Örnek çıktı – çevrimdışı aygıt** 

Bu örnek, Azure portalında **Çevrimdışı** durumu olan bir aygıttan elde edilir.

     Checking device registrationstate: Success
     Device is registered successfully
     Checking connectivity from device to SaaS.. Failure

Aygıt geçerli web proxy yapılandırmasını kullanarak bağlanamadı. Bu web proxy yapılandırması veya ağ bağlantısı sorunu ile ilgili bir sorun olabilir. Bu durumda, web proxy ayarlarınızın doğru olduğundan ve web proxy sunucularınızın çevrimiçi ve erişilebilir olduğundan emin olmalısınız.

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Sync-HcsTime cmdlet ile sorun giderme
Cihazın zamanını görüntülemek için bu cmdlet'i kullanın. Aygıt süresi NTP sunucusuyla bir ofset varsa, bu cmdlet'i kullanarak ntp sunucunuzla zamanı zorla senkronize edebilirsiniz.
- Aygıt ve NTP sunucusu arasındaki ofset 5 dakikadan büyükse, bir uyarı görürsünüz. 
- Ofset 15 dakikayı aşarsa, aygıt çevrimdışı olur. Yine de bir zaman senkronizasyonu zorlamak için bu cmdlet kullanabilirsiniz. 
- Ancak, ofset 15 saati aşarsa, saati zorla senkronize edemeyeceksiniz ve bir hata iletisi gösterilir.

**Örnek çıktı – Sync-HcsTime kullanarak zorunlu zaman senkronizasyonu**

     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Enable-HcsPing ve Devre Dışı-HcsPing cmdletile sorun giderme
Cihazınızdaki ağ arabirimlerinin ICMP ping isteklerine yanıt verdiğinden emin olmak için bu cmdlets'leri kullanın. Varsayılan olarak, StorSimple ağ arabirimleri ping isteklerine yanıt vermez. Bu cmdlet kullanarak cihazınızın çevrimiçi ve ulaşılabilir olup olmadığını bilmek için en kolay yoldur.

**Örnek çıktı – Etkinleştir-HcsPing ve Devre Dışı-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Trace-HcsRoute cmdlet ile sorun giderme
Bu cmdlet'i rota izleme aracı olarak kullanın. Belirli bir süre içinde son hedefe giden yolda her yönlendiriciye paketler gönderir ve ardından her atlamadan döndürülen paketlere göre sonuçları hesaplar. Cmdlet, belirli bir yönlendirici veya bağlantıdaki paket kaybının derecesini gösterdiğinden, hangi yönlendiricilerin veya bağlantıların ağ sorunlarına neden olabileceğini belirleyebilirsiniz.

**Trace-HcsRoute ile bir paketin rotasını nasıl izleyişen örnek çıktı**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Get-HcsRoutingTable cmdlet ile sorun giderme
StorSimple cihazınızın yönlendirme tablosunu görüntülemek için bu cmdlet'i kullanın. Yönlendirme tablosu, Internet Protokolü (IP) ağı üzerinden seyahat eden veri paketlerinin nereye yönlendirileceğini belirlemeye yardımcı olabilecek kurallar kümesidir.

Yönlendirme tablosu, verileri belirtilen ağlara aktaran arabirimleri ve ağ geçidini gösterir. Ayrıca belirli bir hedefe ulaşmak için alınan yol için karar verici olan yönlendirme metrik verir. Yönlendirme ölçütü ne kadar düşükse, tercih de o kadar yüksek.

Örneğin, Internet'e bağlı 2 ağ arabirimi, DATA 2 ve DATA 3 varsa. DATA 2 ve DATA 3 için yönlendirme ölçümleri sırasıyla 15 ve 261 ise, daha düşük yönlendirme ölçümüne sahip DATA 2, Internet'e ulaşmak için kullanılan tercih edilen arabirimdir.

StorSimple cihazınızda Update 1 çalıştırıyorsanız, DATA 0 ağ arabirimibulut trafiği için en yüksek tercihe sahiptir. Bu, bulut özellikli başka arabirimler olsa bile bulut trafiğinin DATA 0 üzerinden yönlendirileceğini gösterir.

Cmdlet'i `Get-HcsRoutingTable` herhangi bir parametre belirtmeden çalıştırıyorsanız (aşağıdaki örnekte görüldüğü gibi), cmdlet hem IPv4 hem de IPv6 yönlendirme tablolarını çıkaracaktır. Alternatif olarak, ilgili `Get-HcsRoutingTable -IPv4` `Get-HcsRoutingTable -IPv6` bir yönlendirme tablosu belirtebilir veya alabilirsiniz.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>Adım adım StorBasit sorun giderme örneği
Aşağıdaki örnek, StorSimple dağıtımının adım adım sorun giderme sini gösterir. Örnek senaryoda, aygıt kaydı ağ ayarlarının veya DNS adının yanlış olduğunu belirten bir hata iletisiyle başarısız olur.

Döndürülen hata iletisi:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Hata aşağıdakilerden herhangi biri neden olabilir:

* Yanlış donanım yükleme
* Hatalı ağ arabirimi(ler)
* Yanlış IP adresi, alt ağ maskesi, ağ geçidi, birincil DNS sunucusu veya web proxy
* Yanlış kayıt anahtarı
* Yanlış güvenlik duvarı ayarları

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Cihaz kayıt sorununu bulmak ve düzeltmek için
1. Cihazınızın yapılandırmasını kontrol edin: `Invoke-HcsSetupWizard`etkin denetleyicide çalıştırın.
   
   > [!NOTE]
   > Kurulum sihirbazı etkin denetleyici üzerinde çalışması gerekir. Etkin denetleyiciye bağlı olduğunuzu doğrulamak için seri konsolunda sunulan banner'a bakın. Banner, denetleyici 0'a mı yoksa denetleyici 1'e mi bağlı olduğunuzu ve denetleyicinin etkin veya pasif olup olmadığını gösterir. Daha fazla bilgi için [cihazınızdaki etkin bir denetleyiciyi](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device)tanımla'ya gidin.
   
2. Aygıtın doğru kabloyla kablolandığından emin olun: Cihazın arka düzlemindeki ağ kablolarını kontrol edin. Kablolama aygıt modeline özgüdür. Daha fazla bilgi için [StorSimple 8100 cihazınızı yükleyin](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızı yükleyin.](storsimple-8600-hardware-installation.md)
   
   > [!NOTE]
   > 10 GbE ağ bağlantı noktası kullanıyorsanız, sağlanan QSFP-SFP bağdaştırıcılarını ve SFP kablolarını kullanmanız gerekir. Daha fazla bilgi için, [10 GbE bağlantı noktaları için önerilen kablolar, anahtarlar ve alıcı vericilerin listesine](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)bakın.
  
3. Ağ arabiriminin sistem durumunu doğrulayın:
   
   * DATA 0 için ağ arabirimlerinin durumunu algılamak için Get-NetAdapter cmdlet'i kullanın. 
   * Bağlantı çalışmıyorsa, **ifindex** durumu arabirimin aşağıda olduğunu gösterir. Daha sonra bağlantı noktasının cihaza ve anahtara ağ bağlantısını kontrol etmeniz gerekir. Ayrıca kötü kabloları ekarte etmek gerekir. 
   * Etkin denetleyicideki DATA 0 bağlantı noktasının arızalı olduğundan şüpheleniyorsanız, denetleyici 1'deki DATA 0 bağlantı noktasına bağlanarak bunu doğrulayabilirsiniz. Bunu doğrulamak için, cihazın arka sindeki ağ kablosunu denetleyici 0'dan koparın, kabloyu denetleyici 1'e bağlayın ve get-NetAdapter cmdlet'i yeniden çalıştırın.
     Denetleyicideki DATA 0 bağlantı noktası başarısız olursa, sonraki adımlar için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md) Sisteminizdeki denetleyiciyi değiştirmeniz gerekebilir.
4. Anahtara bağlantı doğrulayın:
   
   * Birincil kasanızdaki denetleyici 0 ve denetleyici 1'deki DATA 0 ağ arabirimlerinin aynı alt ağda olduğundan emin olun. 
   * Hub'ı veya yönlendiriciyi kontrol edin. Genellikle, her iki denetleyiciyi de aynı hub'a veya yönlendiriciye bağlamanız gerekir. 
   * Bağlantı için kullandığınız anahtarların aynı vLAN'daki her iki denetleyici için DE 0 VERI'ye sahip olduğundan emin olun.
5. Kullanıcı hatalarını ortadan kaldırın:
   
   * Kurulum sihirbazını yeniden çalıştırın **(Invoke-HcsSetupWizard'ı**çalıştırın ) ve hata olmadığından emin olmak için değerleri yeniden girin. 
   * Kullanılan kayıt anahtarını doğrulayın. Aynı kayıt anahtarı, birden çok aygıtı StorSimple Device Manager hizmetine bağlamak için kullanılabilir. Doğru kayıt [anahtarını](storsimple-8000-manage-service.md#get-the-service-registration-key) kullandığınızdan emin olmak için servis kayıt anahtarını al'da yordamı kullanın.
     
     > [!IMPORTANT]
     > Çalışan birden çok hizmetiniz varsa, aygıtı kaydetmek için uygun hizmetin kayıt anahtarının kullanıldığından emin olmanız gerekir. Yanlış StorSimple Aygıt Yöneticisi hizmetine sahip bir aygıt kaydettiyseniz, sonraki adımlar için [Microsoft Desteği'ne başvurmanız](storsimple-8000-contact-microsoft-support.md) gerekir. Aygıtın fabrikada sıfırlanması (veri kaybına neden olabilir) daha sonra istenen hizmete bağlamak için gerçekleştirmeniz gerekebilir.
     > 
     > 
6. Dış ağa bağlantınız olduğunu doğrulamak için Test-Bağlantı cmdlet'ini kullanın. Daha fazla bilgi için [Test-Bağlantı cmdlet ile Sorun](#troubleshoot-with-the-test-connection-cmdlet)Giderme'ye gidin.
7. Güvenlik duvarı paraziti parazitini kontrol edin. Sanal IP (VIP), alt ağ, ağ geçidi ve DNS ayarlarının hepsinin doğru olduğunu doğruladıysanız ve hala bağlantı sorunları görüyorsanız, güvenlik duvarınızın aygıtınızla dış ağ arasındaki iletişimi engellemesi mümkündür. Giden iletişim için StorSimple cihazınızda 80 ve 443 bağlantı noktalarının bulunduğundan emin olmanız gerekir. Daha fazla bilgi için [StorSimple cihazınızIçin Ağ gereksinimlerine](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device)bakın.
8. Kayıtlara bak. Sorun [giderme için kullanılabilen Destek paketlerine ve aygıt günlüklerine](#support-packages-and-device-logs-available-for-troubleshooting)gidin.
9. Yukarıdaki adımlar sorunu çözmezse, yardım için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple aygıtının sorun gidermesinde Tanılama aracını nasıl kullanacağınızı öğrenin.](storsimple-8000-diagnostics.md)

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
