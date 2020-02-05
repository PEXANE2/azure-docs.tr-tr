---
title: Azure VMware çözümleri (AVS)-VM şablonlarıyla Azure 'da sanal makine oluşturma
description: Azure 'da, AVS özel bulutunuz için VMware altyapısında VM şablonları kullanarak sanal makinelerin nasıl oluşturulacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 533aaab13f1b957e709f66b23b511fc199ee0285
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015210"
---
# <a name="create-a-virtual-machine-in-azure-using-vm-templates-on-the-vmware-infrastructure"></a>VMware altyapısında VM şablonları kullanarak Azure 'da sanal makine oluşturma

Bir sanal makineyi Azure portal, AVS yöneticinizin aboneliğiniz için etkinleştirildiği VMware altyapısında VM şablonlarını kullanarak oluşturabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-avs-virtual-machine"></a>AVS sanal makinesi oluştur

1. **Tüm Hizmetler**’i seçin.

2. **AVS sanal makineleri**için arama yapın.

3. **Ekle**'ye tıklayın.

    ![AVS sanal makinesi oluştur](media/create-cloudsimple-virtual-machine.png)

4. Temel bilgileri girin **İleri: boyut**' a tıklayın.

    > [!NOTE]
    > Azure 'da AVS sanal makinesi oluşturma bir VM şablonu gerektirir. Bu VM şablonu, özel bulut vCenter 'unuzda bulunmalıdır. Özel bulutunuzda, istenen işletim sistemi ve yapılandırmalara sahip vCenter kullanıcı arabiriminden bir sanal makine oluşturun. [Sanal makineyi vSphere Web istemcisindeki bir şablona kopyalama](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE_copy.html)bölümündeki yönergeleri kullanarak bir şablon oluşturun.

    ![AVS sanal makinesi oluşturma-temel bilgiler](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | Abonelik | Özel bulutunuz ile ilişkili Azure aboneliği.  |
    | Kaynak Grubu | VM 'nin atanacağı kaynak grubu. Var olan bir grubu seçebilir veya yeni bir grup oluşturabilirsiniz. |
    | Ad | VM 'yi tanımlayacak ad.  |
    | Konum | Bu VM 'nin barındırıldığı Azure bölgesi.  |
    | Özel Bulut | Sanal makineyi oluşturmak istediğiniz, AVS özel bulutu. |
    | Kaynak havuzu | VM için eşlenen kaynak havuzu. Kullanılabilir kaynak havuzlarından seçin. |
    | vSphere şablonu | VM için vSphere şablonu.  |
    | Kullanıcı adı | VM yöneticisinin Kullanıcı adı (Windows şablonları için)|
    | Parola <br>Parolayı onayla | VM yöneticisinin parolası (Windows şablonları için).  |

5. VM için çekirdek sayısını ve bellek kapasitesini seçip **İleri: konfigürasyonlar**' ı tıklatın. Konuk işletim sistemine tam CPU sanallaştırmayı göstermek istiyorsanız onay kutusunu işaretleyin. böylece, donanım sanallaştırma gerektiren uygulamalar ikili çeviri veya paravirtualization olmadan sanal makinelerde çalıştırılabilir. Daha fazla bilgi için [VMware donanım yardımlı sanallaştırma 'Yi kullanıma](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html)sunma VMware makalesine bakın.

    ![AVS sanal makinesi oluşturma-boyut](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimlerini ve diskleri aşağıdaki tablolarda açıklandığı şekilde yapılandırın ve **gözden geçir + oluştur**' a tıklayın.

    ![AVS sanal makinesi oluşturma-yapılandırma](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için, **ağ arabirimi Ekle** ' ye tıklayın ve aşağıdaki ayarları yapılandırın.

    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Ad | Arabirimi tanımlamak için bir ad girin.  |
    | Ağ | Özel bulut vSphere 'unuzda yapılandırılmış dağıtılmış bağlantı noktası grubu listesinden seçin.  |
    | Örünü | VM için yapılandırılmış kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz. [sanal makineniz için bir ağ bağdaştırıcısı seçme](https://kb.vmware.com/s/article/1001805)VMware Bilgi Bankası makalesi. |
    | Önyüklemede açma | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan olarak **etkinleştirilir**. |

    Diskler için, **Disk Ekle** ' ye tıklayın ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama |
    | ------------ | ------------- |
    | Ad | Diski tanımlamak için bir ad girin.  |
    | Boyut | Kullanılabilir boyutlardan birini seçin.  |
    | SCSI denetleyicisi | Disk için bir SCSI denetleyicisi seçin.  |
    | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Bu seçeneklerden birini seçin: <br> -Bağımsız kalıcı: diske yazılan tüm veriler kalıcı olarak yazılmıştır.<br> -Bağımsız kalıcı olmayan: sanal makineyi kapatıp sıfırladığınızda diske yazılan değişiklikler atılır. Bağımsız olarak kalıcı olmayan bağımsız mod, sanal makineyi her zaman aynı durumda yeniden başlatmanıza olanak tanır. Daha fazla bilgi için bkz. [VMware belgeleri](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html).

7. Doğrulama tamamlandıktan sonra ayarları gözden geçirin ve **Oluştur**' a tıklayın. Herhangi bir değişiklik yapmak için üstteki sekmeye tıklayın veya tıklayın.

    ![AVS sanal makinesi oluşturma-gözden geçirme](media/create-cloudsimple-virtual-machine-review.png)

## <a name="view-list-of-avs-virtual-machines"></a>AVS sanal makinelerinin listesini görüntüle

1. **Tüm Hizmetler**’i seçin.

2. **AVS sanal makineleri**için arama yapın.

3. Özel bulutunuzun oluşturulduğu ' ı seçin.

    ![AVS sanal makinelerinin listesi](media/list-cloudsimple-virtual-machines.png)

AVS sanal makinelerinin listesi, Azure portal oluşturulan sanal makineleri içerir.  Eşlenmiş vCenter kaynak havuzundaki özel bulut vCenter üzerinde oluşturulan sanal makineler listede gösterilir.  
