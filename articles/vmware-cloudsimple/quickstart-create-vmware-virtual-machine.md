---
title: "Hızlı başlangıç: Azure'da VMware VM'leri tüketin"
titleSuffix: Azure VMware Solution by CloudSimple
description: CloudSimple tarafından Azure VMware Solution'ı kullanarak Azure portalından VMware VM'leri nasıl yapılandırıp tükettiğinizi öğrenin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4ab613c251bc43a025e0381046805ec998a04227
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019562"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Quickstart - Azure'da VMware VM'leri tüketin

Azure portalında sanal bir makine oluşturmak için CloudSimple yöneticinizin aboneliğiniz için etkinleştirdiği sanal makine şablonlarını kullanın. VM şablonları VMware altyapısında bulunur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Azure'da CloudSimple VM oluşturma bir VM şablonu gerektirir

vCenter UI'den Özel Bulut'unuzda sanal bir makine oluşturun. Şablon oluşturmak [için, Sanal Makineyi VSphere Web İstemci'deki Şablona Klonlama'daki](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html)yönergeleri izleyin. VM şablonunu Özel Bulut vCenter'ınızda saklayın.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portalda sanal makine oluşturma

1. **Tüm Hizmetler**’i seçin.

2. **CloudBasit Sanal Makineleri**Arayın.

3. **Ekle**’ye tıklayın.

    ![CloudSimple sanal makine oluşturma](media/create-cloudsimple-virtual-machine.png)

4. Temel bilgileri girin ve **İleri:Boyut'u**tıklatın.

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
    | Kullanıcı adı | VM yöneticisinin kullanıcı adı (Windows şablonları için).|
    | Parola |  VM yöneticisi için parola (Windows şablonları için). |
    | Parolayı onayla | Parola’yı onaylayın. |

5. VM için çekirdek sayısını ve bellek kapasitesini seçin ve **İleri:Yapılandırmalar'ı**tıklatın. Tam CPU sanallaştırmasını konuk işletim sistemine maruz bırakmak istiyorsanız onay kutusunu seçin. Donanım sanallaştırması gerektiren uygulamalar ikili çeviri veya parasanallaştırma olmadan sanal makinelerde çalıştırılabilir. Daha fazla bilgi için VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">makalesivmware Donanım Destekli Sanallaştırma maruz</a>bakın.

    ![CloudSimple sanal makine oluşturma - boyut](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimlerini ve diskleri aşağıdaki tablolarda açıklandığı şekilde yapılandırın ve **Gözden Geçir + oluştur'u**tıklatın.

    ![CloudSimple sanal makine oluşturma - yapılandırmaları](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için **ağ arabirimi ekle'yi** tıklatın ve aşağıdaki ayarları yapılandırın.

    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Adı | Arabirimi tanımlamak için bir ad girin.  |
    | Ağ | Özel Bulut vSphere'inizde yapılandırılan dağıtılmış bağlantı noktası grubundan birini seçin.  |
    | Bağdaştırıcı | VM için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için, <a href="https://kb.vmware.com/s/article/1001805" target="_blank">sanal makine için bir ağ bağdaştırıcıseçimi seçimi</a>nde VMware bilgi bankası makalesine bakın. |
    | Boot'ta güç | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan **enable**olduğunu. |

    Diskler için **disk ekle'yi** tıklatın ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama |
    | ------------ | ------------- |
    | Adı | Diski tanımlamak için bir ad girin.  |
    | Boyut | Kullanılabilir boyutlardan birini seçin.  |
    | SCSI Denetleyici | Disk için bir SCSI denetleyicisi seçin.  |
    | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Bu seçeneklerden birini seçin: <br> - Bağımsız kalıcı: Diske yazılan tüm veriler kalıcı olarak yazılır.<br> - Bağımsız kalıcı olmayan: Sanal makineyi kapattığınızda veya sıyrettiğinizde diske yazılan değişiklikler atılır.  Bağımsız kalıcı olmayan mod, VM'yi her zaman aynı durumda yeniden başlatmanızı sağlar. Daha fazla bilgi için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware belgelerine</a>bakın.

7. Doğrulama tamamlandığında, ayarları gözden geçirin ve **Oluştur'u**tıklatın. Değişiklik yapmak için, en üstteki sekmeleri tıklatın veya tıklatın.

    ![CloudSimple sanal makine oluşturun - inceleme](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple sanal makinelerin listesini görüntüleyin](azure-create-vm.md#view-list-of-cloudsimple-virtual-machines)
* [Azure'dan CloudSimple sanal makinesini yönetme](azure-manage-vm.md)
