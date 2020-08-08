---
title: Uzak Masaüstü istemcisi Windows sanal masaüstü-Azure sorunlarını giderme
description: Windows sanal masaüstü kiracı ortamında istemci bağlantılarını ayarlarken oluşan sorunları çözme.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 6ac2eb2632ad96538e965e4d1057d7e4a116c05d
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009401"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Uzak masaüstü istemcisinde sorun giderme

Bu makalede, uzak masaüstü istemcisiyle ilgili yaygın sorunlar ve bunların nasıl düzeltileceğini açıklar.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi yanıt vermeyi durduruyor veya açılamıyor

Sürüm 1.2.790 'den başlayarak, Kullanıcı verilerini hakkında sayfasından veya bir komut kullanarak sıfırlayabilirsiniz.

Kullanıcı verilerinizi kaldırmak, varsayılan ayarları geri yüklemek ve tüm çalışma alanlarının aboneliğini kaldırmak için aşağıdaki komutu kullanın.

```cmd
msrdcw.exe /reset [/f]
```

Uzak Masaüstü istemcisinin önceki bir sürümünü kullanıyorsanız istemcisini kaldırmanızı ve yeniden yüklemenizi öneririz.

## <a name="web-client-wont-open"></a>Web istemcisi açılmaz

İlk olarak, tarayıcınızda başka bir Web sitesi açarak internet bağlantınızı test edin; Örneğin, [www.Bing.com](https://www.bing.com).

DNS 'in FQDN 'yi çözediğini onaylamak için **nslookup** kullanın:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi gibi başka bir istemciyle bağlanmayı deneyin ve Web istemcisini açıp açmediğinizi denetleyin.

### <a name="opening-another-site-fails"></a>Başka bir site açma başarısız

Bu, genellikle ağ bağlantısı sorunlarından veya bir ağ kesintisi nedeniyle oluşur. Ağ desteğiyle iletişim kurmanız önerilir.

### <a name="nslookup-cannot-resolve-the-name"></a>Nslookup adı çözümleyemiyor

Bu, genellikle ağ bağlantısı sorunlarından veya bir ağ kesintisi nedeniyle oluşur. Ağ desteğiyle iletişim kurmanız önerilir.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>İstemciniz bağlanamaz, ancak ağınızdaki diğer istemciler bağlanabilir

Web istemcisini kullanırken tarayıcınızın çalışmaya başlaması veya çalışmayı durdurması durumunda sorun gidermek için aşağıdaki yönergeleri izleyin:

1. Tarayıcıyı yeniden başlatın.
2. Tarayıcı tanımlama bilgilerini temizleyin. Bkz. [Internet Explorer 'da tanımlama bilgisi dosyalarını silme](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Tarayıcı önbelleğini temizleyin. [Tarayıcınızın tarayıcı önbelleğini temizleme](https://binged.it/2RKyfdU)bölümüne bakın.
4. Tarayıcıyı özel modda açın.

## <a name="web-client-does-not-show-my-resources"></a>Web istemcisi kaynaklarımı göstermiyor

İlk olarak, kullanmakta olduğunuz Azure Active Directory hesabı kontrol edin. Windows sanal masaüstü için kullanmak istediğinden farklı bir Azure Active Directory hesabıyla oturum açtıysanız, veya özel tarayıcı penceresi kullanmanız gerekir.

Windows sanal masaüstü 'Nü (klasik) kullanıyorsanız, kaynaklarınıza bağlanmak için [Bu makaledeki](./virtual-desktop-fall-2019/connect-web-2019.md) Web istemcisi bağlantısını kullanın.

## <a name="web-client-stops-responding-or-disconnects"></a>Web istemcisi yanıt vermeyi durduruyor veya bağlantıyı keser

Başka bir tarayıcı veya istemci kullanarak bağlanmayı deneyin.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Diğer tarayıcılar ve istemciler de hatalı veya açılmayabilir

Tarayıcıları değiştirdikten sonra bile sorunlar devam ederse, sorun tarayıcınızla, ancak ağınızla birlikte olmayabilir. Ağ desteğiyle iletişim kurmanız önerilir.

## <a name="web-client-keeps-prompting-for-credentials"></a>Web istemcisi kimlik bilgileri istemini tutar

Web istemcisi kimlik bilgileri isteyip istemediğini devam ederse, şu yönergeleri izleyin:

1. Web istemcisi URL 'sinin doğru olduğundan emin olun.
2. Kullandığınız kimlik bilgilerinin, URL 'ye bağlı Windows sanal masaüstü ortamı için olduğunu doğrulayın.
3. Tarayıcı tanımlama bilgilerini temizleyin. Daha ayrıntılı bilgi için bkz. [Internet Explorer 'da tanımlama bilgisi dosyalarını silme](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Tarayıcı önbelleğini temizleyin. Daha ayrıntılı bilgi için bkz. [tarayıcınızın tarayıcı önbelleğini temizleme](https://binged.it/2RKyfdU).
5. Tarayıcınızı özel modda açın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme izlemelerinin sorunlarını giderme hakkında genel bilgi için bkz. [sorun giderme genel bakış, geri bildirim ve destek](troubleshoot-set-up-overview.md).
- Windows sanal masaüstü ortamında bir Windows sanal masaüstü ortamı ve konak havuzu oluştururken oluşan sorunları gidermek için, bkz. [ortam ve konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Sorun giderme öğreticisini öğrenmek için bkz. [öğretici: Kaynak Yöneticisi şablonu dağıtımlarının sorunlarını giderme](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
