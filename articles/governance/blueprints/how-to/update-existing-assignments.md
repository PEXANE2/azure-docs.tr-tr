---
title: Portaldan varolan bir atamayı güncelleştirme
description: Azure Planları'ndaki portaldan varolan bir plan atamasını güncelleştirme mekanizması hakkında bilgi edinin.
ms.date: 04/15/2020
ms.topic: how-to
ms.openlocfilehash: a00a8bcc10b37af576777e3816a794225a3832f7
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381798"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Varolan bir plan ataması güncelleştirme

Bir plan atandığında, atama güncelleştirilebilir. Varolan bir atamayı güncelleştirmenin çeşitli nedenleri vardır:

- [Kaynak kilitleme](../concepts/resource-locking.md) ekleme veya kaldırma
- [Dinamik parametrelerin](../concepts/parameters.md#dynamic-parameters) değerini değiştirme
- Atamayı planın daha yeni **Yayımlanmış** sürümüne yükseltin

## <a name="updating-assignments"></a>Atamaları güncelleştirme

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Atanmış planları** seçin.

1. Planlar listesinde, plan atamasını sol tıklatın. Ardından **Atamayı Güncelleştir** düğmesini tıklatın VEYA plan atamasını sağ tıklatın ve **Atamayı Güncelleştir'i**seçin.

   :::image type="content" source="../media/update-existing-assignments/update-assignment.png" alt-text="Varolan bir plan ataması güncelleştirme" border="false":::

1. **Plan Ata** sayfası, özgün atamadaki tüm değerlerle önceden doldurulmuş olarak yüklenir.
   Plan tanımı **sürümünü,** Kilit **Atama** durumunu ve plan tanımında bulunan dinamik parametrelerden herhangi birini değiştirebilirsiniz. Değişiklik yapıldığında **Atla'yı** tıklatın.

1. Güncelleştirilmiş atama ayrıntıları sayfasında yeni duruma bakın. Bu örnekte, atamaya **Kilitleme'yi** ekledik.

   :::image type="content" source="../media/update-existing-assignments/updated-assignment.png" alt-text="Varolan bir plan ataması güncelleştirildi - kilit modu değiştirildi" border="false":::

1. Açılan işlemi kullanarak diğer **Atama işlemleri** yle ilgili ayrıntıları keşfedin. Seçili atama işlemi yle **Yönetilen kaynaklar** güncelleştirmeleri tablosu.

   :::image type="content" source="../media/update-existing-assignments/assignment-operations.png" alt-text="Plan atamasının atama işlemleri" border="false":::

## <a name="rules-for-updating-assignments"></a>Atamaları güncelleştirme kuralları

Güncelleştirilmiş atamaların dağıtımı birkaç önemli kural izler. Bu kurallar, zaten dağıtılan kaynaklara ne olacağını belirler. İstenen değişiklik ve dağıtılan veya güncelleştirilen yapı kaynağının türü, hangi eylemlerin yapıldığını belirler.

- Rol Atamaları
  - Rol veya rol atanan (kullanıcı, grup veya uygulama) değişirse, yeni bir rol ataması oluşturulur. Daha önce dağıtılan rol atamaları yerinde bırakılır.
- İlke Atamaları
  - İlke atamaparametreleri değiştirilirse, varolan atama güncelleştirilir.
  - İlke atamasının tanımı değiştirilirse, yeni bir ilke ataması oluşturulur.
    Daha önce dağıtılan ilke atamaları yerinde bırakılır.
  - İlke atama artefaktı plandan kaldırılırsa, dağıtılan ilke atamaları yerinde bırakılır.
- Azure Resource Manager şablonları
  - Şablon, Kaynak Yöneticisi aracılığıyla **PUT**olarak işlenir. Her kaynak türü bu eylemi farklı işlerken, Blueprints tarafından çalıştırıldığında bu eylemin etkisini belirlemek için her dahil edilen kaynak için belgeleri gözden geçirin.

## <a name="possible-errors-on-updating-assignments"></a>Atamaları güncelleştirmede olası hatalar

Atamaları güncellerken, yürütüldüğünde kırılan değişiklikler yapmak mümkündür. Bir örnek, bir kaynak grubunun konumunu zaten dağıtıldıktan sonra değiştirmektir. [Azure Kaynak Yöneticisi](../../../azure-resource-manager/management/overview.md) tarafından desteklenen herhangi bir değişiklik yapılabilir, ancak Azure Kaynak Yöneticisi aracılığıyla hataya neden olacak herhangi bir değişiklik de atamanın başarısız olmasına neden olur.

Bir atamanın kaç kez güncelleştirilebileceği konusunda bir sınırlama yoktur. Bir hata oluşursa, hatayı belirleyin ve atamaya başka bir güncelleştirme yapın.  Örnek hata senaryoları:

- Kötü bir parametre
- Zaten varolan bir nesne
- Azure Kaynak Yöneticisi tarafından desteklenmeyen bir değişiklik

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.