---
title: 'ML Studio (klasik): Web hizmeti günlüğünü etkinleştirme-Azure'
description: Machine Learning Studio (klasik) Web Hizmetleri için günlük kaydını etkinleştirmeyi öğrenin. Günlüğe kaydetme, API 'Lerde sorun gidermeye yardımcı olmak için ek bilgiler sağlar.
services: machine-learning
author: likebupt
ms.author: keli19
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
ms.date: 06/15/2017
ms.openlocfilehash: b879fac7482b000e1b92f2fcc3f7586ae7dae321
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87430204"
---
# <a name="enable-logging-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri için günlüğe kaydetmeyi etkinleştirme

**Uygulama hedefi:** ![ Evet ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (klasik) ![ ](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../compare-azure-ml-to-studio-classic.md) yok  


Bu belge, Machine Learning Studio (klasik) Web hizmetlerinin günlüğe kaydetme özelliği hakkında bilgi sağlar. Günlüğe kaydetme, Machine Learning Studio (klasik) API 'Lerinde yapılan çağrılarınızı gidermenize yardımcı olabilecek, yalnızca bir hata numarası ve bir ileti dışında ek bilgiler sağlar.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Web hizmeti için günlüğe kaydetmeyi etkinleştirme

[Azure Machine Learning Studio (klasik) Web Hizmetleri](https://services.azureml.net) portalından günlüğe kaydetmeyi etkinleştirirsiniz. 

1. Üzerinde Azure Machine Learning Studio (klasik) Web Hizmetleri portalında oturum açın [https://services.azureml.net](https://services.azureml.net) . Klasik bir Web hizmeti için Ayrıca, Studio 'da (klasik) Machine Learning Studio (klasik) Web Hizmetleri sayfasında **Yeni Web Hizmetleri deneyimi** ' ne tıklayarak portala de ulaşabilirsiniz.

   ![Yeni Web Hizmetleri deneyimi bağlantısı](./media/web-services-logging/new-web-services-experience-link.png)

2. Üstteki menü çubuğunda, yeni bir Web hizmeti için **Web Hizmetleri** ' ne tıklayın veya klasik Web hizmeti Için **Klasik Web Hizmetleri** ' ne tıklayın.

   ![Yeni veya klasik Web hizmetleri seçin](./media/web-services-logging/select-web-service.png)

3. Yeni bir Web hizmeti için Web hizmeti adına tıklayın. Klasik bir Web hizmeti için Web hizmeti adına tıklayın ve ardından sonraki sayfada uygun uç noktaya tıklayın.

4. Üstteki menü çubuğunda, **Yapılandır**' a tıklayın.

5. **Günlüğe kaydetmeyi etkinleştir** seçeneğini *hata* (yalnızca hataları günlüğe kaydetmek için) veya *Tümü* (tam günlük kaydı için) olarak ayarlayın.

   ![Günlüğe kaydetme düzeyini seçin](./media/web-services-logging/enable-logging.png)

6. **Kaydet**’e tıklayın.

7. Klasik Web Hizmetleri için **ml tanılama** kapsayıcısını oluşturun.

   Tüm Web hizmeti günlükleri, Web hizmetiyle ilişkili depolama hesabında **ml-Diagnostics** adlı bir blob kapsayıcısında tutulur. Yeni Web Hizmetleri için, Web hizmetine ilk kez eriştiğinizde bu kapsayıcı oluşturulur. Klasik Web Hizmetleri için, zaten mevcut değilse kapsayıcıyı oluşturmanız gerekir. 

   1. [Azure Portal](https://portal.azure.com), Web hizmetiyle ilişkili depolama hesabına gidin.

   2. **BLOB hizmeti**altında **kapsayıcılar**' a tıklayın.

   3. **Ml-Diagnostics** kapsayıcısı yoksa **+ kapsayıcı**' ya tıklayın, kapsayıcıya "ml-Diagnostics" adını verin ve **erişim türünü** "blob" olarak seçin. **Tamam** düğmesine tıklayın.

      ![Tanılama günlüklerinizi depolamak için yeni bir kapsayıcı oluşturun](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Klasik bir Web hizmeti için Machine Learning Studio (klasik) içindeki Web Hizmetleri panosunun günlüğe kaydetmeyi etkinleştirme anahtarı da vardır. Ancak, günlüğe kaydetme artık Web Hizmetleri Portalı aracılığıyla yönetildiğinden, bu makalede açıklandığı gibi portalda günlüğe kaydetmeyi etkinleştirmeniz gerekir. Zaten Studio 'da (klasik) günlüğe kaydetmeyi etkinleştirdiyseniz, Web Hizmetleri portalında günlüğe kaydetmeyi devre dışı bırakıp yeniden etkinleştirin.


## <a name="the-effects-of-enabling-logging"></a>Günlüğe kaydetmeyi etkinleştirme etkileri
Günlüğe kaydetme etkinleştirildiğinde, Web hizmeti uç noktasındaki tanılama ve hatalar, kullanıcının çalışma alanıyla bağlantılı Azure depolama hesabındaki **ml-tanılama** blob kapsayıcısında günlüğe kaydedilir. Bu kapsayıcı, bu depolama hesabıyla ilişkili tüm çalışma alanları için tüm Web hizmeti uç noktaları için tüm tanılama bilgilerini barındırır.

Günlükler, bir Azure Depolama hesabını araştırmak için kullanılabilen çeşitli araçlardan herhangi biri kullanılarak görüntülenebilir. En kolay Azure portal depolama hesabına gitmeniz, **kapsayıcılar**' a ve ardından **ml-Diagnostics**kapsayıcısına tıklamalıdır.  

## <a name="log-blob-detail-information"></a>Blob ayrıntısı bilgilerini günlüğe kaydet
Kapsayıcıdaki her blob, aşağıdaki eylemlerden tam olarak biri için tanılama bilgilerini barındırır:

* Toplu yürütme yönteminin yürütülmesi  
* Istek-yanıt yönteminin yürütülmesi  
* Istek-yanıt kapsayıcısını başlatma

Her Blobun adı aşağıdaki formun bir ön ekine sahiptir: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Burada _günlük türü_ aşağıdaki değerlerden biridir:  

* toplu iş  
* puan/istek  
* puan/init  

