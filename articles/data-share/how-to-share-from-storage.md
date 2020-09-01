---
title: Azure Blob depolama ve Azure Data Lake Storage verileri paylaşma ve alma
description: Azure Blob depolama ve Azure Data Lake Storage verileri paylaşmayı ve almayı öğrenin
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 08/28/2020
ms.openlocfilehash: a54a9d4c50852fe78fd245723dd29f487d58f4b1
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89270256"
---
# <a name="share-and-receive-data-from-azure-blob-storage-and-azure-data-lake-storage"></a>Azure Blob depolama ve Azure Data Lake Storage verileri paylaşma ve alma

[!INCLUDE[appliesto-storage](includes/appliesto-storage.md)]

Azure veri paylaşımı, depolama hesabından anlık görüntü tabanlı paylaşımı destekler. Bu makalede, aşağıdaki kaynaklardan verilerin nasıl paylaşılacağını ve alınacağı açıklanmaktadır: Azure Blob depolama, Azure Data Lake Storage 1. Azure Data Lake Storage 2..

Azure veri paylaşımı, dosya, klasör ve dosya sistemlerinin Azure Data Lake gen1 ve Azure Data Lake Gen2 ' den paylaşılmasını destekler. Ayrıca, Azure Blob depolamadan blob, klasör ve kapsayıcı paylaşımını da destekler. Şu anda yalnızca Blok Blobu destekleniyor. Bu kaynaklardan paylaşılan veriler Azure Data Lake Gen2 veya Azure Blob depolama alanına alınabilir.

Dosya sistemleri, kapsayıcılar veya klasörler anlık görüntü tabanlı paylaşımda paylaşıldığında, veri tüketicisi paylaşma verilerinin tam bir kopyasını yapmayı seçebilir veya yalnızca yeni veya güncelleştirilmiş dosyaları kopyalamak için Artımlı anlık görüntü özelliğinden yararlanın. Artımlı anlık görüntü, dosyaların son değiştirilme saatini temel alır. Aynı ada sahip varolan dosyaların üzerine yazılacak.

## <a name="share-data"></a>Veri paylaşımı

### <a name="prerequisites-to-share-data"></a>Veri paylaşma önkoşulları

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Alıcının Azure oturum açma e-posta adresi (e-posta diğer adlarını kullanarak çalışmaz).
* Kaynak Azure veri deposu, veri paylaşma kaynağı oluşturmak için kullanacağınız sunucudan farklı bir Azure aboneliğinde yer alıyorsa, [Microsoft. DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) Azure veri deposunun bulunduğu abonelikte kaydedin. 

### <a name="prerequisites-for-source-storage-account"></a>Kaynak depolama hesabı önkoşulları

* Azure depolama hesabı: henüz yoksa bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) oluşturabilirsiniz
* *Microsoft. Storage/storageAccounts/Write*içinde bulunan depolama hesabına yazma izni. Bu izin, katkıda bulunan rolünde bulunur.
* *Microsoft. Authorization/role atamalar/Write*' de bulunan depolama hesabına rol ataması ekleme izni. Bu izin sahip rolünde bulunur. 

### <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

### <a name="create-a-data-share-account"></a>Veri paylaşma hesabı oluşturma

Azure Kaynak grubunda bir Azure veri paylaşma kaynağı oluşturun.

1. Portalın sol üst köşesindeki menü düğmesini seçin ve ardından **kaynak oluştur** (+) seçeneğini belirleyin.

1. *Veri paylaşımında*arama yapın.

1. Veri paylaşma ' yı seçin ve **Oluştur**' u seçin.

1. Aşağıdaki bilgilerle Azure veri paylaşma kaynağınızın temel ayrıntılarını doldurun. 

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Veri paylaşma hesabınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-resource-group* | Mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Konum | *Doğu ABD 2* | Veri paylaşma hesabınız için bir bölge seçin.
    | Name | *datashareaccount* | Veri paylaşma hesabınız için bir ad belirtin. |
    | | |

1. Veri paylaşma hesabınızı sağlamak için **gözden geçir + oluştur**' u seçin ve **Oluştur** ' a tıklayın. Yeni bir veri paylaşma hesabının sağlanması genellikle yaklaşık 2 dakika veya daha kısa sürer. 

1. Dağıtım tamamlandığında **Kaynağa Git**' i seçin.

### <a name="create-a-share"></a>Bir paylaşma oluşturun

1. Veri paylaşımında genel bakış sayfasına gidin.

    ![Verilerinizi paylaşma](./media/share-receive-data.png "Verilerinizi paylaşma") 

1. **Verilerinizi paylaşmayı Başlat**' ı seçin.

1. **Oluştur**’u seçin.   

1. Paylaşımınızın ayrıntılarını doldurun. Bir ad, paylaşma türü, içerik paylaşma açıklaması ve kullanım koşulları (isteğe bağlı) belirtin. 

    ![EnterShareDetails](./media/enter-share-details.png "Paylaşma ayrıntılarını girin") 

1. **Devam**’ı seçin.

1. Paylaşımınıza veri kümeleri eklemek için **veri kümesi Ekle**' yi seçin. 

    ![Paylaşımınıza veri kümeleri ekleme](./media/datasets.png "Veri kümeleri")

1. Eklemek istediğiniz veri kümesi türünü seçin. Önceki adımda seçtiğiniz paylaşma türüne (anlık görüntü veya yerinde) bağlı olarak farklı bir veri kümesi türleri listesi görürsünüz. 

    ![Adddataset 'ler](./media/add-datasets.png "Veri kümesi Ekle")    

1. Paylaşmak istediğiniz nesneye gidin ve ' veri kümesi Ekle ' seçeneğini belirleyin. 

    ![Selectdataset 'ler](./media/select-datasets.png "Veri kümelerini seçin")    

1. Alıcılar sekmesinde, ' + Alıcı Ekle ' seçeneğini belirleyerek veri tüketicinizin e-posta adreslerini girin. 

    ![AddRecipients](./media/add-recipient.png "Alıcı ekleme") 

1. **Devam**’ı seçin.

1. Anlık görüntü paylaşma türü ' nu seçtiyseniz, veri tüketicisine verilerinizin güncelleştirmelerini sağlamak için anlık görüntü zamanlamasını yapılandırabilirsiniz. 

    ![EnableSnapshots](./media/enable-snapshots.png "Anlık görüntüleri etkinleştir") 

1. Bir başlangıç saati ve yinelenme aralığı seçin. 

1. **Devam**’ı seçin.

1. Gözden geçir + Oluştur sekmesinde paket Içeriklerinizi, ayarlarınızı, alıcılarını ve eşitleme ayarlarınızı gözden geçirin. **Oluştur**’u seçin.

Azure veri paylaşımınız artık oluşturulmuştur ve veri paylaşımınızın alıcısı artık davetinizi kabul etmeye hazırdır. 

## <a name="receive-data"></a>Veri Al

### <a name="prerequisites-to-receive-data"></a>Veri alma önkoşulları
Bir veri paylaşma davetini kabul etmeden önce, aşağıda listelenen birkaç Azure kaynağı sağlamalısınız. 

Bir veri paylaşma davetini kabul etmeden önce tüm önkoşulların tümünün tamamlandığından emin olun. 

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri paylaşma daveti: "Azure Data Share davetiyesi" başlıklı konuyla bir Microsoft Azure daveti **<yourdataprovider@domain.com>** .
* [Microsoft. DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) , bir veri paylaşma kaynağı ve hedef Azure veri mağazalarınızın bulunduğu Azure aboneliğini oluşturacağınız Azure aboneliğine kaydedin.

### <a name="prerequisites-for-target-storage-account"></a>Hedef depolama hesabı önkoşulları

* Azure depolama hesabı: henüz yoksa bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)oluşturabilirsiniz. 
* *Microsoft. Storage/storageAccounts/Write*içinde bulunan depolama hesabına yazma izni. Bu izin, katkıda bulunan rolünde bulunur. 
* *Microsoft. Authorization/role atamalar/Write*' de bulunan depolama hesabına rol ataması ekleme izni. Bu izin sahip rolünde bulunur.  

### <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

### <a name="open-invitation"></a>Daveti aç

1. Daveti e-postayla veya doğrudan Azure portal açabilirsiniz. 

   E-postadaki daveti açmak için, veri sağlayıcınızdan bir davet için gelen kutunuza bakın. Davet, ' **den <yourdataprovider@domain.com> Azure veri paylaşma daveti **başlıklı Microsoft Azure. Azure 'da davetinizi görmek için **daveti görüntüle** ' ye tıklayın. 

   Daveti doğrudan Azure portal açmak için Azure portal **veri paylaşma davetlerini** arayın. Bu, sizi veri paylaşma davetlerinin listesine götürür.

   ![Davetlerin listesi](./media/invitations.png "Davetlerin listesi") 

1. Görüntülemek istediğiniz paylaşma seçeneğini belirleyin. 

### <a name="accept-invitation"></a>Daveti kabul et
1. **Kullanım koşulları**dahil olmak üzere tüm alanların incelendiğini doğrulayın. Kullanım koşullarını kabul ediyorsanız, kabul ettiğinizi belirtmek için kutuyu denetlemeniz gerekir. 

   ![Kullanım koşulları](./media/terms-of-use.png "Kullanım koşulları") 

1. *Hedef veri paylaşma hesabı*altında, veri paylaşımınızı dağıtmak istediğiniz aboneliği ve kaynak grubunu seçin. 

   **Veri** paylaşma hesabı alanı için, mevcut bir veri paylaşma hesabınız yoksa **Yeni oluştur** ' u seçin. Aksi takdirde, veri paylaşımınızı kabul etmek istediğiniz mevcut bir veri paylaşma hesabı seçin. 

   **Alınan paylaşma adı** alanı için, veriler tarafından belirtilen varsayılanı bırakabilir veya alınan paylaşıma yönelik yeni bir ad belirtebilirsiniz. 

   Kullanım koşullarını kabul ettikten ve alınan paylaşımınızı yönetmek için bir veri paylaşma hesabı belirttikten sonra **kabul et ve Yapılandır**' ı seçin. Bir paylaşma aboneliği oluşturulacak. 

   ![Kabul etme seçenekleri](./media/accept-options.png "Kabul etme seçenekleri") 

   Bu sizi, veri paylaşma hesabınızda alınan paylaşımınızdan size götürür. 

   Daveti kabul etmek istemiyorsanız *Reddet*' i seçin. 

### <a name="configure-received-share"></a>Alınan paylaşma yapılandırma
Verileri almak istediğiniz yeri yapılandırmak için aşağıdaki adımları izleyin.

1. **Veri kümeleri** sekmesini seçin. Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin. Hedef veri deposu seçmek için **+ hedefe eşle** öğesini seçin. 

   ![Hedefle eşle](./media/dataset-map-target.png "Hedefle eşle") 

1. Verilerin içinde olmasını istediğiniz bir hedef veri deposu seçin. Hedef veri deposundaki aynı yol ve ada sahip tüm veri dosyalarının üzerine yazılacak. 

   ![Hedef depolama hesabı](./media/map-target.png "Hedef depolama") 

1. Anlık görüntü tabanlı paylaşım için, veri sağlayıcısı verilere düzenli güncelleştirme sağlamak üzere bir anlık görüntü zamanlaması oluşturmışsa, anlık görüntü **zamanlama** sekmesini seçerek de anlık görüntü zamanlamasını etkinleştirebilirsiniz. Anlık görüntü zamanlamasının yanındaki kutuyu işaretleyin ve **+ Etkinleştir**' i seçin.

   ![Anlık görüntü zamanlamasını etkinleştir](./media/enable-snapshot-schedule.png "Anlık görüntü zamanlamasını etkinleştir")

### <a name="trigger-a-snapshot"></a>Anlık görüntü tetikleyin
Bu adımlar yalnızca anlık görüntü tabanlı paylaşım için geçerlidir.

1. **Ayrıntılar** sekmesini ve ardından **tetikleyici anlık görüntüsünü**seçerek bir anlık görüntü tetikleyebilirsiniz. Burada, verilerinizin tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. Veri sağlayıcınızdan ilk kez veri alıyorsanız tam kopya ' ı seçin. 

   ![Görüntüyü Tetikle](./media/trigger-snapshot.png "Görüntüyü Tetikle") 

1. Son çalıştırma durumu *başarılı*olduğunda, alınan verileri görüntülemek için hedef veri deposuna gidin. **Veri kümeleri**' ni seçin ve Hedef yoldaki bağlantıya tıklayın. 

   ![Tüketici veri kümeleri](./media/consumer-datasets.png "Tüketici veri kümesi eşleme") 

### <a name="view-history"></a>Geçmişi görüntüleme
Bu adım yalnızca anlık görüntü tabanlı paylaşım için geçerlidir. Anlık görüntülerinizin geçmişini görüntülemek için **Geçmiş** sekmesini seçin. Burada, son 30 gün için oluşturulan tüm anlık görüntülerin geçmişini bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar
Azure veri paylaşma hizmeti 'ni kullanarak depolama hesabından nasıl veri alacağınızı ve verilerin alınacağını öğrendiniz. Diğer veri kaynaklarından paylaşım hakkında daha fazla bilgi edinmek için [desteklenen veri depolarına](supported-data-stores.md)devam edin.

