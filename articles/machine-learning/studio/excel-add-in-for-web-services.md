---
title: Web Hizmetleri için Excel eklentisi
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Web Hizmetleri, herhangi bir kod yazmaya gerek kalmadan doğrudan Excel'de kullanma
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: f7b5ca7112a6fb79586dc66b385e8015fe10e0b0
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153460"
---
# <a name="excel-add-in-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (klasik) Web Hizmetleri için Excel eklentisi
Excel web hizmetleri herhangi bir kod yazmak zorunda kalmadan doğrudan çağırmak kolaylaştırır.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Çalışma kitabında mevcut bir web hizmetini kullanma adımları

1. Excel eklentisini ve Titanic üzerindeki pastıcılar hakkındaki verileri içeren [örnek Excel dosyasını](https://aka.ms/amlexcel-sample-2)açın. 
 
    > [!NOTE]
    > Web Hizmetleri listesi dosyasına ve altındaki bir onay kutusu "Otomatik-tahmin etmek için" ilgili görürsünüz. Otomatik tahmin ' i etkinleştirirseniz, **Tüm** hizmetlerinizin tahminleri, girişte her değişiklik olduğunda güncelleştirilir. İşaretli değilse, "Tahmin"'ye tıklayın yenileme için gerekir. Etkinleştirmek için otomatik-6. adım bir hizmet düzeyi gidin, tahmin edin.

2. Web hizmeti tıklayarak seçin-"Titanic hayatta tahmin unsuru (Excel Eklentisi örneği) [puan]" Bu örnekte.
   
    ![Web hizmeti seçin](./media/excel-add-in-for-web-services/image1.png)
3. Bu sizi **tahmin** etme bölümüne götürür.  Bu çalışma kitabı zaten örnek veriler içeriyor, ancak boş bir çalışma kitabı için Excel 'de bir hücre seçip **örnek verileri kullan**' a tıklayabilirsiniz.
4. Üst bilgileri ile verileri seçin ve giriş verilerini aralığı simgesine tıklayın.  "Verilerimin üst bilgileri var" kutunun işaretli olduğundan emin olun.
5. **Çıkış**' ın altında çıktının olmasını istediğiniz hücre numarasını girin, örneğin burada "H1" yazın.
6. **Tahmin**' e tıklayın. "Otomatik predıct" onay kutusunu seçerseniz seçili alanlara (giriş olarak belirtilenler) herhangi bir değişiklik isteği ve bir güncelleştirme predıct düğmesine basın etmenize gerek kalmadan çıkış hücrelerin tetikler.
   
    ![Bölüm tahmin edin](./media/excel-add-in-for-web-services/image1.png)

Bir web hizmeti dağıtın veya mevcut bir Web hizmetini kullanın. Web hizmeti dağıtma hakkında daha fazla bilgi için bkz. [öğretici 3: Kredi risk modeli dağıtma](tutorial-part3-credit-risk-deploy.md).

Web hizmetiniz için API anahtarını alın. Gerçekleştirdiğiniz durumlarda bu eylemi yeni Machine Learning web hizmeti bir Machine Learning Klasik web hizmeti mi yayımlanan bağlıdır.

**Klasik Web hizmeti kullanma** 

1. Machine Learning Studio (klasik) bölümünde, sol bölmedeki **Web Hizmetleri** bölümüne tıklayın ve sonra Web hizmeti ' ni seçin.
   
    ![Bir Web hizmeti Studio seçin](./media/excel-add-in-for-web-services/image4.png)
2. Web hizmeti için API anahtarını kopyalayın.
   
    ![Studio API anahtarı](./media/excel-add-in-for-web-services/image5.png)
3. Web hizmetinin **Pano** sekmesinde **istek/yanıt** bağlantısına tıklayın.
4. **İstek URI 'si** bölümünü arayın.  Kopyalayın ve URL'yi kaydedin.

> [!NOTE]
> Artık, klasik bir Machine Learning Web hizmeti için API anahtarını almak üzere [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portalında oturum açmak mümkündür.
> 
> 

**Yeni bir Web hizmeti kullan**

1. [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portalında **Web Hizmetleri**' ne ve ardından Web hizmetinizi seçin. 
2. **Tüketme**' ye tıklayın.
3. **Temel tüketim bilgileri** bölümüne bakın. **Birincil anahtarı** ve **istek-yanıt** URL 'sini kopyalayıp kaydedin.

## <a name="steps-to-add-a-new-web-service"></a>Yeni bir web hizmeti eklemek için adımları

1. Bir web hizmeti dağıtın veya mevcut bir Web hizmetini kullanın. Web hizmeti dağıtma hakkında daha fazla bilgi için bkz. [öğretici 3: Kredi risk modeli dağıtma](tutorial-part3-credit-risk-deploy.md).
2. **Tüketme**' ye tıklayın.
3. **Temel tüketim bilgileri** bölümüne bakın. **Birincil anahtarı** ve **istek-yanıt** URL 'sini kopyalayıp kaydedin.
4. Excel 'de **Web Hizmetleri** bölümüne gidin ( **tahmin** bölümünde yer alıyorsa, Web hizmetleri listesine gitmek için geri okuna tıklayın).
   
    ![Web hizmet seçimi için Git](./media/excel-add-in-for-web-services/image3.png)
5. **Web hizmeti Ekle**' ye tıklayın.
6. URL 'yi, **URL**etiketli Excel eklenti metin kutusuna yapıştırın.
7. API/birincil anahtarını **API anahtarı**etiketli metin kutusuna yapıştırın.
8. **Ekle**'ye tıklayın.
   
    ![Klasik Web hizmeti URL'sini ve API anahtarı.](./media/excel-add-in-for-web-services/image6.png)
9. Web hizmetini kullanmak için yukarıdaki yönergeleri, "bir varolan web hizmeti kullanmak için adımlar." izleyin.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
Çalışma kitabınızı kaydederseniz, eklediğiniz web hizmetleri için API/birincil anahtar da kaydedilir. Bu çalışma kitabını yalnızca güvendiğiniz kişilerle paylaşması gerekir anlamına gelir.

Aşağıdaki yorum bölümünde veya [forumumuzda](https://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)soru sorun.
