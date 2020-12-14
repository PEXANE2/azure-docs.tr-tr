---
title: Öğretici-sanal ağda Azure yay bulutu dağıtma
description: Azure yay bulutu 'nı sanal ağda (v-net ekleme) dağıtın.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 07/21/2020
ms.custom: devx-track-java
ms.openlocfilehash: 1e16c984e48c11961dba0c977d3bdbddbd6bdf36
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97400328"
---
# <a name="tutorial-deploy-azure-spring-cloud-in-azure-virtual-network-vnet-injection"></a>Öğretici: Azure sanal ağ 'da Azure yay bulutu dağıtma (VNet ekleme)

**Bu makale şu şekilde geçerlidir:** ✔️ Java ✔️ C #

Bu öğreticide, sanal ağınızda bir Azure Spring Cloud Service örneğinin nasıl dağıtılacağı açıklanmaktadır. Buna bazen VNet ekleme denir.  

Dağıtım şunları sunar:

* Şirket ağınızdaki Internet 'ten gelen Azure Mağazası bulut uygulamalarının ve hizmet çalışma zamanının yalıtımı
* Azure Spring, şirket içi veri merkezlerinde ve/veya diğer sanal ağlardaki Azure hizmetlerindeki sistemlerle bulut etkileşimi
* Azure Spring Cloud için gelen ve giden ağ iletişimlerini denetlemek için müşterilerin güçleme

## <a name="prerequisites"></a>Önkoşullar
Azure portal veya şu az CLı komutunu çalıştırarak, [kaynak sağlayıcısını kaydetme](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) yönergelerine göre *Microsoft. Appplatform* ve *Microsoft. Containerservice* Azure Spring Cloud kaynak sağlayıcısı 'nı kaydetmeniz gerekir:

```azurecli
az provider register --namespace Microsoft.AppPlatform
az provider register --namespace Microsoft.ContainerService
```
## <a name="virtual-network-requirements"></a>Sanal ağ gereksinimleri
Azure yay bulut hizmeti örneğinizi dağıttığınız sanal ağın aşağıdaki gereksinimleri karşılaması gerekir:

* **Konum**: sanal ağın Azure yay bulut hizmeti örneğiyle aynı konumda bulunması gerekir.
* **Abonelik**: sanal ağın Azure yay bulut hizmeti örneğiyle aynı abonelikte olması gerekir.
* **Alt ağlar**: sanal ağ, bir Azure yay bulut hizmeti örneğine adanmış iki alt ağ içermelidir: 
    * Biri hizmet çalışma zamanı için
    * Bir tane, Spring Boot mikro hizmet uygulamalarınız için. 
    * Bu alt ağlar ve bir Azure yay bulut hizmeti örneği arasında bire bir ilişki vardır. Dağıttığınız her bir hizmet örneği için yeni bir alt ağ kullanmanız gerekir ve her alt ağ yalnızca tek bir hizmet örneği içerebilir.
* **Adres alanı**: CIDR, hem Service Runtime alt ağı hem de Spring Boot mikro hizmet uygulamaları alt ağı için **/28** ' i engeller.
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
    |Name             |*Azure-Spring-Cloud-VNET* girin                   |
    |Konum         |**Doğu ABD** seçin                                |

1. Ileri ' ye tıklayın **: IP adresleri >**. 
 
1. IPv4 adres alanı için 10.1.0.0/16 girin.

1. Alt ağ **Ekle**' yi seçin, sonra alt ağ adı için *hizmet-çalışma zamanı-alt ağ* **adı** ve 10.1.0.0/28 **yazın.** Daha sonra **Ekle**'ye tıklayın.

1. **Alt ağ ekle** ' yi tekrar seçin, sonra **alt ağ adı** ve **alt ağ adres aralığı** girin, örneğin, *uygulamalar-alt ağ* ve 10.1.1.0/28.  **Ekle**'ye tıklayın.

1. **Gözden geçir ve oluştur**’a tıklayın. Rest 'i varsayılan olarak bırakın ve **Oluştur**' a tıklayın.

## <a name="grant-service-permission-to-the-virtual-network"></a>Sanal ağ için hizmet izni verme

Daha önce oluşturduğunuz sanal ağı *Azure-Spring-Cloud-VNET* ' i seçin.

1. **Erişim denetimi (IAM)** seçeneğini belirleyin ve ardından **Ekle > rol ataması** Ekle ' yi seçin.

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

1. ' İ kullanarak Azure portal açın https://portal.azure.com .

1. Üst arama kutusundan **Azure yay bulutu**' nı arayın ve sonuçtan **Azure yay bulutu** ' nı seçin.

1. **Azure yay bulutu** sayfasında **+ Ekle**' yi seçin.

1. Azure yay bulutu **Oluştur** sayfasında formu doldurun. 

1. Sanal ağla aynı kaynak grubunu ve bölgeyi seçin.

1. **Hizmet Ayrıntıları** altındaki **ad** için *Azure-Spring-Cloud-VNET*' i seçin.

1. **Ağ** sekmesini seçin ve aşağıdakileri seçin:

    |Ayar                                |Değer                                             |
    |---------------------------------------|--------------------------------------------------|
    |Kendi sanal ağınıza dağıtın     |**Evet**’i seçin                                    |
    |Sanal ağ                        |*Azure-Spring-Cloud-VNET* seçin                  |
    |Hizmet çalışma zamanı alt ağı                 |*Hizmet-çalışma zamanı-alt ağ* seçin                   |
    |Spring Boot mikro hizmet uygulamaları alt ağı   |*Uygulamaları seçin-alt ağ*                              |

    ![Oluşturma Ağ sekmesi](./media/spring-cloud-v-net-injection/creation-blade-networking-tab.png)

1. **Gözden geçir ve oluştur**’a tıklayın.

1. Belirtimlerinizi doğrulayıp **Oluştur**' a tıklayın.

    ![Belirtimleri doğrula](./media/spring-cloud-v-net-injection/verify-specifications.png)

Dağıtımdan sonra, Azure yay bulut hizmeti örneği için ağ kaynaklarını barındırmak üzere aboneliğinizde iki ek kaynak grubu oluşturulur.  Aşağıdaki yeni kaynak gruplarını bulmak için **Home** ' a gidip üstteki menü öğelerinden **kaynak grupları** ' nı seçin.

*AP-svc-RT_ {Service Instance Name} _ {Service Instance Region}* olarak adlandırılan kaynak grubu, hizmet örneğinin hizmet çalışma zamanına ait ağ kaynaklarını içerir.

  ![Hizmet çalışma zamanı](./media/spring-cloud-v-net-injection/service-runtime-resource-group.png)

*AP-App_ {Service Instance Name} _ {Service Instance Region}* olarak adlandırılan kaynak grubu, hizmet örneğinin Spring Boot mikro hizmet uygulamalarınızın ağ kaynaklarını içerir.

  ![Uygulamalar kaynak grubu](./media/spring-cloud-v-net-injection/apps-resource-group.png)

Bu ağ kaynakları, yukarıda oluşturulan sanal ağınıza bağlanır.

  ![Bağlı cihazla V-net](./media/spring-cloud-v-net-injection/vnet-with-connected-device.png)

   > [!Important]
   > Kaynak grupları Azure Spring Cloud Service tarafından tam olarak yönetilir. Lütfen içindeki bir kaynağı el ile silmeyin veya değiştirmeyin.

## <a name="limitations"></a>Sınırlamalar

Küçük alt ağ aralığı IP adreslerini kaydeder, ancak Azure yay bulutunun tutabilmesine izin verilen en fazla uygulama örneği sayısına yönelik sınırlamalar getirir. 

| Uygulama alt ağı CıDR | Toplam IP sayısı | Kullanılabilir IP 'Ler | En fazla uygulama örnekleri                                        |
| --------------- | --------- | ------------- | ------------------------------------------------------------ |
| /28             | 16        | 8             | <p> 1 çekirdekli uygulama: 96 <br/> 2 çekirdek içeren uygulama: 48<br/>  3 çekirdekle uygulama: 32 <br/> 4 çekirdek içeren uygulama: 24 </p> |
| /27             | 32        | 24            | <p> 1 çekirdekli uygulama: 228<br/> 2 çekirdek içeren uygulama: 144<br/>  3 çekirdekle uygulama: 96 <br/>  4 çekirdek içeren uygulama: 72</p> |
| /26             | 64        | 56            | <p> 1 çekirdekli uygulama: 500<br/> 2 çekirdek içeren uygulama: 336<br/>  3 çekirdekle uygulama: 224<br/>  4 çekirdek içeren uygulama: 168</p> |
| /25             | 128       | 120           | <p> 1 çekirdekli uygulama: 500<br> 2 çekirdek içeren uygulama: 500<br>  3 çekirdekle uygulama: 480<br>  4 çekirdek içeren uygulama: 360</p> |
| /24             | 256       | 248           | <p> 1 çekirdekli uygulama: 500<br/> 2 çekirdek içeren uygulama: 500<br/>  3 çekirdekle uygulama: 500<br/>  4 çekirdek içeren uygulama: 500</p> |

Alt ağlar için, 5 IP adresi Azure tarafından ayrılmıştır ve Azure Spring Cloud için en az 4 adres gereklidir. En az 9 IP adresi gereklidir, bu nedenle/29 ve/30 işlemsel değildir.

Hizmet çalışma zamanı alt ağı için, en az boyut bir/28 ve bu, uygulama örneği sayısı üzerinde hiçbir pul içermez.
## <a name="next-steps"></a>Sonraki adımlar

[VNet 'iniz üzerinde Azure Spring Cloud 'a uygulama dağıtma](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/02-deploy-application-to-azure-spring-cloud-in-your-vnet.md)

## <a name="see-also"></a>Ayrıca bkz.

- [VNET 'te Azure Spring Cloud sorunlarını giderme](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/05-troubleshooting-azure-spring-cloud-in-vnet.md)
- [VNET 'te Azure Spring Cloud çalıştırmaya yönelik müşteri sorumlulukları](https://github.com/microsoft/vnet-in-azure-spring-cloud/blob/master/06-customer-responsibilities-for-running-azure-spring-cloud-in-vnet.md)
