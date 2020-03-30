---
title: Azure VM'deki SSH bağlantı sorunlarını giderme | Microsoft Dokümanlar
description: Linux çalıştıran bir Azure VM için 'SSH bağlantısı başarısız oldu' veya 'SSH bağlantısı reddedildi' gibi sorunları nasıl giderilir?
keywords: ssh bağlantısı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 05/30/2017
ms.author: genli
ms.openlocfilehash: f221a0bdf579dbbf42ecf64e18803decfb718456
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060658"
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Başarısız olan, hata veren veya reddedilen Azure Linux VM SSH bağlantılarıyla ilgili sorunları giderme
Bu makale, Güvenli Kabuk (SSH) hataları, SSH bağlantı hataları veya Bir Linux sanal makineye (VM) bağlanmaya çalıştığınızda SSH reddedilir nedeniyle oluşan sorunları bulmanıza ve düzeltmenize yardımcı olur. Bağlantı sorunlarını gidermek ve gidermek için Linux için Azure portalını, Azure CLI'yi veya VM Access Extension'ı kullanabilirsiniz.


Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek **al'ı**seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Her sorun giderme adımından sonra VM'ye yeniden bağlanmayı deneyin.

1. [SSH yapılandırmasını sıfırla.](#reset-config)
2. [Kullanıcının kimlik bilgilerini sıfırla.](#reset-credentials)
3. Ağ [güvenlik grubu](../../virtual-network/security-overview.md) kurallarının SSH trafiğine izin verdiği doğrulayın.
   * SSH trafiğine izin vermek için [bir Ağ Güvenlik Grubu kuralının](#security-rules) bulunduğundan emin olun (varsayılan olarak TCP bağlantı noktası 22).
   * Azure yük dengeleyicisi kullanmadan bağlantı noktası yeniden yönlendirme / eşleme kullanamazsınız.
4. [VM kaynak durumunu](../../resource-health/resource-health-overview.md)kontrol edin.
   * VM'nin sağlıklı olduğundan emin olun.
   * [Önyükleme tanılama](boot-diagnostics.md)etkinse, VM'nin günlüklerde önyükleme hatalarını bildirmediğini doğrulayın.
5. [VM'yi yeniden başlatın.](#restart-vm)
6. [VM'yi yeniden dağıtın.](#redeploy-vm)

Daha ayrıntılı sorun giderme adımları ve açıklamalar için okumaya devam edin.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>SSH bağlantı sorunlarını gidermek için kullanılabilir yöntemler
Aşağıdaki yöntemlerden birini kullanarak kimlik bilgilerini veya SSH yapılandırmasını sıfırlayabilirsiniz:

* [Azure portalı](#use-the-azure-portal) - SSH yapılandırmasını veya SSH anahtarını hızlı bir şekilde sıfırlamanız gerekiyorsa ve Azure araçlarını yüklü yormuyorsanız harika.
* [Azure VM Seri Konsolu](https://aka.ms/serialconsolelinux) - VM seri konsolu, SSH yapılandırmasına bakılmaksızın çalışır ve VM'inize etkileşimli bir konsol sağlar. Aslında, "Can't SSH" durumları özellikle seri konsolun çözmeye yardımcı olmak için tasarladığı durumlardır. Aşağıda daha fazla ayrıntı bulabilirsiniz.
* [Azure CLI](#use-the-azure-cli) - komut satırındaysanız, SSH yapılandırmasını veya kimlik bilgilerini hızla sıfırlayın. Klasik bir VM ile çalışıyorsanız, [Azure klasik CLI'yi](#use-the-azure-classic-cli)kullanabilirsiniz.
* [Azure VMAccessForLinux uzantısı](#use-the-vmaccess-extension) - SSH yapılandırmasını veya kullanıcı kimlik bilgilerini sıfırlamak için json tanım dosyaları oluşturun ve yeniden kullanın.

Her sorun giderme adımından sonra VM'nize yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, bir sonraki adımı deneyin.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma
Azure portalı, yerel bilgisayarınıza herhangi bir araç yüklemeden SSH yapılandırmasını veya kullanıcı kimlik bilgilerini sıfırlamanın hızlı bir yolunu sağlar.

Başlamak için Azure portalında VM'nizi seçin. Destek + **Sorun Giderme** bölümüne gidin ve aşağıdaki örnekte olduğu gibi **parolayı sıfırla'yı** seçin:

![Azure portalında SSH yapılandırmasını veya kimlik bilgilerini sıfırlama](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a><a id="reset-config" />SSH yapılandırmasını sıfırlama
SSH yapılandırmasını sıfırlamak `Reset configuration only` için, önceki ekran görüntüsünde olduğu gibi **Mod** bölümünden seçin ve ardından **Güncelleştir'i**seçin. Bu eylem tamamlandıktan sonra, VM'nize yeniden erişmeye çalışın.

### <a name="reset-ssh-credentials-for-a-user"></a><a id="reset-credentials" />Bir kullanıcının SSH kimlik bilgilerini sıfırlama
Varolan bir kullanıcının kimlik bilgilerini `Reset SSH public key` sıfırlamak için, önceki ekran görüntüsünde olduğu gibi Mode bölümünden birini veya `Reset password` **Mode** bölümünü seçin. Kullanıcı adını ve SSH anahtarını veya yeni parolayı belirtin ve ardından **Güncelleştir'i**seçin.

Ayrıca bu menüden VM sudo ayrıcalıkları olan bir kullanıcı oluşturabilirsiniz. Yeni bir kullanıcı adı ve ilişkili parola veya SSH anahtarı girin ve sonra **Güncelleştir'i**seçin.

### <a name="check-security-rules"></a><a id="security-rules" />Güvenlik kurallarını denetleme

Ağ güvenlik grubundaki bir kuralın sanal bir makineye veya sanal makineden gelen trafiği engelleyip engellemediğikonusunda IP [akış doğrulamasını](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanın. Gelen "İzin Ver" NSG kuralının var olduğundan ve SSH bağlantı noktası için önceliklendirilmesini sağlamak için etkili güvenlik grubu kurallarını da gözden geçirebilirsiniz (varsayılan 22). Daha fazla bilgi için [vm trafik akışını gidermek için etkili güvenlik kurallarını kullanma'ya](../../virtual-network/diagnose-network-traffic-filter-problem.md)bakın.

### <a name="check-routing"></a>Yönlendirmeyi kontrol edin

Bir rotanın trafiğin sanal bir makineye veya sanal makineden yönlendirilmesine engel olmadığını doğrulamak için Ağ İzleyicisi'nin [Sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) özelliğini kullanın. Ayrıca, ağ arabirimi için tüm etkili yolları görmek için etkili yolları gözden geçirebilirsiniz. Daha fazla bilgi için [vm trafik akışını gidermek için etkili yolları](../../virtual-network/diagnose-network-routing-problem.md)kullanma'ya bakın.

## <a name="use-the-azure-vm-serial-console"></a>Azure VM Seri Konsolu'nu kullanma
[Azure VM Seri Konsolu,](./serial-console-linux.md) Linux sanal makineleri için metin tabanlı bir konsola erişim sağlar. Konsolu, Etkileşimli bir kabuktaki SSH bağlantınızı gidermek için kullanabilirsiniz. Seri Konsol'u kullanmak için [ön koşulları](./serial-console-linux.md#prerequisites) yerine getirebildiğinizden emin olun ve SSH bağlantınızı daha fazla gidermek için aşağıdaki komutları deneyin.

### <a name="check-that-ssh-is-running"></a>SSH'nin çalıştığını kontrol edin
SSH'nin VM'nizde çalışıp çalışmadığını doğrulamak için aşağıdaki komutu kullanabilirsiniz:

```console
ps -aux | grep ssh
```

Herhangi bir çıktı varsa, SSH çalışır durumdadır.

### <a name="check-which-port-ssh-is-running-on"></a>SSH'nin hangi bağlantı noktasında çalıştığını denetleme

Hangi bağlantı noktası SSH'nin çalıştığını denetlemek için aşağıdaki komutu kullanabilirsiniz:

```console
sudo grep Port /etc/ssh/sshd_config
```

Çıktınız şuna benzer:

```output
Port 22
```

## <a name="use-the-azure-cli"></a>Azure CLI kullanma
Henüz yapmadıysanız, en son [Azure CLI'sini](/cli/azure/install-az-cli2) yükleyin ve [az giriş'i](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açın.

Özel bir Linux disk görüntüsü oluşturduysanız ve yüklediyseniz, [Microsoft Azure Linux Aracısı](../extensions/agent-linux.md) sürümünün 2.0.5 veya sonraki sürümlerinin yüklendiğinden emin olun. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklenmiş ve sizin için yapılandırılmıştır.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmayı sıfırlama
Başlangıçta SSH yapılandırmasını varsayılan değerlere sıfırlamayı ve VM'deki SSH sunucusunu yeniden başlatmayı deneyebilirsiniz. Bu, kullanıcı hesabı adını, parolayı veya SSH anahtarlarını değiştirmez.
Aşağıdaki örnekte [az vm kullanıcı sıfırlama-ssh](/cli/azure/vm/user) vm üzerinde SSH `myVM` yapılandırmasını sıfırlamak için `myResourceGroup`kullanır. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
Aşağıdaki örnekte [az vm kullanıcı](/cli/azure/vm/user) güncelleştirmesi, 'de `myUsername` `myResourceGroup` `myPassword`adı geçen `myVM` VM'de belirtilen değere ilişkin kimlik bilgilerini sıfırlamak için Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

SSH anahtar kimlik doğrulamasını kullanıyorsanız, belirli bir kullanıcı için SSH anahtarını sıfırlayabilirsiniz. Aşağıdaki örnekte **az vm access set-linux-user** adlı `~/.ssh/id_rsa.pub` `myUsername`kullanıcı için saklanan SSH anahtarını `myVM` güncellemek için kullanır , vm adlı `myResourceGroup`. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>VMAccess uzantısını kullanma
Linux için VM Erişim Uzantısı, gerçekleştirecek eylemleri tanımlayan bir json dosyasında okur. Bu eylemler, SSHD'yi sıfırlamayı, Bir SSH anahtarını sıfırlamayı veya kullanıcı eklemeyi içerir. VMAccess uzantısını aramak için Azure CLI'yi kullanmaya devam edebilirsiniz, ancak istenirse json dosyalarını birden çok VM'de yeniden kullanabilirsiniz. Bu yaklaşım, daha sonra verilen senaryolar için çağrılabilir json dosyalarının bir depo oluşturmanıza olanak sağlar.

### <a name="reset-sshd"></a>SSHD'yi Sıfırla
Aşağıdaki içerikle `settings.json` birlikte bir dosya oluşturun:

```json
{
    "reset_ssh":"True"
}
```

Azure CLI'yi kullanarak, `VMAccessForLinux` json dosyanızı belirterek SSHD bağlantınızı sıfırlamak için uzantıyı ararsınız. Aşağıdaki örnekte, 'de `myVM` `myResourceGroup`adı geçen VM'de SSHD'yi sıfırlamak için az [vm uzantısı kümesi](/cli/azure/vm/extension) kullanır. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
SSHD düzgün çalışıyor gibi görünüyorsa, veren bir kullanıcının kimlik bilgilerini sıfırlayabilirsiniz. Bir kullanıcının parolasını sıfırlamak için `settings.json`. Aşağıdaki örnekte belirtilen değer `myUsername` için kimlik `myPassword`bilgilerini sıfırlar. Kendi değerlerinizi kullanarak `settings.json` dosyanıza aşağıdaki satırları girin:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Veya bir kullanıcı için SSH anahtarını sıfırlamak `settings.json`için önce bir dosya oluşturun. Aşağıdaki `myUsername` örnek, `myPassword` `myVM` `myResourceGroup`'de adı geçen VM'de belirtilen değere ilişkin kimlik bilgilerini sıfırlar. Kendi değerlerinizi kullanarak `settings.json` dosyanıza aşağıdaki satırları girin:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

json dosyanızı oluşturduktan sonra, json `VMAccessForLinux` dosyanızı belirterek SSH kullanıcı kimlik bilgilerinizi sıfırlamak için uzantıyı aramak için Azure CLI'yi kullanın. Aşağıdaki örnek, `myVM` `myResourceGroup`'de adı geçen VM'deki kimlik bilgilerini sıfırlar. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-classic-cli"></a>Azure klasik CLI'yi kullanma
Henüz yapmadıysanız, [Azure klasik CLI'sini yükleyin ve Azure aboneliğinize bağlanın.](../../cli-install-nodejs.md) Kaynak Yöneticisi modunu aşağıdaki gibi kullandığınızdan emin olun:

```azurecli
azure config mode arm
```

Özel bir Linux disk görüntüsü oluşturduysanız ve yüklediyseniz, [Microsoft Azure Linux Aracısı](../extensions/agent-linux.md) sürümünün 2.0.5 veya sonraki sürümlerinin yüklendiğinden emin olun. Galeri görüntüleri kullanılarak oluşturulan VM'ler için bu erişim uzantısı zaten yüklenmiş ve sizin için yapılandırılmıştır.

### <a name="reset-ssh-configuration"></a>SSH yapılandırmayı sıfırlama
SSHD yapılandırmasının kendisi yanlış yapılandırılmış veya hizmette bir hatayla karşılaşılabilir. SSH yapılandırmasının geçerli olduğundan emin olmak için SSHD'yi sıfırlayabilirsiniz. SSHD'yi sıfırlamak, ilk adımdır.

Aşağıdaki örnek, SSHD'yi kaynak `myVM` grubunda adı verilen `myResourceGroup`bir VM'de sıfırlar. Kendi VM ve kaynak grup adlarınızı aşağıdaki gibi kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Bir kullanıcının SSH kimlik bilgilerini sıfırlama
SSHD düzgün çalışıyor gibi görünüyorsa, veren bir kullanıcının parolasını sıfırlayabilirsiniz. Aşağıdaki `myUsername` örnek, `myPassword` `myVM` `myResourceGroup`'de adı geçen VM'de belirtilen değere ilişkin kimlik bilgilerini sıfırlar. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

SSH anahtar kimlik doğrulamasını kullanıyorsanız, belirli bir kullanıcı için SSH anahtarını sıfırlayabilirsiniz. Aşağıdaki örnek, 'de adı verilen `~/.ssh/id_rsa.pub` `myUsername` `myVM` VM'de adı verilen kullanıcı `myResourceGroup`için depolanan SSH anahtarını güncelleştirir. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```

## <a name="restart-a-vm"></a><a id="restart-vm" />Bir VM’yi yeniden başlatma
SSH yapılandırmasını ve kullanıcı kimlik bilgilerini sıradını veya bunu yaparken bir hatayla karşılaştıysanız, altta yatan işlem sorunlarını gidermek için VM'yi yeniden başlatmayı deneyebilirsiniz.

### <a name="azure-portal"></a>Azure portalında
Azure portalını kullanarak bir VM'yi yeniden başlatmak için VM'nizi seçin ve ardından aşağıdaki örnekte olduğu gibi **Yeniden Başlat'ı** seçin:

![Azure portalında bir VM'yi yeniden başlatma](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Aşağıdaki örnekte az [vm yeniden başlatımı](/cli/azure/vm) `myVM` adlı `myResourceGroup`kaynak grubunda adlı VM'yi yeniden başlatın. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-classic-cli"></a>Azure klasik CLI

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adı geçen VM'yi yeniden başlatır. Kendi değerlerinizi aşağıdaki gibi kullanın:

```console
azure vm restart --resource-group myResourceGroup --name myVM
```

## <a name="redeploy-a-vm"></a><a id="redeploy-vm" />VM’yi yeniden dağıtma
Bir VM'yi Azure'daki başka bir düğüme yeniden dağıtabilirsiniz ve bu da altta yatan ağ sorunlarını düzeltebilir. VM'yi yeniden dağıtma hakkında bilgi için [bkz.](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

> [!NOTE]
> Bu işlem bittikten sonra, geçici disk verileri kaybolur ve sanal makine ile ilişkili dinamik IP adresleri güncelleştirilir.
>
>

### <a name="azure-portal"></a>Azure portalında
Azure portalını kullanarak bir VM'yi yeniden dağıtmak için VM'nizi seçin ve **Destek + Sorun Giderme** bölümüne gidin. Aşağıdaki örnekte olduğu gibi **Yeniden Dağıt'ı** seçin:

![Azure portalında bir VM'yi yeniden dağıtma](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Aşağıdaki örnek, adlı kaynak grubunda adı geçen `myVM` VM'yi yeniden `myResourceGroup`dağıtmak için [az vm yeniden dağıtıla'yı](/cli/azure/vm) kullanın. Kendi değerlerinizi aşağıdaki gibi kullanın:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-classic-cli"></a>Azure klasik CLI

Aşağıdaki örnek, adlı kaynak grubunda `myVM` adı geçen `myResourceGroup`VM'yi yeniden dağıtır. Kendi değerlerinizi aşağıdaki gibi kullanın:

```console
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan VM'ler

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için en yaygın SSH bağlantı hatalarını gidermek için aşağıdaki adımları deneyin. Her adımdan sonra VM'ye yeniden bağlanmayı deneyin.

* [Azure portalından](https://portal.azure.com)uzaktan erişimi sıfırla. Azure portalında VM'nizi seçin ve **ardından Uzaktan Sıfırla'yı seçin...** seçeneğini belirleyin.
* VM’yi yeniden başlatın. Azure [portalında](https://portal.azure.com)VM'nizi seçin ve **Yeniden Başlat'ı**seçin.

* VM'yi yeni bir Azure düğümüne yeniden dağıtın. VM'yi yeniden dağıtma hakkında bilgi için [bkz.](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

    Bu işlem bittikten sonra, geçici disk verileri kaybolur ve sanal makine ile ilişkili dinamik IP adresleri güncellenir.
* Linux tabanlı [sanal makineler için parola veya SSH sıfırlama yönergelerini](../linux/classic/reset-access-classic.md) izleyin:

  * Parolayı veya SSH tuşunu sıfırla.
  * Bir *sudo* kullanıcı hesabı oluşturun.
  * SSH yapılandırmasını sıfırla.
* Herhangi bir platform sorunu için VM'nin kaynak durumunu kontrol edin.<br>
     VM'nizi seçin ve **Ayarlar** > Durumu Kontrol'ü aşağı**kaydırın.**

## <a name="additional-resources"></a>Ek kaynaklar
* Sonraki adımları takip ettikten sonra VM'nize hala SSH yapamıyorsanız, sorununuzu gidermek için ek adımları gözden geçirmek için [daha ayrıntılı sorun giderme adımlarını](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) görün.
* Sorun giderme uygulama erişimi hakkında daha fazla bilgi için Azure [sanal makinesinde çalışan bir uygulamaya Sorun Giderme erişimi](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Klasik dağıtım modelini kullanarak oluşturulan sorun giderme sanal makineleri hakkında daha fazla bilgi için, [Linux tabanlı sanal makineler için parola veya SSH sıfırlama yı nasıl](../linux/classic/reset-access-classic.md)göreceğiz.
