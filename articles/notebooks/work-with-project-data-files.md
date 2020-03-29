---
title: Azure Notebook Önizleme'ye sahip projelerle veri alma ve verme
description: Bir Azure Not Defteri Önizleme projesine dış kaynaklardan nasıl veri getireceğinizi ve projeden nasıl veri dışa aktarabileceğinizi öğrenin.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: e1d4a52ab7f4ad2ca3438af4bc87bec0b79f34d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646985"
---
# <a name="work-with-data-files-in-azure-notebooks-preview-projects"></a>Azure Notebook Önizleme projelerinde veri dosyalarıyla çalışma

Veriler birçok Jupyter dizüstü bilgisayarın, özellikle de veri bilimi için kullanılan dizüstü bilgisayarların can damarıdır. Azure Not Defterleri ile, çeşitli kaynaklardan bir projeye kolayca aktarabilir ve bu verileri not defterlerinden kullanabilirsiniz. Not defterlerinin projede depolanan verileri üretmesini ve başka yerlerde kullanmak üzere indirebetmesini de sağlayabilirsiniz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Çalışan bir not defterinin içindeki **Veri** menüsü, projedeki dosyaların yanı sıra geçerli not defteri oturumu için geçici dosyalarla çalışan **Yükleme** ve **İndir** komutları da sağlar.

Ayrıca, proje içindeki dosyalar da dahil olmak üzere çeşitli veri kaynaklarına doğrudan erişmek için not defteri içindeki kodu da kullanabilirsiniz. Ayrıca, bir kod hücresindeki komutları kullanarak rasgele verilere de erişebilirsiniz. Bu tür veriler not defteri oturumundaki değişkenlerde depolandığı için, proje dosyalarını oluşturmak için kod kullanmadığınız sürece projeye kaydedilmez.

Verilerde kodla çalışmak en iyi çalışan bir not defteriiçinde yaşanır: bu amaçla Azure [Not Defterleri örnek not defterinde verilerinize başlar'a](https://notebooks.azure.com/Microsoft/projects/samples/html/Getting%20to%20your%20Data%20in%20Azure%20Notebooks.ipynb)bakın.

Bu makalenin geri kalanı, proje düzeyinde dosya işlemleri hakkında ayrıntılar sağlar.

## <a name="import-data"></a>Veri içeri aktarma

Proje panosundan veya çalışan bir not defterine **Veri** menüsü veya `curl`.

### <a name="import-files-from-the-project-dashboard"></a>Proje panosundan dosyaları alma

1. Projede, dosyaları almak istediğiniz klasöre gidin.

1. **Url'den** veya **bilgisayardan** **Yükle** komutunu seçin ve almak istediğiniz veriler için gerekli ayrıntıları yansıtın:

   - **URL'den**: **Dosya Adı** alanında projenizdeki not defterine atamak için **Dosya URL** alanına kaynak adresi ve dosya adını girin. Ardından URL'yi yükleme listesine eklemek için **Dosya Ekle'yi** seçin. Ek URL'ler için işlemi yineleyin ve **ardından Bitti'yi**seçin.

     ![URL açılır penceresinden yükleme](media/quickstarts/upload-from-url-popup.png)

   - **Bilgisayardan**: Dosyaları açılır pencereye sürükleyip bırakın veya **Dosyaları Seç'i**seçin, ardından içe aktarmak istediğiniz veri dosyalarına göz atın ve seçin. Dosyayı açmak ve verilerini ayrıştırmak not defterindeki koda bağlı olduğundan, herhangi bir tür ve biçimde istediğiniz sayıda dosyayı bırakabilir veya seçebilirsiniz.

     ![Bilgisayar açılır penceresinden yükleme](media/quickstarts/upload-from-computer-popup.png)

1. Alındıktan sonra, dosyalar proje panosunda görünür ve ilgili klasöre göreli yol adlarını kullanarak not defteri kodu içinde erişilebilir.

### <a name="import-files-from-the-file-menu-in-a-notebook"></a>Not defterinde Dosya menüsünden dosyaları alma

1. Çalışan bir not defterinde **Dosya** > **Yükleme** komutunu seçin:

    ![Not defteri içindeki Dosya Yükleme menüsü komutu](media/file-menu-upload.png)

1. Açılan iletişim kutusunda, yüklemek istediğiniz dosyalara gidin ve seçin. Herhangi bir türde istediğiniz sayıda dosya seçebilirsiniz. Bittiğinde **Aç'ı** seçin.

1. Görünen **Yükleme durumu** açılır listesinde, açılan listeden bir **Hedef Klasörü** seçin:

    - Oturum klasörü (*~/* ): Dosyaları geçerli not defteri oturumuna yükler, ancak projede dosya oluşturmaz. Oturum klasörü proje klasörüne eş, ancak oturum bittikten sonra devam etmez. Oturum dosyalarına kodda erişmek için, dosya adlarını göreli yola önret *... /*.

        Oturum klasörünü kullanmak deneme için yararlıdır ve projeyi uzun süreli olarak gereksinim duyabileceğiniz veya gerekmeyen dosyalarla karmaşayı önler. Ayrıca, çakışmalara neden olmadan ve dosyaları yeniden adlandırmak zorunda kalmadan projedeki dosyalarla aynı adlara sahip oturum klasörüne dosyaları yükleyebilirsiniz. Örneğin, projede *zaten data.csv'nin* bir sürümü olduğunu, ancak *data.csv'nin*farklı bir sürümünü denemek istediğinizi varsaysanız. Dosyayı oturum klasörüne yükleyerek, yüklenen dosyadaki verileri kullanarak not defterini çalıştırabilirsiniz *(.................. /data.csv*) proje dosyasındaki veriler yerine.

    - Proje klasörü (*/project*): koddaki göreli yol adlarını kullanarak erişilebilen dosyaları projeye yükler. Bu klasöre dosya yüklemek, proje panosuna dosya yüklemekle aynıdır. Dosya projeyle birlikte kaydedilir ve daha sonraki oturumlarda kullanılabilir.

        Projede zaten var olan dosyayla aynı ada sahip bir dosya yüklemeyi denerseniz yükleme başarısız olur. Bir dosyanın üzerine yazmak için, yeni dosyayı proje panosundan yükleyin, bu da size üzerine yazma seçeneği sunar.

1. İşlemi tamamlamak için **Yükle'yi Başlat'ı** seçin.

### <a name="create-or-import-files-using-commands"></a>Komutları kullanarak dosya oluşturma veya alma

Hem proje hem de oturum klasörleri içinde dosya oluşturmak için bir terminal içinde veya Python kod hücresi içindeki komutları kullanabilirsiniz. Örneğin, dosyaları doğrudan `curl` `wget` Internet'ten beğenme ve indirme komutları.

Terminaldeki dosyaları indirmek için proje panosundaki **Terminal** komutunu seçin ve ardından uygun komutları girin:

```bash
curl https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv

wget https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv
```

Not defterinde Python kod hücresi kullanırken, komutları `!`.

Proje klasörü varsayılan klasördür, bu nedenle *oil_price.csv* gibi bir hedef dosya adı belirtilmek projedeki dosyayı oluşturur. Oturum dosyası oluşturmak için adı .' ile öne * /* gibi *. /oil_price.csv*.

### <a name="create-files-in-code"></a>Kodda dosya oluşturma

Pandalar `write_csv` işlevi gibi bir dosya oluşturan kod kullanırken, yol adları her zaman proje klasörüne göredir. Kullanma *... /* not defteri durdurulduğunda ve kapatıldığında atılan bir oturum dosyası oluşturur.

## <a name="export-files"></a>Dosyaları dışa aktarma

Verileri proje panosundan veya not defterinden dışa aktarabilirsiniz.

## <a name="export-files-from-the-project-dashboard"></a>Proje panosundan dosyaları dışa aktarma

Proje panosunda, bir dosyaya sağ tıklayın ve **İndir'i**seçin:

![Proje öğesi bağlam menüsünde komutu karşıdan yükleme](media/download-command.png)

Ayrıca bir dosya seçebilir ve panodaki **İndir** komutunu (klavye kısayolu: d) kullanabilirsiniz:

![Proje panosunda araç çubuğu komutunu indirme](media/download-command-toolbar.png)

## <a name="export-files-from-the-data-menu-in-a-notebook"></a>Not defterindeki Veri menüsünden dosyaları dışa aktarma

1. **Dosya** > **İndir** menüsü komutunu seçin:

    ![Not defterinde Veri İndirme menüsü komutu](media/file-menu-download.png)

1. Oturumdaki klasörleri gösteren bir açılır pencere görüntülenir; *proje* klasörü proje dosyalarını içerir:

    ![Dosya ve klasörleri seçtiğiniz Veri İndirme komutu açılır](media/file-menu-download-popup.png)

1. İndirmek istediğiniz dosya ve klasörlerin solundaki kutuları seçin ve ardından **Seçili İndir'i**seçin.

1. Not defteri, seçili dosyaları içeren tek bir *.zip* dosyası hazırlar ve bu dosyayı normalde tarayıcınızdan yaptığınız gibi kaydedersiniz. Not defteri, tek bir dosyayı karşıdan yüklediğinizde bile *bir .zip* dosyası oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

- [Not defterinde bulut verilerine erişme](access-data-resources-jupyter-notebooks.md)
