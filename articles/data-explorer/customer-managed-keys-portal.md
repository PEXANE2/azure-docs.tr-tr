---
title: Azure portalını kullanarak müşteri tarafından yönetilen anahtarları yapılandırma
description: Bu makalede, Azure Veri Gezgini'nde verilerinizde müşteri tarafından yönetilen anahtar şifrelemesi nasıl yapılandırılabildiğini açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: itsagui
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: 180196f2c368207b76811700fd845406098600df
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529430"
---
# <a name="configure-customer-managed-keys-using-the-azure-portal"></a>Azure portalını kullanarak müşteri tarafından yönetilen anahtarları yapılandırma

> [!div class="op_single_selector"]
> * [Portal](customer-managed-keys-portal.md)
> * [C #](customer-managed-keys-csharp.md)
> * [Azure Resource Manager şablonu](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

## <a name="enable-encryption-with-customer-managed-keys-in-the-azure-portal"></a>Azure portalında müşteri tarafından yönetilen anahtarlarla şifreleme yi etkinleştirme

Bu makalede, Azure portalını kullanarak müşteri tarafından yönetilen anahtar şifrelemeyi nasıl etkinleştirdiğinizgösterilmektedir. Varsayılan olarak, Azure Veri Gezgini şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Azure Veri Gezgini kümenizi müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve kümeyle ilişkilendirecek anahtarı belirtin.

1. Azure [portalında](https://portal.azure.com/)Azure [Veri Gezgini küme](create-cluster-database-portal.md#create-a-cluster) kaynağınıza gidin. 
1. Portalın sol bölmesinde **Ayarlar** > **Şifreleme'yi** seçin.
1. **Şifreleme** bölmesinde, Müşteri tarafından **yönetilen anahtar** ayarı için **Açık'ı** seçin.
1. **Anahtar Seç'i**tıklatın.

    ![Müşteri tarafından yönetilen anahtarları yapılandırma](media/customer-managed-keys-portal/cmk-encryption-setting.png)

1. Azure **Key Vault penceresinden Seç tuşu'nda** açılır listeden varolan bir **Anahtar kasası** seçin. Yeni bir Anahtar [Kasası oluşturmak](/azure/key-vault/quick-create-portal#create-a-vault)için **yeni oluştur'u** seçerseniz, **Anahtar Kasası Oluştur** ekranına yönlendirilmiş olursunuz.

1. **Anahtar'ı**seçin.
1. **Sürüm'u**seçin.
1. **Seç'i**tıklatın.

    ![Azure Anahtar Kasası'ndan anahtarı seçin](media/customer-managed-keys-portal/cmk-key-vault.png)

1. Anahtarınızı içeren **Şifreleme** bölmesinde **Kaydet'i**seçin. CMK oluşturma başarılı olduğunda, Bildirimler'de bir başarı **iletisi**görürsünüz.

    ![Müşteri tarafından yönetilen anahtarı kaydet](media/customer-managed-keys-portal/cmk-encryption-setting.png)

Azure Veri Gezgini kümeniz için müşteri tarafından yönetilen anahtarları etkinleştirerek, küme için atanmış bir sistem kimliği oluşturuyor olacaksınız. Ayrıca, seçili Key Vault'taki Azure Veri Gezgini kümenize gerekli get, wrapKey ve unwarpKey izinlerini sağlayacak ve Key Vault özelliklerini elde edeceksiniz. 

> [!NOTE]
> Oluşturulduktan sonra müşteri tarafından yönetilen anahtarı kaldırmak için **Kapalı'yı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da Güvenli Azure Veri Gezgini kümeleri](security.md)
* Şifrelemeyi hazır layarak [Azure Veri Gezgini - Azure portalında kümenizi güvenli](manage-cluster-security.md) hale verin.
* [Azure Kaynak Yöneticisi şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın](customer-managed-keys-resource-manager.md)
* [C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #](customer-managed-keys-csharp.md)



