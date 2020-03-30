---
title: Azure'daki Windows VM'leri hakkında SSS
description: Kaynak Yöneticisi modeliyle oluşturulan Windows sanal makineleri yle ilgili sık sorulan bazı soruların yanıtlarını sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: e3d2673ba52ba0cca36e2a999558313b64716ade
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299623"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows Sanal Makineler hakkında sık sorulan soru
Bu makalede, Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da oluşturulan Windows sanal makineleri yle ilgili bazı sık sorulan sorular ele alılmıştır. Bu konunun Linux sürümü için, [Linux Sanal Makineler hakkında sık sorulan soruya](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Azure'da Microsoft sunucu yazılımını çalıştırmaya yönelik destek ilkesi hakkında bilgi için [Azure Sanal Makineleri için Microsoft sunucu yazılım desteğine](https://support.microsoft.com/kb/2721672)bakın.

Windows 7, Windows 8.1 ve Windows 10'un belirli sürümleri, geliştirme ve test görevleri için MSDN Azure avantaj aboneleri ve MSDN Dev ve Test Pay-As-You-Go aboneleri tarafından kullanılabilir. Yönerge ve kısıtlamalar dahil olmak üzere ayrıntılı bilgi edinmek için bkz. [MSDN aboneleri için Windows İstemci görüntüleri](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski en fazla 32.767 GiB olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure Yönetilen Diskler, verilerin kalıcı olarak saklanması için Azure Sanal Makineleri ile birlikte kullanılmak üzere önerilen disk depolama teklifleridir. Her Sanal Makine ile birden çok Yönetilen Disk kullanabilirsiniz. Yönetilen Diskler iki tür dayanıklı depolama seçeneği sunar: Premium ve Standart Yönetilen Diskler. Fiyatlandırma bilgileri için yönetilen [Diskler](https://azure.microsoft.com/pricing/details/managed-disks)Fiyatlandırması'na bakın.

Azure depolama hesapları, işletim sistemi diski ve tüm veri diskleri için depolama alanı da sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal makineme nasıl erişebilirim?
Windows VM için Uzak Masaüstü Bağlantısı (RDP) kullanarak uzak bağlantı kurun. Talimatlar için, [Windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açabilirsiniz'](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)a bakın. Sunucu Uzak Masaüstü Hizmetleri oturum ana bilgisayarı olarak yapılandırılmadığı sürece en fazla iki eşzamanlı bağlantı desteklenir.  

Uzak Masaüstü ile ilgili sorunlar yaşıyorsanız, [Windows tabanlı Bir Azure Sanal Makine'ye bağlanan Sorun Giderme Uzak Masaüstü bağlantılarına](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. 

Hyper-V'yi biliyorsanız, VMConnect'e benzer bir araç arıyor olabilirsiniz. Sanal makineye konsol erişimi desteklenmediği için Azure benzer bir araç sunmaz.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Verileri depolamak için geçici diski (Varsayılan olarak D: sürücü) kullanabilir miyim?
Verileri depolamak için geçici diski kullanmayın. Bu yalnızca geçici depolama dır, bu nedenle kurtarılamayan verileri kaybetme riskine dersiniz. Sanal makine farklı bir ana bilgisayara geçtiğinde veri kaybı oluşabilir. Sanal makinenin yeniden boyutlandırılması, konağın güncelleştirilmesi veya konaktaki bir donanım hatası, sanal makinenin taşınmasını gerektirecek olası nedenler arasındadır.

D: sürücü harfini kullanması gereken bir uygulamanız varsa, geçici diskin D:'den başka bir şey kullanması için sürücü harflerini yeniden atayabilirsiniz. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Geçici diskin sürücü harfini nasıl değiştirebilirim?
Sürücü harfini sayfa dosyasını taşıyarak ve sürücü harflerini yeniden atayarak değiştirebilirsiniz, ancak adımları belirli bir sırada yaptığınızdan emin olmanız gerekir. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kullanılabilirlik kümesine varolan bir VM ekleyebilir miyim?
Hayır. VM'nizin bir kullanılabilirlik kümesinin parçası olmasını istiyorsanız, VM'yi küme içinde oluşturmanız gerekir. Şu anda oluşturulduktan sonra kullanılabilirlik kümesine VM eklemenin bir yolu yok.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Azure'a sanal bir makine yükleyebilir miyim?
Evet. Yönergeler için bkz. Şirket [içi VM'leri Azure'a Geçirme.](on-prem-to-azure.md)

## <a name="can-i-resize-the-os-disk"></a>İşletim sistemi diskini yeniden boyutlandırabilir miyim?
Evet. Yönergeler için, [Azure Kaynak Grubunda Sanal Makinenin işletim sistemi sürücüsünün nasıl genişletilir'e](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Varolan bir Azure VM'sini kopyalayabilir veya klonlayabilir miyim?
Evet. Yönetilen görüntüleri kullanarak, sanal bir makinenin görüntüsünü oluşturabilir ve ardından görüntüyü birden çok yeni VM oluşturmak için kullanabilirsiniz. Talimatlar için [bkz.](tutorial-custom-images.md)

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Azure Kaynak Yöneticisi aracılığıyla Neden Kanada Orta ve Kanada Doğu bölgelerini göremiyorum?

Kanada Orta ve Kanada Doğu'nun iki yeni bölgesi, mevcut Azure abonelikleri için sanal makine oluşturma için otomatik olarak kaydedilmez. Bu kayıt, sanal bir makine Azure portalı üzerinden Azure Kaynak Yöneticisi kullanılarak başka bir bölgeye dağıtıldığında otomatik olarak yapılır. Sanal bir makine başka bir Azure bölgesine dağıtıldıktan sonra, yeni bölgeler sonraki sanal makineler için kullanılabilir olmalıdır.

## <a name="does-azure-support-linux-vms"></a>Azure Linux VM'lerini destekliyor mu?
Evet. Denemek için hızlı bir Linux VM oluşturmak için, [Portalı kullanarak Azure'da Linux VM oluşturma'ya](../linux/quick-create-portal.md)bakın.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra VM'me NIC ekleyebilir miyim?
Evet, bu artık mümkün. VM'nin önce ayrılmasının durdurulması gerekir. Daha sonra bir NIC ekleyebilir veya kaldırabilirsiniz (VM'deki son NIC olmadığı sürece). 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 15 karakter uzunluğunda olabilir. Kaynaklarınızı adlandırma hakkında daha fazla bilgi için [adlandırma kuralları ve kısıtlamalarına](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging) bakın.

## <a name="are-there-any-resource-group-name-requirements"></a>Kaynak grubu adı gereksinimleri var mı?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Kaynak grupları hakkında daha fazla bilgi için [adlandırma kuralları ve kısıtlamalarına](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming) bakın.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>VM oluştururken kullanıcı adı gereksinimleri nelerdir?

Kullanıcı adları en fazla 20 karakter uzunluğunda olabilir ve bir dönemde (".") sonlanamaz. 

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


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>VM oluştururken parola gereksinimleri nelerdir?

Kullandığınız araca bağlı olarak değişen parola uzunluğu gereksinimleri vardır:
 - Portal - 12 - 72 karakter arasında
 - PowerShell - 8 - 123 karakter arasında
 - CLI - 12 - 123 arası

* Küçük harfli karakter içermeli
* Büyük harfli karakter içermeli
* Rakam içermeli
* Özel karakter içermeli (Regex eşleşmesi [\W_])

Aşağıdaki parolalara izin verilmez:

<table>
    <tr>
        <td>abc@123</td>
        <td>seni seviyorum!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$$word</td>
        <td>pass@word1</td>
        <td>Parola!</td>
        <td>Şifre1</td>
        <td>Şifre22</td>
    </tr>
</table>