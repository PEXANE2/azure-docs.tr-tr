---
title: SCP ile Azure Linux VM'lerine dosya taşıma
description: SCP ve SSH anahtar çiftini kullanarak dosyaları Azure'daki bir Linux VM'ye güvenli bir şekilde taşıyın.
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.topic: article
ms.date: 07/12/2017
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: a0837790b70de42073338bf085ee0f3976b866f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969615"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>SCP kullanarak dosyaları Linux VM'ye taşıma

Bu makalede, Güvenli Kopya (SCP) kullanarak dosyaları iş istasyonunuzdan Azure Linux VM'sine veya bir Azure Linux VM'den iş istasyonunuzun nasıI taşıyabilirsiniz. Dosyaları iş istasyonunuzun ve Linux VM'iniz arasında hızlı ve güvenli bir şekilde taşımak Azure altyapınızı yönetmek için çok önemlidir. 

Bu makale için, [SSH ortak ve özel anahtar dosyalarını](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)kullanarak Azure'da dağıtılan bir Linux VM'ye ihtiyacınız vardır. Ayrıca yerel bilgisayarınız için bir SCP istemcisi gerekir. SSH üzerine inşa edilmiş ve çoğu Linux ve Mac bilgisayarve bazı Windows kabukları varsayılan Bash kabuk dahildir.

## <a name="quick-commands"></a>Hızlı komutlar

Bir dosyayı Linux VM'ye kadar kopyalama

```bash
scp file azureuser@azurehost:directory/targetfile
```

Linux VM'den bir dosyayı aşağı kopyalama

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Örnek olarak, bir Azure yapılandırma dosyasını Bir Linux VM'sine taşır ve hem SCP hem de SSH tuşlarını kullanarak bir günlük dosyası dizinini aşağı çekeriz.   

## <a name="ssh-key-pair-authentication"></a>SSH anahtar çifti kimlik doğrulaması

SCP aktarım katmanı için SSH kullanır. SSH, hedef ana bilgisayardaki kimlik doğrulamasını işler ve dosyayı Varsayılan olarak SSH ile sağlanan şifreli bir tünelde taşır. SSH kimlik doğrulaması için kullanıcı adları ve parolalar kullanılabilir. Ancak, SSH genel ve özel anahtar kimlik doğrulaması bir güvenlik en iyi uygulama olarak önerilir. SSH bağlantıyı doğruladıktan sonra, SCP dosyayı kopyalamaya başlar. Düzgün yapılandırılmış `~/.ssh/config` ve SSH ortak ve özel anahtarları kullanılarak, SCP bağlantısı yalnızca bir sunucu adı (veya IP adresi) kullanılarak kurulabilir. Yalnızca bir SSH anahtarınız varsa, SCP `~/.ssh/` dizinde arar ve VM'de oturum açmak için varsayılan olarak kullanır.

SSH ortak ve `~/.ssh/config` özel anahtarlarınızı yapılandırma hakkında daha fazla bilgi için Bkz. [SSH anahtarları oluştur.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="scp-a-file-to-a-linux-vm"></a>Bir Linux VM'ye bir dosya scp

İlk örnekiçin, bir Azure yapılandırma dosyasını otomasyon dağıtmak için kullanılan bir Linux VM'sine kopyalarız. Bu dosya, sırları içeren Azure API kimlik bilgilerini içerdiğinden, güvenlik önemlidir. SSH tarafından sağlanan şifreli tünel dosyanın içeriğini korur.

Aşağıdaki komut yerel *.azure/config* dosyasını FQDN *myserver.eastus.cloudapp.azure.com*olan bir Azure VM'sine kopyalar. Azure VM'deki yönetici kullanıcı adı *azureuser'dir.* Dosya */home/azureuser/* dizinine yöneliktir. Bu komutta kendi değerlerinizi değiştirin.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>Linux VM'den SCP dizini

Bu örnekte, Linux VM'den iş istasyonunuza kadar günlük dosyalarının bir dizinini kopyalarız. Bir günlük dosyası hassas veya gizli veriler içerebilir veya içermeyebilir. Ancak, SCP kullanarak günlük dosyalarının içeriğini şifrelenir sağlar. Dosyaları aktarmak için SCP'yi kullanmak, günlük dizini ve dosyaları güvenli yken iş istasyonunuzun altına aktarmanın en kolay yoludur.

Aşağıdaki komut, Azure VM'deki */home/azureuser/logs/* dizinindeki dosyaları yerel /tmp dizinine kopyalar:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

Bayrak, `-r` SCP'ye komutta listelenen dizin noktasından dosyaları ve dizinleri özyinelemeli olarak kopyalamasını bildirir.  Ayrıca komut satırı sözdizimi bir `cp` kopya komutu benzer olduğunu unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

* [VMAccess Uzantısını kullanarak Azure Linux VM'lerinde kullanıcıları, SSH'yi ve diskleri kontrol edin veya onarın](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
