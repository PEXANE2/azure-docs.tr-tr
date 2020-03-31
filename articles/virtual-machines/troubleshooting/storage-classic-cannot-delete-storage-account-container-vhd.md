---
title: Azure klasik depolama hesaplarını, kapsayıcıları veya VHD'leri sildiğinizde hataları giderme | Microsoft Dokümanlar
description: Ekli VHD'ler içeren depolama kaynaklarını silerken sorunları nasıl giderilir?
services: storage
author: AngshumanNayakMSFT
tags: top-support-issue,azure-service-management
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/11/2019
ms.author: annayak
ms.openlocfilehash: 95c85309058911d6767eb44efd7b37ddac7a9119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77915046"
---
# <a name="troubleshoot-classic-storage-resource-deletion-errors"></a>Sorun giderme klasik depolama kaynağı silme hataları
Bu makalede, aşağıdaki hatalardan biri Azure klasik depolama hesabı, kapsayıcı veya *.vhd sayfa blob dosyasını silmeye çalışırken oluşursa sorun giderme kılavuzu sağlar. 


Bu makalede, yalnızca klasik depolama kaynakları ile ilgili sorunları kapsar. Bir kullanıcı Azure portalı, PowerShell veya CLI'yi kullanarak klasik bir sanal makineyi silerse Diskler otomatik olarak silinmez. Kullanıcı "Disk" kaynağını silme seçeneğine geçer. Seçeneğin seçilmemesi durumunda, "Disk" kaynağı depolama hesabının, kapsayıcının ve gerçek *.vhd sayfa blob dosyasının silinmesini önler.

Azure diskleri hakkında daha fazla bilgiyi [burada](../../virtual-machines/windows/managed-disks-overview.md)bulabilirsiniz. Azure, bozulmayı önlemek için VM'ye bağlı bir diskin silinmesini önler. Ayrıca, VM'ye bağlı bir sayfa blob'u olan kapsayıcıların ve depolama hesaplarının silinmesini de önler. 

## <a name="what-is-a-disk"></a>"Disk" nedir?
Bir "Disk" kaynağı, bir *.vhd sayfa blob dosyasını sanal bir makineye işletim sistemi veya Veri diski olarak takmak için kullanılır. Bir işletim sistemi diski veya Veri diski kaynağı, silinene kadar *.vhd dosyasında kiralama yapmaya devam edecektir. Aşağıdaki resimde gösterilen yoldaki herhangi bir depolama kaynağı, bir "Disk" kaynağı nın işaret etmesi durumunda silinemez.

![Portalın ekran görüntüsü, disk (klasik) "Özellik" bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/Disk_Lease_Illustration.jpg) 


## <a name="steps-while-deleting-a-classic-virtual-machine"></a>Klasik sanal makineyi silerken adımlar 

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


1. Klasik sanal makineyi silin.
2. "Diskler" onay kutusu seçilirse, sayfa blob *.vhd ile ilişkili **disk kira** (yukarıdaki resimde gösterilir) bozulur. Gerçek sayfa blob *.vhd dosyası hala depolama hesabında var olacaktır.
![PortalEkran görüntüsü, sanal makine ile (klasik) "Sil" hata bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/steps_while_deleting_classic_vm.jpg) 

3. Disk(ler) kirası kırıldıktan sonra, sayfa blob(ları) kendisi silinebilir. Bir depolama hesabı veya kapsayıcı, içinde bulunan tüm "Disk" kaynağı silindikten sonra silinebilir.

>[!NOTE] 
>Kullanıcı VM'yi siler, ancak VHD'yi silerse, depolama ücretleri sayfa blob *.vhd dosyasında tahakkuk etmeye devam eder. Ücretler depolama hesabının türüne uygun olacak, daha fazla ayrıntı için [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/storage/) kontrol edin. Kullanıcı artık VHD(ler)i kullanmak niyetinde değilse, gelecekteki ücretlerden kaçınmak için bunları silin. 

## <a name="unable-to-delete-storage-account"></a>Depolama hesabı silinemiyor 

Kullanıcı artık gerekli olmayan klasik bir depolama hesabını silmeye çalıştığında, kullanıcı aşağıdaki davranışı görebilir.

#### <a name="azure-portal"></a>Azure portalında 
Kullanıcı [Azure portalındaki](https://portal.azure.com) klasik depolama hesabına yönlendirir ve **Sil'i**tıklattı, kullanıcı aşağıdaki iletiyi görür: 

Sanal bir makineye "bağlı" disk(ler) ile

![PortalEkran görüntüsü, sanal makine ile (klasik) "Sil" hata bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_attached_portal.jpg) 


Sanal bir makineye disk(ler) "eklenmemiş"

![Sanal makine (klasik) "Sil" hatasız bölme açık ile portalEkran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_storage_account_disks_unattached_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Kullanıcı, klasik PowerShell cmdlets kullanarak artık kullanılmayan bir depolama hesabını silmeye çalışır. Kullanıcı aşağıdaki iletiyi görür:

> <span style="color:cyan">**Remove-AzureStorageAccount -StorageAccountName myclassicaccount**</span>
> 
> <span style="color:red">Remove-AzureStorageAccount : BadRequest: Depolama hesabı myclassicaccount bazı aktif görüntü(ler) ve/ veya disk(ler), örneğin vardır.  
> myclassicaccount. Bu depolama hesabını silmeden önce bu görüntü(ler) ve/veya disk(ler) kaldırıldığından emin olun.</span>

## <a name="unable-to-delete-storage-container"></a>Depolama kapsayıcısı silinemiyor

Kullanıcı artık gerekli olmayan klasik bir depolama blob kapsayıcısını silmeye çalıştığında, kullanıcı aşağıdaki davranışı görebilir.

#### <a name="azure-portal"></a>Azure portalında 
Azure portalı, "Disk(ler)" kirası kapsayıcıdaki *.vhd sayfa blob dosyasını işaret ediyorsa, kullanıcının bir kapsayıcıyı silmesine izin vermez. Disk(ler) kiralama ile bir vhd(ler) dosyasının yanlışlıkla silinmesini önlemek için tasarım gereğidir. 

![Depolama kapsayıcısı "liste" bölmesi açık olan portalın ekran görüntüsü](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_container_portal.jpg)


#### <a name="azure-powershell"></a>Azure PowerShell
Kullanıcı PowerShell kullanarak silmeyi seçerse, aşağıdaki hataya neden olur. 

> <span style="color:cyan">**Remove-AzureStorageContainer -Bağlam $context -Ad vhds**</span>
> 
> <span style="color:red">Remove-AzureStorageContainer : Uzak sunucu bir hata döndürdü: (412) Şu anda kapsayıcıda bir kira sözleşmesi var ve istekte kira kimliği belirtilmedi... HTTP Durum Kodu: 412 - HTTP Hata İletisi: Şu anda kapsayıcıda bir kira sözleşmesi var ve istekte kira kimliği belirtilmedi.</span>

## <a name="unable-to-delete-a-vhd"></a>bir vhd silemiyor 

Azure sanal makinesini sildikten sonra, kullanıcı vhd dosyasını (sayfa blob) silmeye ve aşağıdaki iletiyi almaya çalışır:

#### <a name="azure-portal"></a>Azure portalında 
Portalda, silme için seçilen lekelerin listesine bağlı olarak iki deneyim olabilir.

1. Yalnızca "Kiralık" blob'ları seçilirse, Sil düğmesi görünmüyor.
![PortalEkran görüntüsü, konteyner blob "liste" bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_portal.jpg)


2. "Kiralık" ve "Kullanılabilir" blobs bir karışımı seçilirse, "Sil" düğmesi gösterir. Ancak "Sil" işlemi, üzerinde Disk kirası olan sayfa lekelerini geride bırakır. 
![Portalın ekran görüntüsü, kap blob "liste"](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_1.jpg)
![bölmesi açık Portal Ekran görüntüsü ile, seçilen blob "sil" bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/unable_to_delete_vhd_leased_and_unleased_portal_2.jpg)

#### <a name="azure-powershell"></a>Azure PowerShell 
Kullanıcı PowerShell kullanarak silmeyi seçerse, aşağıdaki hataya neden olur. 

> <span style="color:cyan">**Remove-AzureStorageBlob -Bağlam $context -Konteyner vhds -Blob "classicvm-os-8698.vhd"**</span>
> 
> <span style="color:red">Remove-AzureStorageBlob : Uzak sunucu bir hata döndürdü: (412) Şu anda blob üzerinde bir kira var ve istekte kira kimliği belirtilmedi... HTTP Durum Kodu: 412 - HTTP Hata İletisi: Şu anda blob üzerinde bir kira ve hiçbir kira kimliği istek belirtilmiştir.</span>


## <a name="resolution-steps"></a>Çözüm adımları

### <a name="to-remove-classic-disks"></a>Klasik Diskleri kaldırmak için
Azure portalında aşağıdaki adımları izleyin:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  Diskler(klasik) gidin. 
3.  Diskler sekmesine ![tıklayın.](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_disks_tab.jpg)
 
4.  Veri diskinizi seçin ve Diski Sil’e tıklayın.
 ![PortalEkran görüntüsü, konteyner blob "liste" bölmesi açık](./media/storage-classic-cannot-delete-storage-account-container-vhd/resolution_click_delete_disk.jpg)
 
5.  Daha önce başarısız olan Sil işlemini yeniden deneyin.
6.  Bir depolama hesabı veya kapsayıcı, tek bir Diski olduğu sürece silinemez.

### <a name="to-remove-classic-images"></a>Klasik Görüntüleri kaldırmak için   
Azure portalında aşağıdaki adımları izleyin:
1.  [Azure portalına](https://portal.azure.com) gidin.
2.  İşletim sistemi görüntülerine gidin (klasik).
3.  Resmi silin.
4.  Daha önce başarısız olan Sil işlemini yeniden deneyin.
5.  Bir depolama hesabı veya kapsayıcı, tek bir Görüntüye sahip olduğu sürece silinemez.
