---
title: Kullanıcı profili ve Azure not defterleri ile kullanmak için kimliği
description: Azure Notebooks ile Kullanıcı profilinizi ve Kullanıcı KIMLIĞINIZI oluşturma ve yönetme, paylaşılan not defterlerinin URL 'sinin bir parçası haline geliyor.
ms.topic: article
ms.date: 02/25/2019
ms.openlocfilehash: 0874fe8223ae9c49ccfe4e8efedf2620117a0f8c
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277601"
---
# <a name="your-profile-and-user-id-for-azure-notebooks"></a>Azure not defterleri için profil ve kullanıcı kimliği

Azure not defterleri, güçlü, işbirliğine dayalı alanı içinde kullanıcı profilinizin genel görüntünüzü başkalarına sunar:

[Bir Azure Notebooks profili sayfası ![](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Kullanıcı Kimliğinizi, projeleri ve Not Defterleri paylaşmak için kullandığınız URL'leri bir parçasıdır. Aşağıdaki listede, farklı bir URL desenleri açıklar:

- `https://notebooks.azure.com/<user_id>`: profil sayfanız.
- `https://notebooks.azure.com/<user_id>/projects`: projeleriniz. Tüm projeler görürsünüz; diğer kullanıcılar yalnızca ortak projelerinizi bakın.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: proje dosyaları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Belirli projelerin klonları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: belirli bir not defteri veya dosyanın HTML önizlemesi.

## <a name="your-user-id"></a>Kullanıcı Kimliğiniz

Azure not defterlerine ilk kez oturum açtığınızda, hesabınızı otomatik olarak "anon-idr3ca" gibi bir geçici kullanıcı kimliği atanır. İle başlayan bir kullanıcı kimliği olduğu sürece "anon-", Azure not defterleri sizden her oturum değiştirmek için:

![Azure not defterleri açarken bir kullanıcı kimliği oluşturmak için istemleri](media/accounts/create-user-id.png)

**Kullanıcı kimliğini Yapılandır** komutu ayrıca geçici kullanıcı adının yanında görünür:

![Geçici bir kimliği kullanılırken görüntülenen bir kullanıcı kimliği komutu yapılandırın](media/accounts/configure-user-id-command.png)

Ayrıca, kullanıcı Kimliğinizi herhangi bir zamanda profil sayfanızdan değiştirebilirsiniz.

Bir kullanıcı KIMLIĞI, dört ve altı harfli, sayı ve kısa çizgi arasında oluşmalıdır. Başka bir karaktere izin verilir ve kullanıcı kimliği başlamak, kısa çizgi ile bitmelidir veya bir satır birden çok kısa çizgi kullanın. Kullanıcı kimlikleri tüm Azure Notebooks hesaplarında benzersiz olduğundan, "Kullanıcı KIMLIĞI zaten kullanımda" iletisini görebilirsiniz. (Bir Microsoft ticari markayı Kullanıcı KIMLIĞI olarak kullanmaya çalıştığınızda ileti de görüntülenir.) Bu durumlarda, farklı bir kullanıcı KIMLIĞI seçin.

> [!Important]
> Kimliğinizi değiştirerek önceki ID'nizi kullanarak paylaşılan herhangi bir URL geçersiz kılar Bağlantıları düzeltin için geri önceki Kimliğinize Kimliğinizi değiştirebilirsiniz. Ancak, başka bir kullanıcı için kullanılmayan bir talep olası sırada kimliği.

## <a name="your-profile"></a>Profilinizi

Profiliniz, `https://notebooks.azure.com/<user_id>`URL 'sindeki herkese açık olarak görüntülenebilir bilgilerden oluşur. Profil sayfanızı Ayrıca, son kullanılan projeler ve tüm yıldızlı projelerin gösterir.

Profilinizi düzenlemek için profil sayfanızdaki **profil bilgilerini düzenle** komutunu kullanın. Profilinizi bölümleri aşağıdaki gibidir:

| Section | İçindekiler |
| --- | --- |
| Profil fotoğrafı | Profil sayfanızdan gösterildiği bir görüntüsü. |
| Hesap Bilgisi | Görünen ad, kullanıcı kimliği ve ortak bir e-posta hesabı. Buradaki e-posta hesabı, diğer kullanıcılara sizinle iletişim kurmak için bir anlamı sağlar ve Azure Notebooks oturum açmak için kullandığınız [hesaptan](azure-notebooks-user-account.md) farklı olabilir. |
| Profil Bilgileri | Konumunuz, şirket, iş unvanı, web sitesi ve kendiniz kısa bir açıklaması. |
| Sosyal profilleri | GItHub, Twitter ve Facebook kimlikleri, bunları paylaşmak istiyorsanız. |
| Gizlilik Ayarları | İki komutları sağlar:<ul><li>**Profilimi dışarı aktar**: fotoğraflarınızın, profil bilgilerinizin ve güvenlik günlüklerinin dahil Azure Notebooks kaydettiği tüm bilgileri içeren bir *. zip* dosyası oluşturur ve indirir.</li><li>**Hesabımı Sil**: Azure Notebooks depolanan tüm kişisel bilgilerinizi kalıcı olarak siler.</li></ul> |
| Site özelliklerini etkinleştirme | Azure not defterleri davranışı yönleri denetlemenizi sağlar:<ul><li>**Not defterleri Için Birleşik ön uç**: daha hızlı ve daha iyi kalıcılık sağlar.</li><li>**Varsayılan olarak JupyterLab 'Da Çalıştır**: varsayılan olarak, Azure Notebooks çoğu kullanıcı için uygun olan basit bir kullanıcı arabirimi sağlar. JupyterLab deneyimli kullanıcılar için daha zengin ancak daha karmaşık bir arabirim sağlar.</li><li>**VNext Web sitesi**: Bu belgede gösterilen, modernlanmış Web yerleşimini etkin hale sunar.</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
