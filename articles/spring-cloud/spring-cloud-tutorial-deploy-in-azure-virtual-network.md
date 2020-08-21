---
title: Öğretici-sanal ağda Azure yay bulutu dağıtma
description: Azure yay bulutu 'nı sanal ağda (v-net ekleme) dağıtın.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 07/21/2020
ms.custom: devx-track-java
ms.openlocfilehash: a7905ae0fdbd797d9b544cb71f44b82af1295246
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88688462"
---
# <a name="tutorial-deploy-azure-spring-cloud-in-azure-virtual-network-vnet-injection"></a>Öğretici: Azure sanal ağ 'da Azure yay bulutu dağıtma (VNet ekleme)

Bu öğreticide, sanal ağınızda bir Azure Spring Cloud Service örneğinin nasıl dağıtılacağı açıklanmaktadır. Buna bazen VNet ekleme denir.  

Dağıtım şunları sunar:

* Şirket ağınızdaki Internet 'ten gelen Azure Mağazası bulut uygulamalarının ve hizmet çalışma zamanının yalıtımı
* Azure Spring, şirket içi veri merkezlerinde ve/veya diğer sanal ağlardaki Azure hizmetlerindeki sistemlerle bulut etkileşimi
* Azure Spring Cloud için gelen ve giden ağ iletişimlerini denetlemek için müşterilerin güçleme

## <a name="prerequisites"></a>Ön koşullar
`Microsoft.AppPlatform` [Azure Portal kaynak sağlayıcısını kaydetme](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types#azure-portal) yönergelerine göre veya şu az CLI komutunu çalıştırarak Azure yay bulut kaynak sağlayıcısını kaydetmeniz gerekir:

```azurecli
az provider register --namespace Microsoft.AppPlatform
```
## <a name="virtual-network-requirements"></a>Sanal ağ gereksinimleri
Azure yay bulut hizmeti örneğinizi dağıttığınız sanal ağın aşağıdaki gereksinimleri karşılaması gerekir:

* **Konum**: sanal ağın Azure yay bulut hizmeti örneğiyle aynı konumda bulunması gerekir.
* **Abonelik**: sanal ağın Azure yay bulut hizmeti örneğiyle aynı abonelikte olması gerekir.
* **Alt ağlar**: sanal ağ, bir Azure yay bulut hizmeti örneğine adanmış iki alt ağ içermelidir: 
    * Biri hizmet çalışma zamanı için
    * Bir tane, Spring Boot mikro hizmet uygulamalarınız için. 
    * Bu alt ağlar ve bir Azure yay bulut hizmeti örneği arasında bire bir ilişki vardır. Birden çok hizmet örneğini tek bir alt ağ üzerinde paylaşamazsınız. Dağıttığınız her hizmet örneği için yeni alt ağlar kullanmanız gerekir.
* **Adres alanı**: hizmet çalışma zamanı alt ağı için en fazla/28 blok ve diğer CIDR, Spring Boot mikro hizmet uygulamaları alt ağı için/24 ' e kadar bir CIDR bloğu.
* **Yol tablosu**: alt ağlarda ilişkili mevcut bir rota tablosu olmalıdır.

Aşağıdaki yordamlarda, sanal ağın Azure Spring Cloud örneğini içerecek şekilde kurulumu açıklanır.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Azure yay bulut hizmeti örneğini barındıracak bir sanal ağınız zaten varsa, 1. Adım 2 ve 3 ' ü atlayın. Sanal ağ için alt ağları hazırlamak üzere 4. adımdan başlayabilirsiniz.

1. Azure portalı menüsünden **Kaynak oluştur**'u seçin. Azure Marketi ' nden **ağ**  >  **sanal ağı**' nı seçin.

1. **Sanal ağ oluştur** iletişim kutusunda aşağıdaki bilgileri girin veya seçin:

    |Ayar          |Değer                                             |
    |-----------------|--------------------------------------------------|
    |Abonelik     |Aboneliğinizi seçin.                         |
    |Kaynak grubu   |Kaynak grubunuzu seçin veya yeni bir tane oluşturun.  |
    |Ad             |*Azure-Spring-Cloud-VNET* girin                   |
    |Konum         |**Doğu ABD** seçin                                |

1. Ileri ' ye tıklayın **: IP adresleri >**. 
 
1. IPv4 adres alanı için 10.1.0.0/16 girin.

1. Alt ağ **Ekle**' yi seçin ve alt ağ **adı** için 10.1.0.0/24 alt ağ **adres aralığı**için *hizmet-çalışma zamanı-alt ağını* girin. Daha sonra **Ekle**'ye tıklayın.

1. **Alt ağ ekle** ' yi tekrar seçin, sonra alt ağ **adı** için *uygulamalar-alt* ağ adı ve 10.1.1.0/24 **yazın.**  **Ekle**'ye tıklayın.

1. **Gözden geçir ve oluştur**’a tıklayın. Rest 'i varsayılan olarak bırakın ve **Oluştur**' a tıklayın.

## <a name="grant-service-permission-to-the-virtual-network"></a>Sanal ağ için hizmet izni verme

Daha önce oluşturduğunuz sanal ağı *Azure-Spring-Cloud-VNET* ' i seçin.

1. **Erişim denetimi (IAM)** seçeneğini belirleyin ve ardından **Ekle > rol ataması**Ekle ' yi seçin.

    ![V-net için erişim denetimi](./media/spring-cloud-v-net-injection/access-control.png)

2. **Rol ataması Ekle** iletişim kutusunda bu bilgileri girin veya seçin:

    |Ayar  |Değer                                             |
    |---------|--------------------------------------------------|
    |Rol     |**Sahip** seçin                                  |
    |Şunu seçin:   |*Azure Spring Cloud kaynak sağlayıcısı* 'nı girin      |

    Ardından *Azure yay bulut kaynak sağlayıcısı*' nı seçin ve **Kaydet**' e tıklayın.

    ![Azure Spring Cloud kaynak sağlayıcısı 'nı v-net 'e verme](./media/spring-cloud-v-net-injection/grant-azure-spring-cloud-resource-provider-to-vnet.png)

Bunu, aşağıdaki az CLı komutunu çalıştırarak de elde edebilirsiniz

```azurecli
VIRTUAL_NETWORK_RESOURCE_ID=`az network vnet show \
    --name ${NAME_OF_VIRTUAL_NETWORK} \
    --resource-group ${RESOURCE_GROUP_OF_VIRTUAL_NETWORK} \
    --query "id" \
    --output tsv`

az role assignment create \
    --role "Owner" \
    --scope ${VIRTUAL_NETWORK_RESOURCE_ID} \
    --assignee e8de9221-a19c-4c81-b814-fd37c6caf9d2
```

## <a name="deploy-azure-spring-cloud-service-instance-in-the-virtual-network"></a>Sanal ağda Azure yay bulut hizmeti örneğini dağıtma

1. ' İ kullanarak Azure portal açın https://ms.portal.azure.com .

1. Üst arama kutusundan **Azure yay bulutu**' nı arayın ve sonuçtan **Azure yay bulutu** ' nı seçin.

1. **Azure yay bulutu** sayfasında **+ Ekle**' yi seçin.

1. Azure yay bulutu **Oluştur** sayfasında formu doldurun. 

1. Sanal ağla aynı kaynak grubunu ve bölgeyi seçin.

1. **Hizmet Ayrıntıları**altındaki **ad** için *Azure-Spring-Cloud-VNET*' i seçin.

1. **Ağ** sekmesini seçin ve aşağıdakileri seçin:

    |Ayar                                |Değer                                             |
    |---------------------------------------|--------------------------------------------------|
    |Kendi sanal ağınıza dağıtın     |**Evet** ' i seçin                                    |
    |Sanal ağ                        |*Azure-Spring-Cloud-VNET* seçin                  |
    |Hizmet çalışma zamanı alt ağı                 |*Hizmet-çalışma zamanı-alt ağ* seçin                   |
    |Spring Boot mikro hizmet uygulamaları alt ağı   |*Uygulamaları seçin-alt ağ*                              |

    ![Oluşturma Ağ sekmesi](./media/spring-cloud-v-net-injection/creation-blade-networking-tab.png)

1. **Gözden geçir ve oluştur**’a tıklayın.

1. Belirtimlerinizi doğrulayıp **Oluştur**' a tıklayın.

Dağıtımdan sonra, Azure yay bulut hizmeti örneği için ağ kaynaklarını barındırmak üzere aboneliğinizde iki ek kaynak grubu oluşturulur.  Aşağıdaki yeni kaynak gruplarını bulmak için **Home** ' a gidip üstteki menü öğelerinden **kaynak grupları** ' nı seçin.

*Azure-Spring-Cloud-Service-runtime_ {Service Instance Name} _ {Service Instance Region}* olarak adlandırılan kaynak grubu, hizmet örneğinin hizmet çalışma zamanına ait ağ kaynaklarını içerir.

  ![Hizmet çalışma zamanı](./media/spring-cloud-v-net-injection/service-runtime-resource-group.png)

*Azure-Spring-Cloud-Service-runtime_ {Service Instance Name} _ {Service Instance Region}* olarak adlandırılan kaynak grubu, hizmet örneğinin Spring Boot mikro hizmet uygulamalarınızın ağ kaynaklarını içerir.

  ![Uygulamalar kaynak grubu](./media/spring-cloud-v-net-injection/apps-resource-group.png)

Bu ağ kaynakları, yukarıda oluşturulan sanal ağınıza bağlanır.

  ![Bağlı cihazla V-net](./media/spring-cloud-v-net-injection/vnet-with-connected-device.png)

   > [!Important]
   > Kaynak grupları Azure Spring Cloud Service tarafından tam olarak yönetilir. Lütfen içindeki bir kaynağı el ile silmeyin veya değiştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

[VNet 'iniz üzerinde Azure Spring Cloud 'a uygulama dağıtma](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/02-deploy-application-to-azure-spring-cloud-in-your-vnet.md)

## <a name="see-also"></a>Ayrıca bkz.

- [VNET 'te Azure Spring Cloud sorunlarını giderme](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/05-troubleshooting-azure-spring-cloud-in-vnet.md)
- [VNET 'te Azure Spring Cloud çalıştırmaya yönelik müşteri sorumlulukları](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/06-customer-responsibilities-for-running-azure-spring-cloud-in-vnet.md)