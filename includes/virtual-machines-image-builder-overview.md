---
author: cynthn
ms.author: cynthn
ms.date: 05/05/2020
ms.topic: include
ms.service: virtual-machines-linux
manager: gwallace
ms.openlocfilehash: 11a9b8609218a6cf56a789b18094d048e26d4af8
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83343346"
---
Standartlaştırılmış sanal makine (VM) görüntüleri, kuruluşların buluta geçiş yapmasına ve dağıtımlarda tutarlılık sağlamanıza olanak tanır. Görüntüler genellikle önceden tanımlanmış güvenlik ve yapılandırma ayarlarını ve gerekli yazılımları içerir. Kendi görüntüleme işlem hattınızı ayarlamak için zaman, altyapı ve kurulum gerekir, ancak Azure VM Image Builder sayesinde görüntünüzü açıklayan basit bir yapılandırma sağlamanız, hizmete göndermeniz ve görüntünün oluşturulup dağıtılması sağlanır.
 
Azure VM görüntü Oluşturucu (Azure görüntü Oluşturucu), bir Windows veya Linux tabanlı Azure Marketi görüntüsü, mevcut özel görüntüler veya Red Hat Enterprise Linux (RHEL) ISO ile başlamanıza ve kendi özelleştirmelerinizi eklemeye başlamanızı sağlar. Görüntü Oluşturucu [HashiCorp Packer](https://packer.io/)üzerinde oluşturulduğundan, mevcut Packer kabuğu sağlayıcısı oluştur betiklerinizi de içeri aktarabilirsiniz. Ayrıca, [Azure Paylaşılan görüntü galerisinde](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries)bir yönetilen görüntü veya VHD olarak, görüntülerinizi nerede barındırıyorsanız istediğinizi belirtebilirsiniz.

> [!IMPORTANT]
> Azure görüntü Oluşturucu Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="preview-features"></a>Önizleme özellikleri

Önizleme için, bu özellikler desteklenir:

- En düşük güvenlik ve kurumsal yapılandırmalarınızı içeren altın taban çizgisi görüntülerinin oluşturulması ve departmanların ihtiyaçları için daha fazla özelleştirme yapmasına izin vermek.
- Var olan görüntülerin düzeltme eki uygulama, görüntü Oluşturucu var olan özel görüntüleri sürekli olarak yamanız için izin verir.
- Mevcut yapılandırma sunucularına (DSC, Chef, Pupevcil hayvan vb.), dosya paylaşımlarına veya diğer yönlendirilebilir sunuculara/hizmetlere bağlanabilmeniz için görüntü oluşturucuyu var olan sanal ağlarınıza bağlayın.
- Azure Paylaşılan görüntü Galerisi ile tümleştirme, görüntüleri küresel olarak dağıtmanıza, sürümüne ve ölçeklendirmenize olanak tanır ve size bir görüntü yönetim sistemi sağlar.
- Mevcut görüntü oluşturma işlem hatlarıyla tümleştirme, işlem hattınızdan görüntü Oluşturucu 'Yu çağırın veya basit önizleme resmi Oluşturucu Azure DevOps görevini kullanın.
- Mevcut bir görüntü özelleştirme işlem hattını Azure 'a geçirin. Görüntüleri özelleştirmek için mevcut betikleri, komutlarınızı ve işlemlerinizi kullanın.
- Azure Stack desteklemek için VHD biçimindeki görüntülerin oluşturulması.
 

## <a name="regions"></a>Bölgeler
Azure görüntü Oluşturucu hizmeti bu bölgelerde önizleme için kullanılabilir olacak. Görüntüler, bu bölgelerin dışına dağıtılabilir.
- Doğu ABD
- Doğu ABD 2
- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- Batı Avrupa

## <a name="os-support"></a>İşletim sistemi desteği
AıB, Azure Marketi temel işletim sistemi görüntülerini destekleyecektir:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7,6, 7,7
- CentOS 7,6, 7,7
- SLES 12 SP4
- SLES 15, SLES 15 SP1
- Windows 10 RS5 kurumsal/kurumsal çoklu oturum/profesyonel
- Windows 2016
- Windows 2019

RHEL ISOs desteği artık desteklenmiyor.
## <a name="how-it-works"></a>Nasıl çalışır?


![Azure Image Builder kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder.png)

Azure görüntü Oluşturucu, bir Azure Kaynak sağlayıcısı tarafından erişilebilen, tam olarak yönetilen bir Azure hizmetidir. Azure görüntü Oluşturucu işleminin üç ana bölümü vardır: kaynak, özelleştirme ve dağıtma, bunlar bir şablonda temsil edilir. Aşağıdaki diyagramda, bazı özellikleriyle birlikte bileşenler gösterilmektedir. 
 


**Image Builder işlemi** 

![Azure Image Builder işleminin kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. Görüntü şablonunu bir. JSON dosyası olarak oluşturun. Bu. JSON dosyası, görüntü kaynağı, özelleştirmeler ve dağıtım hakkındaki bilgileri içerir. [Azure Image Builder GitHub deposunda](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts)birden çok örnek vardır.
1. Bu hizmeti hizmetine gönderirseniz, belirttiğiniz kaynak grubunda bir görüntü şablonu yapıtı oluşturulur. Arka planda, görüntü Oluşturucu kaynak görüntüyü veya ISO ve komut dosyalarını gerektiği şekilde indirir. Bunlar, aboneliğinizde otomatik olarak oluşturulan ayrı bir kaynak grubunda depolanır: IT_ \< destinationresourcegroup>_ \< templatename>. 
1. Görüntü şablonu oluşturulduktan sonra görüntüyü oluşturabilirsiniz. Arka plan resmi tasarımcısında, IT_ \< destinationresourcegroup>_ \< templatename> kaynak grubunda bir VM (varsayılan boyut: Standard_D1_v2), ağ, genel IP, NSG ve depolama oluşturmak için şablon ve kaynak dosyalarını kullanır.
1. Görüntü oluşturma işleminin bir parçası olarak, görüntü Oluşturucu görüntüyü şablona göre dağıtır ve ardından \< işlem için oluşturulan IT_ destinationresourcegroup>_ \< templatename> kaynak grubundaki ek kaynakları siler.


## <a name="permissions"></a>İzinler
(AıB) için kaydolmanız durumunda bu, AıB hizmetine bir hazırlama kaynak grubu (IT_ *) oluşturma, yönetme ve silme izni verir ve bu, görüntü derlemesi için gerekli olan kaynak ekleme haklarına sahip olur. Bu işlem, başarılı bir kayıt sırasında aboneliğinizde kullanılabilir hale getirilen bir AıB hizmet sorumlusu adı (SPN) tarafından yapılır.

Azure VM Image Builder 'ın görüntüleri yönetilen görüntülere veya paylaşılan bir görüntü galerisine dağıtmasını sağlamak için, görüntüleri okuma ve yazma izinlerine sahip Azure Kullanıcı tarafından atanan bir kimlik oluşturmanız gerekir. Azure depolama 'ya erişiyorsanız, bu durumda özel kapsayıcıları okumak için izinler gerekir.

Başlangıçta kimlik oluşturma hakkında [Azure Kullanıcı tarafından atanan yönetilen kimlik belgelerini oluşturmanız](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli) gerekir.

İzin vermek için gereken kimliğe sahip olduktan sonra, bunu yapmak için bir Azure özel rol tanımı kullanabilir ve ardından Kullanıcı tarafından atanan yönetilen kimliği özel rol tanımını kullanacak şekilde atayabilirsiniz.

İzinler [burada](https://github.com/danielsollondon/azvmimagebuilder/blob/master/aibPermissions.md#azure-vm-image-builder-permissions-explained-and-requirements)ayrıntılı olarak açıklanırlar ve örnekler bunun nasıl uygulandığını gösterir.

> [Note!] Daha önce AıB ile, AıB SPN 'yi kullanacaksınız ve görüntü kaynak gruplarına SPN izinleri vermelisiniz. Bu modelden daha sonraki yetenekler için izin vermek üzere taşınıyoruz. 1 Haziran 2020 ' den görüntü Oluşturucu kullanıcı tarafından atanan bir kimliğe sahip olmayan şablonları kabul etmez, var olan şablonların bir [Kullanıcı kimliği](https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-json?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fbreadcrumb%2Ftoc.json#identity)ile hizmete yeniden gönderilmesi gerekir. Burada örnek olarak, Kullanıcı tarafından atanan bir kimlik oluşturma ve bunları bir şablona ekleme işlemlerinin nasıl yapılacağı gösterilmektedir.

## <a name="costs"></a>Maliyetler
Azure Image Builder ile görüntü oluştururken, derlerken ve depolarken bazı işlem, ağ ve depolama maliyetlerine tabi olursunuz. Bu maliyetler, el ile özel görüntüler oluşturma bölümünde tahakkuk eden maliyetlere benzer. Kaynaklar için Azure ücretlerinizi ücretlendirilecektir. 

Görüntü oluşturma işlemi sırasında dosyalar indirilir ve kaynak grubunda depolanır ve bu, `IT_<DestinationResourceGroup>_<TemplateName>` küçük bir depolama maliyetlerine neden olur. Bunları tutmak istemiyorsanız, görüntü oluşturulduktan sonra **görüntü şablonunu** silin.
 
Image Builder, VM için gereken bir D1v2 VM boyutu, depolama alanı ve ağ kullanarak bir VM oluşturur. Bu kaynaklar, derleme işleminin süresi boyunca son verilecek ve görüntü Oluşturucu görüntüyü oluşturmayı tamamladığında silinir. 
 
Azure Image Builder, görüntüyü seçtiğiniz bölgelere dağıtır ve bu da ağ çıkış ücretleri doğuracaktır.
 
## <a name="next-steps"></a>Sonraki adımlar 
 
Azure görüntü Oluşturucu 'Yu denemek için [Linux](../articles/virtual-machines/linux/image-builder.md) veya [Windows](../articles/virtual-machines/windows/image-builder.md) görüntülerini oluşturma makalelerine bakın.
 
