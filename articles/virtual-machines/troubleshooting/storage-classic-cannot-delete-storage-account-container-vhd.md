---
title: Azure klasik depolama hesaplarını, kapsayıcıları veya VHD 'lerini silerken oluşan sorunları giderme | Microsoft Docs
description: Bağlı VHD 'leri içeren depolama kaynaklarını silerken sorun giderme.
services: storage
author: AngshumanNayakMSFT
tags: top-support-issue,azure-service-management
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/11/2019
ms.author: annayak
ms.openlocfilehash: 95c85309058911d6767eb44efd7b37ddac7a9119
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77915046"
---
# <a name="troubleshoot-classic-storage-resource-deletion-errors"></a>Klasik depolama kaynağı silme hatalarıyla ilgili sorunları giderme
Bu makalede, Azure klasik depolama hesabı, kapsayıcı veya *. vhd Sayfa Blobu dosyası silinmeye çalışılırken aşağıdaki hatalardan biri oluştuğunda sorun giderme kılavuzu sunulmaktadır. 


Bu makale yalnızca klasik depolama kaynaklarıyla ilgili sorunları ele alır. Bir Kullanıcı Azure portal, PowerShell veya CLı kullanarak klasik bir sanal makineyi silerse, diskler otomatik olarak silinmez. Kullanıcı "disk" kaynağını silme seçeneğini alır. Seçeneğin seçili olmaması durumunda, "disk" kaynağı depolama hesabının, kapsayıcının ve gerçek *. vhd sayfası blob dosyasının silinmesini engeller.

Azure diskleri hakkında daha fazla bilgiyi [burada](../../virtual-machines/windows/managed-disks-overview.md)bulabilirsiniz. Azure, bir VM 'ye bağlı bir diskin bozulmasını engellemek için silinmesini engeller. Ayrıca, bir VM 'ye bağlı bir Sayfa Blobu olan kapsayıcıların ve depolama hesaplarının silinmesini engeller. 

## <a name="what-is-a-disk"></a>"Disk" nedir?
Bir "disk" kaynağı bir sanal makineye bir işletim sistemi diski veya veri diski olarak bir *. vhd Sayfa Blobu dosyası bağlamak için kullanılır. Bir işletim sistemi diski veya veri diski kaynağı silinene kadar, *. vhd dosyasında bir kira tutmaya devam edecektir. Bir "disk" kaynağı işaret ediyorsa, aşağıdaki resimde gösterilen yoldaki herhangi bir depolama kaynağı silinemez.

![Disk (klasik) "özellik" bölmesi açık olan portalın ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/Disk_Lease_Illustration.jpg) 


## <a name="steps-while-deleting-a-classic-virtual-machine"></a>Klasik bir sanal makineyi silme adımları 

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


1. Klasik sanal makineyi silin.
2. "Diskler" onay kutusu işaretliyse, blob *. vhd ile ilişkili **disk kirası** (yukarıdaki görüntüde gösterilir) bozulur. Gerçek Sayfa Blobu *. vhd dosyası depolama hesabında yine de mevcut olacaktır.
Portalın ekran görüntüsünü ![sanal makine (klasik) "silme" hata bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/steps_while_deleting_classic_vm.jpg) 

3. Disk kirası bozulur, sayfa Blobun kendisi silinebilir. Bir depolama hesabı veya kapsayıcısı, içindeki tüm "disk" kaynakları silindiğinde silinebilir.

>[!NOTE] 
>Kullanıcı VM 'yi silerse ancak VHD 'yi değil, depolama ücretleri Sayfa Blobu *. vhd dosyasında tahakkuk etmeye devam edecektir. Ücretler, depolama hesabı türü ile birlikte olacaktır, daha fazla ayrıntı için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/) bakın. Kullanıcı artık VHD 'leri kullanmayı amaçlamadıysanız, gelecekteki ücretlerden kaçınmak için bu dosyayı silin. 

## <a name="unable-to-delete-storage-account"></a>Depolama hesabı silinemiyor 

Kullanıcı artık gerekmeyen klasik bir depolama hesabını silmeye çalıştığında, Kullanıcı aşağıdaki davranışı görebilir.

#### <a name="azure-portal"></a>Azure portalı 
Kullanıcı [Azure Portal](https://portal.azure.com) klasik depolama hesabına gider ve **Sil**' e tıkladıktan sonra Kullanıcı şu iletiyi görür: 

Bir sanal makineye disk (ler) "eklenmiş"

![Sanal makine (klasik) "Sil" hata bölmesi açık olan portalın ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_attached_portal.jpg) 


Bir sanal makineye "iliştirilmemiş" disk (ler) i

![Sanal makine (klasik) "Sil" hata olmayan bölmesi açık olan portalın ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_unattached_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Kullanıcı, klasik PowerShell cmdlet 'lerini kullanarak artık kullanılmayan bir depolama hesabını silmeye çalışır. Kullanıcı şu iletiyi görür:

> <span style="color:cyan">**Remove-Azurestorampahesabı-StorageAccountName myclassicaccount**</span>
> 
> <span style="color:red">Remove-Azurestonetgeaccount: Rozrequest: depolama hesabı myclassicaccount içinde bazı etkin görüntü (ler) ve/veya disk, ör.  
> myclassicaccount. Bu depolama hesabını silmeden önce bu yansımanın ve/veya disklerin kaldırıldığından emin olun.</span>

## <a name="unable-to-delete-storage-container"></a>Depolama kapsayıcısı silinemiyor

Kullanıcı artık gerekmeyen bir klasik Depolama Blobu kapsayıcısını silmeye çalıştığında, Kullanıcı aşağıdaki davranışı görebilir.

#### <a name="azure-portal"></a>Azure portalı 
Azure portal, kapsayıcıda bir *. vhd sayfa blob dosyasını işaret eden "disk (ler)" kirası varsa, kullanıcının kapsayıcıyı silmesine izin vermez. Bu, diskler üzerinde kira ile VHD (ler) bir dosyayı yanlışlıkla silinmeye engel olmak için tasarımdır. 

![Depolama kapsayıcısı "liste" bölmesi açık olan portalın ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_container_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Kullanıcı PowerShell 'i kullanarak silmeyi seçerse, bu hata aşağıdaki hataya neden olur. 

> <span style="color:cyan">**Remove-Azurestoraygecontainer-Context $context-ad VHD 'leri**</span>
> 
> <span style="color:red">Remove-Azurestoraygecontainer: uzak sunucu bir hata döndürdü: (412) kapsayıcıda bir kira var ve istekte hiçbir kira KIMLIĞI belirtilmemiş.. HTTP durum kodu: 412-HTTP hata Iletisi: Şu anda kapsayıcıda bir kira var ve istekte hiçbir kira KIMLIĞI belirtilmemiş.</span>

## <a name="unable-to-delete-a-vhd"></a>Bir VHD silinemiyor 

Azure sanal makinesini sildikten sonra, Kullanıcı VHD dosyasını (Sayfa Blobu) silmeye çalışır ve aşağıdaki iletiyi alır:

#### <a name="azure-portal"></a>Azure portalı 
Portalda, silinmek üzere seçilen blob 'ların listesine bağlı olarak iki deneyim olabilir.

1. Yalnızca "kiralanan" blob 'lar seçilirse Sil düğmesi gösterilmez.
kapsayıcı blobu "liste" bölmesi açık olan portalın ekran görüntüsünü ![](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_portal.jpg)


2. "Kiralık" ve "kullanılabilir" bloblarının bir karışımı seçilirse, "Sil" düğmesi görüntülenir. Ancak "Sil" işlemi, üzerinde disk kirası olan sayfa bloblarının arkasında kalacak. 
Portalın ekran görüntüsünü ![kapsayıcı blobu "liste" bölmesi](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_1.jpg)
açık olan seçili blob "Sil" bölmesi açık olan portalın ekran görüntüsü ![açın](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_2.jpg)

#### <a name="azure-powershell"></a>Azure PowerShell 
Kullanıcı PowerShell 'i kullanarak silmeyi seçerse, bu hata aşağıdaki hataya neden olur. 

> <span style="color:cyan">**Remove-Azurestooygeblob-Context $context-Container VHD 'ler-blob "classicvm-OS-8698. vhd"** </span>
> 
> <span style="color:red">Remove-Azurestoraygeblob: uzak sunucu bir hata döndürdü: (412) blob üzerinde şu anda bir kira var ve istekte hiçbir kira KIMLIĞI belirtilmemiş.. HTTP durum kodu: 412-HTTP hata Iletisi: Şu anda blob üzerinde bir kira var ve istekte hiçbir kira KIMLIĞI belirtilmemiş.</span>


## <a name="resolution-steps"></a>Çözüm adımları

### <a name="to-remove-classic-disks"></a>Klasik diskleri kaldırmak için
Azure portal şu adımları uygulayın:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  Disklere (klasik) gidin. 
3.  Diskler sekmesine tıklayın. portalın ekran görüntüsünü ![kapsayıcı blobu "liste" bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_disks_tab.jpg)
 
4.  Veri diskinizi seçin ve Diski Sil’e tıklayın.
 kapsayıcı blobu "liste" bölmesi açık olan portalın ekran görüntüsünü ![](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_delete_disk.jpg)
 
5.  Daha önce başarısız olan silme işlemini yeniden deneyin.
6.  Bir depolama hesabı veya kapsayıcısı, tek bir diski olduğu sürece silinemez.

### <a name="to-remove-classic-images"></a>Klasik görüntüleri kaldırmak için   
Azure portal şu adımları uygulayın:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  İşletim sistemi görüntüleri (klasik) sayfasına gidin.
3.  Görüntüyü silin.
4.  Daha önce başarısız olan silme işlemini yeniden deneyin.
5.  Tek bir görüntüsü olduğu sürece bir depolama hesabı veya kapsayıcısı silinemez.
