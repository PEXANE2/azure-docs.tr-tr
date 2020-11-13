---
title: 'Öğretici: & alma verileri kabul etme-Azure veri paylaşma'
description: Öğretici-Azure veri paylaşımının kullanıldığı verileri kabul edin ve alın
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: tutorial
ms.date: 11/12/2020
ms.openlocfilehash: 17c3e9ee157cedd31be39f472f619a2df9ae32a6
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94594190"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share"></a>Öğretici: Azure Veri Paylaşımı’nı kullanarak veri kabul etme ve alma  

Bu öğreticide, Azure veri paylaşma kullanarak bir veri paylaşımının davetini kabul etme hakkında bilgi edineceksiniz. Sizinle paylaşılan verilerin nasıl alınacağını ve her zaman sizinle paylaşılan verilerin en son anlık görüntüsüne sahip olduğunuzdan emin olmak için düzenli yenileme aralığını nasıl etkinleştireceğinizi öğreneceksiniz. 

> [!div class="checklist"]
> * Azure Veri Paylaşma davetini kabul etme
> * Azure veri paylaşma hesabı oluşturma
> * Verileriniz için bir hedef belirtin
> * Zamanlanmış yenileme için veri paylaşımınıza abonelik oluşturma

## <a name="prerequisites"></a>Ön koşullar
Bir veri paylaşma davetini kabul etmeden önce, aşağıda listelenen birkaç Azure kaynağı sağlamalısınız. 

Bir veri paylaşma davetini kabul etmeden önce tüm önkoşulların tümünün tamamlandığından emin olun. 

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Veri paylaşma daveti: "Azure Data Share davetiyesi" başlıklı konuyla bir Microsoft Azure daveti **<yourdataprovider@domain.com>** .
* [Microsoft. DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) , bir veri paylaşma kaynağı ve hedef Azure veri mağazalarınızın bulunduğu Azure aboneliğini oluşturacağınız Azure aboneliğine kaydedin.

### <a name="receive-data-into-a-storage-account"></a>Bir depolama hesabına veri alma

* Azure depolama hesabı: henüz yoksa bir [Azure depolama hesabı](../storage/common/storage-account-create.md)oluşturabilirsiniz. 
* *Microsoft. Storage/storageAccounts/Write* içinde bulunan depolama hesabına yazma izni. Bu izin Katkıda Bulunan rolünde vardır. 
* *Microsoft. Authorization/role atamalar/Write* ' de bulunan depolama hesabına rol ataması ekleme izni. Bu izin Sahip rolünde vardır.  

### <a name="receive-data-into-a-sql-based-target"></a>SQL tabanlı bir hedefte veri alma
Azure SQL veritabanı 'na veri almayı seçerseniz Azure SYNAPSE Analytics, önkoşul listesidir. 

#### <a name="prerequisites-for-receiving-data-into-azure-sql-database-or-azure-synapse-analytics-formerly-azure-sql-dw"></a>Azure SQL veritabanı veya Azure SYNAPSE Analytics 'e veri alma önkoşulları (eski adıyla Azure SQL DW)
Önkoşulları yapılandırmak için [adım adım demo](https://youtu.be/aeGISgK1xro) ' i takip edebilirsiniz.

* Azure SQL veritabanı veya Azure SYNAPSE Analytics (eski adıyla Azure SQL DW).
* *Microsoft. SQL/Servers/veritabanları/Write* 'TA bulunan SQL Server 'da veritabanlarına yazma izni. Bu izin **Katkıda Bulunan** rolünde vardır. 
* Azure SQL veritabanı veya Azure SYNAPSE Analytics 'e erişmek için veri paylaşımının yönetilen kimliği izni. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal, SQL Server 'a gidin ve kendiniz **Azure Active Directory Yöneticisi** olarak ayarlayın.
    1. [Sorgu Düzenleyicisi](../azure-sql/database/connect-query-portal.md#connect-using-azure-active-directory) 'Ni kullanarak Azure SQL veritabanı/veri ambarı 'na bağlanın veya Azure Active Directory kimlik doğrulamasıyla SQL Server Management Studio. 
    1. Veri paylaşımının yönetilen kimliğini bir ' db_datareader, db_datawriter, db_ddladmin ' olarak eklemek için aşağıdaki betiği yürütün. SQL Server kimlik doğrulaması değil Active Directory kullanarak bağlanmanız gerekir. 

        ```sql
        create user "<share_acc_name>" from external provider; 
        exec sp_addrolemember db_datareader, "<share_acc_name>"; 
        exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
        exec sp_addrolemember db_ddladmin, "<share_acc_name>";
        ```      
        *<share_acc_name>* veri paylaşma kaynağınızın adı olduğunu unutmayın. Henüz bir veri paylaşma kaynağı oluşturmadıysanız, bu ön koşul daha sonra geri dönebilirsiniz.         

* Güvenlik duvarı erişimi SQL Server. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal 'deki SQL Server 'da *güvenlik duvarları ve sanal ağlar* ' a gidin.
    1. *Azure hizmetlerinin ve kaynaklarının bu sunucuya erişmesine Izin vermek* için **Evet** ' i tıklatın.
    1. **+ İstemci IP 'Si Ekle** ' ye tıklayın. İstemci IP adresi değişebilir. Bu işlemin bir sonraki Azure portal SQL verilerini paylaşışınızda tekrarlanması gerekebilir. Ayrıca, bir IP aralığı ekleyebilirsiniz.
    1. **Kaydet** ’e tıklayın. 
 
#### <a name="prerequisites-for-receiving-data-into-azure-synapse-analytics-workspace-sql-pool"></a>Azure SYNAPSE Analytics (çalışma alanı) SQL havuzu 'na veri alma önkoşulları

* Bir Azure SYNAPSE Analytics (çalışma alanı) adanmış SQL Havuzu. Sunucusuz SQL havuzunda verilerin alınması Şu anda desteklenmiyor.
* *Microsoft. SYNAPSE/Workspaces/Sqlhavuzlar/Write* Içinde bulunan SYNAPSE çalışma alanındaki SQL havuzuna yazma izni. Bu izin **Katkıda Bulunan** rolünde vardır.
* SYNAPSE çalışma alanı SQL havuzuna erişmek için veri paylaşımının yönetilen kimliği izni. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal ' de SYNAPSE çalışma alanı ' na gidin. Sol gezinmede SQL Active Directory Yöneticisi ' ni seçin ve kendiniz **Azure Active Directory Yöneticisi** olarak ayarlayın.
    1. SYNAPSE Studio 'yu açın, sol gezinmede *Yönet* ' i seçin. Güvenlik altında *erişim denetimi* ' ni seçin. Kendinize **SQL Yöneticisi** veya **çalışma alanı yönetici** rolü atayın.
    1. SYNAPSE Studio 'da sol gezinmeden *Geliştir* ' i seçin. Veri paylaşımının kaynak yönetimli kimliğini bir ' db_datareader, db_datawriter, db_ddladmin ' olarak eklemek için SQL havuzunda aşağıdaki betiği yürütün. 
    
        ```sql
        create user "<share_acc_name>" from external provider; 
        exec sp_addrolemember db_datareader, "<share_acc_name>"; 
        exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
        exec sp_addrolemember db_ddladmin, "<share_acc_name>";
        ```                   
       *<share_acc_name>* veri paylaşma kaynağınızın adı olduğunu unutmayın. Henüz bir veri paylaşma kaynağı oluşturmadıysanız, bu ön koşul daha sonra geri dönebilirsiniz.  

* SYNAPSE çalışma alanı güvenlik duvarı erişimi. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal ' de SYNAPSE çalışma alanı ' na gidin. Sol gezinmede *güvenlik duvarları* ' nı seçin.
    1. *Azure hizmetlerinin ve kaynaklarının bu çalışma alanına erişmesine Izin vermek* için **Açık** ' a tıklayın.
    1. **+ İstemci IP 'Si Ekle** ' ye tıklayın. İstemci IP adresi değişebilir. Bu işlemin bir sonraki Azure portal SQL verilerini paylaşışınızda tekrarlanması gerekebilir. Ayrıca, bir IP aralığı ekleyebilirsiniz.
    1. **Kaydet** ’e tıklayın. 

### <a name="receive-data-into-an-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesine veri alma: 

* Veri sağlayıcısının Veri Gezgini kümesiyle aynı Azure veri merkezindeki bir Azure Veri Gezgini kümesi: henüz yoksa bir [azure Veri Gezgini kümesi](/azure/data-explorer/create-cluster-database-portal)oluşturabilirsiniz. Veri sağlayıcısının kümesinin Azure veri merkezini bilmiyorsanız, kümeyi daha sonra işlem içinde oluşturabilirsiniz.
* *Microsoft. kusto/kümeler/Write* Içinde bulunan Azure Veri Gezgini kümesine yazma izni. Bu izin Katkıda Bulunan rolünde vardır. 
* *Microsoft. Authorization/role atamalar/Write* ' de bulunan Azure Veri Gezgini kümesine rol ataması ekleme izni. Bu izin Sahip rolünde vardır. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="open-invitation"></a>Daveti aç

1. Daveti e-postayla veya doğrudan Azure portal açabilirsiniz. 

   E-postadaki daveti açmak için, veri sağlayıcınızdan bir davet için gelen kutunuza bakın. Davet, ' **den <yourdataprovider@domain.com> Azure veri paylaşma daveti** başlıklı Microsoft Azure. Azure 'da davetinizi görmek için **daveti görüntüle** ' ye tıklayın. 

   Daveti doğrudan Azure portal açmak için Azure portal **veri paylaşma davetlerini** arayın. Bu, sizi veri paylaşma davetlerinin listesine götürür.

   ![Davetlerin listesi](./media/invitations.png "Davetlerin listesi") 

1. Görüntülemek istediğiniz paylaşma seçeneğini belirleyin. 

## <a name="accept-invitation"></a>Daveti kabul et
1. **Kullanım koşulları** dahil olmak üzere tüm alanların incelendiğini doğrulayın. Kullanım koşullarını kabul ediyorsanız, kabul ettiğinizi belirtmek için kutuyu denetlemeniz gerekir. 

   ![Kullanım koşulları](./media/terms-of-use.png "Kullanım koşulları") 

1. *Hedef veri paylaşma hesabı* altında, veri paylaşımınızı dağıtmak istediğiniz aboneliği ve kaynak grubunu seçin. 

   **Veri** paylaşma hesabı alanı için, mevcut bir veri paylaşma hesabınız yoksa **Yeni oluştur** ' u seçin. Aksi takdirde, veri paylaşımınızı kabul etmek istediğiniz mevcut bir veri paylaşma hesabı seçin. 

   **Alınan paylaşma adı** alanı için, veriler tarafından belirtilen varsayılanı bırakabilir veya alınan paylaşıma yönelik yeni bir ad belirtebilirsiniz. 

   Kullanım koşullarını kabul ettikten ve alınan paylaşımınızı yönetmek için bir veri paylaşma hesabı belirttikten sonra **kabul et ve Yapılandır** ' ı seçin. Bir paylaşma aboneliği oluşturulacak. 

   ![Kabul etme seçenekleri](./media/accept-options.png "Kabul etme seçenekleri") 

   Bu sizi, veri paylaşma hesabınızda alınan paylaşımınızdan size götürür. 

   Daveti kabul etmek istemiyorsanız *Reddet* ' i seçin. 

## <a name="configure-received-share"></a>Alınan paylaşma yapılandırma
Verileri almak istediğiniz yeri yapılandırmak için aşağıdaki adımları izleyin.

1. **Veri kümeleri** sekmesini seçin. Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin. Hedef veri deposu seçmek için **+ hedefe eşle** öğesini seçin. 

   ![Hedefle eşle](./media/dataset-map-target.png "Hedefle eşle") 

1. Verilerin içinde olmasını istediğiniz hedef veri deposu türünü seçin. Hedef veri deposundaki tüm veri dosyaları veya tablolar aynı yol ve ada sahip olacak şekilde üzerine yazılır. 

   Yerinde paylaşım için, belirtilen konumda bir veri deposu seçin. Konum, veri sağlayıcısının kaynak veri deposunun bulunduğu Azure veri merkezindedir. Veri kümesi eşlendikten sonra, verilere erişmek için Hedef yoldaki bağlantıyı takip edebilirsiniz.

   ![Hedef depolama hesabı](./media/dataset-map-target-sql.png "Hedef depolama") 

1. Anlık görüntü tabanlı paylaşım için, veri sağlayıcısı verilere düzenli güncelleştirme sağlamak üzere bir anlık görüntü zamanlaması oluşturmışsa, anlık görüntü **zamanlama** sekmesini seçerek de anlık görüntü zamanlamasını etkinleştirebilirsiniz. Anlık görüntü zamanlamasının yanındaki kutuyu işaretleyin ve **+ Etkinleştir** ' i seçin.

   ![Anlık görüntü zamanlamasını etkinleştir](./media/enable-snapshot-schedule.png "Anlık görüntü zamanlamasını etkinleştir")

## <a name="trigger-a-snapshot"></a>Anlık görüntü tetikleyin
Bu adımlar yalnızca anlık görüntü tabanlı paylaşım için geçerlidir.

1. **Ayrıntılar** sekmesini ve ardından **tetikleyici anlık görüntüsünü** seçerek bir anlık görüntü tetikleyebilirsiniz. Burada, verilerinizin tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. Veri sağlayıcınızdan ilk kez veri alıyorsanız tam kopya ' ı seçin. 

   ![Görüntüyü Tetikle](./media/trigger-snapshot.png "Görüntüyü Tetikle") 

1. Son çalıştırma durumu *başarılı* olduğunda, alınan verileri görüntülemek için hedef veri deposuna gidin. **Veri kümeleri** ' ni seçin ve Hedef yoldaki bağlantıya tıklayın. 

   ![Tüketici veri kümeleri](./media/consumer-datasets.png "Tüketici veri kümesi eşleme") 

## <a name="view-history"></a>Geçmişi görüntüleme
Bu adım yalnızca anlık görüntü tabanlı paylaşım için geçerlidir. Anlık görüntülerinizin geçmişini görüntülemek için **Geçmiş** sekmesini seçin. Burada, son 30 gün için oluşturulan tüm anlık görüntülerin geçmişini bulacaksınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak artık gerekli olmadığında, **veri paylaşımının genel bakış** sayfasına gidin ve **Sil** ' i seçerek kaldırın.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Azure veri paylaşımının nasıl kabul edileceği ve alınacağı hakkında daha fazla öğrendiniz. Azure veri paylaşımıyla ilgili kavramlar hakkında daha fazla bilgi edinmek için Azure veri paylaşma terminolojisi ' ne geçin.

> [!div class="nextstepaction"]
> [Azure veri paylaşımında kavramlar](terminology.md)