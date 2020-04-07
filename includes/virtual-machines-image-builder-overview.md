---
author: cynthn
ms.author: cynthn
ms.date: 01/23/2020
ms.topic: include
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 658910dc4291375c7b2ab22e88c599b970b885af
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80419216"
---
Standartlaştırılmış sanal makine (VM) görüntüleri, kuruluşların buluta geçiş etmesine ve dağıtımlarda tutarlılık sağlamasına olanak sağlar. Görüntüler genellikle önceden tanımlanmış güvenlik ve yapılandırma ayarlarını ve gerekli yazılımları içerir. Kendi görüntüleme ardışık bilgisayarınızı ayarlamak zaman, altyapı ve kurulum gerektirir, ancak Azure VM Image Builder ile görüntünüzü açıklayan basit bir yapılandırma sağlayın, hizmete gönderin ve görüntü oluşturulur ve dağıtılır.
 
Azure VM Image Builder (Azure Image Builder), Windows veya Linux tabanlı Azure Marketi görüntüsü, varolan özel resimler veya Red Hat Enterprise Linux (RHEL) ISO ile başlamanızı ve kendi özelleştirmelerinizi eklemenize olanak tanır. Image Builder [HashiCorp Packer](https://packer.io/)üzerine inşa olduğundan, mevcut Packer kabuk provizyoner komut almak da olabilir. Ayrıca, azure [paylaşılan resim galerisinde,](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)yönetilen bir resim veya VHD olarak görüntülerinizin nerede barındırılmak istediğinizi de belirtebilirsiniz.

> [!IMPORTANT]
> Azure Image Builder şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="preview-features"></a>Önizleme özellikleri

Önizleme için bu özellikler desteklenir:

- Minimum güvenlik ve kurumsal yapılandırmalarınızı içeren ve departmanların gereksinimlerine göre daha fazla özelleştirmelerine olanak tanıyan altın temel görüntülerin oluşturulması.
- Varolan görüntülerin yamalanması, Image Builder sürekli olarak varolan özel görüntüleri yamanıza olanak sağlar.
- Mevcut yapılandırma sunucularına (DSC, Chef, Puppet vb.), dosya paylaşımlarına veya diğer rahatsız sunuculara/hizmetlere bağlanabilmeniz için görüntü oluşturucuyu mevcut sanal ağlarınıza bağlayın.
- Azure Paylaşılan Resim Galerisi ile tümleştirme, görüntüleri genel olarak dağıtmanıza, sürümlendirmenize ve ölçeklendirmenize olanak tanır ve size bir görüntü yönetim sistemi sağlar.
- Varolan görüntü oluşturma boru hatlarıyla tümleştirme yapın, yalnızca ardışık bilgisayarınızdan Image Builder'ı arayın veya basit Önizleme Görüntü Oluşturucusu Azure DevOps Görevini kullanın.
- Varolan görüntü özelleştirme ardışık hattını Azure'a geçirin. Görüntüleri özelleştirmek için varolan komutlarınızı, komutlarınızı ve işlemlerinizi kullanın.
- VHD formatında görüntülerin oluşturulması.
 

## <a name="regions"></a>Bölgeler
Azure Resim Oluşturucu Hizmeti bu bölgelerde önizleme için kullanılabilir. Görüntüler bu bölgelerin dışına dağıtılabilir.
- Doğu ABD
- Doğu ABD 2
- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- Batı Avrupa

## <a name="os-support"></a>İşletim sistemi desteği
AIB, Azure Marketi taban işletim sistemi görüntülerini destekleyecektir:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7.6, 7.7
- CentOS 7.6, 7.7
- SLES 12 SP4
- SLES 15, SLES 15 SP1
- Windows 10 RS5 Kurumsal/Kurumsal çok oturumlu/Profesyonel
- Windows 2016
- Windows 2019

RHEL ISOs desteği küçümsüyor, daha fazla bilgi için lütfen Şablon belgelerini inceleyin.

## <a name="how-it-works"></a>Nasıl çalışır?


![Azure Image Builder'ın kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder.png)

Azure Image Builder, bir Azure kaynak sağlayıcısı tarafından erişilebilen tam olarak yönetilen bir Azure hizmetidir. Azure Image Builder işleminin üç ana bölümü vardır: kaynak, özelleştirme ve dağıtma, bunlar şablonda temsil edilir. Aşağıdaki diyagram, bazı özellikleri ile bileşenleri gösterir. 
 


**Görüntü Oluşturucu işlemi** 

![Azure Image Builder işleminin kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. .json dosyası olarak Resim Şablonu oluşturun. Bu .json dosyası görüntü kaynağı, özelleştirmeler ve dağıtım hakkında bilgi içerir. Azure Image Builder [GitHub deposunda](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts)birden çok örnek vardır.
1. Hizmete gönderin, bu belirttiğiniz kaynak grubunda bir Resim Şablonu yapı oluşturur. Arka planda, Image Builder kaynak görüntüyü veya ISO'yu ve gerektiğinde komut dosyalarını indirir. Bunlar, aboneliğinizde otomatik olarak oluşturulan ayrı bir kaynak grubunda, IT_\<DestinationResourceGroup\<>_ TemplateName> biçiminde depolanır. 
1. Görüntü Şablonu oluşturulduktan sonra görüntüyü oluşturabilirsiniz. Arka planda Image Builder, hedef kaynak grubunda bir VM (varsayılan boyut: Standard_D1_v2), ağ, genel IP,\<NSG\<ve depolama oluşturmak için şablon ve kaynak dosyalarını IT_ HedefKaynak Grubu>_ ŞablonAdı> kaynak grubunda kullanır.
1. Görüntü oluşturmanın bir parçası olarak, Resim oluşturucu görüntüyü şablona göre dağıtır,\<ardından IT_\<DestinationResourceGroup>_ TemplateName'deki ek kaynakları siler> işlem için oluşturulan kaynak grubu.


## <a name="permissions"></a>İzinler

Azure VM Image Builder'ın görüntüleri yönetilen resimlere veya Paylaşılan Resim Galerisi'ne dağıtmasına izin vermek için kaynak gruplarında "Azure Sanal Makine Resim Oluşturucusu" (uygulama kimliği: cf32a0cc-373c-47c9-9156-0db11f6a6dfc) hizmeti için 'Katılımcı' izinleri sağlamanız gerekir. 

Varolan özel yönetilen bir resim veya resim sürümünü kullanıyorsanız, Azure Resim Oluşturucusu'nun bu kaynak gruplarına en az 'Reader' erişimi gerekir.

Azure CLI'yi kullanarak erişim atayabilirsiniz:

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName>
```

PowerShell'i kullanarak erişim atayabilirsiniz:

```azurePowerShell-interactive
New-AzRoleAssignment -ObjectId ef511139-6170-438e-a6e1-763dc31bdf74 -Scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName> -RoleDefinitionName Contributor
```


Hizmet hesabı bulunamazsa, rol atamasını eklediğiniz abonelik kaynak sağlayıcısı için henüz kaydedilmemiş olabilir.


## <a name="costs"></a>Maliyetler
Azure Image Builder ile görüntü oluştururken, oluştururken ve depolarken bazı hesaplama, ağ oluşturma ve depolama maliyetlerine maruz kalacaktır. Bu maliyetler, özel görüntülerin el ile oluşturulmasında oluşan maliyetlere benzer. Kaynaklar için Azure fiyatlarınızdan ücretlendirilirsiniz. 

Görüntü oluşturma işlemi sırasında, dosyalar indirilir ve `IT_<DestinationResourceGroup>_<TemplateName>` kaynak grubunda depolanır ve bu da küçük bir depolama maliyetine neden olur. Bunları saklamak istemiyorsanız, resim yapıldıktan sonra **Resim Şablonu'nu** silin.
 
Image Builder, D1v2 VM boyutu ve VM için gereken depolama ve ağ kullanarak bir VM oluşturur. Bu kaynaklar oluşturma işlemi süresince devam eder ve Görüntü Oluşturucu görüntüyü oluşturmayı bitirdikten sonra silinir. 
 
Azure Image Builder, görüntüyü seçtiğiniz bölgelere dağıtır ve bu da ağ çıkış ücretlerine neden olabilir.
 
## <a name="next-steps"></a>Sonraki adımlar 
 
Azure Image Builder'ı denemek için [Linux](../articles/virtual-machines/linux/image-builder.md) veya [Windows](../articles/virtual-machines/windows/image-builder.md) görüntüleri oluşturmak için makalelere bakın.
 
 
