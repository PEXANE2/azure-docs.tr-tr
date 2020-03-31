---
title: Excel'de web hizmeti tüketin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan web hizmetlerini doğrudan Excel'den aramayı kolaylaştırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: 333ed411ab818cff77a7cba6c7de4f42c36f5b6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218223"
---
# <a name="consuming-an-azure-machine-learning-studio-classic-web-service-from-excel"></a>Excel'den Azure Machine Learning Studio (klasik) Web Hizmeti tüketme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan web hizmetlerini doğrudan Excel'den aramayı kolaylaştırır.

Excel 2013 (veya daha sonra) veya Excel Online kullanıyorsanız, Excel [Excel eklentisini](excel-add-in-for-web-services.md)kullanmanızı öneririz.



## <a name="steps"></a>Adımlar
Bir web hizmeti yayımlayın. [Öğretici 3: Dağıtım kredi riski modeli](tutorial-part3-credit-risk-deploy.md) nasıl yapılacağını açıklar. Şu anda Excel çalışma kitabı özelliği yalnızca tek bir çıktısı olan İstek/Yanıt hizmetleri (yani tek bir puanlama etiketi) için desteklenir. 

Bir web hizmetiniz olduktan sonra stüdyonun solundaki **WEB HİzMETLerİ** bölümüne tıklayın ve ardından Excel'den tüketmek üzere web hizmetini seçin.

**Klasik Web Hizmeti**

1. Web hizmeti için **PANO** sekmesinde **İSTEK/YANıT** hizmeti için bir satır vardır. Bu hizmetin tek bir çıktısı varsa, bu satırdaki **Excel Çalışma Kitabı İndir** bağlantısını görmeniz gerekir.

    ![Studio (klasik) Web Hizmeti portalını kullanarak Excel Çalışma Kitabını İndirin](./media/consuming-from-excel/excellink.png)
2. Excel **Çalışma Kitabını İndir'e**tıklayın.

**Yeni Web Hizmeti**

1. Azure Machine Learning Web Service **portalında, Tüket'i**seçin.
2. Tüketim sayfasında, Web **hizmeti tüketim seçenekleri** bölümünde Excel simgesini tıklatın.

**Çalışma kitabını kullanma**

1. Çalışma kitabını aç.
2. Bir Güvenlik Uyarısı görüntülenir; **Düzenlemeyi Etkinleştir** düğmesine tıklayın.

    ![Korumalı görünüm güvenlik uyarısını kaldırmak için düzenlemeyi etkinleştirme](./media/consuming-from-excel/enableeditting.png)
3. Bir Güvenlik Uyarısı görüntülenir. Elektronik tablonuzdaki makroları çalıştırmak için **İçeriği Etkinleştir** düğmesini tıklatın.

    ![Makroları devre dışı bırakarak Güvenlik Uyarısını kapatmak için İçeriği etkinleştirme](./media/consuming-from-excel/enablecontent.png)
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. RRS web hizmetine veya **PARAMETRELERE**giriş olarak mavi sütunlar gereklidir. RRS hizmetinin çıktısını not edin, TAHMIN **DEĞERLerİ** yeşil. Belirli bir satırın tüm sütunları doldurulduğunda, çalışma kitabı otomatik olarak puanlama API'sini çağırır ve puanalan sonuçları görüntüler.

    ![Parametre girişleri ve ortaya çıkan tahmin edilen değerler için tablo](./media/consuming-from-excel/sampletable.png)
5. Birden fazla satır puan lamak için ikinci satırı verilerle doldurun ve öngörülen değerler üretilir. Hatta aynı anda birkaç satır yapıştırabilirsiniz.

Verileri görselleştirmeye yardımcı olmak için öngörülen değerlerle Excel özelliklerinden herhangi birini (grafikler, güç eşlemi, koşullu biçimlendirme, vb.) kullanabilirsiniz.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
Makroların çalışması için API Anahtarınızın elektronik tablonun bir parçası olması gerekir. Bu, çalışma kitabını yalnızca güvendiğiniz varlıklarla/kişilerle paylaşmanız gerektiği anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bu iki durumda RRS çağrısı yapılır:

1. Bir satırın tüm **PARAMETRELERİnDE** İlk KEZ İçERİğİ
2. PARAMETRELERDEN herhangi biri, tüm **PARAMETRELERInE** girilen bir satırda **değişir.**