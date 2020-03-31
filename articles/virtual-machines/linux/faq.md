---
title: Azure'da Linux VM'leri için sık sorulan sorular
description: Kaynak Yöneticisi modeli yle oluşturulan Linux sanal makineleri hakkında sık sorulan bazı sorulara yanıt sağlar.
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 3c6a5e011a536cc9c34565d4f72a9bee6c6a5254
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945161"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Linux Sanal Makineler hakkında sık sorulan soru
Bu makalede, Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da oluşturulan Linux sanal makineleri yle ilgili bazı sık sorulan sorular ele alılmıştır. Bu konunun Windows sürümü için, [Windows Sanal Makineler hakkında sık sorulan soruya](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bakın

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Daha fazla bilgi için [Azure Onaylı Dağıtımlar'da Linux'a](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski en fazla 32.767 GiB olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Yönetilen Diskler, verilerin kalıcı olarak saklanması için Azure Sanal Makineleri ile birlikte kullanılmak üzere önerilen disk depolama teklifleridir. Her Sanal Makine ile birden çok Yönetilen Disk kullanabilirsiniz. Yönetilen Diskler iki tür dayanıklı depolama seçeneği sunar: Premium ve Standart Yönetilen Diskler. Fiyatlandırma bilgileri için yönetilen [Diskler](https://azure.microsoft.com/pricing/details/managed-disks)Fiyatlandırması'na bakın.

Azure depolama hesapları, işletim sistemi diski ve tüm veri diskleri için depolama alanı da sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal makineme nasıl erişebilirim?
Güvenli Kabuk (SSH) kullanarak sanal makinede oturum açmak için uzak bir bağlantı kurun. [Windows'dan](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya Linux ve [Mac'ten](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)nasıl bağlanılailgili talimatlara bakın. Varsayılan olarak, SSH en fazla 10 eş zamanlı bağlantıya izin verir. Yapılandırma dosyasını düzenleyerek bu sayıyı artırabilirsiniz.

Sorun yaşıyorsanız, [Güvenli Shell (SSH) bağlantılarını giderme](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)sorununa göz atın.

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Verileri depolamak için geçici diski (/dev/sdb1) kullanabilir miyim?
Verileri depolamak için geçici diski (/dev/sdb1) kullanmayın. Sadece geçici depolama için orada. Kurtarılamayan verileri kaybetme riskine sayılıyorsun.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Varolan bir Azure VM'sini kopyalayabilir veya klonlayabilir miyim?
Evet. Yönergeler için, [Kaynak Yöneticisi dağıtım modelinde bir Linux sanal makinenin kopyasını nasıl oluşturulur' bölümüne](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Azure Kaynak Yöneticisi aracılığıyla Neden Kanada Orta ve Kanada Doğu bölgelerini göremiyorum?
Kanada Orta ve Kanada Doğu'nun iki yeni bölgesi, mevcut Azure abonelikleri için sanal makine oluşturma için otomatik olarak kaydedilmez. Bu kayıt, sanal bir makine Azure portalı üzerinden Azure Kaynak Yöneticisi kullanılarak başka bir bölgeye dağıtıldığında otomatik olarak yapılır. Sanal bir makine başka bir Azure bölgesine dağıtıldıktan sonra, yeni bölgeler sonraki sanal makineler için kullanılabilir olmalıdır.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra VM'me NIC ekleyebilir miyim?
Evet, bu artık mümkün. VM'nin önce ayrılmasının durdurulması gerekir. Daha sonra bir NIC ekleyebilir veya kaldırabilirsiniz (VM'deki son NIC olmadığı sürece). 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 64 karakter uzunluğunda olabilir. Kaynaklarınızı adlandırma hakkında daha fazla bilgi için [adlandırma kuralları ve kısıtlamalarına](/azure/architecture/best-practices/resource-naming) bakın.

## <a name="are-there-any-resource-group-name-requirements"></a>Kaynak grubu adı gereksinimleri var mı?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Kaynak grupları hakkında daha fazla bilgi için [adlandırma kuralları ve kısıtlamalarına](/azure/architecture/best-practices/resource-naming) bakın.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>VM oluştururken kullanıcı adı gereksinimleri nelerdir?

Kullanıcı adları 1 - 32 karakter uzunluğunda olmalıdır.

Aşağıdaki kullanıcı adlarının izin verilmemektedir:

| | | | |
|-----------------|-----------|--------------------|----------|
| `administrator` | `admin`   | `user`             | `user1`  |
| `test`          | `user2`   | `test1`            | `user3`  |
| `admin1`        | `1`       | `123`              | `a`      |
| `actuser`       | `adm`     | `admin2`           | `aspnet` |
| `backup`        | `console` | `david`            | `guest`  |
| `john`          | `owner`   | `root`             | `server` |
| `sql`           | `support` | `support_388945a0` | `sys`    |
| `test2`         | `test3`   | `user4`            | `user5`  |
| `video`         |

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>VM oluştururken parola gereksinimleri nelerdir?

Kullandığınız araca bağlı olarak değişen parola uzunluğu gereksinimleri vardır:
 - Portal - 12 - 72 karakter arasında
 - PowerShell - 8 - 123 karakter arasında
 - CLI - 12 - 123 arası
 

Parolalar ayrıca aşağıdaki 4 karmaşıklık gereksiniminden 3'ünü karşılamalıdır:

* Küçük harfli karakter içermeli
* Büyük harfli karakter içermeli
* Rakam içermeli
* Özel karakter içermeli (Regex eşleşmesi [\W_])

Aşağıdaki parolalara izin verilmez:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Parola!</td>
        <td style="text-align:center">Şifre1</td>
        <td style="text-align:center">Şifre22</td>
        <td style="text-align:center">seni seviyorum!</td>
    </tr>
</table>