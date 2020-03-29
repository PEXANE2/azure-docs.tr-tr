---
title: Azure'da Linux VM'leri için bir SSH anahtar çifti oluşturma ve kullanma
description: Kimlik doğrulama işleminin güvenliğini artırmak için Azure'daki Linux VM'leri için bir SSH genel-özel anahtar çifti oluşturma ve kullanma.
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 12/06/2019
ms.author: cynthn
ms.openlocfilehash: af18a32143ebc9db7be923b09de106b79022321f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969051"
---
# <a name="quick-steps-create-and-use-an-ssh-public-private-key-pair-for-linux-vms-in-azure"></a>Hızlı adımlar: Azure'da Linux VM'leri için bir SSH genel özel anahtar çifti oluşturun ve kullanın

Güvenli bir kabuk (SSH) anahtar çiftiyle, Azure'da kimlik doğrulama için SSH anahtarlarını kullanan sanal makineler (VM'ler) oluşturarak oturum açma gereksinimini ortadan kaldırabilirsiniz. Bu makalede, Linux VM'ler için bir SSH ortak-özel anahtar dosya çiftinin nasıl hızla oluşturacağınızı ve kullanacağınızı gösterilmektedir. Bu adımları Azure Cloud Shell, macOS veya Linux ana bilgisayar, Linux için Windows Alt Sistemi ve OpenSSH'i destekleyen diğer araçlarla tamamlayabilirsiniz. 

> [!NOTE]
> SSH anahtarları kullanılarak oluşturulan VM'ler varsayılan olarak parolalar devre dışı bırakılmış olarak yapılandırılır ve bu da kaba kuvvet tahmin saldırılarının zorluğunu büyük ölçüde artırır. 

Daha fazla arka plan ve örnek için, [SSH anahtar çiftleri oluşturmak için ayrıntılı adımlara](create-ssh-keys-detailed.md)bakın.

Windows bilgisayarında SSH tuşları oluşturmanın ve kullanmanın ek yolları için, [Windows'da Windows'da SSH tuşlarının nasıl kullanılacağına](ssh-from-windows.md)bakın.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

SSH `ssh-keygen` ortak ve özel anahtar dosyaları oluşturmak için komutu kullanın. Varsayılan olarak, bu dosyalar ~/.ssh dizininde oluşturulur. Özel anahtar dosyasına erişmek için farklı bir konum ve isteğe bağlı bir parola *(parola)* belirtebilirsiniz. Verilen konumda aynı ada sahip bir SSH anahtar çifti varsa, bu dosyalar üzerine yazılır.

Aşağıdaki komut, RSA şifrelemesi ve 4096 bit uzunluğunda bir SSH anahtar çifti oluşturur:

```bash
ssh-keygen -m PEM -t rsa -b 4096
```

Az [vm oluşturma](/cli/azure/vm#az-vm-create) komutuyla VM'inizi oluşturmak için Azure `--generate-ssh-keys` [CLI'yi](/cli/azure) kullanıyorsanız, isteğe bağlı olarak bu seçeneği kullanarak SSH ortak ve özel anahtar dosyaları oluşturabilirsiniz. Anahtar `--ssh-dest-key-path` dosyaları, seçenekte aksi belirtilmedikçe ~/.ssh dizininde saklanır. Seçenek `--generate-ssh-keys` varolan anahtar dosyalarının üzerine yazmayacak, bunun yerine bir hata döndürecek. Aşağıdaki komutta, *VMname* ve *RGname'yi* kendi değerlerinizle değiştirin:

```azurecli
az vm create --name VMname --resource-group RGname --generate-ssh-keys 
```

## <a name="provide-an-ssh-public-key-when-deploying-a-vm"></a>VM dağıtırken Bir SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarlarını kullanan bir Linux VM oluşturmak için, Azure portalı, Azure CLI, Azure Kaynak Yöneticisi şablonları veya diğer yöntemleri kullanarak VM'yi oluştururken SSH ortak anahtarınızı belirtin:

* [Azure portal ile Linux sanal makinesi oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makinesi oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bir Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Bir SSH ortak anahtarının biçimini bilmiyorsanız, gerekirse kendi ortak anahtar dosyanızın yolunu `~/.ssh/id_rsa.pub` ve dosya adını değiştirerek ortak anahtarınızı aşağıdaki `cat` komutla görüntüleyebilirsiniz:

```bash
cat ~/.ssh/id_rsa.pub
```

Tipik bir ortak anahtar değeri şu örnekte görünür:

```
ssh-rsa AAAAB3NzaC1yc2EAABADAQABAAACAQC1/KanayNr+Q7ogR5mKnGpKWRBQU7F3Jjhn7utdf7Z2iUFykaYx+MInSnT3XdnBRS8KhC0IP8ptbngIaNOWd6zM8hB6UrcRTlTpwk/SuGMw1Vb40xlEFphBkVEUgBolOoANIEXriAMvlDMZsgvnMFiQ12tD/u14cxy1WNEMAftey/vX3Fgp2vEq4zHXEliY/sFZLJUJzcRUI0MOfHXAuCjg/qyqqbIuTDFyfg8k0JTtyGFEMQhbXKcuP2yGx1uw0ice62LRzr8w0mszftXyMik1PnshRXbmE2xgINYg5xo/ra3mq2imwtOKJpfdtFoMiKhJmSNHBSkK7vFTeYgg0v2cQ2+vL38lcIFX4Oh+QCzvNF/AXoDVlQtVtSqfQxRVG79Zqio5p12gHFktlfV7reCBvVIhyxc2LlYUkrq4DHzkxNY5c9OGSHXSle9YsO3F1J5ip18f6gPq4xFmo6dVoJodZm9N0YMKCkZ4k1qJDESsJBk2ujDPmQQeMjJX3FnDXYYB182ZCGQzXfzlPDC29cWVgDZEXNHuYrOLmJTmYtLZ4WkdUhLLlt5XsdoKWqlWpbegyYtGZgeZNRtOOdN6ybOPJqmYFd2qRtb4sYPniGJDOGhx4VodXAjT09omhQJpE6wlZbRWDvKC55R2d/CSPHJscEiuudb+1SG2uA/oik/WQ== username@domainname
```

Azure portalında veya Kaynak Yöneticisi şablonunda kullanmak üzere ortak anahtar dosyasının içeriğini kopyalayıp yapıştırıyorsanız, sondaki beyaz alanı kopyalamadığınızdan emin olun. macOS'ta ortak bir anahtarı kopyalamak için, `pbcopy`ortak anahtar dosyasını ' ya ' ya aktarabilirsiniz. Benzer şekilde Linux'ta, ortak anahtar dosyasını `xclip`.

Azure'da Linux VM'nize yerleştirdiğiniz ortak anahtar, anahtar çiftini oluşturduğunuzda farklı bir konum belirtmediğiniz sürece varsayılan olarak ~/.ssh/id_rsa.pub'da depolanır. Varolan bir ortak anahtarla VM'nizi oluşturmak için [Azure CLI 2.0'ı](/cli/azure) kullanmak için, [az vm](/cli/azure/vm#az-vm-create) create `--ssh-key-values` komutunu kullanarak bu ortak anahtarın değerini ve isteğe bağlı olarak konumunu seçeneğiyle belirtin. Aşağıdaki komutta, *VMname,* *RGname*ve *keyFile'ı* kendi değerlerinizle değiştirin:

```azurecli
az vm create --name VMname --resource-group RGname --ssh-key-values mysshkey.pub
```

VM'nizle birden çok SSH tuşu kullanmak istiyorsanız, bunları bu gibi `--ssh-key-values sshkey-desktop.pub sshkey-laptop.pub`boşluk ayrılmış bir listeye girebilirsiniz.


## <a name="ssh-into-your-vm"></a>VM’ye SSH uygulama

Azure VM'nizde genel anahtar ve yerel sisteminizdeki özel anahtarla, VM'nizin IP adresini veya DNS adını kullanarak VM'nize SSH girer. Aşağıdaki komutta *azureuser* ve *myvm.westus.cloudapp.azure.com* yönetici kullanıcı adı ve tam nitelikli alan adı (veya IP adresi) ile değiştirin:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluşturduğunuzda bir parola belirttiyseniz, oturum açma işlemi sırasında istendiğinde bu parolayı girin. VM ~/.ssh/known_hosts dosyanıza eklenir ve Azure VM'nizdeki ortak anahtar veya sunucu adı ~/.ssh/known_hosts'den kaldırılınceye kadar yeniden bağlanmanız istenmez.

VM tam zamanında erişim ilkesini kullanıyorsa, VM'ye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında ilke hakkında daha fazla bilgi için, [tam zamanında ilkeyi kullanarak sanal makine erişimini yönet'e](../../security-center/security-center-just-in-time.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* SSH anahtar çiftleri ile çalışma hakkında daha fazla bilgi için, [SSH anahtar çiftleri oluşturmak ve yönetmek için ayrıntılı adımlar](create-ssh-keys-detailed.md)bakın.

* Azure VM'lere SSH bağlantılarında sorun yaşıyorsanız, [Azure Linux VM'ye yapılan Sorun Giderme SSH bağlantılarına](troubleshoot-ssh-connection.md)bakın.
