---
title: Web hizmetini Excel 'de kullanma
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan Web hizmetlerini doğrudan Excel 'den çağırmayı kolaylaştırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: 3314852de2a5fc76ff152d05649fabb5eac2757e
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168881"
---
# <a name="consuming-an-azure-machine-learning-studio-classic-web-service-from-excel"></a>Excel 'den bir Azure Machine Learning Studio (klasik) Web hizmeti kullanma

 Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan Web hizmetlerini doğrudan Excel 'den çağırmayı kolaylaştırır.

Excel 2013 (veya üzeri) veya Excel Online kullanıyorsanız, Excel [Excel eklentisini](excel-add-in-for-web-services.md)kullanmanızı öneririz.



## <a name="steps"></a>Adımlar
Web hizmeti yayımlama. [Öğretici 3: Kredi risk modelini dağıtma](tutorial-part3-credit-risk-deploy.md) , bunun nasıl yapılacağını açıklar. Şu anda Excel çalışma kitabını özelliği yalnızca tek bir çıkış (diğer bir deyişle, tek bir Puanlama etiketi) sahip istek/yanıt hizmetler için desteklenir. 

Bir Web hizmetiniz olduktan sonra, Studio 'nun sol tarafındaki **Web Hizmetleri** bölümüne tıklayın ve sonra Excel 'den kullanmak için Web hizmeti ' ni seçin.

**Klasik Web hizmeti**

1. Web hizmetinin **Pano** sekmesinde, **istek/yanıt** hizmeti için bir satırdır. Bu hizmetin tek bir çıkışı varsa, bu satırda **Excel çalışma kitabı indir** bağlantısını görmeniz gerekir.

    ![Studio (klasik) Web hizmeti portalını kullanarak Excel çalışma kitabını indirme](./media/consuming-from-excel/excellink.png)
2. **Excel çalışma kitabını indir**' e tıklayın.

**Yeni Web hizmeti**

1. Azure Machine Learning Web hizmeti portalında **tüketme**' yi seçin.
2. Kullanım sayfasında, **Web hizmeti tüketim seçenekleri** bölümünde, Excel simgesine tıklayın.

**Çalışma kitabını kullanma**

1. Çalışma kitabını açın.
2. Bir güvenlik uyarısı görünür; **Düzenle 'Yi etkinleştir** düğmesine tıklayın.

    ![Korumalı Görünüm güvenlik uyarısını kaldırmak için Düzenle 'yi etkinleştir](./media/consuming-from-excel/enableeditting.png)
3. Bir güvenlik uyarısı görüntülenir. Elektronik tablonuzun makrolarını çalıştırmak için **Içeriği etkinleştir** düğmesine tıklayın.

    ![Makroları devre dışı bırakma güvenlik uyarısını kapatmak için Içeriği etkinleştirin](./media/consuming-from-excel/enablecontent.png)
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. , RR 'ler Web hizmetine veya **parametrelerine**giriş olarak Blue sütunları gereklidir. RR hizmetinin çıkışını, **tahmin EDILEN değerleri** yeşil olarak dikkate alın. Belirli bir satır için tüm sütunları dolduğunda, çalışma kitabına otomatik olarak Puanlama API'yi çağıran ve puanlanmış sonuçların görüntüler.

    ![Parametre girişleri ve sonuç tahmini değerleri için tablo](./media/consuming-from-excel/sampletable.png)
5. Birden fazla satır puanlamak için dolgu verileri ve tahmin edilen değerler ikinci satırı oluşturulur. Aynı anda birden çok satır bile yapıştırabilirsiniz.

Verileri görselleştirme yardımcı olmak için tahmin edilen değerleri (grafikleri power map, koşullu biçimlendirme, vb.) Excel özelliklerinden herhangi birini kullanabilirsiniz.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
İş için makrolar, API anahtarınızı elektronik bir parçası olmalıdır. Bu çalışma kitabını yalnızca varlık/güvenilir kişiler ile paylaşmalıdır anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bu iki durumda bir RRS çağrısıyla yapılır:

1. Bir satırda, tüm **parametrelerinde** içerik olduğunda ilk kez
2. **Parametrelerin** tüm **parametreleri** girilen bir satırdaki herhangi bir zaman değişir.