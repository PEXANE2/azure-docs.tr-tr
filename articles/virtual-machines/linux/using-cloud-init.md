---
title: Azure 'da Linux VM 'Leri için Cloud-init desteğine genel bakış
description: Azure 'da sağlama sırasında VM yapılandırma için Cloud-init özelliklerine genel bakış.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 01/23/2019
ms.author: danis
ms.openlocfilehash: 0309d9a794a978c736ffc4689c46565ee8fb5b00
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78226700"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Azure 'da sanal makineler için Cloud-init desteği
Bu makalede, Azure 'da sağlama sırasında bir sanal makineyi (VM) veya sanal makine ölçek kümelerini yapılandırmak üzere [Cloud-init](https://cloudinit.readthedocs.io) için mevcut destek açıklanmaktadır. Bu Cloud-init yapılandırması, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalıştırılır.  

VM sağlama, Azure 'un konak adı, Kullanıcı adı, parola vb. gibi sanal makine oluşturma parametre değerlerini geçi, ve önyükleme yaptığı sürece VM için kullanılabilir hale getirme işlemidir. ' Sağlama Aracısı ' bu değerleri kullanır, VM 'yi yapılandırır ve tamamlandığında rapor gönderir. 

Azure, [bulut-init](https://cloudinit.readthedocs.io)ve [Azure Linux Aracısı (wala)](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)olmak üzere iki sağlama aracısını destekler.

## <a name="cloud-init-overview"></a>Cloud-init genel bakış
[Cloud-init](https://cloudinit.readthedocs.io) , Linux VM 'yi ilk kez önyüklediğinde bir Linux sanal makinesini özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Cloud-init önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanıza uygulayabileceğiniz gerekli aracı yoktur.  `#cloud-config` dosyalarınızı veya diğer girdileri doğru şekilde biçimlendirme hakkında daha fazla bilgi için bkz. [Cloud-init belge sitesi](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` dosyalar, Base64 olarak kodlanmış metin dosyalarıdır.

Cloud-init Ayrıca dağıtımlar arasında da çalışmaktadır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz ayırıcı için yerel paket yönetim aracını otomatik olarak kullanır.

Etkin olarak desteklenen Linux distro ortaklarımızla birlikte kullanılabilir cloud-init etkinleştirilmiş görüntüleri Azure Market'te sahip olmak için çalışıyoruz. Bu görüntüler, Cloud-init dağıtımlarınızın ve yapılandırmalarının VM 'Ler ve sanal makine ölçek kümeleri ile sorunsuz bir şekilde çalışmasını sağlayacak. Başlangıçta, Azure üzerinde işletim sistemi ile Cloud-init işlevlerini sağlamak için, onaylanan Linux 'taki iş ortakları ve yukarı akış ile işbirliği yaptık, paketler güncelleştirilir ve paket depolarında herkese açık hale getirilir. 

Cloud-init ' i Azure 'da, desteklenen Linux 'un Azure 'da, paket desteğinin ve sonra görüntü desteğinin kullanılabilir hale getirmek için iki aşama vardır:
* ' Azure 'da Cloud-init paket desteği ', Cloud-init paketlerinin hangi sürümlerinde desteklendiği veya önizlemede kullanıldığı, bu paketleri özel görüntüde IŞLETIM sistemiyle kullanabilirsiniz.
* görüntü zaten Cloud-init kullanmak üzere yapılandırıldıysa ' Image Cloud-init Ready ' belgeleri.


### <a name="canonical"></a>Canonical
| Yayımcı/sürüm| Sunduğu | SKU | Sürüm | görüntü bulutu-init Ready | Azure 'da Cloud-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|Kurallı 18,04 |UbuntuServer |18,04-LTS |en son |evet | evet |
|Kurallı 16,04|UbuntuServer |16.04-LTS |en son |evet | evet |
|Kurallı 14,04|UbuntuServer |14.04.5-LTS |en son |evet | evet |

### <a name="rhel"></a>RHEL
| Yayımcı/sürüm | Sunduğu | SKU | Sürüm | görüntü bulutu-init Ready | Azure 'da Cloud-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|RedHat 7,6 |RHEL |7-RAW-CI |7.6.2019072418 |evet | Evet-paket sürümünden destek: *18.2-1. el7_6.2*|
|RedHat 7,7 |RHEL |7-RAW-CI |7.7.2019081601 | Evet (Bu bir önizleme görüntüsüdür ve tüm RHEL 7,7 görüntüleri Cloud-init ' i destekledikten sonra, bu adım 2020 ' nin kaldırılmasına, bildirimin verildiğine dikkat edin. | Evet-paket sürümünden destek: *18.5 -3. EL7*|
|RedHat 7,7 |RHEL |7-HAM | yok| Şubat 2020 ' den başlamak için görüntü yok| Evet-paket sürümünden destek: *18.5 -3. EL7*|
|RedHat 7,7 |RHEL |7-LVM | yok| Şubat 2020 ' den başlamak için görüntü yok| Evet-paket sürümünden destek: *18.5 -3. EL7*|
|RedHat 7,7 |RHEL |7,7 | yok| Şubat 2020 ' den başlamak için görüntü yok | Evet-paket sürümünden destek: *18.5 -3. EL7*|
|RedHat 7,7 |RHEL-byos | RHEL-lvm77 | yok|Şubat 2020 ' den başlamak için görüntü yok  | Evet-paket sürümünden destek: *18.5 -3. EL7*|

### <a name="centos"></a>CentOS

| Yayımcı/sürüm | Sunduğu | SKU | Sürüm | görüntü bulutu-init Ready | Azure 'da Cloud-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|OpenLogic 7,7 |CentOS |7-CI |7.7.20190920 |Evet (Bu bir önizleme görüntüsüdür ve tüm CentOS 7,7 görüntüleri Cloud-init ' i destekledikten sonra, bu adım 2020 ' nin kaldırılmasına ve bu bildirimin verildiğine dikkat edin. | Evet-paket sürümünden destek: *18.5 -3. EL7. CentOS*|

* Cloud-init etkin olacak CentOS 7,7 görüntüleri, Şubat 2020 ' de burada güncelleştirilir. 

### <a name="oracle"></a>Oracle

| Yayımcı/sürüm | Sunduğu | SKU | Sürüm | görüntü bulutu-init Ready | Azure 'da Cloud-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|Oracle 7,7 |Oracle-Linux |77-CI |7.7.01| Önizleme resmi (Bu bir önizleme görüntüsüdür ve tüm Oracle 7,7 görüntüleri Cloud-init ' i destekledikten sonra, bu adım 2020 ' den kaldırılacağına dikkat edilecek) | Hayır, önizleme aşamasında paket: *18.5-3.0.1. EL7*

### <a name="debian--suse-sles"></a>& SuSE SLES
Şu anda Önizleme desteği için çalışıyoruz, Şubat ve Mart 2020 ' de güncelleştirmeler beklenir.

Şu anda Azure Stack, Cloud-init özellikli görüntülerin sağlanması destekleyecektir.


## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Cloud-init ve Linux Aracısı (WALA) arasındaki fark nedir?
WALA, VM 'Leri sağlamak ve yapılandırmak ve [Azure uzantılarını](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux)işlemek için kullanılan bir Azure platforma özgü aracıdır. 

Mevcut Cloud-init müşterilerinin geçerli Cloud-init betiklerini kullanmasına izin vermek için VM 'Leri yapılandırma görevini geliştirdik ve zengin Cloud-init yapılandırma işlevselliğinden faydalanmak için yeni müşteriler. Linux sistemlerini yapılandırmaya yönelik Cloud-init betiklerine mevcut yatırımlarınızın varsa, Cloud-init işlemini etkinleştirmek için **ek ayarlar gerekmez** . 

Cloud-init, Azure uzantılarını işleyemez, bu nedenle, uzantıları işlemek için görüntüde hala wala gereklidir, ancak Cloud-init tarafından sağlanmaya dönüştürülmekte olan, desteklenen Linux Distro 'lara görüntüleri için wala 'nın devre dışı olması gerekir yüklendi ve doğru kurulum.

Bir VM oluştururken, sağlama sırasında Azure CLı `--custom-data` anahtarını eklemezseniz, Cloud-init veya WALA VM 'yi sağlamak için gereken en düşük VM sağlama parametrelerini alır ve varsayılan olarak dağıtımı tamamlar.  `--custom-data` anahtarıyla Cloud-init yapılandırmasına başvurdıysanız, özel verilerinizde bulunan her şey VM 'nin önyüklemesinde Cloud-init tarafından kullanılabilir olacaktır.

VM 'lere uygulanan Cloud init yapılandırmalarının zaman kısıtlamaları yoktur ve zaman aşımına uğraarak dağıtımın başarısız olmasına neden olmaz. Bu, WALA için geçerli değildir, WALA varsayılanlarını özel verileri işleyecek şekilde değiştirirseniz, bu, VM oluşturma süresi olan 40dakikalık toplam VM sağlama süresi süresini aşamaz.

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Cloud-init etkin bir sanal makine dağıtma
Cloud-init etkin bir sanal makine dağıtmak, dağıtım sırasında bulut-init özellikli bir dağıtıma başvurmak kadar basittir.  Linux dağıtım bakım ve Cloud-init ' i temel Azure yayımlanmış görüntülerine etkinleştirip tümleştirmelerini tercih etmek gerekir. Dağıtmak istediğiniz görüntünün Cloud-init ' i onayladıktan sonra, görüntüyü dağıtmak için Azure CLı ' yi kullanabilirsiniz. 

Bu görüntüyü dağıtmanın ilk adımı, [az Group Create](/cli/azure/group) komutuyla bir kaynak grubu oluşturmaktır. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Sonraki adım, geçerli kabuğunuzun içinde *kabuğunuzda Cloud-init. txt* adlı bir dosya oluşturmak ve aşağıdaki yapılandırmayı yapıştırmaktır. Bu örnekte, dosyayı yerel makinenizde değil Cloud Shell oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Başta birinci satır olmak üzere cloud-init dosyasının tamamının doğru bir şekilde kopyalandığından emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Dosyadan çıkmak için `ctrl-X` tuşuna basın, dosyayı kaydetmek için `y` yazın ve çıkışta dosya adını doğrulamak için `enter` 'e basın.

Son adım [az VM Create](/cli/azure/vm) komutuyla bir VM oluşturmaktır. 

Aşağıdaki örnek, *centos74* adlı bir sanal makine oluşturur ve varsayılan anahtar konumunda henüz yoksa SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  `--custom-data` parametresini kullanarak cloud-init yapılandırma dosyanızı geçirin. Dosyayı mevcut çalışma dizininizin dışına kaydettiyseniz *cloud-init.txt* yapılandırmasının tam yolunu belirtin. Aşağıdaki örnek, *centos74*adlı bir sanal makine oluşturur:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

VM oluşturulduğunda Azure CLı, dağıtımınıza özgü bilgileri gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.  VM 'nin oluşturulması, yüklenecek paketlerin ve başlatılacak uygulamayı biraz zaman alır. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. VM 'de SSH oluşturabilir ve Cloud-init günlüklerini görüntülemek için sorun giderme bölümünde özetlenen adımları kullanabilirsiniz. 

## <a name="troubleshooting-cloud-init"></a>Cloud-init sorunlarını giderme
VM sağlandıktan sonra Cloud-init, VM 'yi yapılandırmak için `--custom-data` tanımlanmış tüm modüller ve betiklerle çalışır.  Yapılandırmadan herhangi bir hata veya iş sorunu gidermeniz gerekiyorsa, **/var/log/Cloud-init.log**dosyasında bulunan Cloud-init günlüğünde modül adını (örneğin`disk_setup` veya `runcmd`) aramanız gerekir.

> [!NOTE]
> Her modül hatası önemli bir Cloud-init genel yapılandırma hatasına neden olmaz. Örneğin, `runcmd` modülünü kullanarak, komut dosyası başarısız olursa, runcmd modülü yürütüldüğü için Cloud-init hala sağlama başarılı olur.

Cloud-init günlüğü hakkında daha fazla bilgi için [Cloud-init belgelerine](https://cloudinit.readthedocs.io/en/latest/topics/logging.html) bakın 

## <a name="next-steps"></a>Sonraki adımlar
Cloud-init yapılandırma değişikliklerinin örnekleri için aşağıdaki belgelere bakın:
 
- [VM 'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede var olan paketleri güncelleştirmek için bir paket yöneticisi çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştir](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yüklemesi, yapılandırma dosyalarını güncelleştirme ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
 
