---
title: Sanal ağınızda Bir Azure Veri Gezgini kümesi & DB oluşturma
description: Bu makalede, sanal ağınızda bir Azure Veri Gezgini kümesioluşturmayı öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: basaba
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/24/2020
ms.openlocfilehash: 4dff471fa0f2194756409e01512ed223a1d46024
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241445"
---
# <a name="create-an-azure-data-explorer-cluster-in-your-virtual-network"></a>Sanal aağınızda Azure Veri Gezgini kümesi oluşturma

Azure Veri Gezgini, sanal ağınızda (VNet) bir kümeyi bir alt ağa dağıtmayı destekler. Bu özellik, kümeye Azure sanal ağınızdan veya şirket içi erişiminizden özel olarak erişmenizi, sanal ağınızdaki Olay Hub'ı ve Depolama gibi kaynağa erişmenizi ve gelen ve giden trafiği kısıtlamanızı sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-network-security-group-nsg"></a>Ağ Güvenlik Grubu (NSG) oluşturma

[Ağ Güvenlik Grupları (NSG),](/azure/virtual-network/security-overview) Bir VNet içinde ağ erişimini denetleme olanağı sağlar. Azure Veri Gezgini'ne iki uç nokta HTTPs (443) ve TDS (1433) kullanılarak erişilebilir. Aşağıdaki NSG kuralları, kümenizin yönetimi, izlenmesi ve düzgün çalışması için bu uç noktalara erişime izin verecek şekilde yapılandırılmalıdır.

Ağ güvenlik grubunu oluşturmak için:

1. **Portalın** sol üst köşesinde + kaynak oluştur düğmesini seçin.
1. Ağ *Güvenlik Grubu'ni*arayın.
1. **Ağ Güvenliği Grubu**altında, ekranın alt kısmında, **Oluştur'u**seçin.
1. Ağ **güvenlik grubu oluştur** penceresinde aşağıdaki bilgileri doldurun.

   ![NSG formu oluşturma](media/vnet-create-cluster-portal/network-security-group.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Kümeniz için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | Kaynak grubunuz | Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Adı | AzureDataExplorerNsg | Kaynak grubunda Ağ Güvenlik Grubunuzu (NSG) tanımlayan bir ad seçin.  |
    | Bölge | *Batı ABD* | İhtiyaçlarınızı en iyi karşılayan bölgeyi seçin.
    | | | |

1. Küme ayrıntılarınızı gözden geçirmek için **Gözden Geçir + oluştur'u** ve kümeyi sağlamak için **Oluştur'u** seçin.

1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

    ![Kaynağa git](media/vnet-create-cluster-portal/notification-nsg.png)

1. Gelen **güvenlik kuralları** sekmesinde **+Ekle'yi**seçin.
1. Gelen **güvenlik kuralı ekle** penceresinde aşağıdaki bilgileri doldurun.

    ![NSG gelen kural formunu oluşturma](media/vnet-create-cluster-portal/nsg-inbound-rule.png)

    **Ayar** | **Önerilen değer** 
    |---|---|
    | Kaynak | ServiceTag
    | Kaynak servis etiketi | AzureDataExplorerYönetimi
    | Kaynak bağlantı noktası aralıkları | *
    | Hedef | VirtualNetwork
    | Hedef bağlantı noktası aralıkları | *
    | Protokol | TCP
    | Eylem | İzin Ver
    | Öncelik | 100
    | Adı | AzureDataExplorerManagement'a izin verme
    | | |
    
1. [VNet dağıtımı](/azure/data-explorer/vnet-deloyment#dependencies-for-vnet-deployment)na yönelik bağımlılıklara göre tüm gelen ve giden bağımlılıklar için önceki iki adımı yineleyin. Alternatif olarak, giden kurallar, 443 ve 80 bağlantı noktaları için *Internet'e* izin vermek için tek bir kuralla değiştirilebilir.
    
    Gelen ve giden bağımlılıklar için NSG kuralları aşağıdaki gibi görünmelidir:

    ![NSG kuralları](media/vnet-create-cluster-portal/nsg-rules.png)

## <a name="create-public-ip-addresses"></a>Ortak IP adresleri oluşturma

Sorgu (Motor) genel IP adresleri oluşturmak için:

1. **Portalın** sol üst köşesinde + kaynak oluştur düğmesini seçin.
1. Ağ *Güvenlik Grubu'ni*arayın.
1. **Genel IP adresi**altında, ekranın alt kısmında, **Oluştur'u**seçin.
1. Ortak **IP adresi oluştur** bölmesinde, aşağıdaki bilgileri tamamlayın.
   
   ![Ortak IP formu oluşturma](media/vnet-create-cluster-portal/public-ip-blade.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | IP Sürümü | IPv4 | IP sürümünü seçin. Biz sadece IPv4'u destekliyoruz.|
    | Sku | Standart | Sorgu (Engine) URI uç noktası için **Standart'a** ihtiyacımız vardır. |
    | Adı | motor-pip | Kaynak grubundaki genel IP adresinizi tanımlayan bir ad seçin.
    | Abonelik | Aboneliğiniz | Herkese açık IP'niz için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | Kaynak grubunuz | Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Konum | *Batı ABD* | İhtiyaçlarınızı en iyi karşılayan bölgeyi seçin.
    | | | |

1. Ortak IP adresini oluşturmak için **Oluştur'u** seçin.

1. Yutma (Veri Yönetimi) genel IP adresini oluşturmak için aynı talimatları izleyin ve 
    * **Sku**: Temel
    * **IP adresi ataması**: Statik

## <a name="create-virtual-network-and-subnet"></a>Sanal Ağ ve alt ağ oluşturma

Sanal ağ ve alt ağ oluşturmak için:

1. **Portalın** sol üst köşesinde + kaynak oluştur düğmesini seçin.
1. Sanal *Ağ*ara.
1. **Sanal Ağ**altında, ekranın alt kısmında, **Oluştur'u**seçin.
1. Sanal **ağ oluştur** penceresinde, aşağıdaki bilgileri tamamlayın.

   ![Sanal ağ formu oluşturma](media/vnet-create-cluster-portal/vnet-blade.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Kümeniz için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | Kaynak grubunuz | Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Adı | AzureDataExplorerVnet | Kaynak grubundaki sanal ağınızı tanımlayan bir ad seçin.
    | Bölge | *Batı ABD* | İhtiyaçlarınızı en iyi karşılayan bölgeyi seçin.
    | | | |

    > [!NOTE]
    > Üretim iş yükleri için, [VNet'inizdeki plan alt net boyutuna](/azure/data-explorer/vnet-deloyment#plan-subnet-size-in-your-vnet) göre alt net boyutunuzu planlayın

1. Küme ayrıntılarınızı gözden geçirmek için **Gözden Geçir + oluştur'u** ve kümeyi sağlamak için **Oluştur'u** seçin.

1. Dağıtım tamamlandığında **kaynağa git'i**seçin.
1. **Subnets** blade'e gidin ve **varsayılan** alt ağı seçin.
    
    ![Subnets bıçak](media/vnet-create-cluster-portal/subnets.png)

1. **Varsayılan** alt ağ pencerenizde:
    1. Açılan menüden **Ağ Güvenlik Grubunuzu** seçin. 
    1. Bu durumda **AzureDataExplorerNsg**ağ güvenlik grup adınızı seçin. 
    1. **Kaydet**

    ![Alt ağı yapılandırma](media/vnet-create-cluster-portal/subnet-nsg.png)

## <a name="create-a-cluster"></a>Küme oluşturma

Bir Azure kaynak grubunda tanımlı bir bilgi işlem ve depolama kaynakları kümesi yle bir Azure Veri Gezgini [kümesi oluşturun.](create-cluster-database-portal.md#create-a-cluster)

1. Küme oluşturmayı tamamlamadan önce, **Azure Veri Gezgini küme oluşturma penceresinde,** önceki sekmelerde oluşturulan kaynakları kullanarak sanal ağ ayrıntıları sağlamak için **Ağ** sekmesini seçin:

   ![Küme vnet formu oluşturma](media/vnet-create-cluster-portal/create-cluster-form-vnet.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Ağ kaynakları için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Sanal Ağ | AzureDataExplorerVnet | Önceki adımlarda oluşturulan sanal ağı seçin.
    | Alt ağ | default | Önceki adımlarda oluşturulan alt ağı seçin.
    | Genel IP Sorgula | motor-pip | Önceki adımlarda oluşturulan ortak IP sorgusunu seçin.
    | Veri alma Genel IP | dm-pip | Önceki adımlarda oluşturulan genel IP'yi seçin.
    | | | |

1. Kümenizi oluşturmak için **Gözden Geçir + oluştur'u** seçin.
1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

Azure Veri Gezgini kümenizi sanal ağınıza dağıtmak için, VNet Azure Kaynak Yöneticisi [şablonunuzda Azure Veri Gezgini kümesini dağıt'ı](https://azure.microsoft.com/resources/templates/101-kusto-vnet/) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Veri Gezgini'ni Sanal Ağınıza Dağıtma](vnet-create-cluster-portal.md)