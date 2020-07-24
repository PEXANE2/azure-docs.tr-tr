---
title: XSLT haritalar ile XML dönüştürme
description: Enterprise Integration Pack ile Azure Logic Apps XML dönüştürmek için XSLT haritaları ekleme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/06/2019
ms.openlocfilehash: ff6bd1a52e05d55d43969644141f39aed8c8eb63
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072649"
---
# <a name="transform-xml-with-maps-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps’te haritalarla XML’i dönüştürme

Mantıksal uygulamanız, Azure Logic Apps'teki kurumsal tümleştirme senaryolarında XML verilerini farklı biçimlere dönüştürmek için Genişletilebilir Stil Sayfası Dili Dönüşümleri (XSLT) eşlemelerini kullanabilir. Eşleme, XML belgesindeki verilerin başka bir biçime nasıl dönüştürüleceğini tanımlayan bir XML belgesidir. 

Örneğin, YYYMMDD tarih biçimini kullanan bir müşteriden düzenli olarak B2B siparişleri veya faturalar aldığınızı varsayalım. Ancak, kuruluşunuz MMDDYYY tarih biçimini kullanır. Sipariş veya fatura ayrıntılarını müşteri etkinlik veritabanınıza depolamadan önce YYYMMDD tarih biçimini MMDDYYY biçimine dönüştüren bir harita tanımlayabilir ve kullanabilirsiniz.

Haritalar gibi tümleştirme hesapları ve yapıtlarla ilgili sınırlar için bkz. [Azure Logic Apps Için sınırlara ve yapılandırma bilgileri](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Haritalar ve diğer yapıtları, kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümleri için depoladığınız bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) .

* Haritanız bir dış derlemeye başvuruyorsa, *hem derlemeyi hem de eşlemeyi* tümleştirme hesabınıza yüklemeniz gerekir. [*Önce derlemenizi karşıya*](#add-assembly)yüklediğinizden emin olun ve ardından derlemeye başvuran Haritayı karşıya yükleyin.

  Derlemeniz 2 MB veya daha küçükse, derlemenizi *doğrudan* Azure Portal tümleştirme hesabınıza ekleyebilirsiniz. Ancak, derlemeniz veya haritanız 2 MB 'tan büyükse ancak [derlemeler ya da haritalar için boyut sınırından](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)büyük değilse, şu seçeneklere sahipsiniz:

  * Derlemeler için, derlemenizi ve kapsayıcının konumunu karşıya yükleyebileceğiniz bir Azure Blob kapsayıcısına ihtiyacınız vardır. Bu şekilde, derlemeyi tümleştirme hesabınıza eklediğinizde daha sonra bu konumu sağlayabilirsiniz. 
  Bu görev için şu öğeler gereklidir:

    | Öğe | Açıklama |
    |------|-------------|
    | [Azure depolama hesabı](../storage/common/storage-account-overview.md) | Bu hesapta, derlemeniz için bir Azure Blob kapsayıcısı oluşturun. [Depolama hesabı oluşturmayı](../storage/common/storage-account-create.md)öğrenin. |
    | Blob kapsayıcı | Bu kapsayıcıda, derlemenizi karşıya yükleyebilirsiniz. Ayrıca, derlemeyi tümleştirme hesabınıza eklediğinizde bu kapsayıcının konumu da gerekir. [BLOB kapsayıcısı oluşturmayı](../storage/blobs/storage-quickstart-blobs-portal.md)öğrenin. |
    | [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) | Bu araç, depolama hesaplarını ve BLOB kapsayıcılarını daha kolay yönetmenize yardımcı olur. Depolama Gezgini kullanmak için [Azure Depolama Gezgini indirin ve yükleyin](https://www.storageexplorer.com/). Ardından, [Depolama Gezgini kullanmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md)bölümündeki adımları izleyerek Depolama Gezgini depolama hesabınıza bağlanın. Daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Depolama Gezgini ile nesne depolamada blob oluşturma](../storage/blobs/storage-quickstart-blobs-storage-explorer.md). <p>Ya da Azure portal, depolama hesabınızı bulun ve seçin. Depolama hesabı menüsünden **Depolama Gezgini**' yi seçin. |
    |||

  * Haritalar için şu anda [Azure Logic Apps REST API haritaları](/rest/api/logic/maps/createorupdate)kullanarak daha büyük haritalar ekleyebilirsiniz.

Haritalar oluştururken ve eklerken bir mantıksal uygulamaya ihtiyacınız yoktur. Ancak, bir eşleme kullanmak için mantıksal uygulamanız, bu eşlemeyi depoladığınız bir tümleştirme hesabına bağlanması gerekir. [Mantıksal uygulamaları tümleştirme hesaplarına bağlamayı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)öğrenin. Henüz bir Logic App yoksa [Logic Apps oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-assembly"></a>

## <a name="add-referenced-assemblies"></a>Başvurulan derlemeler ekleme

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Tümleştirme hesabınızı bulmak ve açmak için, ana Azure menüsünde **tüm hizmetler**' i seçin. 
   Arama kutusuna "tümleştirme hesabı" yazın. 
   **Tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesabını bul](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Derlemenizi eklemek istediğiniz tümleştirme hesabını seçin, örneğin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında **derlemeler** kutucuğunu seçin.

   !["Derlemeler" ı seçin](./media/logic-apps-enterprise-integration-maps/select-assemblies.png)

1. **Derlemeler** sayfası açıldıktan sonra **Ekle**' yi seçin.

   !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-maps/add-assembly.png)

Derleme dosyanızın boyutuna bağlı olarak, [en fazla 2](#smaller-assembly) MB veya 2 MB 'a kadar [ancak 8 MB 'a kadar](#larger-assembly)olan bir derlemeyi karşıya yükleme adımlarını izleyin.
Tümleştirme hesaplarındaki derleme miktarları sınırları için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits).

> [!NOTE]
> Derlemenizi değiştirirseniz haritada değişiklik olup olmadığı de eşlemenizi güncelleştirmeniz gerekir.

<a name="smaller-assembly"></a>

### <a name="add-assemblies-up-to-2-mb"></a>2 MB 'a kadar derlemeler ekleyin

1. **Derleme Ekle**altında, derlemeniz için bir ad girin. **Küçük dosyayı** seçili tut. **Derleme** kutusunun yanındaki klasör simgesini seçin. Karşıya yüklediğiniz derlemeyi bulun ve seçin, örneğin:

   ![Daha küçük derlemeyi karşıya yükle](./media/logic-apps-enterprise-integration-maps/upload-assembly-file.png)

   Bütünleştirilmiş kod **adı** özelliğinde, derlemeyi seçtikten sonra derlemenin dosya adı otomatik olarak görünür.

1. Hazırsanız **Tamam**' ı seçin.

   Derleme dosyanız karşıya yüklemeyi tamamladıktan sonra, derleme **derlemeler** listesinde görünür.

   ![Karşıya yüklenen derlemeler listesi](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies-list.png)

   Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında, artık **yüklenen derlemelerin sayısını** gösterir, örneğin:

   ![Karşıya yüklenen derlemeler](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies.png)

<a name="larger-assembly"></a>

### <a name="add-assemblies-more-than-2-mb"></a>2 MB 'tan fazla derleme Ekle

Daha büyük derlemeler eklemek için, derlemenizi Azure Storage hesabınızdaki bir Azure Blob kapsayıcısına yükleyebilirsiniz. Derlemeleri ekleme adımlarınız, blob kapsayıcının ortak okuma erişimine sahip olup olmadığına bağlı olarak farklılık gösterir. İlk olarak, aşağıdaki adımları izleyerek blob kabın genel okuma erişimine sahip olup olmadığını kontrol edin: [BLOB kapsayıcısı için genel erişim düzeyini ayarlama](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcı erişim düzeyini denetle

1. Azure Depolama Gezgini açın. Gezgin penceresinde, henüz genişletilmemişse Azure aboneliğinizi genişletin.

1. **Depolama hesapları** > {*Storage-Account*} > **BLOB kapsayıcıları**' nı genişletin. Blob kapsayıcınızı seçin.

1. Blob kapsayıcının kısayol menüsünde, **ortak erişim düzeyini ayarla**' yı seçin.

   * Blob kapsayıcıda en az ortak erişim varsa, **iptal**' i seçin ve bu sayfada daha sonra bu adımları izleyin: [ortak erişimle kapsayıcılara yükleme](#public-access-assemblies)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kabınızda ortak erişim yoksa, **iptal**' i seçin ve bu sayfada daha sonra bu adımları izleyin: [ortak erişim olmadan kapsayıcılara yükleme](#no-public-access-assemblies)

     ![Genel erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-assemblies"></a>

#### <a name="upload-to-containers-with-public-access"></a>Ortak erişimle kapsayıcılara yükleme

1. Derlemeyi depolama hesabınıza yükleyin. 
   Sağ taraftaki pencerede **Yükle**' yi seçin.

1. Karşıya yüklemeyi tamamladıktan sonra, karşıya yüklenen derlemenizi seçin. Araç çubuğunda, derlemenin URL 'sini kopyalamak için **URL 'Yi Kopyala** ' yı seçin.

1. **Derleme Ekle** bölmesinin açık olduğu Azure Portal geri dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosya (2 MB 'tan büyük)** seçeneğini belirleyin.

   **İçerik URI 'si** kutusu artık **derleme** kutusu yerine görüntülenir.

1. **IÇERIK URI** 'si kutusunda, DERLEMENIZIN URL 'sini yapıştırın. 
   Derlemenizi ekleme işini sona erdirin.

Derlemeniz karşıya yüklemeyi tamamladıktan sonra, şema **derlemeler** listesinde görünür.
Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler** **altında, artık** karşıya yüklenen derlemelerin sayısını gösterir.

<a name="no-public-access-assemblies"></a>

#### <a name="upload-to-containers-without-public-access"></a>Ortak erişim olmadan kapsayıcılara yükleme

1. Derlemeyi depolama hesabınıza yükleyin. 
   Sağ taraftaki pencerede **Yükle**' yi seçin.

1. Karşıya yüklemeyi tamamladıktan sonra, derlemenizin paylaşılan erişim imzasını (SAS) oluşturun. 
   Derlemenin kısayol menüsünde, **paylaşılan erişim Imzası al**' ı seçin.

1. **Paylaşılan erişim imzası** bölmesinde **kapsayıcı düzeyinde paylaşılan erişim imzası URI**  >  **Oluştur**' u seçin. 
   SAS URL 'SI oluşturulduktan sonra, **URL** kutusunun yanındaki **Kopyala**' yı seçin.

1. **Derleme Ekle** bölmesinin açık olduğu Azure Portal geri dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosya (2 MB 'tan büyük)** seçeneğini belirleyin.

   **İçerik URI 'si** kutusu artık **derleme** kutusu yerine görüntülenir.

1. **İçerik URI 'si** kutusunda, daha önce oluşturmuş olduğunuz SAS URI 'sini yapıştırın. Derlemenizi ekleme işini sona erdirin.

Derlemeniz karşıya yüklemeyi tamamladıktan sonra, derleme **şemalar** listesinde görünür. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler** **altında, artık** karşıya yüklenen derlemelerin sayısını gösterir.

## <a name="create-maps"></a>Haritalar oluşturma

Harita olarak kullanabileceğiniz bir XSLT belgesi oluşturmak için, [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)kullanarak bir BizTalk tümleştirme projesi oluşturmak Için Visual Studio 2015 kullanabilirsiniz. Bu projede, iki XML şema dosyası arasında öğeleri görsel olarak eşlemenizi sağlayan bir tümleştirme eşleme dosyası oluşturabilirsiniz. Bu projeyi derledikten sonra bir XSLT belgesi alırsınız.
Tümleştirme hesaplarındaki eşleme miktarları sınırları için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits). 

## <a name="add-maps"></a>Haritalar ekleme

Haritalarınızın başvurduğu derlemeleri karşıya yükledikten sonra, eşlemenizi karşıya yükleyebilirsiniz.

1. Henüz oturum açmadıysanız Azure hesabı kimlik bilgilerinizle [Azure Portal](https://portal.azure.com) oturum açın. 

1. Tümleştirme hesabınız zaten açık değilse, ana Azure menüsünde **tüm hizmetler**' i seçin. 
   Arama kutusuna "tümleştirme hesabı" yazın. 
   **Tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesabını bul](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Eşlemenizi eklemek istediğiniz tümleştirme hesabını seçin, örneğin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında **haritalar** kutucuğunu seçin.

   !["Haritalar" ı seçin](./media/logic-apps-enterprise-integration-maps/select-maps.png)

1. **Haritalar** sayfası açıldıktan sonra **Ekle**' yi seçin.

   !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-maps/add-map.png)  

<a name="smaller-map"></a>

### <a name="add-maps-up-to-2-mb"></a>2 MB 'a kadar haritalar ekleyin

1. **Eşleme Ekle**' nin altında, haritanız için bir ad girin. 

1. **Eşleme türü**altında, türü seçin, örneğin: **sıvı**, **XSLT**, **XSLT 2,0**veya **XSLT 3,0**.

1. **Küçük dosyayı** seçili tut. **Harita** kutusunun yanındaki klasör simgesini seçin. Karşıya yüklediğiniz eşlemeyi bulun ve seçin, örneğin:

   ![Haritayı karşıya yükle](./media/logic-apps-enterprise-integration-maps/upload-map-file.png)

   **Name** özelliğini boş bıraktıysanız, eşleme dosyasını seçtikten sonra haritanın dosya adı otomatik olarak bu özellikte görüntülenir. 
   Ancak, benzersiz bir ad kullanabilirsiniz.

1. Hazırsanız **Tamam**' ı seçin. 
   Harita dosyanız karşıya yüklemeyi tamamladıktan sonra harita **haritalar** listesinde görünür.

   ![Karşıya yüklenen haritalar listesi](./media/logic-apps-enterprise-integration-maps/uploaded-maps-list.png)

   Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında, **haritalar** kutucuğu artık yüklenen haritalar sayısını gösterir, örneğin:

   ![Haritalar karşıya yüklendi](./media/logic-apps-enterprise-integration-maps/uploaded-maps.png)

<a name="larger-map"></a>

### <a name="add-maps-more-than-2-mb"></a>2 MB 'tan fazla harita ekleme

Şu anda, daha büyük haritalar eklemek için [Azure Logic Apps REST API haritaları](/rest/api/logic/maps/createorupdate)kullanın.

<!--

To add larger maps, you can upload your map to 
an Azure blob container in your Azure storage account. 
Your steps for adding maps differ based whether your 
blob container has public read access. So first, check 
whether or not your blob container has public read 
access by following these steps: 
[Set public access level for blob container](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### Check container access level

1. Open Azure Storage Explorer. In the Explorer window, 
   expand your Azure subscription if not already expanded.

1. Expand **Storage Accounts** > {*your-storage-account*} > 
   **Blob Containers**. Select your blob container.

1. From your blob container's shortcut menu, 
   select **Set Public Access Level**.

   * If your blob container has at least public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers with public access](#public-access)

     ![Public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * If your blob container doesn't have public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers without public access](#public-access)

     ![No public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-maps"></a>

### Add maps to containers with public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**. 

1. After you finish uploading, select your 
   uploaded map. On the toolbar, choose **Copy URL** 
   so that you copy the map's URL.

1. Return to the Azure portal where the 
   **Add Map** pane is open. Choose **Large file**. 

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste your map's URL. 
   Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

<a name="no-public-access-maps"></a>

### Add maps to containers with no public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**.

1. After you finish uploading, generate a 
   shared access signature (SAS) for your schema. 
   From your map's shortcut menu, 
   select **Get Shared Access Signature**.

1. In the **Shared Access Signature** pane, select 
   **Generate container-level shared access signature URI** > **Create**. 
   After the SAS URL gets generated, next to the **URL** box, choose **Copy**.

1. Return to the Azure portal where the 
   **Add Maps** pane is open. Choose **Large file**.

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste the SAS URI 
   you previously generated. Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

-->

## <a name="edit-maps"></a>Haritaları düzenleme

Mevcut bir eşlemeyi güncelleştirmek için, istediğiniz değişiklikleri içeren yeni bir eşleme dosyası yüklemeniz gerekir. Ancak, önce varolan Haritayı düzenlenmek üzere indirebilirsiniz.

1. [Azure Portal](https://portal.azure.com), henüz açık değilse, tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **tüm hizmetler**' i seçin. Arama kutusuna "tümleştirme hesabı" yazın. **Tümleştirme hesapları**' nı seçin.

1. Eşlemenizi güncelleştirmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında **haritalar** kutucuğunu seçin.

1. **Haritalar** sayfası açıldıktan sonra haritalarınızı seçin. 
   Önce Haritayı indirmek ve düzenlemek için **İndir**' i seçin ve Haritayı kaydedin.

1. Güncelleştirilmiş Haritayı karşıya yüklemeye hazırsanız **haritalar** sayfasında, güncelleştirmek istediğiniz eşlemeyi seçin ve **Güncelleştir**' i seçin.

1. Karşıya yüklemek istediğiniz güncelleştirilmiş eşlemeyi bulun ve seçin. 
   Harita dosyanız karşıya yüklemeyi tamamladıktan sonra, güncelleştirilmiş harita **haritalar** listesinde görüntülenir.

## <a name="delete-maps"></a>Haritaları silme

1. [Azure Portal](https://portal.azure.com), henüz açık değilse, tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **tüm hizmetler**' i seçin. 
   Arama kutusuna "tümleştirme hesabı" yazın. 
   **Tümleştirme hesapları**' nı seçin.

1. Eşlemenizi silmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında **haritalar** kutucuğunu seçin.

1. **Haritalar** sayfası açıldıktan sonra haritalarınızı seçin ve **Sil**' i seçin.

1. Haritayı silmek istediğinizi onaylamak için **Evet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md)  
* [Şemalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-schemas.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)
