---
title: "Öğretici: Org'unuzun dışında paylaşın - Azure Veri Paylaşımı"
description: Öğretici - Azure Veri Paylaşımı'nı kullanarak verileri müşterilerle ve iş ortaklarıyla paylaşın
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: a8265680f74b2d5679d1ebfbb2873dd096f498a3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77083055"
---
# <a name="tutorial-share-data-using-azure-data-share"></a>Öğretici: Azure Veri Paylaşımı'nı kullanarak verileri paylaşma  

Bu eğitimde, yeni bir Azure Veri Paylaşımı'nı nasıl ayarlayacağınızı ve verilerinizi Azure kuruluşunuz dışındaki müşteriler ve iş ortaklarıyla paylaşmaya nasıl başlayacağınızı öğreneceksiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Veri Paylaşımı oluşturun.
> * Veri Paylaşımınıza veri kümelerini ekleyin.
> * Veri Paylaşımınız için anlık görüntü çizelgesietkinleştirin. 
> * Veri Paylaşımınıza alıcıları ekleyin. 

## <a name="prerequisites"></a>Ön koşullar

* Azure Aboneliği: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/) oluşturun.
* Alıcınızın Azure giriş e-posta adresi (e-posta takma adlarını kullanmak çalışmaz).
* Kaynak Azure veri deposu, Veri Paylaşımı kaynağını oluşturmak için kullanacağınız abonelikten farklı bir Azure aboneliğindeyse, Azure veri deposunun bulunduğu abonelikteki [Microsoft.DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) kaydedin. 

### <a name="share-from-a-storage-account"></a>Bir depolama hesabından paylaşın:

* Azure Depolama hesabınız: Zaten hesabınız yoksa, bir Azure [Depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) oluşturabilirsiniz
* *Microsoft.Storage/storageAccounts/write*adresinde bulunan depolama hesabına yazma izni. Bu izin Katılımcı rolünde vardır.
* *Microsoft.Authorization/role assignments/write'da*bulunan depolama hesabına rol ataması ekleme izni. Bu izin Sahibi rolünde vardır. 


### <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşın:

* Paylaşmak istediğiniz tablolara ve görünümlere sahip bir Azure SQL Veritabanı veya Azure Synapse Analytics (eski adıyla Azure SQL Veri Ambarı).
* *Microsoft.Sql/servers/databases/write*adresinde bulunan SQL sunucusundaki veritabanlarına yazma izni. Bu izin Katılımcı rolünde vardır.
* Veri ambarına erişmek için veri paylaşımı için izin. Bu aşağıdaki adımlar la yapılabilir: 
    1. Kendinizi SQL sunucusu için Azure Active Directory Yöneticisi olarak ayarlayın.
    1. Azure Etkin Dizini'ni kullanarak Azure SQL Veritabanı/Veri Ambarı'na bağlanın.
    1. Veri Paylaşımı kaynağı Yönetilen Kimliği db_datareader olarak eklemek için aşağıdaki komut dosyasını yürütmek için Sorgu Düzenleyicisi'ni (önizleme) kullanın. SQL Server kimlik doğrulaması değil, Active Directory kullanarak bağlanmanız gerekir. 
    
        ```sql
        create user "<share_acct_name>" from external provider;     
        exec sp_addrolemember db_datareader, "<share_acct_name>"; 
        ```                   
       *<share_acc_name>* Veri Paylaşımı kaynağınızın adı olduğunu unutmayın. Henüz bir Veri Paylaşımı kaynağı oluşturmadıysanız, bu ön koşula daha sonra geri dönebilirsiniz.  

* Gezinmek ve paylaşmak istediğiniz tabloları ve/veya görünümleri seçmek için 'db_datareader' erişimi olan bir Azure SQL Veritabanı Kullanıcısı. 

* İstemci IP SQL Server Firewall erişimi. Bu aşağıdaki adımlar la yapılabilir: 
    1. Azure portalındaki SQL sunucusunda *Güvenlik Duvarları'na ve sanal ağlara* gidin
    1. Azure Hizmetlerine erişime izin vermek için **"geçiş"** seçeneğini tıklatın.
    1. **İstemci IP Ekle'yi** ve **Kaydet'i**tıklatın. İstemci IP adresi değiştirilebilir. Bu işlemin, Azure portalından SQL verilerini bir sonraki paylaşımında yinelenmesi gerekebilir. Ayrıca bir IP aralığı ekleyebilirsiniz. 

### <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini'nden Paylaş
* Paylaşmak istediğiniz veritabanlarına sahip bir Azure Veri Gezgini kümesi.
* *Microsoft.Kusto/clusters/write'da*bulunan Azure Veri Gezgini kümesine yazma izni. Bu izin Katılımcı rolünde vardır.
* *Microsoft.Authorization/role assignments/write'da*bulunan Azure Veri Gezgini kümesine rol ataması ekleme izni. Bu izin Sahibi rolünde vardır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-data-share-account"></a>Veri Paylaşım Hesabı Oluşturma

Azure kaynak grubunda bir Azure Veri Paylaşımı kaynağı oluşturun.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

1. Veri *Paylaşımı'nı*ara.

1. Veri Paylaşımı'nı seçin ve **Oluştur'u**seçin.

1. Azure Veri Paylaşımı kaynağınızın temel ayrıntılarını aşağıdaki bilgilerle doldurun. 

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Adı | *datashareacount* | Veri paylaşım hesabınız için bir ad belirtin. |
    | Abonelik | Aboneliğiniz | Veri paylaşım hesabınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-resource-group* | Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Konum | *Doğu ABD 2* | Veri paylaşım hesabınız için bir bölge seçin.
    | | |

1. Veri paylaşım hesabınızı sağlamak için **Oluştur'u** seçin. Yeni bir veri paylaşımı hesabı sağlama genellikle yaklaşık 2 dakika veya daha az sürer. 

1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

## <a name="create-a-data-share"></a>Veri Paylaşımı Oluşturma

1. Veri Paylaşımı Genel Bakış sayfanıza gidin.

    ![Verilerinizi paylaşın](./media/share-receive-data.png "Verilerinizi paylaşın") 

1. **Verilerinizi paylaşmaya başlat'ı**seçin.

1. **Oluştur'u**seçin.   

1. Veri Paylaşımınızın ayrıntılarını doldurun. Bir ad, paylaşım türü, paylaşım içeriğinin açıklaması ve kullanım koşulları (isteğe bağlı) belirtin. 

    ![GirişShareDetails](./media/enter-share-details.png "Ayrıntıları Paylaş'ı girin") 

1. **Devam Et'i** seçin

1. Veri Paylaşımınıza Veri Kümeleri eklemek için **Veri Kümesi Ekle'yi**seçin. 

    ![Veri kümeleri](./media/datasets.png "Veri kümeleri")

1. Eklemek istediğiniz veri kümesi türünü seçin. Önceki adımda seçtiğiniz paylaşım türüne (anlık görüntü veya yerinde) bağlı olarak farklı bir veri kümesi türü listesi görürsünüz. Bir Azure SQL Veritabanı'ndan veya Azure SQL Veri Ambarı'ndan paylaşım varsa, bazı SQL kimlik bilgileri için istenir. Ön koşulların bir parçası olarak oluşturduğunuz kullanıcıyı kullanarak kimlik doğrulaması.

    ![Veri Setleri Ekle](./media/add-datasets.png "Veri Setleri Ekle")    

1. Paylaşmak istediğiniz nesneye gidin ve 'Veri Kümeleri Ekle'yi seçin. 

    ![Veri kümelerini seçin](./media/select-datasets.png "Veri Kümelerini Seçin")    

1. Alıcılar sekmesine ,'+ Alıcı Ekle' seçeneğini seçerek Veri Tüketicinizin e-posta adreslerini girin. 

    ![Alıcıları Ekler](./media/add-recipient.png "Alıcı ekleme") 

1. **Devam Et'i** seçin

1. Anlık görüntü paylaşım türünü seçtiyseniz, verilerinizin güncelleştirmelerini veri tüketicinize sağlamak için anlık görüntü zamanlamasını yapılandırabilirsiniz. 

    ![Anlık Görüntüleri Etkinleştir](./media/enable-snapshots.png "Anlık görüntüleri etkinleştirme") 

1. Bir başlangıç saati ve yineleme aralığı seçin. 

1. **Devam Et'i** seçin

1. Gözden Geçir + Oluştur sekmesinde Paket İçeriklerinizi, Ayarlarınızı, Alıcılarınızı ve Eşitleme Ayarlarınızı inceleyin. **Oluştur**’u seçin

Azure Veri Paylaşımınız artık oluşturuldu ve Veri Paylaşımınızın alıcısı davetinizi kabul etmeye hazır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Veri Paylaşımı oluşturmayı ve alıcıları nasıl davet edeceğinizi öğrendiniz. Bir Veri Tüketicisinin bir veri paylaşımını nasıl kabul edip alabileceğini öğrenmek için, veri eğitimini [kabul etmeye ve almaya](subscribe-to-data-share.md) devam edin. 
