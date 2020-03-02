---
title: Form tanıyıcı örnek etiketleme aracını dağıtma
titleSuffix: Azure Cognitive Services
description: Denetimli öğrenme konusunda yardımcı olmak için form tanıyıcı örnek etiketleme aracını dağıtmanıza yönelik farklı yollar edinin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: pafarley
ms.openlocfilehash: fa419d7dd9668ac2ce8f2b0eb904117c7e22692d
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78207849"
---
# <a name="deploy-the-sample-labeling-tool"></a>Örnek etiketleme aracını dağıtma

Form tanıyıcı örnek etiketleme Aracı, bir Docker kapsayıcısında çalışan bir uygulamadır. Denetimli öğrenme amacına göre form belgelerini el ile etiketlemek için kullanabileceğiniz faydalı bir kullanıcı arabirimi sağlar. [Etiketlerle eğit](./quickstarts/label-tool.md) hızlı başlangıç, aracın en yaygın senaryo olan yerel bilgisayarınızda nasıl çalıştırılacağını gösterir. 

Bu kılavuz, örnek etiketleme aracını dağıtıp çalıştırmak için alternatif yollar açıklayacak. 

## <a name="deploy-with-azure-container-instances"></a>Azure Container Instances ile dağıtma

Etiket aracını bir Docker Web uygulaması kapsayıcısında çalıştırabilirsiniz. İlk olarak, Azure portal [Yeni bir Web uygulaması kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.WebSite) . Formu aboneliğiniz ve kaynak grubu ayrıntılarınız ile birlikte girin. Gerekli alanlara aşağıdaki bilgileri girin:
* **Yayımla**: Docker kapsayıcısı
* **İşletim** Sistem: Linux

Sonraki sayfada, Docker kapsayıcı kurulumu için aşağıdaki alanları girin:

* **Seçenekler**: tek kapsayıcı
* **Görüntü kaynağı**: Azure Container Registry
* **Erişim türü**: genel
* **Resim ve etiket**: MCR.Microsoft.com/Azure-Cognitive-Services/Custom-form/labeltool:latest

Aşağıdaki adımlar isteğe bağlıdır. Uygulamanız dağıtımı tamamladıktan sonra, bunu çalıştırabilir ve etiket aracına çevrimiçi olarak erişebilirsiniz.

### <a name="connect-to-azure-ad-for-authorization"></a>Yetkilendirme için Azure AD 'ye bağlanma

Web uygulamanızı Azure Active Director 'a (AAD) bağlamanız önerilir, böylece yalnızca kimlik bilgilerinizle bulunan birinin oturum açması ve uygulamayı kullanabilmesi için kullanabilirsiniz. AAD 'ye bağlanmak için [App Service uygulamanızı yapılandırma](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) bölümündeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Eğitim verilerini el ile etiketlemek ve denetimli öğrenimi yapmak için aracının nasıl kullanılacağını öğrenmek için [Etiketler hızlı eğitimi](./quickstarts/label-tool.md) ' ne dönün.
