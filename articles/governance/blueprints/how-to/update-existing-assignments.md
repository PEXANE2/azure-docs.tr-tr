---
title: Portaldan mevcut bir atamayı güncelleştirme
description: Azure 'da portaldan mevcut bir şema atamasını güncelleştirme mekanizması hakkında bilgi edinin.
ms.date: 11/21/2019
ms.topic: how-to
ms.openlocfilehash: 3cce84127475b2f11388b4ad4eb48dedec19cd1d
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873174"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Mevcut bir şema atamasını güncelleştirme

Bir şema atandığında, atama güncelleştirilebilen olabilir. Mevcut bir atamayı güncelleştirmek için aşağıdakiler de dahil olmak üzere çeşitli nedenler vardır:

- [Kaynak kilitlemeyi](../concepts/resource-locking.md) ekleme veya kaldırma
- [Dinamik parametrelerin](../concepts/parameters.md#dynamic-parameters) değerini değiştirme
- Atamayı, şema 'in daha yeni **yayımlanmış** bir sürümüne yükseltin

## <a name="updating-assignments"></a>Atamalar güncelleştiriliyor

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Soldaki sayfadan **atanan** şemalar ' ı seçin.

1. Planlar listesinde, BLUEPRINT atamasını sol tıklatın. Ardından, **atamayı Güncelleştir** DÜĞMESINE tıklayın veya BLUEPRINT atamasını sağ tıklayıp **atamayı Güncelleştir**' i seçin.

   ![Mevcut bir şema atamasını güncelleştirme](../media/update-existing-assignments/update-assignment.png)

1. **Şema ata** sayfası, orijinal atamadan tüm değerlerle önceden doldurulmuş olarak yüklenir.
   **Şema tanım sürümünü**, **kilit atama** durumunu ve şema tanımında bulunan dinamik parametrelerden herhangi birini değiştirebilirsiniz. Değişiklik yapıldığında **ata** ' ya tıklayın.

1. Güncelleştirilmiş atama ayrıntıları sayfasında, yeni durum ' a bakın. Bu örnekte, atamaya **kilitleme** ekledik.

   ![Mevcut bir şema ataması güncelleştirildi-kilit modu değişti](../media/update-existing-assignments/updated-assignment.png)

1. Açılan eklentiyi kullanarak diğer **atama işlemleriyle** ilgili ayrıntıları keşfedebilirsiniz. **Yönetilen kaynakların** tablosu seçili atama işlemine göre güncelleştirilir.

   ![Şema atamasının atama işlemleri](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Atamaları güncelleştirme kuralları

Güncelleştirilmiş atamaların dağıtımı, bazı önemli kurallara uyar. Bu kurallar, zaten dağıtılmış kaynaklara ne olduğunu belirlemek için belirlenir. Dağıtılan veya güncellenen yapıt kaynağının istenen değişikliği ve türü, hangi eylemlerin alınacağını tespit edilir.

- Rol Atamaları
  - Rol ya da rol atanan (Kullanıcı, Grup veya uygulama) değişirse yeni bir rol ataması oluşturulur. Daha önce dağıtılan rol atamaları yerinde kalır.
- İlke Atamaları
  - İlke atamasının parametreleri değiştirilirse, mevcut atama güncellenir.
  - İlke atamasının tanımı değiştirilirse yeni bir ilke ataması oluşturulur.
    Daha önce dağıtılan ilke atamaları yerinde kalır.
  - İlke atama yapıtı Blueprint 'ten kaldırılırsa, dağıtılan ilke atamaları yerinde kalır.
- Azure Resource Manager şablonları
  - Şablon, Kaynak Yöneticisi aracılığıyla bir **PUT**olarak işlenir. Her kaynak türü bu eylemi farklı şekilde işlediğinde, bu eylemin, planlar tarafından çalıştırıldığında etkisini öğrenmek için dahil edilen her kaynak için belgeleri gözden geçirin.

## <a name="possible-errors-on-updating-assignments"></a>Atamaları güncelleştirmede olası hatalar

Atamaları güncelleştirirken, yürütüldüğünde kesen değişiklikler yapmak mümkündür. Bir örnek, daha önce dağıtıldıktan sonra kaynak grubunun konumunu değiştiriyor. [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) tarafından desteklenen herhangi bir değişiklik yapılabilir, ancak Azure Resource Manager üzerinden bir hatayla sonuçlanarak yapılan herhangi bir değişiklik atamanın başarısızlığının oluşmasına neden olur.

Atamanın kaç kez güncelleştirilemeyebilir. Bir hata oluşursa, hatayı saptayın ve atamanın başka bir güncelleştirmesini yapın.  Örnek hata senaryoları:

- Hatalı parametre
- Zaten varolan bir nesne
- Azure Resource Manager tarafından desteklenmeyen bir değişiklik

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.