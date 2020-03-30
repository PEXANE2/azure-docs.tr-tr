---
title: Azure'daki Linux VM'lerde depolama kaynağı silme hatalarını giderme | Microsoft Dokümanlar
description: Ekli VHD'ler içeren depolama kaynaklarını silerken sorunları nasıl giderilir?
keywords: ''
services: virtual-machines
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 50ab4b0f1e676ffcba0ce69ab6aa957e4c77ab88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058165"
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Depolama kaynağı silme hatalarını giderme

Bazı senaryolarda, Azure Kaynak Yöneticisi dağıtımında bir Azure depolama hesabını, kapsayıcısını veya blob'u silmeye çalışırken aşağıdaki hatalardan biriyle karşılaşabilirsiniz:

> **Depolama hesabı 'StorageAccountName' silinemedi. Hata: Depolama hesabı, yapıtlarının kullanımda olması nedeniyle silinemez.**
> 
> **# konteyner(ler) dışında #<br>silmek için başarısız oldu): vhds: Şu anda konteyner üzerinde bir kira ve hiçbir kira kimliği istek belirtilmiştir.**
> 
> **# blobs # silmek<br>için başarısız: BlobName.vhd: Şu anda blob bir kira ve hiçbir kira kimliği isteği belirtilmiştir.**

Azure VM'lerinde kullanılan VHD'ler, Azure'daki standart veya premium depolama hesabında sayfa blobolarak depolanan .vhd dosyalarıdır. Azure diskleri hakkında daha fazla bilgi için [yönetilen disklere Giriş'e](../linux/managed-disks-overview.md)bakın.

Azure, bozulmayı önlemek için VM'ye bağlı bir diskin silinmesini önler. Ayrıca, VM'ye bağlı bir sayfa blob'u olan kapsayıcıların ve depolama hesaplarının silinmesini de önler. 

Bu hatalardan birini alırken bir depolama hesabı, kapsayıcı veya blob silme işlemi: 
1. VM'ye bağlı lekeleri belirleme
2. [Bağlı işletim sistemi **diskiyle** VM'leri silme](#step-2-delete-vm-to-detach-os-disk)
3. [Tüm veri **disk(leri)** kalan VM(ler)den ayırın](#step-3-detach-data-disk-from-the-vm)

Bu adımlar tamamlandıktan sonra depolama hesabını, kapsayıcıyı veya blob'u yeniden silmeyi deneyin.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>Adım 1: VM'ye bağlı blob'u tanımlayın

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Senaryo 1: Bir blob silme – ekli VM tanımlamak
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. Depolama hesabına gidin, **Blob Service** altında **Kapsayıcılar**seçin ve silmek için blob gidin.
3. Blob **Lease State** **Kiralanmışsa,** Blob meta veri bölmesini açmak için Meta **Verilerini Edit'i** sağ tıklatın ve seçin. 

    ![Portalın ekran görüntüsü, Depolama hesabı lekeleri ve sağ tıklayın > "Meta Verileri Edit" vurgulanır](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Blob meta veri bölmesinde, **MicrosoftAzureCompute_VMName**değerini denetleyin ve kaydedin. Bu değer, VHD'nin bağlı olduğu VM'nin adıdır. (Bu alan yoksa **bkz.**
5. Blob meta veri bölmesinde, **MicrosoftAzureCompute_DiskType**değerini denetleyin ve kaydedin. Bu değer, ekteki diskin işletim sistemi veya veri diski olup olmadığını tanımlar (Bu alan yoksa **bkz.** 

     !["Blob Metadata" bölmesi açık olan portalın ekran görüntüsü](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Blob disk türü **OSDisk** ise [Adım 2 izleyin: OS diski ayırmak için VM'yi silin.](#step-2-delete-vm-to-detach-os-disk) Aksi takdirde, blob disk türü **DataDisk** ise Adım 3'teki adımları [izleyin: Veri diskini VM'den ayırın.](#step-3-detach-data-disk-from-the-vm) 

> [!IMPORTANT]
> **MicrosoftAzureCompute_VMName** ve **MicrosoftAzureCompute_DiskType** blob meta verilerinde görünmüyorsa, blob'un açıkça kiralanmış olduğunu ve VM'ye bağlı olmadığını gösterir. Kiralanan lekeler önce kirayı bozmadan silinemez. Kirayı kırmak için blob'a sağ tıklayın ve **kirayı kır'ı**seçin. VM'ye bağlı olmayan kiralık lekeler, blob'un silinmesini engeller, ancak kapsayıcının veya depolama hesabının silinmesini engellemez.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Senaryo 2: Bir kapsayıcıyı silme - VM'lere bağlı kapsayıcıiçindeki tüm blob(lar)ı tanımlayın
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. **Blob Service** altında depolama hesabına gidin **Kapsayıcılar'ı**seçin ve silinecek kapsayıcıyı bulun.
3. Kapsayıcıyı açmak için tıklatın ve içindeki lekelerin listesi görünür. Blob Type = **Sayfa blob** ve Lease State = Bu listeden **kiralanan** tüm lekeleri tanımlayın. Bu lekelerin her biriyle ilişkili VM'yi tanımlamak için Senaryo 1'i izleyin.

    ![Portalın ekran görüntüsü, Depolama hesabı lekeleri ve "Lease State" ile "Kiralık" vurgulanan](./media/troubleshoot-vhds/utd-disks-sm.png)

4. **OSDisk** ile VM(ler)'i silmek ve **DataDisk'i**ayırmak için [Adım 2](#step-2-delete-vm-to-detach-os-disk) ve [Adım 3'ü](#step-3-detach-data-disk-from-the-vm) izleyin. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Senaryo 3: Depolama hesabını silme - VM'lere bağlı depolama hesabındaki tüm blob(lar)ı tanımlayın
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. **Blob Service** altında depo hesabına gidin **Blobs**seçin.
3. **Kapsayıcılar** bölmesinde, Kira **Durumu** **Kiralanan** tüm kapsayıcıları **tanımlayın** ve kiralanan her kapsayıcı için [Senaryo 2'yi](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) izleyin.
4. **OSDisk** ile VM(ler)'i silmek ve **DataDisk'i**ayırmak için [Adım 2](#step-2-delete-vm-to-detach-os-disk) ve [Adım 3'ü](#step-3-detach-data-disk-from-the-vm) izleyin. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>Adım 2: OS diskini ayırmak için VM'yi silme
VHD bir işletim sistemi diskiyse, ekteki VHD silinemeden önce VM'yi silmeniz gerekir. Bu adımlar tamamlandıktan sonra aynı VM'ye iliştirilen veri diskleri için ek bir eylem gerekmez:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Hub menüsünde **Sanal Makineler'i**seçin.
3. VHD'nin bağlı olduğu VM'yi seçin.
4. Sanal makineyi hiçbir şeyin etkin olarak kullanmadığından ve artık sanal makineye ihtiyacınız olmadığından emin olun.
5. **Sanal Makine ayrıntıları** bölmesinin üst kısmında Sil'i seçin ve onaylamak için **Evet'i** tıklatın. **Delete**
6. VM silinmelidir, ancak VHD korunabilir. Ancak, VHD artık bir VM'ye bağlı olmamalı veya üzerinde bir kira sözleşmesi olmamalıdır. Kira sözleşmesinin serbest bırakılması birkaç dakika sürebilir. Kiranın serbest bırakıldığını doğrulamak için blob konumuna ve **Blob özellikleri** bölmesine göz atın, **Kira Durumu** **Kullanılabilir**olmalıdır.

## <a name="step-3-detach-data-disk-from-the-vm"></a>Adım 3: Veri diskini VM'den ayırma
VHD bir veri diskiyse, kirayı kaldırmak için VHD'yi VM'den ayırın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Hub menüsünde **Sanal Makineler'i**seçin.
3. VHD'nin bağlı olduğu VM'yi seçin.
4. **Sanal Makine ayrıntıları** bölmesindeki **Diskler'i** seçin.
5. VHD'nin bağlı olduğu silinecek veri diskini seçin. VHD'nin URL'sini işaretleyerek diske hangi blob'un eklenmiş olduğunu belirleyebilirsiniz.
6. **VHD URI** alanında yolu kontrol etmek için diske tıklayarak blob konumunu doğrulayabilirsiniz.
7. **Diskler** bölmesinin üstünde **ki Edit'i** seçin.
8. Silinmek üzere veri diskinin **ayırsimgesine** tıklayın.

     !["Blob Metadata" bölmesi açık olan portalın ekran görüntüsü](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. **Kaydet'i**seçin. Disk artık VM'den ayrılmıştır ve VHD artık kiralanmış değildir. Kira sözleşmesinin serbest bırakılması birkaç dakika sürebilir. Kiranın serbest bırakıldığını doğrulamak için, blob konumuna ve **Blob özellikleri** bölmesine göz atın, **Kira Durumu** değeri **Kilidi** veya **Kullanılabilir**olmalıdır.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm

