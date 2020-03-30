---
title: Depolama Gezgini ile Azure Blob Depolama kaynaklarını yönetme | Microsoft Dokümanlar
description: Depolama Gezgini ile Azure Blob Kapsayıcılarını ve Blob'ları yönetme
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2019
ms.author: cawa
ms.openlocfilehash: 56c20c995a95058b5039b7268c7b7b1426e900fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244400"
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer"></a>Azure Blob Depolama kaynaklarını Depolama Gezgini'yle yönetme

## <a name="overview"></a>Genel Bakış

[Azure Blob Depolama,](storage/blobs/storage-dotnet-how-to-use-blobs.md) http veya HTTPS üzerinden dünyanın her yerinden erişilebilen metin veya ikili veri gibi büyük miktarlarda yapılandırılmamış veri depolamak için bir hizmettir.
Verileri genel olarak herkese açık kullanıma sunmak veya uygulama verilerini özel olarak depolamak için Blob Storage’ı kullanabilirsiniz. Bu makalede, blob kapsayıcıları ve lekeleri ile çalışmak için Depolama Gezgini'ni nasıl kullanacağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* [Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)
* [Azure depolama hesabına veya hizmetine bağlanma](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Tüm lekeler sadece blobs mantıksal bir gruplama bir blob konteyner, ikamet etmelidir. Bir hesap sınırsız sayıda kapsayıcı içerebilir ve her kapsayıcı sınırsız sayıda blob saklayabilir.

Aşağıdaki adımlar, Depolama Gezgini içinde bir blob kapsayıcısının nasıl oluşturulabildiğini gösterir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, blob kapsayıcısını oluşturmak istediğiniz depolama hesabını genişletin.
3. Blob **Containers**sağ tıklatın ve - bağlam menüsünden - **Blob Konteyner oluştur'u**seçin.

   ![Blob kapsayıcıları bağlam menüsü oluşturma][0]
4. **Blob Containers** klasörün altında bir metin kutusu görünür. Blob kapsayıcınızın adını girin. Bkz. Blob kapsayıcılarını adlandırma kuralları ve kısıtlamaları hakkında bilgi için [bir kapsayıcı oluştur.](storage/blobs/storage-quickstart-blobs-dotnet.md#create-a-container)

   ![Blob Konteynerleri metin kutusu oluşturma][1]
5. Blob kapsayıcısını oluşturmak için yapıldığında **Enter** tuşuna basın veya iptal etmek için **Esc** tuşuna basın. Blob kapsayıcısı başarıyla oluşturulduktan sonra, seçili depolama hesabı için **Blob Kapsayıcıları** klasörü altında görüntülenir.

   ![Blob Konteyner oluşturuldu][2]

## <a name="view-a-blob-containers-contents"></a>Blob konteynerinin içeriğini görüntüleme

Blob kapsayıcılar blobs ve klasörler (aynı zamanda lekeler içerebilir) içerir.

Aşağıdaki adımlar, Depolama Gezgini içindeki bir blob kapsayıcısının içeriğini nasıl görüntülenebildiğini gösterir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. Görüntülemek istediğiniz blob kapsayıcısına sağ tıklayın ve bağlam menüsünden **Blob Konteyner Düzenleyicisini Aç'ı**seçin.
   Ayrıca görüntülemek istediğiniz blob kabına çift tıklayabilirsiniz.

   ![Blob container editor bağlam menüsünü aç][19]
5. Ana bölmede blob konteynerinin içindekiler görüntülenir.

   ![Blob konteyner editörü][3]

## <a name="delete-a-blob-container"></a>Blob kapsayıcısı silme

Blob kapsayıcıları kolayca oluşturulabilir ve gerektiğinde silinebilir. (Tek tek lekeleri nasıl silmek için görmek için, bölüme bakın, [bir blob kapsayıcılekeleri yönetme](#managing-blobs-in-a-blob-container).)

Aşağıdaki adımlar, Depolama Gezgini içindeki bir blob kapsayıcısının nasıl silinir gösteriş olduğunu gösterir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, görüntülemek istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. Silmek istediğiniz blob kapsayıcısına sağ tıklayın ve - bağlam menüsünden - **Sil'i**seçin.
   Ayrıca, şu anda seçili blob kapsayıcısını silmek için **Sil** tuşuna da basabilirsiniz.

   ![Blob kapsayıcı bağlam menüsünü silme][4]
5. Onay iletişim kutusunda **Evet**’i seçin.

   ![Blob Konteyner onayIni silme][5]

## <a name="copy-a-blob-container"></a>Blob kabını kopyalama

Depolama Gezgini, bir blob kapsayıcısını panoya kopyalamanızı ve sonra bu blob kapsayıcısını başka bir depolama hesabına yapıştırmamanızı sağlar. (Tek tek lekelerin nasıl kopyalanıştırılabildiğini görmek için, [blob kabındaki lekeleri yönetme](#managing-blobs-in-a-blob-container)bölümüne bakın.)

Aşağıdaki adımlar, bir blob kapsayıcısının bir depolama hesabından diğerine nasıl kopyalanıriş olduğunu gösterir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, kopyalamak istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. Kopyalamak istediğiniz blob kapsayıcısına sağ tıklayın ve - bağlam menüsünden - **Blob Kapsayıcıyı Kopyala'yı**seçin.

   ![Blob kapsayıcı bağlam menüsünü kopyala][6]
5. Blob kabını yapıştırmak istediğiniz istenilen "hedef" depolama hesabına sağ tıklayın ve - bağlam menüsünden - **Ekle Blob Kapsayıcı'yı**seçin.

   ![Blob konteyner bağlam menüsünü yapıştır][7]

## <a name="get-the-sas-for-a-blob-container"></a>Blob kapsayıcısı için SAS alma

[Paylaşılan erişim imzası (SAS)](storage/common/storage-dotnet-shared-access-signature-part-1.md), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar.
Başka bir deyişle, hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan, depolama hesabınızdaki nesnelere belirli bir süre için ve belirli bir izin kümesiyle sınırlı istemci izinleri verebilirsiniz.

Aşağıdaki adımlar, bir blob kapsayıcı için bir SAS oluşturmak için nasıl gösteriş:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, SAS almak istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. İstenilen blob kapsayıcısını sağ tıklatın ve - bağlam menüsünden - **Paylaşılan Erişim İmzasını Al'ı**seçin.

   ![SAS bağlam menüsünü alın][8]
5. **Paylaşılan Erişim İmzası** iletişim kutusunda ilkeyi, başlangıç ve sona erme tarihlerini, saat dilimini ve kaynak için istediğiniz erişim düzeylerini belirtin.

   ![SAS seçeneklerini alın][9]
6. SAS seçeneklerini belirtmeyi tamamladığınızda **Oluştur**’u seçin.
7. İkinci bir **Paylaşılan Erişim İmzailetişim** kutusu, depolama kaynağına erişmek için kullanabileceğiniz URL ve QueryStrings ile birlikte blob kapsayıcısını listeleyen görüntüler.
   Panoya kopyalamak istediğiniz URL’nin yanındaki **Kopyala** öğesini seçin.

   ![SAS URL'lerini kopyala][10]
8. İşiniz bittiğinde **Kapat**’ı seçin.

## <a name="manage-access-policies-for-a-blob-container"></a>Blob kapsayıcısı için Erişim İlkeleri'ni yönetme

Aşağıdaki adımlar, bir blob kapsayıcısı için erişim ilkelerinin nasıl yönetilen (ekleve kaldırılacak) gösteriş yapılacağını gösterir:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. İstenilen blob kapsayıcısını seçin ve bağlam menüsünden **Erişim İlkelerini Yönet'i**seçin.

   ![Erişim ilkelerini yönet bağlam menüsü][11]
5. **Access İlkeleri** iletişim kutusu, seçili blob kapsayıcısı için zaten oluşturulmuş erişim ilkelerini listeler.

   ![Erişim İlkesi seçenekleri][12]
6. Erişim ilkesi yönetim görevine bağlı olarak aşağıdaki adımları izleyin:

   * **Yeni bir erişim ilkesi ekleme** - **Ekle**’yi seçin. Oluşturulduktan sonra, **Erişim İlkeleri** iletişim kutusunda yeni eklenen erişim ilkesi (varsayılan ayarlarla birlikte) gösterilir.
   * **Erişim ilkesini edin** - İstenilen tüm değişlmeleri yapve **Kaydet'** i seçin.
   * **Erişim ilkesini kaldırma** - Kaldırmak istediğiniz erişim ilkesinin yanındaki **Kaldır** öğesini seçin.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Blob kapsayıcısı için Genel Erişim Düzeyini ayarlama

Varsayılan olarak, her blob kapsayıcı "Ortak erişim yok" olarak ayarlanır.

Aşağıdaki adımlar, bir blob kapsayıcısı için ortak erişim düzeyinin nasıl belirtilen gösteriş olduğunu gösterir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, erişim ilkelerini yönetmek istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. İstenilen blob kapsayıcısını seçin ve bağlam menüsünden **Ortak Erişim Düzeyini Ayarla'yı**seçin.

   ![Genel erişim düzeyi bağlam menüsünü ayarlama][13]
5. Kapsayıcı **Ortak Erişim Düzeyini Ayarla** iletişim kutusunda, istenen erişim düzeyini belirtin.

   ![Genel erişim düzeyi seçeneklerini ayarlama][14]
6. **Uygula**’yı seçin.

## <a name="managing-blobs-in-a-blob-container"></a>Blob kabında lekeleri yönetme

Bir blob kapsayıcı oluşturduktan sonra, bu blob kapsayıcısına bir blob yükleyebilir, yerel bilgisayarınıza bir blob indirebilir, yerel bilgisayarınızda bir damla açabilir ve çok daha fazlasını yapabilirsiniz.

Aşağıdaki adımlar, blob kapsayıcısı içindeki lekelerin (ve klasörlerin) nasıl yönetilenini gösterir.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, yönetmek istediğiniz blob kapsayıcısını içeren depolama hesabını genişletin.
3. Depolama hesabının **Blob Kapları'nı**genişletin.
4. Görüntülemek istediğiniz blob kabına çift tıklayın.
5. Ana bölmede blob konteynerinin içindekiler görüntülenir.

   ![Blob kabını görüntüle][3]
6. Ana bölmede blob konteynerinin içindekiler görüntülenir.
7. Gerçekleştirmek istediğiniz göreve bağlı olarak aşağıdaki adımları izleyin:

   * **Dosyaları blob kapsayıcısına yükleme**

     1. Ana bölmenin araç çubuğunda **Yükle'yi**ve ardından açılan menüden **Dosyaları Yükle'yi** seçin.

        ![Dosyaları karşıya yükleme menüsü][15]
     2. **Dosyaları Karşıya Yükle** iletişim kutusunda, **Dosyalar** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek karşıya yüklemek istediğiniz dosyaları belirleyin.

        ![Dosya yükleme seçenekleri][16]
     3. **Blob türünü**belirtin. Bkz. Daha fazla bilgi için [bir kapsayıcı oluştur.](storage/blobs/storage-quickstart-blobs-dotnet.md#create-a-container)
     4. İsteğe bağlı olarak, seçili dosyanın(lar) yüklendiği hedef klasörü belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Yükle'yi**seçin.
   * **Blob kapsayıcısına klasör yükleme**

     1. Ana bölmenin araç **çubuğunda,** açılan menüden Yükle ve ardından **Klasörü Yükle'yi** seçin.

        ![Klasörü karşıya yükle menüsü][17]
     2. **Klasörü karşıya yükle** iletişim kutusunda, **Klasör** metin kutusunun sağ tarafındaki üç noktayı (**…**) seçerek içeriklerini karşıya yüklemek istediğiniz klasörü belirleyin.

        ![Klasör ekleme seçenekleri][18]
     3. **Blob türünü**belirtin. Bkz. Daha fazla bilgi için [bir kapsayıcı oluştur.](storage/blobs/storage-quickstart-blobs-dotnet.md#create-a-container)
     4. İsteğe bağlı olarak, seçili klasörün içeriklerinin yükleneceği bir hedef klasör belirtin. Hedef klasör mevcut değilse, oluşturulur.
     5. **Yükle'yi**seçin.
   * **Yerel bilgisayarınıza bir damla indirme**

     1. İndirmek istediğiniz blob'u seçin.
     2. Ana bölmedeki araç çubuğunda **İndir**’i seçin.
     3. İndirilen blob iletişim kutusunu **nereye kaydedebileceğinizi belirtin'de,** blob'un indirilmesini istediğiniz yeri ve vermek istediğiniz adı belirtin.  
     4. **Kaydet'i**seçin.
   * **Yerel bilgisayarınızda bir leke açma**

     1. Açmak istediğiniz blob seçin.
     2. Ana bölmedeki araç çubuğunda **Aç**’ı seçin.
     3. Blob indirilir ve blob'un temel dosya türüile ilişkili uygulama kullanılarak açılır.
   * **Panoya bir leke kopyalama**

     1. Kopyalamak istediğiniz blob'u seçin.
     2. Ana bölmedeki araç çubuğunda **Kopyala**’yı seçin.
     3. Sol bölmede, başka bir blob kapsayıcısına gidin ve ana bölmede görüntülemek için çift tıklatın.
     4. Ana bölmenin araç çubuğunda, blob'un bir kopyasını oluşturmak için **Yapıştır'ı** seçin.
   * **Blob silme**

     1. Silmek istediğiniz blob'u seçin.
     2. Ana bölmedeki araç çubuğunda **Sil**’i seçin.
     3. Onay iletişim kutusunda **Evet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [En son Depolama Gezgini yayın notlarını ve videolarını](https://www.storageexplorer.com) görüntüleyin.
* [Azure bloblarını, tablolarını, kuyruklarını ve dosyalarını kullanarak uygulama oluşturma](https://azure.microsoft.com/documentation/services/storage/) hakkında bilgi edinin.

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
