---
title: Azure DevTest Labs'de laboratuvar oluşturma | Microsoft Belgeleri
description: Bu makale, Azure portal ve Azure DevTest Labs kullanarak laboratuvar oluşturma sürecinde size yol gösterir.
ms.topic: article
ms.date: 10/12/2020
ms.openlocfilehash: 9fa33a59dd35bfe3469f30f2349f8a08c45bd5e3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92058438"
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma

Azure DevTest Labs'deki bir laboratuvar, Sanal Makineler (VM'ler) gibi kaynaklardan oluşan grupları kapsayan ve limitlerin yanı sıra kotalar belirleyerek söz konusu kaynakları daha iyi yönetmenizi sağlayan bir altyapıdır. Bu makalede, Azure portalını kullanarak laboratuvar oluşturma işlemi ayrıntılı olarak açıklanmıştır.

## <a name="prerequisites"></a>Önkoşullar

Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## <a name="get-started-with-azure-devtest-labs-in-minutes"></a>Azure DevTest Labs ile dakikalar içinde çalışma başlama

Aşağıdaki bağlantıya tıklayarak, Azure DevTest Labs yeni bir laboratuvar oluşturmaya başlayabilmeniz için Azure portal sayfasına aktarılacaktır.

[Azure DevTest Labs ile dakikalar içinde çalışma başlama](https://go.microsoft.com/fwlink/?LinkID=627034&clcid=0x409)

## <a name="fill-out-settings-for-your-new-account"></a>Yeni hesabınızın ayarlarını doldurun

**DevTest Labs oluşturma** sayfasında, aşağıdaki ayarları doldurun.

> [!TIP]
> Her sayfanın en altında, **Otomasyon için bir şablon indirmeyi** sağlayan bir bağlantı bulacaksınız.

### <a name="basic-settings"></a>Temel ayarlar

Varsayılan olarak, **temel ayarlar** sekmesini görürsünüz. 

![temel ayarlar](./media/devtest-lab-create-lab/basic-settings.png)

Bu değerleri doldurun:

|Ad|Açıklama|
|---|---|
|**Abonelik** | Gereklidir. Laboratuvarla ilişkilendirilecek **Abonelik** seçimini yapın.|
|**Kaynak grubu**| Gereklidir. Laboratuvar için **kaynak grubu için bir ad** girin. Bir tane yoksa yeni bir tane oluşturun.|
|**Laboratuvar adı**| Gereklidir. Laboratuvar için bir **ad** girin.|
|**Konum**|Gereklidir. Laboratuvarın depolayabileceği bir konum seçin.|
|**Genel ortamlar**| Bkz. [genel ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md).

### <a name="auto-shutdown-settings"></a>Otomatik olarak kapatılıyor ayarları

Ayarlarını görmek için **otomatik kapatılma** sayfasına geçin. Otomatik kapatma, bir laboratuvardaki tüm makineleri her gün zamanlanan bir saatte otomatik olarak kapatmanızı sağlar.

![Otomatik olarak kapatılıyor sekmesi](./media/devtest-lab-create-lab/auto-shutdown.png)

Sayfasında, **Otomatik kapatmayı** etkinleştirebilir ve tüm laboratuvarın VM 'lerinin otomatik olarak kapatılmasını sağlayacak parametreleri tanımlayabilirsiniz. VM'nin otomatik olarak kapatılmasını istediğiniz zamanı belirtmenizi sağlayan otomatik kapanma özelliği temel olarak maliyet tasarrufu sağlayan bir özelliktir. Laboratuvar oluşturulduktan sonra otomatik kapatmalar ayarlarını, [Azure DevTest Labs laboratuvar için tüm Ilkeleri yönetme](./devtest-lab-set-lab-policy.md#set-auto-shutdown)makalesinde açıklanan adımları izleyerek değiştirebilirsiniz.

### <a name="networking"></a>Ağ

Laboratuvar oluştururken sizin için varsayılan bir ağ oluşturulur. Ayarı istediğiniz gibi değiştirmek/yapılandırmak için **ağ** sekmesine geçin. Örneğin, var olan bir sanal ağı seçin.

![Ağ sekmesi ](./media/devtest-lab-create-lab/networking.png)

### <a name="tags"></a>Etiketler

Laboratuvarda oluşturacağınız tüm kaynaklara eklenecek özel etiketler oluşturmak istiyorsanız **Etiketler** için **AD** ve **DEĞER** bilgilerini girin. Laboratuvarları kategorilere göre yönetmek ve düzenlemek istiyorsanız etiketler faydalı olacaktır. Laboratuvarı oluşturduktan sonra etiket ekleme dahil olmak üzere etiketler hakkında daha fazla bilgi için bkz. [Laboratuvara etiket ekleme](devtest-lab-add-tag.md).

![Etiketler sekmesi ](./media/devtest-lab-create-lab/tags.png)

### <a name="review-and-create"></a>Gözden geçir ve oluştur

İşiniz bittiğinde **Oluştur**' u seçin. Portal sayfasının sağ üst kısmındaki **Bildirimler** alanını izleyerek Laboratuvar oluşturma işleminin durumunu izleyebilirsiniz. 

![Sekme oluştur](./media/devtest-lab-create-lab/create-1.png)

## <a name="completed-the-creation"></a>Oluşturma tamamlandı

Tamamlandıktan sonra, sayfanın alt kısmında ve bildirim penceresinde **Kaynağa Git** düğmesi görünür. Alternatif olarak, yeni oluşturulan Laboratuvarı laboratuvarlar listesinde görmek için **DevTest Labs** sayfasını yenileyin.  

![Hizmet oluşturuluyor](./media/devtest-lab-create-lab/create-2.png)

**Kaynağa Git** düğmesine basın ve yeni DevTest Labs hesabınızın giriş sayfasına yönlendirilirsiniz.

![Kaynak](./media/devtest-lab-create-lab/go-to-resource.png)

Ayrıca, Azure portal **DevTest Labs** için arama yapabilirsiniz. Listeden yeni hesabınızı seçin ve giriş sayfasına gidin. 

![Hizmet oluşturuldu](./media/devtest-lab-create-lab/created.png)

## <a name="next-steps"></a>Sonraki adımlar

Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

