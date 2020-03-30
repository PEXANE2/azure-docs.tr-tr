---
title: XML'i şemalarla doğrulayın
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta XML belgelerini doğrulamak için şema ekleme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/06/2019
ms.openlocfilehash: ade29318fdd1510d03ddd208534258a30b256e98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979372"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps’te şemalarla XML’i doğrulama

Belgelerin geçerli XML kullandığını ve Azure Logic Apps'taki kurumsal tümleştirme senaryoları için beklenen verileri önceden tanımlanmış biçimde sahip olup olmadığını denetlemek için, mantık uygulamanız şemaları kullanabilir. Şema, uygulamaların işletmelerarası (B2B) senaryolarında değiş tokuş ettiği iletileri de doğrulayabilir.

Tümleştirme hesapları ve şema gibi yapılarla ilgili sınırlar [için Azure Mantık Uygulamaları için Sınırlar ve yapılandırma bilgilerine](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Şemalarınızı ve diğer yapılarınızı kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümler için depoladığın bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) 

  Şemanız 2 [MB veya daha](#smaller-schema)küçükse, şemanızı doğrudan Azure portalından entegrasyon hesabınıza ekleyebilirsiniz. Ancak, şemanız 2 MB'dan büyükse ancak [şema boyutu sınırından](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)büyük değilse, şemanızı bir Azure depolama hesabına yükleyebilirsiniz. 
  Bu şemayı entegrasyon hesabınıza eklemek için, daha sonra tümleştirme hesabınızdan depolama hesabınıza bağlantı verebilirsiniz. 
  Bu görev için, ihtiyacınız olan öğeler şunlardır: 

  * Şemanız için bir blob kapsayıcısı oluşturduğunuz [Azure depolama hesabı.](../storage/common/storage-account-overview.md) Depolama [hesabı oluşturmayı](../storage/common/storage-account-create.md)öğrenin. 

  * Şema saklamak için blob konteyner. [Blob kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)oluşturmayı öğrenin. 
  Şemayı tümleştirme hesabınıza eklediğinizde, daha sonra kabınızın içeriği URI'ye ihtiyacınız olacaktır.

  * [Depolama](../vs-azure-tools-storage-manage-with-storage-explorer.md)hesaplarını ve blob kapsayıcılarını yönetmek için kullanabileceğiniz Azure Depolama Gezgini. 
  Depolama Gezgini'ni kullanmak için aşağıdaki seçeneklerden birini seçin:
  
    * Azure portalında depolama hesabınızı bulun ve seçin. 
    Depolama hesabı menüsünden **Storage Explorer'ı**seçin.

    * Masaüstü sürümü için [Azure Depolama Gezgini'ni indirin ve yükleyin.](https://www.storageexplorer.com/) 
    Ardından, [Depolama Gezgini ile başlayın'daki](../vs-azure-tools-storage-manage-with-storage-explorer.md)adımları izleyerek Depolama Gezgini'ni depolama hesabınıza bağlayın. 
    Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Depolama Gezgini ile nesne depolama alanında bir leke oluşturun.](../storage/blobs/storage-quickstart-blobs-storage-explorer.md)

Şema oluştururken ve eklerken bir mantık uygulamasına ihtiyacınız yoktur. Ancak, bir şema kullanmak için, mantık uygulamanızın bu şemayı depoladığınız bir tümleştirme hesabına bağlanması gerekir. [Mantık uygulamalarını tümleştirme hesaplarına nasıl bağlayacaklarını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)öğrenin. Henüz bir mantık uygulamanız yoksa, [mantık uygulamaları oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

## <a name="add-schemas"></a>Şema ekleme

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

1. Tümleştirme hesabınızı bulmak ve açmak için ana Azure menüsünde **Tüm hizmetler'i**seçin. Arama kutusuna "tümleştirme hesabı"nı girin. **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-schemas/find-integration-account.png)

1. Şemanızı eklemek istediğiniz tümleştirme hesabını seçin, örneğin:

   ![Entegrasyon hesabını seçin](./media/logic-apps-enterprise-integration-schemas/select-integration-account.png)

1. Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında **Şems** döşemesini seçin.

   !["Şemalar"ı seçin](./media/logic-apps-enterprise-integration-schemas/select-schemas.png)

1. **Şema** sayfası açıldıktan sonra **Ekle'yi**seçin.

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-schemas/add-schema.png)

Şema (.xsd) dosyanızın boyutuna bağlı olarak, [2 MB'a kadar](#smaller-schema) veya 2 MB'dan fazla olan ve [8 MB'a kadar](#larger-schema)olan bir şemayı yüklemek için gereken adımları izleyin.

<a name="smaller-schema"></a>

### <a name="add-schemas-up-to-2-mb"></a>2 MB'a kadar şema ekleme

1. **Şema Ekle'nin**altında, şemanız için bir ad girin. 
   **Küçük dosyayı** seçili tutun. **Şema** kutusunun yanında klasör simgesini seçin. Yüklediğiniz şemayı bulun ve seçin, örneğin:

   ![Daha küçük şema yükleyin](./media/logic-apps-enterprise-integration-schemas/upload-smaller-schema-file.png)

1. Hazır **olduğunuzda, Tamam'ı**seçin.

   Şema yüklemeyi bitirdikten sonra şema **Şemalar** listesinde görünür.

<a name="larger-schema"></a>

### <a name="add-schemas-more-than-2-mb"></a>Şemalar 2 MB'dan fazla ekleyin

Daha büyük şemalar eklemek için şemanızı Azure depolama hesabınızdaki bir Azure blob kapsayıcısına yükleyebilirsiniz. Şema ekleme adımlarınız, blob kapsayıcınızın herkese açık okuma erişimine sahip olup olmadığına bağlı olarak farklılık gösterir. Bu nedenle, önce blob kapsayıcınızın genel okuma erişimi olup olmadığını şu adımları izleyerek kontrol edin: [Blob kapsayıcısı için genel erişim düzeyini ayarlayın](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcı erişim düzeyini denetleme

1. Azure Depolama Gezgini'ni açın. Explorer penceresinde, azure aboneliğinizi zaten genişletilmediyse genişletin.

1. Depolama > **Hesaplarını** genişlet{*depolama hesabınız*} **blob kapları**>. Blob kabınızı seçin.

1. Blob kapsayıcınızın kısayol menüsünden **Genel Erişim Düzeyini Ayarla'yı**seçin.

   * Blob kapsayıcınız en az herkese açık erişime sahipse, **İptal'i**seçin ve bu sayfada aşağıdaki adımları izleyin: [Herkese açık olan kapsayıcılara yükleyin](#public-access)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kapsayıcınızın herkese açık erişimi yoksa, **İptal'i**seçin ve bu sayfada aşağıdaki adımları izleyin: [Herkese açık erişim olmadan kapsayıcılara yükleyin](#public-access)

     ![Herkese açık erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access"></a>

#### <a name="upload-to-containers-with-public-access"></a>Herkese açık olan kapsayıcılara yükleme

1. Şemayı depolama hesabınıza yükleyin. 
   Sağ pencerede **Yükle'yi**seçin.

1. Yüklemeyi bitirdikten sonra, yüklenen şemanızı seçin. Araç çubuğunda, şemonun URL'sini kopyalamak için **URL Kopyala'yı** seçin.

1. **Şema Ekle** bölmesinin açık olduğu Azure portalına dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosyayı seçin (2 MB'dan büyük)**. 

   **Content URI** kutusu artık **Şema** kutusu yerine görünür.

1. Content **URI** kutusuna şemazınızın URL'sini yapıştırın. 
   Şemanızı eklemeyi bitirin.

Şema yüklemeyi bitirdikten sonra şema **Şemalar** listesinde görünür. Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Şema** döşemesi artık yüklenen şema ların sayısını gösterir.

<a name="no-public-access"></a>

#### <a name="upload-to-containers-without-public-access"></a>Herkese açık olmayan kapsayıcılara yükleme

1. Şemayı depolama hesabınıza yükleyin. 
   Sağ pencerede **Yükle'yi**seçin.

1. Yüklemeyi bitirdikten sonra şemanız için paylaşılan bir erişim imzası (SAS) oluşturun. 
   Şemanızın kısayol menüsünden Paylaşılan **Erişim İmzası Al'ı**seçin.

1. Paylaşılan **Erişim** İmzabayında, **kapsayıcı düzeyinde paylaşılan erişim imzası URI** > **Oluştur'u**oluştur'u seçin. 
   SAS URL'si oluşturulduktan **sonra, URL** kutusunun yanında **Kopyala'yı**seçin.

1. **Şema Ekle** bölmesinin açık olduğu Azure portalına dönün. **Büyük dosyayı**seçin.

   **Content URI** kutusu artık **Şema** kutusu yerine görünür.

1. Content **URI** kutusuna, daha önce oluşturduğunuz SAS URI'yi yapıştırın. Şemanızı eklemeyi bitirin.

Şema yüklemeyi bitirdikten sonra şema **Şemalar** listesinde görünür. Entegrasyon hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında, **Şema** döşemesi artık yüklenen şema ların sayısını gösterir.

## <a name="edit-schemas"></a>Şemaları edin

Varolan bir şemayı güncelleştirmek için, istediğiniz değişiklikleri içeren yeni bir şema dosyası yüklemeniz gerekir. Ancak, önce düzenleme için varolan şea indirebilirsiniz.

1. Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>zaten açık değilse tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme hesabı"nı girin. 
   **Tümleştirme hesaplarını**seçin.

1. Şemanızı güncelleştirmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında **Şems** döşemesini seçin.

1. **Şema** sayfası açıldıktan sonra şemanızı seçin. 
   Önce şemayı indirmek ve bunları yapmak için **İndir'i**seçin ve şemayı kaydedin.

1. Güncelleştirilmiş şemayı **Schemas** sayfasına yüklemeye hazır olduğunuzda, güncelleştirmek istediğiniz şemayı seçin ve **Güncelleştir'i**seçin.

1. Yüklemek istediğiniz güncelleştirilmiş şemayı bulun ve seçin. 
   Şema dosyanız yüklemeyi tamamladıktan sonra, güncelleştirilmiş şema **Şemalar** listesinde görünür.

## <a name="delete-schemas"></a>Şemaları silme

1. Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>zaten açık değilse tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme hesabı"nı girin. 
   **Tümleştirme hesaplarını**seçin.

1. Şemanızı silmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **Genel Bakış** sayfasında, **Bileşenler**altında **Şems** döşemesini seçin.

1. **Şems** sayfası açıldıktan sonra şemanızı seçin ve **Sil'i**seçin.

1. Şemayı silmek istediğinizi doğrulamak için **Evet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
* [Haritalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)
