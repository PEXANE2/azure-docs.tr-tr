---
title: Azure Notebooks önizlemeyi yapılandırın ve yönetin
description: Azure Notebooks Kullanıcı arabirimi ve doğrudan Terminal erişimi aracılığıyla proje meta verilerini, proje dosyalarını, projenin ortamını ve kurulum adımlarını yönetmeyi öğrenin.
ms.topic: how-to
ms.date: 02/28/2020
ms.custom: tracking-python
ms.openlocfilehash: df64c9d90252c31118b66943b6a182319e3f1cc2
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84554297"
---
# <a name="manage-and-configure-projects-in-azure-notebooks-preview"></a><a id="manage-and-configure-projects" />Azure Notebooks önizlemede projeleri yönetme ve yapılandırma

Azure Notebooks önizlemede bulunan bir proje, temel olarak Jupyter Not defterlerinin çalıştığı ve bir dosya klasörüyle birlikte açıklayıcı meta verilerle birlikte temel alınan Linux sanal makinesinin bir yapılandırmadır. 

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Notebooks içindeki proje panosu, dosyaları yönetmenizi ve proje özelliklerini yapılandırmanızı sağlar:

- Projenin çalıştırıldığı işlem katmanı, ücretsiz katman veya bir Azure sanal makinesi olabilir.
- Bir ad, açıklama, projeyi paylaşırken kullanılan tanımlayıcıyı ve projenin public veya Private olduğunu içeren proje meta verileri.
- Projenin Not defteri, verileri ve diğer dosya sistemleri gibi yönettiğiniz diğer dosyalar.
- Başlangıç betikleri ya da doğrudan Terminal aracılığıyla yönettiğiniz bir projenin ortamı.
- Terminal üzerinden erişebileceğiniz Günlükler.

> [!Note]
> Burada açıklanan yönetim ve yapılandırma özellikleri yalnızca projeyi başlangıçta oluşturan Proje sahibi tarafından kullanılabilir. Ancak, projeyi kendi hesabınıza kopyalayabilir, bu durumda sahip olursunuz ve projeyi istediğiniz şekilde yapılandırabilirsiniz.

Azure Notebooks, her bir not defteri veya başka bir dosya çalıştırdığınızda temel alınan sanal makineyi başlatır. Sunucu dosyaları otomatik olarak kaydeder ve 60 dakika etkinlik dışı kaldıktan sonra kapatır. Ayrıca, sunucuyu dilediğiniz zaman **kapatın** komutunu (klavye kısayolu: h) kullanabilirsiniz.

## <a name="compute-tier"></a>İşlem Katmanı

Varsayılan olarak, projeler **ücretsiz işlem** katmanında çalışır, bu da kötüye kullanımı engellemek için 4 GB bellek ve 1 GB veri ile sınırlıdır. Bu sınırlamaları atlayabilir ve bir Azure aboneliğinde sağladığınız farklı bir sanal makineyi kullanarak işlem gücünü artırabilirsiniz. Daha fazla bilgi için bkz. [veri bilimi sanal makinelerini kullanma](use-data-science-virtual-machine.md).

## <a name="edit-project-metadata"></a>Proje meta verilerini Düzenle

Proje panosunda **proje ayarları**' nı seçin ve ardından aşağıdaki tabloda açıklandığı gibi projenin meta verilerini içeren **bilgi** sekmesini seçin. Proje meta verilerini dilediğiniz zaman değiştirebilirsiniz.

| Ayar | Description |
| --- | --- |
| Proje adı | Projeniz için Azure Notebooks görüntüleme amacıyla kullanılan kolay bir ad. Örneğin, "Python 'da Merhaba Dünya". |
| Proje Kimliği | Projeyi paylaşmak için kullandığınız URL 'nin bir parçası haline gelen özel bir tanımlayıcı. Bu KIMLIK yalnızca harf, rakam ve kısa çizgi kullanabilir, 30 karakterle sınırlıdır ve [ayrılmış bir proje kimliği](create-clone-jupyter-notebooks.md#reserved-project-ids)olamaz. Ne kullanacağınızdan emin değilseniz, yaygın olarak kullanılan bir kural, "My-Not-Project" (uzunluk sınırına uyması gerekirse kesildi) gibi boşlukların tire içine açık olduğu, proje adınızın küçük harfli bir sürümünü kullanmaktır. |
| Ortak proje | Ayarlanırsa, bağlantıya sahip olan kişilerin projeye erişmesine izin verir. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Klonları gizle | Ayarlanırsa, diğer kullanıcılar bu proje için yapılmış olan klonların bir listesini göremez. Klonların gizlenmesi, bir sınıfı öğretme için bir not defteri kullanırken olduğu gibi, aynı kuruluşun parçası olmayan birçok kişiyle paylaşılan projeler için yararlıdır. |

> [!Important]
>
> Proje KIMLIĞI değiştirildiğinde, daha önce paylaştığınız projenin tüm bağlantıları geçersiz hale gelebilir.

## <a name="manage-project-files"></a>Proje dosyalarını Yönet

Proje panosu, projenin klasör sisteminin içeriğini gösterir. Bu dosyaları yönetmek için çeşitli komutları kullanabilirsiniz.

### <a name="create-new-files-and-folders"></a>Yeni dosya ve klasörler oluşturma

**+ New** komutu (klavye kısayolu: n) yeni dosya veya klasör oluşturur. Komutunu kullanırken öncelikle oluşturulacak öğe türünü seçin:

| Öğe türü | Description | Komut davranışı |
| --- | --- | --- |
| **Not Defteri** | Jupyter Not defteri | Not defterinin dosya adını ve dilini belirttiğiniz bir açılan pencere görüntüler. |
| **Klasör** | Bir alt klasör | Projenin dosya listesinde klasör adını girdiğiniz bir düzenleme alanı oluşturur. |
| **Boş dosya** | İçine metin, veri vb. gibi içerik depolayabilen bir dosya. | Projenin dosya listesinde dosya adını girdiğiniz bir düzenleme alanı oluşturur. |
| **Markdown** | Bir Markaşağı dosyası. | Projenin dosya listesinde dosya adını girdiğiniz bir düzenleme alanı oluşturur. |

### <a name="upload-files"></a>Dosyaları karşıya yükleme

**Karşıya yükle** komutu, diğer konumlardan veri içeri aktarmak için iki seçenek sunar: **URL 'den** ve **bilgisayardan**. Daha fazla bilgi için bkz. [Azure Not defteri projelerinde veri dosyalarıyla çalışma](work-with-project-data-files.md).

### <a name="select-file-specific-commands"></a>Dosyaya özgü komutları seçin

Projenin dosya listesindeki her öğe, sağ tıklama kısayol menüsü aracılığıyla komutlar sağlar:

![Dosya bağlam menüsündeki komutlar](media/project-file-commands.png)

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Çalıştır | r (veya öğesine tıklayın) | Bir not defteri dosyası çalıştırır. Diğer dosya türleri görüntülenmek üzere açılır.  |
| Bağlantıyı Kopyala | y | Dosyaya bir bağlantıyı panoya kopyalar. |
| Jupyıter laboratuvarında Çalıştır | uygulanmaz | JupyterLab içinde, Jupyter 'ın normal olarak sağladığı daha fazla geliştirici odaklı bir arabirim olan bir not defteri çalıştırır. |
| Önizleme | p | Dosyanın HTML önizlemesini açar; Not defterleri için Önizleme, Not defterini salt okunurdur. Daha fazla bilgi için [Önizleme](#preview) bölümüne bakın. |
| Dosyayı Düzenle | ı | Dosyayı düzenlenmek üzere açar. |
| İndir | d | Dosya veya klasörün içeriğini içeren bir ZIP dosyasını indirir. |
| Rename | a | Dosya veya klasör için yeni bir ad ister. |
| Sil | x | Onay ister, sonra dosyayı projeden kalıcı olarak kaldırır. Silme işlemleri geri alınamaz. |
| Taşı | m | Bir dosyayı aynı projede farklı bir klasöre taşıın. |

#### <a name="preview"></a>Önizleme

Bir dosyanın veya Not defterinin önizlemesi, içeriklerin salt okuma görünümüdür; Not defteri hücrelerinin çalıştırılması devre dışı bırakıldı. Önizleme, dosya veya Not defteri bağlantısı olan ancak Azure Notebooks oturum açmamış herkese gösterilir. Oturum açıldıktan sonra, Kullanıcı Not defterini kendi hesaplarına kopyalayabilir veya Not defterini yerel bilgisayarlarına indirebilir.

Önizleme sayfası klavye kısayollarıyla çeşitli araç çubuğu komutlarını destekler:

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Paylaş | s | Bir bağlantı alabileceğiniz, sosyal medya paylaşma, katıştırma için HTML alma ve e-posta gönderme için bir paylaşım açılan penceresini görüntüler. |
| Kopyalama | c  | Not defterini hesabınıza kopyalayın. |
| Çalıştır | r | İzin verildiyse, Not defterini çalıştırır. |
| İndir | d | Not defterinin bir kopyasını indirir. |

## <a name="configure-the-project-environment"></a>Proje ortamını yapılandırma

Not defterlerinizin çalıştığı temeldeki sanal makinenin ortamını yapılandırmanın üç yolu vardır:

- Tek seferlik başlatma betiği Ekle
- Kurulum adımlarını belirtmek için projenin ortam ayarlarını kullanın
- Sanal makineye bir terminal üzerinden erişin.

Tüm proje yapılandırması formları, sanal makine her başlatıldığında uygulanır ve bu nedenle proje içindeki tüm not defterlerini etkiler.

### <a name="one-time-initialization-script"></a>Bir kerelik başlatma betiği

İlk kez Azure Notebooks proje için bir sunucu oluşturduğunda, projede *aznbsetup.sh*adlı bir dosya arar. Bu dosya varsa, Azure Notebooks çalıştırır. Betiğin çıkışı proje klasörünüzde *. aznbsetup. log*olarak depolanır.

### <a name="environment-setup-steps"></a>Ortam kurulum adımları

Ortamı yapılandıran bireysel adımları oluşturmak için projenin ortam ayarlarını kullanabilirsiniz.

Proje panosunda proje **ayarları**' nı seçin ve ardından proje için kurulum adımlarını eklemek, kaldırmak ve değiştirmek istediğiniz **ortam** sekmesini seçin:

![Ortam sekmesi seçiliyken proje ayarları açılır penceresi](media/project-settings-environment-steps.png)

Bir adım eklemek için, önce **+ Ekle**' yi seçin ve ardından **işlem** açılır listesinden bir adım türü seçin:

![Yeni bir ortam için işlem Seçici Kurulum adımı](media/project-settings-environment-details.png)

Bundan sonra proje, seçtiğiniz işlem türüne bağlıdır:

- **Requirements. txt**: ikinci açılan listede, projede zaten olan bir *requirements. txt* dosyası seçin. Ardından görüntülenen üçüncü açılan listeden bir Python sürümü seçin. Bir bir not defteri sunucusunu başlatırken Azure Notebooks, *requirements. txt* dosyası kullanarak, `pip install -r` *requirements. txt* dosyasıyla çalışır. Paketleri Not defterinin içinden açıkça yüklemeniz gerekmez.

- **Kabuk betiği**: ikinci aşağı açılan listede, ortamı başlatmak için çalıştırmak istediğiniz komutları içeren, projede bir bash Shell betiği (genellikle *. sh* uzantılı bir dosya) seçin.

- **Environment. yml**: ikinci açılan listede, bir Conda ortamı kullanarak Python projeleri için bir *ortamlar. yml* dosyası seçin.

   > [!WARNING]
   > Bu bir önizleme hizmeti geliştirildiği `Environment.yml` için, ayarın projenize beklendiği şekilde uygulanamadığı bilinen bir sorun vardır. İçindeki proje ve jupi Not defterleri, belirtilen ortam dosyasını yok olarak yüklemez.

Adım ekleme işiniz bittiğinde **Kaydet**' i seçin.

### <a name="use-the-terminal"></a>Terminali kullanma

Proje panosunda, **Terminal** komutu sunucuya doğrudan erişim sağlayan bir Linux terminali açar. Terminal içinde verileri indirebilir, dosyaları düzenleyebilir veya yönetebilir, süreçler inceleyebilir ve hatta vi ve nano gibi araçları kullanabilirsiniz.

> [!Note]
> Projenizin ortamında başlangıç betikleriniz varsa, terminali açmak kurulumun hala devam ettiğini belirten bir ileti görüntüleyebilir.

Terminalde herhangi bir standart Linux komutu verebilirsiniz. Ayrıca, `ls` bir sanal makinede bulunan *anaconda2_501*, *anaconda3_420*, *anaconda3_501*, *ifsharp*ve *R*gibi farklı ortamları, projeyi içeren bir *Proje* klasörüyle birlikte görmek için giriş klasöründe de kullanabilirsiniz:

![Azure Notebooks 'de Proje terminali](media/project-terminal.png)

Belirli bir ortamı etkilemek için, önce dizinleri bu ortam klasörüyle değiştirin.

Python ortamları için, `pip` `conda` her bir ortamın *bin* klasörünü bulabilir ve değiştirebilirsiniz. Ortamlar için yerleşik diğer adları da kullanabilirsiniz:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Sunucuda yapılan değişiklikler, *Proje* klasöründe oluşturduğunuz dosyalar ve klasörler hariç yalnızca geçerli oturum için geçerlidir. Örneğin, proje klasöründeki bir dosyayı düzenlediğinizde oturumlar arasında kalıcı olur, ancak ile paketler `pip install` değildir.

> [!Note]
> `python`Veya kullanıyorsanız `python3` , Python 'un sistem tarafından yüklenen sürümlerini Not defterleri için kullanılmayan, çağırır. Her ikisi gibi işlemler için izinleriniz yok `pip install` , bu nedenle sürüme özgü diğer adları kullandığınızdan emin olun.

## <a name="access-notebook-logs"></a>Not defteri günlüklerine erişin

Bir not defteri çalıştırırken sorunlarla karşılaşırsanız, Jupyter 'dan alınan çıkış *. NB. log*adlı bir klasörde depolanır. Bu günlüklere, **Terminal** komutu veya proje panosu aracılığıyla erişebilirsiniz.

Genellikle, yerel olarak jupi çalıştırdığınızda bir terminal penceresinden başlatılmış olabilirsiniz. Terminal penceresi, çekirdek durumu gibi çıktıyı gösterir.

Günlükleri görüntülemek için terminalde aşağıdaki komutu kullanın:

```bash
cat .nb.log
```

Ayrıca, bir Python not defterindeki kod hücresinden komutunu da kullanabilirsiniz:

```bash
!cat .nb.log
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Not defterinde bulut verilerine erişme](access-data-resources-jupyter-notebooks.md)
