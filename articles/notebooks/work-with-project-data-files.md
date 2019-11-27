---
title: İçeri aktarma ve Azure not defterleri ile projeleri ile verileri dışarı aktarma
description: Verileri bir Azure not defterleri projeye dış kaynaklardan hale getirme ve bir proje verileri dışarı aktarma.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: bd7ba27859e9d05c0d57c2f78b6449c2bc48ca33
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277393"
---
# <a name="work-with-data-files-in-azure-notebook-projects"></a>Azure not defteri projeleri veri dosyalarıyla çalışma

Birçok Jupyter not defterleri, özellikle veri bilimi için kullanılan not defterleri, lifeblood verilerdir. Azure not defterleri ile kolayca çeşitli kaynaklardan projesine içeri aktarın ve sonra bu verilerden bir not defterlerini kullanabilirsiniz. Daha sonra başka bir yerde kullanmak için indirebilirsiniz projesinde depolanan verileri üretme not defterleri de olabilir.

Çalışan bir not defteri içindeki **veri** menüsü Ayrıca, aynı zamanda projedeki dosyalarla ve geçerli not defteri oturumunun geçici dosyalarına çalışan **karşıya yükleme** ve **indirme** komutları sağlar.

Kod içinde bir not defteri çeşitli veri kaynaklarından doğrudan erişmek için bir proje içinde dosyaları dahil olmak üzere de kullanabilirsiniz. Rasgele verileri bir kod hücresine komutlarını kullanarak da erişebilirsiniz. Bu tür veriler, Not Defteri oturumunda değişkenlerinde depolandığından, özellikle proje dosyalarını oluşturmak için kod kullanmadığınız sürece projede kaydedilmez.

Veriler üzerinde kodla çalışma, çalışan bir not defteri içinde en iyi deneyimdir: Bu amaçla, [Azure Notebooks örnek not defterinde verilerinize alma](https://notebooks.azure.com/Microsoft/projects/samples/html/Getting%20to%20your%20Data%20in%20Azure%20Notebooks.ipynb)bölümüne bakın.

Bu makalenin geri kalanında proje düzeyi dosya işlemleri hakkında ayrıntılar sağlar.

## <a name="import-data"></a>Veri içeri aktarma

Dosya **menüsünü veya** `curl`gibi bir komutu kullanarak proje panosundan veya çalışan bir not defteri içinden bir projeye dosyaları taşıyabilirsiniz.

### <a name="import-files-from-the-project-dashboard"></a>Proje Panosu dosyalarından içeri aktarma

1. Proje dosyalarını içeri aktarmak istediğiniz klasöre gidin.

1. **Karşıya yükle** komutunu, **URL 'den** veya **bilgisayardan** ve içeri aktarmak istediğiniz veriler için gerekli ayrıntıları Project ' e seçin:

   - **URL 'den**: dosya **URL 'si** alanına kaynak adresini ve **dosya adı** alanında projenizdeki not defterine atanacak dosya adını girin. Ardından, URL 'YI karşıya yükle listesine eklemek için **+ Dosya Ekle** ' yi seçin. İşlemi ek URL 'Ler için tekrarlayın, sonra **bitti**' yi seçin.

     ![URL açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-url-popup.png)

   - **Bilgisayardan**: dosyaları açılan pencereye sürükleyip bırakın veya **dosyaları seç**' i seçin ve ardından içeri aktarmak istediğiniz veri dosyalarını seçin. Bırakın veya kod dosyasını açın ve verileri ayrıştırmak için not defterinde olduğu için herhangi bir sayıda herhangi bir türü ve biçim dosyalarını seçin.

     ![Bilgisayar açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-computer-popup.png)

1. İçeri sonra dosyaları proje Panosu üzerinde görünür ve not defteri kodu içeren klasöre göreli yol adları kullanarak içinde erişilebilir.

### <a name="import-files-from-the-file-menu-in-a-notebook"></a>Dosyaları bir not defterinde Dosya menüsünden içeri aktarma

1. Çalışan bir not defteri içinde **dosya** > **karşıya yükle** komutunu seçin:

    ![Karşıya yükleme komutu içinde bir not defteri dosyası](media/file-menu-upload.png)

1. Açılan iletişim kutusuna gidin ve karşıya yüklemek istediğiniz dosyaları seçin. Herhangi bir sayıda tüm dosya türlerini seçebilirsiniz. Bittiğinde **Aç** ' ı seçin.

1. Görüntülenen **karşıya yükleme durumu** açılan penceresinde, açılan listeden bir **hedef klasör** seçin:

    - Oturum klasörü ( *~/* ): dosyaları geçerli not defteri oturumuna yükler ancak projede dosya oluşturmaz. Oturum klasör proje klasörüne eşdüzeyde, ancak oturumu sona erdikten sonra kalmıyor. Koddaki oturum dosyalarına erişmek için, dosya adlarını göreli yol ile önek yapın *. /* .

        Oturum klasörü kullanılarak deneme için yararlıdır ve proje olabilir veya uzun vadeli temelinde gerekmeyebilir dosyalarla karışıklığı ortadan kaldırır. Çakışmalarına neden olmadan ve dosyaları yeniden adlandırmak gerek kalmadan proje dosyalarında aynı adlara sahip oturum klasöre dosyalar da karşıya yükleyebilirsiniz. Örneğin, projede zaten bir *Data. csv* sürümüne sahip olduğunu, ancak farklı bir *Data. csv*sürümüyle denemek istediğinizi varsayalım. Dosyayı oturum klasörüne karşıya yükleyerek, karşıya yüklenen dosyadaki verileri kullanarak not defterini çalıştırabilirsiniz (kullanarak kodda bulunan *... /DATAEM CSV*), proje dosyasındaki veriler yerine.

    - Proje klasörü ( */Project*): kodda göreli yol adları kullanılarak erişilebilecekleri projeye dosya yükler. Bir dosyayı bu klasöre yüklemek proje panosundaki bir dosyayı karşıya yüklemeyi aynıdır. Dosyanın projeyle kaydedilir ve daha sonraki oturumlarda kullanılabilir.

        Zaten projede var olan bir aynı ada sahip bir dosyayı karşıya yüklemeyi denerseniz, başarısız karşıya yükleniyor. Bir dosyanın üzerine yazmak için üzerine yazma seçeneğini sunar Yeni Proje panosunu dosyasından bunun yerine, yükleyin.

1. İşlemi gerçekleştirmek için **karşıya yüklemeyi Başlat** ' ı seçin.

### <a name="create-or-import-files-using-commands"></a>Oluşturma veya içeri aktarma komutlarını kullanarak dosyaları

Hem proje hem de oturum klasörlerdeki dosyaları oluşturmak için bir Python kodu hücreyi veya bir terminal içinde komutlarını kullanabilirsiniz. Örneğin, `curl` ve `wget` gibi komutlar doğrudan Internet 'ten dosya indirir.

Terminaldeki dosyaları indirmek için, proje panosunda **Terminal** komutunu seçin ve ardından uygun komutları girin:

```bash
curl https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv

wget https://raw.githubusercontent.com/petroleum101/figures/db46e7f48b8aab67a0dfe31696f6071fb7a84f1e/oil_price/oil_price.csv -o oil_price.csv
```

Bir not defterinde Python kod hücresi kullanırken, `!`komutları ön ekine ekleyin.

Proje klasörü varsayılan klasördür, bu nedenle *oil_price. csv* gibi bir hedef dosya adının belirtilmesi projede dosyayı oluşturur. Bir oturum dosyası oluşturmak için adı ile önek yapın *..*  *... /oil_price. csv*.

### <a name="create-files-in-code"></a>Kod içinde dosyaları oluşturma

Pandas `write_csv` işlevi gibi bir dosya oluşturan kodu kullanırken, yol adları her zaman proje klasörüne görelidir. Kullanılarak *... /* Not defteri durdurulduğunda ve kapatıldığında atılan bir oturum dosyası oluşturur.

## <a name="export-files"></a>Dosyaları dışarı aktarma

Proje panosunu veya içinden veri aktarabilirsiniz bir not defteri.

## <a name="export-files-from-the-project-dashboard"></a>Proje Panosu dosyalarından dışarı aktarma

Proje panosunda, bir dosyaya sağ tıklayın ve **İndir**' i seçin:

![Proje öğesi bağlam menüsü komutu indirin](media/download-command.png)

Ayrıca bir dosya seçebilir ve panoda **İndir** komutunu (klavye kısayolu: d) kullanabilirsiniz:

![Proje Panosu üzerinde araç çubuğu komutuna indirin](media/download-command-toolbar.png)

## <a name="export-files-from-the-data-menu-in-a-notebook"></a>Bir not defteri veri menüsünden dosyalarını Dışarı Aktar

1. **Dosya** > **İndir** menü komutunu seçin:

    ![Veri yükleme komutu içinde bir not defteri](media/file-menu-download.png)

1. Oturumdaki klasörleri gösteren bir açılan pencere görüntülenir; Proje *klasörü proje* dosyalarını içerir:

    ![Veri yükleme komut dosyaları ve klasörleri seçin açılan menüsü](media/file-menu-download-popup.png)

1. İndirmek istediğiniz dosya ve klasörlerin solundaki kutuları seçin ve ardından **Seçileni indir**' i seçin.

1. Not defteri, seçili dosyaları içeren tek bir *. zip* dosyası hazırlar ve bu dosya daha sonra tarayıcınızdan yaptığınız şekilde kaydedilir. Tek bir dosyayı indirdiğinizde bile not defteri bir *. zip* dosyası oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir not defterinde bulut verilerine erişme](access-data-resources-jupyter-notebooks.md)
