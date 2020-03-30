---
title: Web hizmetleri için Excel eklentisi
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Web hizmetlerini herhangi bir kod yazmadan doğrudan Excel'de kullanma.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: e30103589c1baf9a165839cd041ff511a119c5ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204384"
---
# <a name="excel-add-in-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (klasik) web hizmetleri için Excel Eklentisi

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
Excel, herhangi bir kod yazmaya gerek kalmadan web hizmetlerini doğrudan aramayı kolaylaştırır.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Çalışma Kitabında Varolan Bir Web Hizmetini Kullanma Adımları

1. Excel eklentisi ve Titanik'teki yolcularla ilgili verileri içeren [örnek Excel dosyasını](https://aka.ms/amlexcel-sample-2)açın. 
 
    > [!NOTE]
    > Web Hizmetleri'nin dosyayla ilgili listesini ve en altta "Otomatik tahmin" için bir onay kutusunu görürsünüz. **Tüm** hizmetlerinizin tahminlerini otomatik olarak tahmin etmenizi etkinleştiriseniz, girişlerde her değişiklik olduğunda güncellenir. İşaretlenmemişse, yenilemek için "Tümlerini Tahmin Et"e tıklamanız gerekir. Servis düzeyinde otomatik tahminde bulunmaktadır etkinleştirmek için adım 6'ya gidin.

2. Bu örnekte " Titanic Survivor Predictor (Excel Add-in Sample) [Score]" seçeneğini tıklayarak web hizmetini seçin.
   
    ![Web hizmetini seçin](./media/excel-add-in-for-web-services/image1.png)
3. Bu sizi **Tahmin** bölümüne götürür.  Bu çalışma kitabı zaten örnek veriler içerir, ancak boş bir çalışma kitabı için Excel'de bir hücre seçebilir ve **örnek verileri kullan'ı**tıklatabilirsiniz.
4. Üstbilgi içeren verileri seçin ve giriş veri aralığı simgesini tıklatın.  "Verilerimin üstbilgileri var" kutusunun işaretli olduğundan emin olun.
5. **Output**altında, örneğin burada "H1" gibi çıktının olmasını istediğiniz hücre numarasını girin.
6. **Tahmin Et'i**tıklatın. "Otomatik tahmin" onay kutusunu seçerseniz, seçili alanlardaki herhangi bir değişiklik (giriş olarak belirtilenler) bir isteği ve tahmin düğmesine basmanıza gerek kalmadan çıktı hücrelerinin güncelliğini tetikler.
   
    ![Bölümü tahmin et](./media/excel-add-in-for-web-services/image1.png)

Bir web hizmeti dağıtın veya varolan bir Web hizmetini kullanın. Bir web hizmetini dağıtma hakkında daha fazla bilgi için [Bkz. Öğretici 3: Kredi risk modelini dağıtın.](tutorial-part3-credit-risk-deploy.md)

Web hizmetiniz için API anahtarını alın. Bu eylemi nerede gerçekleştirdiğiniz, Yeni Makine Öğrenimi web hizmetinin Klasik Machine Learning web hizmetini yayınlayıp yayınlamadığınıza bağlıdır.

**Klasik web hizmeti kullanma** 

1. Machine Learning Studio'da (klasik), sol bölmedeki **WEB HİzMETLerİ** bölümünü tıklatın ve ardından web hizmetini seçin.
   
    ![Stüdyo bir Web hizmeti seçin](./media/excel-add-in-for-web-services/image4.png)
2. Web hizmeti için API anahtarını kopyalayın.
   
    ![Stüdyo API anahtarı](./media/excel-add-in-for-web-services/image5.png)
3. Web hizmeti için **PANO** sekmesinde, **İsteM/YANıT** bağlantısını tıklatın.
4. **İstek URI** bölümüne bakın.  URL'yi kopyalayın ve kaydedin.

> [!NOTE]
> Klasik Machine Learning web hizmetinin API anahtarını almak için [Azure Machine Learning Web Services](https://services.azureml.net) portalında oturum açabilirsiniz.
> 
> 

**Yeni web hizmeti kullanma**

1. Azure [Machine Learning Web Services](https://services.azureml.net) portalında **Web Hizmetleri'ni**tıklatın ve ardından web hizmetinizi seçin. 
2. **Tüket'i**tıklatın.
3. **Temel tüketim bilgileri** bölümüne bakın. **Birincil Anahtarı** ve **İstek-Yanıt** URL'sini kopyalayın ve kaydedin.

## <a name="steps-to-add-a-new-web-service"></a>Yeni web hizmeti ekleme adımları

1. Bir web hizmeti dağıtın veya varolan bir Web hizmetini kullanın. Bir web hizmetini dağıtma hakkında daha fazla bilgi için [Bkz. Öğretici 3: Kredi risk modelini dağıtın.](tutorial-part3-credit-risk-deploy.md)
2. **Tüket'i**tıklatın.
3. **Temel tüketim bilgileri** bölümüne bakın. **Birincil Anahtarı** ve **İstek-Yanıt** URL'sini kopyalayın ve kaydedin.
4. Excel'de Web **Hizmetleri** bölümüne gidin **(Tahmin** et bölümündeyseniz, web hizmetleri listesine gitmek için arka ok'u tıklatın).
   
    ![Web hizmeti seçimine gitme](./media/excel-add-in-for-web-services/image3.png)
5. **Web Hizmeti Ekle'yi**tıklatın.
6. URL etiketli Excel eklentimetin kutusuna **URL'yi**yapıştırın.
7. API/Birincil anahtarı **API tuşu**etiketli metin kutusuna yapıştırın.
8. **Ekle**’ye tıklayın.
   
    ![Klasik bir Web hizmeti için URL ve API anahtarı.](./media/excel-add-in-for-web-services/image6.png)
9. Web hizmetini kullanmak için, "Varolan bir web Hizmetini Kullanma Adımları" başlıklı önceki yönergeleri izleyin.

## <a name="sharing-your-workbook"></a>Çalışma Kitabınızı Paylaşma
Çalışma kitabınızı kaydederseniz, eklediğiniz web hizmetleri için API/Birincil anahtar da kaydedilir. Bu, çalışma kitabını yalnızca güvendiğiniz kişilerle paylaşmanız gerektiği anlamına gelir.

Aşağıdaki yorum bölümünde veya [forumumuzda](https://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)herhangi bir soru sorun.
