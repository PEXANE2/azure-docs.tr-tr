---
title: Bir planın yaşam döngüsünü anlama
description: Plan tanımının geçtiği yaşam döngüsü ve plan atamalarını güncelleştirme ve kaldırma dahil olmak üzere her aşamayla ilgili ayrıntılar hakkında bilgi edinin.
ms.date: 07/30/2019
ms.topic: conceptual
ms.openlocfilehash: 46da42ec97f16af91ef8e41362a085d466bbb887
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677381"
---
# <a name="understand-the-lifecycle-of-an-azure-blueprint"></a>Azure Blueprint'in yaşam döngüsünü anlama

Azure'daki birçok kaynak gibi, Azure Blueprints'teki bir plan da tipik ve doğal bir yaşam döngüsüne sahiptir. Artık ihtiyaç duyulmadığında veya ilgili olmadığında oluşturulur, dağıtılır ve son olarak silinirler. Azure Blueprints standart yaşam döngüsü işlemlerini destekler. Daha sonra, DevOps'te önemli bir unsur olan Altyapılarını Kod olarak yöneten kuruluşlar için ortak sürekli tümleştirme ve sürekli dağıtım ardışık noktalarını destekleyen ek statü düzeyleri sağlamak için bunların üzerine inşa eder.

Bir planı ve aşamaları tam olarak anlamak için, standart bir yaşam döngüsünü ele alacağız:

> [!div class="checklist"]
> - Plan oluşturma ve düzenleme
> - Planı yayınlama
> - Planın yeni bir sürümünü oluşturma ve düzenleme
> - Planın yeni bir sürümünü yayımlama
> - Planın belirli bir sürümünü silme
> - Planı silme

## <a name="creating-and-editing-a-blueprint"></a>Plan oluşturma ve düzenleme

Bir plan oluştururken, ona yapıtları ekleyin, yönetim grubuna veya aboneye kaydedin ve benzersiz bir ad ve benzersiz bir sürüm sağladı. Plan şu anda **Taslak** modunda ve henüz atanamıyor. **Taslak** modundayken güncelleştirilip değiştirilmeye devam edebilir.

**Taslak** modunda hiç yayınlanmamış bir **plan, Blueprint Tanımları** sayfasında **Yayınlanmış**olanlardan farklı bir simge görüntüler. **En Son Sürüm,** hiç yayınlanmamış bu planlar için **Taslak** olarak görüntülenir.

[Azure portalı](../create-blueprint-portal.md#create-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#create-a-blueprint)ile bir plan oluşturun ve edin.

## <a name="publishing-a-blueprint"></a>Bir plan yayınlama

**Taslak** modunda bir planda planlanan tüm değişiklikler yapıldıktan sonra, **yayımlanabilir** ve atama için kullanılabilir hale getirilebilir. Planın **Yayımlanmış** sürümü değiştirilemez. Bir kez **Yayınlandı,** plan **Taslak** planları farklı bir simge ile görüntüler ve **Son Sürüm** sütununda sağlanan sürüm numarasını görüntüler.

[Azure portalı](../create-blueprint-portal.md#publish-a-blueprint) veya REST [API](../create-blueprint-rest-api.md#publish-a-blueprint)ile bir plan yayımlayın.

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Planın yeni bir sürümünü oluşturma ve düzenleme

Bir planın **Yayımlanmış** sürümü değiştirilemez. Ancak, planın yeni bir sürümü varolan plana eklenebilir ve gerektiğinde değiştirilebilir. Düzenleyerek varolan bir planda değişiklik yapın. Yeni değişiklikler kaydedildiğinde, plan artık **Yayımlanmamış Değişiklikler'e**sahiptir. Bu değişiklikler planın **yeni** taslak sürümüdür.

[Azure portalı](../create-blueprint-portal.md#edit-a-blueprint)ile bir plan edin.

## <a name="publishing-a-new-version-of-the-blueprint"></a>Planın yeni bir sürümünü yayımlama

Bir planın düzenesene **kadar** düzenlenen her sürümün atanabilmesi için yayımlanmalıdır. Bir planda **Yayımlanmamış Değişiklikler** yapıldığında ancak **Yayımlanmadığında,** Plan ını **Yayımla** düğmesi plan ını edin sayfasında kullanılabilir. Düğme görünmüyorsa, plan zaten **yayımlanmış** ve **Yayımlanmamış Değişiklikler**yok.

> [!NOTE]
> Tek bir plan, her biri aboneliklere atanabilecek birden çok **Yayınlanmış** sürümüne sahip olabilir.

**Yayımlanmamış Değişiklikler**içeren bir plan yayımlamak için, yeni bir plan yayımlamak için aynı adımları kullanın.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Planın belirli bir sürümünü silme

Bir planın her sürümü benzersiz bir nesnedir ve ayrı ayrı **yayınlanabilir.** Bu nedenle, bir planın her sürümü de silinebilir. Bir planın bir sürümünü silerken, bu planın diğer sürümleri üzerinde herhangi bir etkisi yoktur.

> [!NOTE]
> Etkin atamaları olan bir planı silmek mümkün değildir. Önce atamaları silin ve sonra kaldırmak istediğiniz sürümü silin.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Blueprint tanımlarını** seçin ve bir sürümünü silmek istediğiniz planı bulmak için filtre seçeneklerini kullanın. Sayfayı açmak için seçin.

1. Yayınlanan **sürümler** sekmesini seçin ve silmek istediğiniz sürümü bulun.

1. Bu sürümü silmek ve **bu sürümü sil'i**seçmek için sürüme sağ tıklayın.

## <a name="deleting-the-blueprint"></a>Planı silme

Çekirdek planı da silinebilir. Çekirdek planı silmek, **taslak** ve **yayınlanmış** planlar da dahil olmak üzere bu planın tüm plan sürümlerini de siler. Bir planın bir sürümünü silmede olduğu gibi, çekirdek planı silerken plan sürümlerinin herhangi birinin varolan atamaları kaldırılmaz.

> [!NOTE]
> Etkin atamaları olan bir planı silmek mümkün değildir. Önce atamaları silin ve sonra kaldırmak istediğiniz sürümü silin.

[Azure portalı](../create-blueprint-portal.md#delete-a-blueprint) veya REST [API](../create-blueprint-rest-api.md#delete-a-blueprint)ile bir planı silin.

## <a name="assignments"></a>Atamalar

Yaşam döngüsü sırasında bir plan bir aboneye atanabilir birkaç nokta vardır. Planın bir sürümünün modu **Yayımlandığında,** bu sürüm bir aboneye atanabilir. Bu yaşam döngüsü, daha yeni bir sürüm geliştirilirken bir planın sürümlerinin kullanılmasını ve etkin bir şekilde atanmasını sağlar.

Planların sürümleri atandığı için, nereye ve hangi parametrelerle atandıklarını anlamak önemlidir. Parametreler statik veya dinamik olabilir. Daha fazla bilgi edinmek için [statik ve dinamik parametrelere](parameters.md)bakın.

### <a name="updating-assignments"></a>Atamaları güncelleştirme

Bir plan atandığında, atama güncelleştirilebilir. Varolan bir atamayı güncelleştirmenin çeşitli nedenleri vardır:

- [Kaynak kilitleme](resource-locking.md) ekleme veya kaldırma
- [Dinamik parametrelerin](parameters.md#dynamic-parameters) değerini değiştirme
- Atamayı planın daha yeni **Yayımlanmış** sürümüne yükseltin

Nasıl yapılacağını öğrenmek için [varolan atamaları güncelleştir'e](../how-to/update-existing-assignments.md)bakın.

### <a name="unassigning-assignments"></a>Atamaları atama

Plana artık gerek yoksa, yönetim grubundan veya abonelikten atanmamış olabilir. Plan atamasırasında aşağıdakiler oluşur:

- Plan [kaynak kilitleme kaldırma](resource-locking.md)
- Plan atama nesnesinin silinmesi
- (Koşullu) Sistem **tarafından atanan yönetilen** bir kimlik kullanıldıysa,

> [!NOTE]
> Plan ataması tarafından dağıtılan tüm kaynaklar yerinde kalır, ancak artık Azure Planları tarafından korunmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../how-to/update-existing-assignments.md)öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.