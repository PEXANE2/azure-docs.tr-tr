---
title: Azure Arama .NET Management SDK sürümü 2'ye yükseltme
titleSuffix: Azure Cognitive Search
description: Önceki sürümlerden Azure Arama .NET Management SDK sürüm 2'ye yükseltin. Yenilikleri ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b18e9688141ee64eb7dfcb82ce58db198e324b5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847531"
---
# <a name="upgrading-versions-of-the-azure-search-net-management-sdk"></a>Azure Arama .NET Yönetimi SDK sürümlerini yükseltme

> [!Important]
> Bu içerik hala yapım aşamasındadır. Azure Arama Yönetimi .NET SDK sürümü 3.0 NuGet'de kullanılabilir. Yeni sürüme nasıl yükseltilen açıklamak için bu geçiş kılavuzunu güncelleştirmeye çalışıyoruz. 
>

[Azure Arama .NET Management SDK'nın](https://aka.ms/search-mgmt-sdk)1.0.2 veya daha eski sürümünü kullanıyorsanız, bu makale, sürüm 2'yi kullanmak üzere uygulamanızı yükseltmenize yardımcı olur.

Azure Arama .NET Management SDK sürümü 2 önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla küçüktür, bu nedenle kodunuzu değiştirmek yalnızca en az çaba yı gerektirmelidir. Yeni SDK sürümünü kullanmak için kodunuzu nasıl değiştireceğinize ilişkin yönergeler için [yükseltme adımları'na](#UpgradeSteps) bakın.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Sürüm 2'deki yenilikler
Azure Arama .NET Management SDK sürümü 2, Azure Arama Yönetimi REST API'sinin önceki SDK sürümleriyle, özellikle de 2015-08-19'daki genel sürümü hedeflemektedir. SDK'daki değişiklikler, SDK'nın kullanılabilirliğini artırmak için kesinlikle istemci tarafı değişiklikleridir. Bu değişiklikler şunlardır:

* `Services.CreateOrUpdate`ve onun eşzamanlı sürümleri artık otomatik olarak `SearchService` hükmü yoklar ve hizmet sağlama tamamlanana kadar geri dönmeyin. Bu, bu tür anket kodu kendiniz yazmak zorunda sizi kurtarır.
* Hizmet sağlama yı el ile yoklamak istiyorsanız, yeni `Services.BeginCreateOrUpdate` yöntemi veya eşzamanlı sürümlerinden birini kullanabilirsiniz.
* Yeni `Services.Update` yöntemler ve onun eşzamanlı sürümleri SDK'ya eklendi. Bu yöntemler, bir hizmetin artımlı güncellenmesini desteklemek için HTTP PATCH'i kullanır. Örneğin, artık yalnızca istenen `SearchService` `partitionCount` ve `replicaCount` özellikleri içeren bu yöntemlere bir örnek geçirerek bir hizmet ölçeklendirebilirsiniz. Eski arama `Services.Get`yolu , döndürülen `SearchService`değiştirme ve `Services.CreateOrUpdate` onu geçirme hala desteklenir, ancak artık gerekli değildir. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet `Microsoft.Azure.Management.Search` Paket Yöneticisi Konsolunu kullanmak veya proje referanslarınıza sağ tıklayarak ve "NuGet Paketlerini Yönet"i seçerek NuGet başvurunuzu güncelleyin. Visual Studio'da.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden oluşturun. Kodunuzu nasıl yapılandırıldığına bağlı olarak, başarılı bir şekilde yeniden oluşturulabilir. Eğer öyleyse, gitmeye hazırsın!

Yapınız başarısız olursa, bunun nedeni değişen Bazı SDK arabirimleri (örneğin, birim sınama amacıyla) uygulamış olması olabilir. Bunu çözmek için, '. gibi `BeginCreateOrUpdateWithHttpMessagesAsync`yeni yöntemleri uygulamanız gerekir.

Herhangi bir yapı hatasını düzelttikte, isterseniz yeni işlevselliklerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK'daki yeni özellikler [sürüm 2'deki yeniliklerde](#WhatsNew)ayrıntılı olarak açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
SDK hakkındaki görüşlerinizi bekliyoruz. Sorunlarla karşılaşırsanız, lütfen sorularınızı [Stack Overflow'a](https://stackoverflow.com/questions/tagged/azure-cognitive-search?tab=Newest)gönderin. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun dosyalayabilirsiniz. Sorun başlığınızı "[arama]" ile etiketlediğinden emin olun.
