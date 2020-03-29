---
title: Bilgi tabanlarını yönet - QnA Maker
description: QnA Maker, bilgi tabanı ayarlarına ve içeriğine erişim sağlayarak bilgi temellerinizi yönetmenize olanak tanır.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 57839125011016daed5f0b3d441a83e8db488198
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80071149"
---
# <a name="create-knowledge-base-and-manage-settings"></a>Bilgi tabanı oluşturma ve ayarları yönetme

QnA Maker, bilgi tabanı ayarlarına ve veri kaynaklarına erişim sağlayarak bilgi temellerinizi yönetmenize olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
> * Azure portalında oluşturulan bir [QnA Maker kaynağı.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) Kaynağı oluşturduğunuzda seçtiğiniz Azure Active Directory ID, Abonelik, QnA kaynak adınızı hatırlayın.

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

1. Azure kimlik bilgilerinizle [QnAMaker.ai](https://QnAMaker.ai) portalında oturum açın.

1. QnA Maker portalında **bilgi tabanı oluştur'u**seçin.

1. **Oluştur** sayfasında, QnA Maker kaynağınız zaten varsa **Adım 1'i** atlayın.

    Kaynağı henüz oluşturmadıysanız, Bir **QnA hizmeti oluştur'u**seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Kaynağı oluşturduğunuzda seçtiğiniz Azure Active Directory ID, Abonelik, QnA kaynak adınızı hatırlayın.

    Azure portalında kaynağı oluşturmayı bitirdiğinizde, QnA Maker portalına dönün, tarayıcı sayfasını yenileyin ve **Adım 2'ye**devam edin.

1. **Adım**3'te, Etkin dizininizi, aboneliğinizi, hizmetinizi (kaynağı) ve hizmette oluşturulan tüm bilgi tabanlarının dilini seçin.

   ![QnA Maker hizmet bilgi tabanı seçme ekran görüntüsü](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. **Adım 3,** bilgi tabanınızı `My Sample QnA KB`adlandırın.

1. **Adım**4'te, ayarları aşağıdaki tabloyla yapılandırın:

    |Ayar|Değer|
    |--|--|
    |**URL'lerden, .pdf veya .docx dosyalarından çoklu dönüş çıkarma olanağı sağlar.**|İşaretli|
    |**Varsayılan yanıt metni**| `Quickstart - default answer not found.`|
    |**+ URL ekle**|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting`|
    |**Chit-sohbet**|**Profesyonel'i** Seçin|

1. **Adım 5**, **KB oluştur'u**seçin.

    Ayıklama işleminin belgeyi okuması ve soruları ve yanıtları tanımlaması birkaç dakika sürer.

    QnA Maker bilgi tabanını başarıyla oluşturduktan sonra **Bilgi bankası** sayfası açılır. Bu sayfadaki bilgi tabanının içeriğini edinebilirsiniz.

## <a name="edit-knowledge-base"></a>Bilgi bankasını düzenleme

1.  Üst gezinti çubuğunda **bilgi üslerimi** seçin.

       Oluşturduğunuz veya sizinle paylaştığınız tüm **hizmetleri, değiştirilen son** tarihin azalan sırasına göre sıralanmış olarak görebilirsiniz.

       ![Bilgi Temellerim](../media/qnamaker-how-to-edit-kb/my-kbs.png)

1. Üzerinde ayarlamalar yapmak için belirli bir bilgi tabanı seçin.

1.  **Ayarlar'ı**seçin. Aşağıdaki liste değiştirebileceğiniz alanları içerir.

       |Hedef|Eylem|
       |--|--|
       |URL ekle|**Bilgi tabanını yönet -> '+ URL ekle'** bağlantısını tıklayarak Bilgi tabanına yeni SSS içeriği eklemek için yeni URL'ler ekleyebilirsiniz.|
       |URL'yi sil|Varolan URL'leri sil simgesini seçerek silebilirsiniz, çöp kutusu.|
       |İçeriği yenileyin|Bilgi tabanınızın mevcut URL'lerin en son içeriğini taramasını istiyorsanız, **Yenile** onay kutusunu seçin. Bu, bilgi tabanını en son URL içeriğiyle bir kez günceller. Bu, düzenli bir güncelleştirme zamanlamasını ayarlamıyor.|
       |Dosya ekle|Desteklenen bir dosya belgesini bilgi tabanının bir parçası olacak şekilde, **Bilgi Tabanını Yönet'i**seçerek ve ardından **Dosya Ekle seçeneğini** seçerek ekleyebilirsiniz|
    |İçeri Aktarma|**Ayrıca, Alma Bilgisi tabanı** düğmesini seçerek varolan herhangi bir bilgi tabanını da içe aktarabilirsiniz. |
    |Güncelleştirme|Bilgi tabanının güncelleştirilmesi, bilgi tabanınızla ilişkili QnA Maker hizmetini oluştururken kullanılan **yönetim fiyatlandırma katmanına** bağlıdır. Gerekirse yönetim katmanını Azure portalından da güncelleştirebilirsiniz.

  1. Bilgi tabanında değişiklik yapmayı bitirdikten sonra, değişiklikleri sürdürmek için sayfanın sağ üst köşesinde **Kaydet'i** seçin ve eğitin.

       ![Kaydet ve Eğit](../media/qnamaker-how-to-edit-kb/save-and-train.png)

       >[!CAUTION]
       >**Kaydet ve eğit'i**seçmeden önce sayfayı terk ederseniz, tüm değişiklikler kaybolur.



## <a name="manage-large-knowledge-bases"></a>Büyük bilgi tabanlarını yönetme

* **Veri kaynak grupları**: Qna'lar çıkarıldıkları veri kaynağına göre gruplandırılır. Veri kaynağını genişletebilir veya daraltabilirsiniz.

    ![Veri kaynağı soru ve yanıtlarını daraltmak ve genişletmek için QnA Maker veri kaynağı çubuğunu kullanın](../media/qnamaker-how-to-edit-kb/data-source-grouping.png)

* **Arama bilgi tabanı**: Bilgi Bankası tablosunun üst kısmındaki metin kutusuna yazarak bilgi tabanını arayabilirsiniz. Soru, yanıt veya meta veri içeriğini aramak için enter'u tıklatın. Arama filtresini kaldırmak için X simgesine tıklayın.

    ![Görünümü yalnızca filtre eşleştirme öğelerine düşürmek için soruların ve yanıtların üzerindeki QnA Maker arama kutusunu kullanın](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

* **Pagination**: Büyük bilgi tabanlarını yönetmek için veri kaynakları arasında hızlı bir şekilde ilerleyin

    ![Soru ve cevap sayfalarında gezinmek için soruların ve yanıtların üzerindeki QnA Maker pagination özelliklerini kullanın](../media/qnamaker-how-to-edit-kb/pagination.png)

## <a name="delete-knowledge-bases"></a>Bilgi tabanlarını silme

Bir bilgi tabanı (KB) silerken kalıcı bir işlemdir. Geri alınamaz. Bilgi tabanını silmeden önce, bilgi tabanını QnA Maker portalının **Ayarlar** sayfasından dışa aktarmanız gerekir.

Bilgi tabanınızı ortak çalışanlarla paylaşırsanız,](işbirliği-bilgi-base.md) sonra silin, herkes KB erişimini kaybeder.

## <a name="next-steps"></a>Sonraki adımlar

Bir [kaynaktaki](language-knowledge-base.md) tüm bilgi tabanlarının dilini yönetme hakkında bilgi edinin.

* QnA kümelerini edit
* QnA Maker tarafından kullanılan Azure kaynaklarını yönetme