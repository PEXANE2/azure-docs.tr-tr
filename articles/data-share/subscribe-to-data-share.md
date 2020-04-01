---
title: 'Öğretici: Veri almak & kabul et - Azure Veri Paylaşımı'
description: Öğretici - Azure Veri Paylaşımı'nı kullanarak veri kabul edin ve alın
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 5b7d9cd7e7d438cf2beac76d5d8bcc78d377a8f4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77083097"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share"></a>Öğretici: Azure Veri Paylaşımı'nı kullanarak veri kabul edin ve alın  

Bu eğitimde, Azure Veri Paylaşımı'nı kullanarak veri paylaşımı davetini nasıl kabul edeceğinizi öğreneceksiniz. Sizinle paylaşılan verileri nasıl alacağınızı ve sizinle paylaşılan verilerin en son anlık görüntüsüne her zaman sahip olmanızı sağlamak için düzenli bir yenileme aralığını nasıl etkinleştireceğinizi öğreneceksiniz. 

> [!div class="checklist"]
> * Azure Veri Paylaşımı davetini kabul etme
> * Azure Veri Paylaşımı hesabı oluşturma
> * Verileriniz için bir hedef belirtin
> * Zamanlanmış yenileme için veri paylaşımınıza abonelik oluşturma

## <a name="prerequisites"></a>Ön koşullar
Veri paylaşımı davetini kabul etmeden önce, aşağıda listelenen bir dizi Azure kaynağı sağlamanız gerekir. 

Veri paylaşımı davetini kabul etmeden önce tüm ön koşulların tamamlandığından emin olun. 

* Azure Aboneliği: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri Paylaşımı daveti: Microsoft Azure'dan "Azure Veri Paylaşımı **<yourdataprovider@domain.com>** daveti" başlıklı bir davet.
* Bir Veri Paylaşımı kaynağı ve hedef Azure veri depolarınızın bulunduğu Azure aboneliğini oluşturacağınız Azure aboneliğinde [Microsoft.DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) kaydedin.

### <a name="receive-data-into-a-storage-account"></a>Bir depolama hesabına veri alın: 

* Azure Depolama hesabı: Zaten hesabınız yoksa, bir Azure [Depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)oluşturabilirsiniz. 
* *Microsoft.Storage/storageAccounts/write*adresinde bulunan depolama hesabına yazma izni. Bu izin Katılımcı rolünde vardır. 
* *Microsoft.Authorization/role assignments/write'da*bulunan depolama hesabına rol ataması ekleme izni. Bu izin Sahibi rolünde vardır.  

### <a name="receive-data-into-a-sql-based-source"></a>Verileri SQL tabanlı bir kaynağa alın:

* *Microsoft.Sql/servers/databases/write*adresinde bulunan SQL sunucusundaki veritabanlarına yazma izni. Bu izin Katılımcı rolünde vardır. 
* Azure SQL Veritabanı'na veya Azure SQL Veri Ambarına erişmek için veri paylaşımı kaynağının yönetilen kimliği için izin. Bu aşağıdaki adımlar la yapılabilir: 
    1. Kendinizi SQL sunucusu için Azure Active Directory Yöneticisi olarak ayarlayın.
    1. Azure Etkin Dizini'ni kullanarak Azure SQL Veritabanı/Veri Ambarı'na bağlanın.
    1. Veri Paylaşımı Yönetilen Kimliği 'db_datareader, db_datawriter, db_ddladmin' olarak eklemek için aşağıdaki komut dosyasını yürütmek için Sorgu Düzenleyicisi'ni (önizleme) kullanın. SQL Server kimlik doğrulaması değil, Active Directory kullanarak bağlanmanız gerekir. 

        ```sql
        create user "<share_acc_name>" from external provider; 
        exec sp_addrolemember db_datareader, "<share_acc_name>"; 
        exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
        exec sp_addrolemember db_ddladmin, "<share_acc_name>";
        ```      
        *<share_acc_name>* Veri Paylaşımı kaynağınızın adı olduğunu unutmayın. Henüz bir Veri Paylaşımı kaynağı oluşturmadıysanız, bu ön koşula daha sonra geri dönebilirsiniz.         

* İstemci IP SQL Server Firewall erişimi. Bu aşağıdaki adımlar la yapılabilir: 
    1. Azure portalındaki SQL sunucusunda *Güvenlik Duvarları'na ve sanal ağlara* gidin
    1. Azure Hizmetlerine erişime izin vermek için **"geçiş"** seçeneğini tıklatın.
    1. **İstemci IP Ekle'yi** ve **Kaydet'i**tıklatın. İstemci IP adresi değiştirilebilir. Azure portalından bir SQL hedefine veri aldığınızda bu işlemin yinelenmesi gerekebilir. Ayrıca bir IP aralığı ekleyebilirsiniz. 


### <a name="receive-data-into-an-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesine veri alın: 

* Veri sağlayıcısının Veri Gezgini kümesiyle aynı Azure veri merkezinde bir Azure Veri Gezgini kümesi: Zaten bir kümeniz yoksa, bir [Azure Veri Gezgini kümesi](https://docs.microsoft.com/azure/data-explorer/create-cluster-database-portal)oluşturabilirsiniz. Veri sağlayıcısının kümesinin Azure veri merkezini bilmiyorsanız, kümeyi daha sonra oluşturabilirsiniz.
* *Microsoft.Kusto/clusters/write'da*bulunan Azure Veri Gezgini kümesine yazma izni. Bu izin Katılımcı rolünde vardır. 
* *Microsoft.Authorization/role assignments/write'da*bulunan Azure Veri Gezgini kümesine rol ataması ekleme izni. Bu izin Sahibi rolünde vardır. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="open-invitation"></a>Açık davet

1. Veri sağlayıcınızdan gelen bir davet için gelen kutunuzu kontrol edin. Davet, Microsoft **Azure'dan, Azure Veri <yourdataprovider@domain.com>Paylaşımı daveti. ** Birden çok davet varsa doğru paylaşımı kabul ettiğinizden emin olmak için paylaşım adını not alın. 

1. Davetinizi Azure'da görmek için **Görünüm davetiyesinde** seçin. Bu sizi Alınan Paylaşımlar görünümüne götürür.

   ![Davetiye](./media/invitations.png "Davetiye listesi") 

1. Görüntülemek istediğiniz paylaşımı seçin. 

## <a name="accept-invitation"></a>Daveti kabul et
1. **Kullanım Koşulları**da dahil olmak üzere tüm alanların gözden geçirildikten emin olun. Kullanım koşullarını kabul ederseniz, kabul ettiğinizi belirtmek için kutuyu işaretlemeniz gerekir. 

   ![Kullanım koşulları](./media/terms-of-use.png "Kullanım koşulları") 

1. *Hedef Veri Paylaşımı Hesabı*altında, Veri Paylaşımınızı dağıtacağınız Abonelik ve Kaynak Grubu'nu seçin. 

   Veri **Paylaşımı Hesabı** alanı için, varolan bir Veri Paylaşımı hesabınız yoksa **yeni oluştur'u** seçin. Aksi takdirde, veri paylaşımınızı kabul etmek istediğiniz varolan bir Veri Paylaşımı hesabını seçin. 

   Alınan **Paylaşım Adı** alanı için, veri sağlama tarafından belirtilen varsayılan ı bırakabilir veya alınan pay için yeni bir ad belirtebilirsiniz. 

   ![Hedef veri paylaşımı hesabı](./media/target-data-share.png "Hedef veri paylaşımı hesabı") 

1. Kullanım koşullarını kabul edip payınız için bir konum belirledikten *sonra, Kabul Et'de Seçin ve yapılandırın.* Bir paylaşım aboneliği oluşturulur.

   Anlık görüntü tabanlı paylaşım için, bir sonraki ekran verilerinizin kopyalanması için bir hedef depolama hesabı seçmeniz ister. 

   ![Seçenekleri kabul et](./media/accept-options.png "Seçenekleri kabul et") 

   Daveti şimdi kabul etmeyi ancak hedef veri deponuzu daha sonra yapılandırmayı tercih ediyorsanız, *kabul et ve daha sonra yapılandır.* Depolama alanınızı daha sonra yapılandırmaya devam etmek için, veri paylaşımı yapılandırmanızı nasıl sürdüreceklerine ilişkin ayrıntılı adımlar için [veri kümesi eşlemeleri sayfasını yapılandırın](how-to-configure-mapping.md) sayfasına bakın. 

   Yerinde paylaşım için, veri paylaşımı yapılandırmanızı nasıl sürdüreceklerine ilişkin ayrıntılı adımlar için [veri kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md) sayfasına bakın. 

   Daveti kabul etmek istemiyorsanız *Reddet'i*seçin. 

## <a name="configure-storage"></a>Depolama alanını yapılandırma
1. *Hedef Depolama Ayarları*altında, verilerinizi almak istediğiniz Abonelik, Kaynak grubu ve depolama hesabını seçin. 

   ![Hedef depolama ayarları](./media/target-storage-settings.png "Hedef depolama") 

1. Verilerinizin düzenli olarak güncelleştirilmesini sağlamak için anlık görüntü ayarlarını etkinleştirdiğinizden emin olun. Yalnızca veri sağlayıcınız veri paylaşımına dahil edilmişse anlık görüntü ayar çizelgesi göreceğinizi unutmayın. 

   ![Anlık görüntü ayarları](./media/snapshot-settings.png "Anlık görüntü ayarları") 

1. *Kaydet'i*seçin. 

> [!IMPORTANT]
> SQL tabanlı veri alıyorsanız ve bu verileri SQL tabanlı bir kaynağa almak istiyorsanız, veri setinizin hedefi olarak bir SQL Server'ı nasıl yapılandıracağınızı öğrenmek için [bir veri kümesi eşleme](how-to-configure-mapping.md) kılavuzunu yapılandırın' ı ziyaret edin. 

## <a name="trigger-a-snapshot"></a>Anlık görüntü tetikleme
Bu adımlar yalnızca anlık görüntü tabanlı paylaşım için geçerlidir.

1. Alınan Paylaşımlar -> Ayrıntılar sekmesinde **Tetikleyici anlık görüntü'nü**seçerek anlık görüntü tetikleyebilirsiniz. Burada, verilerinizin tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. Veri sağlayıcınızdan ilk kez veri alıyorsanız, tam kopyayı seçin. 

   ![Tetikanlık](./media/trigger-snapshot.png "Tetikanlık") 

1. Son çalıştırma durumu *başarılı*olduğunda, alınan verileri görüntülemek için hedef veri deposuna gidin. **Veri Kümeleri'ni**seçin ve Hedef Yol'daki bağlantıyı tıklatın. 

   ![Tüketici veri kümeleri](./media/consumer-datasets.png "Tüketici veri kümesi eşleme") 

## <a name="view-history"></a>Geçmişi görüntüleme
Anlık görüntülerinizin geçmişini görüntülemek için Alınan Paylaşımlar -> Geçmişi'ne gidin. Burada, son 60 gün içinde oluşturulan tüm anlık görüntülerin geçmişini bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, bir Azure Veri Paylaşımını nasıl kabul edin ve alacağınızı öğrendiniz. Azure Veri Paylaşımı kavramları hakkında daha fazla bilgi edinmek için Kavramlara devam [edin: Azure Veri Paylaşımı Terminolojisi.](terminology.md)