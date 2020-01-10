---
title: Azure Notebooks önizlemeyi yapılandırın ve yönetin
description: Azure Notebooks Kullanıcı arabirimi ve doğrudan Terminal erişimi aracılığıyla proje meta verilerini, proje dosyalarını, projenin ortamını ve kurulum adımlarını yönetmeyi öğrenin.
ms.topic: how-to
ms.date: 05/13/2019
ms.openlocfilehash: 5c97372133315e6f0bcd3b854793b6b4746b5ba5
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646271"
---
# <a name="a-idmanage-and-configure-projects--manage-and-configure-projects-in-azure-notebooks-preview"></a>Azure Notebooks önizlemede projeleri yönetmek ve yapılandırmak <a id="manage-and-configure-projects" />

Azure Notebooks önizlemede bulunan bir proje, temel olarak Jupyter Not defterlerinin çalıştığı ve bir dosya klasörüyle birlikte açıklayıcı meta verilerle birlikte temel alınan Linux sanal makinesinin bir yapılandırmadır. 

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Proje Panosu Azure not defterlerinde dosyaları yönetmek ve aksi takdirde proje özelliklerini yapılandırmanıza olanak sağlar:

- Projenin çalıştırıldığı işlem katmanı, ücretsiz katman veya bir Azure sanal makinesi olabilir.
- Bir ad, açıklama, projeyi paylaşırken kullanılan tanımlayıcıyı ve projenin public veya Private olduğunu içeren proje meta verileri.
- Projenin Not defteri, verileri ve diğer dosya sistemleri gibi yönettiğiniz diğer dosyalar.
- Başlangıç betikleri ya da doğrudan Terminal aracılığıyla yönettiğiniz bir projenin ortamı.
- Terminal üzerinden erişebileceğiniz Günlükler.

> [!Note]
> Burada açıklanan yönetim ve yapılandırma özellikleri yalnızca projeyi başlangıçta oluşturan Proje sahibi tarafından kullanılabilir. Ancak, projeyi kendi hesabınıza kopyalayabilir, bu durumda sahip olursunuz ve projeyi istediğiniz şekilde yapılandırabilirsiniz.

Azure not defterleri, her bir not defteri veya başka bir dosyaya çalıştırdığınızda temel sanal makine başlar. Sunucu otomatik olarak dosyaları kaydeder ve 60 dakika işlem yapılmadığında kapanır. Sunucu ile dilediğiniz zaman durdurabilirsiniz **kapatma** komut (klavye kısayolu: h).

## <a name="compute-tier"></a>İşlem Katmanı

Varsayılan olarak, projeler **ücretsiz işlem** katmanında çalışır, bu da kötüye kullanımı engellemek için 4 GB bellek ve 1 GB veri ile sınırlıdır. Bu sınırlamaları atlayabilir ve bir Azure aboneliğinde sağladığınız farklı bir sanal makineyi kullanarak işlem gücünü artırabilirsiniz. Daha fazla bilgi için bkz. [veri bilimi sanal makinelerini kullanma](use-data-science-virtual-machine.md).

## <a name="edit-project-metadata"></a>Proje meta verilerini düzenleme

Proje Panosu üzerinde seçin **proje ayarları**, ardından **bilgi** sekmesinde, aşağıdaki tabloda açıklandığı gibi proje meta verileri içerir. Proje meta verileri dilediğiniz zaman değiştirebilirsiniz.

| Ayar | Açıklama |
| --- | --- |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. Örneğin, "Hello World içinde Python". |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL parçası haline gelir özel tanımlayıcısı. Bu KIMLIK yalnızca harf, rakam ve kısa çizgi kullanabilir, 30 karakterle sınırlıdır ve [ayrılmış bir proje kimliği](create-clone-jupyter-notebooks.md#reserved-project-ids)olamaz. Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel proje | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Klonları Gizle | Ayarlanırsa, kullanıcılar bu proje için yapılmış kopyalar listesini göremiyorsanız. Klonları gizleme aynı kuruluştaki bir parçası gibi olmayan birçok kişilerle paylaşılan projeler için kullanışlı bir not defteri kullanırken bir sınıf ders için. |

> [!Important]
>
> Proje kimliği değiştiriliyor herhangi bir bağlantı, daha önce paylaştığınız proje geçersiz kılar.

## <a name="manage-project-files"></a>Proje dosyalarını yönetme

Proje Panosu proje klasörü sistem içeriğini gösterir. Bu dosyaları yönetmek için çeşitli komutlara kullanabilirsiniz.

### <a name="create-new-files-and-folders"></a>Yeni dosyalar ve klasörler oluşturma

**+ Yeni** komut (klavye kısayolu: n) yeni dosya veya klasörleri oluşturur. İlk komutunu kullanırken, oluşturulacak öğesi türünü seçin:

| Öğe türü | Açıklama | Komut davranışı |
| --- | --- | --- |
| **Not Defteri** | Jupyter not defteri | Not defterinin dosya adı ve dil belirtin açılır pencere görüntüler. |
| **Klasör** | Bir alt klasör | Klasör adı girin, projenin dosya listesinde bir düzenleme alanı oluşturur. |
| **Boş dosya** | Bir dosya metin, veri, vb. gibi herhangi bir içeriği saklayabilirsiniz. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |
| **Markdown** | Bir Markdown dosyası. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |

### <a name="upload-files"></a>Dosyaları karşıya yükleme

**Karşıya** komut diğer konumlardan veri aktarmaya yönelik iki seçenek sağlar: **URL'den** ve **bilgisayarı**. Daha fazla bilgi için [veri dosyalarını Azure not defteri projelerde çalışabilirsiniz](work-with-project-data-files.md).

### <a name="select-file-specific-commands"></a>Dosya özel komutları seçin

Projenin dosya listesindeki her öğeyi sağ bağlam menüsü komutları sağlar:

![Bir dosya bağlam menüsü komutları](media/project-file-commands.png)

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Çalıştırın | r (veya tıklayın) | Bir not defteri dosyası çalıştırır. Diğer dosya türleri görüntüleme için açılır.  |
| Bağlantıyı Kopyala | Y | Panoya bir dosyaya bir bağlantı kopyalar. |
| Jupyter laboratuar ortamında çalıştırma | j | Bir not defteri olan Jupyter normalde sağladığından bir geliştirici odaklı daha arabirimi JupyterLab içinde çalıştırır. |
| Önizleme | p | Dosyanın bir HTML önizlemesini açılır; Not defterleri için not defterinin salt okunur bir işleme bir önizlemedir. Daha fazla bilgi için [Önizleme](#preview) bölümü. |
| Dosyasını düzenleyin | Ben | Dosya düzenleme için açar. |
| İndirin | d | Dosya veya klasör içeriğini içeren bir zip dosyası indirir. |
| Yeniden adlandır | a | Dosya veya klasör için yeni bir ad sorar. |
| Sil | x | Onay ister ve ardından dosyayı projeden kalıcı olarak kaldırır. Silme işlemleri geri alınamaz. |
| Taşı | m | Bir dosyayı, aynı projede farklı bir klasöre taşır. |

#### <a name="preview"></a>Önizleme

Bir dosya veya dizüstü bilgisayar İçindekiler salt okunur görünüm önizlemesidir; Not Defteri hücreleri çalıştıran devre dışı bırakıldı. Not Defteri ve dosya için bir bağlantı vardır, ancak Azure not defterleri açmadı herkes tarafından önizlemesi gösterilmektedir. Bir kullanıcı oturum açtıktan sonra kendi hesabı not defterini kopyalayabilirsiniz veya not defteri kendi yerel bilgisayarlarına yükleyebilirsiniz.

Önizleme sayfası klavye kısayollarıyla birkaç araç çubuğu komutlarını destekler:

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Paylaşın | s | İçinden bağlantısını almak, sosyal medyada paylaşın, HTML eklemek için alabilir ve bir e-posta paylaşım açılan pencere görüntüler. |
| Kopyala | c  | Not defterini hesabınıza kopyalayın. |
| Çalıştırın | r | Bunu yapmak izin verilmez, Not defterini çalıştırılır. |
| İndirin | d | Not defterini bir kopyasını yükler. |

## <a name="configure-the-project-environment"></a>Proje ortamını yapılandırma

Not defterlerinizi içinde çalıştığı temel sanal makine ortamı yapılandırmak için üç yolu vardır:

- Bir kez başlatma komut dosyası Ekle
- Kurulum adımları belirtmek için projenin ortam ayarlarını kullan
- Sanal makine, bir terminal erişin.

Sanal makine başlatılır ve bu nedenle proje içindeki tüm not defterlerinin etkiler her proje yapılandırması biçimlerinin uygulanır.

### <a name="one-time-initialization-script"></a>Tek seferlik başlatma betiği

İlk kez Azure Notebooks proje için bir sunucu oluşturduğunda, projede *aznbsetup.sh*adlı bir dosya arar. Bu dosya varsa, Azure Notebooks çalıştırır. Betik çıktısı proje klasörünüzde depolanır *. aznbsetup.log*.

### <a name="environment-setup-steps"></a>Ortam Kurulumu adımları

Ortam yapılandırma her bir adımı oluşturmak için projenin ortam ayarlarını kullanabilirsiniz.

Proje Panosu üzerinde seçin **proje ayarları**, ardından **ortam** Burada, ekleme, kaldırma ve kurulum adımlarını proje için değiştirmek sekmesinde:

![Ortam sekmesi seçili proje ayarları açılır](media/project-settings-environment-steps.png)

Bir adım eklemek için önce seçin **+ Ekle**, bir adım türü seçip **işlemi** aşağı açılan listesi:

![Yeni ortam Kurulum adımı için işlem Seçici](media/project-settings-environment-details.png)

Ardından Proje bilgi işlemi, seçtiğiniz türüne bağlıdır:

- **Requirements.txt**: İkinci aşağı açılan listesinde seçin bir *requirements.txt* projede zaten var olan dosyayı. Ardından görüntülenen üçüncü aşağı açılan listeden bir Python sürümünü seçin. Kullanarak bir *requirements.txt* dosya, Azure not defterleri çalıştırmaları `pip install -r` ile *requirements.txt* bir not defteri sunucusu başlatma sırasında dosya. Açıkça içinde notebook paketleri yüklemeniz gerekmez.

- **Kabuk betiği**: projedeki bir bash Kabuk betiği ikinci aşağı açılan listeden seçin (genellikle bir dosyayla *.sh* uzantısı) ortamı başlatmak için çalıştırmak istediğiniz herhangi bir komut içerir.

- **Environment.yml**: İkinci aşağı açılan listesinde seçin bir *environments.yml* conda ortamı kullanarak Python projeleri için dosya.

Adımları eklemeyi tamamladığınızda, seçin **Kaydet**.

### <a name="use-the-terminal"></a>Terminal kullanma

Proje Panosu üzerinde **Terminal** komut sunucuya doğrudan erişim sağlayan bir Linux terminal açar. Terminal içinde verileri indirmek, düzenleyebilir veya dosyalarını yönetme, işlemleri incelemek ve hatta VI ve nano gibi araçları kullanın.

> [!Note]
> Projenizin ortamında başlangıç betikleriniz varsa, terminal açma Kurulum'un devam ediyor belirten bir ileti görüntüleyebilir.

Terminalde tüm standart Linux komutlarını gönderebilir. Ayrıca `ls` gibi sanal makinede mevcut farklı ortamları görmek için giriş klasörü içinde *anaconda2_501*, *anaconda3_420*, *anaconda3_501*, *IfSharp*, ve *R*, ile birlikte bir *proje* projeyi içeren klasörü:

![Proje terminalde Azure Not Defterleri](media/project-terminal.png)

Belirli bir ortama etkilemek için bu ortam klasöre dizinleri önce değiştirin.

Python ortamları için bulabileceğinizi `pip` ve `conda` içinde *bin* her bir ortamın klasör. Yerleşik diğer adlar ortamları için de kullanabilirsiniz:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Sunucuya yapılan değişiklikleri uygulamak yalnızca dosya ve klasörleri oluşturduğunuz dışında geçerli oturum için *proje* kendisini klasör. Örneğin, proje klasöründeki bir dosyayı düzenleme oturumları arasında kalıcıdır ancak ile paketleri `pip install` değil.

> [!Note]
> Kullanırsanız `python` veya `python3`, not defterleri için kullanılmaz Python, sistem tarafından yüklenen sürümleri çağırır. İster işlemleri için izniniz yok `pip install` ya da, bu nedenle sürüme özgü diğer adlar kullandığınızdan emin olun.

## <a name="access-notebook-logs"></a>Not Defteri günlüklerine erişme

Bir not defteri çalışırken sorun yaşarsanız, jupyter tarafından sağlanan çıkış adlı bir klasörde depolanır *. nb.log*. Bu günlükler aracılığıyla erişebileceğiniz **Terminal** komut veya proje Panosu.

Jupyter yerel olarak çalıştırırken genellikle, bu bir terminal penceresinden başlatılmamış olabilir. Terminal penceresinde, çekirdek durumu gibi bir çıktı gösterir.

Günlükleri görüntülemek için terminalde aşağıdaki komutu kullanın:

```bash
cat .nb.log
```

Ayrıca, bir kod hücresine bir Python not defteri gelen komutunu da kullanabilirsiniz:

```bash
!cat .nb.log
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: Proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Bir not defteri bulut verilerine erişim](access-data-resources-jupyter-notebooks.md)
