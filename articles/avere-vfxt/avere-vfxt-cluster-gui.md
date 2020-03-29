---
title: Avere vFXT kontrol paneline erişin - Azure
description: Avere vFXT'yi yapılandırmak için vFXT kümesine ve tarayıcı tabanlı Avere Denetim Masası'na nasıl bağlanır?
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/14/2019
ms.author: rohogue
ms.openlocfilehash: fe2fc062f690498f3d1f588887279aa33d2434b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75416142"
---
# <a name="access-the-vfxt-cluster"></a>vFXT kümesine erişme

Küme ayarlarını ayarlamak ve kümeyi izlemek için Avere Denetim Masası'nı kullanın. Avere Control Panel küme için tarayıcı tabanlı bir grafik arabirimidir.

vFXT kümesi özel bir sanal ağ içinde yer aldığı için, bir SSH tüneli oluşturmanız veya kümenin yönetim IP adresine ulaşmak için başka bir yöntem kullanmanız gerekir.

İki temel adım vardır:

1. İş istasyonunuz la özel sanal ağ arasında bağlantı kurma
1. Kümenin denetim panelini bir web tarayıcısında yükleme

> [!NOTE]
> Bu makalede, küme denetleyicisi veya kümesanal ağ içinde başka bir VM üzerinde ortak bir IP adresi ayarladığınız varsayar. Bu makalede, kümeye erişmek için bu VM'nin ana bilgisayar olarak nasıl kullanılacağı açıklanmaktadır. Sanal ağ erişimi için VPN veya ExpressRoute kullanıyorsanız, [Avere Kontrol Paneline Bağlan'a](#connect-to-the-avere-control-panel-in-a-browser)atlayın.

Bağlanmadan önce, küme denetleyicisini oluştururken kullandığınız SSH ortak/özel anahtar çiftinin yerel makinenize yüklendiğinden emin olun. Yardıma ihtiyacınız varsa [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) için SSH tuşları belgelerini okuyun. Ortak anahtar yerine parola kullandıysanız, bağlandığınızda parolayı girmeniz istenir.

## <a name="create-an-ssh-tunnel"></a>Bir SSH tüneli oluşturma

Linux tabanlı veya Windows 10 istemci sisteminin komut satırından bir SSH tüneli oluşturabilirsiniz.

Bu formu içeren bir SSH tünel komutu kullanın:

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*\@*controller_public_IP*

Bu komut, küme denetleyicisinin IP adresi aracılığıyla kümenin yönetim IP adresine bağlanır.

Örnek:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Küme oluşturmak için SSH ortak anahtarınızı kullandıysanız ve istemci sistemine eşleşen anahtar yüklüyse kimlik doğrulama otomatiktir. Bir parola kullandıysanız, sistem parolayı girmenizi ister.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Tarayıcıda Avere Kontrol Paneline bağlanma

Bu adım, vFXT kümesindeki yapılandırma yardımcı programına bağlanmak için bir web tarayıcısı kullanır.

* Bir SSH tünel bağlantısı için web tarayıcınızı açın ve ' e `https://127.0.0.1:8443`gidin.

  Tüneli oluşturduğunuzda küme IP adresine bağlandınız, bu nedenle tarayıcıdaki localhost IP adresini kullanmanız yeterlidir. 8443'ten başka yerel bir bağlantı noktası kullandıysanız, bunun yerine bağlantı noktası numaranızı kullanın.

* Kümeye ulaşmak için VPN veya ExpressRoute kullanıyorsanız, tarayıcınızdaki küme yönetimi IP adresine gidin. Örnek: ``https://203.0.113.51``

Tarayıcınıza bağlı olarak Gelişmiş'i **tıklatmanız** ve sayfaya devam etmek için güvenli olduğundan doğrulamanız gerekebilir.

Küme oluşturulurken `admin` sağladığınız kullanıcı adını ve yönetim parolasını girin.

![Kullanıcı adı 'admin' ve parola ile doldurulan sayfadaki Avere oturumunun ekran görüntüsü](media/avere-vfxt-gui-login.png)

**Giriş'e** veya klavyenizde enter tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Kümenin denetim paneline giriş yaptıktan sonra [desteği](avere-vfxt-enable-support.md)etkinleştirin.
