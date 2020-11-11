---
title: Azure Hibrit Avantajı ve Linux VM 'Leri
description: Azure Hibrit Avantajı, Azure üzerinde çalışan Linux sanal makinelerinizde para tasarrufu yapmanıza olanak sağlar.
services: virtual-machines
documentationcenter: ''
author: mathapli
manager: westonh
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/22/2020
ms.author: mathapli
ms.openlocfilehash: 8437c83faf8dfcec0a21add2006b6cf627447dd1
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516450"
---
# <a name="public-preview-azure-hybrid-benefit--how-it-applies-for-linux-virtual-machines"></a>Genel Önizleme: Azure Hibrit Avantajı – nasıl uygulandığı Linux Sanal Makineleri

## <a name="overview"></a>Genel Bakış

Azure Hibrit Avantajı, önceden var olan Red hat veya SUSE yazılım aboneliğinizi kullanarak şirket içi Red Hat Enterprise Linux (RHEL) ve SUSE Linux Enterprise Server (SLES) sanal makinelerinizi (VM) Azure 'a daha kolay bir şekilde geçirebilmesini sağlar. Bu avantajla, yazılım ücreti RHEL veya SLES Aboneliğiniz kapsamında olduğu için yalnızca sanal makinenizin altyapı maliyetleri için ödeme yaparsınız. Bu avantaj, tüm RHEL ve SLES Market (PAYG) görüntüleri için geçerlidir.

> [!IMPORTANT]
> Linux VM 'Leri için Azure Hibrit Avantajı Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="benefit-description"></a>Avantaj açıklaması

Azure Hibrit Avantajı aracılığıyla, şirket içi RHEL ve SLES sunucularınızı, Azure 'da var olan RHEL ve SLES PAYG sanal makinelerini Azure 'a dönüştürerek kendi abonelik (KCG) faturanızı daha kolay hale getirebilirsiniz. Genellikle, Azure 'daki PAYG görüntülerinden dağıtılan VM 'Ler hem bir altyapı ücretini hem de yazılım ücretini ücretlendirir. Azure Hibrit Avantajı, PAYG VM 'Leri yeniden dağıtım olmadan bir KCG faturalandırma modeline dönüştürülebilir ve kapalı kalma riskini ortadan kaldırabilirsiniz.

:::image type="content" source="./media/ahb-linux/azure-hybrid-benefit-cost.png" alt-text="Linux VM 'lerinde Azure Hibrit Avantajı maliyet görselleştirmesi.":::

Bir RHEL veya SLES VM 'de avantajı etkinleştirdikten sonra, genellikle bir PAYG VM 'si üzerinde tahakkuk eden ek yazılım ücreti için ücret ödemeyecektir. Bunun yerine, VM 'niz yalnızca işlem donanımı ücretini ve yazılım ücreti gerektirmeyen bir KCG ücreti yayacaktır.

İsterseniz, avantajlı bir sanal makineyi bir PAYG faturalama modeline geri döndüğünüzde de dönüştürebilirsiniz.

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux-vms"></a>Linux VM 'Ler için Azure Hibrit Avantajı uygunluk kapsamı

Tüm RHEL ve SLES Market PAYG görüntüleri için Azure Hibrit Avantajı kullanılabilir. Avantaj, RHEL veya SLES Market KCG görüntüleri veya özel görüntüler için henüz kullanılamamaktadır.

Linux VM 'lerle avantajı zaten kullanıyorsanız, ayrılmış örnekler, adanmış konaklar ve SQL hibrit avantajları Azure Hibrit Avantajı uygun değildir.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Azure Hibrit Avantajı Şu anda Linux VM 'Leri için bir önizleme aşamasındadır. Önizlemeye erişim kazandıktan sonra, Azure CLı kullanarak avantaja izin verebilir.

### <a name="public-preview"></a>Genel Önizleme

Azure Hibrit Avantajı (Linux için) Şu anda genel önizleme aşamasındadır. Red Hat ve SUSE dağıtımları avantajını etkinleştirmek için aşağıdaki adımları kullanabilirsiniz. 

### <a name="red-hat-customers"></a>Red Hat müşterileri

RHEL için Azure Hibrit Avantajı, Azure 'da kullanıma uygun olan ve bu aboneliklerin bir veya birkaçını, [Red Hat bulut erişimi](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) programıyla Azure 'da kullanılmak üzere etkinleştiren etkin/kullanılmamış RHEL abonelikleri olan müşteriler tarafından kullanılabilir. 

1.  [Red Hat bulut erişimi müşteri arabirimini](https://access.redhat.com/management/cloud)kullanarak Azure 'da kullanılmak üzere uygun RHEL aboneliklerinizden birini veya birkaçını etkinleştirin.
1.  Red Hat bulut erişimi etkinleştirme işlemi sırasında verdiğiniz Azure abonelikleri Azure Hibrit Avantajı özelliğini kullanmasına izin verilir.
1.  Azure Hibrit Avantajı mevcut RHEL PAYG sanal makinelerinizden ve Azure Marketi PAYG görüntülerinden dağıttığınız tüm yeni RHEL VM 'lerine uygulayın.
1.  RHEL VM 'leriniz için güncelleştirme kaynaklarını yapılandırma ve RHEL abonelik uyumluluk yönergeleri için önerilen [sonraki adımları](https://access.redhat.com/articles/5419341) izleyin.


### <a name="suse-customers"></a>SUSE müşterileri

1.    SUSE genel bulut programına kaydolun
1.    Azure CLı aracılığıyla mevcut sanal makinelerinize avantajını uygulayın
1.    VM 'lerinizi, ayrı bir güncelleştirme kaynağıyla avantajını alacak şekilde kaydedin


### <a name="enable-and-disable-the-benefit-in-the-azure-cli"></a>Azure CLı 'de avantajı etkinleştirme ve devre dışı bırakma

Mevcut VM 'Leri güncelleştirmek için ' az VM Update ' komutunu kullanabilirsiniz. RHEL VM 'Ler için, "RHEL_BYOS" öğesinin lisans türü parametresiyle komutunu çalıştırın. SLES sanal makineleri için, "SLES_BYOS" öğesinin--License-Type parametresiyle komutunu çalıştırın.

#### <a name="cli-example-to-enable-the-benefit"></a>Avantajı sağlamak için CLı örneği:
```azurecli
# This will enable the benefit on a RHEL VM
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS

# This will enable the benefit on a SLES VM
az vm update -g myResourceGroup -n myVmName --license-type SLES_BYOS
```
#### <a name="cli-example-to-disable-the-benefit"></a>Avantajları devre dışı bırakmak için CLı örneği:
Avantajı devre dışı bırakmak için "none" lisans türü değerini kullanın
```azurecli
# This will disable the benefit on a VM
az vm update -g myResourceGroup -n myVmName --license-type None
```

#### <a name="cli-example-to-enable-the-benefit-on-a-large-number-of-vms"></a>Çok sayıda VM 'nin avantajını etkinleştirmek için CLı örneği
Çok sayıda VM 'nin avantajını etkinleştirmek için, `--ids` Azure CLI 'de parametresini kullanabilirsiniz.

```azurecli
# This will enable the benefit on a RHEL VM. In this example, ids.txt is an
# existing text file containing a delimited list of resource IDs corresponding
# to the VMs using the benefit
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS --ids $(cat ids.txt)
```

Aşağıdaki örneklerde, biri abonelik düzeyinde olmak üzere kaynak kimliklerinin bir listesini almak için iki yöntem gösterilmektedir.
```azurecli
# To get a list of all the resource IDs in a resource group:
$(az vm list -g MyResourceGroup --query "[].id" -o tsv)

# To get a list of all the resource IDs of VMs in a subscription:
az vm list -o json | jq '.[] | {VMName: .name, ResourceID: .id}'
```

## <a name="check-ahb-status-of-a-vm"></a>Bir VM 'nin AHB durumunu denetleme
Bir VM 'nin AHB durumunu iki şekilde görüntüleyebilirsiniz: Azure CLı veya Azure Instance Metadata Service (Azure ıMDS) kullanarak.


### <a name="azure-cli"></a>Azure CLI

`az vm get-instance-view`Bu amaçla komut kullanılıyor olabilir. Yanıtta bir licenseType alanı bulun. LicenseType alanı varsa ve değer ' RHEL_BYOS ' veya ' SLES_BYOS ' ise, sanal makinenizin avantajı etkin olur.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

VM 'nin içinden, sanal makinenin licenseType öğesini tespit etmek için ıTıD Atsınanan meta verileri sorgulayabilirsiniz. ' RHEL_BYOS ' veya ' SLES_BYOS ' için licenseType değeri, sanal makinenizin avantajın etkin olduğunu gösterir. Atsınanan meta veriler hakkında [buradan](./instance-metadata-service.md#attested-data) daha fazla bilgi edinin

## <a name="compliance"></a>Uyumluluk

### <a name="red-hat"></a>Red Hat

RHEL için Azure Hibrit Avantajı kullanan müşteriler, Azure Marketi RHEL teklifleriyle ilişkili standart [yasal hüküm](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Cloud_Software_Subscription_Agreement_for_Microsoft_Azure.pdf) ve [Gizlilik bildirimini](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Privacy_Statement_for_Microsoft_Azure.pdf) kabul etmiş olursunuz.

RHEL için Azure Hibrit Avantajı kullanan müşterilerin, bu VM 'lere yazılım güncelleştirmeleri ve düzeltme ekleri sağlamak için üç seçeneği vardır:

1.  [Red Hat güncelleştirme altyapısı (rhuı)](../workloads/redhat/redhat-rhui.md) (varsayılan seçenek)
1.  Red Hat uydu sunucusu
1.  Red Hat abonelik Yöneticisi

RHUı seçeneğini seçmeden müşteriler, bu VM 'lere RHEL abonelikleri eklemeden AHB RHEL VM 'lerinin ana güncelleştirme kaynağı olarak RHUı kullanmaya devam edebilir.  RHUı seçeneğini seçmeden müşteriler RHEL abonelik uyumluluğunu sağlamaktan sorumludur.

Red Hat uydu sunucusu veya Red Hat abonelik Yöneticisi ' ni seçerek müşteriler RHUı yapılandırmasını kaldırmalı ve sonra AHB RHEL VM 'lerine bir bulut erişimi etkinleştirilmiş RHEL aboneliği iliştirmelidir.  

Red Hat abonelik uyumluluğu, yazılım güncelleştirmeleri ve AHB RHEL VM 'lerinin kaynakları hakkında daha fazla bilgi [burada](https://access.redhat.com/articles/5419341)bulabilirsiniz.

### <a name="suse"></a>SUSE

SLES sanal makinelerinize yönelik Azure Hibrit Avantajı kullanabilmeniz için öncelikle SUSE genel bulut programına kaydolmalısınız. Program hakkında daha fazla bilgi edinin. SUSE abonelikleri satın aldıktan sonra, SUSE müşteri merkezini, abonelik Yönetim Aracı sunucusunu veya SUSE Manager 'ı kullanarak kendi güncelleştirme kaynağınıza bu abonelikleri kullanarak sanal makinelerinizi kaydetmeniz gerekir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
*S: bir ' RHEL_BYOS ' lisans türünü bir SLES görüntüsü ile kullanabilir miyim veya bunun tersini yapabilir miyim?*

Y: Hayır. VM 'niz üzerinde çalışan ile yanlış eşleşen bir lisans türü girmeye çalışmak, hiçbir faturalandırma meta verilerini güncelleştirmeyecektir. Ancak yanlışlıkla yanlış lisans türünü girerseniz, sanal makinenizin yeniden doğru lisans türüne güncelleştirilmesi yine de avantaja izin vermez.

*S: Red Hat bulut erişimi ile kaydoldum, ancak yine de RHEL VM 'lerimin avantajını etkinleştiremedik. Ne yapmalıyım?*

Y: Red Hat bulut erişimi aboneliğinin, Red Hat 'ten Azure 'a yayılması biraz zaman alabilir. Bir iş gününden sonra hatayı görmeye devam ediyorsanız, Microsoft destek 'e başvurun.

## <a name="common-issues"></a>Genel sorunlar
Bu bölüm, karşılaşılan yaygın sorunların ve hafifletme adımlarının bir listesini içerir.

| Hata | Risk azaltma |
| ----- | ---------- |
| "Kayıtlarımız Azure aboneliğinizde Red Hat bulutu erişimini başarıyla etkinleştirmediyseniz, bu eylem tamamlanamadı...." | RHEL VM 'lerle ilgili avantajını kullanabilmeniz için, önce Azure aboneliklerinizi Red Hat bulut erişimi ile kaydetmeniz gerekir. Red Hat bulut erişimi için Azure aboneliklerinizi kaydetme hakkında daha fazla bilgi edinmek üzere bu bağlantıyı ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLI](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest&preserve-view=true) kullanarak Azure hibrit avantajı için VM oluşturma ve güncelleştirme ve lisans türleri (RHEL_BYOS SLES_BYOS) ekleme hakkında bilgi edinin.
