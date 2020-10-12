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
ms.author: alsin
ms.openlocfilehash: d62eaf96354627e0c1e4e0a31bb16fb3265f66ac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91279782"
---
# <a name="preview-azure-hybrid-benefit--how-it-applies-for-linux-virtual-machines"></a>Önizleme: Azure Hibrit Avantajı – Linux Sanal Makineleri için nasıl uygulanır?

## <a name="overview"></a>Genel Bakış

Azure Hibrit Avantajı, önceden var olan Red hat veya SUSE yazılım aboneliğinizi kullanarak şirket içi Red Hat Enterprise Linux (RHEL) ve SUSE Linux Enterprise Server (SLES) sanal makinelerinizi (VM) Azure 'a daha kolay bir şekilde geçirebilmesini sağlar. Bu avantajla, yazılım ücreti RHEL veya SLES Aboneliğiniz kapsamında olduğu için yalnızca sanal makinenizin altyapı maliyetleri için ödeme yaparsınız. Bu avantaj, tüm RHEL ve SLES Market (PAYG) görüntüleri için geçerlidir.

> [!IMPORTANT]
> Linux VM 'Leri için Azure Hibrit Avantajı Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="benefit-description"></a>Avantaj açıklaması

Azure Hibrit Avantajı aracılığıyla, şirket içi RHEL ve SLES sunucularınızı, Azure 'da var olan RHEL ve SLES PAYG sanal makinelerini Azure 'a dönüştürerek kendi abonelik (KCG) faturanızı daha kolay hale getirebilirsiniz. Genellikle, Azure 'daki PAYG görüntülerinden dağıtılan VM 'Ler hem bir altyapı ücretini hem de yazılım ücretini ücretlendirir. Azure Hibrit Avantajı, PAYG VM 'Leri yeniden dağıtım olmadan bir KCG faturalandırma modeline dönüştürülebilir ve kapalı kalma riskini ortadan kaldırabilirsiniz.

:::image type="content" source="./media/ahb-linux/azure-hybrid-benefit-cost.png" alt-text="Linux VM 'lerinde Azure Hibrit Avantajı maliyet görselleştirmesi.":::

Bir RHEL veya SLES VM 'de avantajı etkinleştirdikten sonra, genellikle bir PAYG VM 'si üzerinde tahakkuk eden ek yazılım ücreti için ücret ödemeyecektir. Bunun yerine, VM 'niz yalnızca işlem donanımı ücretini ve yazılım ücreti gerektirmeyen bir KCG ücreti yayacaktır.

İsterseniz, avantajlı bir sanal makineyi bir PAYG faturalama modeline geri döndüğünüzde de dönüştürebilirsiniz.

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux-vms"></a>Linux VM 'Ler için Azure Hibrit Avantajı uygunluk kapsamı

Tüm RHEL ve SLES Market PAYG görüntüleri için Azure Hibrit Avantajı kullanılabilir. Avantaj, RHEL veya SLES Market KCG görüntüleri veya özel görüntüler için henüz kullanılamamaktadır.

Linux VM 'lerle avantajı zaten kullanıyorsanız, ayrılmış örnekler, adanmış konaklar ve SQL hibrit avantajları Azure Hibrit Avantajı uygun değildir.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Azure Hibrit Avantajı Şu anda Linux VM 'Leri için bir önizleme aşamasındadır. Önizlemeye erişim kazandıktan sonra, Azure portal veya Azure CLı kullanarak avantaja izin verebilir.

### <a name="preview"></a>Önizleme

Bu aşamada, formu [buraya](https://aka.ms/ahb-linux-form)doldurarak avantaja erişebilirsiniz. Formu doldurduktan sonra Azure abonelikleriniz avantajdan etkinleştirilir ve Microsoft 'tan üç iş günü içinde bir onay alacaksınız.

### <a name="red-hat-customers"></a>Red Hat müşterileri

1.    Yukarıdaki önizleme isteği formunu doldurun
1.    [Red Hat bulut erişim programına](https://aka.ms/rhel-cloud-access) kaydolun
1.    Azure aboneliklerinizi bulut erişimi için etkinleştirin ve avantajını kullanmak istediğiniz VM 'Leri içeren abonelikleri etkinleştirin
1.    Azure portal veya Azure CLı aracılığıyla mevcut sanal makinelerinize avantajını uygulayın
1.    VM 'lerinizi, ayrı bir güncelleştirme kaynağıyla avantajını alacak şekilde kaydedin

### <a name="suse-customers"></a>SUSE müşterileri

1.    Yukarıdaki önizleme isteği formunu doldurun
1.    SUSE genel bulut programına kaydolun
1.    Azure portal veya Azure CLı aracılığıyla mevcut sanal makinelerinize avantajını uygulayın
1.    VM 'lerinizi, ayrı bir güncelleştirme kaynağıyla avantajını alacak şekilde kaydedin

### <a name="enable-and-disable-the-benefit-in-the-azure-portal"></a>Azure portal avantajı etkinleştirin ve devre dışı bırakın

**Yapılandırma** dikey penceresini ziyaret ederek ve bu adımları izleyerek var olan sanal makinelerde avantaja izin verebilir. VM oluşturma deneyimi sırasında yeni VM 'lerde avantaja izin verebilir.

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
Bir VM 'nin AHB durumunu üç şekilde görüntüleyebilirsiniz: portalda, Azure CLı kullanarak veya Azure Instance Metadata Service (Azure ıMDS) kullanarak.


### <a name="portal"></a>Portal

VM 'niz için, AHB 'nin etkin olup olmadığını görmek için yapılandırma dikey penceresini görüntüleyin ve lisanslama durumunu denetleyin.

### <a name="azure-cli"></a>Azure CLI

`az vm get-instance-view`Bu amaçla komut kullanılıyor olabilir. Yanıtta bir licenseType alanı bulun. LicenseType alanı varsa ve değer ' RHEL_BYOS ' veya ' SLES_BYOS ' ise, sanal makinenizin avantajı etkin olur.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

VM 'nin içinden, sanal makinenin licenseType öğesini tespit etmek için ıTıD Atsınanan meta verileri sorgulayabilirsiniz. ' RHEL_BYOS ' veya ' SLES_BYOS ' için licenseType değeri, sanal makinenizin avantajın etkin olduğunu gösterir. Atsınanan meta veriler hakkında [buradan](https://docs.microsoft.com/azure/virtual-machines/linux/instance-metadata-service#attested-data) daha fazla bilgi edinin

## <a name="compliance"></a>Uyumluluk

### <a name="red-hat"></a>Red Hat

RHEL sanal makinelerinize yönelik Azure Hibrit Avantajı kullanmak için, önce Red Hat bulut erişim programına kaydolmalısınız. Bunu, Red Hat bulut erişimi sitesi aracılığıyla yapabilirsiniz. VM 'nizin avantajını etkinleştirdikten sonra, sanal makineyi Red Hat abonelik Yöneticisi veya Red Hat uydu ile kendi güncelleştirme kaynağınıza kaydetmeniz gerekir. Güncelleştirmeler için kaydolmak, desteklenen bir durumda kalacağından emin olur.

### <a name="suse"></a>SUSE

SLES sanal makinelerinize yönelik Azure Hibrit Avantajı kullanabilmeniz için öncelikle SUSE genel bulut programına kaydolmalısınız. Program hakkında daha fazla bilgi edinin. SUSE abonelikleri satın aldıktan sonra, SUSE müşteri merkezini, abonelik Yönetim Aracı sunucusunu veya SUSE Manager 'ı kullanarak kendi güncelleştirme kaynağınıza bu abonelikleri kullanarak sanal makinelerinizi kaydetmeniz gerekir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
*S: bir ' RHEL_BYOS ' lisans türünü bir SLES görüntüsü ile kullanabilir miyim veya bunun tersini yapabilir miyim?*

Y: Hayır. VM 'niz üzerinde çalışan ile yanlış eşleşen bir lisans türü girmeye çalışmak, hiçbir faturalandırma meta verilerini güncelleştirmeyecektir. Ancak yanlışlıkla yanlış lisans türünü girerseniz, sanal makinenizin yeniden doğru lisans türüne güncelleştirilmesi yine de avantaja izin vermez.

*S: Red Hat bulut erişimi ile kaydoldum, ancak yine de RHEL VM 'lerimin avantajını etkinleştiremedik. Ne yapmalıyım?*

Y: Red Hat bulut erişimi aboneliğinin, Red Hat 'ten Azure 'a yayılması biraz zaman alabilir. Bir iş gününden sonra hatayı görmeye devam ediyorsanız, Microsoft destek 'e başvurun.

## <a name="common-errors"></a>Sık karşılaşılan hatalar
Bu bölüm, azaltma için sık karşılaşılan hataların ve adımların bir listesini içerir.

| Hata | Risk azaltma |
| ----- | ---------- |
| "Abonelik, Azure Hibrit Avantajı Linux önizlemesi için kayıtlı değil. Adım adım yönergeler için bkz. https://aka.ms/ahb-linux " | https://aka.ms/ahb-linux-formAzure hibrit avantajı Linux önizlemesi için kaydolmak üzere konumundaki formu doldurun.
| "Kayıtlarımız Azure aboneliğinizde Red Hat bulutu erişimini başarıyla etkinleştirmediyseniz, bu eylem tamamlanamadı...." | RHEL VM 'lerle ilgili avantajını kullanabilmeniz için, önce Azure aboneliklerinizi Red Hat bulut erişimi ile kaydetmeniz gerekir. Red Hat bulut erişimi için Azure aboneliklerinizi kaydetme hakkında daha fazla bilgi edinmek üzere bu bağlantıyı ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar
* Formu [burada](https://aka.ms/ahb-linux-form)doldurarak Önizlemeyi kullanmaya başlayın.
