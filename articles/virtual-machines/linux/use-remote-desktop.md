---
title: Azure'da Linux VM için Uzak Masaüstü'nü Kullanma
description: Grafik araçları kullanarak Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü (xrdp) nasıl yükleyip yapılandırılamayı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 09/12/2019
ms.author: cynthn
ms.openlocfilehash: 2b1b708618c60153b8dbce69b26d832fa18b25aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476612"
---
# <a name="install-and-configure-remote-desktop-to-connect-to-a-linux-vm-in-azure"></a>Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü yükleme ve yapılandırma
Azure'daki Linux sanal makineleri (VM'ler) genellikle komut satırından güvenli bir kabuk (SSH) bağlantısı kullanılarak yönetilir. Linux'ta yeni olduğunda veya hızlı sorun giderme senaryoları için uzak masaüstünün kullanımı daha kolay olabilir. Bu makalede, Kaynak Yöneticisi dağıtım modelini kullanarak Linux VM'niz için bir masaüstü ortamının[(xfce)](https://www.xfce.org)ve uzak masaüstünün[(xrdp)](https://www.xrdp.org)nasıl yüklenir ve yapılandırılabildiğini ayrıntıları.


## <a name="prerequisites"></a>Ön koşullar
Bu makale, Azure'da varolan bir Ubuntu 18.04 LTS VM gerektirir. VM oluşturmanız gerekiyorsa, aşağıdaki yöntemlerden birini kullanın:

- [Azure CLI](quick-create-cli.md)
- [Azure portalı](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Linux VM'nize bir masaüstü ortamı yükleme
Azure'daki çoğu Linux VM'de varsayılan olarak yüklü bir masaüstü ortamı yoktur. Linux VM'leri genellikle masaüstü ortamı yerine SSH bağlantıları kullanılarak yönetilir. Linux'ta seçebileceğiniz çeşitli masaüstü ortamları vardır. Masaüstü ortamı seçtiğinize bağlı olarak, 1 ila 2 GB disk alanı tüketebilir ve gerekli tüm paketleri yüklemek ve yapılandırmak 5 ila 10 dakika sürebilir.

Aşağıdaki örnek, hafif [xfce4](https://www.xfce.org/) masaüstü ortamını bir Ubuntu 18.04 LTS VM'ye yükler. Diğer dağıtımlar için komutlar biraz `yum` değişir (Red Hat Enterprise Linux'u yüklemek ve uygun `selinux` kuralları yapılandırmak veya örneğin SUSE'yi yüklemek için kullanmak). `zypper`

İlk olarak, SSH VM için. Aşağıdaki örnek, *azureuser*kullanıcı adı ile *myvm.westus.cloudapp.azure.com* adlı VM bağlanır. Kendi değerlerinizi kullanın:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Windows kullanıyorsanız ve SSH kullanma hakkında daha fazla bilgiye ihtiyacınız varsa, [Windows ile SSH tuşlarının nasıl kullanılacağına](ssh-from-windows.md)bakın.

Sonra, aşağıdaki gibi `apt` kullanarak xfce yükleyin:

```bash
sudo apt-get update
sudo apt-get -y install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>Uzak bir masaüstü sunucusu yükleme ve yapılandırma
Artık yüklü bir masaüstü ortamınız olduğuna göre, gelen bağlantıları dinlemek için uzak bir masaüstü hizmetini yapılandırın. [xrdp,](http://xrdp.org) çoğu Linux dağıtımında bulunan ve xfce ile iyi çalışan bir açık kaynak uzak masaüstü protokolü (RDP) sunucusudur. Ubuntu VM'nize xrdp'yi aşağıdaki şekilde yükleyin:

```bash
sudo apt-get -y install xrdp
sudo systemctl enable xrdp
```

Xrdp'ye oturumunuza başladığınızda hangi masaüstü ortamını kullanacağınızı söyleyin. Xrdp'yi masaüstü ortamınız olarak kullanmak üzere yapılandırın:

```bash
echo xfce4-session >~/.xsession
```

Değişikliklerin aşağıdaki gibi etkili olması için xrdp hizmetini yeniden başlatın:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Yerel kullanıcı hesabı parolası ayarlama
VM'nizi oluşturduğunuzda kullanıcı hesabınız için bir parola oluşturduysanız, bu adımı atlayın. Yalnızca SSH anahtar kimlik doğrulamasını kullanıyorsanız ve yerel hesap parolası seti yoksa, VM'nizde oturum açmak için xrdp'yi kullanmadan önce bir parola belirtin. xrdp kimlik doğrulama için SSH anahtarlarını kabul edemez. Aşağıdaki örnekte, *azureuser*kullanıcı için bir parola belirtir:

```bash
sudo passwd azureuser
```

> [!NOTE]
> Parola belirtme, şu anda parola girişine izin vermek için SSHD yapılandırmanızı güncelleştirmez. Güvenlik açısından bakıldığında, anahtar tabanlı kimlik doğrulaması kullanarak VM'nize bir SSH tüneli ile bağlanmak ve ardından xrdp'ye bağlanmak isteyebilirsiniz. Bu nedenle, uzak masaüstü trafiğine izin vermek için ağ güvenliği grubu kuralı nı oluşturma yla ilgili aşağıdaki adımı atlayın.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>Uzak Masaüstü trafiği için Ağ Güvenlik Grubu kuralı oluşturma
Uzak Masaüstü trafiğinin Linux VM'nize ulaşmasına izin vermek için, 3389 bağlantı noktasındaki TCP'nin VM'nize erişmesine olanak tanıyan bir ağ güvenliği grubu kuralının oluşturulması gerekir. Ağ güvenliği grubu kuralları hakkında daha fazla bilgi için ağ [güvenlik grubu nedir?](../../virtual-network/security-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [Ağ güvenliği grubu kuralı nı oluşturmak için Azure portalını da kullanabilirsiniz.](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Aşağıdaki örnek, *3389*portundaki [az vm açık bağlantı noktası](/cli/azure/vm#az-vm-open-port) ile bir ağ güvenlik grubu kuralı oluşturur. SSH oturumundan VM'nize değil, Azure CLI'den aşağıdaki ağ güvenlik grubu kuralını açın:

```azurecli
az vm open-port --resource-group myResourceGroup --name myVM --port 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>Linux VM'nizi Uzak Masaüstü istemcisiyle bağlayın
Yerel uzak masaüstü istemcinizi açın ve Linux VM'nizin IP adresine veya DNS adına bağlanın. VM'nize kullanıcı hesabının kullanıcı adını ve parolasını aşağıdaki şekilde girin:

![Uzak Masaüstü istemcinizi kullanarak xrdp'ye bağlanın](./media/use-remote-desktop/remote-desktop-client.png)

Kimlik doğrulaması yaptıktan sonra, xfce masaüstü ortamı yüklenir ve aşağıdaki örneğe benzer:

![xrdp ile xfce masaüstü ortamı](./media/use-remote-desktop/xfce-desktop-environment.png)

Yerel RDP istemciniz ağ düzeyinde kimlik doğrulaması (NLA) kullanıyorsa, bu bağlantı ayarını devre dışı bırakmanız gerekebilir. XRDP şu anda NLA'yı desteklemiyor. [Ayrıca FreeRDP](https://www.freerdp.com)gibi NLA'yı destekleyen alternatif RDP çözümlerine de bakabilirsiniz.


## <a name="troubleshoot"></a>Sorun giderme
Uzak Bir Masaüstü istemcisi kullanarak Linux VM'nize bağlanamıyorsanız, VM'nizin RDP bağlantılarını aşağıdaki gibi dinlediğini doğrulamak için Linux VM'nizi kullanın: `netstat`

```bash
sudo netstat -plnt | grep rdp
```

Aşağıdaki örnekte, Beklendiği gibi TCP bağlantı noktası 3389'da VM dinleme si gösterilmektedir:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

*Xrdp-sesman* hizmeti dinlemiyorsa, Ubuntu VM'de hizmeti aşağıdaki gibi yeniden başlatın:

```bash
sudo service xrdp restart
```

Hizmetin neden yanıt verilemeyeceğini gösteren göstergeler için Ubuntu VM'nizdeki */var/log* oturumunu gözden geçirin. Ayrıca herhangi bir hata görüntülemek için uzak bir masaüstü bağlantı girişimi sırasında syslog izleyebilirsiniz:

```bash
tail -f /var/log/syslog
```

Red Hat Enterprise Linux ve SUSE gibi diğer Linux dağıtımlarının, hizmetleri yeniden başlatmanın ve gözden geçirilebilecek alternatif günlük dosya konumları için farklı yolları olabilir.

Uzak masaüstü istemcinizde herhangi bir yanıt almazsanız ve sistem günlüğünde herhangi bir olay görmezseniz, bu davranış uzak masaüstü trafiğinin VM'ye erişemeyeceğini gösterir. 3389 bağlantı noktasında TCP'ye izin verme kuralınız olduğundan emin olmak için ağ güvenlik grubu kurallarınızı gözden geçirin. Daha fazla bilgi için [Sorun Giderme uygulaması bağlantısı sorunlarına](../windows/troubleshoot-app-connection.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Linux VM'lerle SSH tuşları oluşturma ve kullanma hakkında daha fazla bilgi için [Azure'daki Linux VM'leri için SSH anahtarları oluşturma 'na](mac-create-ssh-keys.md)bakın.

Windows'dan SSH kullanma hakkında daha fazla bilgi için [Windows ile SSH tuşlarının nasıl kullanılacağına](ssh-from-windows.md)bakın.

