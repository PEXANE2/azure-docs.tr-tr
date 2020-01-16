---
title: Şemaları kullanarak XML doğrulama
description: Enterprise Integration Pack ile Azure Logic Apps XML belgelerini doğrulamak için şemalar ekleme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/06/2019
ms.openlocfilehash: ade29318fdd1510d03ddd208534258a30b256e98
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979372"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps içindeki şemalarla XML doğrulama

Belgelerin geçerli XML kullanmasını ve Azure Logic Apps ' deki kurumsal tümleştirme senaryoları için önceden tanımlı biçimde beklenen verilerin bulunduğunu denetlemek için mantıksal uygulamanız şemaları kullanabilir. Bir şema, Logic Apps 'in işletmeden işletmeye (B2B) senaryolarında değiş tokuş edilen iletileri de doğrulayabilir.

Tümleştirme hesapları ve şemalar gibi yapıtlarla ilgili sınırlar için, [Azure Logic Apps Için sınırlara ve yapılandırma bilgilerine](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Şemaları ve diğer yapıtları, kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümleri için depoladığınız bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) . 

  Şemanız [2 MB veya daha küçükse](#smaller-schema), şemanızı doğrudan Azure Portal tümleştirme hesabınıza ekleyebilirsiniz. Ancak, şemanız 2 MB 'den büyükse ancak [şema boyutu sınırından](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)büyük değilse, şemanızı bir Azure depolama hesabına yükleyebilirsiniz. 
  Bu şemayı tümleştirme hesabınıza eklemek için, tümleştirme hesabınızdan depolama hesabınıza bağlanabilirsiniz. 
  Bu görev için ihtiyacınız olan öğeler aşağıda verilmiştir: 

  * Şemanız için bir blob kapsayıcısı oluşturduğunuz [Azure depolama hesabı](../storage/common/storage-account-overview.md) . [Depolama hesabı oluşturmayı](../storage/common/storage-account-create.md)öğrenin. 

  * Şemanızı depolamaya yönelik blob kapsayıcısı. [BLOB kapsayıcısı oluşturmayı](../storage/blobs/storage-quickstart-blobs-portal.md)öğrenin. 
  Şemayı tümleştirme hesabınıza eklediğinizde daha sonra kapsayıcının içerik URI 'sine ihtiyacınız vardır.

  * Depolama hesaplarını ve BLOB kapsayıcılarını yönetmek için kullanabileceğiniz [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md). 
  Depolama Gezgini kullanmak için, burada iki seçenekten birini belirleyin:
  
    * Azure portal depolama hesabınızı bulun ve seçin. 
    Depolama hesabı menüsünden **Depolama Gezgini**' yi seçin.

    * Masaüstü sürümü için [Azure Depolama Gezgini indirin ve yükleyin](https://www.storageexplorer.com/). 
    Ardından, [Depolama Gezgini kullanmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md)bölümündeki adımları izleyerek Depolama Gezgini depolama hesabınıza bağlanın. 
    Daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Depolama Gezgini ile nesne depolamada blob oluşturma](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

Şemaları oluştururken ve eklerken bir mantıksal uygulamaya ihtiyacınız yoktur. Ancak, bir şema kullanmak için mantıksal uygulamanız, bu şemayı depoladığınız bir tümleştirme hesabına bağlanması gerekir. [Mantıksal uygulamaları tümleştirme hesaplarına bağlamayı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)öğrenin. Henüz bir Logic App yoksa [Logic Apps oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

## <a name="add-schemas"></a>Şema ekle

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

1. Tümleştirme hesabınızı bulmak ve açmak için, ana Azure menüsünde **tüm hizmetler**' i seçin. Arama kutusuna "tümleştirme hesabı" yazın. **Tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesabını bul](./media/logic-apps-enterprise-integration-schemas/find-integration-account.png)

1. Şemanızı eklemek istediğiniz tümleştirme hesabını seçin, örneğin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-schemas/select-integration-account.png)

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında, **şemalar** kutucuğunu seçin.

   !["Şemalar" ı seçin](./media/logic-apps-enterprise-integration-schemas/select-schemas.png)

1. **Şemalar** sayfası açıldıktan sonra **Ekle**' yi seçin.

   !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-schemas/add-schema.png)

Şema (. xsd) dosyanızın boyutunu temel alarak, [en fazla 2](#smaller-schema) MB veya 2 MB 'ye kadar olan bir ŞEMAYı [8 MB 'a kadar](#larger-schema)karşıya yükleme adımlarını izleyin.

<a name="smaller-schema"></a>

### <a name="add-schemas-up-to-2-mb"></a>2 MB 'a kadar şemalar ekleyin

1. **Şema ekle**' nin altında, şemanız için bir ad girin. 
   **Küçük dosyayı** seçili tut. **Şema** kutusunun yanındaki klasör simgesini seçin. Karşıya yüklediğiniz şemayı bulun ve seçin, örneğin:

   ![Daha küçük şemayı karşıya yükle](./media/logic-apps-enterprise-integration-schemas/upload-smaller-schema-file.png)

1. Hazırsanız **Tamam**' ı seçin.

   Şemanızın karşıya yüklenmesi tamamlandıktan sonra, şema **şemalar** listesinde görünür.

<a name="larger-schema"></a>

### <a name="add-schemas-more-than-2-mb"></a>2 MB 'tan fazla şema ekle

Daha büyük şemalar eklemek için, şemanızı Azure Storage hesabınızdaki bir Azure Blob kapsayıcısına yükleyebilirsiniz. Şemaları ekleme adımlarınız, blob kapsayıcının ortak okuma erişimine sahip olup olmadığına bağlı olarak farklılık gösterir. İlk olarak, aşağıdaki adımları izleyerek blob kabın genel okuma erişimine sahip olup olmadığını kontrol edin: [BLOB kapsayıcısı için genel erişim düzeyini ayarlama](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcı erişim düzeyini denetle

1. Azure Depolama Gezgini açın. Gezgin penceresinde, henüz genişletilmemişse Azure aboneliğinizi genişletin.

1. **Depolama hesapları** > {*Storage-Account*} > **BLOB kapsayıcıları**' nı genişletin. Blob kapsayıcınızı seçin.

1. Blob kapsayıcının kısayol menüsünde, **ortak erişim düzeyini ayarla**' yı seçin.

   * Blob kapsayıcıda en az ortak erişim varsa, **iptal**' i seçin ve bu sayfada daha sonra bu adımları izleyin: [ortak erişimle kapsayıcılara yükleme](#public-access)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kabınızda ortak erişim yoksa, **iptal**' i seçin ve bu sayfada daha sonra bu adımları izleyin: [ortak erişim olmadan kapsayıcılara yükleme](#public-access)

     ![Genel erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access"></a>

#### <a name="upload-to-containers-with-public-access"></a>Ortak erişimle kapsayıcılara yükleme

1. Şemayı depolama hesabınıza yükleyin. 
   Sağ taraftaki pencerede **Yükle**' yi seçin.

1. Karşıya yüklemeyi tamamladıktan sonra, karşıya yüklenen şemayı seçin. Araç çubuğunda, şemanın URL 'sini kopyalamak için **URL 'Yi Kopyala** ' yı seçin.

1. **Şema ekle** bölmesinin açık olduğu Azure Portal geri dönün. 
   Derlemeniz için bir ad girin. 
   **Büyük dosya (2 MB 'tan büyük)** seçeneğini belirleyin. 

   **İçerik URI 'si** kutusu artık **şema** kutusu yerine görüntülenir.

1. **IÇERIK URI** 'si kutusunda, ŞEMANıZıN URL 'sini yapıştırın. 
   Şemanızı ekleme işlemi bitti.

Şemanızın karşıya yüklenmesi tamamlandıktan sonra, şema **şemalar** listesinde görünür. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler** **altında, artık** karşıya yüklenen şemaların sayısını gösterir.

<a name="no-public-access"></a>

#### <a name="upload-to-containers-without-public-access"></a>Ortak erişim olmadan kapsayıcılara yükleme

1. Şemayı depolama hesabınıza yükleyin. 
   Sağ taraftaki pencerede **Yükle**' yi seçin.

1. Karşıya yüklemeyi tamamladıktan sonra, şemanız için bir paylaşılan erişim imzası (SAS) oluşturun. 
   Şemanızın kısayol menüsünde, **paylaşılan erişim Imzası al**' ı seçin.

1. **Paylaşılan erişim imzası** bölmesinde, Oluştur **kapsayıcı düzeyi paylaşılan erişim imzası URI 'si** > **Oluştur**' u seçin. 
   SAS URL 'SI oluşturulduktan sonra, **URL** kutusunun yanındaki **Kopyala**' yı seçin.

1. **Şema ekle** bölmesinin açık olduğu Azure Portal geri dönün. **Büyük dosya**seçin.

   **İçerik URI 'si** kutusu artık **şema** kutusu yerine görüntülenir.

1. **İçerik URI 'si** kutusunda, daha önce oluşturmuş olduğunuz SAS URI 'sini yapıştırın. Şemanızı ekleme işlemi bitti.

Şemanızın karşıya yüklenmesi tamamlandıktan sonra, şema **şemalar** listesinde görünür. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler** **altında, artık** karşıya yüklenen şemaların sayısını gösterir.

## <a name="edit-schemas"></a>Şemaları Düzenle

Mevcut bir şemayı güncelleştirmek için, istediğiniz değişiklikleri içeren yeni bir şema dosyası yüklemeniz gerekir. Ancak, önce varolan şemayı düzenlenmek üzere indirebilirsiniz.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, henüz açık değilse, tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **tüm hizmetler**' i seçin. 
   Arama kutusuna "tümleştirme hesabı" yazın. 
   **Tümleştirme hesapları**' nı seçin.

1. Şemanızı güncelleştirmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında, **şemalar** kutucuğunu seçin.

1. **Şemalar** sayfası açıldıktan sonra şemanızı seçin. 
   Önce şemayı indirmek ve düzenlemek için **İndir**' i seçin ve şemayı kaydedin.

1. Güncelleştirilmiş şemayı karşıya yüklemeye hazırsanız, **şemalar** sayfasında, güncelleştirmek istediğiniz şemayı seçin ve **Güncelleştir**' i seçin.

1. Karşıya yüklemek istediğiniz güncelleştirilmiş şemayı bulun ve seçin. 
   Şema dosyanız karşıya yüklemeyi tamamladıktan sonra, güncelleştirilmiş şema **şemalar** listesinde görüntülenir.

## <a name="delete-schemas"></a>Şemaları Sil

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, henüz açık değilse, tümleştirme hesabınızı bulun ve açın.

1. Ana Azure menüsünde **tüm hizmetler**' i seçin. 
   Arama kutusuna "tümleştirme hesabı" yazın. 
   **Tümleştirme hesapları**' nı seçin.

1. Şemanızı silmek istediğiniz tümleştirme hesabını seçin.

1. Tümleştirme hesabınızın **genel bakış** sayfasında, **Bileşenler**altında, **şemalar** kutucuğunu seçin.

1. **Şemalar** sayfası açıldıktan sonra, şemanızı seçin ve **Sil**' i seçin.

1. Şemayı silmek istediğinizi onaylamak için **Evet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
* [Haritalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)
