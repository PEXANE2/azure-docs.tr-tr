---
title: Azure Notebook Önizleme ile kullanım için kullanıcı profili ve kimliği
description: Paylaşılan not defterlerinin URL'sinin bir parçası haline gelen Azure Notebook'larla kullanıcı profilinizi ve kullanıcı kimliğinizi oluşturma ve yönetme.
ms.topic: conceptual
ms.date: 02/25/2019
ms.openlocfilehash: d90eebf1b7b463e038bc5e54f51df0eb6ca746c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646288"
---
# <a name="your-profile-and-user-id-for-azure-notebooks-preview"></a>Azure Dizüstü Bilgisayarlar Önizleme için profiliniz ve kullanıcı kimliğiniz

Azure Not Defterleri'nin güçlü ve işbirlikçi alanı içinde, kullanıcı profiliniz herkese açık imajınızı başkalarına sunar:

[![Azure Not Defterleri profil sayfası](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Kullanıcı kimliğiniz, projeleri ve not defterlerini paylaşmak için kullandığınız URL'lerin bir parçasıdır. Aşağıdaki liste farklı URL desenleri açıklar:

- `https://notebooks.azure.com/<user_id>`: Profil sayfanız.
- `https://notebooks.azure.com/<user_id>/projects`: Projeleriniz. Tüm projeleri görüyorsunuz; diğer kullanıcılar yalnızca ortak projelerinizi görür.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Proje dosyaları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Belirli bir projenin klonları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Belirli bir not defterinin veya dosyanın HTML önizlemesi.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="your-user-id"></a>Kullanıcı Kimliğiniz

Azure Not Defterleri'nde ilk kez oturum açtığınızda, hesabınıza otomatik olarak "anon-idr3ca" gibi geçici bir kullanıcı kimliği atanır. "Anon-" ile başlayan bir kullanıcı kimliğiniz olduğu sürece, Azure Dizüstü Bilgisayarlar oturum açtığınızda bu kimliği değiştirmenizi ister:

![Azure Not Defterleri'nde oturum açarken kullanıcı kimliği oluşturma komut istemi](media/accounts/create-user-id.png)

Geçici kullanıcı adının yanında **bir Yapıkullanıcı Kimliği** komutu da görünür:

![Geçici bir kimlik kullanırken görünen Kullanıcı Kimliği komutunu yapılandırın](media/accounts/configure-user-id-command.png)

Ayrıca, profil sayfanızda istediğiniz zaman kullanıcı kimliğinizi değiştirebilirsiniz.

Kullanıcı kimliği dört ila on altı harf, sayı ve tire arasında olmalıdır. Başka karaktere izin verilmez ve kullanıcı kimliği tire ile başlayamaz veya sona eremez veya satırda birden çok tire kullanamaz. Kullanıcı kimlikleri tüm Azure Not Defterleri hesaplarında benzersiz olduğundan, "Kullanıcı Kimliği zaten kullanılıyor" iletisini görebilirsiniz. (Bir Microsoft ticari markasını kullanıcı kimliği olarak kullanmaya çalışırsanız ileti de görüntülenir.) Bu gibi durumlarda, farklı bir kullanıcı kimliği seçin.

> [!Important]
> Kimliğinizi değiştirmek, önceki kimliğinizi kullanarak paylaştığınız tüm URL'leri geçersiz kılmaktadır. Bağlantıları yeniden geçersiz saymak için kimliğinizi önceki kimliğinize geri değiştirebilirsiniz. Ancak, bu arada başka bir kullanıcının kullanılmayan bir kimlik talep etmesi mümkündür.

## <a name="your-profile"></a>Profiliniz

Profiliniz URL'de genel olarak görüntülenebilir `https://notebooks.azure.com/<user_id>`bilgilerden oluşur. Profil sayfanız ayrıca en son kullandığınız projeleri ve yıldızlı projeleri de gösterir.

Profilinizi yeniden yapmak için profil sayfanızdaki **Profil Bilgilerini Edit** komutunu kullanın. Profilinizin bölümleri aşağıdaki gibidir:

| Section | İçindekiler |
| --- | --- |
| Profil fotoğrafı | Profil sayfanızda gösterilen bir resim. |
| Hesap Bilgileri | Görüntülü adınız, kullanıcı kimliğiniz ve herkese açık e-posta hesabınız. Buradaki e-posta hesabı, diğer kullanıcılara sizinle iletişim kurma ortalaması sağlar ve Azure Not Defterleri'nde oturum açmanız için kullandığınız [hesaptan](azure-notebooks-user-account.md) farklı olabilir. |
| Profil Bilgileri | Konumunuz, şirketiniz, iş unvanınız, web siteniz ve kısa bir açıklamanız. |
| Sosyal Profiller | GItHub, Twitter ve Facebook teşekküllerini paylaşmak isterseniz. |
| Gizlilik Ayarları | İki komut sağlar:<ul><li>**Profilimi Dışa Aktar**: Fotoğraf, profil bilgileriniz ve güvenlik günlükleriniz de dahil olmak üzere Azure Notebook'ların profilinizde kaydettiği tüm bilgileri içeren bir *.zip* dosyası oluşturur ve indirir.</li><li>**Hesabımı Sil**: Azure Not Defterlerinde depolanan tüm kişisel bilgilerinizi kalıcı olarak siler.</li></ul> |
| Site Özelliklerini Etkinleştir | Azure Not Defterlerinin davranışlarının yönlerini denetlemenize olanak tanır:<ul><li>**Dizüstü Bilgisayarlar için Birleşik Frontend**: daha hızlı dizüstü bilgisayar başlatma ve daha iyi kalıcılık sağlar.</li><li>**Varsayılan olarak JupyterLab'da çalıştırın**: Azure Notebook'lar varsayılan olarak çoğu kullanıcı için uygun basit bir kullanıcı arabirimi sağlar. JupyterLab deneyimli kullanıcılar için daha zengin ama daha karmaşık bir arayüz sağlar.</li><li>**VNext Web Sitesi**: bu dokümantasyonda gösterilen modernleştirilmiş web düzenini sağlar.</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter dizüstü çalıştırmak oluşturmak](tutorial-create-run-jupyter-notebook.md)
