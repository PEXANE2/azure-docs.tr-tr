---
title: Cloud-init kullanarak sorun giderme
description: Cloud-init kullanarak bir Azure VM sağlama sorunlarını giderin.
author: danielsollondon
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: danis
ms.reviewer: cynthn
ms.openlocfilehash: 2bf0443465f0cfd98f8bce93e60f9007ac7503be
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042093"
---
# <a name="troubleshooting-vm-provisioning-with-cloud-init"></a>Cloud-init ile VM sağlama sorunlarını giderme

Sağlama yapmak için Cloud-init ' i kullanarak genelleştirilmiş özel görüntüler oluşturuyorsanız ancak VM 'nin doğru bir şekilde oluşturulmadığını tespit ediyorsanız, özel görüntülerinizin sorunlarını gidermeniz gerekecektir.

Bazı örnekler, sağlama sorunları:
- VM, 40 dakika boyunca ' oluşturma ' ile takılmış ve VM oluşturma işlemi başarısız olarak işaretlendi
- CustomData işlenmedi
- Kısa ömürlü disk takılamaz
- Kullanıcılar oluşturulmaz veya Kullanıcı erişimi sorunları vardır
- Ağ iletişimi doğru ayarlanmadı
- Dosya veya bölüm hatasını takas etme

Bu makalede, Cloud-init sorunlarını giderme adımları anlatılmaktadır. Daha ayrıntılı bilgi için bkz. [Cloud-init derin DIVE](https://docs.microsoft.com/azure/virtual-machines/linux/cloud-init-deep-dive).

## <a name="step-1-test-the-deployment-without-customdata"></a>1. Adım: bir dağıtımı customData olmadan test etme

Cloud-init, VM oluşturulduğunda kendisine geçirilen customData öğesini kabul edebilir. Öncelikle bu, dağıtımlarla ilgili herhangi bir soruna neden olmadığından emin olmalısınız. Herhangi bir yapılandırmaya geçmeden VM 'yi sağlamayı deneyin. VM 'yi sağlayamazsa, geçirdiğiniz yapılandırmanın uygulanamamakta olduğunu fark ederseniz, [Adım 4]()' e geçin. 

## <a name="step-2-review-image-requirements"></a>2. Adım: görüntü gereksinimlerini gözden geçirme
VM sağlama başarısızlığının birincil nedeni, işletim sistemi görüntüsünün Azure 'da çalışmaya yönelik önkoşulları karşılamamasına neden olur. Azure 'da sağlamaya çalışmadan önce görüntülerinizin doğru şekilde hazırlandığından emin olun. 


Aşağıdaki makalelerde, Azure 'da desteklenen çeşitli Linux dağıtımlarını hazırlama adımları gösterilmektedir:

- [CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SLES ve openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Diğerleri: onaylı olmayan dağıtımlar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Desteklenen Azure Cloud-init görüntüleri](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init)için Linux dağıtımları, görüntüyü Azure 'da doğru şekilde sağlamak için gereken tüm paketleri ve konfigürasyonları zaten vardır. SANAL makinenizin kendi seçkin görüntüsünden oluşturmamakta olduğunu fark ederseniz, isteğe bağlı customData ile Cloud-init için yapılandırılmış desteklenen bir Azure Marketi görüntüsünü deneyin. CustomData, bir Azure Market görüntüsü ile düzgün çalışıyorsa, bu durumda, seçkin resimde bir sorun olabilir.

## <a name="step-3-collect--review-vm-logs"></a>3. Adım: VM günlüklerini toplama & İnceleme

VM sağlamadığında, Azure, 20 dakika boyunca ' oluşturma ' durumu gösterir ve ardından VM 'yi yeniden başlatır ve son olarak bir hata ile işaretlemeden önce, VM dağıtımını başarısız olarak işaretlemeden önce 20 dakika bekleyin `OSProvisioningTimedOut` .

VM çalışırken, sağlanmasının neden başarısız olduğunu anlamak için VM 'deki günlüklere ihtiyacınız olacak.  VM sağlamasının neden başarısız olduğunu anlamak için VM 'yi durdurmayın. VM 'yi çalışır durumda tutun. Günlükleri toplamak için, başarısız VM 'yi çalışır durumda tutmanız gerekir. Günlükleri toplamak için aşağıdaki yöntemlerden birini kullanın:

- [Seri Konsol](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console-grub-single-user-mode)

- VM oluşturmadan önce [önyükleme tanılamayı etkinleştirin](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-monitor#enable-boot-diagnostics) ve önyükleme sırasında bunları [görüntüleyin](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-monitor#view-boot-diagnostics) .

- İşletim sistemi diskini eklemek ve bağlamak için [az VM Repair çalıştırın](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-linux-vm-using-azure-virtual-machine-repair-commands) , bu da bu günlükleri toplamanıza olanak tanır:
```bash
/var/log/cloud-init*
/var/log/waagent*
/var/log/syslog*
/var/log/rsyslog*
/var/log/messages*
/var/log/kern*
/var/log/dmesg*
/var/log/boot*
```
İlk sorun giderme 'yi başlatmak için Cloud-init günlükleriyle başlayın ve hatanın nerede oluştuğunu anlayın, daha ayrıntılı bilgi edinmek için diğer günlükleri kullanın ve ek öngörüler sağlayın. 
* /var/log/Cloud-Init.log
* /var/log/Cloud-init-output.log
* Seri/önyükleme günlükleri

Tüm günlüklerde "başarısız", "uyarı", "uyar", "Err", "Error", "ERROR" için arama başlatın. Büyük/küçük harfe duyarlı aramaları yoksaymak için yapılandırma ayarlanması önerilir. 

> [!TIP]
> Özel görüntüde sorun giderirken, görüntü sırasında bir Kullanıcı eklemeyi göz önünde bulundurmanız gerekir. Hazırlama yönetici kullanıcıyı ayarlayamazsa, işletim sisteminde oturum açmaya devam edebilirsiniz.

## <a name="analyzing-the-logs"></a>Günlükler çözümleniyor

İşte her bir Cloud-init günlüğünde nelerin aranacağı hakkında daha fazla ayrıntı.

### <a name="varlogcloud-initlog"></a>/var/log/Cloud-Init.log

Varsayılan olarak, hata ayıklama veya daha yüksek önceliğe sahip tüm Cloud-init olayları öğesine yazılır `/var/log/cloud-init.log` . Bu, Cloud-init başlatma sırasında oluşan her olayın ayrıntılı günlüklerini sağlar. 

Örneğin:

```console
2019-10-10 04:51:25,321 - util.py[DEBUG]: Failed mount of '/dev/sr0' as 'auto': Unexpected error while running command.
Command: ['mount', '-o', 'ro,sync', '-t', 'auto', u'/dev/sr0', '/run/cloud-init/tmp/tmpLIrklc']
Exit code: 32
Reason: -
Stdout:
Stderr: mount: unknown filesystem type 'udf'
2020-01-31 00:21:53,352 - DataSourceAzure.py[WARNING]: /dev/sr0 was not mountable
```


Bir hata veya uyarı bulduktan sonra, Cloud-init ' in hata veya uyarı vermeden önce hangi amaçla çalıştığını anlamak için Cloud-init günlüğünde geriye doğru okuyun. Çoğu durumda, Cloud-init, işletim sistemi komutlarını çalıştırmak veya hatadan önce sağlama işlemleri gerçekleştirmelidir, bu da hata durumunda hataların neden görüntülendiğiyle ilgili öngörüler sağlayabilir. Aşağıdaki örnek, Cloud-init ' ın bir hata olmadan önce bir cihazı bağlamaya çalıştığını gösterir.

```output
2019-10-10 04:51:24,010 - util.py[DEBUG]: Running command ['mount', '-o', 'ro,sync', '-t', 'auto', u'/dev/sr0', '/run/cloud-init/tmp/tmpXXXXX'] with allowed return codes [0] (shell=False, capture=True)
```

[Seri konsoluna](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console-grub-single-user-mode)erişiminiz varsa, Cloud-init ' nin çalıştırmaya çalıştığı komutu yeniden çalıştırmayı deneyebilirsiniz.

Günlüğe kaydetme, `/var/log/cloud-init.log` /etc/Cloud/Cloud.exe içinde de yeniden yapılandırılabilir ve 05_logging. cfg. Cloud-init günlüğü hakkında daha fazla bilgi için [Cloud-init belgelerine](https://cloudinit.readthedocs.io/en/latest/topics/logging.html)bakın. 

### <a name="varlogcloud-init-outputlog"></a>/var/log/Cloud-init-output.log

`stdout` `stderr` [Cloud-init aşamaları](cloud-init-deep-dive.md)sırasında ve üzerinden bilgi edinebilirsiniz. Bu normalde, her bir aşamanın zaman damgasıyla birlikte yönlendirme tablosu bilgilerini, ağ bilgilerini, SSH ana bilgisayar anahtarı doğrulama bilgilerini `stdout` ve `stderr` her bir Cloud-init aşamasını içerir. İsterseniz `stderr` ve `stdout` günlük kaydı yeniden yapılandırılabilir `/etc/cloud/cloud.cfg.d/05_logging.cfg` .

### <a name="serialboot-logs"></a>Seri/önyükleme günlükleri 

Cloud-init birden çok bağımlılığı vardır. Bunlar, ağ, depolama, ISO bağlama özelliği ve geçici diski bağlama ve biçimlendirme gibi Azure 'da görüntüler için gerekli önkoşullara belgelenmiştir. Bunlardan herhangi biri hata oluşturabilir ve Cloud-Init 'in başarısız olmasına neden olabilir. Örneğin, VM bir DHCP kirası alamazsanız, Cloud-init başarısız olur.

Hala Cloud-init 'in sağlayamamasının nedenini yalıtadıysanız, hangi Cloud-init aşamalarını ve modüllerin ne zaman çalışacağını anlamanız gerekir. Daha ayrıntılı bilgi için bkz. [Cloud-init](cloud-init-deep-dive.md) ' i daha derin inceleyin.


## <a name="step-4-investigate-why-the-configuration-isnt-being-applied"></a>4. Adım: yapılandırmanın neden uygulanamadığını araştırın
Cloud-init ' teki tüm hatalar önemli bir sağlama hatasına neden olmaz. Örneğin, `runcmd` modülünü bir Cloud-init yapılandırmasında kullanıyorsanız, çalıştığı komuttan sıfır olmayan bir çıkış kodu, VM sağlamasının başarısız olmasına neden olur. Bunun nedeni, Cloud-init ' in ilk 3 aşamasında gerçekleşen çekirdek sağlama işlevlerinden sonra çalışır. Yapılandırmanın neden uygulanmadığından ilgili sorunları gidermek için, adım 3 ' teki günlükleri ve Cloud-init modüllerini el ile inceleyin. Örneğin:

- `runcmd`-betikler hatasız çalışıyor mu? Yapılandırmayı, beklendiği gibi çalıştırıldıklarından emin olmak için terminalden el ile çalıştırın.
- Paket yükleme-VM 'nin paket depolarına erişimi var mı?
- Ayrıca `customData` , VM 'ye sunulan veri yapılandırmasını da denetlemeniz gerekir, bu, ' de bulunur `/var/lib/cloud/instances/<unique-instance-identifier>/user-data.txt` .


## <a name="next-steps"></a>Sonraki adımlar

Hala Cloud-init 'nin yapılandırmayı çalıştırmadığından yalıtılamadıysanız, her bir Cloud-init aşamasında ne olacağı ve modüllerin ne zaman çalıştığı hakkında daha yakından bakmanız gerekir. Daha fazla bilgi için bkz. [Cloud-init yapılandırmasını daha ayrıntılı hale döndürme](https://docs.microsoft.com/azure/virtual-machines/linux/cloud-init-deep-dive) . 
