---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/27/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: dc12d77e9a57938051300277940b6e5107e45496
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234532"
---
## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

### <a name="azure-roles"></a>Azure rolleri
Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, *katkıda bulunan*, *sahip* veya *yönetici* rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır. Abonelikte sahip olduğunuz izinleri görüntülemek için [Azure Portal](https://portal.azure.com)gidin, sağ üst köşedeki Kullanıcı adınızı seçin, **diğer seçenekler** (...) öğesini seçin ve ardından **izinlerim**' i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin.

Data Factory için veri kümeleri, bağlı hizmetler, işlem hatları, tetikleyiciler ve tümleştirme çalışma zamanları gibi alt kaynaklar oluşturup yönetmek için aşağıdaki gereksinimlerin karşılanması gerekir:

- Azure portalda alt öğe oluşturup yönetebilmek için kaynak grubu düzeyinde **Data Factory Katılımcısı** rolüne veya daha üst düzey bir role sahip olmanız gerekir.
- PowerShell veya SDK ile alt öğe oluşturup yönetebilmek için kaynak düzeyinde **katkıda bulunan** rolü veya daha üst düzey bir rol yeterli olacaktır.

Kullanıcı rolü eklemeye ilişkin örnek yönergeler için [Rol ekleme](../articles/billing/billing-add-change-azure-subscription-administrator.md) makalesine bakın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Data Factory Katılımcısı rolü](../articles/role-based-access-control/built-in-roles.md#data-factory-contributor)
- [Azure Data Factory için roller ve izinler](../articles/data-factory/concepts-roles-permissions.md)

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu hızlı başlangıçta, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hem *kaynak* hem de *hedef* veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../articles/storage/common/storage-quickstart-create-account.md). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu hızlı başlangıçta, Azure depolama hesabınızın adına ve anahtarına ihtiyacınız olacaktır. Aşağıdaki yordamda, depolama hesabınızın adını ve anahtarını alma adımlarını verilmiştir: 

1. Bir Web tarayıcısında [Azure Portal](https://portal.azure.com) gidin ve Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
2. **Tüm hizmetler** >  **** depolamadepolama > **hesapları**' nı seçin.
3. **Depolama hesapları** sayfasında, depolama hesabınız için filtre uygulayın (gerekirse) ve ardından depolama hesabınızı seçin. 
4. *Hesap\<adı >*  - **depolama hesabı** sayfasının kenar çubuğundan ayarlar etiketine gidin ve erişim anahtarları ' nı seçin. Hesap adı > - **erişim tuşları** görünür.  *\<*

   ![Depolama hesabı adını ve anahtarını alma](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. **Depolama hesabı adı** ve **key1** kutularının değerlerini panoya kopyalayın. Bunları Not Defteri’ne veya başka bir düzenleyiciye yapıştırın ve kaydedin. Bunları daha sonra bu hızlı başlangıçta kullanacaksınız.   

#### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma
Bu bölümde, Azure Blob depolama alanında **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz.

1.  >  ****  *Hesap\<adı >*  - erişim tuşları sayfasının kenar çubuğu ' ndan genel bakış**BLOB 'ları**' nı seçin.
2. Hesap adı > **** Bloblar sayfasının araç çubuğunda kapsayıcı ' yı seçin.  *\<*  - 
3. **Yeni kapsayıcı** iletişim kutusunda ad olarak **adftutorial** girin ve ardından **Tamam**’ı seçin. Hesap adı > **** blob 'lar sayfası, kapsayıcılar listesinde adföğreticisi içerecek şekilde güncelleştirilir.  *\<*  - 

   ![Kapsayıcılar listesi](media/data-factory-quickstart-prerequisites/list-of-containers.png)

#### <a name="add-an-input-folder-and-file-for-the-blob-container"></a>Blob kapsayıcısı için bir giriş klasörü ve dosyası ekleyin
Bu bölümde, yeni oluşturduğunuz kapsayıcıda **giriş** adlı bir klasör oluşturur ve ardından giriş klasörüne örnek bir dosya yüklersiniz. Başlamadan önce, **Not defteri**gibi bir metin düzenleyicisini açın ve aşağıdaki **içerikle birlikte bir** dosya Düzenleyicisi oluşturun:

```emp.txt
John, Doe
Jane, Doe
```

Dosyayı **C:\ADFv2QuickStartPSH** klasörüne kaydedin. (Klasör zaten mevcut değilse, oluşturun.) Sonra Azure portal geri dönüp şu adımları izleyin:

1. Kaldığınız yerden -  **** *Hesap adı > Blobları sayfasında, güncelleştirilmiş kapsayıcı listesinden adföğreticisi ' ni seçin. \<* (Pencereyi kapattıysanız veya başka bir sayfaya bir sorun yaşıyorsanız, [Azure Portal](https://portal.azure.com) yeniden oturum açın, **tüm hizmetler** > **depolama** > **depolama hesapları**' nı seçin, depolama hesabınızı seçin ve ardından Bloblar ' ı seçin. **adföğreticisi.)**  > 
2. **Adföğreticisi** kapsayıcı sayfasının araç çubuğunda **karşıya yükle**' yi seçin.
3. **Blobu karşıya yükle** sayfasında, **dosyalar** kutusunu seçin ve ardından, ve sonra da **. txt** dosyasını seçin.
4. **Gelişmiş** başlık ' ı genişletin. Sayfa artık gösterildiği gibi görüntülenir:

   ![Gelişmiş bağlantı seçeneğini belirleme](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
5. **Klasöre yükle** kutusuna **giriş**' i girin.
6. Ardından **Yükle** düğmesini seçin. Listede **emp.txt** dosyasını ve karşıya yükleme durumunu görmeniz gerekir.
7. **BLOB yükle** sayfasını kapatmak için **Kapat** simgesini (bir **X**) seçin.

**Adföğreticisi** kapsayıcı sayfasını açık tutun. Bu hızlı başlangıcın sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız.