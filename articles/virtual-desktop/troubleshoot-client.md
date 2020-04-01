---
title: Sorun Giderme Uzak Masaüstü istemcisi Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü kiracı ortamında istemci bağlantıları ayarlarken sorunları nasıl giderebilirsiniz.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 595762e6e8f22dddff30f1cff8c4bb79e89624b1
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473859"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Uzak Masaüstü istemcisini giderme

Bu makalede, Uzak Masaüstü istemcisi ile ortak sorunları ve bunları nasıl düzeltmek için açıklanmaktadır.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi yanıt vermeyi durdurur veya açılamaz

Sürüm 1.2.790'dan başlayarak, kullanıcı verilerini Hakkında sayfasından veya bir komut kullanarak sıfırlayabilirsiniz.

Kullanıcı verilerinizi kaldırmak, varsayılan ayarları geri yüklemek ve tüm Çalışma Alanlarından aboneliğinizi kaldırmak için aşağıdaki komutu kullanın.

```cmd
msrdcw.exe /reset [/f]
```

Uzak Masaüstü istemcisinin önceki bir sürümünü kullanıyorsanız, istemciyi kaldırmanızı ve yeniden yüklemenizi öneririz.

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