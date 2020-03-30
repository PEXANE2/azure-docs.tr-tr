---
title: Bilişsel Hizmetleri bir beceriye bağla
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme boru hattına Bilişsel Hizmetler hepsi bir leştirilmiş abonelik ekleme yönergeleri.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 254c912114e3f1c7a495f389bc6a6416cbde7e11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472460"
---
# <a name="attach-a-cognitive-services-resource-to-a-skillset-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki bir beceriye Bilişsel Hizmetler kaynağı ekleme 

Azure Bilişsel Arama'da bir zenginleştirme ardışık lığı yapılandırırken, sınırlı sayıda belgeyi ücretsiz olarak zenginleştirebilirsiniz. Daha büyük ve daha sık karşılaşılan iş yükleri için faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz gerekir.

Bu makalede, zenginleştirme ardışık bir yapıt tanımlayan bir beceriye anahtar atayarak bir kaynağın nasıl eklenmesini öğreneceksiniz.

## <a name="resources-used-during-enrichment"></a>Zenginleştirme sırasında kullanılan kaynaklar

Azure Bilişsel Arama, görüntü analizi ve optik karakter tanıma (OCR), doğal dil işleme için [Metin Analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) ve Metin [Çevirisi](https://azure.microsoft.com/services/cognitive-services/translator-text-api/)gibi diğer zenginleştirmeler için [Bilgisayar Vizyonu](https://azure.microsoft.com/services/cognitive-services/computer-vision/) da dahil olmak üzere Bilişsel Hizmetlere bağımlıdır. Azure Bilişsel Arama'da zenginleştirme bağlamında, bu AI algoritmaları bir *becerinin*içine sarılır , *bir beceriye*yerleştirilir ve dizin oluşturma sırasında bir *dizinleyici* tarafından başvurur.

## <a name="how-billing-works"></a>Faturalandırma nasıl çalışır?

+ Azure Bilişsel Arama, görüntü ve metin zenginleştirme için faturalandırmak için bir beceri de sağladığınız Bilişsel Hizmetler kaynak anahtarını kullanır. Faturalandırılabilir becerilerin yürütülmesi [Bilişsel Hizmetler ödeme-as-you gitmek fiyat.](https://azure.microsoft.com/pricing/details/cognitive-services/)

+ Görüntü ayıklama, belgeler zenginleştirmeden önce çatladığında oluşan bir Azure Bilişsel Arama işlemidir. Görüntü ayıklama faturalandırılabilir. Resim çıkarma fiyatlandırması için [Azure Bilişsel Arama fiyatlandırma sayfasına](https://go.microsoft.com/fwlink/?linkid=2042400)bakın.

+ Metin çıkarma da belge çatlama tümceciği sırasında oluşur. Faturalandırılabilir değil.

+ Koşullu, Şekillendirici, Metin Birleştirme ve Metin Bölme becerileri de dahil olmak üzere Bilişsel Hizmetler olarak adlandırılmaz beceriler faturalandırılamaz.

## <a name="same-region-requirement"></a>Aynı bölge gereksinimi

Azure Bilişsel Arama ve Azure Bilişsel Hizmetleri'nin aynı bölgede bulunmasını şart görüyoruz. Aksi takdirde, bu iletiyi çalışma zamanında alırsınız:`"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Bir hizmeti bölgeler arasında taşımanın bir yolu yoktur. Bu hatayı alırsanız, Azure Bilişsel Arama ile aynı bölgede yeni bir Bilişsel Hizmetler kaynağı oluşturmanız gerekir.

> [!NOTE]
> Bazı yerleşik beceriler bölgesel olmayan Bilişsel Hizmetlere dayanır (örneğin, [Metin Çevirisi Becerisi).](cognitive-search-skill-text-translation.md) Bölgesel olmayan bir beceri kullanmak, isteğinizin Azure Bilişsel Arama bölgesi dışındaki bir bölgede hizmet verebileceği anlamına gelir. Bölgesel olmayan hizmetler hakkında daha fazla bilgi için, [Bölgeye göre Bilişsel Hizmetler ürününe](https://aka.ms/allinoneregioninfo) bakın.

## <a name="use-free-resources"></a>Ücretsiz kaynakları kullanma

AI zenginleştirme eğitimini ve hızlı başlatma alıştırmalarını tamamlamak için sınırlı, ücretsiz işleme seçeneğini kullanabilirsiniz.

Ücretsiz (Sınırlı zenginleştirmeler) kaynakları, dizinleyici başına günde 20 belgeyle sınırlıdır. Sayacı sıfırlamak için dizin oluşturucuyu silebilir ve yeniden oluşturabilirsiniz.

1. İçe Aktar veri sihirbazını açın:

   ![Veri Aktar sihirbazını açma](media/search-get-started-portal/import-data-cmd.png "Veri Aktar sihirbazını açma")

1. Bir veri kaynağı seçin ve **AI zenginleştirme (İsteğe Bağlı) eklemeye devam edin.** Bu sihirbazın adım adım gözden geçirilmesi için Azure [portalında dizin oluştur'a](search-get-started-portal.md)bakın.

1. **Genişlet in Bilişsel Hizmetler** ve sonra Ücretsiz **(Sınırlı zenginleştirmeler)** seçin:

   ![Genişletilmiş Ekle Bilişsel Hizmetler bölümü](./media/cognitive-search-attach-cognitive-services/attach1.png "Genişletilmiş Ekle Bilişsel Hizmetler bölümü")

1. Artık **bilişsel beceriler ekle**de dahil olmak üzere sonraki adımlara devam edebilirsiniz.

## <a name="use-billable-resources"></a>Faturalandırılabilir kaynakları kullanma

Günde 20'den fazla zenginleştirme oluşturan iş yükleri için faturalandırılabilir Bilişsel Hizmetler kaynağı eklediğinden emin olun. Bilişsel Hizmetler API'lerini aramak istemeseniz bile, her zaman faturalandırılabilir Bir Bilişsel Hizmetler kaynağı eklemenizi öneririz. Kaynak eklemek günlük sınırı geçersiz kılar.

Sadece Bilişsel Hizmetler API'leri arayan beceriler için ücretlendirilirsiniz. API tabanlı olmayan [özel beceriler](cognitive-search-create-custom-skill-example.md)veya [metin birleştirme,](cognitive-search-skill-textmerger.md) [metin ayırıcı](cognitive-search-skill-textsplit.md)ve [şekillendirici](cognitive-search-skill-shaper.md)gibi beceriler için faturalandırılmamanız gerekmez.

1. İçe Aktarma veri sihirbazını açın, bir veri kaynağı seçin ve **AI zenginleştirme (İsteğe Bağlı) eklemeye**devam edin.

1. **Genişlet In Ataş Bilişsel Hizmetler** ve ardından yeni Bilişsel Hizmetler kaynak **oluştur'u**seçin. Kaynağı oluşturabilmeniz için yeni bir sekme açılır:

   ![Bilişsel Hizmetler kaynağı oluşturma](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Bilişsel Hizmetler kaynağı oluşturma")

1. **Konum** listesinde, Azure Bilişsel Arama hizmetinizin bulunduğu bölgeyi seçin. Performans nedenleriyle bu bölgeyi kullandığınızdan emin olun. Bu bölgeyi kullanmak, bölgeler arasında giden bant genişliği ücretlerini de geçersiz kılar.

1. Fiyatlandırma **katmanı** listesinde, Azure Bilişsel Arama tarafından sağlanan yerleşik becerileri destekleyen Vizyon ve Dil özellikleri de dahil olmak üzere, Hepsi bir arada Bilişsel Hizmetler özelliklerini almak için **S0'ı** seçin.

   S0 katmanı [için, Bilişsel Hizmetler fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/cognitive-services/)belirli iş yüklerinin fiyatlarını bulabilirsiniz.
  
   + Teklif **Seç** listesinde, **Bilişsel Hizmetlerin** seçildiğinden emin olun.
   + **Dil** özellikleri altında, **Metin Analizi Standardı** oranları AI dizine uygulanır.
   + **Vision** özellikleri altında, **Computer Vision S1** için fiyatlar geçerlidir.

1. Yeni Bilişsel Hizmetler kaynağını sağlamak için **Oluştur'u** seçin.

1. İçe Aktarma verisi sihirbazını içeren önceki sekmeye dönün. Bilişsel Hizmetler kaynağını göstermek için **Yenile'yi** seçin ve ardından kaynağı seçin:

   ![Bilişsel Hizmetler kaynağını seçin](./media/cognitive-search-attach-cognitive-services/attach2.png "Bilişsel Hizmetler kaynağını seçin")

1. Verilerinizde çalıştırmak istediğiniz belirli bilişsel becerileri seçmek için **bilişsel beceriler ekle** bölümünü genişletin. Sihirbazın geri kalanını tamamlayın.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağına varolan bir beceri yi ekleme

Varolan bir becerikaynağınız varsa, yeni veya farklı bir Bilişsel Hizmetler kaynağına ekleyebilirsiniz.

1. **Hizmete genel bakış** **sayfasında, Skillsets'i**seçin:

   ![Beceri kümeleri sekmesi](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Beceri kümeleri sekmesi")

1. Skillset'in adını seçin ve sonra varolan bir kaynak seçin veya yeni bir kaynak oluşturun. Değişikliklerinizi onaylamak için **Tamam'ı** seçin.

   ![Skillset kaynak listesi](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Skillset kaynak listesi")

   **Ücretsiz (Sınırlı zenginleştirmeler)** seçeneğinin sizi günlük 20 belgeyle sınırladığını ve yeni bir faturalandırılabilir kaynak sağlamak için **yeni Bilişsel Hizmetler oluştur kaynağını** kullanabileceğinizi unutmayın. Yeni bir kaynak oluşturursanız, Bilişsel Hizmetler kaynakları listesini yenilemek için **Yenile'yi** seçin ve ardından kaynağı seçin.

## <a name="attach-cognitive-services-programmatically"></a>Bilişsel Hizmetleri programlı olarak ekle

Skillset'i programlı olarak tanımlarken, skillset'e bir `cognitiveServices` bölüm ekleyin. Bu bölümde, beceri ile ilişkilendirmek istediğiniz Bilişsel Hizmetler kaynağının anahtarını ekleyin. Kaynağın Azure Bilişsel Arama kaynağınızla aynı bölgede olması gerektiğini unutmayın. Ayrıca `@odata.type`içerir ve `#Microsoft.Azure.Search.CognitiveServicesByKey`ayarlayın.

Aşağıdaki örnekte bu desen gösterilmektedir. Tanımın `cognitiveServices` sonundaki bölüme dikkat edin.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Örnek: Tahmini maliyetleri

Bilişsel arama diziniile ilişkili maliyetleri tahmin etmek için, bazı numaraları çalıştırabilmeniz için ortalama bir belgenin nasıl göründüğüne dair bir fikirle başlayın. Örneğin, yaklaşık:

+ 1000 PDF.
+ Her biri altı sayfa.
+ Sayfa başına bir resim (6.000 resim).
+ Sayfa başına 3.000 karakter.

Her PDF'nin belge kırılması, görüntü ve metin çıkarma, görüntülerin optik karakter tanıma (OCR) ve kuruluşların varlık tanıması gibi bir ardışık kaynak birimi varsayalım.

Bu makalede gösterilen fiyatlar varsayımsaldır. Tahmin sürecini göstermek için kullanılırlar. Maliyetleriniz daha düşük olabilir. İşlemlerin gerçek fiyatları için [bkz.](https://azure.microsoft.com/pricing/details/cognitive-services)

1. Metin ve resim içeriğiyle belge kırılması için metin ayıklama şu anda ücretsizdir. 6.000 görüntü için, çıkarılan her 1.000 resim için 1 $ varsayalım. Bu adım için 6.00 dolar.

2. İngilizce 6.000 görüntü OCR için, OCR bilişsel beceri en iyi algoritma (DescribeText) kullanır. Analiz edilecek 1.000 görüntü başına 2,50 $ bir maliyet varsayarsak, bu adım için 15,00 $ ödeyecek.

3. Varlık çıkarma için, sayfa başına toplam üç metin kaydınız olur. Her kayıt 1000 karakterdir. Sayfa başına üç metin kaydı 6.000 sayfayla çarpılır ve 18.000 metin kaydına eşittir. 1.000 metin kaydı başına 2,00 TL varsayarsak, bu adım 36,00 TL'ye mal olur.

Hepsini bir araya getirerek, açıklanan skillset ile bu tür 1.000 PDF belgeleri yutmak için yaklaşık 57,00 $ ödemek istiyorum.

## <a name="next-steps"></a>Sonraki adımlar
+ [Azure Bilişsel Arama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanların haritası nasıl eşlenir?](cognitive-search-output-field-mapping.md)
