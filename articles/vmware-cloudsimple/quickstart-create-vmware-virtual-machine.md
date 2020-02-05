---
title: Azure VMware Solutions (AVS) hızlı başlangıç-Azure 'da VMware VM 'Leri oluşturma
description: Azure VMware çözümlerini (AVS) kullanarak Azure portal VMware VM 'lerini oluşturma ve yapılandırma hakkında bilgi edinin
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 575d59d60ebfcfdbe4d234d608e8d988006773c2
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019562"
---
# <a name="quickstart---create-vmware-vms-on-azure"></a>Hızlı başlangıç-Azure 'da VMware VM 'Leri oluşturma

Azure portal bir sanal makine oluşturmak için, AVS yöneticinizin aboneliğiniz için etkinleştirildiği sanal makine şablonlarını kullanın. VM şablonları VMware altyapısında bulunur.

## <a name="avs-vm-creation-on-azure-requires-a-vm-template"></a>Azure 'da AVS VM oluşturma VM şablonu gerektirir

VCenter kullanıcı arabiriminden, AVS özel bulutunuzda bir sanal makine oluşturun. Bir şablon oluşturmak için, [sanal makineyi vSphere Web istemcisinde bir şablona kopyalama](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html)bölümündeki yönergeleri izleyin. VM şablonunu AVS özel bulut vCenter 'unuzda depolayın.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portal bir sanal makine oluşturun

1. **Tüm Hizmetler**’i seçin.

2. **AVS sanal makineleri**için arama yapın.

3. **Ekle**'ye tıklayın.

    ![AVS sanal makinesi oluştur](media/create-cloudsimple-virtual-machine.png)

4. Temel bilgileri girin ve **İleri: boyut**' a tıklayın.

    ![AVS sanal makinesi oluşturma-temel bilgiler](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | Abonelik | AVS özel bulutunuz ile ilişkili Azure aboneliği. |
    | Kaynak Grubu | VM 'nin atanacağı kaynak grubu. Var olan bir grubu seçebilir veya yeni bir grup oluşturabilirsiniz. |
    | Ad | VM 'yi tanımlayacak ad.  |
    | Konum | Bu VM 'nin barındırıldığı Azure bölgesi.  |
    | AVS özel bulutu | Sanal makineyi oluşturmak istediğiniz, AVS özel bulutu. |
    | Kaynak havuzu | VM için eşlenen kaynak havuzu. Kullanılabilir kaynak havuzlarından seçin. |
    | vSphere şablonu | VM için vSphere şablonu.  |
    | Kullanıcı adı | VM yöneticisinin Kullanıcı adı (Windows şablonları için).|
    | Parola |  VM yöneticisinin parolası (Windows şablonları için). |
    | Parolayı onayla | Parolayı onaylayın. |

5. VM için çekirdek sayısını ve bellek kapasitesini seçip **İleri: konfigürasyonlar**' ı tıklatın. Konuk işletim sistemine tam CPU sanallaştırmayı göstermek istiyorsanız onay kutusunu seçin. Donanım sanallaştırma gerektiren uygulamalar, ikili çeviri veya paravirtualization olmadan sanal makinelerde çalıştırılabilir. Daha fazla bilgi için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">VMware donanım yardımlı sanallaştırma 'Yi kullanıma</a>sunma VMware makalesine bakın.

    ![AVS sanal makinesi oluşturma-boyut](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimlerini ve diskleri aşağıdaki tablolarda açıklandığı şekilde yapılandırın ve **gözden geçir + oluştur**' a tıklayın.

    ![AVS sanal makinesi oluşturma-yapılandırma](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için, **ağ arabirimi Ekle** ' ye tıklayın ve aşağıdaki ayarları yapılandırın.

    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Ad | Arabirimi tanımlamak için bir ad girin.  |
    | Ağ | AVS özel bulut vSphere 'unuzda yapılandırılmış dağıtılmış bağlantı noktası grubu listesinden seçim yapın. |
    | Örünü | VM için yapılandırılmış kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz. <a href="https://kb.vmware.com/s/article/1001805" target="_blank">sanal makineniz için bir ağ bağdaştırıcısı seçme</a>VMware Bilgi Bankası makalesi. |
    | Önyüklemede açma | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan olarak **etkinleştirilir**. |

    Diskler için, **Disk Ekle** ' ye tıklayın ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama |
    | ------------ | ------------- |
    | Ad | Diski tanımlamak için bir ad girin. |
    | Boyut | Kullanılabilir boyutlardan birini seçin. |
    | SCSI denetleyicisi | Disk için bir SCSI denetleyicisi seçin. |
    | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Bu seçeneklerden birini seçin: <br> -Bağımsız kalıcı: diske yazılan tüm veriler kalıcı olarak yazılmıştır.<br> -Bağımsız kalıcı olmayan: sanal makineyi kapatıp sıfırladığınızda diske yazılan değişiklikler atılır. Bağımsız olarak kalıcı olmayan bağımsız mod, sanal makineyi her zaman aynı durumda yeniden başlatmanıza olanak tanır. Daha fazla bilgi için bkz. <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware belgeleri</a>.

7. Doğrulama tamamlandığında ayarları gözden geçirin ve **Oluştur**' a tıklayın. Herhangi bir değişiklik yapmak için üstteki sekmeye tıklayın veya tıklayın.

    ![AVS sanal makinesi oluşturma-gözden geçirme](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AVS sanal makinelerinin listesini görüntüle](azure-create-vm.md#view-list-of-avs-virtual-machines)
* [AVS sanal makinesini Azure 'dan yönetin](azure-manage-vm.md)
