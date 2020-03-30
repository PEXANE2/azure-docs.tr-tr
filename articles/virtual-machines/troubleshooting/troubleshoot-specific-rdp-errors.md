---
title: Azure VM'ler için özel RDP hata iletileri | Microsoft Dokümanlar
description: Azure'daki bir Windows sanal makinesine Uzak Masaüstü bağlantısını kullanmaya çalışırken alabileceğiniz belirli hata iletilerini anlama
keywords: Uzak masaüstü hatası, uzak masaüstü bağlantı hatası, VM,uzak masaüstü sorun giderme bağlanamıyor
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 851c5eb4ebfee4e4a4836a07b51578dd2b0c68cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266877"
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Azure’da Windows VM’si için belirli RDP hata iletileri sorunları giderme
Azure'daki bir Windows sanal makinesine (VM) Uzak Masaüstü bağlantısını kullanırken belirli bir hata iletisi alabilirsiniz. Bu makalede, karşılaşılan daha yaygın hata iletilerinin bazılarının yanı sıra bunları gidermek için sorun giderme adımlarını ayrıntılarıyla birlikte anlatılır. RDP kullanarak VM'nize bağlanmada sorun yaşıyorsanız ancak belirli bir hata iletisi ile karşılaşmıyorsanız, [Uzak Masaüstü için sorun giderme kılavuzuna](troubleshoot-rdp-connection.md)bakın.

Belirli hata iletileri hakkında bilgi için aşağıdakilere bakın:

* [Lisans sağlamak için kullanılabilir uzak masaüstü lisans sunucuları olmadığından uzak oturum kesildi.](#rdplicense)
* [Uzak Masaüstü bilgisayar "adı" bulamıyorum.](#rdpname)
* [Kimlik doğrulama hatası oluştu. Yerel Güvenlik Otoritesi ile bağlantı kurulamıyor.](#rdpauth)
* [Windows Security hatası: Kimlik bilgileriniz çalışmadı.](#wincred)
* [Bu bilgisayar uzak bilgisayara bağlanamıyor.](#rdpconnect)

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Lisans sağlamak için kullanılabilir Uzak Masaüstü Lisans Sunucuları olmadığından uzak oturum kesildi.
Neden: Uzak Masaüstü Sunucusu rolü için 120 günlük lisans yetkisiz kullanım süresi doldu ve lisans yüklemeniz gerekiyor.

Geçici çözüm olarak, RDP dosyasının yerel bir kopyasını portaldan kaydedin ve bağlanmak için PowerShell komut isteminde bu komutu çalıştırın. Bu adım, sadece bu bağlantı için lisanslamadevre dışı bırak:

        mstsc <File name>.RDP /admin

VM'ye ikiden fazla aynı anda Uzak Masaüstü bağlantısına gerçekten ihtiyacınız yoksa, Uzak Masaüstü Sunucusu rolünü kaldırmak için Server Manager'ı kullanabilirsiniz.

Daha fazla bilgi için Azure VM blog yazısının ["Kullanılabilir Uzak Masaüstü Lisans Sunucuları Yok" ile başarısız olduğunu](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/)görün.

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Uzak Masaüstü bilgisayarı "ad" bulamıyor.
Neden: Bilgisayarınızdaki Uzak Masaüstü istemcisi, RDP dosyasının ayarlarında bilgisayarın adını çözemez.

Olası çözümler:

* Bir kuruluşun intranetindeyseniz, bilgisayarınızın proxy sunucusuna erişebildiğinizden ve bu sunucuya HTTPS trafiği gönderebildiğinizden emin olun.
* Yerel olarak depolanan bir RDP dosyası kullanıyorsanız, portal tarafından oluşturulan dosyayı kullanmayı deneyin. Bu adım, sanal makine veya bulut hizmeti ve VM'nin bitiş noktası bağlantı noktası için doğru DNS adına sahip olduğunuzu sağlar. Portal tarafından oluşturulan örnek bir RDP dosyası aşağıda veda edebilirsiniz:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Bu RDP dosyasının adres bölümü:

* Bu örnekte VM ("tailspin-azdatatier.cloudapp.net" içeren bulut hizmetinin tam nitelikli alan adı).
* Uzak Masaüstü trafiği (55919) için bitiş noktasının harici TCP bağlantı noktası.

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Kimlik doğrulama hatası oluştu. Yerel Güvenlik Otoritesi ile bağlantı kuramaz.
Neden: Hedef VM, kimlik bilgilerinizin kullanıcı adı bölümünde güvenlik yetkilisini bulamıyor.

Kullanıcı adınız *SecurityAuthority*\\*Kullanıcı Adı* (örnek: CORP\User1) biçiminde olduğunda, *SecurityAuthority* bölümü VM'nin bilgisayar adı (yerel güvenlik yetkilisi için) veya Active Directory etki alanı adıdır.

Olası çözümler:

* Hesap VM'ye yerelse, VM adının doğru yazıldığından emin olun.
* Hesap Etkin Dizin etki alanındaysa, etki alanı adının yazımını denetleyin.
* Etkin Dizin etki alanı hesabıysa ve etki alanı adı doğru yazılmışsa, bu etki alanında bir etki alanı denetleyicisinin kullanılabilir olduğunu doğrulayın. Bu, başlatılmadığı için etki alanı denetleyicileri içeren Azure sanal ağlarında sık karşılaşılan bir sorundur. Geçici çözüm olarak, etki alanı hesabı yerine yerel yönetici hesabı kullanabilirsiniz.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows Security hatası: Kimlik bilgileriniz çalışmadı.
Neden: Hedef VM hesap adınızı ve parolanızı doğrulayamıyor.

Windows tabanlı bir bilgisayar, yerel bir hesabın veya etki alanı hesabının kimlik bilgilerini doğrulayabilir.

* Yerel hesaplar için *ComputerName*\\*Kullanıcı Adı* sözdizimini kullanın (örnek: SQL1\Admin4798).
* Etki alanı hesapları için *DomainName*\\*Kullanıcı Adı* sözdizimini (örnek: CONTOSO\peterodman) kullanın.

VM'nizi yeni bir Active Directory ormanında bir etki alanı denetleyicisine terfi ettirdiyseniz, oturum açmış olduğunuz yerel yönetici hesabı, yeni orman ve etki alanında aynı parolaya sahip eşdeğer bir hesaba dönüştürülür. Yerel hesap daha sonra silinir.

Örneğin, yerel hesap DC1\DCAdmin ile oturum açmış ve daha sonra corp.contoso.com etki alanı için yeni bir ormanda etki alanı denetleyicisi olarak sanal makine terfi, DC1\DCAdmin yerel hesabı silinir ve yeni bir etki alanı hesabı (CORP\DCAdmin) olur aynı parolaile oluşturulur.

Hesap adının, sanal makinenin geçerli bir hesap olarak doğrulayabildiği bir ad olduğundan ve parolanın doğru olduğundan emin olun.

Yerel yönetici hesabının parolasını değiştirmeniz gerekiyorsa, [parolayı veya Windows sanal makineleri için Uzak Masaüstü hizmetini nasıl sıfırlarsınız'a](reset-rdp.md)bakın.

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Bu bilgisayar uzak bilgisayara bağlanamıyor.
Neden: Bağlanmak için kullanılan hesabın Uzak Masaüstü oturum açma hakları yoktur.

Her Windows bilgisayarı, uzaktan oturum açabilen hesapları ve grupları içeren bir Uzak Masaüstü kullanıcıları yerel grubuna sahiptir. Bu hesaplar Uzak Masaüstü kullanıcıları yerel grubunda listelenmese de yerel yöneticiler grubunun üyeleri de erişime sahiptir. Etki alanı birleştirilmiş makineler için, yerel yöneticiler grubu etki alanı için etki alanı yöneticilerini de içerir.

Bağlanmak için kullandığınız hesabın Uzak Masaüstü oturum açma haklarına sahip olduğundan emin olun. Geçici çözüm olarak, Uzak Masaüstü üzerinden bağlanmak için bir etki alanı veya yerel yönetici hesabı kullanın. Uzak Masaüstü kullanıcıları yerel grubuna istenilen hesabı eklemek için Microsoft Yönetim Konsolu snap-in **(System Tools > Yerel Kullanıcılar ve Gruplar > Grupları > Uzak Masaüstü Kullanıcıları**kullanın).

## <a name="next-steps"></a>Sonraki adımlar
Bu hataların hiçbiri oluştuysa ve RDP kullanarak bağlanmayla ilgili bilinmeyen bir sorununuz varsa, [Uzak Masaüstü için sorun giderme kılavuzuna](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

* VM'de çalışan uygulamalara erişmedeki sorun giderme adımları [için, Azure VM'de çalışan bir uygulamaya sorun giderme erişimi](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)ne bakın.
* Azure'daki bir Linux VM'ye bağlanmak için Secure Shell (SSH) kullanarak sorun yaşıyorsanız, [Azure'daki bir Linux VM'ye olan Sorun Giderme SSH bağlantılarına](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

