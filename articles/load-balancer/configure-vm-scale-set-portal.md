---
title: Sanal makine ölçek kümesini mevcut bir Azure Load Balancer Yapılandırma Azure portal
description: Bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer nasıl yapılandıracağınızı öğrenin.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: 8e55b2a87813da802ec1b00c610b33da4461c008
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84809451"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-portal"></a>Azure portal kullanarak bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer yapılandırma

Bu makalede, bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer nasıl yapılandıracağınızı öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği.
- Abonelikte sanal makine ölçek kümesinin dağıtılacağı mevcut bir standart SKU yük dengeleyici.
- Sanal makine ölçek kümesi için bir Azure sanal ağı.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.



## <a name="deploy-virtual-machine-scale-set-with-existing-load-balancer"></a>Sanal makine ölçek kümesini mevcut yük dengeleyiciye dağıt

Bu bölümde, mevcut bir Azure yük dengeleyicisiyle Azure portal bir sanal makine ölçek kümesi oluşturacaksınız.

> [!NOTE]
> Aşağıdaki adımlarda, **Myvnet** adlı bir sanal ağ ve **Myloadbalancer** adlı bir Azure yük dengeleyici daha önce dağıtıldı.

1. Ekranın sol üst kısmında, **kaynak**  >  **işlem**  >  **sanal makine ölçek kümesi** oluştur veya Market aramasında **sanal makine ölçek kümesini** ara ' ya tıklayın.

2. **Oluştur**'u seçin.

3. **Sanal makine ölçek kümesi oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | Ayar                        | Değer                                                                                                 |
    |--------------------------------|-------------------------------------------------------------------------------------------------------|
    | **Proje ayrıntıları**            |                                                                                                       |
    | Abonelik                   | Azure aboneliğinizi seçin                                                                        |
    | Kaynak Grubu                 | Yeni Oluştur ' u seçin, **Myresourcegroup**yazın ve ardından Tamam ' ı seçin veya var olan bir kaynak grubunu seçin. |
    | **Ölçek kümesi ayrıntıları**          |                                                                                                       |
    | Sanal makine ölçek kümesi adı | **Myvmss** girin                                                                                      |
    | Bölge                         | **Doğu ABD 2** seçin                                                                                    |
    | Kullanılabilirlik alanı              | **Hiçbirini** seçme                                                                                       |
    | **Örnek ayrıntıları**           |                                                                                                       |
    | Görüntü                          | **Ubuntu Server 18,04 LTS** 'yi seçin                                                                    |
    | Azure spot örneği            | **Hayır** seçin                                                                                         |
    | Boyut                           | Varsayılan konumda bırak                                                                                      |
    | **Yönetici hesabı**      |                                                                                                       |
    | Kimlik doğrulaması türü            | **Parola** seçin                                                                                   |
    | Kullanıcı adı                       | Yönetici Kullanıcı adınızı girin        |
    | Parola                       | Yönetici parolanızı girin    |
    | Parolayı onayla               | Yönetici parolanızı yeniden girin |


    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-01.png" alt-text="Sanal makine ölçek kümesi oluşturun." border="true":::

4. **Ağ** sekmesini seçin.

5. **Ağ** sekmesinden bu bilgileri girin veya seçin:

     Ayar                           | Değer                                                    |
    |-----------------------------------|----------------------------------------------------------|
    | **Sanal Ağ Yapılandırması** |                                                          |
    | Sanal ağ                   | **Myvnet** veya var olan sanal ağınızı seçin.      |
    | **Yük Dengeleme**                |                                                          |
    | Yük dengeleyici kullanma               | **Evet** ' i seçin                                           |
    | **Yük Dengeleme ayarları**       |                                                          |
    | Yük dengeleme seçenekleri            | **Azure Yük dengeleyiciyi** seçin                           |
    | Yük dengeleyici seçin            | **Myloadbalancer** veya mevcut yük dengeleyicinizi seçin |
    | Bir arka uç havuzu seçin             | **Mybackendpool** veya mevcut arka uç havuzunuzu seçin.  |

    :::image type="content" source="./media/vm-scale-sets/create-vm-scale-set-02.png" alt-text="Sanal makine ölçek kümesi oluşturun." border="true":::

6. **Yönetim** sekmesini seçin.

7. **Yönetim** sekmesinde, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.

8. Mavi **gözden geçir + oluştur** düğmesini seçin.

9. Ayarları gözden geçirin ve **Oluştur** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, mevcut bir Azure Load Balancer bir sanal makine ölçek kümesi dağıttınız.  Sanal Makine Ölçek Kümeleri ve yük dengeleyici hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Load Balancer nedir?](load-balancer-overview.md)
- [Sanal makine ölçek kümeleri nedir?](../virtual-machine-scale-sets/overview.md)
