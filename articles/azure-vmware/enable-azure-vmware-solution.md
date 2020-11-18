---
title: Azure VMware Çözüm kaynağınızı etkinleştirme
description: Azure VMware Çözüm kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla düğüm isteyebilirsiniz.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: c9c31d8ffbf62a511f4f2ab19f995b6bc0ee0ca4
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695155"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Azure VMware Çözüm kaynağını etkinleştirme
[Azure VMware Çözüm](introduction.md) kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla düğüm isteyebilirsiniz.

## <a name="eligibility-criteria"></a>Uygunluk ölçütleri

Azure aboneliğinde bir Azure hesabınızın olması gerekir. Azure aboneliği aşağıdaki ölçütlerden biriyle uyumlu olmalıdır:

* Microsoft ile [Azure Kurumsal Anlaşma (EA)](../cost-management-billing/manage/ea-portal-agreements.md) kapsamındaki bir abonelik.
* Bir Azure planı kapsamındaki bulut çözümü sağlayıcısı (CSP) tarafından yönetilen bir abonelik.


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>EA müşterileri için Azure VMware çözümünü etkinleştirme
Azure VMware Çözüm kaynağını oluşturmadan önce, düğümlerinizin ayrılması için bir destek bileti göndermeniz gerekir. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve düğümlerinizi ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla düğüm ayırdıysanız, aynı işleme gidebilirsiniz.


1. Azure portal, **Yardım + Destek** altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
   - **Sorun türü:** Teknik
   - **Abonelik:** Aboneliğinizi seçin
   - **Hizmet:** Tüm hizmetler Azure VMware çözümüne >
   - **Kaynak:** Genel soru 
   - **Özet:** Kapasiteye ihtiyaç duyuyor
   - **Sorun türü:** Kapasite yönetimi sorunları
   - **Sorun alt türü:** Ek konak kotası/kapasitesi için müşteri Isteği

1. Destek bileti **açıklamasında** , **Ayrıntılar** sekmesinde şunları belirtin:

   - POC veya üretim 
   - Bölge Adı
   - Düğüm sayısı
   - Diğer ayrıntılar

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 düğümlerini çalıştırmak için en az üç düğüm önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek düğümler istiyorsanız, düğümlerin ayrılması için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Düğümlerinizi sağlayabilmeniz için önce **Microsoft. AVS** kaynak sağlayıcısını Azure Portal kaydettiğinizden emin olun.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>CSP müşterileri için Azure VMware çözümünü etkinleştirme 

CSP 'Ler, müşterileri için Azure VMware çözümünü etkinleştirmek üzere [Microsoft Iş ortağı merkezini](https://partner.microsoft.com) kullanmalıdır. 

   >[!IMPORTANT] 
   >Azure VMware çözüm hizmeti çok kiracılı bir ortam sağlamıyor ve bu nedenle barındırma iş ortakları henüz desteklenmiyor. 

1. **Iş Ortağı Merkezi**'Nde, **müşteriler** alanına erişmek için **CSP** 'yi seçin.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Microsoft Iş ortağı merkezi müşterileri alanı" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. Müşteriyi seçin ve ardından **Ürün Ekle**' yi seçin.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft İş Ortağı Merkezi" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. **Azure planı** ' nı seçin ve ardından **Sepete Ekle**' yi seçin. 

1. Müşterinizin Azure planı aboneliğini genel olarak ayarlamayı gözden geçirin ve son inceleyin. Daha fazla bilgi için bkz. [Microsoft Iş Ortağı Merkezi belgeleri](https://docs.microsoft.com/partner-center/azure-plan-manage).

Azure planı yapılandırıldıktan ve gerekli RBAC izinleri bir CSP olarak yer aldıktan sonra, bir Azure plan aboneliğine yönelik kotayı etkinleştirmek için Microsoft 'u devreye alırsınız. **Şirket adına yönetici** (Aobo) yordamını kullanarak Iş ortağı merkezinden Azure Portal erişin.

1. [Iş Ortağı Merkezi](https://partner.microsoft.com)' nde oturum açın.

1. **Müşteriler** alanına erişmek için **CSP** 'yi seçin.

1. Müşteri ayrıntıları ' nı genişletin ve **Microsoft Azure yönetim portalı**' yi seçin.

1. Azure portal, **Yardım + Destek** altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
   - **Sorun türü:** Teknik
   - **Abonelik:** Aboneliğinizi seçin
   - **Hizmet:** Tüm hizmetler Azure VMware çözümüne >
   - **Kaynak:** Genel soru 
   - **Özet:** Kapasiteye ihtiyaç duyuyor
   - **Sorun türü:** Kapasite yönetimi sorunları
   - **Sorun alt türü:** Ek konak kotası/kapasitesi için müşteri Isteği

1. Destek bileti **açıklamasında** , **Ayrıntılar** sekmesinde şunları belirtin:

   - POC veya üretim 
   - Bölge Adı
   - Düğüm sayısı
   - Diğer ayrıntılar
   - Birden çok müşteriyi barındırmak için tasarlanmıştır mı?

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 düğümlerini çalıştırmak için en az üç düğüm önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek düğümler istiyorsanız, düğümlerin ayrılması için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Azure planına eklendikten ve kota etkinleştirildikten sonra, müşteri veya iş ortağı Yöneticisi Azure portal aracılığıyla bir Azure VMware çözümü özel bulutu dağıtabilir. Düğümlerinizi sağlayabilmeniz için önce **Microsoft. AVS** kaynak sağlayıcısını Azure Portal kaydettiğinizden emin olun.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure VMware Çözüm kaynağınızı etkinleştirdikten ve uygun ağa sahip olduğunuzda [özel bir bulut oluşturabilirsiniz](tutorial-create-private-cloud.md).