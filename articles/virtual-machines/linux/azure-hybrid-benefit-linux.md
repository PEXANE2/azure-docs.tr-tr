---
title: Azure Hibrit Avantajı ve Linux VM 'Leri
description: Azure Hibrit Avantajı Azure 'da çalışan Linux sanal makinelerinizde tasarruf etmenize nasıl yardımcı olabileceğini öğrenin.
services: virtual-machines
documentationcenter: ''
author: mathapli
manager: westonh
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/22/2020
ms.author: mathapli
ms.openlocfilehash: 1bc108f76ac35b13474de18d473f5728dbad9d23
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97560025"
---
# <a name="how-azure-hybrid-benefit-applies-for-linux-virtual-machines"></a>Azure Hibrit Avantajı Linux sanal makineleri için nasıl uygulanır

Azure Hibrit Avantajı, bulutta Red Hat Enterprise Linux (RHEL) ve SUSE Linux Enterprise Server (SLES) sanal makinelerini (VM) çalıştırmanın maliyetlerini önemli ölçüde düşürmenize yardımcı olan bir lisanslama avantajıdır. Bu avantajla, RHEL veya SLES aboneliğiniz yazılım ücretini kapsadığından sanal makinenizin yalnızca altyapı maliyetleri için ödeme yaparsınız. Bu avantaj, tüm RHEL ve SLES Market (PAYG) görüntüleri için geçerlidir.

Linux sanal makineleri için Azure Hibrit Avantajı artık genel kullanıma sunulmuştur.

## <a name="benefit-description"></a>Avantaj açıklaması

Azure Hibrit Avantajı aracılığıyla, şirket içi RHEL ve SLES sunucularınızı Azure 'da var olan RHEL ve SLES PAYG sanal makinelerini Azure 'a geçirerek kendi abonelik (KCG) faturanızı (BYOS) faturalandırmayı getirebilirsiniz. Genellikle, Azure 'daki PAYG görüntülerinden dağıtılan VM 'Ler hem altyapı ücreti hem de yazılım ücreti ücretlendirir. Azure Hibrit Avantajı, PAYG VM 'Leri yeniden dağıtım olmadan bir KCG faturalandırma modeline dönüştürülebilir, bu sayede kapalı kalma riskini önleyebilirsiniz.

:::image type="content" source="./media/ahb-linux/azure-hybrid-benefit-cost.png" alt-text="Linux VM 'lerinde Azure Hibrit Avantajı maliyet görselleştirmesi.":::

Bir RHEL veya SLES VM 'de avantajı etkinleştirdikten sonra, genellikle bir PAYG VM 'si üzerinde tahakkuk eden ek yazılım ücreti için ücretlendirilirsiniz. Bunun yerine, VM 'niz, yalnızca işlem donanımı ücretini ve yazılım ücreti gerektirmeyen bir KCG ücreti tahakkuk eder.

Ayrıca, avantajı etkin olan bir VM 'yi bir PAYG faturalama modeline geri dönüştürmeyi seçebilirsiniz.

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux-vms"></a>Linux VM 'Ler için Azure Hibrit Avantajı uygunluk kapsamı

Azure Hibrit Avantajı Azure Marketi 'ndeki tüm RHEL ve SLES PAYG görüntüleri için kullanılabilir. Avantaj, Azure Marketi 'nden RHEL veya SLES BYOS görüntüleri veya özel görüntüleri için henüz kullanılamamaktadır.

Linux VM 'lerle avantajı zaten kullanıyorsanız, ayrılmış örnekler, Azure adanmış konak örnekleri ve SQL hibrit avantajları Azure Hibrit Avantajı için uygun değildir.

## <a name="get-started"></a>Kullanmaya başlayın

### <a name="red-hat-customers"></a>Red Hat müşterileri

RHEL için Azure Hibrit Avantajı, bu ölçütlerin her ikisini de karşılayan Red Hat müşterileri tarafından kullanılabilir:

- Azure 'da kullanıma uygun etkin veya kullanılmayan RHEL abonelikleri var
- [Red Hat bulut erişimi](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) programıyla Azure 'da kullanılmak üzere Bu aboneliklerden bir veya daha fazlasını etkinleştirdi

Red Hat avantajını kullanmaya başlamak için:

1. [Red Hat bulut erişimi müşteri arabirimini](https://access.redhat.com/management/cloud)kullanarak Azure 'da kullanılmak üzere uygun RHEL aboneliklerinizden birini veya birkaçını etkinleştirin.

   Red Hat bulut erişimi etkinleştirme işlemi sırasında sağladığınız Azure aboneliklerinin Azure Hibrit Avantajı özelliğini kullanmasına izin verilir.
1. Mevcut RHEL PAYG sanal makinelerinizden ve Azure Marketi PAYG görüntülerinden dağıttığınız tüm yeni RHEL VM 'lerine Azure Hibrit Avantajı uygulayın.
1. RHEL VM 'leriniz için güncelleştirme kaynaklarını yapılandırma ve RHEL abonelik uyumluluk yönergeleri için önerilen [sonraki adımları](https://access.redhat.com/articles/5419341) izleyin.


### <a name="suse-customers"></a>SUSE müşterileri

SUSE avantajı 'nı kullanmaya başlamak için:

1. SUSE genel bulut programı ile kaydolun.
1. Azure CLı aracılığıyla mevcut sanal makinelerinize avantajını uygulayın.
1. Avantajlarınızı alan VM 'lerinizi ayrı bir güncelleştirme kaynağıyla kaydedin.


## <a name="enable-and-disable-the-benefit-in-the-azure-cli"></a>Azure CLı 'de avantajı etkinleştirme ve devre dışı bırakma

`az vm update`Mevcut VM 'leri güncelleştirmek için komutunu kullanabilirsiniz. RHEL VM 'Leri için komutunu `--license-type` parametresiyle çalıştırın `RHEL_BYOS` . SLES sanal makineleri için komutunu `--license-type` parametresiyle çalıştırın `SLES_BYOS` .

### <a name="cli-example-to-enable-the-benefit"></a>Avantajı sağlamak için CLı örneği
```azurecli
# This will enable the benefit on a RHEL VM
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS

# This will enable the benefit on a SLES VM
az vm update -g myResourceGroup -n myVmName --license-type SLES_BYOS
```
### <a name="cli-example-to-disable-the-benefit"></a>Avantajı devre dışı bırakmak için CLı örneği
Avantajı devre dışı bırakmak için, bir `--license-type` değeri kullanın `None` :

```azurecli
# This will disable the benefit on a VM
az vm update -g myResourceGroup -n myVmName --license-type None
```

### <a name="cli-example-to-enable-the-benefit-on-a-large-number-of-vms"></a>Çok sayıda VM 'nin avantajını etkinleştirmek için CLı örneği
Çok sayıda VM 'nin avantajını etkinleştirmek için, `--ids` Azure CLI 'de parametresini kullanabilirsiniz:

```azurecli
# This will enable the benefit on a RHEL VM. In this example, ids.txt is an
# existing text file that contains a delimited list of resource IDs corresponding
# to the VMs using the benefit
az vm update -g myResourceGroup -n myVmName --license-type RHEL_BYOS --ids $(cat ids.txt)
```

Aşağıdaki örneklerde, kaynak kimliklerinin bir listesini almak için iki yöntem gösterilmektedir: bir kaynak grubu düzeyi ve bir diğeri de abonelik düzeyi.

```azurecli
# To get a list of all the resource IDs in a resource group:
$(az vm list -g MyResourceGroup --query "[].id" -o tsv)

# To get a list of all the resource IDs of VMs in a subscription:
az vm list -o json | jq '.[] | {VMName: .name, ResourceID: .id}'
```

## <a name="apply-the-azure-hybrid-benefit-at-vm-create-time"></a>VM oluşturma zamanında Azure Hibrit Avantajı uygulama
Mevcut Kullandıkça Öde VM 'lerine Azure Hibrit Avantajı uygulamanın yanı sıra, VM oluşturma sırasında çağırabilirsiniz. Bunu yapmanın avantajları şunlardır:
- Aynı görüntüyü ve işlemi kullanarak hem PAYG hem de KCG VM 'Leri sağlayabilirsiniz.
- Gelecekteki lisans modu değişikliklerine, yalnızca bir KCG görüntüsü ile kullanılamayacak veya kendi VM 'nizi getirmenize izin vermez.
- Bu sanal makine, güncel ve güvenli kalmasını sağlamak için varsayılan olarak Red Hat güncelleştirme altyapısına (RHUı) bağlanır. Dağıtım sonrasında güncelleştirilmiş mekanizmayı dilediğiniz zaman değiştirebilirsiniz.

## <a name="check-the-azure-hybrid-benefit-status-of-a-vm"></a>Bir sanal makinenin Azure Hibrit Avantajı durumunu denetleme
Azure CLı kullanarak veya Azure Instance Metadata Service kullanarak bir sanal makinenin Azure Hibrit Avantajı durumunu görüntüleyebilirsiniz.

### <a name="azure-cli"></a>Azure CLI’si

`az vm get-instance-view`Bu amaçla komutunu kullanabilirsiniz. `licenseType`Yanıttaki bir alanı bulun. `licenseType`Alan varsa ve değer `RHEL_BYOS` veya Ise `SLES_BYOS` , sanal makinenizin avantajı etkinleştirilmiştir.

```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVm
```

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

VM 'nin içinden, sanal makinenin değerini öğrenmek için Azure Instance Metadata Service 'te atsınanan meta verileri sorgulayabilirsiniz `licenseType` . `licenseType`Veya değeri, `RHEL_BYOS` `SLES_BYOS` sanal makinenizin avantajın etkin olduğunu gösterir. [Atsınanan meta veriler hakkında daha fazla bilgi edinin](./instance-metadata-service.md#attested-data).

## <a name="compliance"></a>Uyumluluk

### <a name="red-hat"></a>Red Hat

RHEL için Azure Hibrit Avantajı kullanan müşteriler, Azure Marketi RHEL teklifleriyle ilişkili standart [yasal hüküm](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Cloud_Software_Subscription_Agreement_for_Microsoft_Azure.pdf) ve [Gizlilik bildirimini](http://www.redhat.com/licenses/cloud_CSSA/Red_Hat_Privacy_Statement_for_Microsoft_Azure.pdf) kabul etmiş olursunuz.

RHEL için Azure Hibrit Avantajı kullanan müşterilerin, bu VM 'lere yazılım güncelleştirmeleri ve düzeltme ekleri sağlamak için üç seçeneği vardır:

- [Red Hat güncelleştirme altyapısı](../workloads/redhat/redhat-rhui.md) (varsayılan seçenek)
- Red Hat uydu sunucusu
- Red Hat abonelik Yöneticisi

RHUı seçeneğini seçen müşteriler, bu VM 'lere RHEL abonelikleri iliştirmeden Azure Hibrit Avantajı RHEL VM 'Leri için ana güncelleştirme kaynağı olarak RHUı kullanmaya devam edebilir. RHUı seçeneğini seçen müşteriler RHEL abonelik uyumluluğunu sağlamaktan sorumludur.

Red Hat uydu sunucusu veya Red Hat abonelik Yöneticisi 'Ni seçen müşteriler RHUı yapılandırmasını kaldırmalı ve ardından Azure Hibrit Avantajı RHEL VM 'lerine bulut erişimi etkinleştirilmiş bir RHEL aboneliği iliştirmelidir.  

Red Hat abonelik uyumluluğu, yazılım güncelleştirmeleri ve Azure Hibrit Avantajı RHEL VM 'lerinin kaynakları hakkında daha fazla bilgi için, [Azure hibrit avantajı Ile RHEL aboneliklerini kullanma hakkında Red Hat makalesine](https://access.redhat.com/articles/5419341)bakın.

### <a name="suse"></a>SUSE

SLES sanal makinelerinize yönelik Azure Hibrit Avantajı kullanmak için, önce [SUSE genel bulut programına](https://www.suse.com/media/guide/suse_public_cloud_service_provider_program_overview.pdf)kaydolmalısınız. SUSE abonelikleri satın aldıktan sonra, bu abonelikleri kullanan sanal makinelerinizi kendi güncelleştirme kaynağınıza kaydetmeniz gerekir. Bu kayıt için SUSE müşteri merkezini, abonelik Yönetim Aracı sunucusunu veya SUSE Manager 'ı kullanın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
*S: bir lisans türünü `RHEL_BYOS` BIR SLES görüntüsü ile kullanabilir miyim veya bunun tersini yapabilir miyim?*

Y: Hayır, bunu yapamazsınız. VM 'niz üzerinde çalışan dağıtım ile yanlış eşleşen bir lisans türü girmeye çalışmak, hiçbir faturalandırma meta verilerini güncelleştirmez. Ancak yanlışlıkla yanlış lisans türünü girerseniz, sanal makinenizin doğru lisans türüne güncelleştirilmesi yine de avantaja izin vermez.

*S: Red Hat bulut erişimi ile kaydoldum, ancak yine de RHEL VM 'lerimin avantajını etkinleştiremiyorum. Ne yapmam gerekir?*

Y: Red Hat bulut erişim aboneliği kaydı, Red Hat 'ten Azure 'a yayılır. Bir iş gününden sonra hatayı görmeye devam ediyorsanız, Microsoft destek 'e başvurun.

*S: RHEL BYOS "altın görüntüsünü kullanarak bir VM dağıttım. Bu görüntülerin faturalandırmasını BYOS 'den PAYG 'ye dönüştürebilir miyim?*

Y: Hayır, bunu yapamazsınız. Azure Hibrit Avantajı yalnızca Kullandıkça Öde görüntülerinde dönüştürmeyi destekler.

*S: kendi RHEL görüntümi Şirket içinden (Azure geçişi, Azure Site Recovery veya başka bir şekilde) Azure 'a karşıya yükledim. Bu görüntülerin faturalandırmasını BYOS 'den PAYG 'ye dönüştürebilir miyim?*

Y: Hayır, bunu yapamazsınız. Azure Hibrit Avantajı özelliği şu anda yalnızca Azure Marketi 'ndeki RHEL ve SLES görüntüleri için kullanılabilir. 

*S: kendi RHEL görüntümi Şirket içinden (Azure geçişi, Azure Site Recovery veya başka bir şekilde) Azure 'a karşıya yükledim. Azure Hibrit Avantajı avantajına bir şey yapmam gerekiyor mu?*

Y: Hayır, hayır. Karşıya yüklediğiniz RHEL görüntüleri zaten BYOS olarak kabul edilir ve yalnızca Azure altyapı maliyetleri için ücretlendirilirsiniz. Şirket içi ortamlarınızda olduğu gibi, RHEL abonelik maliyetlerinden siz sorumlusunuz. 

*S: Azure Marketi RHEL ve SLES SAP görüntülerinden dağıtılan VM 'lerde Azure Hibrit Avantajı kullanabilir miyim?*

Y: Evet, bunu yapabilirsiniz. `RHEL_BYOS`RHEL VM 'leri için lisans türünü ve `SLES_BYOS` Azure Marketi RHEL ve SLES SAP görüntülerinden dağıtılan VM 'lerin dönüştürmelerini kullanabilirsiniz.

*S: RHEL ve SLES için sanal makine ölçek kümelerinde Azure Hibrit Avantajı kullanabilir miyim?*

Y: Hayır, bunu yapamazsınız. Sanal Makine Ölçek Kümeleri Şu anda RHEL ve SLES için Azure Hibrit Avantajı kapsamında değildir.

*S: RHEL ve SLES için ayrılmış örneklerde Azure Hibrit Avantajı kullanabilir miyim?*

Y: Hayır, bunu yapamazsınız. Ayrılmış örnekler şu anda RHEL ve SLES için Azure Hibrit Avantajı kapsamında değildir.

*S: RHEL görüntülerinde SQL Server için dağıtılan bir sanal makinede Azure Hibrit Avantajı kullanabilir miyim?*

Y: Hayır, bunu yapamazsınız. Bunları desteklemeye yönelik bir plan yoktur.
 

## <a name="common-problems"></a>Sık karşılaşılan sorunlar
Bu bölümde karşılaşabileceğiniz yaygın sorunlar ve hafifletme adımları listelenmektedir.

| Hata | Risk azaltma |
| ----- | ---------- |
| "Kayıtlarımız Azure aboneliğinizde Red Hat bulutu erişimini başarıyla etkinleştirmediyseniz, bu eylem tamamlanamadı...." | RHEL VM 'lerle ilgili avantajını kullanmak için önce [Azure aboneliklerinizi Red Hat bulut erişimi ile kaydetmeniz](https://access.redhat.com/management/cloud)gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLı kullanarak VM oluşturma ve güncelleştirme ve Azure Hibrit Avantajı için lisans türleri (RHEL_BYOS SLES_BYOS) ekleme hakkında bilgi edinin](/cli/azure/vm?preserve-view=true&view=azure-cli-latest)
