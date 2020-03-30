---
title: Linux VM’leri için Windows ile SSH anahtarlarını kullanma
description: Azure'daki bir Linux sanal makinesine bağlanmak için Windows bilgisayarında SSH anahtarlarını nasıl oluşturacağınızı ve kullanacağınızı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: e01fb23bbf1720f7d8df9c269373c1b8dc3ec75c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034800"
---
# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Windows ile Azure'da SSH tuşları nasıl kullanılır?

Bu makalede, Azure'da bir Linux sanal makine (VM) oluşturmak ve bunlara bağlanmak için Windows bilgisayarında *güvenli kabuk* (SSH) anahtarları oluşturmanın ve kullanmanın yolları açıklanmaktadır. Bir Linux veya macOS istemcisinden SSH tuşlarını kullanmak için [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) kılavuza bakın.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="windows-packages-and-ssh-clients"></a>Windows paketleri ve SSH istemcileri
Bir *SSH istemcisi*kullanarak Azure'daki Linux VM'lerine bağlanır ve yönetirsiniz. Linux veya macOS çalıştıran bilgisayarlargenellikle SSH anahtarları oluşturmak ve yönetmek ve SSH bağlantıları oluşturmak için Bir Dizi SSH komutuna sahiptir. 

Windows bilgisayarlarda her zaman karşılaştırılabilir SSH komutları yüklü değildir. Windows 10'un son sürümleri, SSH anahtarlarını oluşturmak ve yönetmek ve ssh bağlantılarını bir komut isteminden yapmak için [OpenSSH istemci komutları](https://blogs.msdn.microsoft.com/commandline/2018/03/07/windows10v1803/) sağlar. Son Windows 10 sürümleri, [Linux'un](https://docs.microsoft.com/windows/wsl/about) Bash kabuğu içinde yerel olarak bir SSH istemcisi gibi yardımcı programları çalıştırıp bunlara erişebilmek için Windows Alt Sistemini de içerir. 

Yerel olarak yükleyebileceğiniz diğer yaygın Windows SSH istemcileri aşağıdaki paketlere dahildir:

* [Macun](https://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [Windows için Git](https://git-for-windows.github.io/)
* [MobaXterm](https://mobaxterm.mobatek.net/)
* [Cygwin](https://cygwin.com/)

Azure [Bulut Shell'de](../../cloud-shell/overview.md)Bash'te bulunan SSH yardımcı hizmetlerini de kullanabilirsiniz. 

* Azure portalında [https://shell.azure.com](https://shell.azure.com) veya Azure [portalında](https://portal.azure.com)web tarayıcınızda Bulut Shell'e erişin. 
* [Azure Hesabı uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)yükleyerek Cloud Shell'e Visual Studio Code içinden terminal olarak erişin.

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma
Aşağıdaki bölümlerde Windows'da bir SSH anahtar çifti oluşturmak için iki seçenek açıklayınız. Bir kabuk komutu`ssh-keygen`( ) veya gui aracı (PuTTYgen) kullanabilirsiniz. Ayrıca, bir anahtar oluşturmak için Powershell kullanırken ortak anahtarı ssh.com(SECSH) biçimi olarak yükleyin. CLI kullanırken, yüklemeden önce anahtarı OpenSSH biçimine dönüştürün. 

### <a name="create-ssh-keys-with-ssh-keygen"></a>Ssh-keygen ile SSH tuşları oluşturma

Windows'da SSH istemci araçlarını destekleyen bir komut kabuğu çalıştırıyorsanız (veya Azure Bulut `ssh-keygen` Bulutu kullanıyorsanız), komutu kullanarak bir SSH anahtar çifti oluşturun. Aşağıdaki komutu yazın ve istemleri yanıtlayın. Seçilen konumda bir SSH anahtar çifti varsa, bu dosyalar üzerine yazılır. 

```bash
ssh-keygen -t rsa -b 2048
```

Daha fazla arka plan ve bilgi için, ssh `ssh-keygen`anahtarlarını kullanarak oluşturmak için [hızlı](mac-create-ssh-keys.md) veya [ayrıntılı](create-ssh-keys-detailed.md) adımlara bakın.

### <a name="create-ssh-keys-with-puttygen"></a>PuTTYgen ile SSH tuşları oluşturma

SSH tuşlarını oluşturmak için GUI tabanlı bir araç kullanmayı tercih ederseniz, [PuTTY indirme paketine](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)dahil olan PuTTYgen tuş jeneratörünü kullanabilirsiniz. 

PuTTYgen ile Bir SSH RSA anahtar çifti oluşturmak için:

1. PuTTYgen'i başlatın.

2. **Generate** (Oluştur) düğmesine tıklayın. Varsayılan olarak PuTTYgen 2048 bit SSH-2 RSA tuşu üretir.

4. Anahtar için rasgelelik sağlamak için boş alanda fareyi hareket ettirin.

5. Ortak anahtar oluşturulduktan sonra, isteğe bağlı olarak bir parola girin ve onaylayın. Özel SSH anahtarınızla VM'ye kimlik doğrulaması yaptığınızda parola için istenirsiniz. Parola olmadan, birisi özel anahtarınızı alırsa, bu anahtarı kullanan herhangi bir VM veya hizmette oturum açabilir. Bir parola oluşturmanızı öneririz. Ancak, parolayı unutursanız, bunu kurtarma yolu yoktur.

6. Ortak anahtar pencerenin üst kısmında görüntülenir. Linux VM oluştururken bu ortak anahtarın tamamını kopyalayıp Azure portalına veya Azure Kaynak Yöneticisi şablonuna yapıştırabilirsiniz. Ayrıca, bir kopyasını bilgisayarınıza kaydetmek için **ortak anahtarı kaydet'i** de seçebilirsiniz:

    ![PuTTY ortak anahtar dosyanı kaydetme](./media/ssh-from-windows/save-public-key.png)

7. İsteğe bağlı olarak, özel anahtarı PuTTy özel anahtar biçiminde (.ppk dosyası) kaydetmek **için, özel anahtarı kaydet'i**seçin. VM'ye SSH bağlantısı yapmak için Daha sonra .ppk dosyasının PuTTY'yi kullanması gerekir.

    ![PuTTY özel anahtar dosyalarını kaydet](./media/ssh-from-windows/save-ppk-file.png)

    Özel anahtarı openssh biçiminde kaydetmek istiyorsanız, birçok SSH istemcisi tarafından kullanılan özel anahtar biçimi, **Dönüşümler** > **Dışa Aktar OpenSSH anahtarını**seçin.

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>VM dağıtırken Bir SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarlarını kullanan bir Linux VM oluşturmak için, Azure portalını veya diğer yöntemleri kullanarak VM'yi oluştururken SSH ortak anahtarınızı sağlayın.

Aşağıdaki örnek, bir Linux VM oluştururken bu ortak anahtarı Azure portalına nasıl kopyalayıp yapıştıracağınızı gösterir. Ortak anahtar genellikle yeni VM~.ssh/authorized_key dizininde depolanır.

   ![Azure portalında bir VM oluştururken ortak anahtarı kullanın](./media/ssh-from-windows/use-public-key-azure-portal.png)


## <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

Windows'dan Linux VM'nize SSH bağlantısı yapmanın bir yolu, bir SSH istemcisi kullanmaktır. Windows sisteminizde yüklü bir SSH istemciniz varsa veya Azure Bulut Su şurasında Bash'teki SSH araçlarını kullanıyorsanız, tercih edilen yöntem budur. GUI tabanlı bir araç tercih ederseniz, PuTTY ile bağlayabilirsiniz.  

### <a name="use-an-ssh-client"></a>Bir SSH istemcisi kullanma
Azure VM'nizde genel anahtar ve yerel sisteminizdeki özel anahtarla, VM'nizin IP adresini veya DNS adını kullanarak VM'nize SSH. *Azureuser* ve *myvm.westus.cloudapp.azure.com* aşağıdaki komutu yönetici kullanıcı adı ve tam nitelikli alan adı (veya IP adresi) ile değiştirin:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluşturduğunuzda bir parola yapılandırıldıysanız, oturum açma işlemi sırasında istendiğinde parolayı girin.

VM tam zamanında erişim ilkesini kullanıyorsa, VM'ye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında ilke hakkında daha fazla bilgi için, [tam zamanında ilkeyi kullanarak sanal makine erişimini yönet'e](../../security-center/security-center-just-in-time.md)bakın.

### <a name="connect-with-putty"></a>PuTTY ile bağlantı kurun

[PuTTY indirme paketini](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) yüklediyseniz ve daha önce bir PuTTY özel anahtar (.ppk) dosyası oluşturduysanız, PuTTY ile bir Linux VM'ye bağlanabilirsiniz.

1. Putty'yi başlat.

2. Azure portalından VM'nizin ana bilgisayar adını veya IP adresini girin:

    ![Yeni PuTTY bağlantısını açın](./media/ssh-from-windows/putty-new-connection.png)

3. **Bağlantı** > **SSH** > **Auth** kategorisini seçin. PuTTY özel anahtarınıza (.ppk dosyası) göz atın ve seçin:

    ![Kimlik doğrulaması için PuTTY özel anahtarınızı seçin](./media/ssh-from-windows/putty-auth-dialog.png)

4. VM'nize bağlanmak için **Aç'ı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Ayrıntılı adımlar, seçenekler ve SSH anahtarlarıyla çalışmanın gelişmiş örnekleri için, [SSH anahtar çiftleri oluşturmak için ayrıntılı adımlara](create-ssh-keys-detailed.md)bakın.

* PowerShell'i Azure Cloud Shell'de SSH anahtarları oluşturmak ve Linux VM'lerine SSH bağlantıları yapmak için de kullanabilirsiniz. [PowerShell quickstart](../../cloud-shell/quickstart-powershell.md#ssh)bakın.

* Linux VM'lerinize bağlanmak için SSH'yi kullanmakta güçlük çekiyorsanız, [Bir Azure Linux VM'ine bağlanan Sorun Giderme SSH bağlantılarına](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.
