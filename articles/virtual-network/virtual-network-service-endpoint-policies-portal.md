---
title: Hizmet bitiş noktası ilkeleri oluşturma ve ilişkilendirme - Azure portalı
titlesuffix: Azure Virtual Network
description: Bu makalede, Azure portalını kullanarak hizmet bitiş noktası ilkelerini nasıl ayarlayıp ilişkilendireceklerini öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: RDhillon
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: rdhillon
ms.openlocfilehash: d26fd2fec5f9d5ab8e9d82ff2c6bd83b11c72e99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651271"
---
# <a name="create-change-or-delete-service-endpoint-policy-using-the-azure-portal"></a>Azure portalını kullanarak hizmet bitiş noktası ilkesi oluşturma, değiştirme veya silme

Hizmet bitiş noktası ilkeleri, sanal ağ trafiğini hizmet bitiş noktaları üzerinden belirli Azure kaynaklarına filtrelemenize olanak tanır. Hizmet uç noktası ilkelerini bilmiyorsanız, daha fazla bilgi edinmek için [hizmet uç noktası ilkelerine genel bakış](virtual-network-service-endpoint-policies-overview.md) alabilirsiniz.

 Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Hizmet bitiş noktası ilkesi oluşturma
> * Hizmet uç noktası ilkesi tanımı oluşturma
> * Alt ağ ile sanal ağ oluşturma
> * Hizmet bitiş noktası ilkesini bir alt ağla ilişkilendirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-service-endpoint-policy"></a>Hizmet bitiş noktası ilkesi oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. Arama bölmesinde ,"hizmet bitiş noktası ilkesi" yazın ve **Hizmet bitiş noktası ilkesini** seçin ve ardından **Oluştur'u**seçin.

![Hizmet bitiş noktası ilkesi oluşturma](./media/virtual-network-service-endpoint-policies-portal/create-sep-resource.png)

3. **Temel Bilgiler'de** aşağıdaki bilgileri girin veya seçin 

   - Abonelik : İlke için aboneliğinizi seçin
   - Kaynak grubu : **Yeni Oluştur'u** seçin ve *myResourceGroup'a* girin
   - Adı : myEndpointPolicy
   - Yer : Merkez ABD
 
   ![Hizmet uç noktası ilkesi temelleri oluşturma](./media/virtual-network-service-endpoint-policies-portal/create-sep-basics.png)

4. Kaynak bölmesine + **Kaynak** Altına **Ekle'yi** seçin ve aşağıdaki bilgileri girin veya seçin **Add a resource**

   - Hizmet : Yalnızca **Microsoft.Storage** Service Endpoint İlkeleri ile kullanılabilir
   - Kapsam : Tek **Hesap'tan**birini seçin, **abonelikteki tüm hesaplar** ve **kaynak grubundaki tüm hesaplar**
   - Abonelik : Depolama hesabı için aboneliğinizi seçin. İlke ve depolama hesapları farklı aboneliklerde olabilir.
   - Kaynak grubu : Kaynak grubunuzu seçin. Kapsam "Kaynak grubundaki tüm hesaplar" veya "Tek hesap" olarak ayarlanmışsa gereklidir.  
   - Kaynak : Seçili Abonelik veya Kaynak Grubu altında Azure Depolama kaynağınızı seçin
   - Kaynak eklemeyi bitirmek için alttaki **Ekle** düğmesine tıklayın

   ![Hizmet uç noktası ilkesi tanımı - kaynak](./media/virtual-network-service-endpoint-policies-portal/create-sep-add-resource.png)

   - Yukarıdaki adımları gerektiği gibi yineleyerek daha fazla kaynak ekleyin

5. İsteğe bağlı: Girin veya seçin, **Etiketler**aşağıdaki bilgileri:
   
   - Anahtar : İlke için anahtarınızı seçin. Örn: Bölüm     
   - Değer : Anahtar için değer çiftini girin. Ör: Finans

6. **Gözden Geçir + Oluştur'u**seçin. Bilgileri doğrulayın ve **Oluştur'u**tıklatın. Daha fazla değişme yapmak için **önceki'** yi tıklatın. 

   ![Hizmet uç noktası ilkesi son doğrulamaları oluşturma](./media/virtual-network-service-endpoint-policies-portal/create-sep-review-create.png)
  
## <a name="view-endpoint-policies"></a>Uç nokta ilkelerini görüntüleyin 

1. Portaldaki *Tüm hizmetler* kutusunda, hizmet bitiş noktası *ilkeleri*yazmaya başlayın. **Hizmet Bitiş Noktası İlkeleri'ni**seçin.
2. **Abonelikler**altında, aşağıdaki resimde gösterildiği gibi abonelik ve kaynak grubunuzu seçin

   ![İlkeyi göster](./media/virtual-network-service-endpoint-policies-portal/sep-view.png)
       
3. İlkeyi seçin ve daha fazla ilke tanımı görüntülemek veya eklemek için **İlke Tanımları'nı** tıklatın.

   ![İlke tanımlarını göster](./media/virtual-network-service-endpoint-policies-portal/sep-policy-definition.png)

4. İlkenin ilişkili olduğu alt ağları görüntülemek için **İlişkili alt ağları** seçin. Henüz hiçbir alt ağ ilişkilendirilmezse, bir sonraki adımdaki yönergeleri izleyin.

   ![İlişkili alt ağlar](./media/virtual-network-service-endpoint-policies-portal/sep-associated-subnets.png)
 
5. Bir ilkeyi bir alt ağla ilişkilendirme

>[!WARNING] 
> İlkeyi verilen alt ağa ilişkilendirmeden önce alt ağdan erişilen tüm kaynakların ilke tanımına eklenmiş olduğundan emin olun. İlke ilişkilendirildikten sonra, yalnızca hizmet bitiş noktaları üzerinden *izin verilen* kaynaklara erişime izin verilir. 
>
> Ayrıca, hizmet bitiş noktası ilkesiyle ilişkilendirilmekte olan alt ağda yönetilen Azure hizmetinin bulunmadığından emin olun

- Bir ilkeyi bir alt ağla ilişkilendirebilmek için önce sanal bir ağ ve alt ağ oluşturmanız gerekir. Bu sorundan yardım almak için lütfen [Sanal Ağ Oluştur](./quick-create-portal.md) makalesine bakın.

- Sanal ağa ve alt ağ kurulumu yaptıktan sonra, Azure Depolama için Sanal Ağ Hizmeti Bitiş Noktalarını yapılandırmanız gerekir. Sanal Ağ bıçak, **Hizmet bitiş noktaları**seçin ve sonraki bölmede **Microsoft.Storage** seçin ve **Subnets** altında istenilen VNet veya Subnet seçin

- Şimdi, aşağıda gösterildiği gibi Subnet için Hizmet Bitiş Noktası'nı yapılandırmadan önce Hizmet Bitiş Noktası ilkelerini zaten oluşturduysanız, yukarıdaki bölmedeki açılır noktadan Hizmet Bitiş Noktası İlkesi'ni seçmeyi seçebilirsiniz

    ![Hizmet bitiş noktası oluştururken alt ağı ilişkilendirme](./media/virtual-network-service-endpoint-policies-portal/vnet-config-service-endpoint-add-sep.png)

- VEYA Hizmet Bitiş Noktaları zaten yapılandırıldıktan sonra Hizmet Bitiş Noktası ilkelerini ilişkilendiriyorsanız, aşağıda gösterildiği gibi **İlişkili Alt Ağlar** bölmesine yönlendirerek Alt ağı Hizmet Bitiş Noktası İlkesi açısından ilişkilendirmeyi seçebilirsiniz

    ![SEP ile ortak alt ağ](./media/virtual-network-service-endpoint-policies-portal/sep-edit-subnet-association.png)

>[!WARNING] 
>Tüm bölgelerdeki Azure Depolama kaynaklarına erişim, bu alt ağdan Hizmet Bitiş Noktası İlkesi uyarınca kısıtlanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir hizmet bitiş noktası ilkesi oluşturdunuz ve bir alt ağla ilişkilendirdin. Hizmet bitiş noktası ilkeleri hakkında daha fazla bilgi edinmek için [hizmet uç noktası ilkelerine genel bakış abakın.](virtual-network-service-endpoint-policies-overview.md)
