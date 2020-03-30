---
title: Web hizmeti günlüğü - Azure Machine Learning Studio (klasik) | Microsoft Dokümanlar
description: Machine Learning Studio (klasik) web hizmetleri için günlük yazmayı nasıl etkinleştirmenizi öğrenin. Günlüğe kaydetme, API'lerin sorun giderimi için ek bilgiler sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217855"
---
# <a name="enable-logging-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (klasik) web hizmetleri için günlük kaydetmeyi etkinleştirme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu belge, Machine Learning Studio (klasik) web hizmetlerinin günlük yeteneği hakkında bilgi sağlar. Günlük, yalnızca bir hata numarası ve bir iletinin ötesinde, Machine Learning Studio (klasik) API'lerine yaptığınız çağrıları gidermenize yardımcı olabilecek ek bilgiler sağlar.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Web hizmeti için günlüğe kaydetmeyi etkinleştirme

[Azure Machine Learning Studio (klasik) Web Hizmetleri](https://services.azureml.net) portalından oturum açmanızı etkinleştirin. 

1. Azure Machine Learning Studio (klasik) Web Hizmetleri [https://services.azureml.net](https://services.azureml.net)portalında oturum açın. Klasik bir web hizmeti için, Studio'daki Machine Learning Studio (klasik) Web Hizmetleri sayfasında **Yeni Web Hizmetleri Deneyimi'ni** tıklayarak portala da ulaşabilirsiniz ( klasik).

   ![Yeni Web Hizmetleri Deneyimi bağlantısı](./media/web-services-logging/new-web-services-experience-link.png)

2. Üst menü çubuğunda, Yeni web hizmeti için **Web Hizmetleri'ni** tıklatın veya Klasik web hizmeti için **Klasik Web Hizmetleri'ni** tıklatın.

   ![Yeni veya Klasik web hizmetlerini seçin](./media/web-services-logging/select-web-service.png)

3. Yeni bir web hizmeti için web hizmeti adını tıklatın. Klasik web hizmeti için web hizmeti adını tıklatın ve sonraki sayfada uygun bitiş noktasını tıklatın.

4. Üst menü çubuğunda **Yapıla'yı**tıklatın.

5. Günlük **Oturum Aç** seçeneğini *Hata* (yalnızca hataları günlüğe kaydetmek için) veya *Tümü* (tam günlüğe kaydetme için) olarak ayarlayın.

   ![Günlük düzeyini seçin](./media/web-services-logging/enable-logging.png)

6. **Kaydet**'e tıklayın.

7. Klasik web hizmetleri için **ml-teşhis** kabını oluşturun.

   Tüm web servis günlükleri, web hizmetiyle ilişkili depolama hesabında **ml-diagnostics** adlı bir blob konteynerinde tutulur. Yeni web hizmetleri için bu kapsayıcı, web hizmetine ilk kez erişinizde oluşturulur. Klasik web hizmetleri için, kapsayıcızaten yoksa oluşturmanız gerekir. 

   1. Azure [portalında,](https://portal.azure.com)web hizmetiyle ilişkili depolama hesabına gidin.

   2. **Blob Hizmeti**’nin altında, **Kapsayıcılar**’a tıklayın.

   3. Kapsayıcı **ml-tanılama** yoksa, **+Konteyner'i**tıklatın, kapsayıcıya "ml-tanılama" adını verin ve **Access türünü** "Blob" olarak seçin. **Tamam**'a tıklayın.

      ![Tanılama günlüklerinizi depolamak için yeni bir kapsayıcı oluşturma](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Klasik bir web hizmeti için, Machine Learning Studio'daki Web Hizmetleri Panosu 'nun (klasik) ayrıca günlük oturum açmayı etkinleştirmek için bir anahtarı vardır. Ancak, günlüğe kaydetme artık Web Hizmetleri portalı üzerinden yönetildiği için, bu makalede açıklandığı gibi portalda günlüğe kaydetmeyi etkinleştirmeniz gerekir. Studio'da (klasik) oturum açmayı zaten etkinleştirdiyseniz, Web Hizmetleri Portalı'nda oturum açmayı devre dışı bırakıp yeniden etkinleştirin.


## <a name="the-effects-of-enabling-logging"></a>Günlüğe kaydetmeyi etkinleştirme etkileri
Günlüğe kaydetme etkinleştirildiğinde, web hizmeti bitiş noktasından gelen tanılama ve hatalar, kullanıcının çalışma alanına bağlı Azure Depolama Hesabı'ndaki **ml tanılama** blob konteynerinde günlüğe kaydedilir. Bu kapsayıcı, bu depolama hesabıyla ilişkili tüm çalışma alanları için tüm web hizmeti uç noktalarının tüm tanılama bilgilerini tutar.

Günlükler, bir Azure Depolama Hesabı'nı keşfetmek için kullanılabilen çeşitli araçlardan herhangi birini kullanarak görüntülenebilir. En kolayı Azure portalındaki depolama hesabına gitmek, **Kapsayıcılar'ı**tıklatın ve ardından kapsayıcı **ml tanılama'yı**tıklatın.  

## <a name="log-blob-detail-information"></a>Günlük blob detay bilgileri
Kapsayıcıdaki her blob, tam olarak aşağıdaki eylemlerden biri için tanılama bilgilerini tutar:

* Toplu İşlem yönteminin yürütülmesi  
* İstek-Yanıt yönteminin yürütülmesi  
* İstek-Yanıt kapsayıcısının başlatılması

Her blob adı aşağıdaki formu bir önek vardır: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


_Log türü_ aşağıdaki değerlerden biridir:  

* toplu iş  
* puan/istekler  
* puan/init  

