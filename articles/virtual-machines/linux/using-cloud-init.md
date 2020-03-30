---
title: Azure'da Linux VM'leri için bulut içi desteğine genel bakış
description: Azure'da bir VM'yi sağlama zamanında yapılandırmak için bulut giriş özelliklerine genel bakış.
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
ms.openlocfilehash: 1f0395956fa6977be5d1d6f4f4faf06b84c094d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79465048"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için bulut-init desteği
Bu makalede, Azure'da sağlama zamanında sanal makine (VM) veya sanal makine ölçeği kümelerini yapılandırmak için [bulut init](https://cloudinit.readthedocs.io) için var olan destek açıklanmaktadır. Bu bulut-init yapılandırmaları, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalıştırılır.  

VM Sağlama, Azure'un ev sahibi adı, kullanıcı adı, parola vb. gibi VM Oluşturma parametre değerlerinizi aktaracağı ve bunları önyükleme yle VM'de kullanılabilir hale getireceği bir işlemdir. Bir 'sağlama aracısı' bu değerleri tüketir, VM'yi yapılandıracak ve tamamlandığında geri rapor verir. 

Azure, iki sağlama aracısını [ve](https://cloudinit.readthedocs.io) [Azure Linux Aracısını (WALA)](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)destekler.

## <a name="cloud-init-overview"></a>bulut-init genel bakış
[cloud-init](https://cloudinit.readthedocs.io) ilk kez önyükleme olarak bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çağrıldığı için, yapılandırmanızı uygulamak için ek adımlar veya gerekli aracılar yoktur.  Dosyalarınızı veya diğer girişlerinizi `#cloud-config` nasıl düzgün biçimlendirecek hakkında daha fazla bilgi için bulut girişi [belgeleme sitesine](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data)bakın.  `#cloud-config`dosyalar base64 kodlanmış metin dosyalarıdır.

bulut-init de dağıtımlar arasında çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. cloud-init seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Azure pazarda bulut ekleme özellikli görüntülerin kullanılabilmesi için onaylanan Linux dağıtım ortaklarımızla aktif olarak çalışıyoruz. Bu görüntüler, bulut-init dağıtımlarınızın ve yapılandırmalarınızın VM'ler ve sanal makine ölçek kümeleriyle sorunsuz çalışmasını sağlar. Başlangıçta, Azure'daki işletim sistemi yle bulut giriş işlevlerini sağlamak için onaylanan Linux dağıtım ortakları ve upstream ile işbirliği yapıyoruz, ardından paketler güncelleştirildi ve dağıtım paketi depolarında herkese açık hale getirilir. 

Azure'da onaylanan Linux dağıtım işletim sistemi, paket desteği ve ardından görüntü desteği için bulut initedi kullanılabilir hale getirmenin iki aşaması vardır:
* 'Azure'da bulut-init paketi desteği' belgelerinde, paketlerin desteklendiği veya önizlemede olduğu, böylece işletim sistemi ile bu paketleri özel bir resimde kullanabilirsiniz.
* 'görüntü bulut-init hazır' belgeler görüntü zaten bulut init kullanmak için yapılandırılmıştır.


### <a name="canonical"></a>Canonical
| Yayımcı / Sürüm| Sunduğu | SKU | Sürüm | görüntü bulut-init hazır | Azure'da bulut-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|Kanonik 18.04 |UbuntuServer |18.04-LTS |en son |evet | evet |
|Kanonik 16.04|UbuntuServer |16.04-LTS |en son |evet | evet |
|Kanonik 14.04|UbuntuServer |14.04.5-LTS |en son |evet | evet |

### <a name="rhel"></a>RHEL
| Yayımcı / Sürüm | Sunduğu | SKU | Sürüm | görüntü bulut-init hazır | Azure'da bulut-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|RedHat 7.6 |RHEL |7-RAW-CI |7.6.2019072418 |evet | Evet - paket sürümünden destek: *18.2-1.el7_6.2*|
|RedHat 7.7 |RHEL |7-RAW-CI |7.7.2019081601 | Evet (bu bir önizleme görüntü olduğunu unutmayın, ve bir kez tüm RHEL 7.7 görüntüleri bulut-init destek, bu orta 2020 kaldırılacak, haber verilecektir) | Evet - paket sürümünden destek: *18.5-3.el7*|
|RedHat 7.7 |RHEL |7-HAM | yok| hayır - Nisan 2020 sonuna kadar tamamlamak için görüntü güncellemeleri| Evet - paket sürümünden destek: *18.5-3.el7*|
|RedHat 7.7 |RHEL |7-LVM | yok| hayır - Nisan ayı sonunda tamamlamak için görüntü güncellemeleri| Evet - paket sürümünden destek: *18.5-3.el7*|
|RedHat 7.7 |RHEL |7.7 | yok| hayır - Nisan ayı sonunda tamamlamak için görüntü güncellemeleri | Evet - paket sürümünden destek: *18.5-3.el7*|
|RedHat 7.7 |rhel-byos | rhel-lvm77 | yok|hayır - Nisan ayı sonunda tamamlamak için görüntü güncellemeleri  | Evet - paket sürümünden destek: *18.5-3.el7*|

### <a name="centos"></a>CentOS

| Yayımcı / Sürüm | Sunduğu | SKU | Sürüm | görüntü bulut-init hazır | Azure'da bulut-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|OpenLogic 7.7 |CentOS |7-CI |7.7.20190920 |Evet (bu bir önizleme görüntü olduğunu unutmayın, ve bir kez tüm CentOS 7.7 görüntüleri bulut-init destek, bu orta 2020 kaldırılacak, haber verilecektir) | Evet - paket sürümünden destek: *18.5-3.el7.centos*|

* Bulut girişetkin olacak CentOS 7.7 görüntüleri Mart 2020'de burada güncellenecek 

### <a name="oracle"></a>Oracle

| Yayımcı / Sürüm | Sunduğu | SKU | Sürüm | görüntü bulut-init hazır | Azure'da bulut-init paketi desteği|
|:--- |:--- |:--- |:--- |:--- |:--- |
|Oracle 7.7 |Oracle-Linux |77-ci |7.7.01| önizleme görüntü (bu bir önizleme görüntü olduğunu unutmayın, ve bir kez tüm Oracle 7.7 görüntüleri bulut-init destek, bu orta 2020 kaldırılacak, haber verilecektir) | hayır, önizleme, paket: *18.5-3.0.1.el7*

### <a name="debian--suse-sles"></a>Debian & SuSE SLES
Şu anda desteği önizlemek, Şubat ve Mart 2020'de güncellemeler beklemek için çalışıyoruz.

Şu anda Azure Yığını, bulut init özellikli görüntülerin sağlanmasını destekleyecektir.


## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Cloud-init ve Linux Agent (WALA) arasındaki fark nedir?
WALA, VM'leri sağlamak ve yapılandırmak ve Azure [uzantılarını](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux)işlemek için kullanılan Azure platformuna özgü bir aracıdır. 

Mevcut bulut init müşterilerinin mevcut bulut init komut dosyalarını veya yeni müşterilerin zengin bulut yapılandırma yapılandırma işlevlerinden yararlanabilmeleri için Linux Aracısı yerine bulut init kullanacak şekilde VM'leri yapılandırma görevini artırıyoruz. Linux sistemlerini yapılandırmak için bulut init komut dosyalarına yönelik mevcut yatırımlarınız varsa, bunları bulut girintisi işlemini etkinleştirmek için **ek ayar gerekmez.** 

bulut-init Azure uzantıları işleyemez, bu nedenle WALA hala uzantıları işlemek için görüntü gereklidir, ancak bulut-init tarafından hükme dönüştürülmektedir onaylanan Linux dağıtım görüntüleri için, onun sağlama kodu devre dışı olması gerekir, onlar WALA olacak yüklü ve doğru kurulum.

Bir VM oluştururken, sağlama süresine Azure `--custom-data` CLI anahtarını eklemezseniz, bulut init veya WALA, VM'yi sağlamak ve dağıtımı varsayılanlarla tamamlamak için gereken en az VM sağlama parametrelerini alır.  Anahtarla bulut init yapılandırmasına `--custom-data` başvurursanız, özel verilerinizde ne varsa, VM önyüklemeleri bulut ayarı için kullanılabilir olacaktır.

VM'lere uygulanan bulut init yapılandırmalarında zaman kısıtlamaları yoktur ve zamanlama yla dağıtımın başarısız olmasına neden olmaz. Bu WALA için geçerli değildir, özel verileri işlemek için WALA varsayılan değiştirirseniz, 40mins toplam VM sağlama süresi ödeneği geçemez, eğer öyleyse, VM Oluşturma başarısız olur.

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Bulut init etkin Sanal Makine dağıtma
Bulut init etkin leştirilmiş bir sanal makineyi dağıtmak, dağıtım sırasında bulut init etkin leştirilmiş bir dağıtıma başvurmak kadar kolaydır.  Linux dağıtım destekçileri, bulut ların azure tarafından yayınlanan resimlerini etkinleştirmeyi ve bunların tabanına entegre etmeyi seçmelidir. Dağıtmak istediğiniz resmin bulut içerisinetkin olduğunu doğruladıktan sonra, görüntüyü dağıtmak için Azure CLI'yi kullanabilirsiniz. 

Bu görüntüyü dağıtmanın ilk [adımı, az grubu oluşturma](/cli/azure/group) komutuna sahip bir kaynak grubu oluşturmaktır. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Bir sonraki adım, geçerli kabuğunuzda *cloud-init.txt* adlı bir dosya oluşturmak ve aşağıdaki yapılandırmayı yapıştırmaktır. Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Başta birinci satır olmak üzere cloud-init dosyasının tamamının doğru bir şekilde kopyalandığından emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Dosyadan çıkmak için `ctrl-X` `y` basın, dosyayı `enter` kaydetmek için yazın ve çıkışta dosya adını onaylamak için basın.

Son adım [az vm oluşturma](/cli/azure/vm) komutu ile bir VM oluşturmaktır. 

Aşağıdaki örnek, *centos74* adında bir VM oluşturur ve varsayılan anahtar konumunda zaten yoksa SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  `--custom-data` parametresini kullanarak cloud-init yapılandırma dosyanızı geçirin. Dosyayı mevcut çalışma dizininizin dışına kaydettiyseniz *cloud-init.txt* yapılandırmasının tam yolunu belirtin. Aşağıdaki *örnekcentos74*adlı bir VM oluşturur:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

VM oluşturulduğunda, Azure CLI dağıtımınıza özel bilgileri gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.  VM'nin oluşturulması, paketlerin yüklenmesi ve uygulamanın başlaması biraz zaman alır. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. VM'ye SSH'yi sokabilir ve bulut girişi günlüklerini görüntülemek için Sorun Giderme bölümünde belirtilen adımları kullanabilirsiniz. 

## <a name="troubleshooting-cloud-init"></a>Bulut ların karşıdan vurulmasında sorun giderme
VM sağlandıktan sonra, bulut init VM'yi yapılandırmak için `--custom-data` tanımlanan tüm modüller ve komut dosyası nda çalışacaktır.  Yapılandırmadaki hataları veya eksiklikleri gidermeniz gerekiyorsa,`disk_setup` **/var/log/cloud-init.log**adresinde bulunan bulut giriş günlüğündeki modül adını (veya `runcmd` örneğin) aramanız gerekir.

> [!NOTE]
> Her modül hatası, genel yapılandırma hatasına karşı önemli bir bulut hatasıyla sonuçlanmaz. Örneğin, `runcmd` modülü kullanarak, komut dosyası başarısız olursa, cloud-init, runcmd modülü çalıştırıldığı için başarılı olan hükmü bildirmeye devam eder.

Bulut girişi günlüğe kaydetme hakkında daha fazla bilgi için [bulut girişi belgelerine](https://cloudinit.readthedocs.io/en/latest/topics/logging.html) bakın 

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik bulut binme örnekleri için aşağıdaki belgelere bakın:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
 
