---
title: Azure Notebooks Preview ile kullanım için Kullanıcı profili ve KIMLIĞI
description: Azure Notebooks ile Kullanıcı profilinizi ve Kullanıcı KIMLIĞINIZI oluşturma ve yönetme, paylaşılan not defterlerinin URL 'sinin bir parçası haline geliyor.
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: d90eebf1b7b463e038bc5e54f51df0eb6ca746c4
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646288"
---
# <a name="your-profile-and-user-id-for-azure-notebooks-preview"></a>Azure Notebooks önizlemesi için profiliniz ve Kullanıcı KIMLIĞINIZ

Azure not defterleri, güçlü, işbirliğine dayalı alanı içinde kullanıcı profilinizin genel görüntünüzü başkalarına sunar:

[Bir Azure Notebooks profili sayfası ![](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Kullanıcı Kimliğinizi, projeleri ve Not Defterleri paylaşmak için kullandığınız URL'leri bir parçasıdır. Aşağıdaki listede, farklı bir URL desenleri açıklar:

- `https://notebooks.azure.com/<user_id>`: Profil sayfanızı.
- `https://notebooks.azure.com/<user_id>/projects`: Uygulamanızın proje. Tüm projeler görürsünüz; diğer kullanıcılar yalnızca ortak projelerinizi bakın.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Proje dosyaları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Belirli proje kopyalar.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Belirli bir not defteri veya dosya HTML önizlemesi.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="your-user-id"></a>Kullanıcı Kimliğiniz

Azure not defterlerine ilk kez oturum açtığınızda, hesabınızı otomatik olarak "anon-idr3ca" gibi bir geçici kullanıcı kimliği atanır. İle başlayan bir kullanıcı kimliği olduğu sürece "anon-", Azure not defterleri sizden her oturum değiştirmek için:

![Azure not defterleri açarken bir kullanıcı kimliği oluşturmak için istemleri](media/accounts/create-user-id.png)

A **yapılandırma kullanıcı kimliği** komutu, geçici kullanıcı adının yanında da görünür:

![Geçici bir kimliği kullanılırken görüntülenen bir kullanıcı kimliği komutu yapılandırın](media/accounts/configure-user-id-command.png)

Ayrıca, kullanıcı Kimliğinizi herhangi bir zamanda profil sayfanızdan değiştirebilirsiniz.

Bir kullanıcı KIMLIĞI, dört ve altı harfli, sayı ve kısa çizgi arasında oluşmalıdır. Başka bir karaktere izin verilir ve kullanıcı kimliği başlamak, kısa çizgi ile bitmelidir veya bir satır birden çok kısa çizgi kullanın. Kullanıcı kimlikleri tüm Azure Notebooks hesaplarında benzersiz olduğundan, "Kullanıcı KIMLIĞI zaten kullanımda" iletisini görebilirsiniz. (Bir Microsoft ticari markayı Kullanıcı KIMLIĞI olarak kullanmaya çalıştığınızda ileti de görüntülenir.) Bu durumlarda, farklı bir kullanıcı KIMLIĞI seçin.

> [!Important]
> Kimliğinizi değiştirerek önceki ID'nizi kullanarak paylaşılan herhangi bir URL geçersiz kılar Bağlantıları düzeltin için geri önceki Kimliğinize Kimliğinizi değiştirebilirsiniz. Ancak, başka bir kullanıcı için kullanılmayan bir talep olası sırada kimliği.

## <a name="your-profile"></a>Profilinizi

Profilinizi, URL'de herkes tarafından izlenen bilgi oluşan `https://notebooks.azure.com/<user_id>`. Profil sayfanızı Ayrıca, son kullanılan projeler ve tüm yıldızlı projelerin gösterir.

Profilinizi düzenlemek için **profil bilgilerini Düzenle** Profil sayfanızdaki komutu. Profilinizi bölümleri aşağıdaki gibidir:

| Section | İçindekiler |
| --- | --- |
| Profil fotoğrafı | Profil sayfanızdan gösterildiği bir görüntüsü. |
| Hesap Bilgisi | Görünen ad, kullanıcı kimliği ve ortak bir e-posta hesabı. E-posta hesabı buraya farklı olabilir ve diğer kullanıcıların sizinle iletişim kurmak için bir ortalama sağlar [hesabı](azure-notebooks-user-account.md) Azure not defterlerine kendisini imzalamak için kullanın. |
| Profil Bilgileri | Konumunuz, şirket, iş unvanı, web sitesi ve kendiniz kısa bir açıklaması. |
| Sosyal profilleri | GItHub, Twitter ve Facebook kimlikleri, bunları paylaşmak istiyorsanız. |
| Gizlilik Ayarları | İki komutları sağlar:<ul><li>**Profilim dışarı**: oluşturur ve indirir bir *.zip* Azure not defterleri kaydeder, fotoğraf, profil bilgileri ve güvenlik günlükleri gibi profilinizdeki tüm bilgileri içeren dosya.</li><li>**Hesabım Sil**: Azure not defterlerinde depolanan tüm kişisel bilgilerinizi kalıcı olarak siler.</li></ul> |
| Site özelliklerini etkinleştirme | Azure not defterleri davranışı yönleri denetlemenizi sağlar:<ul><li>**Birleşik not defterleri için ön uç**: daha hızlı not defteri başlatma ve daha iyi kalıcılığını etkinleştirir.</li><li>**JupyterLab içinde varsayılan olarak çalıştığı**: varsayılan olarak, Azure not defterleri çoğu kullanıcı için uygun olan basit bir kullanıcı arabirimi sağlar. JupyterLab deneyimli kullanıcılar için daha zengin ancak daha karmaşık bir arabirim sağlar.</li><li>**VNext Web sitesi**: Bu belgede gösterilen Modernleştirilmiş web düzeni sağlar.</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
