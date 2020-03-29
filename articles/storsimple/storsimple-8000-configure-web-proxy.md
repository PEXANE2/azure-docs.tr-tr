---
title: StorSimple 8000 serisi cihaz için web proxy'yi ayarlama | Microsoft Dokümanlar
description: StorSimple cihazınızIçin web proxy ayarlarını yapılandırmak için StorSimple için Windows PowerShell'i nasıl kullanacağınızı öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: ''
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: alkohli
ms.openlocfilehash: 956cf45eb9e246f2e1f917f2bf487ac14deba90e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65204260"
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>StorSimple aygıtınız için web proxy'yi yapılandırın

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple aygıtınız için web proxy ayarlarını yapılandırmak ve görüntülemek için StorSimple için Windows PowerShell'in nasıl kullanılacağını açıklar. Web proxy ayarları, bulutla iletişim kurarken StorSimple aygıtı tarafından kullanılır. Bir web proxy sunucusu, bant genişliği gereksinimlerini kolaylaştırmak ve hatta analitik konusunda yardımcı olmak için başka bir güvenlik katmanı eklemek, içeriği filtrelemek, önbellek eklemek için kullanılır.

Bu öğreticideki kılavuz yalnızca StorSimple 8000 serisi fiziksel aygıtlar için geçerlidir. Web proxy yapılandırması StorSimple Cloud Appliance (8010 ve 8020) üzerinde desteklenmez.

Web proxy, StorSimple aygıtınız için _isteğe bağlı_ bir yapılandırmadır. StorSimple için web proxy'yi yalnızca Windows PowerShell üzerinden yapılandırabilirsiniz. Yapılandırma aşağıdaki gibi iki adımlı bir işlemdir:

1. Önce StorSimple cmdlets için kurulum sihirbazı veya Windows PowerShell üzerinden web proxy ayarlarını yapılandırırsınız.
2. Daha sonra StorSimple cmdlets için Windows PowerShell üzerinden yapılandırılan web proxy ayarlarını etkinleştirin.

Web proxy yapılandırması tamamlandıktan sonra, hem Microsoft Azure StorBasit Aygıt Yöneticisi hizmetinde hem de StorSimple için Windows PowerShell'de yapılandırılan web proxy ayarlarını görüntüleyebilirsiniz.

Bu öğretici okuduktan sonra, mümkün olacak:

* Kurulum sihirbazı ve cmdlets kullanarak web proxy yapılandırın.
* Cmdlets kullanarak web proxy etkinleştirin.
* Azure portalında web proxy ayarlarını görüntüleyin.
* Web proxy yapılandırması sırasında hataları giderme.


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden web proxy'yi yapılandırın

Web proxy ayarlarını yapılandırmak için aşağıdakilerden birini kullanırsınız:

* Yapılandırma adımlarını size yönlendirmek için kurulum sihirbazı.
* StorSimple için Windows PowerShell'de cmdletler.

Bu yöntemlerin her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Kurulum sihirbazı aracılığıyla web proxy'yi yapılandır

Web proxy yapılandırması için adımlar boyunca size rehberlik etmek için kurulum sihirbazını kullanın. Cihazınızda web proxy'sini yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Kurulum sihirbazı aracılığıyla web proxy yapılandırmak için

1. Seri konsol menüsünde, seçenek 1'i seçin, **tam erişimle giriş yapın** ve aygıt yöneticisi **parolasını**sağlayın. Kurulum sihirbazı oturumunu başlatmak için aşağıdaki komutu yazın:
   
    `Invoke-HcsSetupWizard`
2. Aygıt kaydı için kurulum sihirbazını ilk kez kullandıysanız, web proxy yapılandırmasına ulaşana kadar gerekli tüm ağ ayarlarını yapılandırmanız gerekir. Aygıtınız zaten kayıtlıysa, web proxy yapılandırmasına ulaşana kadar yapılandırılan tüm ağ ayarlarını kabul edin. Kurulum sihirbazında, web proxy ayarlarını yapılandırması istendiğinde **Evet**yazın.
3. Web **Proxy URL'si için,** web proxy sunucunuzun IP adresini veya tam nitelikli alan adını (FQDN) ve bulutla iletişim kurarken cihazınızın kullanmasını istediğiniz TCP bağlantı noktası numarasını belirtin. Şu biçimi kullanın:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Varsayılan olarak, TCP bağlantı noktası numarası 8080 belirtilir.
4. Kimlik doğrulama türünü **NTLM,** **Temel**veya **Yok**olarak seçin. Temel proxy sunucusu yapılandırması için en az güvenli kimlik doğrulamasıdır. NT LAN Yöneticisi (NTLM), bir kullanıcının kimliğini doğrulamak için üç yönlü bir ileti sistemi (bazen dört, ek bütünlük gerekiyorsa) kullanan son derece güvenli ve karmaşık bir kimlik doğrulama protokolüdür. Varsayılan kimlik doğrulaması NTLM'dir. Daha fazla bilgi için [Temel](https://hc.apache.org/httpclient-3.x/authentication.html) ve [NTLM kimlik doğrulamabilgisine](https://hc.apache.org/httpclient-3.x/authentication.html)bakın. 
   
   > [!IMPORTANT]
   > **StorSimple Device Manager hizmetinde, aygıt için proxy sunucu yapılandırmasında Temel veya NTLM kimlik doğrulaması etkinleştirildiğinde aygıt izleme grafikleri çalışmaz. İzleme çizelgelerin çalışması için kimlik doğrulamanın NONE olarak ayarlandığından emin olmanız gerekir.**
  
5. Kimlik doğrulamasını etkinleştirdiyseniz, bir **Web Proxy Kullanıcı Adı** ve Web Proxy **Şifresi**girin. Ayrıca parolayı onaylamanız gerekir.
   
    ![StorSimple Device1'de Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751830.png)

Cihazınızı ilk kez kaydediyorsanız, kayıt la devam edin. Aygıtınız zaten kayıtlıysa sihirbaz çıkar. Yapılandırılan ayarlar kaydedilir.

Web proxy şimdi etkinleştirildi. [Web proxy'yi etkinleştir](#enable-web-proxy) adımını atlayabilir ve doğrudan Azure [portalındaki web proxy ayarlarını görüntüleyebilirsiniz.](#view-web-proxy-settings-in-the-azure-portal)

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>StorSimple cmdlets için Windows PowerShell üzerinden web proxy yapılandırma

Web proxy ayarlarını yapılandırmanın alternatif bir yolu StorSimple cmdlets için Windows PowerShell üzerinden. Web proxy'sini yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Cmdlets ile web proxy yapılandırmak için
1. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.** İstendiğinde, **aygıt yöneticisi parolasını**girin. Varsayılan `Password1`parola.
2. Komut istemine şunları yazın:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    İstendiğinde parolayı sağlayın ve onaylayın.
   
    ![StorSimple Device3'te Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751831.png)

Web proxy şimdi yapılandırıldı ve etkin leştirilmesi gerekiyor.

## <a name="enable-web-proxy"></a>Web proxy'sini etkinleştirme

Web proxy varsayılan olarak devre dışı bırakılır. StorSimple cihazınızdaki web proxy ayarlarını yapılandırdıktan sonra, web proxy ayarlarını etkinleştirmek için StorSimple için Windows PowerShell'i kullanın.

> [!NOTE]
> **Web proxy'sini yapılandırmak için kurulum sihirbazını kullandıysanız bu adım gerekli değildir. Web proxy varsayılan olarak bir kurulum sihirbazı oturumundan sonra etkinleştirilir.**


Cihazınızda web proxy'sini etkinleştirmek için StorSimple için Windows PowerShell'de aşağıdaki adımları gerçekleştirin:

#### <a name="to-enable-web-proxy"></a>Web proxy'sini etkinleştirmek için
1. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.** İstendiğinde, **aygıt yöneticisi parolasını**girin. Varsayılan `Password1`parola.
2. Komut istemine şunları yazın:
   
    `Enable-HcsWebProxy`
   
    Artık StorSimple cihazınızda web proxy yapılandırmasını etkinleştirdin.
   
    ![StorSimple Device4'te Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-portal"></a>Azure portalında web proxy ayarlarını görüntüleme

Web proxy ayarları Windows PowerShell arabirimi üzerinden yapılandırılır ve portal içinden değiştirilemez. Ancak, bu yapılandırılmış ayarları portalda görüntüleyebilirsiniz. Web proxy'sini görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-web-proxy-settings"></a>Web proxy ayarlarını görüntülemek için
1. Aygıtlar > **StorSimple Aygıt Yöneticisi hizmetine**gidin. Bir aygıtı seçin ve tıklatın ve ardından **Aygıt ayarları > Ağına**gidin.

    ![Ağ'ı tıklatın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-1.png)

2. Ağ **ayarları** çubuğunda, **Web proxy** döşemesini tıklatın.

    ![Web proxy'yi tıklatın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-2.png)

3. Web **proxy** blade'inde, StorSimple cihazınızda yapılandırılan web proxy ayarlarını gözden geçirin.
   
    ![Web proxy ayarlarını görüntüleme](./media/storsimple-8000-configure-web-proxy/view-web-proxy-3.png)


## <a name="errors-during-web-proxy-configuration"></a>Web proxy yapılandırması sırasında hatalar

Web proxy ayarları yanlış yapılandırılırsa, hata iletileri StorSimple için Windows PowerShell kullanıcıya görüntülenir. Aşağıdaki tabloda bu hata iletilerinin bazıları, bunların olası nedenleri ve önerilen eylemler açıklanmaktadır.

| Seri no. | HRESULT hata Kodu | Olası kök neden | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Komut pasif denetleyiciden çalıştırılır ve etkin denetleyici ile iletişim kuramaz. |Etkin denetleyicideki komutu çalıştırın. Komutu pasif denetleyiciden çalıştırmak için, bağlayımı pasiften etkin denetleyiciye sabitlemeniz gerekir. Bu bağlantı bozuksa Microsoft Desteği'ni meşgul edmelisiniz. |
| 2. |0x800710dd - Operasyon tanımlayıcısı geçerli değil |Proxy ayarları StorSimple Cloud Appliance'ta desteklenmez. |Proxy ayarları StorSimple Cloud Appliance'ta desteklenmez. Bunlar yalnızca StorSimple fiziksel aygıtında yapılandırılabilir. |
| 3. |0x80070057 - Geçersiz parametre |Proxy ayarları için sağlanan parametrelerden biri geçerli değildir. |URI doğru biçimde sağlanmaz. Aşağıdaki biçimi kullanın:`http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC sunucusu kullanılamıyor |Temel neden aşağıdakilerden biridir:</br></br>Küme açık değil. </br></br>Datapath hizmeti çalışmıyor.</br></br>Komut pasif denetleyiciden çalıştırılır ve etkin denetleyici ile iletişim kuramaz. |Kümenin dolduğundan ve datapath hizmetinin çalıştığından emin olmak için Microsoft Desteği'ni meşgul edin.</br></br>Komutu etkin denetleyiciden çalıştırın. Komutu pasif denetleyiciden çalıştırmak istiyorsanız, pasif denetleyicinin etkin denetleyiciyle iletişim kurabileceğinden emin olmalısınız. Bu bağlantı bozuksa Microsoft Desteği'ni meşgul edmelisiniz. |
| 5. |0x800706be - RPC arama başarısız oldu |Küme düştü. |Kümenin açık olduğundan emin olmak için Microsoft Desteği'ni meşgul edin. |
| 6. |0x8007138f - Küme kaynağı bulunamadı |Platform hizmet küme kaynağı bulunamadı. Bu, yükleme uygun olmadığında gerçekleşebilir. |Cihazınızda bir fabrika sıfırlama gerçekleştirmeniz gerekebilir. Bir platform kaynağı oluşturmanız gerekebilir. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| 7. |0x8007138c - Küme kaynağı çevrimiçi değil |Platform veya datapath küme kaynakları çevrimiçi değildir. |Veri yolu ve platform hizmet kaynağının çevrimiçi olduğundan emin olmak için Microsoft Destek'e başvurun. |

> [!NOTE]
> * Yukarıdaki hata iletileri listesi ayrıntılı değildir.
> * Web proxy ayarlarıyla ilgili hatalar StorSimple Device Manager hizmetinizde Azure portalında görüntülenmez. Yapılandırma tamamlandıktan sonra web proxy ile ilgili bir sorun varsa, aygıt durumu klasik portalda **Çevrimdışı** olarak değişecektir.|

## <a name="next-steps"></a>Sonraki Adımlar
* Cihazınızı dağıtırken veya web proxy ayarlarını yapılandırırken herhangi bir sorunla karşılaşırsanız, [StorSimple aygıt dağıtımınızı sorun gidermeye](storsimple-troubleshoot-deployment.md)bakın.
* StorSimple Device Manager hizmetini nasıl kullanacağınızı öğrenmek için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

