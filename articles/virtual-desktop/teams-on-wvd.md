---
title: Windows sanal masaüstündeki Microsoft ekipleri-Azure
description: Microsoft ekiplerini Windows sanal masaüstü 'nde kullanma.
author: Heidilohr
ms.topic: how-to
ms.date: 07/28/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2032a7c9d9cd9b17da956dc829234462f8b9e726
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88509612"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Microsoft ekiplerini Windows sanal masaüstü 'nde kullanma

>[!IMPORTANT]
>Ekipler için medya iyileştirmesi Microsoft 365 Kamu ortamlarında desteklenmez.

>[!NOTE]
>Microsoft ekipleri için medya iyileştirmesi yalnızca Windows 10 makinelerinde Windows Masaüstü istemcisi için kullanılabilir. Medya iyileştirmeleri Windows Masaüstü istemcisi sürümü 1.2.1026.0 veya üstünü gerektirir.

Windows sanal masaüstündeki Microsoft ekipleri sohbet ve işbirliğini destekler. Medya iyileştirmeleriyle, çağrı ve toplantı işlevlerini de destekler. Sanal Masaüstü Altyapısı (VDı) ortamlarında Microsoft ekipleri kullanma hakkında daha fazla bilgi edinmek için bkz. [sanallaştırılmış Masaüstü Altyapısı Için takımlar](/microsoftteams/teams-for-vdi/).

Microsoft ekipleri için medya iyileştirmesi sayesinde, Windows Masaüstü istemcisi, takımlar ve toplantılar için ses ve videoyu yerel olarak işler. Windows sanal masaüstündeki Microsoft ekiplerini, iyileştirilmiş çağrı ve toplantılar olmadan diğer istemcilerle kullanmaya devam edebilirsiniz. Takımlar sohbeti ve işbirliği özellikleri tüm platformlarda desteklenir. Uzak oturumunuzdaki yerel cihazları yeniden yönlendirmek için [bir konak havuzunun Uzak Masaüstü Protokolü özelliklerini Özelleştir](#customize-remote-desktop-protocol-properties-for-a-host-pool)' e bakın.

## <a name="prerequisites"></a>Ön koşullar

Windows sanal masaüstü 'nde Microsoft ekipleri kullanabilmeniz için şu işlemleri yapmanız gerekir:

- [Ağınızı](/microsoftteams/prepare-network/) Microsoft ekipleri için hazırlayın.
- Windows [masaüstü istemcisini](connect-windows-7-10.md) , Windows [Bilgisayarı üzerindeki takımlar için Microsoft ekipleri donanım gereksinimlerini](/microsoftteams/hardware-requirements-for-the-teams-app#hardware-requirements-for-teams-on-a-windows-pc/)karşılayan bir Windows 10 veya Windows 10 IoT kurumsal cihazına yükler.
- Windows 10 çoklu oturum veya Windows 10 Enterprise sanal makinesine (VM) bağlanın.
- Makine başına yükleme kullanarak takımlar masaüstü uygulamasını konağa yükleme. Microsoft ekipleri için medya iyileştirmesi, takımlar masaüstü uygulaması sürüm 1.3.00.4461 veya üzerini gerektirir.

## <a name="install-the-teams-desktop-app"></a>Takımlar masaüstü uygulamasını yükler

Bu bölümde, Windows 10 çoklu oturum veya Windows 10 Enterprise VM yansımanıza takımlar masaüstü uygulamasını nasıl yükleyeceğiniz gösterilmektedir. Daha fazla bilgi edinmek için bkz. [VDI üzerinde takımlar masaüstü uygulamasını yüklemeyi veya güncelleştirmeyi](/microsoftteams/teams-for-vdi#install-or-update-the-teams-desktop-app-on-vdi)deneyin.

### <a name="prepare-your-image-for-teams"></a>Görüntünüzü takımlar için hazırlama

Takımlar için medya iyileştirmesini etkinleştirmek üzere konakta aşağıdaki kayıt defteri anahtarını ayarlayın:

1. Başlat menüsünde, **Regedit** komutunu yönetici olarak çalıştırın. **HKEY_LOCAL_MACHINE \Software\microsoft\ekiplerine**gidin.
2. Takımlar anahtarı için aşağıdaki değeri oluşturun:

| Ad             | Tür   | Veri/değer  |
|------------------|--------|-------------|
| IsWVDEnvironment | DWORD  | 1           |

### <a name="install-the-teams-websocket-service"></a>Takımlar WebSocket hizmetini yükler

VM yansımanıza en son [WebSocket hizmetini](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4AQBt) yükler. Yükleme hatasıyla karşılaşırsanız, yeniden [dağıtılabilir en son Microsoft Visual C++](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) yükleyip yeniden deneyin.

#### <a name="latest-websocket-service-versions"></a>En son WebSocket hizmeti sürümleri

Aşağıdaki tabloda WebSocket hizmetinin en son sürümleri listelenmektedir:

|Sürüm        |Sürüm tarihi  |
|---------------|--------------|
|1.0.2006.11001 |07/28/2020    |
|0.11.0         |05/29/2020    |

#### <a name="updates-for-version-10200611001"></a>Sürüm 1.0.2006.11001 için güncelleştirmeler

- Bir çağrı veya toplantı sırasında ekipler uygulamasının bir şekilde çıkarılmasına neden olan bir sorun düzeltildi.
- Birden çok Monitor Masaüstü oturumlarında paylaşılacak bir izleyici seçme desteği eklendi.

### <a name="install-microsoft-teams"></a>Microsoft ekipleri 'nı yükler

Ekipler masaüstü uygulamasını makine başına veya Kullanıcı başına yükleme kullanarak dağıtabilirsiniz. Microsoft ekiplerinizi Windows sanal masaüstü ortamınıza yüklemek için:

1. Ortamınızla eşleşen [TAKıMLAR MSI paketini](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) indirin. 64 bitlik bir yükleyicinin bir 64 bit işletim sisteminde kullanılması önerilir.

      > [!IMPORTANT]
      > Ekipler masaüstü istemci sürümünün en son güncelleştirmesi, ekiplerin sohbet, kanallar ve takvim 'de UTC saat dilimi olduğunu göstermiştir bir sorunu düzeltti. İstemcinin yeni sürümü, uzak oturum saat dilimini gösterecektir.

2. MSI 'yi konak VM 'ye yüklemek için aşağıdaki komutlardan birini çalıştırın:

    - Kullanıcı başına yükleme

        ```powershell
        msiexec /i <path_to_msi> /l*v <install_logfile_name>
        ```

        Bu işlem, takımları **% AppData%** Kullanıcı klasörüne yükleyecek olan varsayılan yüklemedir. Ekipler, kalıcı olmayan bir kurulumda Kullanıcı başına yükleme ile düzgün çalışmaz.

    - Makine başına yükleme

        ```powershell
        msiexec /i <path_to_msi> /l*v <install_logfile_name> ALLUSER=1
        ```

        Bu, ekipleri 64 bitlik bir işletim sisteminde Program Files (x86) klasörüne ve 32 bit işletim sistemindeki Program Files klasörüne yüklenir. Bu noktada, altın görüntü kurulumu tamamlanmıştır. Kalıcı olmayan kurulum için makine başına takımlar yükleme gerekir.

        Ekipler yüklenirken ayarlanmış olabilecek iki bayrak vardır, **AllUser = 1** ve **ALLUSERS = 1**. Bu parametreler arasındaki farkı anlamak önemlidir. **AllUser = 1** PARAMETRESI yalnızca VDI ortamlarında, makine başına yükleme belirtmek için kullanılır. **ALLUSERS = 1** parametresi VDI olmayan ve VDI ortamlarında kullanılabilir. Bu parametreyi ayarladığınızda, takımlar makine genelinde yükleyici, Denetim Masası 'ndaki program ve Özellikler ' in yanı sıra Windows ayarlarındaki uygulamalar & Özellikler ' de görünür. Makinede yönetici kimlik bilgilerine sahip tüm kullanıcılar takımları kaldırabilir. 
       
        > [!NOTE]
        > Kullanıcılar ve Yöneticiler, oturum açma sırasında şu anda takımlar için otomatik başlatmayı devre dışı bırakamıyorum.

3. MSI 'yi konak VM 'den kaldırmak için şu komutu çalıştırın:

      ```powershell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      Bu, işletim sistemi ortamına bağlı olarak, takımları Program Files (x86) klasöründen veya Program Files klasöründen kaldırır.

      > [!NOTE]
      > Ekipleri ALLUSER = 1 MSI ayarıyla yüklediğinizde otomatik güncelleştirmeler devre dışı bırakılır. Ekipleri ayda en az bir kez güncelleştirdiğinizden emin olmanızı öneririz. Ekipler masaüstü uygulamasını dağıtma hakkında daha fazla bilgi edinmek için, [takımlar masaüstü UYGULAMASıNı VM 'ye dağıtmayı](/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm/)inceleyin.

### <a name="verify-media-optimizations-loaded"></a>Medya iyileştirmelerinin yüklendiğini doğrulama

WebSocket hizmetini ve takımlar masaüstü uygulamasını yükledikten sonra, takımlar medya iyileştirmelerinin yüklendiğini doğrulamak için aşağıdaki adımları izleyin:

1. Kullanıcı profili görüntünüzü seçin ve ardından **hakkında**' yı seçin.
2. **Sürüm**seçin.

      Medya iyileştirmesi yüklüyse, başlık, **WVD Media optimize edilmiş**olarak gösterilir. Başlıkta **WVD Media bağlı değil**görünüyorsa, takımlar uygulamasından çıkıp yeniden deneyin.

3. Kullanıcı profili görüntünüzü seçin ve ardından **Ayarlar**' ı seçin.

      Medya iyileştirmesi yüklüyse, yerel olarak bulunan ses cihazları ve kameralar cihaz menüsünde numaralandırılır. Menü **uzak sesi**gösteriyorsa, takımlar uygulamasından çıkıp yeniden deneyin. Cihazlar hala menüde görünmüyorsa, yerel bilgisayarınızdaki gizlilik ayarlarını kontrol edin. **Ayarlar**  >  **Gizlilik**  >  **Uygulama izinleri** altındaki **uygulamaların mikrofona erişmesine izin ver** ayarının **Açık**olduğundan emin olun. Uzak oturumla bağlantıyı kesin, sonra yeniden bağlayın ve ses ve video cihazlarını tekrar denetleyin. Aramalara ve toplantılara video ile katılabilmek için, ayrıca uygulamaların kameranıza erişmesine izin vermeniz gerekir.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Sanallaştırılmış bir ortamda takımlar kullanmak, sanallaştırılmamış bir ortamda takımlar kullanmaktan farklıdır. Sanallaştırılmış ortamlarda ekiplerin sınırlamaları hakkında daha fazla bilgi için, [sanallaştırılmış Masaüstü Altyapısı Için takımları](/microsoftteams/teams-for-vdi#known-issues-and-limitations)inceleyin.

### <a name="client-deployment-installation-and-setup"></a>İstemci dağıtımı, yükleme ve kurulum

- Makine başına yüklemede, VDı 'daki takımlar, VDı ekipleri olmayan ekipler istemcilerinin de aynı şekilde otomatik olarak güncellenmez. İstemciyi güncelleştirmek için yeni bir MSI yükleyerek VM görüntüsünü güncelleştirmeniz gerekir.
- Takımlar için medya iyileştirmesi yalnızca Windows 10 çalıştıran makinelerde Windows Masaüstü istemcisi için desteklenir.
- Bir uç noktada tanımlanan açık HTTP proxy 'lerinin kullanımı desteklenmez.

### <a name="calls-and-meetings"></a>Çağrılar ve toplantılar

- Windows sanal masaüstü ortamlarında takımlar masaüstü istemcisi canlı olayları desteklemez. Şimdilik, uzak oturumunuzda [takımlar web istemcisinden](https://teams.microsoft.com) canlı olaylara katılmanız önerilir.
- Çağrılar veya toplantılar Şu anda uygulama paylaşımını desteklememektedir. Masaüstü oturumları masaüstü paylaşımını destekler.
- Denetimi verme ve denetimi alma şu anda desteklenmiyor.
- Windows sanal masaüstündeki takımlar tek seferde yalnızca bir gelen video girişini destekler. Diğer bir deyişle, her biri Ekranınızı paylaşmaya çalıştığında, Toplantı lideri ekranının yerine ekranın görünmesini sağlar.
- WebRTC sınırlamaları nedeniyle, gelen ve giden video akış çözünürlüğü 720p ile sınırlıdır.
- Takımlar uygulaması, diğer cihazlarla HID düğmelerini veya LED denetimlerini desteklemez.

Ekipler, sanallaştırılmış ortamlarla ilgili olmayan bilinen sorunlar için bkz. [kuruluşunuzdaki destek takımları](/microsoftteams/known-issues)

## <a name="uservoice-site"></a>UserVoice sitesi

Takımlar [UserVoice sitesindeki](https://microsoftteams.uservoice.com/)Windows sanal masaüstündeki Microsoft ekipleri için geri bildirim sağlayın.

## <a name="collect-teams-logs"></a>Takımlar günlüklerini topla

Windows sanal masaüstü ortamınızda takımlar masaüstü uygulamasıyla ilgili sorunlarla karşılaşırsanız, konak VM üzerinde **% AppData% \Microsoft\Teams\logs.txt** altında istemci günlüklerini toplayın.

Çağrılar ve toplantılar ile ilgili sorunlarla karşılaşırsanız, takımlar Web istemci günlüklerini, **CTRL**  +  **alt**  +  **SHIFT**  +  **1**tuş bileşimiyle toplayın. Günlükler, konak VM 'sindeki **%userprofile%\downloads\msteams tanılama günlüğü DATE_TIME.txt** yazılır.

## <a name="contact-microsoft-teams-support"></a>Microsoft ekipleri desteğiyle iletişim kurun

Microsoft ekipleri desteğiyle iletişim kurmak için [Microsoft 365 yönetim merkezine](/microsoft-365/admin/contact-support-for-business-products)gidin.

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Bir konak havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme

Bir konak havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini, çok Monitor deneyimi veya mikrofon ve ses yeniden yönlendirmeyi etkinleştirme gibi özelleştirme, kullanıcılarınız için gereksinimlerinize göre en iyi deneyim sunmanızı sağlar.

Ekipleri medya iyileştirmesi ile kullanılırken cihaz yeniden yönlendirmelerini etkinleştirme gerekli değildir. Medya iyileştirmesi olmadan takımlar kullanıyorsanız, mikrofon ve kamera yeniden yönlendirmeyi etkinleştirmek için aşağıdaki RDP özelliklerini ayarlayın:

- `audiocapturemode:i:1` Yerel cihazdan ses yakalamaya izin verebilir ve Uzak oturumdaki ses uygulamalarını yeniden yönlendirir.
- `audiomode:i:0` Yerel bilgisayarda ses çalar.
- `camerastoredirect:s:*` Tüm kameraları yeniden yönlendirir.

Daha fazla bilgi edinmek için, [bir konak havuzunun Uzak Masaüstü Protokolü özelliklerini özelleştirin](customize-rdp-properties.md)' i inceleyin.
