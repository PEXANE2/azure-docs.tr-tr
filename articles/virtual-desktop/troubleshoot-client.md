---
title: Sorun Giderme Uzak Masaüstü istemcisi Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü kiracı ortamında istemci bağlantıları ayarlarken sorunları nasıl giderebilirsiniz.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: e3a240901ffca2c126e2b61eaee0cf287cc31d6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127497"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Uzak Masaüstü istemcisini giderme

Bu makalede, Uzak Masaüstü istemcisi ile ortak sorunları ve bunları nasıl düzeltmek için açıklanmaktadır.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi yanıt vermeyi durdurur veya açılamaz

Bant dışı (OOB) istemci kayıt larını temizlemek için aşağıdaki PowerShell cmdlets'i kullanın.

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

**%AppData%\RdClientRadc'a** gidin ve tüm içeriği silin.

Windows 7 ve Windows 10 için Uzak Masaüstü istemcisi kaldırın ve yeniden yükleyin.

## <a name="web-client-wont-open"></a>Web istemcisi açılmıyor

İlk olarak, tarayıcınızda başka bir web sitesi açarak internet bağlantınızı test edin; örneğin, [www.bing.com](https://www.bing.com).

DNS'nin FQDN'yi çözebileceğini doğrulamak için **nslookup'u** kullanın:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi gibi başka bir istemciyle bağlanmayı deneyin ve web istemcisini açıp açamayacağınızı kontrol edin.

### <a name="opening-another-site-fails"></a>Başka bir site açma başarısız olur

Bu genellikle ağ bağlantısı sorunları veya ağ kesintisi neden olur. Ağ desteğiyle iletişim kurmanızı öneririz.

### <a name="nslookup-cannot-resolve-the-name"></a>Nslookup adı çözemez

Bu genellikle ağ bağlantısı sorunları veya ağ kesintisi neden olur. Ağ desteğiyle iletişim kurmanızı öneririz.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>Müşteriniz bağ kuramıyor, ancak ağınızdaki diğer istemciler bağlanabilir

Tarayıcınız web istemcisini kullanırken çalışmaya başlarsa veya çalışmayı durdurursa, sorun giderme için aşağıdaki yönergeleri izleyin:

1. Tarayıcıyı yeniden başlatın.
2. Tarayıcı çerezlerini temizleyin. [Bkz. Internet Explorer'daki çerez dosyaları nasıl silinir.](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer)
3. Tarayıcı önbelleğini temizleyin. [Tarayıcınız için net tarayıcı önbelleğine](https://binged.it/2RKyfdU)bakın.
4. Özel modda tarayıcıyı açın.

## <a name="web-client-stops-responding-or-disconnects"></a>Web istemcisi yanıt vermeyi durdurur veya bağlantıyı keser

Başka bir tarayıcı veya istemci kullanarak bağlanmayı deneyin.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Diğer tarayıcılar ve istemciler de arıza veya açmak için başarısız

Tarayıcıları değiştirdikten sonra bile sorunlar devam ederse, sorun tarayıcınızda değil, ağınızla ilgili olabilir. Ağ desteğiyle iletişim kurmanızı öneririz.

## <a name="web-client-keeps-prompting-for-credentials"></a>Web istemcisi kimlik bilgileri için istekte tutar

Web istemcisi kimlik bilgilerini sormaya devam ederse, aşağıdaki yönergeleri izleyin:

1. Web istemcisi URL'sinin doğru olduğunu onaylayın.
2. Kullandığınız kimlik bilgilerinin URL'ye bağlı Windows Sanal Masaüstü ortamı için olduğunu doğrulayın.
3. Tarayıcı çerezlerini temizleyin. Daha fazla bilgi için [Internet Explorer'daki çerez dosyalarının nasıl silinine](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer)bakın.
4. Tarayıcı önbelleğini temizleyin. Daha fazla bilgi için [tarayıcınız için tarayıcı önbelleğini temizle'ye](https://binged.it/2RKyfdU)bakın.
5. Tarayıcınızı Özel modda açın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Windows Sanal Masaüstü ortamında kiracı ve ana bilgisayar havuzu oluştururken sorunları gidermek için [Bkz. Kiracı ve ana bilgisayar havuzu oluşturma.](troubleshoot-set-up-issues.md)
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Virtual Desktop ile PowerShell kullanırken sorunları gidermek için [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)