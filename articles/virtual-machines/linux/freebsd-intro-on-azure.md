---
title: Azure üzerinde FreeBSD’ye giriş
description: Azure'da FreeBSD sanal makineleri kullanma hakkında bilgi edinin
author: thomas1206
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: mimckitt
ms.openlocfilehash: 0825a29b45ea701315a57ff5248731e64e29de32
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261644"
---
# <a name="introduction-to-freebsd-on-azure"></a>Azure üzerinde FreeBSD’ye giriş
Bu makalede, Azure'da bir FreeBSD sanal makine çalıştırmagenel bir bakış sağlar.

## <a name="overview"></a>Genel Bakış
Microsoft Azure için FreeBSD, modern sunuculara, masaüstü bilgisayarlara ve gömülü platformlara güç sağlamak için kullanılan gelişmiş bir bilgisayar işletim sistemidir.

Microsoft Corporation, [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) önceden yapılandırılmış ile Azure'da FreeBSD'nin görüntülerini kullanıma sunmaktadır. Şu anda, aşağıdaki FreeBSD sürümleri Microsoft tarafından görüntü olarak sunulmaktadır:

- [Azure Marketi'nde FreeBSD 10.4](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD104)
- [Azure Marketi'nde FreeBSD 11.2](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD112)
- [Azure Marketi'nde FreeBSD 12.0](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD120)

Aracı, FreeBSD VM ile Azure kumaşı arasındaki iletişimden, VM'yi ilk kullanımda sağlama (kullanıcı adı, parola veya SSH anahtarı, ana bilgisayar adı, vb.) ve seçici VM uzantıları için işlevselliği etkinleştirme gibi işlemler için sorumludur.

FreeBSD'nin gelecekteki sürümlerine gelince, strateji güncel kalmak ve freebsd sürüm mühendislik ekibi tarafından yayımlandıktan kısa bir süre sonra en son sürümleri kullanılabilir hale getirmektir.

### <a name="create-a-freebsd-vm-through-azure-cli-on-freebsd"></a>FreeBSD'de Azure CLI ile FreeBSD VM oluşturma
Öncelikle Bir FreeBSD makinede komutu takip rağmen [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) yüklemeniz gerekir.

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

FreeBSD makinenize bash yüklenmediyse, yüklemeden önce aşağıdaki komutu çalıştırın. 

```bash
sudo pkg install bash
```

FreeBSD makinenize python yüklenmediyse, yüklemeden önce aşağıdaki komutları çalıştırın. 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Yükleme sırasında, size `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`istenir. Eğer cevap `y` ve `/etc/rc.conf` `a path to an rc file to update`olarak girerseniz, `ERROR: [Errno 13] Permission denied`sorun la karşılaşabilirsiniz. Bu sorunu gidermek için, dosyaya `etc/rc.conf`karşı geçerli kullanıcıya yazma hakkı vermelisiniz.

Artık Azure'da oturum açabilir ve FreeBSD VM'nizi oluşturabilirsiniz. Aşağıda bir FreeBSD 11.0 VM oluşturmak için bir örnektir. Ayrıca, yeni oluşturulan `--public-ip-address-dns-name` Public IP için genel olarak benzersiz bir DNS adı ile parametre ekleyebilirsiniz. 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

Daha sonra, yukarıdaki dağıtım çıktısında yazdırılan ip adresi aracılığıyla FreeBSD VM'nizde oturum açabilirsiniz. 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>FreeBSD için VM uzantıları
FreeBSD'de vm uzantıları aşağıda veda edilmiştir.

### <a name="vmaccess"></a>VMAccess
[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) uzantısı şunları yapabilir:

* Orijinal sudo kullanıcısının parolasını sıfırla.
* Belirtilen parolayla yeni bir sudo kullanıcısı oluşturun.
* Verilen anahtarla ortak ana bilgisayar anahtarını ayarlayın.
* Ana bilgisayar anahtarı sağlanmazsa VM sağlama sırasında sağlanan ortak ana bilgisayar anahtarını sıfırla.
* SSH bağlantı noktasını açın (22) ve reset_ssh doğru ayarlanmışsa sshd_config geri yükleyin.
* Mevcut kullanıcıyı kaldırın.
* Diskleri kontrol edin.
* Eklenen bir diski onarın.

### <a name="customscript"></a>Özel Senaryo
[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) uzantısı şunları yapabilir:

* Sağlanırsa, özelleştirilmiş komut dosyalarını Azure Depolama'dan veya harici genel depolamadan (örneğin, GitHub) indirin.
* Giriş noktası komut dosyasını çalıştırın.
* Satır satırkomutlarını destekleyin.
* Kabuk ve Python komut dosyalarında Windows stili newline'ı otomatik olarak dönüştürün.
* Kabuk ve Python komut dosyalarındaki BOM'u otomatik olarak kaldırın.
* CommandToExecute'da hassas verileri koruyun.

> [!NOTE]
> FreeBSD VM artık yalnızca CustomScript sürüm 1.x'i destekler.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Kimlik doğrulama: kullanıcı adları, parolalar ve SSH tuşları
Azure portalını kullanarak bir FreeBSD sanal makinesi oluştururken, bir kullanıcı adı, parola veya SSH ortak anahtarı sağlamanız gerekir.
Azure'da FreeBSD sanal makine dağıtmak için kullanıcı adları, sanal makinede bulunan sistem hesaplarının adlarıyla (UID <100) eşleşmemelidir (örneğin, kök).
Şu anda yalnızca RSA SSH anahtarı desteklenir. Çok satırlı SSH tuşu `---- BEGIN SSH2 PUBLIC KEY ----` ile `---- END SSH2 PUBLIC KEY ----`başlamalı ve onunla bitmelidir.

## <a name="obtaining-superuser-privileges"></a>Süper kullanıcı ayrıcalıkları edinme
Azure'da sanal makine örneği dağıtımı sırasında belirtilen kullanıcı hesabı ayrıcalıklı bir hesaptır. Sudo paketi yayınlanan FreeBSD görüntü yüklendi.
Bu kullanıcı hesabı üzerinden oturum açtıktan sonra, komut sözdizimini kullanarak komutları root olarak çalıştırabilirsiniz.

```
$ sudo <COMMAND>
```

İsteğe bağlı olarak bir kök `sudo -s`kabuğu nu kullanarak elde edebilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar
[Azure VM Konuk Aracısı](https://github.com/Azure/WALinuxAgent/) sürüm 2.2.2'de Azure'da FreeBSD VM için sağlama hatasına neden olan bilinen bir [sorun](https://github.com/Azure/WALinuxAgent/pull/517) vardır. Düzeltme, Azure [VM Guest Agent](https://github.com/Azure/WALinuxAgent/) sürüm 2.2.3 ve daha sonraki sürümler tarafından yakalandı. 

## <a name="next-steps"></a>Sonraki adımlar
* FreeBSD VM oluşturmak için [Azure Marketi'ne](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeBSD112) gidin.
