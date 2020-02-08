---
title: 'Öğretici: & alma verileri kabul etme-Azure veri paylaşma'
description: Öğretici-Azure veri paylaşımının kullanıldığı verileri kabul edin ve alın
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 5b7d9cd7e7d438cf2beac76d5d8bcc78d377a8f4
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77083097"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share"></a>Öğretici: Azure veri paylaşımının kullanıldığı verileri kabul edin ve alın  

Bu öğreticide, Azure veri paylaşma kullanarak bir veri paylaşımının davetini kabul etme hakkında bilgi edineceksiniz. Sizinle paylaşılan verilerin nasıl alınacağını ve her zaman sizinle paylaşılan verilerin en son anlık görüntüsüne sahip olduğunuzdan emin olmak için düzenli yenileme aralığını nasıl etkinleştireceğinizi öğreneceksiniz. 

> [!div class="checklist"]
> * Azure Veri Paylaşma davetini kabul etme
> * Azure veri paylaşma hesabı oluşturma
> * Verileriniz için bir hedef belirtin
> * Zamanlanmış yenileme için veri paylaşımınıza abonelik oluşturma

## <a name="prerequisites"></a>Önkoşullar
Bir veri paylaşma davetini kabul etmeden önce, aşağıda listelenen birkaç Azure kaynağı sağlamalısınız. 

Bir veri paylaşma davetini kabul etmeden önce tüm önkoşulların tümünün tamamlandığından emin olun. 

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri paylaşma daveti: "Azure Data Share davetiyesi **<yourdataprovider@domain.com>** " başlıklı konuyla bir Microsoft Azure daveti.
* [Microsoft. DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) , bir veri paylaşma kaynağı ve hedef Azure veri mağazalarınızın bulunduğu Azure aboneliğini oluşturacağınız Azure aboneliğine kaydedin.

### <a name="receive-data-into-a-storage-account"></a>Bir depolama hesabına veri alma: 

* Azure depolama hesabı: henüz yoksa bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)oluşturabilirsiniz. 
* *Microsoft. Storage/storageAccounts/Write*içinde bulunan depolama hesabına yazma izni. Bu izin, katkıda bulunan rolünde bulunur. 
* *Microsoft. Authorization/role atamalar/Write*' de bulunan depolama hesabına rol ataması ekleme izni. Bu izin sahip rolünde bulunur.  

### <a name="receive-data-into-a-sql-based-source"></a>SQL tabanlı bir kaynağa veri alma:

* *Microsoft. SQL/Servers/veritabanları/Write*'TA bulunan SQL Server 'da veritabanlarına yazma izni. Bu izin, katkıda bulunan rolünde bulunur. 
* Azure SQL veritabanı veya Azure SQL veri ambarı 'na erişmek için veri paylaşımının yönetilen kimliği izni. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Kendinizi SQL Server için Azure Active Directory yöneticisi olarak ayarlayın.
    1. Azure Active Directory kullanarak Azure SQL veritabanı/veri ambarına bağlanın.
    1. Veri paylaşımının yönetilen kimliğini bir ' db_datareader, db_datawriter, db_ddladmin ' olarak eklemek için aşağıdaki betiği yürütmek üzere sorgu Düzenleyicisi 'ni (Önizleme) kullanın. SQL Server kimlik doğrulaması değil Active Directory kullanarak bağlanmanız gerekir. 

        ```sql
        create user "<share_acc_name>" from external provider; 
        exec sp_addrolemember db_datareader, "<share_acc_name>"; 
        exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
        exec sp_addrolemember db_ddladmin, "<share_acc_name>";
        ```      
        *< Share_acc_name >* veri paylaşma kaynağınızın adı olduğunu unutmayın. Henüz bir veri paylaşma kaynağı oluşturmadıysanız, bu ön koşul daha sonra geri dönebilirsiniz.         

* İstemci IP SQL Server güvenlik duvarı erişimi. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal 'deki SQL Server 'da *güvenlik duvarları ve sanal ağlar* ' a gidin.
    1. Azure hizmetlerine erişime izin vermek **için Aç düğmesine** tıklayın.
    1. **+ ISTEMCI IP Ekle** ' ye tıklayın ve **Kaydet**' e tıklayın. İstemci IP adresi değişebilir. Bu işlemin bir SQL hedefine Azure portal bir dahaki sefer veri alıyorsanız tekrarlanması gerekebilir. Ayrıca, bir IP aralığı ekleyebilirsiniz. 


### <a name="receive-data-into-an-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesine veri alma: 

* Veri sağlayıcısının Veri Gezgini kümesiyle aynı Azure veri merkezindeki bir Azure Veri Gezgini kümesi: henüz yoksa bir [azure Veri Gezgini kümesi](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal)oluşturabilirsiniz. Veri sağlayıcısının kümesinin Azure veri merkezini bilmiyorsanız, kümeyi daha sonra işlem içinde oluşturabilirsiniz.
* *Microsoft. kusto/kümeler/Write*Içinde bulunan Azure Veri Gezgini kümesine yazma izni. Bu izin, katkıda bulunan rolünde bulunur. 
* *Microsoft. Authorization/role atamalar/Write*' de bulunan Azure Veri Gezgini kümesine rol ataması ekleme izni. Bu izin sahip rolünde bulunur. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/)’ında oturum açın.

## <a name="open-invitation"></a>Daveti aç

1. Veri sağlayıcınızdan bir davet için gelen kutunuzu denetleyin. Davet, **<yourdataprovider@domain.com>Azure Veri Paylaşma davetini** başlıklı Microsoft Azure. Birden çok davetiye varsa doğru paylaşımın kabul edildiğinizden emin olmak için, paylaşılan adı ' nı bir yere göz atın. 

1. Davetinizi Azure 'da görmek için **daveti görüntüle** ' yi seçin. Bu sizi aldığınız paylaşımlar görünümüne götürür.

   ![Lerinin](./media/invitations.png "Davetlerin listesi") 

1. Görüntülemek istediğiniz paylaşma seçeneğini belirleyin. 

## <a name="accept-invitation"></a>Daveti kabul et
1. **Kullanım koşulları**dahil olmak üzere tüm alanların incelendiğini doğrulayın. Kullanım koşullarını kabul ediyorsanız, kabul ettiğinizi belirtmek için kutuyu denetlemeniz gerekir. 

   ![Kullanım koşulları](./media/terms-of-use.png "Kullanım koşulları") 

1. *Hedef veri paylaşma hesabı*altında, veri paylaşımınızı dağıtmak istediğiniz aboneliği ve kaynak grubunu seçin. 

   **Veri** paylaşma hesabı alanı için, mevcut bir veri paylaşma hesabınız yoksa **Yeni oluştur** ' u seçin. Aksi takdirde, veri paylaşımınızı kabul etmek istediğiniz mevcut bir veri paylaşma hesabı seçin. 

   **Alınan paylaşma adı** alanı için, veriler tarafından belirtilen varsayılanı bırakabilir veya alınan paylaşıma yönelik yeni bir ad belirtebilirsiniz. 

   ![Hedef veri paylaşma hesabı](./media/target-data-share.png "Hedef veri paylaşma hesabı") 

1. Kullanım koşullarını kabul ettikten ve paylaşımınız için bir konum belirttikten sonra *kabul et ve Yapılandır*' ı seçin. Bir paylaşma aboneliği oluşturulacak.

   Anlık görüntü tabanlı paylaşım için bir sonraki ekranda, verilerinize kopyalanacak bir hedef depolama hesabı seçmeniz istenir. 

   ![Kabul etme seçenekleri](./media/accept-options.png "Kabul etme seçenekleri") 

   Şimdi daveti kabul etmek ancak hedef veri deponuzu daha sonra yapılandırmak isterseniz, *kabul et ve Yapılandır*' ı seçin. Depolama alanınızı daha sonra yapılandırmaya devam etmek için veri paylaşma yapılandırmanızı nasıl sürdürüleceği hakkında ayrıntılı adımlar için bkz. veri [kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md) sayfası. 

   Yerinde paylaşım için, veri paylaşımı yapılandırmanızı nasıl sürdürüleceği hakkında ayrıntılı adımlar için bkz. veri [kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md) sayfası. 

   Daveti kabul etmek istemiyorsanız *Reddet*' i seçin. 

## <a name="configure-storage"></a>Depolama alanını yapılandırma
1. *Hedef depolama ayarları*altında, verilerinizi almak istediğiniz aboneliği, kaynak grubunu ve depolama hesabını seçin. 

   ![Hedef depolama ayarları](./media/target-storage-settings.png "Hedef depolama") 

1. Verilerinizin düzenli olarak güncelleştirilmesini sağlamak için, anlık görüntü ayarlarını etkinleştirdiğinizden emin olun. Veri sağlayıcınız onu veri paylaşımında içerdiğine yalnızca bir anlık görüntü ayarı görürsünüz. 

   ![Anlık görüntü ayarları](./media/snapshot-settings.png "Anlık görüntü ayarları") 

1. *Kaydet*’i seçin. 

> [!IMPORTANT]
> SQL tabanlı verileri alıyorsanız ve bu verileri SQL tabanlı bir kaynağa almak istiyorsanız, veri kümeniz için hedef olarak SQL Server yapılandırmayı öğrenmek üzere [bir veri kümesi eşleme](how-to-configure-mapping.md) nasıl yapılır Kılavuzu ' nu ziyaret edin. 

## <a name="trigger-a-snapshot"></a>Anlık görüntü tetikleyin
Bu adımlar yalnızca anlık görüntü tabanlı paylaşım için geçerlidir.

1. Alınan paylaşımlar-> Ayrıntılar sekmesinde bir anlık görüntüyü **tetikleyici anlık görüntüsünü**seçerek tetikleyebilirsiniz. Burada, verilerinizin tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. Veri sağlayıcınızdan ilk kez veri alıyorsanız tam kopya ' ı seçin. 

   ![Görüntüyü Tetikle](./media/trigger-snapshot.png "Görüntüyü Tetikle") 

1. Son çalıştırma durumu *başarılı*olduğunda, alınan verileri görüntülemek için hedef veri deposuna gidin. **Veri kümeleri**' ni seçin ve Hedef yoldaki bağlantıya tıklayın. 

   ![Tüketici veri kümeleri](./media/consumer-datasets.png "Tüketici veri kümesi eşleme") 

## <a name="view-history"></a>Geçmişi görüntüleme
Anlık görüntülerinizin geçmişini görüntülemek için, alınan paylaşımlar-> geçmişi ' ne gidin. Burada, son 60 gün için oluşturulan tüm anlık görüntülerin geçmişini bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Azure veri paylaşımının nasıl kabul edileceği ve alınacağı hakkında daha fazla öğrendiniz. Azure veri paylaşımında kavramlar hakkında daha fazla bilgi edinmek için bkz. [Azure veri paylaşma terminolojisi](terminology.md).