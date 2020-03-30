---
title: CloudSimple tarafından Azure VMware Çözümü - VM şablonları ile Azure'da sanal bir makine oluşturun
description: CloudSimple Private Cloud'unuzun VMware altyapısındaki VM şablonlarını kullanarak Azure'da sanal makinelerin nasıl oluşturulacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ee3029de9826aee17dc76d0e69f08b3c1068423b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244699"
---
# <a name="create-a-virtual-machine-in-azure-using-vm-templates-on-the-vmware-infrastructure"></a>VMware altyapısındaki VM şablonlarını kullanarak Azure'da sanal bir makine oluşturun

Azure portalında, CloudSimple yöneticinizin aboneliğiniz için etkinleştirdiği VMware altyapısındaki VM şablonlarını kullanarak sanal bir makine oluşturabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-cloudsimple-virtual-machine"></a>CloudSimple sanal makine oluşturma

1. **Tüm Hizmetler**’i seçin.

2. **CloudBasit Sanal Makineleri**Arayın.

3. **Ekle**’ye tıklayın.

    ![CloudSimple sanal makine oluşturma](media/create-cloudsimple-virtual-machine.png)

4. Temel bilgileri girin **Sonraki:Boyut'** u tıklatın.

    > [!NOTE]
    > Azure'da CloudSimple sanal makine oluşturma için bir VM şablonu gerektirir.  Bu VM şablonu Özel Bulut vCenter'ınızda bulunmalıdır.  VCenter UI'nizden özel bulutunuzda istediğiniz işletim sistemi ve yapılandırmalarla sanal bir makine oluşturun.  Sanal [Makineyi vSphere Web İstemci'sinde Şablona Klonlama'daki](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE_copy.html)yönergeleri kullanarak şablon oluşturun.

    ![CloudSimple sanal makine oluşturun - temel bilgiler](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | Abonelik | Özel Bulut'unuzla ilişkili Azure aboneliği.  |
    | Kaynak Grubu | VM'nin atanacağı kaynak grubu. Varolan bir grubu seçebilir veya yeni bir grup oluşturabilirsiniz. |
    | Adı | VM'yi tanımlamak için ad.  |
    | Konum | Bu VM'nin barındırıldığı Azure bölgesi.  |
    | Özel Bulut | Sanal makineyi oluşturmak istediğiniz CloudSimple Private Cloud. |
    | Kaynak Havuzu | VM için eşlenen kaynak havuzu. Kullanılabilir kaynak havuzlarından seçim yapabilirsiniz. |
    | vSphere Şablonu | VM için vSphere şablonu.  |
    | Kullanıcı adı | VM yöneticisinin kullanıcı adı (Windows şablonları için)|
    | Parola <br>Parolayı onayla | VM yöneticisi için parola (Windows şablonları için).  |

5. VM için çekirdek sayısını ve bellek kapasitesini seçin ve **İleri:Yapılandırmalar'ı**tıklatın. Donanım sanallaştırması gerektiren uygulamaların ikili çeviri veya parasanallaştırma olmadan sanal makinelerde çalıştırabilmesi için tam CPU sanallaştırmasını konuk işletim sistemine maruz bırakmak istiyorsanız onay kutusunu seçin. Daha fazla bilgi için VMware [makalesivmware Donanım Destekli Sanallaştırma maruz](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html)bakın.

    ![CloudSimple sanal makine oluşturma - boyut](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimlerini ve diskleri aşağıdaki tablolarda açıklandığı şekilde yapılandırın ve **Gözden Geçir + oluştur'u**tıklatın.

    ![CloudSimple sanal makine oluşturma - yapılandırmaları](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için **ağ arabirimi ekle'yi** tıklatın ve aşağıdaki ayarları yapılandırın.

    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Adı | Arabirimi tanımlamak için bir ad girin.  |
    | Ağ | Özel Bulut vSphere'inizde yapılandırılan dağıtılmış bağlantı noktası grubundan birini seçin.  |
    | Bağdaştırıcı | VM için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için, [sanal makine için bir ağ bağdaştırıcıseçimi seçimi](https://kb.vmware.com/s/article/1001805)nde VMware bilgi bankası makalesine bakın. |
    | Boot'ta güç | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan **enable**olduğunu. |

    Diskler için **disk ekle'yi** tıklatın ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama |
    | ------------ | ------------- |
    | Adı | Diski tanımlamak için bir ad girin.  |
    | Boyut | Kullanılabilir boyutlardan birini seçin.  |
    | SCSI Denetleyici | Disk için bir SCSI denetleyicisi seçin.  |
    | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Bu seçeneklerden birini seçin: <br> - Bağımsız kalıcı: Diske yazılan tüm veriler kalıcı olarak yazılır.<br> - Bağımsız kalıcı olmayan: Sanal makineyi kapattığınızda veya sıyrettiğinizde diske yazılan değişiklikler atılır.  Bağımsız kalıcı olmayan mod, VM'yi her zaman aynı durumda yeniden başlatmanızı sağlar. Daha fazla bilgi için [VMware belgelerine](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html)bakın.

7. Doğrulama tamamlandıktan sonra, ayarları gözden geçirin ve **Oluştur'u**tıklatın. Değişiklik yapmak için, en üstteki sekmeleri tıklatın veya tıklatın.

    ![CloudSimple sanal makine oluşturun - inceleme](media/create-cloudsimple-virtual-machine-review.png)

## <a name="view-list-of-cloudsimple-virtual-machines"></a>CloudSimple sanal makinelerin listesini görüntüleyin

1. **Tüm Hizmetler**’i seçin.

2. **CloudBasit Sanal Makineleri**Arayın.

3. Özel Bulut'unuzun oluşturulduğu alanı seçin.

    ![CloudSimple Sanal Makineler Listesi](media/list-cloudsimple-virtual-machines.png)

CloudSimple sanal makinelerin listesi, Azure portalından oluşturulan sanal makineleri içerir.  Eşlenen vCenter kaynak havuzunda Private Cloud vCenter'da oluşturulan sanal makineler listede gösterilir.  
