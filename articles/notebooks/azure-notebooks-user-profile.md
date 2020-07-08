---
title: Azure Notebooks Preview ile kullanım için Kullanıcı profili ve KIMLIĞI
description: Azure Notebooks ile Kullanıcı profilinizi ve Kullanıcı KIMLIĞINIZI oluşturma ve yönetme, paylaşılan not defterlerinin URL 'sinin bir parçası haline geliyor.
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: 9a1ff7f92faec21f537f068f0a33473700ddfed8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85831361"
---
# <a name="your-profile-and-user-id-for-azure-notebooks-preview"></a>Azure Notebooks önizlemesi için profiliniz ve Kullanıcı KIMLIĞINIZ

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Notebooks güçlü, işbirliğine dayalı alanı içinde Kullanıcı profiliniz, genel görüntünüzü başkalarına sunar:

[![Azure Notebooks profili sayfası](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Kullanıcı KIMLIĞINIZ, projeleri ve not defterlerini paylaşmak için kullandığınız URL 'lerin bir parçasıdır. Aşağıdaki listede farklı URL desenleri açıklanmaktadır:

- `https://notebooks.azure.com/<user_id>`: Profil sayfanız.
- `https://notebooks.azure.com/<user_id>/projects`: Projeleriniz. Tüm projeleri görürsünüz; diğer kullanıcılar yalnızca genel projelerinizi görür.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Proje dosyaları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Belirli bir projenin klonları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Belirli bir not defteri veya dosyanın HTML önizlemesi.

## <a name="your-user-id"></a>Kullanıcı Kimliğiniz

Azure Notebooks ilk kez oturum açarken, hesabınıza otomatik olarak geçici bir kullanıcı KIMLIĞI atanır (örneğin, "Anon-idr3ca"). "Anon-" ile başlayan bir Kullanıcı KIMLIĞINIZ olduğu sürece, Azure Notebooks her oturum açışınızda bunu değiştirmenizi ister:

![Azure Notebooks oturum açarken kullanıcı KIMLIĞI oluşturmak için istemde bulun](media/accounts/create-user-id.png)

**Kullanıcı kimliğini Yapılandır** komutu ayrıca geçici kullanıcı adının yanında görünür:

![Geçici bir KIMLIK kullanırken görüntülenen kullanıcı KIMLIĞI komutunu yapılandırma](media/accounts/configure-user-id-command.png)

Ayrıca, Kullanıcı KIMLIĞINIZI profil sayfanızda dilediğiniz zaman değiştirebilirsiniz.

Bir kullanıcı KIMLIĞI, dört ve altı harfli, sayı ve kısa çizgi arasında oluşmalıdır. Başka hiçbir karaktere izin verilmez ve Kullanıcı KIMLIĞI kısa çizgi ile başlayamaz veya bitemez ya da bir satırda birden çok tire kullanamaz. Kullanıcı kimlikleri tüm Azure Notebooks hesaplarında benzersiz olduğundan, "Kullanıcı KIMLIĞI zaten kullanımda" iletisini görebilirsiniz. (Bir Microsoft ticari markayı Kullanıcı KIMLIĞI olarak kullanmaya çalıştığınızda ileti de görüntülenir.) Bu durumlarda, farklı bir kullanıcı KIMLIĞI seçin.

> [!Important]
> KIMLIĞINIZI değiştirmek, önceki KIMLIĞINIZI kullanarak paylaştığınız tüm URL 'Leri geçersiz kılar. Bağlantıları yeniden doğrulamak için KIMLIĞINIZI önceki KIMLIĞINIZE geri dönüştürebilirsiniz. Ancak, başka bir Kullanıcı bu sırada kullanılmamış olan KIMLIĞI talep etmek için mümkündür.

## <a name="your-profile"></a>Profiliniz

Profiliniz, URL 'deki genel olarak görüntülenebilir bilgilerden oluşur `https://notebooks.azure.com/<user_id>` . Profil sayfanız Ayrıca son kullanılan projelerinizi ve tüm yıldızlı projelerini gösterir.

Profilinizi düzenlemek için profil sayfanızdaki **profil bilgilerini düzenle** komutunu kullanın. Profilinizin bölümleri aşağıdaki gibidir:

| Section | İçindekiler |
| --- | --- |
| Profil fotoğrafı | Profil sayfanızda gösterilen bir görüntü. |
| Hesap Bilgileri | Görünen ad, Kullanıcı KIMLIĞI ve genel e-posta hesabı. Buradaki e-posta hesabı, diğer kullanıcılara sizinle iletişim kurmak için bir anlamı sağlar ve Azure Notebooks oturum açmak için kullandığınız [hesaptan](azure-notebooks-user-account.md) farklı olabilir. |
| Profil bilgileri | Konumunuz, şirketiniz, iş unvanı, Web siteniz ve kendinize ait kısa bir açıklama. |
| Sosyal profiller | Bunları paylaşmak istiyorsanız GItHub, Twitter ve Facebook kimlikleriniz. |
| Gizlilik Ayarları | İki komut sağlar:<ul><li>**Profilimi dışarı aktar**: fotoğraflarınızın, profil bilgilerinizin ve güvenlik günlüklerinin dahil Azure Notebooks kaydettiği tüm bilgileri içeren bir *. zip* dosyası oluşturur ve indirir.</li><li>**Hesabımı Sil**: Azure Notebooks depolanan tüm kişisel bilgilerinizi kalıcı olarak siler.</li></ul> |
| Site özelliklerini etkinleştir | Azure Notebooks davranışının yönlerini denetlemenizi sağlar:<ul><li>**Not defterleri Için Birleşik ön uç**: daha hızlı ve daha iyi kalıcılık sağlar.</li><li>**Varsayılan olarak JupyterLab 'Da Çalıştır**: varsayılan olarak, Azure Notebooks çoğu kullanıcı için uygun olan basit bir kullanıcı arabirimi sağlar. Jupyıterlab, deneyimli kullanıcılar için daha zengin ancak daha karmaşık bir arabirim sağlar.</li><li>**VNext Web sitesi**: Bu belgede gösterilen, modernlanmış Web yerleşimini etkin hale sunar.</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md)
