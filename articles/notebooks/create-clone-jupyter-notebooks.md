---
title: Jupyıter not defterlerini oluşturma ve kopyalama-Azure Notebooks önizlemesi
description: Azure Notebooks Preview projeleri, başka bir kaynaktan yeni veya kopya oluşturabileceğiniz bir not defteri ve ilgili dosya koleksiyonunu yönetir.
ms.topic: how-to
ms.date: 02/25/2019
ms.openlocfilehash: b29ff336c09a3bbf05a57c8a3a503b1875b76e54
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78360667"
---
# <a name="create-and-clone-projects-in-azure-notebooks-preview"></a>Azure Notebooks önizlemede proje oluşturma ve kopyalama

Azure Notebooks Jupyıter not defterlerinizi ve ilgili dosyaları *Projeler*adlı mantıksal gruplar halinde düzenler. Bir kapsayıcı olarak ilk proje oluşturma sonra oluşturduğunuzda veya diğer proje dosyalarının yanı sıra bir klasördeki bir veya daha fazla not defterlerini kopyalayın. (Bu işlem [öğreticide](tutorial-create-run-jupyter-notebook.md)gösterilmiştir.)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Bir proje, meta verileri ve özel kurulum adımları ve paket yükleme dahil olmak üzere çalıştırmak, hangi Not sunucuyu etkileyen diğer yapılandırma ayarlarını da korur. Daha fazla bilgi için bkz. [projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md).

## <a name="use-the-my-projects-dashboard"></a>Projelerim panosunu kullanma

`https://notebooks.azure.com/<userID>/projects` konumundaki **Projelerim** panonuz, projeleri görüntülediğiniz, yönettiğiniz ve oluşturduğunuz yerdir:

[Azure Notebooks içindeki projelerim panosunu ![](media/my-projects-dashboard.png)](media/my-projects-dashboard.png#lightbox)

Panoda yapabilecekleriniz kullanıcı kimliği sahip bir hesapla oturum mi oturum açmadıysanız bağlıdır:

| Komut | Kimler kullanabilir? | Açıklama |
| --- | --- | --- |
| **Çalışmaz** | Sahip | Project server başlar ve Jupyter'de proje klasörünü açar. (Daha yaygın olarak, ilk olarak bir proje klasörüne gidin ve sonra bir not defteri oradan başlatın.) |
| **İndir** | Herkes | Seçili projenin bir kopyasını ZIP dosyası olarak indirir. |
| **Paylaşım** | Herkes | Seçilen bir projenin URL 'sini elde edebilir, sosyal medya paylaşabilir, URL ile bir e-posta gönderebilir ve bir "başlatma Not defteri" rozeti (bkz. [bir başlatma rozet alma](#obtain-a-launch-badge)) ile birlikte IÇIN hem HTML hem de markı kodu elde edebilirsiniz. |
| **Silme** | Sahip | Seçili proje siler. Bu işlem geri alınamaz. |
| **Den** | Sahip | Project server başlar ve ardından bu sunucu için terminal bash ile yeni bir tarayıcı penceresi açılır. |
| **+ Yeni proje** | Sahip | Yeni bir proje oluşturur. Bkz. [Yeni proje oluşturma](#create-a-new-project). |
| **GitHub deposunu karşıya yükle** | Sahip | Bir projeyi Github'dan içeri aktarır. [GitHub 'dan bir projeyi Içeri aktarın](#import-a-project-from-github). |
| **Oluşturulacak** | Herkes | Seçilen proje kendi hesabına kopyalar. Oturum imzalamanızı ister. Bkz. [projeyi kopyalama](#clone-a-project). |

### <a name="obtain-a-launch-badge"></a>Başlatma rozet alın

**Share** komutunu kullandığınızda ve **ekleme** sekmesini seçtiğinizde, "Not defteri başlatma" rozetini oluşturan HTML kodunu veya Marku 'yi kopyalayabilirsiniz:

![Not Defteri rozet başlatın](https://notebooks.azure.com/launch.png)

Bir Azure not defterleri projesi yoksa, uygun kullanıcı adı ve depo adları değiştirerek aşağıdaki şablonlarını kullanarak doğrudan Github'dan klonlar bir bağlantı oluşturabilirsiniz:

```html
<a href="https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>"><img src="https://notebooks.azure.com/launch.png" /></a>
```

```markdown
[![Azure Notebooks](https://notebooks.azure.com/launch.png)](https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>)
```

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

**+ Yeni proje** komutunu kullandığınızda Azure Notebooks **Yeni bir proje oluştur** açılır penceresi görüntüler. Bu açılan pencerede, aşağıdaki bilgileri girin ve ardından **Oluştur**' u seçin:

| Alan | Açıklama |
| --- | --- |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. Örneğin, "Not Defterim projem". |
| Proje Kimliği | Bir projeyi paylaşmak için kullandığınız URL 'nin bir parçası haline gelen özel bir tanımlayıcı (form `https://notebooks.azure.com/<user_id>/projects/<project_id>`). Bu KIMLIK yalnızca harf, rakam ve kısa çizgi kullanabilir, 30 karakterle sınırlıdır ve [ayrılmış bir proje kimliği](#reserved-project-ids)olamaz. Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Bu Benioku projeyle Başlat | Ayarlanırsa, projede varsayılan bir *README.MD* dosyası oluşturur. İsterseniz, projeniz için belgeler sağladığınız bir *README.MD* dosyası. |

### <a name="reserved-project-ids"></a>Ayrılmış proje kimlikleri

Aşağıdaki ayrılmış sözcükler kendileri proje kimliği olarak kullanılamaz. Bununla birlikte, bu ayrılmış sözcükler, daha uzun proje kimliklerinin bir parçası olarak kullanılabilir.

| | | | | | |
| --- | --- | --- | --- | --- | --- |
| hakkında | account | yönetim | API | lenemeyen | ders |
| content | pano | Explorer | SSS | yardım | 'si |
| ana sayfa | aktarmaya | Kitaplığı | yönetim | Yeni | Mini |
| not defterleri | pdf | önizleme | Fiyat | profile | search |
| status | destek | test | | | |

Bu sözcüklerden birini proje KIMLIĞI olarak kullanmaya çalışırsanız, **Yeni proje oluştur** ve **proje ayarları** açılır pencere sayısı "kitaplık kimliği ayrılmış bir tanımlayıcıdır."

Proje KIMLIĞI aynı zamanda bir projenin URL 'sinin parçası olduğundan, ad engelleyici yazılım "duyurusu" gibi belirli anahtar sözcüklerin kullanımını engelleyebilir. Böyle durumlarda, proje KIMLIĞINDE farklı bir kelime kullanın.

## <a name="import-a-project-from-github"></a>Bir projeyi Github'dan alma

Tüm genel GitHub deposunu, herhangi bir veri ve *README.MD* dosyası dahil olmak üzere proje olarak kolayca içeri aktarabilirsiniz. **GitHub depoyu karşıya yükle** komutunu kullanın, açılan pencerede aşağıdaki ayrıntıları sağlayın ve **içeri aktar**' ı seçin:

| Alan | Açıklama |
| --- | --- |
| GitHub deposu | Github.com kaynak depoda adı. Örneğin, Azure bilişsel hizmetler için Jupyıter not defterlerini [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)kopyalamak için, "Microsoft/bilişsel hizmetler-not defterleri" yazın.  |
| Yinelemeli olarak kopyalama | GitHub depoları, birden çok alt deposu içerebilir. Üst deponun ve tüm alt öğelerini kopyalamak istiyorsanız bu seçeneği ayarlayın. Çok sayıda alt öğeleri bir depo için mümkün olduğu için bu seçeneği temizleyin ihtiyaç duymadıkça bırakın. |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. |
| Proje Kimliği | Bir projeyi paylaşmak için kullandığınız URL 'nin bir parçası haline gelen özel bir tanımlayıcı (form `https://notebooks.azure.com/<user_id>/projects/<project_id>`). Bu KIMLIK yalnızca harf, rakam ve kısa çizgi kullanabilir, 30 karakterle sınırlıdır ve [ayrılmış bir proje kimliği](#reserved-project-ids)olamaz. Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |

Bir depo Github'dan alma geçmişini alır. Yeni değişiklikleri, Github'dan değişiklikleri çekme ve benzeri terminalden standart Git komutlarını kullanabilirsiniz.

## <a name="clone-a-project"></a>Bir projesini kopyalama

Kopyalama, ardından çalıştırın ve herhangi bir not defteri veya diğer proje dosyasında değişiklik kendi hesabınız mevcut bir projeyi bir kopyasını oluşturur. Hangi deneyleri veya diğer iş özgün proje bozmadan bunu kendi projeleriniz kopyalarını için kopyalama de kullanabilirsiniz.

Bir proje kopyalamak için:

1. **Projelerim** panosunda, istenen projeye sağ tıklayın ve **Kopyala** ' yı (klavye kısayolu: c) seçin.

    ![Proje bağlam menüsünde kopyalama komutu](media/clone-command.png)

1. **Projeyi Kopyala** açılır penceresinde, kopya için bir ad ve kimlik girin ve kopyanın genel olup olmadığını belirtin. Bu ayarlar, [Yeni bir proje](#create-a-new-project)için ile aynıdır.

    ![Kopya proje açılan menüsü](media/clone-project.png)

1. **Kopyala düğmesini seçtikten** sonra Azure Notebooks doğrudan kopyaya gider.

## <a name="next-steps"></a>Sonraki adımlar

- [Örnek not defterlerini keşfet](azure-notebooks-samples.md)
- [Nasıl yapılır: projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: bir not defteri içinden paket yüklemesi](install-packages-jupyter-notebook.md)
- [Nasıl yapılır: bir slayt gösterisi sunma](present-jupyter-notebooks-slideshow.md)
- [Nasıl yapılır: veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Nasıl yapılır: veri kaynaklarına erişme](access-data-resources-jupyter-notebooks.md)
- [Nasıl yapılır: Azure Machine Learning kullanma](use-machine-learning-services-jupyter-notebooks.md)
