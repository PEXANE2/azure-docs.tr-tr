---
title: Azure sanal makineleri için birden fazla IP adresi - Portal | Microsoft Dokümanlar
description: Azure portalını kullanarak sanal bir makineye birden çok IP adresi atamayı öğrenin | Kaynak Yöneticisi.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: anavin
ms.openlocfilehash: a5bb9bb9c584aef8ac79b3c275d01d3c498da843
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060626"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Azure portalını kullanarak sanal makinelere birden çok IP adresi atama

> [!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
> 
> Bu makalede, Azure portalını kullanarak Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla sanal makine (VM) nasıl oluşturulacak açıklanmaktadır. Klasik dağıtım modeli aracılığıyla oluşturulan kaynaklara birden çok IP adresi atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için [dağıtım modellerini anlayın](../resource-manager-deployment-model.md) makalesini okuyun.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="create-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Birden çok IP adresi ile VM oluşturma

Birden çok IP adresi veya statik özel IP adresi içeren bir VM oluşturmak istiyorsanız, bu adresi PowerShell veya Azure CLI kullanarak oluşturmanız gerekir. Nasıl yapılacağını öğrenmek için, bu makalenin üst kısmındaki PowerShell veya CLI seçeneklerini tıklatın. Tek bir dinamik özel IP adresine ve (isteğe bağlı olarak) tek bir genel IP adresine sahip bir VM oluşturabilirsiniz. [Windows VM Oluştur](../virtual-machines/virtual-machines-windows-hero-tutorial.md) veya Linux [VM](../virtual-machines/linux/quick-create-portal.md) makaleleri oluşturma adımlarını izleyerek portalı kullanın. VM'yi oluşturduktan sonra, IP adresi türünü dinamikten statike değiştirebilir ve ip adresleri ekle adımlarını bu makalenin [VM bölümüne](#add) izleyerek portalı kullanarak ek IP adresleri ekleyebilirsiniz.

## <a name="add-ip-addresses-to-a-vm"></a><a name="add"></a>VM'ye IP adresleri ekleme

Ardından gelen adımları tamamlayarak Azure ağ arabirimine özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerdeki örnekler, [senaryoda](#scenario)açıklanan üç IP yapılandırması ile zaten bir VM'niz olduğunu varsayar, ancak gerekli değildir.

### <a name="core-steps"></a><a name="coreadd"></a>Temel adımlar

1. Azure portalına göz https://portal.azure.com atın ve gerekirse bu portalda oturum açın.
2. Portalda, filtre kutusunda *sanal makineleri* > **diğer hizmetleri** tıklatın ve ardından **Sanal makineler'i**tıklatın.
3. Sanal **makineler** bölmesinde, IP adresleri eklemek istediğiniz VM'yi tıklatın. **Ağ** Sekmesine gidin. Sayfadaki **Ağ arabirimine** tıklayın. Aşağıdaki resimde gösterildiği gibi: 


    ![VM’ye genel IP adresi ekleme](./media/virtual-network-multiple-ip-addresses-portal/figure200319.png)
4. Ağ **arabirimi** bölmesinde **IP yapılandırmalarını**tıklatın.

5. Seçtiğiniz NIC için görünen bölmede **IP yapılandırmalarını**tıklatın. **Eklemek**istediğiniz IP adresi türüne bağlı olarak, takip eden bölümlerden birinde adımları tamamla'yı tıklatın ve ardından **Tamam'ı**tıklatın. 

### <a name="add-a-private-ip-address"></a>**Özel IP adresi ekleme**

Yeni bir özel IP adresi eklemek için aşağıdaki adımları tamamlayın:

1. Bu makalenin [Temel adımlar](#coreadd) bölümündeki adımları tamamlayın.
2. **Ekle**’ye tıklayın. Görünen **IP ekle yapılandırma** bölmesinde, *Statik* özel IP adresi olarak *10.0.0.7* içeren *IPConfig-4* adında bir IP yapılandırması oluşturun ve **ardından Tamam'ı**tıklatın.

    > [!NOTE]
    > Statik bir IP adresi eklerken, NIC'nin bağlı olduğu alt ağüzerinde kullanılmayan, geçerli bir adres belirtmeniz gerekir. Seçtiğiniz adres kullanılamıyorsa, portal IP adresi için bir X görüntüler ve farklı bir adres seçmeniz gerekir.

3. Tamam'ı tıklattığınızda bölme kapanır ve listelenen yeni IP yapılandırmasını görürsünüz. **IP ekle yapılandırma** bölmesini kapatmak için **Tamam'ı** tıklatın.
4. Ek IP yapılandırmaları eklemek için **Ekle'yi** tıklatabilir veya IP adresleri eklemeyi bitirmek için tüm açık bıçakları kapatabilirsiniz.
5. Bu makalenin Bir VM işletim sistemi bölümüne IP [ekle adreslerindeki](#os-config) adımları tamamlayarak özel IP adreslerini VM işletim sistemine ekleyin.

### <a name="add-a-public-ip-address"></a>Herkese açık bir IP adresi ekleme

Ortak BIR IP adresi, ortak bir IP adresi kaynağını yeni bir IP yapılandırmasına veya varolan bir IP yapılandırmasına ilişkilendirerek eklenir.

> [!NOTE]
> Genel IP adreslerinin nominal ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun. Abonelikte kullanılabilecek genel IP adreslerinin sayısının bir sınırı vardır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
> 

### <a name="create-a-public-ip-address-resource"></a><a name="create-public-ip"></a>Ortak IP adresi kaynağı oluşturma

Ortak IP adresi, ortak bir IP adresi kaynağının ayarlarından biridir. Şu anda bir IP yapılandırmasına ilişkilendirmek istediğiniz bir IP yapılandırması ile ilişkili olmayan bir genel IP adresi kaynağınız varsa, aşağıdaki adımları atlayın ve istediğiniz gibi aşağıdaki bölümlerden birinde adımları tamamlayın. Kullanılabilir bir genel IP adresi kaynağınız yoksa, oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalına göz https://portal.azure.com atın ve gerekirse bu portalda oturum açın.
3. Portalda, kaynak > **Oluşturma** > Ortak IP adresi **oluşturun'u**tıklatın.**Public IP address**
4. Görünen **ortak IP adresi oluştur** bölmesinde, **bir Ad**girin, bir IP adresi **atama** türü, **Abonelik,** **Kaynak grubu**ve **Konum**seçin, ardından aşağıdaki resimde gösterildiği gibi **Oluştur'u**tıklatın:

    ![Ortak IP adresi kaynağı oluşturma](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Ortak IP adresi kaynağını bir IP yapılandırmasına ilişkilendirmek için izleyen bölümlerden birinde adımları tamamlayın.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Ortak IP adresi kaynağını yeni bir IP yapılandırması ile ilişkilendirme

1. Bu makalenin [Temel adımlar](#coreadd) bölümündeki adımları tamamlayın.
2. **Ekle**’ye tıklayın. Görünen **IP ekle yapılandırma** bölmesinde *IPConfig-4*adında bir IP yapılandırması oluşturun. Genel **IP adresini** etkinleştirin ve görünen genel IP **adresi seç** bölmesinden varolan, kullanılabilir bir genel IP adresi kaynağını seçin.

    Genel IP adresi kaynağını seçtikten sonra **Tamam'ı** tıklatın ve bölme kapanır. Varolan bir genel IP adresiniz yoksa, bu makalenin [genel IP adresi kaynak](#create-public-ip) oluşturma bölümündeki adımları tamamlayarak bir tane oluşturabilirsiniz. 

3. Yeni IP yapılandırmasını gözden geçirin. Özel bir IP adresi açıkça atanmamış olsa da, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiğinden, bir tanesi otomatik olarak IP yapılandırmasına atanır.
4. Ek IP yapılandırmaları eklemek için **Ekle'yi** tıklatabilir veya IP adresleri eklemeyi bitirmek için tüm açık bıçakları kapatabilirsiniz.
5. Bu makalenin Bir VM işletim sistemi bölümüne IP adresleri ekle işletim sisteminizin adımlarını tamamlayarak Özel IP adresini [VM işletim sistemine](#os-config) ekleyin. İşletim sistemine genel IP adresini eklemeyin.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Ortak IP adresi kaynağını varolan bir IP yapılandırması ile ilişkilendirme

1. Bu makalenin [Temel adımlar](#coreadd) bölümündeki adımları tamamlayın.
2. Ortak IP adresi kaynağını eklemek istediğiniz IP yapılandırmasını tıklatın.
3. Görünen IPConfig bölmesinde **IP adresini**tıklatın.
4. Görünen **genel IP adresini seç** bölmesinde herkese açık bir IP adresi seçin.
5. **Kaydet'i** tıklatın ve bölmeler kapanır. Varolan bir genel IP adresiniz yoksa, bu makalenin [genel IP adresi kaynak](#create-public-ip) oluşturma bölümündeki adımları tamamlayarak bir tane oluşturabilirsiniz.
3. Yeni IP yapılandırmasını gözden geçirin.
4. Ek IP yapılandırmaları eklemek için **Ekle'yi** tıklatabilir veya IP adresleri eklemeyi bitirmek için tüm açık bıçakları kapatabilirsiniz. İşletim sistemine genel IP adresini eklemeyin.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
