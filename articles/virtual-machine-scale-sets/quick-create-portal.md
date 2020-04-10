---
title: Quickstart - Azure portalında ayarlanmış sanal makine ölçeği oluşturma
description: Azure portalında sanal makine ölçeğini nasıl hızlı bir şekilde oluşturabilirsiniz öğrenerek dağıtımlarınıza başlayın.
author: ju-shim
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: quickstart
ms.custom: mvc, H1Hack27Feb2017
ms.date: 10/23/2019
ms.author: jushiman
ms.openlocfilehash: 410ae93ea109e5757ccb5b65d353a600574dd595
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010486"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında sanal makine ölçek kümesi oluşturma

Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki sanal makine sayısını el ile ölçeklendirebilir veya CPU, bellek talebi ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Azure Load Balancer daha sonra ölçek kümesindeki sanal makine örneklerine trafiği dağıtır. Bu hızlı başlangıçta, Azure portalında sanal makine ölçek kümesi oluşturacaksınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.


## <a name="log-in-to-azure"></a>Azure'da oturum açma
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Azure [yük dengeleyicisi](../load-balancer/load-balancer-overview.md) gelen trafiği sağlıklı sanal makine örnekleri arasında dağıtır. 

İlk olarak, portalı kullanarak ortak bir Standart Yük Dengeleyicioluşturun. Oluşturduğunuz ad ve ortak IP adresi otomatik olarak yük bakiyesinin ön ucu olarak yapılandırılır.

1. Arama kutusunda yük **dengeleyiciyazın.** Arama sonuçlarında **Market** altında **Yük dengeleyicisini**seçin.
1. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesine aşağıdaki bilgileri girin veya seçin:

    | Ayar                 | Değer   |
    | ---| ---|
    | Abonelik  | Aboneliğinizi seçin.    |    
    | Kaynak grubu | Metin kutusuna **yeni oluştur'u** ve *myVMSSResourceGroup* yazın'ı seçin.|
    | Adı           | *myLoadBalancer*         |
    | Bölge         | **Doğu ABD**’yi seçin.       |
    | Tür          | **Genel'i**seçin.       |
    | SKU           | **Standart'ı**seçin.       |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Genel IP adresi adı  | *MyPip*   |
    | Atama| Statik |

1. İşi **bittiğinde, Gözden Geçir + oluştur'u** seçin 
1. Doğrulamageçtikten sonra **Oluştur'u**seçin. 

![Yük dengeleyici oluşturma](./media/virtual-machine-scale-sets-create-portal/load-balancer.png)

## <a name="create-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma
RHEL, CentOS, Ubuntu veya SLES gibi bir Linux görüntüsü ya da Windows Server görüntüsü ile ölçek kümesi dağıtabilirsiniz.

1. Arama kutusuna **ölçek kümesi** yazın. Sonuçlarda, **Marketplace**altında Sanal **makine ölçek kümelerini**seçin. **Sanal makine ölçeği oluştur ayar** sayfası açılır. 
1. **Temel Bilgiler** sekmesinde, **Proje ayrıntıları** altında, doğru aboneliğin seçildiğinden emin olun, ardından **Yeni oluştur** kaynak grubunu seçin. Ad için *myVMSSResourceGroup* yazın ve ardından **Tamam'ı** seçin. 
1. Ölçek kümenizin adı olarak *myScaleSet* yazın.
1. **Bölge'de,** bölgenize yakın bir bölge seçin.
1. **Orchestrator**için **ScaleSet VM** varsayılan değerini bırakın.
1. **Resim**için bir pazar görüntüsü seçin. Bu örnekte, *Ubuntu Server 18.04 LTS'yi*seçtik.
1. İstediğiniz kullanıcı adını girin ve tercih ettiğiniz kimlik doğrulaması türünü seçin.
   - **Parola** en az 12 karakter uzunluğunda olmalı ve şu dört karmaşıklık gereksiniminden üçünü karşılamalıdır: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. Daha fazla bilgi için [kullanıcı adı ve parola gereksinimlerine](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) bakın.
   - Bir Linux OS disk görüntüsü seçerseniz bunun yerine **SSH genel anahtarını** seçebilirsiniz. Yalnızca genel anahtarınızı sağlayın (örn. *~/.ssh/id_rsa.pub*). [SSH anahtarları oluşturmak ve kullanmak](../virtual-machines/linux/mac-create-ssh-keys.md) için portaldan Azure Cloud Shell’i kullanabilirsiniz.
   
    ![Sanal makine ölçek kümesi oluşturma](./media/virtual-machine-scale-sets-create-portal/quick-create-scaleset.png)

1. Diğer sayfaları taşımak için **İleri'yi** seçin. 
1. **Örnek** ve **Diskler** sayfaları için varsayılanları bırakın.
1. **Ağ** sayfasında, Yük **dengelemesi**altında, ölçek kümesi örneklerini bir yük dengeleyicisinin arkasına koymak için **Evet'i** seçin. 
1. **Yük dengeleme seçeneklerinde**Azure **yük dengeleyicisi'ni**seçin.
1. **Bir yük dengeleyiciseçin**, daha önce oluşturduğunuz *myLoadBalancer'ı* seçin.
1. **Bir arka uç havuzu seçin,** yeni **oluştur'u**seçin , *myBackendPool*yazın, sonra **Oluştur'u**seçin.
1. İşi bittiğinde **Gözden Geçir + oluştur'u**seçin. 
1. Doğrulamageçtikten sonra, ölçek kümesini dağıtmak için **Oluştur'u** seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında kaynak grubunu, ölçek kümesini ve tüm ilgili kaynakları silin. Bunu yapmak için, ölçek kümesi için kaynak grubunu seçin ve sonra **Sil'i**seçin.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure portalında temel bir ölçek kümesi oluşturdunuz. Daha fazla bilgi edinmek için Azure sanal makine ölçek kümelerinin nasıl oluşturulacağı ve yönetileceğine ilişkin öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Azure sanal makine ölçek kümeleri oluşturma ve yönetme](tutorial-create-and-manage-powershell.md)
