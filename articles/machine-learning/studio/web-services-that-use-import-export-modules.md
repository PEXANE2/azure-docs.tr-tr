---
title: Web hizmetlerinde verileri içeri/dışarı aktarma
titleSuffix: ML Studio (classic) - Azure
description: Bir web hizmetinden veri alıp göndermek için verileri içeri aktar ve dışarı veri modülleri'ni kullanmayı öğrenin.
services: machine-learning
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 144a3bc0d9e0499a238e4033d37d5e4d3fa61e05
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79204076"
---
# <a name="deploy-azure-machine-learning-studio-classic-web-services-that-use-data-import-and-data-export-modules"></a>Veri Içeri aktarma ve veri dışa aktarma modülleri kullanan Azure Machine Learning Studio (klasik) Web hizmetlerini dağıtma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Tahmine dayalı bir deneme oluştururken, genellikle bir web hizmeti giriş ve çıkış ekleyin. Denemeyi dağıttığınızda, tüketicilerin gönderebilir ve giriş ve çıkışları aracılığıyla web hizmetinden veri al. Bazı uygulamalar için bir tüketicinin verileri bir veri akışı'ndan kullanılabilir veya Azure Blob Depolama gibi bir dış veri kaynağı zaten bulunan. Bu gibi durumlarda, web hizmeti giriş ve çıkışları kullanarak verileri okumasına ve yazmasına gerek yoktur. Bunlar, bunun yerine, toplu işlem yürütme hizmeti (BES) veri kaynağından verileri içeri aktarma modülü kullanarak verileri okumak için kullanabilir ve puanlama sonuç verileri dışarı aktarma modülünü kullanarak farklı veri konumuna yazma.

Verileri içeri aktar ve dışarı aktarma veri modülleri, okuma ve konumlar gibi HTTP, Hive sorgusu, bir Azure SQL veritabanı, Azure tablo depolama, Azure Blob Depolama, veri akışı aracılığıyla bir Web URL'si sağlayan çeşitli veri veya bir şirket içi SQL veritabanı için yazma.

Bu konuda kullanır "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi" örnek ve veri kümesi censusdata adlı bir Azure SQL tablosuna zaten yüklendi varsayar.

## <a name="create-the-training-experiment"></a>Eğitim denemenizi oluşturma
Açtığınızda "örnek 5: Eğitimi, Test, değerlendir ikili sınıflandırma: yetişkinlere yönelik veri kümesi" örnek örnek yetişkinlere yönelik Görselleştirmenizdeki gelir ikili sınıflandırma veri kümesini kullanır. Ve deneme tuvaline aşağıdaki görüntüye benzer olacaktır:

![Denemeyi ilk yapılandırması.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

Azure SQL tablosundan verileri okumak için:

1. Veri kümesi modülü silin.
2. İçeri aktarma bileşenleri arama kutusuna yazın.
3. Sonuçlar listesinden, deneme tuvaline bir *veri Içeri aktarma* modülü ekleyin.
4. *Veri alma* modülünün çıkışı, *Temizleme eksik veri* modülünün girişi.
5. Özellikler bölmesinde, **veri kaynağı** açılan menüsünde **Azure SQL veritabanı** ' nı seçin.
6. **Veritabanı sunucusu adı**, **veritabanı adı**, **Kullanıcı adı**ve **parola** alanlarına veritabanınız için uygun bilgileri girin.
7. Veritabanı sorgusu alanına şu sorguyu girin.

     [yaş] seçin

        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     dbo.censusdata;
8. Deneme tuvalinin alt kısmındaki **Çalıştır**' a tıklayın.

## <a name="create-the-predictive-experiment"></a>Tahmine dayalı deneme oluşturma
Sonraki web hizmetini dağıttığınız Tahmine dayalı denemeye ayarlayın.

1. Deneme tuvalinin alt kısmındaki **Web hizmeti ayarla** ' yı tıklatın ve tahmine **dayalı Web hizmeti ' ni seçin [önerilir]** .
2. *Web hizmeti giriş* ve *Web hizmeti çıkış modüllerini* tahmine dayalı deneyden kaldırın.
3. Dışarı aktarma bileşenleri arama kutusuna yazın.
4. Sonuçlar listesinden, deneme tuvaline bir *veri dışa aktarma* modülü ekleyin.
5. *Puan modeli* modülünün çıkışı, *verileri dışarı aktarma* modülünün girişi.
6. Özellikler bölmesinde, veri hedefi açılan menüsünde **Azure SQL veritabanı** ' nı seçin.
7. **Veritabanı sunucusu adı**, **veritabanı adı**, **sunucu Kullanıcı hesabı adı**ve **sunucu Kullanıcı hesabı parolası** alanlarında, veritabanınız için uygun bilgileri girin.
8. **Kaydedilecek sütunların virgülle ayrılmış listesi** alanında, puanlanmış Etiketler yazın.
9. **Veri tablosu adı alanına**dbo yazın. ScoredLabels. Tablo mevcut değilse, denemeyi çalıştırma ya da web hizmeti olarak adlandırılır oluşturulur.
10. **DataTable sütunları alanının virgülle ayrılmış listesinde** ScoredLabels yazın.

Son web hizmetini çağıran bir uygulama yazdığınızda, farklı giriş sorgusu veya hedef tablo, çalışma zamanında belirtmek isteyebilirsiniz. Bu giriş ve çıkışları yapılandırmak için Web hizmeti parametreleri özelliğini kullanarak *veri modülünü Içeri aktarma* veri *kaynağı* özelliğini ve *verileri dışarı aktar* veri hedefi özelliğini ayarlayın.  Web hizmeti parametreleri hakkında daha fazla bilgi için Cortana Intelligence ve blog Machine Learning [Azure Machine Learning Studio Web hizmeti parametreleri girişine](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) bakın.

Sorgu Al ve hedef tablo için Web hizmeti parametreleri yapılandırmak için:

1. *Veri Içeri aktarma* modülünün Özellikler bölmesinde, **veritabanı sorgu** alanının sağ üst kısmındaki simgeye tıklayın ve **Web hizmeti parametresi olarak ayarla**' yı seçin.
2. *Veri dışarı aktarma* modülünün Özellikler bölmesinde, **veri tablosu adı** alanının sağ üst kısmındaki simgeye tıklayın ve **Web hizmeti parametresi olarak ayarla**' yı seçin.
3. *Veri modülünü dışarı aktar* Özellikler bölmesinin en altında, **Web hizmeti parametreleri** bölümünde veritabanı sorgusu ' na tıklayın ve sorguyu yeniden adlandırın.
4. **Veri tablosu adı** ' na tıklayın ve bu **tabloyu**yeniden adlandırın.

İşiniz bittiğinde, denemenizi aşağıdaki görüntüye benzer olmalıdır:

![Son deneme görünümünü.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Artık denemeyi bir web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Klasik veya yeni bir web hizmeti için dağıtabilirsiniz.

### <a name="deploy-a-classic-web-service"></a>Klasik Web hizmetini dağıtma
Klasik Web hizmeti olarak dağıtma ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin alt kısmında, Çalıştır'a tıklayın.
2. Çalıştırma tamamlandığında **Web Hizmeti Dağıt** ' a tıklayın ve **Web Hizmeti Dağıt [klasik]** seçeneğini belirleyin.
3. Web hizmeti panosunda API anahtarınızı bulun. Kopyalayın ve daha sonra kullanmak üzere kaydedin.
4. **Varsayılan uç nokta** tablosunda, **Batch EXECUTION** bağlantısına tıklayarak API yardım sayfasını açın.
5. C# Visual Studio 'da bir konsol uygulaması oluşturun: **Yeni** > **Project** > **Visual C#**  > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)** .
6. API Yardım sayfasında, sayfanın altındaki **örnek kod** bölümünü bulun.
7. Kopyalama ve yapıştırma C# Program.cs dosyanıza örnek kod ve blob depolama için tüm başvuruları kaldırın.
8. *Apikey* değişkeninin değerini daha önce kaydedilen API anahtarıyla güncelleştirin.
9. İstek bildirimini bulun ve *verileri içeri* aktarma ve *veri verme* modüllerine geçirilen Web hizmeti parametrelerinin değerlerini güncelleştirin. Bu durumda, özgün sorguyu ancak yeni bir tablo adı tanımlayın.

        var request = new BatchExecutionRequest()
        {
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Uygulamayı çalıştırın.

Tamamlanma çalıştırma, yeni bir tablo Puanlama sonuçlarını içeren bir veritabanına eklenir.

### <a name="deploy-a-new-web-service"></a>Yeni bir Web hizmetini dağıtma

> [!NOTE]
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi için, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md)makalesine bakın.

Yeni bir Web hizmeti dağıtma ve bunu kullanmak için bir uygulama oluşturmak için:

1. Deneme tuvalinin alt kısmındaki **Çalıştır**' a tıklayın.
2. Çalıştırma tamamlandığında **Web Hizmeti Dağıt** ' a tıklayın ve **Web Hizmeti Dağıt [yeni]** seçeneğini belirleyin.
3. Deneme dağıtımı sayfasında, Web hizmetiniz için bir ad girin ve bir fiyatlandırma planı seçin ve **Dağıt**' a tıklayın.
4. **Hızlı başlangıç** sayfasında, **tüketme**' ye tıklayın.
5. **Örnek kod** bölümünde **Batch**' e tıklayın.
6. C# Visual Studio 'da bir konsol uygulaması oluşturun: **Yeni** > **Project** > **Visual C#**  > **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)** .
7. Kopyalama ve yapıştırma C# örnek kodu Program.cs dosyanıza.
8. *Apikey* değişkeninin değerini **temel tüketim bilgisi** bölümünde bulunan **birincil anahtarla** güncelleştirin.
9. *ScoreRequest* bildirimini bulun ve *verileri Içeri* aktarma ve *veri verme* modüllerine geçirilen Web hizmeti parametrelerinin değerlerini güncelleştirin. Bu durumda, özgün sorguyu ancak yeni bir tablo adı tanımlayın.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Uygulamayı çalıştırın.

