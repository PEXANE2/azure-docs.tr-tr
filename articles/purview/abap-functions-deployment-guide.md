---
title: Azure purview 'da SAP R3 for köprülerine yönelik meta veri ayıklama ABAP işlev modülünü dağıtma
description: Bu makalede, SAP sunucusunda ABAP işlev modülünü dağıtma adımları özetlenmektedir
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 911238efafb948b304455cf75cc4ec2c3c605c76
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102042104"
---
# <a name="deploy-the-metadata-extraction-abap-function-module-for-the-sap-r3-family-of-bridges"></a>SAP R3 for köprülerine yönelik meta veri ayıklama ABAP işlev modülünü dağıtın 
Bu makalede, SAP sunucusunda ABAP işlev modülünü dağıtma adımları özetlenmektedir
## <a name="overview"></a>Genel Bakış 

SAP Business Suite 4 HANA (S/4HANA), ECC ve R/3 ERP Köprüsü, SAP sunucusundan meta verileri ayıklamak için kullanılabilir. Bu, SAP sunucusuna ABAP Function modülü yerleştirilerek elde edilir. Bu işlev modülüne, SAP sunucusu içinde bulunan meta verileri sorgulamak ve indirmek (bir metin dosyası olarak) için köprü tarafından uzaktan erişilebilir.
Yürütüldüğünde, her iki köprü de:

1.  Önceki bir köprüden yürütmeden yerel olarak zaten indirilmiş olan bir dosyanın meta verilerini içeri aktarır.

2.  ABAP Module API 'sini çağırır, indirmeyi bekleyin ve sonra bu dosyadan meta verileri içeri aktarın.

Bu belge, Bu modülün dağıtılması için gereken adımların ayrıntılarını sağlar.

> [!Note] 
>Aşağıdaki yönergeler SAP GUI v. 7.2 temelinde derlendi

## <a name="deployment-of-the-module"></a>Modülün dağıtımı 

### <a name="create-a-package"></a>Paket oluşturma 

Bu adım isteğe bağlıdır ve var olan bir paket kullanılabilir.

1.  SAP S/4HANA veya SAP ECC sunucusunda oturum açın ve \" Nesne Gezgini 'ni \" (SE80 Transaction) açın.

2.  Listeden seçenek \" paketi \" ' ni seçin ve yeni paket için bir ad girin (örneğin, Z \_ miti) ve ardından ' Display ' düğmesine basın

3.  ' Paket oluştur ' penceresinde ' Evet 'e basın. Sonuç olarak, bir pencere \" paketi Oluşturucusu: oluşturma paketi \" açılır. ' Kısa açıklama ' alanına değer girin ve \" devam \" simgesine basın.

4.  ' Yerel çalışma ekranı isteği için sor ' penceresinde ' kendi Istekleri ' düğmesine basın. ' Geliştirme ' isteği ' ni seçin.

### <a name="create-a-function-group"></a>Işlev grubu oluşturma 

Nesne Gezgini ' nde \" listeden Işlev grubu ' \" nu seçin ve adını aşağıdaki giriş alanına yazın (örneğin, Z \_ miti \_ fgroup). Görünüm simgesine basın.

1.  \"Nesne oluştur \" penceresinde, yeni bir işlev grubu oluşturmak için Evet ' e basın.

2.  Kısa metin alanında uygun bir açıklama belirtin \" \" ve düğme Kaydet ' e \" basın \" .

3.  Önceki adımda hazırlanan paketi seçin \" bir paket oluşturun \" ve simge Kaydet ' e tıklayın \" \" .

4.  Simgeye devam et düğmesine basarak bir isteği onaylayın \" \" .

5.  Bu Işlev grubunu etkinleştirin.

### <a name="create-the-abap-function-module"></a>ABAP Function modülünü oluşturma 

İşlev grubu oluşturulup seçildikten sonra, depo tarayıcısında adına sağ tıklayın ve \" Oluştur = Işlev modülü ' nü seçin \> \" .

\" \_ \_ \" \" İşlev modülü 'ne indirme \" ve \" kısa metin \" girişini uygun açıklamayla doldurma adlı Z mitı 'yi girin.

Modül oluşturulduğunda, aşağıdaki bilgileri belirtin:

1.  \"Öznitelikler \" sekmesine gidin.

2.  Işlem türü = Remote-Enabled Işlev modülünü seçin.

    :::image type="content" source="media/abap-functions-deployment-guide/processing-type.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

3.  \"Kaynak kodu \" sekmesine gidin. İşlevi için kodu dağıtmanın iki yolu vardır:

    a.  Ana menüden, yardımcı programlar = diğer yardımcı programlar = karşıya yükle/Indir = karşıya yükle ' yi seçerek [Z \_ mitı \_ indirme](https://github.com/Azure/Purview-Samples/tree/master/connectors/sap) dosya dosyasını karşıya yükleyin \> \> \> .

    b.  Alternatif olarak, dosyayı açın, içeriğini kopyalayın ve \" kaynak kodu \" alanına yapıştırın.

4.  \"İçeri aktar sekmesine gidin \" ve aşağıdaki parametreleri oluşturun:

    a.  P \_ alanı türü DD02L-TABNAME (Isteğe bağlı = true)

    b.  *P \_ Yerel \_ yol türü dizesi* (isteğe bağlı = true)

    c.  *P \_ DIL türü L001TAB-VERI varsayılan \' E\'*

    d.  *ROWCOMP TYPE, bu yüzden \_ Int varsayılan 0*

    e.  *ROWCOUNT türü, \_ Int varsayılan 0*

    > [!Note]
    > \"Hepsi Için değer geçir ' i seçin \"

    :::image type="content" source="media/abap-functions-deployment-guide/import.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

5.  "Tablolar" sekmesine gidin ve aşağıdakileri tanımlayın:

    *\_TAB512 gıbı DıŞARı aktarma tablosu*

    :::image type="content" source="media/abap-functions-deployment-guide/export-table.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

6.  \"Özel durumlar sekmesine gidin \" ve aşağıdaki özel durumu tanımlayın:

    *E \_ Exp \_ GUI \_ DownloadFailed*

    :::image type="content" source="media/abap-functions-deployment-guide/exceptions.png" alt-text="kaynakları kaydetme seçenekleri" border="true":::

7.  İşlevi kaydedin (CTRL + S tuşlarına basın veya Işlev modülü = \> Kaydet ' i seçin. Ana menüdeki).

8.  \" \" Araç çubuğunda Etkinleştir simgesine tıklayın (CTRL + F3) ve \" \" iletişim penceresinde devam düğmesine basın. İstenirse, ana işlev modülüyle birlikte etkinleştirilmesi için oluşturulan eklemeleri seçmelisiniz.

### <a name="testing-the-function"></a>Işlevi test etme 

Tüm önceki adımlar tamamlandığında, işlevi test etmek için aşağıdaki adımları izleyin:

1.  Z \_ mitı \_ indirme işlevi modülünü açın.

2.  \"Ana menüden Function Module = \> Test = \> test işlevi modülünü seçin \" (veya F8 tuşuna basın).

3.  Yerel dosya sistemindeki klasörün yolunu P \_ yerel \_ yoluna girin ve \" \" araç çubuğundaki yürütme simgesine basın (veya F8 tuşuna basın).

4.  \_Meta veri içeren bir dosya indirilmeli veya güncelleniyorsa, ilgilendiğiniz alanın adını P alanı alanına koyun. İşlev çalışmayı bitirdiğinde, P \_ yerel \_ yol parametresinde belirtilen klasör içinde meta verileri olan birkaç dosya içermelidir. Dosya adları, P alanı alanında belirtime alanları taklit edebilir \_ .

İşlev, yürütmesini tamamlayacak ve meta verileri, SAP S/4HANA veya ECC Server ile yüksek hızda ağ bağlantısı olan bir makinede başlatılması durumunda çok daha hızlı indirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [SAP ECC kaynağını kaydetme ve tarama](register-scan-sapecc-source.md)
- [SAP S/4HANA kaynağını kaydedin ve tarayın](register-scan-saps4hana-source.md)