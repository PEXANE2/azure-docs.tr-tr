---
title: Azure Dizüstü Bilgisayar önizlemesini yapılandırma ve yönetme
description: Hem Azure Notebook UI'si hem de doğrudan terminal erişimi nde proje meta verilerini, proje dosyalarını, projenin ortamını ve kurulum adımlarını nasıl yöneteceğimize öğrenin.
ms.topic: how-to
ms.date: 02/28/2020
ms.openlocfilehash: 1674effda2cb9bda45f49c91ca618225b0a75f0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280605"
---
# <a name="manage-and-configure-projects-in-azure-notebooks-preview"></a><a id="manage-and-configure-projects" />Azure Dizüstü Bilgisayarlar Önizleme'de projeleri yönetme ve yapılandırma

Azure Not Defterleri Önizleme'deki bir proje, aslında Jupyter dizüstü bilgisayarların bir dosya klasörü ve açıklayıcı meta verilerle birlikte çalıştırdığı temel Linux sanal makinesinin bir yapılandırmasıdır. 

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Not Defterleri'ndeki proje panosu, dosyaları yönetmenize ve projenin özelliklerini yapılandırmanıza olanak tanır:

- Projenin çalıştığı ve ücretsiz katman veya Azure sanal makinesi olabilecek bilgi işlem katmanı.
- Projeyi paylaşırken kullanılan bir ad, açıklama, tanımlayıcı ve projenin ortak mı yoksa özel mi olduğunu içeren proje meta verileri.
- Projenin not defteri, veri ve diğer dosyalar gibi yönettiğiniz diğer dosyalar.
- Başlangıç komut dosyaları aracılığıyla veya doğrudan terminal aracılığıyla yönettiğiniz bir projenin ortamı.
- Terminalden erişebildiğiniz günlükler.

> [!Note]
> Burada açıklanan yönetim ve yapılandırma özellikleri yalnızca projeyi başlangıçta oluşturan proje sahibi tarafından kullanılabilir. Ancak, projeyi kendi hesabınıza kopyalayabilirsiniz, bu durumda sahibi olursunuz ve projeyi istediğiniz gibi yapılandırabilirsiniz.

Azure Not Defterleri, bir not defteri veya başka bir dosya çalıştırdığınızda temel deki sanal makineyi başlatır. Sunucu dosyaları otomatik olarak kaydeder ve 60 dakikalık bir etkinlik ten sonra kapanır. Kapatma **komutu** (klavye kısayolu: h) ile sunucuyu istediğiniz zaman durdurabilirsiniz.

## <a name="compute-tier"></a>İşlem katmanı

Varsayılan olarak, projeler kötüye kullanımı önlemek için 4 GB bellek ve 1 GB veriyle sınırlı **olan Ücretsiz Bilgi İşlem** katmanında çalışır. Azure aboneliğinde sizin için sağlanan farklı bir sanal makineyi kullanarak bu sınırlamaları atlayabilir ve bilgi işlem gücünü artırabilirsiniz. Daha fazla bilgi için [Data Science Virtual Machines nasıl kullanılır.](use-data-science-virtual-machine.md)

## <a name="edit-project-metadata"></a>Proje meta verilerini düzenlemesi

Proje panosunda **Proje Ayarları'nı**seçin ve ardından aşağıdaki tabloda açıklandığı gibi projenin meta verilerini içeren **Bilgiler** sekmesini seçin. Proje meta verilerini istediğiniz zaman değiştirebilirsiniz.

| Ayar | Açıklama |
| --- | --- |
| Proje adı | Azure Notebook'ların görüntüleme amacıyla kullandığı projeniz için kolay bir ad. Örneğin, "Python'da Merhaba Dünya". |
| Proje Kimliği | Projeyi paylaşmak için kullandığınız URL'nin bir parçası haline gelen özel bir tanımlayıcı. Bu kimlik yalnızca harfleri, sayıları ve tireleri kullanabilir, 30 karakterle sınırlıdır ve ayrılmış bir [proje kimliği](create-clone-jupyter-notebooks.md#reserved-project-ids)olamaz. Ne kullanacağınızdan emin değilseniz, ortak bir kural, boşlukların tirelere dönüştürüldüğü proje adınızın küçük bir sürümünü kullanmaktır (uzunluk sınırına sığması için gerekirse kesilir). |
| Kamu projesi | Ayarlanırsa, bağlantıya sahip olan herkesin projeye erişmesine izin verir. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Klonları gizle | Ayarlanırsa, diğer kullanıcılar bu proje için yapılmış klonların listesini göremiyor. Klonları gizlemek, aynı kuruluşun parçası olmayan birçok kişiyle paylaşılan projeler için (örneğin, bir sınıfa ders verebilmek için not defteri kullanmak gibi) yararlıdır. |

> [!Important]
>
> Proje kimliğini değiştirmek, projeyle daha önce paylaşmış olabileceğiniz bağlantıları geçersiz kılmaktadır.

## <a name="manage-project-files"></a>Proje dosyalarını yönetme

Proje panosu, projenin klasör sisteminin içeriğini gösterir. Bu dosyaları yönetmek için çeşitli komutları kullanabilirsiniz.

### <a name="create-new-files-and-folders"></a>Yeni dosya ve klasörler oluşturma

**+ Yeni** komut (klavye kısayolu: n) yeni dosya veya klasörler oluşturur. Komutu kullanırken, ilk olarak oluşturmak için öğe türünü seçin:

| Madde türü | Açıklama | Komut davranışı |
| --- | --- | --- |
| **Not Defteri** | Bir Jupyter dizüstü bilgisayar | Not defterinin dosya adını ve dilini belirttiğiniz bir açılır pencere görüntüler. |
| **Klasör** | Bir alt klasör | Projenin dosya listesinde klasör adını girdiğiniz bir edit alanı oluşturur. |
| **Boş Dosya** | Metin, veri, vb. gibi içerikleri depolayabildiğiniz bir dosya. | Projenin dosya listesinde dosya adını girdiğiniz bir edit alanı oluşturur. |
| **Markdown** | Markdown dosyası. | Projenin dosya listesinde dosya adını girdiğiniz bir edit alanı oluşturur. |

### <a name="upload-files"></a>Dosyaları karşıya yükleme

**Yükle** komutu diğer konumlardan veri almak için iki seçenek sağlar: **URL'den** ve **Bilgisayardan.** Daha fazla bilgi için Azure [Notebook projelerinde veri dosyalarıyla Çalışma'ya](work-with-project-data-files.md)bakın.

### <a name="select-file-specific-commands"></a>Dosyaya özgü komutları seçin

Projenin dosya listesindeki her öğe, sağ tıklatma bağlam menüsünden komutlar sağlar:

![Dosya bağlamı menüsündeki komutlar](media/project-file-commands.png)

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Çalıştırın | r (veya tıklayın) | Bir not defteri dosyası çalıştırın. Diğer dosya türleri görüntülemek için açılır.  |
| Bağlantıyı Kopyala | y | Dosyanın panosuna giden bağlantıyı kopyalar. |
| Jupyter Lab çalıştırın | J | Jupyter normalde sağladığından daha geliştirici odaklı bir arayüz olan JupyterLab'da bir dizüstü bilgisayar çalıştırır. |
| Önizleme | p | Dosyanın HTML önizlemesini açar; not defterleri için önizleme, not defterinin salt okunur şekilde işlenmesidir. Daha fazla bilgi için [Önizleme](#preview) bölümüne bakın. |
| Dosyayı edit | ı | Düzenleme için dosyayı açar. |
| İndirme | d | Dosyayı veya bir klasörün içeriğini içeren bir zip dosyası indirir. |
| Yeniden Adlandır | a | Dosya veya klasör için yeni bir ad ister. |
| Sil | x | Onay ister, sonra dosyayı projeden kalıcı olarak kaldırır. Silmeler geri alınamaz. |
| Taşı | m | Bir dosyayı aynı projede farklı bir klasöre taşır. |

#### <a name="preview"></a>Önizleme

Bir dosyanın veya not defterinin önizlemesi, içeriğin salt okunur görünümüdür; çalışan not defteri hücreleri devre dışı bırakılır. Önizleme, dosya veya not defterine bağlantısı olan ancak Azure Not Defteri'nde oturum açmamış herkese gösterilir. Oturum açKen, kullanıcı not defterini kendi hesabına kopyalayabilir veya not defterini yerel bilgisayarına indirebilir.

Önizleme sayfası klavye kısayolları ile çeşitli araç çubuğu komutlarını destekler:

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Paylaş | s | Bağlantı edinebileceğiniz, sosyal medyada paylaşabileceğiniz, katıştırma için HTML elde edebileceğiniz ve e-posta gönderebileceğiniz paylaşım açılır penceresini görüntüler. |
| Kopyalama | c  | Not defterini hesabınıza kopyala. |
| Çalıştırın | r | Bunu yapmanıza izin verilirse not defterini çalıştırın. |
| İndirme | d | Not defterinin bir kopyasını indirir. |

## <a name="configure-the-project-environment"></a>Proje ortamını yapılandırma

Dizüstü defterlerinizin çalıştırıldığı altta yatan sanal makinenin ortamını yapılandırmanın üç yolu vardır:

- Tek seferlik başlatma komut dosyası ekleme
- Kurulum adımlarını belirtmek için projenin ortam ayarlarını kullanma
- Bir terminal üzerinden sanal makineerişin.

Sanal makine başlatıldığında tüm proje yapılandırma biçimleri uygulanır ve böylece proje içindeki tüm defterleri etkiler.

### <a name="one-time-initialization-script"></a>Tek seferlik başlatma komut dosyası

İlk kez Azure Not Defteri proje için bir sunucu oluşturur, *projede aznbsetup.sh*adlı bir dosya arar. Bu dosya varsa, Azure Not Defterleri çalıştırın. Komut dosyasının çıktısı proje klasörünüzde *.aznbsetup.log*olarak depolanır.

### <a name="environment-setup-steps"></a>Ortam kurulum adımları

Ortamı yapılandıran tek tek adımlar oluşturmak için projenin ortam ayarlarını kullanabilirsiniz.

Proje panosunda **Proje Ayarları'nı**seçin ve ardından proje için kurulum adımlarını eklediğiniz, kaldırdığınız ve değiştirdiğiniz **Çevre** sekmesini seçin:

![Seçili Çevre sekmesiyle proje ayarları açılır](media/project-settings-environment-steps.png)

Adım eklemek için önce **+ Ekle'yi**seçin, ardından **İşlem** açılır listesinde bir adım türü seçin:

![Yeni bir Ortam kurulum adımı için çalışma seçici](media/project-settings-environment-details.png)

Daha sonra yansıttığınız bilgiler seçtiğiniz işlem türüne bağlıdır:

- **Requirements.txt**: İkinci açılan listede, projede zaten bulunan *bir requirements.txt* dosyasını seçin. Ardından, görünen üçüncü açılır listeden bir Python sürümü seçin. Azure Notebook, *bir requirements.txt* dosyası `pip install -r` kullanarak, bir dizüstü bilgisayar sunucusu başlatırken *requirements.txt* dosyasıyla çalışır. Not defterinin içinden paketleri açıkça yüklemeniz gerekmez.

- **Kabuk komut dosyası**: İkinci açılır listede, projede bir bash kabuk komutdosyası (genellikle *.sh* uzantılı bir dosya) seçin ve ortamı niçin çalıştırmak istediğiniz komutları içerir.

- **Environment.yml**: İkinci açılır listede, conda ortamı nı kullanan Python projeleri için bir *ortam.yml* dosyası seçin.

   > [!WARNING]
   > Bu, geliştirilmekte olan bir önizleme hizmeti olduğundan, `Environment.yml` şu anda ayarın beklendiği gibi projenize uygulanmadığı bilinen bir sorun vardır. Proje ve içindeki Jupyter dizüstü bilgisayarlar şu anda belirtilen ortam dosyasını yüklemez.

Adım eklemeyi bitirdiğinizde **Kaydet'i**seçin.

### <a name="use-the-terminal"></a>Terminali kullanma

Proje panosunda Terminal **komutu,** sunucuya doğrudan erişim sağlayan bir Linux terminali açar. Terminal içinde veri indirebilir, dosyaları dedebilir veya yönetebilir, süreçleri inceleyebilir ve hatta vi ve nano gibi araçları kullanabilirsiniz.

> [!Note]
> Projenizin ortamında başlangıç komut dosyalarınız varsa, terminali açmak kurulumun hala devam ettiğini belirten bir ileti görüntüleyebilir.

Terminalde herhangi bir standart Linux komutu yayınlayabilirsiniz. `ls` Ayrıca, projeyi içeren bir *proje* klasörüyle birlikte *sanal*makinede anaconda2_501, *anaconda3_420,* *anaconda3_501,* *IfSharp*ve *R*gibi sanal makinede bulunan farklı ortamları görmek için ev klasöründe de kullanabilirsiniz:

![Azure Dizüstü Bilgisayarlar'da proje terminali](media/project-terminal.png)

Belirli bir ortamı etkilemek için dizinleri önce o ortam klasöründe değiştirin.

Python ortamları için, `pip` her `conda` ortamın *çöp kutusu* klasörünü bulabilirsiniz. Ortamlar için yerleşik takma adlar da kullanabilirsiniz:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Sunucuda yapılan değişiklikler, *proje* klasöründe oluşturduğunuz dosya ve klasörler dışında yalnızca geçerli oturumiçin geçerli dir. Örneğin, proje klasöründe bir dosyayı düzenleme oturumları arasında `pip install` kalıcıdır, ancak olan paketler bunlar değildir.

> [!Note]
> Python'un `python` `python3`not defterleri için kullanılmayan sistem yüklü sürümlerini kullanır veya çağırırsınız. Benzer işlemler `pip install` için izinleriniz de yoktur, bu nedenle sürüme özgü takma adları kullandığınızdan emin olun.

## <a name="access-notebook-logs"></a>Not defteri günlüklerine erişin

Not defteri çalıştırırken sorunlarla karşılanırsanız, Jupyter'dan gelen çıktı *.nb.log*adlı klasörde depolanır. Bu günlüklere **Terminal** komutu veya proje panosundan erişebilirsiniz.

Genellikle Jupyter'ı yerel olarak çalıştırırken bir terminal penceresinden başlatmış olabilirsiniz. Terminal penceresi çekirdek durumu gibi çıktıları gösterir.

Günlükleri görüntülemek için terminalde aşağıdaki komutu kullanın:

```bash
cat .nb.log
```

Python not defterindeki bir kod hücresinden gelen komutu da kullanabilirsiniz:

```bash
!cat .nb.log
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl kullanılır: Proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Not defterinde bulut verilerine erişme](access-data-resources-jupyter-notebooks.md)
