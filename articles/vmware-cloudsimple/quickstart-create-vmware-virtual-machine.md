---
title: CloudSimple hızlı başlangıç ile Azure VMware çözümü-Azure 'da VMware VM 'lerini kullanma
description: Bu hızlı başlangıçta, CloudSimple tarafından Azure VMware çözümünü kullanarak Azure portal VMware VM 'lerini yapılandırma ve kullanma hakkında bilgi edineceksiniz.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dd4faf6df54d478654c59ffc18bf8c5873d576b9
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816663"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Hızlı Başlangıç: Azure'da VMware sanal makinelerini kullanma

Azure portal bir sanal makine oluşturmak için özel bulut vCenter 'unuzda bulunan sanal makine şablonlarını kullanabilirsiniz. VCenter Yöneticisi özel bulutta ek şablonlar oluşturabilir.

## <a name="create-a-vm-template"></a>VM şablonu oluşturma

İlk olarak, vCenter Kullanıcı arabirimini kullanarak özel bulutunuzda bir sanal makine oluşturun. Bir şablon oluşturmak için, VMware makalesindeki yönergeleri izleyerek [sanal makineyi vSphere Web istemcisinde bir şablona kopyalayın](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). VM şablonunu özel bulut vCenter 'unuzda depolayın.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portal bir sanal makine oluşturun

1. **Tüm Hizmetler**’i seçin.

2. **Cloudsimple sanal makineleri**için arama yapın.

3. **Add (Ekle)** seçeneğini belirleyin.

    ![Ekle 'yi seçin](media/create-cloudsimple-virtual-machine.png)

4. Sanal makineyle ilgili olarak aşağıdaki bilgileri girin ve ardından İleri ' **yi seçin: Boyut**.

    ![CloudSimple sanal makinesi oluşturma-temel kavramlar](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | **Abonelik** | Özel bulutunuz ile ilişkili Azure aboneliği.  |
    | **Kaynak grubu** | VM 'nin atanacağı kaynak grubu. Var olan bir grubu seçebilir veya yeni bir grup oluşturabilirsiniz. |
    | **Name** | VM 'yi tanımlayacak bir ad.  |
    | **Location** | VM 'nin barındırıldığı Azure bölgesi.  |
    | **Özel bulut** | Sanal makineyi oluşturmak istediğiniz CloudSimple özel bulutu. |
    | **ResourcePool** | VM için eşlenmiş bir kaynak havuzu. Kullanılabilir kaynak havuzlarından seçin. |
    | **vSphere şablonu** | VM için vSphere şablonu.  |
    | **Kullanıcı adı** | VM yöneticisinin Kullanıcı adı (Windows şablonları için).|
    | **Parola** |  VM yöneticisinin parolası (Windows şablonları için). |
    | **Parolayı onayla** | Önceki alanda belirtilen parola. |

5. Sanal makine için çekirdek sayısını ve bellek kapasitesini seçin. Tam CPU sanallaştırmayı Konuk işletim sistemine sunmak istiyorsanız Konuk işletim sistemi **olarak** Seç ' i seçin. Donanım sanallaştırma gerektiren uygulamalar, ikili çeviri veya paravirtualization olmadan sanal makinelerde çalıştırılabilir. Daha fazla bilgi için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">VMware donanım yardımlı sanallaştırma 'Yi kullanıma</a>sunma VMware makalesine bakın. İşiniz bittiğinde ileri ' yi seçin **: Yapılandırma**.

    ![CloudSimple sanal makinesi oluşturma-boyut](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimlerini ve diskleri aşağıdaki tablolarda açıklandığı şekilde yapılandırın ve ardından **gözden geçir + oluştur**' u seçin.

    ![CloudSimple sanal makinesi oluşturma-yapılandırma](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için, **ağ arabirimi Ekle** ' yi seçin ve ardından aşağıdaki ayarları yapılandırın:
    
    | Ayar | Açıklama |
    | ------------ | ------------- |
    | **Name** | Arabirimi tanımlamak için bir ad girin.  |
    | **Ağ** | Özel bulut vSphere 'unuzda yapılandırılmış dağıtılmış bağlantı noktası grupları listesinden seçim yapın.  |
    | **Örünü** | VM için yapılandırılmış kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz. <a href="https://kb.vmware.com/s/article/1001805" target="_blank">sanal makineniz için ağ bağdaştırıcısı seçme</a>VMware makalesi. |
    | **Önyüklemede açma** | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan olarak **etkinleştirilir**. |

    Diskler için, **Disk Ekle** ' yi seçin ve ardından aşağıdaki ayarları yapılandırın:

    | Ayar | Açıklama |
    | ------------ | ------------- |
    | **Name** | Diski tanımlamak için bir ad girin.  |
    | **Boyut** | Kullanılabilir boyutlardan birini seçin.  |
    | **SCSI denetleyicisi** | Disk için bir SCSI denetleyicisi seçin.  |
    | **Modundaysa** | Modu, diskin anlık görüntülerle nasıl katıldığını belirtir. Bu seçeneklerden birini seçin: <br> **Bağımsız kalıcı**: Diske yazılan tüm değişiklikler kalıcı olarak yazılmıştır.<br> **Bağımsız olarak kalıcı olmayan**: Sanal makineyi kapattığınızda veya sıfırladığınızda diske yazılan değişiklikler atılır. Bağımsız olarak kalıcı olmayan bağımsız mod, sanal makineyi her zaman aynı durumda yeniden başlatmanıza olanak tanır. Daha fazla bilgi için bkz. <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware belgeleri</a>.

7. Doğrulama tamamlandıktan sonra ayarları gözden geçirin ve **Oluştur**' u seçin. Değişiklik yapmak için üstteki sekmeleri seçin veya öncekini seçin **: Yapılandırma**.

    ![CloudSimple sanal makinesi oluşturma-Inceleme ve oluşturma](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple sanal makinelerinin listesini görüntüle](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Azure 'dan CloudSimple sanal makinelerini yönetme](https://docs.azure.cloudsimple.com/azure-manage-vm/)
