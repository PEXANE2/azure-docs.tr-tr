---
title: XSLT haritalarıyla XML'i dönüştürün
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta XML'i dönüştürmek için XSLT haritaları ekleyin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/06/2019
ms.openlocfilehash: e186b9713c8464f8f37e1e0bf112c4118621925c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979402"
---
# <a name="transform-xml-with-maps-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps’te haritalarla XML’i dönüştürme

XML verilerini Azure Logic Apps'taki kurumsal tümleştirme senaryoları biçimleri arasında aktarmak için, mantık uygulamanız haritaları veya daha özel olarak Genişletilebilir Stil sayfası Dil Dönüşümleri (XSLT) eşlemlerini kullanabilir. Harita, XML belgesinden başka bir biçime nasıl veri dönüştürüldüğünü açıklayan bir XML belgesidir. 

Örneğin, YYYMMDD tarih biçimini kullanan bir müşteriden düzenli olarak B2B siparişleri veya faturaları aldığınızı varsayalım. Ancak, kuruluşunuz MMDDYYY tarih biçimini kullanır. Sipariş veya fatura ayrıntılarını müşteri etkinlik veritabanınızda depolamadan önce YYYMMDD tarih biçimini MMDDYYY biçimine dönüştüren bir harita tanımlayabilir ve kullanabilirsiniz.

Tümleştirme hesapları ve haritalar gibi yapılarla ilgili sınırlar [için Azure Mantık Uygulamaları için Sınırlar ve yapılandırma bilgilerine](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümleri için haritalarınızı ve diğer yapılarınızı depoladığınız bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* Haritanız harici bir derlemeye başvuruyorsa, *hem derlemeyi hem de haritayı* tümleştirme hesabınıza yüklemeniz gerekir. [*Önce assemblyinizi,*](#add-assembly)sonra da derlemeye atıfta bulunulan haritayı yüklediğinizden emin olun.

  Derlemeniz 2 MB veya daha küçükse, derlemenizi *doğrudan* Azure portalından entegrasyon hesabınıza ekleyebilirsiniz. Ancak, derlemeniz veya haritanız 2 MB'dan büyükse ancak [derlemeler veya haritalar için boyut sınırından](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)büyük değilse, şu seçeneklere sahipsiniz:

  * Derlemeler için, derlemenizi ve bu kapsayıcının konumunu yükleyebileceğiniz bir Azure blob kapsayıcısına ihtiyacınız vardır. Bu şekilde, derlemeyi tümleştirme hesabınıza eklediğinizde bu konumu daha sonra sağlayabilirsiniz. 
  Bu görev için şu öğelere ihtiyacınız vardır:

    | Öğe | Açıklama |
    |------|-------------|
    | [Azure depolama hesabı](../storage/common/storage-account-overview.md) | Bu hesapta, derlemeniz için bir Azure blob kapsayıcısı oluşturun. [Depolama hesabı oluşturmayı](../storage/common/storage-account-create.md)öğrenin. |
    | Blob kapsayıcı | Bu kapsayıcıda, derlemenizi yükleyebilirsiniz. Derlemeyi tümleştirme hesabınıza eklediğinizde de bu kapsayıcının konumuna ihtiyacınız vardır. [Blob kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)oluşturmayı öğrenin. |
    | [Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) | Bu araç, depolama hesaplarını ve blob kaplarını daha kolay yönetmenize yardımcı olur. Depolama Gezgini'ni kullanmak için [Azure Depolama Gezgini'ni indirin ve yükleyin.](https://www.storageexplorer.com/) Ardından, [Depolama Gezgini ile başlayın'daki](../vs-azure-tools-storage-manage-with-storage-explorer.md)adımları izleyerek Depolama Gezgini'ni depolama hesabınıza bağlayın. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Depolama Gezgini ile nesne depolama alanında bir leke oluşturun.](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) <p>Veya Azure portalında depolama hesabınızı bulun ve seçin. Depolama hesabı menüsünden **Storage Explorer'ı**seçin. |
    |||

  * Haritalar için, şu anda Azure [Logic Apps REST API - Haritalar'ı](https://docs.microsoft.com/rest/api/logic/maps/createorupdate)kullanarak daha büyük haritalar ekleyebilirsiniz.

Harita oluştururken ve eklerken bir mantık uygulamasına ihtiyacınız yoktur. Ancak, bir harita kullanmak için mantık uygulamanızın bu haritayı depoladığınız bir tümleştirme hesabına bağlanması gerekir. [Mantık uygulamalarını tümleştirme hesaplarına nasıl bağlayacaklarını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)öğrenin. Henüz bir mantık uygulamanız yoksa, [mantık uygulamaları oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-assembly"></a>

## <a name="add-referenced-assemblies"></a>Başvurulan derlemeler ekleme

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Tümleştirme hesabınızı bulmak ve açmak için ana Azure menüsünde **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme hesabı"nı girin. 
   **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Örneğin, derlemenizi eklemek istediğiniz tümleştirme hesabını seçin:

   ![Entegrasyon hesabını seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Derlemeler** döşemesini seçin.

   !["Derlemeler"i seçin](./media/logic-apps-enterprise-integration-maps/select-assemblies.png)

1. **Derlemeler** sayfası açıldıktan sonra **Ekle'yi**seçin.

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-maps/add-assembly.png)

Derleme dosyanızın boyutuna bağlı olarak, [2 MB'a veya 2 MB'dan fazla](#smaller-assembly) olan ancak yalnızca 8 [MB'a kadar](#larger-assembly)olan bir derlemeyi yüklemek için gereken adımları izleyin.
Tümleştirme hesaplarındaki montaj miktarları üzerindeki sınırlar için Azure [Mantık Uygulamaları için Sınırlar ve yapılandırma bölümüne](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)bakın.

> [!NOTE]
> Derlemenizi değiştirirseniz, haritada değişiklik olup olmadığını da güncellemelisiniz.

<a name="smaller-assembly"></a>

### <a name="add-assemblies-up-to-2-mb"></a>2 MB'a kadar montajlar ekleme

1. **Montaj Ekle**altında, derlemeniz için bir ad girin. **Küçük dosyayı** seçili tutun. **Derleme** kutusunun yanında klasör simgesini seçin. Örneğin, yüklediğiniz derlemeyi bulun ve seçin:

   ![Daha küçük montaj yükleme](./media/logic-apps-enterprise-integration-maps/upload-assembly-file.png)

   Derleme **Adı** özelliğinde, derlemeyi seçtikten sonra derlemenin dosya adı otomatik olarak görüntülenir.

1. Hazır **olduğunuzda, Tamam'ı**seçin.

   Derleme dosyanız yüklemeyi tamamladıktan sonra derleme **Derlemeler** listesinde görünür.

   ![Yüklenen derlemeler listesi](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies-list.png)

   Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Derlemeler** döşemesi artık yüklenen derlemelerin sayısını gösterir, örneğin:

   ![Yüklenen derlemeler](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies.png)

<a name="larger-assembly"></a>

### <a name="add-assemblies-more-than-2-mb"></a>2 MB'dan fazla derleme ekleme

Daha büyük derlemeler eklemek için, derlemenizi Azure depolama hesabınızdaki bir Azure blob kapsayıcısına yükleyebilirsiniz. Derleme ekleme adımlarınız, blob kapsayıcınızın herkese açık okuma erişimine sahip olup olmadığına bağlı olarak farklılık gösterir. Bu nedenle, önce blob kapsayıcınızın genel okuma erişimi olup olmadığını şu adımları izleyerek kontrol edin: [Blob kapsayıcısı için genel erişim düzeyini ayarlayın](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcı erişim düzeyini denetleme

1. Azure Depolama Gezgini'ni açın. Explorer penceresinde, azure aboneliğinizi zaten genişletilmediyse genişletin.

1. Depolama > **Hesaplarını** genişlet{*depolama hesabınız*} **blob kapları**>. Blob kabınızı seçin.

1. Blob kapsayıcınızın kısayol menüsünden **Genel Erişim Düzeyini Ayarla'yı**seçin.

   * Blob kapsayıcınız en az herkese açık erişime sahipse, **İptal'i**seçin ve bu sayfada aşağıdaki adımları izleyin: [Herkese açık olan kapsayıcılara yükleyin](#public-access-assemblies)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kapsayıcınızın herkese açık erişimi yoksa, **İptal'i**seçin ve bu sayfada aşağıdaki adımları izleyin: [Herkese açık erişim olmadan kapsayıcılara yükleyin](#no-public-access-assemblies)

     ![Herkese açık erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-assemblies"></a>

#### <a name="upload-to-containers-with-public-access"></a>Herkese açık olan kapsayıcılara yükleme

1. Derlemeyi depolama hesabınıza yükleyin. 
   Sağ pencerede **Yükle'yi**seçin.

1. Yüklemeyi bitirdikten sonra, yüklenen derlemenizi seçin. Araç çubuğunda, derlemenin URL'sini kopyalamak için **URL Kopyala'yı** seçin.

1. **Montaj Ekle** bölmesinin açık olduğu Azure portalına dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosyayı seçin (2 MB'dan büyük)**.

   **İçerik URI** kutusu artık **Derleme** kutusu yerine görünür.

1. Content **URI** kutusuna, derlemenizin URL'sini yapıştırın. 
   Montajınızı eklemeyi bitirin.

Derlemeniz yüklemeyi tamamladıktan sonra, şema **Derlemeler** listesinde görünür.
Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Derlemeler** döşemesi artık yüklenen derlemelerin sayısını gösterir.

<a name="no-public-access-assemblies"></a>

#### <a name="upload-to-containers-without-public-access"></a>Herkese açık olmayan kapsayıcılara yükleme

1. Derlemeyi depolama hesabınıza yükleyin. 
   Sağ pencerede **Yükle'yi**seçin.

1. Yüklemeyi tamamladıktan sonra, derlemeniz için paylaşılan bir erişim imzası (SAS) oluşturun. 
   Derlemenizin kısayol menüsünden **Paylaşılan Erişim İmzası**Al'ı'yı seçin.

1. Paylaşılan **Erişim** İmzabayında, **kapsayıcı düzeyinde paylaşılan erişim imzası URI** > **Oluştur'u**oluştur'u seçin. 
   SAS URL'si oluşturulduktan **sonra, URL** kutusunun yanında **Kopyala'yı**seçin.

1. **Montaj Ekle** bölmesinin açık olduğu Azure portalına dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosyayı seçin (2 MB'dan büyük)**.

   **İçerik URI** kutusu artık **Derleme** kutusu yerine görünür.

1. Content **URI** kutusuna, daha önce oluşturduğunuz SAS URI'yi yapıştırın. Montajınızı eklemeyi bitirin.

Derlemeniz yüklemeyi tamamladıktan sonra, derleme **Şemalar** listesinde görünür. Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Derlemeler** döşemesi artık yüklenen derlemelerin sayısını gösterir.

## <a name="create-maps"></a>Haritalar oluşturma

Harita olarak kullanabileceğiniz bir XSLT belgesi oluşturmak için, [Kurumsal Entegrasyon Paketini](logic-apps-enterprise-integration-overview.md)kullanarak Bir BizTalk Tümleştirme projesi oluşturmak için Visual Studio 2015'i kullanabilirsiniz. Bu projede, iki XML şema dosyası arasındaki öğeleri görsel olarak eşlemenize olanak tanıyan bir tümleştirme eşleme dosyası oluşturabilirsiniz. Bu projeyi yaptıktan sonra bir XSLT belgesi alırsınız.
Tümleştirme hesaplarındaki harita miktarları üzerindeki sınırlar [için Azure Mantık Uygulamaları için Sınırlar ve yapılandırma bölümüne](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)bakın. 

## <a name="add-maps"></a>Haritalar ekleme

Haritanızın referans olduğu derlemeleri yükledikten sonra, artık haritanızı yükleyebilirsiniz.

1. Daha önce oturum açmadıysanız, Azure hesap kimlik bilgilerinizle [Azure portalında](https://portal.azure.com) oturum açın. 

1. Tümleştirme hesabınız zaten açık değilse, ana Azure menüsünde **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme hesabı"nı girin. 
   **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Haritanızı eklemek istediğiniz tümleştirme hesabını seçin( örneğin:

   ![Entegrasyon hesabını seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Haritalar** döşemesini seçin.

   !["Haritalar" seçeneğini seçin](./media/logic-apps-enterprise-integration-maps/select-maps.png)

1. **Haritalar** sayfası açıldıktan sonra **Ekle'yi**seçin.

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-maps/add-map.png)  

<a name="smaller-map"></a>

### <a name="add-maps-up-to-2-mb"></a>2 MB'a kadar harita ekleme

1. **Harita Ekle'nin**altında, haritanız için bir ad girin. 

1. **Harita türü**altında, örneğin türü seçin: **Sıvı**, XSLT , **XSLT** **2.0**, veya **XSLT 3.0**.

1. **Küçük dosyayı** seçili tutun. **Harita** kutusunun yanında klasör simgesini seçin. Örneğin, yüklediğiniz haritayı bulun ve seçin:

   ![Haritayı yükle](./media/logic-apps-enterprise-integration-maps/upload-map-file.png)

   **Ad** özelliğini boş bıraktıysanız, harita dosyasını seçtikten sonra haritanın dosya adı otomatik olarak o özellikte görünür. 
   Ancak, herhangi bir benzersiz ad kullanabilirsiniz.

1. Hazır **olduğunuzda, Tamam'ı**seçin. 
   Harita dosyanız yüklemeyi tamamladıktan sonra, harita **Haritalar** listesinde görünür.

   ![Yüklenen haritalar listesi](./media/logic-apps-enterprise-integration-maps/uploaded-maps-list.png)

   Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Haritalar** döşemesi artık yüklenen haritaların sayısını gösterir, örneğin:

   ![Yüklenen haritalar](./media/logic-apps-enterprise-integration-maps/uploaded-maps.png)

<a name="larger-map"></a>

### <a name="add-maps-more-than-2-mb"></a>2 MB'dan fazla harita ekleme

Şu anda, daha büyük haritalar eklemek için [Azure Logic Apps REST API - Haritalar'ı](https://docs.microsoft.com/rest/api/logic/maps/createorupdate)kullanın.

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

Varolan bir haritayı güncelleştirmek için, istediğiniz değişiklikleri içeren yeni bir harita dosyası yüklemeniz gerekir. Ancak, önce düzenleme için varolan haritayı indirebilirsiniz.

1. Azure [portalında,](https://portal.azure.com)zaten açık değilse tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **Tüm hizmetler'i**seçin. Arama kutusuna "tümleştirme hesabı"nı girin. **Tümleştirme hesaplarını**seçin.

1. Haritanızı güncelleştirmek istediğiniz entegrasyon hesabını seçin.

1. Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Haritalar** döşemesini seçin.

1. **Haritalar** sayfası açıldıktan sonra haritanızı seçin. 
   Önce haritayı indirmek ve bunları yapmak için **İndir'i**seçin ve haritayı kaydedin.

1. Güncelleştirilmiş haritayı **Haritalar** sayfasına yüklemeye hazır olduğunuzda, güncelleştirmek istediğiniz haritayı seçin ve **Güncelleştir'i**seçin.

1. Yüklemek istediğiniz güncelleştirilmiş haritayı bulun ve seçin. 
   Harita dosyanız yüklemeyi tamamladıktan sonra, güncelleştirilmiş harita **Haritalar** listesinde görünür.

## <a name="delete-maps"></a>Haritaları silme

1. Azure [portalında,](https://portal.azure.com)zaten açık değilse tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme hesabı"nı girin. 
   **Tümleştirme hesaplarını**seçin.

1. Haritanızı silmek istediğiniz entegrasyon hesabını seçin.

1. Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Haritalar** döşemesini seçin.

1. **Haritalar** sayfası açıldıktan sonra haritanızı seçin ve **Sil'i**seçin.

1. Haritayı silmek istediğinizi doğrulamak için **Evet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md)  
* [Şemalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-schemas.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)
