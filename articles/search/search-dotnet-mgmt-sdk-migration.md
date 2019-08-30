---
title: Azure Search .NET Yönetim SDK 'Sı sürüm 2 ' ye yükseltme-Azure Search
description: Önceki sürümlerden Azure Search .NET Yönetim SDK 'Sı sürüm 2 ' ye yükseltin. Nelerin yeni olduğunu ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
author: brjohnstmsft
manager: nitinme
ms.author: brjohnst
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.openlocfilehash: 2a59cff7f5313b0ac5a060d698950a4c82160f67
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182258"
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version"></a>Azure Search .NET Yönetim SDK sürümü sürümüne yükseltme 

> [!Important]
> Bu içerik hala yapım aşamasındadır. Azure Search Management .NET SDK 3,0 sürümü NuGet üzerinde kullanılabilir. Yeni sürüme nasıl yükseltileceğini açıklamak için bu geçiş kılavuzunu güncelleştirme üzerinde çalışıyoruz. 
>

[Azure Search .NET Yönetim SDK 'sının](https://aka.ms/search-mgmt-sdk)sürüm 1.0.2 veya daha eski bir sürümünü kullanıyorsanız, bu makale uygulamanızı sürüm 2 ' yi kullanacak şekilde yükseltmenize yardımcı olur.

Azure Search .NET Yönetim SDK 'sının 2. sürümü önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla düşüktür, bu nedenle kodunuzun değiştirilmesi yalnızca en az çaba gerektirir. Kodunuzu yeni SDK sürümünü kullanacak şekilde değiştirme hakkında yönergeler için bkz. [yükseltme adımları](#UpgradeSteps) .

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Sürüm 2 ' deki yenilikler
Azure Search .NET Yönetim SDK 'sının 2. sürümü, genel olarak kullanılan Azure Search yönetimi REST API sürümünü önceki SDK sürümleri, özellikle 2015-08-19 olarak hedefler. SDK 'nın üzerinde yapılan değişiklikler, SDK 'nın kullanılabilirliğini geliştirmek için kesinlikle istemci tarafı değişikliklerdir. Bu değişiklikler şunları içerir:

* `Services.CreateOrUpdate`ve zaman uyumsuz sürümleri artık sağlamayı `SearchService` otomatik olarak yoklamıştır ve hizmet sağlama tamamlanana kadar geri dönmez. Bu, sizi bir tür yoklama kodu yazmak zorunda kalmanızı sağlar.
* Hizmet sağlamayı hala el ile yoklamak istiyorsanız yeni `Services.BeginCreateOrUpdate` yöntemini veya zaman uyumsuz sürümlerinden birini kullanabilirsiniz.
* SDK 'ya `Services.Update` yeni yöntemler ve zaman uyumsuz sürümler eklenmiştir. Bu yöntemler, bir hizmetin artımlı güncelleştirilmesini desteklemek için HTTP PATCH kullanır. Örneğin, artık yalnızca istenen `SearchService` `partitionCount` ve `replicaCount` özellikleri içeren bu yöntemlere bir örnek geçirerek bir hizmeti ölçeklendirebilirsiniz. Çağırma `Services.Get`, döndürülme `SearchService`ve geçirme `Services.CreateOrUpdate` gibi eski yöntem desteklenmeye devam eder, ancak artık gerekli değildir. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet ' i paket `Microsoft.Azure.Management.Search` Yöneticisi konsolu 'nu kullanarak veya proje başvurularınızı sağ tıklatıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek NuGet başvurunuz ' ı güncelleştirin. Visual Studio 'da.

NuGet yeni paketleri ve bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzun nasıl yapılandırıldığına bağlı olarak, başarıyla yeniden oluşturulabilir. Öyleyse başlamaya hazırsınız demektir!

Derlemeniz başarısız olursa, bunun nedeni bazı SDK arabirimlerini (örneğin, birim testi amaçları için) uygulamış olmanız olabilir. Bunu çözmek için gibi yeni yöntemleri `BeginCreateOrUpdateWithHttpMessagesAsync`uygulamanız gerekir.

Herhangi bir derleme hatasını düzelttikten sonra, isterseniz yeni işlevlerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK 'daki yeni özellikler [sürüm 2 ' deki](#WhatsNew)Yenilikler bölümünde ayrıntılı olarak açıklanmıştır.

## <a name="conclusion"></a>Sonuç
SDK 'daki geri bildirimlerinize hoş geldiniz. Sorunlarla karşılaşırsanız [Azure Search MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch)hakkında yardım almak için bize danışabilirsiniz. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun oluşturabilirsiniz. Sorun başlığınız "[Azure Search]" ile öneklediğinizden emin olun.

Azure Search kullandığınız için teşekkürler!
