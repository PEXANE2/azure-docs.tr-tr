---
title: Mevcut bir Azure Yük Bakiyesi - Azure portalı ile ayarlanan sanal makine ölçeğini yapılandırın
description: Varolan bir Azure Yük Dengeleyicisi ile sanal makine ölçeği kümesini nasıl yapılandıracağımız öğrenin.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: article
ms.date: 03/25/2020
ms.openlocfilehash: b2a83b226a4b2ddbbd554783de6a2b5c85c92f0a
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349718"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-portal"></a>Azure portalını kullanarak mevcut bir Azure Yük Dengeleyicisiyle sanal makine ölçeği kümesini yapılandırın

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal makine ölçeğini nasıl yapılandıracağınız öğrenilir. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği.
- Sanal makine ölçeği kümesinin dağıtılanacağı abonelikteki mevcut standart sku yük dengeleyicisi.
- Sanal makine ölçeği kümesi için bir Azure Sanal Ağı.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.



## <a name="deploy-virtual-machine-scale-set-with-existing-load-balancer"></a>Mevcut yük dengeleyicisi ile sanal makine ölçeği kümesini dağıtma

Bu bölümde, Azure portalında mevcut bir Azure yük dengeleyicisi ile ayarlanmış sanal bir makine ölçeği oluşturursunuz.

> [!NOTE]
> Aşağıdaki adımlar, **myVNet** adında bir sanal ağ ve **myLoadBalancer** adlı bir Azure yük dengeleyicisinin daha önce dağıtıldığını varsayar.

1. Ekranın sol üst tarafında, **bir kaynak** > **Oluştur Sanal** > **makine ölçeği kümesini** tıklatın veya pazar aramasında **ayarlanan Sanal makine ölçeğini** arayın.

2. **Oluştur'u**seçin.

3. **Sanal makine ölçeği kümesi oluştur'da,** **TemelLer** sekmesinde bu bilgileri girin veya seçin:

    | Ayar                        | Değer                                                                                                 |
    |--------------------------------|-------------------------------------------------------------------------------------------------------|
    | **Proje ayrıntıları**            |                                                                                                       |
    | Abonelik                   | Azure aboneliğinizi seçin                                                                        |
    | Kaynak Grubu                 | Yeni Oluştur'u seçin, **myResourceGroup'a**girin, ardından Tamam'ı seçin veya varolan bir kaynak grubu seçin. |
    | **Ölçek kümesi ayrıntıları**          |                                                                                                       |
    | Sanal makine ölçek kümesi adı | **myVMSS** girin                                                                                      |
    | Bölge                         | **Doğu ABD 2'yi** seçin                                                                                    |
    | Kullanılabilirlik alanı              | **Hiçbiri'ni** Seçin                                                                                       |
    | **Örnek ayrıntıları**           |                                                                                                       |
    | Görüntü                          | **Ubuntu Server 18.04 LTS'yi** seçin                                                                    |
    | Azure Spot örneği            | **Hayır'ı** seçin                                                                                         |
    | Boyut                           | Varsayılan olarak bırakın                                                                                      |
    | **Yönetici hesabı**      |                                                                                                       |
    | Kimlik doğrulaması türü            | **Parola** Seçin                                                                                   |
    | Kullanıcı adı                       | Yönetici kullanıcı adınızı girin        |
    | Parola                       | Yönetici şifrenizi girin    |
    | Parolayı onayla               | Yönetici şifrenizi yeniden girin |


    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-01.png" alt-text="Sanal makine ölçek kümesi oluşturun." border="true":::

4. **Ağ** sekmesini seçin.

5. **Ağ** sekmesine bu bilgileri girin veya seçin:

     Ayar                           | Değer                                                    |
    |-----------------------------------|----------------------------------------------------------|
    | **Sanal Ağ Yapılandırması** |                                                          |
    | Sanal ağ                   | **myVNet'i** veya mevcut sanal ağınızı seçin.      |
    | **Yük dengeleme**                |                                                          |
    | Yük dengeleyicisi kullanma               | **Evet'i** Seçin                                           |
    | **Yük dengeleme ayarları**       |                                                          |
    | Yük dengeleme seçenekleri            | **Azure yük bakiyesi** seçin                           |
    | Yük dengeleyicisi seçin            | **myLoadBalancer** veya mevcut yük bakiyecinizi seçin |
    | Arka uç havuzu seçin             | **myBackendPool'u** veya mevcut arka uç havuzunuzu seçin.  |

    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-02.png" alt-text="Sanal makine ölçek kümesi oluşturun." border="true":::

6. **Yönetim** sekmesini seçin.

7. **Yönetim** sekmesinde, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın.

8. Mavi **Gözden Geçir + oluştur** düğmesini seçin.

9. Ayarları gözden geçirin ve **Oluştur** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal bir makine ölçeği dağıttınız.  Sanal makine ölçek kümeleri ve yük dengeleyicisi hakkında daha fazla bilgi edinmek için bkz:

- [Azure Load Balancer nedir?](load-balancer-overview.md)
- [Sanal makine ölçek kümeleri nedir?](../virtual-machine-scale-sets/overview.md)
