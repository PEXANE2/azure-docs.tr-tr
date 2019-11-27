---
title: Yapılandırma ve Azure not defteri projeleri yönetme
description: Proje meta veri, proje dosyaları, projenin ortam ve kurulum adımları Azure not defterleri UI ve terminal doğrudan erişim aracılığıyla nasıl yönetileceği.
ms.topic: article
ms.date: 05/13/2019
ms.openlocfilehash: 56c265122894412e79b3d5a7b256964c49ab81a6
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277637"
---
# <a name="manage-and-configure-projects"></a>Projeleri yönetme ve yapılandırma

Bir Azure not defterlerinde temelde bir Jupyter not defterleri, dosya klasörü ve açıklayıcı meta verileri ile birlikte çalıştığı temel Linux sanal makine yapılandırmasını projesidir. Proje Panosu Azure not defterlerinde dosyaları yönetmek ve aksi takdirde proje özelliklerini yapılandırmanıza olanak sağlar:

- Projenin çalıştırıldığı işlem katmanı, ücretsiz katman veya bir Azure sanal makinesi olabilir.
- Bir ad, açıklama, projeyi paylaşırken kullanılan tanımlayıcıyı ve projenin public veya Private olduğunu içeren proje meta verileri.
- Projenin Not defteri, verileri ve diğer dosya sistemleri gibi yönettiğiniz diğer dosyalar.
- Başlangıç betikleri ya da doğrudan Terminal aracılığıyla yönettiğiniz bir projenin ortamı.
- Terminal üzerinden erişebileceğiniz Günlükler.

> [!Note]
> Burada açıklanan yönetim ve yapılandırma özellikleri yalnızca projeyi başlangıçta oluşturan Proje sahibi tarafından kullanılabilir. Ancak, projeyi kendi hesabınıza kopyalayabilir, bu durumda sahip olursunuz ve projeyi istediğiniz şekilde yapılandırabilirsiniz.

Azure not defterleri, her bir not defteri veya başka bir dosyaya çalıştırdığınızda temel sanal makine başlar. Sunucu otomatik olarak dosyaları kaydeder ve 60 dakika işlem yapılmadığında kapanır. Ayrıca, sunucuyu dilediğiniz zaman **kapatın** komutunu (klavye kısayolu: h) kullanabilirsiniz.

## <a name="compute-tier"></a>İşlem Katmanı

Varsayılan olarak, projeler **ücretsiz işlem** katmanında çalışır, bu da kötüye kullanımı engellemek için 4 GB bellek ve 1 GB veri ile sınırlıdır. Bu sınırlamaları atlayabilir ve bir Azure aboneliğinde sağladığınız farklı bir sanal makineyi kullanarak işlem gücünü artırabilirsiniz. Daha fazla bilgi için bkz. [veri bilimi sanal makinelerini kullanma](use-data-science-virtual-machine.md).

## <a name="edit-project-metadata"></a>Proje meta verilerini düzenleme

Proje panosunda **proje ayarları**' nı seçin ve ardından aşağıdaki tabloda açıklandığı gibi projenin meta verilerini içeren **bilgi** sekmesini seçin. Proje meta verileri dilediğiniz zaman değiştirebilirsiniz.

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

**+ New** komutu (klavye kısayolu: n) yeni dosya veya klasör oluşturur. İlk komutunu kullanırken, oluşturulacak öğesi türünü seçin:

| Öğe türü | Açıklama | Komut davranışı |
| --- | --- | --- |
| **Mini** | Jupyter not defteri | Not defterinin dosya adı ve dil belirtin açılır pencere görüntüler. |
| **Klasör** | Bir alt klasör | Klasör adı girin, projenin dosya listesinde bir düzenleme alanı oluşturur. |
| **Boş dosya** | Bir dosya metin, veri, vb. gibi herhangi bir içeriği saklayabilirsiniz. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |
| **MARKDOWN** | Bir Markdown dosyası. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |

### <a name="upload-files"></a>Dosyaları karşıya yükleme

**Karşıya yükle** komutu, diğer konumlardan veri içeri aktarmak için iki seçenek sunar: **URL 'den** ve **bilgisayardan**. Daha fazla bilgi için bkz. [Azure Not defteri projelerinde veri dosyalarıyla çalışma](work-with-project-data-files.md).

### <a name="select-file-specific-commands"></a>Dosya özel komutları seçin

Projenin dosya listesindeki her öğeyi sağ bağlam menüsü komutları sağlar:

![Bir dosya bağlam menüsü komutları](media/project-file-commands.png)

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Çalıştırın | r (veya tıklayın) | Bir not defteri dosyası çalıştırır. Diğer dosya türleri görüntüleme için açılır.  |
| Bağlantıyı Kopyala | Y | Panoya bir dosyaya bir bağlantı kopyalar. |
| Jupyter laboratuar ortamında çalıştırma | j | Bir not defteri olan Jupyter normalde sağladığından bir geliştirici odaklı daha arabirimi JupyterLab içinde çalıştırır. |
| Önizleme | p | Dosyanın bir HTML önizlemesini açılır; Not defterleri için not defterinin salt okunur bir işleme bir önizlemedir. Daha fazla bilgi için [Önizleme](#preview) bölümüne bakın. |
| Dosyasını düzenleyin | Ben | Dosya düzenleme için açar. |
| İndir | g | Dosya veya klasör içeriğini içeren bir zip dosyası indirir. |
| Yeniden Adlandır | a | Dosya veya klasör için yeni bir ad sorar. |
| Sil | x | Onay ister ve ardından dosyayı projeden kalıcı olarak kaldırır. Silme işlemleri geri alınamaz. |
| Taşı | m | Bir dosyayı, aynı projede farklı bir klasöre taşır. |

#### <a name="preview"></a>Önizleme

Bir dosya veya dizüstü bilgisayar İçindekiler salt okunur görünüm önizlemesidir; Not Defteri hücreleri çalıştıran devre dışı bırakıldı. Not Defteri ve dosya için bir bağlantı vardır, ancak Azure not defterleri açmadı herkes tarafından önizlemesi gösterilmektedir. Bir kullanıcı oturum açtıktan sonra kendi hesabı not defterini kopyalayabilirsiniz veya not defteri kendi yerel bilgisayarlarına yükleyebilirsiniz.

Önizleme sayfası klavye kısayollarıyla birkaç araç çubuğu komutlarını destekler:

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Paylaş | s | İçinden bağlantısını almak, sosyal medyada paylaşın, HTML eklemek için alabilir ve bir e-posta paylaşım açılan pencere görüntüler. |
| Kopyalama | c  | Not defterini hesabınıza kopyalayın. |
| Çalıştırın | r | Bunu yapmak izin verilmez, Not defterini çalıştırılır. |
| İndir | g | Not defterini bir kopyasını yükler. |

## <a name="configure-the-project-environment"></a>Proje ortamını yapılandırma

Not defterlerinizi içinde çalıştığı temel sanal makine ortamı yapılandırmak için üç yolu vardır:

- Bir kez başlatma komut dosyası Ekle
- Kurulum adımları belirtmek için projenin ortam ayarlarını kullan
- Sanal makine, bir terminal erişin.

Sanal makine başlatılır ve bu nedenle proje içindeki tüm not defterlerinin etkiler her proje yapılandırması biçimlerinin uygulanır.

### <a name="one-time-initialization-script"></a>Tek seferlik başlatma betiği

İlk kez Azure Notebooks proje için bir sunucu oluşturduğunda, projede *aznbsetup.sh*adlı bir dosya arar. Bu dosya varsa, Azure Notebooks çalıştırır. Betiğin çıkışı proje klasörünüzde *. aznbsetup. log*olarak depolanır.

### <a name="environment-setup-steps"></a>Ortam Kurulumu adımları

Ortam yapılandırma her bir adımı oluşturmak için projenin ortam ayarlarını kullanabilirsiniz.

Proje panosunda proje **ayarları**' nı seçin ve ardından proje için kurulum adımlarını eklemek, kaldırmak ve değiştirmek istediğiniz **ortam** sekmesini seçin:

![Ortam sekmesi seçili proje ayarları açılır](media/project-settings-environment-steps.png)

Bir adım eklemek için, önce **+ Ekle**' yi seçin ve ardından **işlem** açılır listesinden bir adım türü seçin:

![Yeni ortam Kurulum adımı için işlem Seçici](media/project-settings-environment-details.png)

Ardından Proje bilgi işlemi, seçtiğiniz türüne bağlıdır:

- **Requirements. txt**: ikinci açılan listede, projede zaten olan bir *requirements. txt* dosyası seçin. Ardından görüntülenen üçüncü aşağı açılan listeden bir Python sürümünü seçin. Bir bir not defteri sunucusunu başlatırken, *requirements. txt* dosyasını kullanarak, *requirements. txt* dosyası ile `pip install -r` Azure Notebooks çalışır. Açıkça içinde notebook paketleri yüklemeniz gerekmez.

- **Kabuk betiği**: ikinci aşağı açılan listede, ortamı başlatmak için çalıştırmak istediğiniz komutları içeren, projede bir bash Shell betiği (genellikle *. sh* uzantılı bir dosya) seçin.

- **Environment. yml**: ikinci açılan listede, bir Conda ortamı kullanarak Python projeleri için bir *ortamlar. yml* dosyası seçin.

Adım ekleme işiniz bittiğinde **Kaydet**' i seçin.

### <a name="use-the-terminal"></a>Terminal kullanma

Proje panosunda, **Terminal** komutu sunucuya doğrudan erişim sağlayan bir Linux terminali açar. Terminal içinde verileri indirmek, düzenleyebilir veya dosyalarını yönetme, işlemleri incelemek ve hatta VI ve nano gibi araçları kullanın.

> [!Note]
> Projenizin ortamında başlangıç betikleriniz varsa, terminal açma Kurulum'un devam ediyor belirten bir ileti görüntüleyebilir.

Terminalde tüm standart Linux komutlarını gönderebilir. Ayrıca, bir sanal makinede bulunan *anaconda2_501*, *anaconda3_420*, *Anaconda3_501*, *ifsharp*ve *R*gibi farklı ortamları, projeyi içeren bir *Proje* klasörüyle birlikte görmek için giriş klasöründeki `ls` de kullanabilirsiniz:

![Proje terminalde Azure Not Defterleri](media/project-terminal.png)

Belirli bir ortama etkilemek için bu ortam klasöre dizinleri önce değiştirin.

Python ortamları için, her bir ortamın *bin* klasöründe `pip` ve `conda` bulabilirsiniz. Yerleşik diğer adlar ortamları için de kullanabilirsiniz:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Sunucuda yapılan değişiklikler, *Proje* klasöründe oluşturduğunuz dosyalar ve klasörler hariç yalnızca geçerli oturum için geçerlidir. Örneğin, proje klasöründeki bir dosyayı düzenlediğinizde oturumlar arasında kalıcı olur, ancak `pip install` olan paketler değildir.

> [!Note]
> `python` veya `python3`kullanıyorsanız, Python 'un, Not defterleri için kullanılmayan, sistemde yüklü olan sürümlerini çağırabilirsiniz. `pip install` gibi işlemler için izinleriniz yok, bu nedenle sürüme özgü diğer adları kullandığınızdan emin olun.

## <a name="access-notebook-logs"></a>Not Defteri günlüklerine erişme

Bir not defteri çalıştırırken sorunlarla karşılaşırsanız, Jupyter 'dan alınan çıkış *. NB. log*adlı bir klasörde depolanır. Bu günlüklere, **Terminal** komutu veya proje panosu aracılığıyla erişebilirsiniz.

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

- [Nasıl yapılır: proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Bir not defterinde bulut verilerine erişme](access-data-resources-jupyter-notebooks.md)
