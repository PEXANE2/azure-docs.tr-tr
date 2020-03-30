---
title: Web hizmetlerinde Alma/İhracat Verileri
titleSuffix: ML Studio (classic) - Azure
description: Bir web hizmetinden veri göndermek ve almak için Veri Alma ve Dışa Aktarma modüllerini nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204076"
---
# <a name="deploy-azure-machine-learning-studio-classic-web-services-that-use-data-import-and-data-export-modules"></a>Veri Alma ve Veri Verme modüllerini kullanan Azure Machine Learning Studio (klasik) web hizmetlerini dağıtma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Tahmine dayalı bir deneme oluşturduğunuzda, genellikle bir web hizmeti giriş ve çıktısı eklersiniz. Denemeyi dağıttığınızda, tüketiciler giriş ve çıktılar aracılığıyla web hizmetinden veri gönderip alabilir. Bazı uygulamalarda, bir tüketicinin verileri veri akışından kullanılabilir veya Azure Blob depolama gibi harici bir veri kaynağında zaten bulunabilir. Bu gibi durumlarda, web hizmeti giriş ve çıktılarını kullanarak veri okuma ve yazma gerekmez. Bunun yerine, Bir Alma Verisi modüllerini kullanarak veri kaynağından gelen verileri okumak ve bir Dışa aktarma Verisi modüllerini kullanarak puanlama sonuçlarını farklı bir veri konumuna yazmak için Toplu Yürütme Hizmeti'ni (BES) kullanabilirler.

Veri Alma ve Dışa Aktar veri modülleri, HTTP üzerinden Web URL'si, Kovan Sorgusu, Azure SQL veritabanı, Azure Tablo depolama, Azure Blob depolama, Veri Akışı sağlama veya şirket içi SQL veritabanı gibi çeşitli veri konumlarından okuyabilir ve yazabilir.

Bu konu "Örnek 5: Tren, Test, İkili Sınıflandırma için Değerlendirin: Yetişkin Veri Kümesi" örneğini kullanır ve veri kümesinin nüfus sayımı verileri adlı bir Azure SQL tablosuna yüklendiğini varsayar.

## <a name="create-the-training-experiment"></a>Eğitim denemesini oluşturma
"Örnek 5: Tren, Test, İkili Sınıflandırma için Değerlendirin: Yetişkin Dataset" örneğini açtığınızda, örnek Yetişkin Nüfus Sayımı Gelir İkiliSi Sınıflandırması veri kümesini kullanır. Ve tuvaldeki deney aşağıdaki resme benzer:

![Deneyin ilk yapılandırması.](./media/web-services-that-use-import-export-modules/initial-look-of-experiment.png)

Azure SQL tablosundaki verileri okumak için:

1. Veri kümesi modülünü silin.
2. Bileşenler arama kutusunda içe aktarma yazın.
3. Sonuç listesinden, deneme tuvaline bir *İçe Aktarma Veri* modülü ekleyin.
4. *İçe Alma Verisi* modülünün çıktısını *Temiz Eksik Veri* modülünün girdisini bağlayın.
5. Özellikler bölmesinde, **Veri Kaynağı** açılır düşüşünde Azure **SQL Veritabanı'nı** seçin.
6. Veritabanı **sunucu adı,** **Veritabanı adı,** **Kullanıcı adı**ve **Parola** alanlarında, veritabanınız için uygun bilgileri girin.
7. Veritabanı sorgu alanına aşağıdaki sorguyu girin.

     seçin [yaş],

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
     dbo.censusdata'dan;
8. Deneme tuvalinin altında **Çalıştır'ı**tıklatın.

## <a name="create-the-predictive-experiment"></a>Tahmine dayalı deneyi oluşturma
Daha sonra web hizmetinizi dağıttığınız tahmin eyine ayarlarsınız.

1. Deneme tuvalinin alt kısmında, **Web Hizmeti Ayarla'yı** tıklatın ve **Tahmine Dayalı Web Hizmeti [Önerilen] seçeneğini belirleyin.**
2. Web *Hizmeti Girişi* ve *Web Hizmeti Çıktımodüllerini* tahmin denemesinden kaldırın.
3. Bileşenler arama kutusunda dışa aktarma yazın.
4. Sonuç listesinden, deneme tuvaline bir *Dışa Aktarma Verileri* modülü ekleyin.
5. *Puan Modeli* modülünün çıktısını *Dışa Aktarma Verileri* modülüne bağlayın.
6. Özellikler bölmesinde, veri hedefi açılır düşüşünde **Azure SQL Veritabanı'nı** seçin.
7. Veritabanı **sunucu adı**, **Veritabanı adı**, Sunucu kullanıcı **hesabı adı**ve Server kullanıcı **hesabı şifre** alanları, veritabanınız için uygun bilgileri girin.
8. Alan **kaydedilecek sütunların Virgülle ayrılmış listesinde** Puanlı Etiketler yazın.
9. Veri **tablosu ad alanında,** dbo yazın. Puanlı Etiketler. Tablo yoksa, deneme çalıştırıldığında veya web hizmeti çağrıldığında oluşturulur.
10. **Virgülle ayrılmış veri tablosu sütunları** alanı listesinde, ScoredLabels yazın.

Son web hizmetini çağıran bir uygulama yazdığınızda, çalışma zamanında farklı bir giriş sorgusu veya hedef tablo belirtmek isteyebilirsiniz. Bu girdi ve çıktıları yapılandırmak için, *Alma Veri* modülü *Veri kaynağı* özelliğini ve *Dışa Aktarma Veri* modu veri hedef özelliğini ayarlamak için Web Hizmet Parametreleri özelliğini kullanın.  Web Hizmeti Parametreleri hakkında daha fazla bilgi için Cortana Intelligence ve Machine Learning blogundaki [Azure Machine Learning Studio Web Servis Parametreleri girişine](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) bakın.

Alma sorgusu ve hedef tablo için Web Hizmet Parametrelerini yapılandırmak için:

1. *İçe Aktar Veri* modülüözellikleri bölmesinde, **Veritabanı sorgu** alanının sağ üst kısmındaki simgeyi tıklatın ve web **hizmeti parametresi olarak ayarla'yı**seçin.
2. *Dışa* Aktarma Verileri modülüiçin özellikler bölmesinde, Veri tablosu **ad** alanının sağ üst kısmındaki simgeyi tıklatın ve **web hizmeti parametresi olarak Ayarla'yı**seçin.
3. *Dışa* Aktarma Verileri modülü özellikleri bölmesinin alt kısmında, **Web Hizmet Parametreleri** bölümünde Veritabanı sorgusunu tıklatın ve Sorgu'yu yeniden adlandırın.
4. **Veri tablosu adını** tıklatın ve **tabloyu**yeniden adlandırın.

Yaptığınızda, denemeniz aşağıdaki resme benzer olmalıdır:

![Deneyin son bakışı.](./media/web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Artık denemeyi bir web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Klasik veya Yeni web hizmetine dağıtabilirsiniz.

### <a name="deploy-a-classic-web-service"></a>Klasik Web Hizmeti Dağıtma
Klasik Web Hizmeti olarak dağıtmak ve bunu tüketmek için bir uygulama oluşturmak için:

1. Deneme tuvalinin altında Çalıştır'ı tıklatın.
2. Çalışma tamamlandığında, Web **Hizmeti Dağıt'ı** tıklatın ve **Web Hizmeti Dağıt'ı [Klasik]** seçeneğini belirleyin.
3. Web hizmeti panosunda API anahtarınızı bulun. Daha sonra kullanmak üzere kopyalayın ve kaydedin.
4. Varsayılan **Bitiş Noktası** tablosunda, API Yardım Sayfasını açmak için **Toplu Yürütme** bağlantısını tıklatın.
5. Visual Studio'da c# konsol uygulaması oluşturun: **Yeni** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.
6. API Yardım Sayfasında, sayfanın altındaki **Örnek Kod** bölümünü bulun.
7. C# örnek kodunu Program.cs dosyanıza kopyalayıp yapıştırın ve blob depolamasına yapılan tüm başvuruları kaldırın.
8. *ApiKey* değişkeninin değerini daha önce kaydedilmiş API anahtarıyla güncelleştirin.
9. İstek bildirimini bulun ve *Alma Verileri* ve *Dışa Aktarma Verileri* modüllerine geçirilen Web Servis Parametreleri değerlerini güncelleştirin. Bu durumda, özgün sorguyu kullanırsınız, ancak yeni bir tablo adı tanımlarsınız.

        var request = new BatchExecutionRequest()
        {
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Uygulamayı çalıştırın.

Çalıştırma tamamlandıktan sonra, puanlama sonuçlarını içeren veritabanına yeni bir tablo eklenir.

### <a name="deploy-a-new-web-service"></a>Yeni Bir Web Hizmeti Dağıtma

> [!NOTE]
> Yeni bir web hizmetini dağıtmak için, web hizmetini dağıttığınız abonelikte yeterli izinlere sahip olmalısınız. Daha fazla bilgi için Azure [Machine Learning Web Services portalını kullanarak bir Web hizmetini yönet'e](manage-new-webservice.md)bakın.

Yeni Web Hizmeti olarak dağıtmak ve bunu tüketmek için bir uygulama oluşturmak için:

1. Deneme tuvalinin altında **Çalıştır'ı**tıklatın.
2. Çalışma tamamlandığında, Web **Hizmeti Dağıt'ı** tıklatın ve **Web Hizmeti Dağıt'ı [Yeni]** seçeneğini belirleyin.
3. Denemedağıt sayfasında, web hizmetinizin adını girin ve bir fiyatlandırma planı seçin, ardından **Dağıt'ı**tıklatın.
4. Hızlı **Başlangıç** **sayfasında, Tüket'i**tıklatın.
5. Örnek **Kod** bölümünde **Toplu İşlem'i**tıklatın.
6. Visual Studio'da c# konsol uygulaması oluşturun: **Yeni** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.
7. C# örnek kodunu kopyalayıp Program.cs dosyanıza yapıştırın.
8. **Temel tüketim bilgileri** bölümünde bulunan Birincil **Anahtar** ile *apiKey* değişkeninin değerini güncelleştirin.
9. *Puan* bildirimini bulun ve *Alma Verileri* ve *Dışa Aktarma Verileri* modüllerine geçirilen Web Servis Parametreleri değerlerini güncelleştirin. Bu durumda, özgün sorguyu kullanırsınız, ancak yeni bir tablo adı tanımlarsınız.

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

