---
title: Jupyter dizüstü bilgisayarlar oluşturma ve klonlama - Azure Dizüstü Bilgisayarlar Önizleme
description: Azure Not Defterleri Önizleme projeleri, başka bir kaynaktan yeni veya klon oluşturabileceğiniz bir dizüstü bilgisayar koleksiyonu ve ilgili dosyalar yönetir.
ms.topic: how-to
ms.date: 02/25/2019
ms.openlocfilehash: b29ff336c09a3bbf05a57c8a3a503b1875b76e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280579"
---
# <a name="create-and-clone-projects-in-azure-notebooks-preview"></a>Azure Dizüstü Bilgisayarlar Önizleme'de proje oluşturma ve klonlama

Azure Not Defterleri, Jupyter not defterlerinizi ve ilgili dosyalarınızı *projeler*adı verilen mantıksal gruplar halinde düzenler. Önce kapsayıcı olarak bir proje oluşturursunuz, ardından diğer proje dosyalarının yanında bir klasör içinde bir veya daha fazla not defteri oluşturur veya klonlarsınız. (Bu işlem [öğretici](tutorial-create-run-jupyter-notebook.md)de gösterilmiştir.)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Proje ayrıca, özel kurulum adımları ve paket yükleme dahil olmak üzere dizüstü bilgisayarların çalıştırdığı sunucuyu etkileyen meta verileri ve diğer yapılandırma ayarlarını da korur. Daha fazla bilgi için projeleri yönet ve yapılandırma ya da [yapıya bakın.](configure-manage-azure-notebooks-projects.md)

## <a name="use-the-my-projects-dashboard"></a>Projelerim panosunu kullanma

**Projelerim** panosu, `https://notebooks.azure.com/<userID>/projects` projeleri görüntülediğiniz, yönettiğiniz ve oluşturduğunuz yerdir:

[![Azure Not Defterlerinde Projelerim panosu](media/my-projects-dashboard.png)](media/my-projects-dashboard.png#lightbox)

Panoda yapabileceklerin, kullanıcı kimliğine sahip olan hesapta oturum açıp açmadığınıza bağlıdır:

| Komut | Kullanılabilir | Açıklama |
| --- | --- | --- |
| **Çalıştırmak** | Sahip | Proje sunucusunu başlatır ve Jupyter'da proje klasörünü açar. (Daha yaygın olarak, önce bir proje klasörüne gidin, sonra oradan bir not defteri başlatın.) |
| **İndir** | Herkes | Seçilen projenin bir kopyasını ZIP dosyası olarak karşıdan yüklenir. |
| **Paylaşım** | Herkes | Seçili bir projenin URL'sini elde edebileceğiniz, sosyal medyada paylaşabileceğiniz, URL ile e-posta gönderebileceğiniz ve URL ile birlikte "başlatma not defteri" rozeti ile hem HTML veya Markdown kodu alabileceğiniz paylaşım açılır penceresini görüntüler (bkz. başlatma [rozeti edinin).](#obtain-a-launch-badge) |
| **Sil** | Sahip | Seçili projeyi siler. Bu işlem geri alınamaz. |
| **Terminal** | Sahip | Proje sunucusunu başlatır ve ardından bu sunucu için bash terminali ile yeni bir tarayıcı penceresi açar. |
| **+ Yeni Proje** | Sahip | Yeni bir proje oluşturur. Bkz. [Yeni bir proje oluştur.](#create-a-new-project) |
| **GitHub Repo Yükle** | Sahip | GitHub'dan bir proje aktuyor. [GitHub'dan bir proje alma.](#import-a-project-from-github) |
| **Kopyalama** | Herkes | Seçili bir projeyi kendi hesabınıza kopyalar. Zaten oturum açmanızı ister. Bkz. [Bir projeyi Klonla.](#clone-a-project) |

### <a name="obtain-a-launch-badge"></a>Başlatma rozeti edinin

**Paylaş** komutunu kullandığınızda ve **Embed** sekmesini seçtiğinizde, "başlatma not defteri" rozeti oluşturan HTML kodunu veya Markdown'ı kopyalayabilirsiniz:

![Başlatma dizüstü bilgisayar rozeti](https://notebooks.azure.com/launch.png)

Bir Azure Not Defteri projeniz yoksa, uygun kullanıcı adı ve depo adlarını yerine, doğrudan aşağıdaki şablonları kullanarak GitHub'dan klonlayan bir bağlantı oluşturabilirsiniz:

```html
<a href="https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>"><img src="https://notebooks.azure.com/launch.png" /></a>
```

```markdown
[![Azure Notebooks](https://notebooks.azure.com/launch.png)](https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>)
```

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

+ Yeni **Proje** komutunu kullandığınızda, Azure Not Defterleri yeni proje **oluşturma** açılır penceresini görüntüler. Bu açılır pencerede, aşağıdaki bilgileri girin ve sonra **Oluştur'u**seçin:

| Alan | Açıklama |
| --- | --- |
| Proje adı | Azure Notebook'ların görüntüleme amacıyla kullandığı projeniz için kolay bir ad. Örneğin, "Not Defteri Projem". |
| Proje Kimliği | Projeyi paylaşmak için kullandığınız URL'nin bir parçası haline gelen özel `https://notebooks.azure.com/<user_id>/projects/<project_id>`bir tanımlayıcı (form). Bu kimlik yalnızca harfleri, sayıları ve tireleri kullanabilir, 30 karakterle sınırlıdır ve ayrılmış bir [proje kimliği](#reserved-project-ids)olamaz. Ne kullanacağınızdan emin değilseniz, ortak bir kural, boşlukların tirelere dönüştürüldüğü proje adınızın küçük bir sürümünü kullanmaktır (uzunluk sınırına sığması için gerekirse kesilir). |
| Genel | Ayarlanırsa, bağlantıya sahip olan herkesin projeye erişmesine izin verir. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Bu projeyi README ile başlatma | Ayarlanırsa, projede varsayılan *README.md* dosyası oluşturur. *README.md* dosyası, istenirse projeniz için belge sağladığınız dosyadır. |

### <a name="reserved-project-ids"></a>Ayrılmış proje titreleri

Aşağıdaki ayrılmış sözcükler kendileri tarafından proje teşekklü olarak kullanılamaz. Ancak, bu ayrılmış sözcükler daha uzun proje dislerinin bir parçası olarak kullanılabilir.

| | | | | | |
| --- | --- | --- | --- | --- | --- |
| hakkında | account | yönetim | API | blog | sınıf |
| content | pano | Keşfet | SSS | Yardım | html |
| giriş sayfası | içeri aktar | kitaplık | yönetim | new | Dizüstü |
| not defterleri | pdf | Önizleme | Fiyatlandırma | profil | search |
| durum | destek | test | | | |

Bu sözcüklerden birini proje kimliği olarak kullanmaya çalışırsanız, **Yeni Proje Oluştur** ve Proje **Ayarları** açılır pencereleri "Kitaplık kimliği ayrılmış bir tanımlayıcıdır" olarak belirtilir.

Proje kimliği de projenin URL'sinin bir parçası olduğundan, reklam engelleyici yazılımı "reklam" gibi belirli anahtar kelimelerin kullanımını engelleyebilir. Bu gibi durumlarda, proje kimliğinde farklı bir sözcük kullanın.

## <a name="import-a-project-from-github"></a>GitHub'dan proje alma

Herhangi bir veri ve *README.md* dosyalarını içeren bir proje olarak tüm ortak GitHub repo'yu kolayca içe aktarabilirsiniz. Upload **GitHub Repo** komutunu kullanın, açılır pencerede aşağıdaki ayrıntıları sağlayın, ardından **Içe Aktar'ı**seçin:

| Alan | Açıklama |
| --- | --- |
| GitHub deposu | github.com'daki kaynak deposunun adı. Örneğin, Azure Bilişsel Hizmetler [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)için Jupyter not defterlerini klonlamak için "Microsoft/cognitive-services-notebooks" girin.  |
| Yinelemeli olarak klonlama | GitHub depoları birden çok alt depo içerebilir. Üst depoyu ve tüm alt larını klonlamak istiyorsanız bu seçeneği ayarlayın. Bir deponun çok sayıda çocuğu olması mümkün olduğundan, ihtiyacınız olduğunu bilmediğiniz sürece bu seçeneği açık bırakın. |
| Proje adı | Azure Notebook'ların görüntüleme amacıyla kullandığı projeniz için kolay bir ad. |
| Proje Kimliği | Projeyi paylaşmak için kullandığınız URL'nin bir parçası haline gelen özel `https://notebooks.azure.com/<user_id>/projects/<project_id>`bir tanımlayıcı (form). Bu kimlik yalnızca harfleri, sayıları ve tireleri kullanabilir, 30 karakterle sınırlıdır ve ayrılmış bir [proje kimliği](#reserved-project-ids)olamaz. Ne kullanacağınızdan emin değilseniz, ortak bir kural, boşlukların tirelere dönüştürüldüğü proje adınızın küçük bir sürümünü kullanmaktır (uzunluk sınırına sığması için gerekirse kesilir). |
| Genel | Ayarlanırsa, bağlantıya sahip olan herkesin projeye erişmesine izin verir. Özel bir proje oluştururken, bu seçeneği temizleyin. |

GitHub'dan bir depo almak da geçmişini aktarıyor. Terminaldeki standart Git komutlarını kullanarak yeni değişiklikler yapabilir, Değişiklikleri GitHub'dan çekebilirsiniz.

## <a name="clone-a-project"></a>Projeyi klonlama

Klonlama, kendi hesabınızda varolan bir projenin bir kopyasını oluşturur ve projedeki herhangi bir not defterini veya diğer dosyayı çalıştırıp değiştirebilirsiniz. Orijinal projeyi rahatsız etmeden deneme veya başka işler yaptığınız kendi projenizin kopyalarını yapmak için klonlamayı da kullanabilirsiniz.

Bir projeyi klonlamak için:

1. **Projelerim** panosunda, istediğiniz projeyi sağ tıklatın ve **Klon'u** seçin (klavye kısayolu: c).

    ![Proje bağlamı menüsünde klon komutu](media/clone-command.png)

1. Klon **Projesi** açılır penceresinde, klon için bir ad ve kimlik girin ve klonun herkese açık olup olmadığını belirtin. Bu ayarlar yeni bir [proje](#create-a-new-project)için aynıdır.

    ![Klon Projesi açılır](media/clone-project.png)

1. **Klon** düğmesini seçtikten sonra Azure Not Defterleri doğrudan kopyaya doğru ilerler.

## <a name="next-steps"></a>Sonraki adımlar

- [Örnek not defterlerini keşfedin](azure-notebooks-samples.md)
- [Nasıl yapilir: Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapilir: Paketleri dizüstü bilgisayarın içinden yükleme](install-packages-jupyter-notebook.md)
- [Nasıl yapılsın: Slayt gösterisi ni sunma](present-jupyter-notebooks-slideshow.md)
- [Nasıl kullanılır: Veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Nasıl kullanılır: Veri kaynaklarına erişin](access-data-resources-jupyter-notebooks.md)
- [Nasıl kullanılır: Azure Machine Learning'i kullanma](use-machine-learning-services-jupyter-notebooks.md)
