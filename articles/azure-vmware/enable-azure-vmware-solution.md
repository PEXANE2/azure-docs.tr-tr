---
title: Azure VMware Çözüm kaynağınızı etkinleştirme
description: Azure VMware Çözüm kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla ana bilgisayar isteyebilirsiniz.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: 6d614dffc4ab3127e1e6740b1a8773e5fd7c23ff
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97630896"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Azure VMware Çözüm kaynağını etkinleştirme
[Azure VMware Çözüm](introduction.md) kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla ana bilgisayar isteyebilirsiniz.

## <a name="eligibility-criteria"></a>Uygunluk ölçütleri

Azure aboneliğinde bir Azure hesabınızın olması gerekir. Azure aboneliği aşağıdaki ölçütlerden biriyle uyumlu olmalıdır:

* Microsoft ile [Azure Kurumsal Anlaşma (EA)](../cost-management-billing/manage/ea-portal-agreements.md) kapsamındaki bir abonelik.
* Mevcut CSP Azure 'da bir bulut çözümü sağlayıcısı (CSP) tarafından yönetilen abonelik veya bir Azure planı.


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>EA müşterileri için Azure VMware çözümünü etkinleştirme
Azure VMware Çözüm kaynağını oluşturmadan önce, konaklarınızın ayrılmış olması için bir destek bileti göndermeniz gerekir. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve konaklarınızı ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla ana bilgisayar ayırmak istiyorsanız, aynı işlemden geçmeniz gerekir.


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
   - Ana bilgisayar sayısı
   - Diğer ayrıntılar

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 ana bilgisayarlarını çalıştırmak için en az üç ana bilgisayar önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek konaklar isterseniz, Konakları ayırmak için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Konaklarınızı sağlayabilmeniz için önce **Microsoft. AVS** kaynak sağlayıcısını Azure Portal kaydettiğinizden emin olun.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>CSP müşterileri için Azure VMware çözümünü etkinleştirme 

CSP 'Ler, müşterileri için Azure VMware çözümünü etkinleştirmek üzere [Microsoft Iş ortağı merkezini](https://partner.microsoft.com) kullanmalıdır. Bu makalede, iş ortakları için satın alma yordamını göstermek üzere bir örnek olarak [CSP Azure planı](/partner-center/azure-plan-lp) kullanılmaktadır.

   >[!IMPORTANT] 
   >Azure VMware çözüm hizmeti bir çoklu kiracı gerekli değil. Bunu gerektiren barındırma iş ortakları desteklenmez. 

1. **Iş Ortağı Merkezi**'Nde, **müşteriler** alanına erişmek için **CSP** 'yi seçin.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Microsoft Iş ortağı merkezi müşterileri alanı" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. Müşteriyi seçin ve ardından **Ürün Ekle**' yi seçin.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft İş Ortağı Merkezi" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. **Azure planı** ' nı seçin ve ardından **Sepete Ekle**' yi seçin. 

1. Müşterinizin Azure planı aboneliğini genel olarak ayarlamayı gözden geçirin ve son inceleyin. Daha fazla bilgi için bkz. [Microsoft Iş Ortağı Merkezi belgeleri](/partner-center/azure-plan-manage).

Azure planı yapılandırıldıktan ve abonelik için gereken [Azure RBAC izinleri](/partner-center/azure-plan-manage) varsa, bir Azure plan aboneliğine yönelik kotayı etkinleştirmek için Microsoft 'u devreye alırsınız. **Şirket adına yönetici** (Aobo) yordamını kullanarak [Microsoft iş ortağı merkezi](https://partner.microsoft.com) 'nden Azure Portal erişin.

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
   - Ana bilgisayar sayısı
   - Diğer ayrıntılar
   - Birden çok müşteriyi barındırmak için tasarlanmıştır mı?

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 ana bilgisayarlarını çalıştırmak için en az üç ana bilgisayar önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek konaklar isterseniz, Konakları ayırmak için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Abonelik hizmet sağlayıcısı tarafından yönetiliyorsa, yönetim ekibinin, Iş Ortağı Merkezi 'nden (Aobo) **adına yeniden yönetici** kullanarak Azure Portal erişmesi gerekir. Azure portal bir [Cloud Shell](../cloud-shell/overview.md) örneği başlatın ve **Microsoft. AVS** kaynak sağlayıcısını kaydedin ve Azure VMware çözümü özel bulutunun dağıtımına devam edin.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

1. Abonelik doğrudan müşteri tarafından yönetiliyorsa **Microsoft. AVS** kaynak sağlayıcısı 'nın kaydı abonelikte yeterli izinlere sahip bir kullanıcı tarafından yapılmalıdır, daha fazla ayrıntı ve kaynak sağlayıcısını kaydetme yolları için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md) . 


## <a name="next-steps"></a>Sonraki adımlar

Azure VMware Çözüm kaynağınızı etkinleştirdikten ve uygun ağa sahip olduğunuzda [özel bir bulut oluşturabilirsiniz](tutorial-create-private-cloud.md).
