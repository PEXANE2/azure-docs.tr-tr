---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/25/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 5104c3b4446a8d3747ce7cc1648ef05dd117eb3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74013788"
---
Depolama Gezgini 1.10.0, kullanıcıların yönetilen diskleri yüklemesini, indirmesini ve kopyalamasını ve anlık görüntü oluşturmasını sağlar. Bu ek özellikler nedeniyle, verileri şirket içinde Azure'a geçirmek ve verileri Azure bölgelerine geçirmek için Depolama Gezgini'ni kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız olacak:
- Bir Azure aboneliği
- Bir veya daha fazla Azure yönetilen disk
- Azure Depolama [Gezgini'nin](https://azure.microsoft.com/features/storage-explorer/) en son sürümü

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

Depolama Gezgini'niz Azure'a bağlı değilse, kaynakları yönetmek için kullanamazsınız. Bu bölüm, Depolama Gezgini'ni kullanarak kaynakları yönetebilmeniz için azure hesabınıza bağlamayı geçer.

1. Azure Depolama Gezgini'ni başlatın ve soldaki **eklenti** simgesine tıklayın.

    ![Eklenti simgesine tıklayın](media/disks-upload-vhd-to-managed-disk-storage-explorer/plug-in-icon.png)

1. **Azure Hesabı Ekle'yi**seçin ve **sonra İleri'yi**tıklatın.

    ![Azure Hesabı ekleme](media/disks-upload-vhd-to-managed-disk-storage-explorer/connect-to-azure.png)

1. Azure **Oturum Aç iletişim** kutusunda, Azure kimlik bilgilerinizi girin.

    ![Oturum açma iletişim kutusunda Azure oturum açma](media/disks-upload-vhd-to-managed-disk-storage-explorer/sign-in.png)

1. Listeden aboneliğinizi seçip **Uygula**’ya tıklayın.

    ![Aboneliğinizi seçme](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-subscription.png)

## <a name="upload-a-managed-disk-from-an-on-prem-vhd"></a>Bir ön-prem VHD'den yönetilen bir disk yükleme

1. Sol bölmede **Diskleri** genişletin ve diskinizi yüklemek istediğiniz kaynak grubunu seçin.

    ![Kaynak grubu 1'i seçin](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-rg1.png)

1. **Yükle'yi**seçin.

    ![Karşıya yükleme seçme](media/disks-upload-vhd-to-managed-disk-storage-explorer/upload-button.png)

1. **Upload VHD'de** kaynağınız VHD'yi, diskin adını, işletim sistemi türünü, diski yüklemek istediğiniz bölgeyi ve hesap türünü belirtin. Bazı bölgelerde Kullanılabilirlik bölgeleri desteklenir, bu bölgeler için seçtiğiniz bir bölge seçebilirsiniz.
1. Diskinizi yüklemeye başlamak için **Oluştur'u** seçin.

    ![VHD iletişim kutusunu yükle](media/disks-upload-vhd-to-managed-disk-storage-explorer/upload-vhd-dialog.png)

1. Yüklemenin durumu artık **Etkinlikler'de**görüntülenir.

    ![Yükleme durumu](media/disks-upload-vhd-to-managed-disk-storage-explorer/activity-uploading.png)

1. Yükleme bittiyse ve diski sağ bölmede **görmüyorsanız, Yenile'yi**seçin.

## <a name="download-a-managed-disk"></a>Yönetilen bir diski indirin

Aşağıdaki adımlar, yönetilen bir diskin prem VHD'ye nasıl indirilen ekidir. Bir diskin durumu indirilebilmesi için **eklenmemiş** olmalıdır, **ekli** bir disk indiremezsiniz.

1. Sol bölmede, zaten genişletilmediyse, **Diskleri** genişletin ve diskinizi indirmek istediğiniz kaynak grubunu seçin.

    ![Kaynak grubu 1'i seçin](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-rg1.png)

1. Sağ bölmede, indirmek istediğiniz diski seçin.
1. **İndir'i** seçin ve ardından diski kaydetmek istediğiniz yeri seçin.

    ![Yönetilen bir diski indirin](media/disks-upload-vhd-to-managed-disk-storage-explorer/download-button.png)

1. **Kaydet'i** seçin ve diskiniz indirilmeye başlayacak. İndirme durumu **Etkinlikler'de**görüntülenir.

    ![İndirme durumu](media/disks-upload-vhd-to-managed-disk-storage-explorer/activity-downloading.png)

## <a name="copy-a-managed-disk"></a>Yönetilen diski çoğaltma

Depolama Gezgini ile, bölge içinde veya arasında bir manged disk kopyalayabilirsiniz. Bir diski kopyalamak için:

1. Soldaki **Diskler** açılır bölümünden, kopyalamak istediğiniz diski içeren kaynak grubunu seçin.

    ![Kaynak grubu 1'i seçin](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-rg1.png)

1. Sağ bölmede, kopyalamak istediğiniz diski seçin ve **Kopyala'yı**seçin.

    ![Yönetilen diski çoğaltma](media/disks-upload-vhd-to-managed-disk-storage-explorer/copy-button.png)

1. Sol bölmede, diski yapıştırmak istediğiniz kaynak grubunu seçin.

    ![Kaynak grubu 2'yi seçin](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-rg2.png)

1. Sağ bölmede **Yapıştır'ı** seçin.

    ![Yönetilen bir disk yapıştır](media/disks-upload-vhd-to-managed-disk-storage-explorer/paste-button.png)

1. Disk **yapıştır** iletişim kutusunda, değerleri doldurun. Desteklenen bölgelerde bir Kullanılabilirlik bölgesi de belirtebilirsiniz.

    ![Disk iletişim kutusunu yapıştır](media/disks-upload-vhd-to-managed-disk-storage-explorer/paste-disk-dialog.png)

1. **Yapıştır'ı** seçin ve diskiniz kopyalamaya başlar, durum **Etkinlikler'de**görüntülenir.

    ![Yapıştır durumu kopyala](media/disks-upload-vhd-to-managed-disk-storage-explorer/activity-copying.png)

## <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

1. Soldaki **Diskler** açılır bölümünden, anlık görüntü almak istediğiniz diski içeren kaynak grubunu seçin.

    ![Kaynak grubu 1'i seçin](media/disks-upload-vhd-to-managed-disk-storage-explorer/select-rg1.png)

1. Sağda, anlık görüntü almak istediğiniz diski seçin ve **Anlık Görüntü Oluştur'u**seçin.

    ![Anlık görüntü oluşturma](media/disks-upload-vhd-to-managed-disk-storage-explorer/create-snapshot-button.png)

1. **Anlık Görüntü Oluştur'da,** anlık görüntünün adını ve oluşturmak istediğiniz kaynak grubunu belirtin. Ardından **Oluştur**’u seçin.

    ![Anlık görüntü iletişim kutusu oluşturma](media/disks-upload-vhd-to-managed-disk-storage-explorer/create-snapshot-dialog.png)

1. Anlık görüntü oluşturulduktan sonra, Azure portalındaki anlık görüntügörüntülemek için **Etkinliklerde** **Portal'da Aç'ı** seçebilirsiniz.

    ![Portalda anlık açık](media/disks-upload-vhd-to-managed-disk-storage-explorer/open-in-portal.png)

## <a name="next-steps"></a>Sonraki adımlar
