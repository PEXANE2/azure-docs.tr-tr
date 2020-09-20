---
title: Azure VMware Çözüm kaynağınızı etkinleştirme
description: Azure VMware Çözüm kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla düğüm isteyebilirsiniz.
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 72a7aca97067ce4e9ed0e901e4016c82b3549eb1
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817907"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Azure VMware Çözüm kaynağını etkinleştirme
Azure VMware Çözüm kaynağınızı etkinleştirmek için bir destek isteği göndermeyi öğrenin. Ayrıca, mevcut Azure VMware çözümünüz özel bulutunuzda daha fazla düğüm isteyebilirsiniz.

## <a name="eligibility-criteria"></a>Uygunluk ölçütleri

* Microsoft ile bir [Azure Kurumsal Anlaşma (EA)](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements) gerekir.
* Azure aboneliğinde bir Azure hesabınızın olması gerekir.


## <a name="enable-azure-vmware-solution-resource"></a>Azure VMware Çözüm kaynağını etkinleştir
Azure VMware Çözüm kaynağını oluşturmadan önce, düğümlerinizin ayrılması için bir destek bileti göndermeniz gerekir. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve düğümlerinizi ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla düğüm ayırdıysanız, aynı işleme gidebilirsiniz.


1. Azure portal, **Yardım + Destek**altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
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