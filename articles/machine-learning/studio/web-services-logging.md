---
title: Web hizmeti günlüğü-Azure Machine Learning Studio (klasik) | Microsoft Docs
description: Machine Learning Studio (klasik) Web Hizmetleri için günlük kaydını etkinleştirmeyi öğrenin. Günlük API'leri gidermenize yardımcı olacak ek bilgiler sağlar.
services: machine-learning
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
ms.date: 06/15/2017
ms.openlocfilehash: 90e7692fe0e254074d8176d719d0ca9abad54a9b
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79217855"
---
# <a name="enable-logging-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri için günlüğe kaydetmeyi etkinleştirme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu belge, Machine Learning Studio (klasik) Web hizmetlerinin günlüğe kaydetme özelliği hakkında bilgi sağlar. Günlüğe kaydetme, Machine Learning Studio (klasik) API 'Lerinde yapılan çağrılarınızı gidermenize yardımcı olabilecek, yalnızca bir hata numarası ve bir ileti dışında ek bilgiler sağlar.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Bir Web hizmeti için günlük kaydını etkinleştirme

[Azure Machine Learning Studio (klasik) Web Hizmetleri](https://services.azureml.net) portalından günlüğe kaydetmeyi etkinleştirirsiniz. 

1. [https://services.azureml.net](https://services.azureml.net)Azure Machine Learning Studio (klasik) Web Hizmetleri portalında oturum açın. Klasik bir Web hizmeti için Ayrıca, Studio 'da (klasik) Machine Learning Studio (klasik) Web Hizmetleri sayfasında **Yeni Web Hizmetleri deneyimi** ' ne tıklayarak portala de ulaşabilirsiniz.

   ![Yeni Web Hizmetleri deneyiminizin bağlantı](./media/web-services-logging/new-web-services-experience-link.png)

2. Üstteki menü çubuğunda, yeni bir Web hizmeti için **Web Hizmetleri** ' ne tıklayın veya klasik Web hizmeti Için **Klasik Web Hizmetleri** ' ne tıklayın.

   ![Yeni veya Klasik web hizmetleri seçin](./media/web-services-logging/select-web-service.png)

3. Yeni web hizmeti, web hizmeti adına tıklayın. Klasik web hizmeti, web hizmeti adına tıklayın ve ardından sonraki sayfada uygun uç noktaya tıklayın.

4. Üstteki menü çubuğunda, **Yapılandır**' a tıklayın.

5. **Günlüğe kaydetmeyi etkinleştir** seçeneğini *hata* (yalnızca hataları günlüğe kaydetmek için) veya *Tümü* (tam günlük kaydı için) olarak ayarlayın.

   ![Günlüğe kaydetme düzeyini seçin](./media/web-services-logging/enable-logging.png)

6. **Kaydet** düğmesine tıklayın.

7. Klasik Web Hizmetleri için **ml tanılama** kapsayıcısını oluşturun.

   Tüm Web hizmeti günlükleri, Web hizmetiyle ilişkili depolama hesabında **ml-Diagnostics** adlı bir blob kapsayıcısında tutulur. Yeni web hizmetleri için bu kapsayıcı web hizmetine erişim ilk kez oluşturulur. Klasik web hizmetleri için henüz yoksa kapsayıcıyı oluşturun gerekir. 

   1. [Azure Portal](https://portal.azure.com), Web hizmetiyle ilişkili depolama hesabına gidin.

   2. **Blob Hizmeti**’nin altında, **Kapsayıcılar**’a tıklayın.

   3. **Ml-Diagnostics** kapsayıcısı yoksa **+ kapsayıcı**' ya tıklayın, kapsayıcıya "ml-Diagnostics" adını verin ve **erişim türünü** "blob" olarak seçin. **Tamam** düğmesine tıklayın.

      ![Tanılama günlüklerinizi depolamak için yeni bir kapsayıcı oluşturun](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Klasik bir Web hizmeti için Machine Learning Studio (klasik) içindeki Web Hizmetleri panosunun günlüğe kaydetmeyi etkinleştirme anahtarı da vardır. Ancak, günlük kaydı Web Hizmetleri portalından artık yönetildiğinden, bu makalede anlatıldığı gibi portal üzerinden günlük etkinleştirmeniz gerekir. Zaten Studio 'da (klasik) günlüğe kaydetmeyi etkinleştirdiyseniz, Web Hizmetleri portalında günlüğe kaydetmeyi devre dışı bırakıp yeniden etkinleştirin.


## <a name="the-effects-of-enabling-logging"></a>Etkilerini günlüğünü etkinleştirme
Günlüğe kaydetme etkinleştirildiğinde, Web hizmeti uç noktasındaki tanılama ve hatalar, kullanıcının çalışma alanıyla bağlantılı Azure depolama hesabındaki **ml-tanılama** blob kapsayıcısında günlüğe kaydedilir. Bu kapsayıcıdaki tüm web hizmeti uç noktaları için bu depolama hesabıyla ilişkili tüm çalışma alanları için tüm tanılama bilgileri tutar.

Herhangi bir Azure depolama hesabını keşfetmek kullanılabilen çeşitli araçları kullanarak günlükleri görüntülenebilir. En kolay Azure portal depolama hesabına gitmeniz, **kapsayıcılar**' a ve ardından **ml-Diagnostics**kapsayıcısına tıklamalıdır.  

## <a name="log-blob-detail-information"></a>Günlük blob ayrıntı bilgileri
Kapsayıcıdaki her blob, aşağıdaki eylemleri tam olarak birine ait tanılama bilgilerini tutar:

* Bir toplu iş yürütme yönteminin yürütülmesi  
* Bir istek-yanıt yönteminin yürütülmesi  
* İstek-yanıt kapsayıcı başlatma

Her blob adı bir önek aşağıdaki biçime sahiptir: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Burada _günlük türü_ aşağıdaki değerlerden biridir:  

* toplu iş  
* puan/istekleri  
* puan/başlatma  

