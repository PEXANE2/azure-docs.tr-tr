---
title: Azure Kaynak taşıyıcısı ile bölgeler arasında Azure SQL kaynaklarını taşıma
description: Azure SQL kaynaklarını Azure Kaynak taşıyıcısı ile başka bir bölgeye taşımayı öğrenin
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0718151039d88ffb76a07ce082c08fb011dab88b
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653519"
---
# <a name="tutorial-move-azure-sql-database-resources-to-another-region"></a>Öğretici: Azure SQL veritabanı kaynaklarını başka bir bölgeye taşıma

Bu öğreticide Azure SQL veritabanlarının ve elastik havuzların [Azure Kaynak taşıyıcısı](overview.md)kullanarak farklı bir Azure bölgesine nasıl taşınacağını öğrenin.

> [!NOTE]
> Azure Kaynak taşıyıcısı Şu anda önizleme aşamasındadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları ve gereksinimleri denetleyin.
> * Taşımak istediğiniz kaynakları seçin.
> * Kaynak bağımlılıklarını çözümleyin.
> * SQL Server hazırlayın ve hedef bölgeye taşıyın.
> * Veritabanları ve elastik havuzlar hazırlayın ve taşıyın.
> * Taşımayı atmak mı yoksa kaydetmek mi istediğinize karar verin. 
> * Taşıma işleminden sonra kaynak bölgedeki kaynakları isteğe bağlı olarak kaldırın. 

> [!NOTE]
> Öğreticiler, bir senaryoyu denemek için en hızlı yolu gösterir ve varsayılan seçenekleri kullanır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun. Ardından [Azure Portal](https://portal.azure.com)oturum açın.

## <a name="prerequisites"></a>Önkoşullar

-  Taşımak istediğiniz kaynakları içeren abonelikte *sahip* erişiminizin olduğunu denetleyin.
    - Azure aboneliğindeki belirli bir kaynak ve hedef çifti için ilk kez kaynak eklediğinizde, kaynak taşıyıcısı abonelik tarafından güvenilen [sistem tarafından atanan bir yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (eski adıyla yönetilen hizmet tanımlaması (MSI)) oluşturur.
    - Kimliği oluşturmak ve gerekli rolü (katkıda bulunan ya da kaynak abonelikte Kullanıcı erişimi Yöneticisi) atamak için, kaynak eklemek için kullandığınız hesabın abonelikte *sahip* izinleri olması gerekir. Azure rolleri hakkında [daha fazla bilgi edinin](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) .
- Aboneliğin, hedef bölgede taşıdığınız kaynakları oluşturmak için yeterli kotası olması gerekir. Kota içermiyorsa [ek sınırlamalar isteyin](/azure/azure-resource-manager/management/azure-subscription-service-limits).
- Kaynakları taşıdığınız hedef Bölgeyle ilişkili fiyatlandırmayı ve ücretleri doğrulayın. Size yardımcı olması için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.
    

## <a name="check-sql-requirements"></a>SQL gereksinimlerini denetleme

1. Başka bir bölgeye geçmek için hangi veritabanı/elastik havuz özelliklerinin desteklendiğini [denetleyin](support-matrix-move-region-sql.md) .
2. Hedef bölgede, her kaynak sunucu için bir hedef sunucu oluşturun. [Daha fazla bilgi edinin](/azure/azure-sql/database/active-geo-replication-security-configure#how-to-configure-logins-and-users).
4. Veritabanları, saydam veri şifrelemesi (TDE) ile şifrelenirse ve Azure Key Vault ' de kendi şifreleme anahtarınızı kullanıyorsanız, anahtar kasalarını başka bir bölgeye taşımayı [öğrenin](../key-vault/general/move-region.md) .
5. SQL Data Sync etkinse, üye veritabanlarının taşınması desteklenir. Taşıma işleminden sonra SQL Data Sync 'i yeni hedef veritabanına ayarlamanız gerekir.
6. Taşımadan önce Gelişmiş veri güvenliği ayarlarını kaldırın. Taşıdıktan sonra, hedef bölgedeki SQL Server düzeyinde [ayarları yapılandırın](/azure/sql-database/sql-database-advanced-data-security) .
7. Denetim etkinse, taşıdıktan sonra ilkeler varsayılana sıfırlanır. Taşıma işleminden sonra [denetimi tekrar ayarlayın](/azure/sql-database/sql-database-auditing) .
7. Kaynak veritabanının yedekleme bekletme ilkeleri hedef veritabanına taşınır. Taşıma işleminden sonra ayarları değiştirme hakkında [daha fazla bilgi edinin](/azure/sql-database/sql-database-long-term-backup-retention-configure ) .
8. Taşıma işleminden önce sunucu düzeyi güvenlik duvarı kurallarını kaldırın. Veritabanı düzeyinde güvenlik duvarı kuralları, taşıma sırasında kaynak sunucudan hedef sunucuya kopyalanır. Taşıma işleminden sonra hedef bölgede SQL Server için [güvenlik duvarı kuralları ayarlayın](/azure/sql-database/sql-database-server-level-firewall-rule) .
9. Taşımadan önce, oto ayarlama ayarlarını kaldırın. Taşıdıktan sonra tekrar [ayarlamayı ayarlayın](/azure/sql-database/sql-database-automatic-tuning-enable) .
10. Taşımadan önce veritabanı uyarı ayarlarını kaldırın. Taşıdıktan sonra [sıfırlayın](/azure/sql-database/sql-database-insights-alerts-portal) .
    
## <a name="select-resources"></a>Kaynakları seçin

Taşımak istediğiniz kaynakları seçin.

- Seçili kaynak bölgedeki herhangi bir kaynak grubunda, desteklenen herhangi bir kaynak türünü seçebilirsiniz.
- Kaynakları kaynak bölgeyle aynı abonelikte yer alan bir hedef bölgeye taşırsınız. Aboneliği değiştirmek istiyorsanız, kaynaklar taşındıktan sonra bunu yapabilirsiniz.

1. Azure portal, *kaynak taşıyıcısı*için arama yapın. Ardından, **Hizmetler**' ın altında **Azure Kaynak taşıyıcısı**' ı seçin.

     ![Azure portal kaynak taşıyıcısı için arama sonuçları](./media/tutorial-move-region-sql/search.png)

2. **Genel bakış**' da **başlayın ' a tıklayın.**

    ![Başka bir bölgeye taşınacak kaynakları ekleme düğmesi](./media/tutorial-move-region-sql/get-started.png)

3. Kaynak **taşıma**  >  **kaynağı + hedef**bölümünde, kaynak aboneliğini ve bölgeyi seçin.
4. **Hedef**bölümünde, kaynakları taşımak istediğiniz bölgeyi seçin. Ardından **İleri**'ye tıklayın.
5. **Meta veri bölgesi**' nde, taşıdığınız kaynaklarla ilgili meta verileri nerede depolamak istediğinizi seçin. Bu amaçla özel olarak bir kaynak grubu oluşturulur. Ardından **İleri**'ye tıklayın.

    ![Kaynak ve hedef bölge seçme sayfası](./media/tutorial-move-region-sql/source-target.png)

6. **Taşınacak kaynaklar**bölümünde **kaynakları seç**' e tıklayın.
7. **Kaynakları seçin**bölümünde kaynakları seçin. Yalnızca taşıma için desteklenen kaynaklar ekleyebilirsiniz. Sonra da **Bitti**’ye tıklayın.

    ![Taşınacak SQL kaynaklarını seçmek için sayfa](./media/tutorial-move-region-sql/select-resources.png)

8. **Taşınacak kaynaklar**bölümünde **İleri**' ye tıklayın.

9. **Gözden geçir + Ekle**bölümünde kaynak ve hedef ayarlarını kontrol edin. Taşıma hakkındaki meta verilerin, meta veri bölgesinde bu amaçla oluşturulmuş bir kaynak grubunda depolandığını anladığınızdan emin olun.


    ![Ayarları gözden geçirmek ve taşımaya devam etmek için sayfa](./media/tutorial-move-region-sql/review.png)

10. Kaynakları eklemeye başlamak için **devam**' a tıklayın.
11. Ekleme işlemi başarıyla tamamlandıktan sonra, bildirim simgesine **taşımak için kaynak ekleme** ' ye tıklayın.
12. Bildirime tıkladıktan sonra **bölgeler arası** sayfasında kaynakları gözden geçirin.


> [!NOTE]
> 
> - SQL Server, *El Ile yapılan bir atamanın bekleyen* durumunda.
> - Diğer eklenen kaynaklar *hazırlama bekleme* durumunda.
> - Bir taşıma koleksiyonundan bir kaynağı kaldırmak istiyorsanız, bunu yapmak için yöntemi taşıma sürecinde nerede olduğunuza bağlıdır. [Daha fazla bilgi edinin](remove-move-resources.md).

## <a name="resolve-dependencies"></a>Bağımlılıkları çözümle


1. **Bölgelerde**, kaynaklar, **sorunlar** sütununda *bağımlılıkları doğrula* iletisini gösterip, **bağımlılıkları doğrula** düğmesine tıklayın. Doğrulama işlemi başlar.
2. Bağımlılıklar bulunursa, **Bağımlılıklar Ekle**' ye tıklayın.

    ![Bağımlılıklar eklemek için düğme](./media/tutorial-move-region-sql/add-dependencies.png)
   
3. **Bağımlılıklar Ekle**' de, bağımlı kaynakları seçin > **Bağımlılıklar ekleyin**. Bildirimlerde ilerlemeyi izleyin.

4. Gerekirse ek bağımlılıklar ekleyin ve bağımlılıkları yeniden doğrulayın. 

5. **Bölgeler arası** sayfasında, kaynakların şimdi *hazırlama bekleme* durumunda olduğunu doğrulayın ve sorun yok.

    ![Hazırlama Bekleme durumundaki kaynakları gösteren sayfa](./media/tutorial-move-region-sql/prepare-pending.png)



## <a name="move-the-sql-server"></a>SQL Server taşıyın

Hedef bölgede bir hedef SQL Server atayın ve taşımayı yürütün.

### <a name="assign-a-target-sql-server"></a>Hedef SQL Server atama

1. **Bölgelerde**, SQL Server kaynak Için, **hedef yapılandırma** sütununda, **kaynak atanmadı**' a tıklayın.
2. Hedef bölgede mevcut bir SQL Server kaynağı seçin. 
    
    ![Taşıma işlemi için ayarlanan SQL Server durumunun gösterildiği giriş bekleniyor](./media/tutorial-move-region-sql/sql-server-commit-move-pending.png) 

    
> [!NOTE]
> Kaynak SQL Server durum, *taşıma taşıma bekleniyor*olarak değişir. 

### <a name="commit-the-sql-server-move"></a>SQL Server taşımayı Yürüt

1. **Bölgeler arasında**içinde SQL Server seçin ve ardından **taşımayı Yürüt**' e tıklayın.
2. **Kaynakları kaydet**' de, **Yürüt**' e tıklayın.

    ![SQL Server taşımayı tamamlamak için sayfa](./media/tutorial-move-region-sql/commit-sql-server.png)

3. Bildirim çubuğunda taşıma ilerlemesini izleyin.

> [!NOTE]
> İşlemeden sonra, SQL Server *kaynak silme beklemede* durumunda olur.


## <a name="prepare-resources-to-move"></a>Taşınacak kaynakları hazırlama

Kaynak SQL Server taşındığında, diğer kaynakları taşımaya hazırlanabilirsiniz.

## <a name="prepare-an-elastic-pool"></a>Elastik havuz hazırlama

1. **Bölgeler arasında**, kaynak elastik havuzu ' nu (demo-test1-elaçıkartma havuzu) seçin ve ardından **hazırla**' ya tıklayın.

    ![Kaynakları hazırlama düğmesi](./media/tutorial-move-region-sql/prepare-elastic.png)

2. **Kaynakları hazırlama**bölümünde **hazırla**' ya tıklayın.
3. Bildirimler hazırlama işleminin başarılı olduğunu gösterip **Yenile**' ye tıklayın.

> [!NOTE]
> Elastik havuz, şimdi bir *taşıma hareketi bekliyor* durumundadır.

## <a name="prepare-a-single-database"></a>Tek bir veritabanı hazırlama

1. **Bölgeler arasında**, tek veritabanını seçin (elastik havuzda değil) ve ardından **hazırla**' ya tıklayın.

    ![Seçili kaynakları hazırlama düğmesi](./media/tutorial-move-region-sql/prepare-db.png)

2. **Kaynakları hazırlama**bölümünde **hazırla**' ya tıklayın.
3. Bildirimler hazırlama işleminin başarılı olduğunu gösterip **Yenile**' ye tıklayın.

> [!NOTE]
> Veritabanı şu anda bir *başlatma taşıma beklemede* durumunda ve hedef bölgede oluşturulmuştur.


## <a name="move-the-pool-and-prepare-pool-databases"></a>Havuzu taşıma ve havuz veritabanlarını hazırlama

Elastik havuzda veritabanlarını hazırlamak için, elastik havuz bir işlem *taşıma bekleyen* durumunda olmalıdır. Bu duruma geçmek için havuz taşımayı başlatın.

#### <a name="initiate-move---elastic-pool"></a>Move-elastik havuzu Başlat

1. **Bölgeler arasında**, kaynak elastik havuzu ' nu (demo-test1-elaçıkartma havuzu) seçin ve ardından **taşımayı Başlat**' a tıklayın.
2. **Kaynakları taşıma**bölümünde, **taşımayı Başlat**' a tıklayın.

    
    ![Elastik havuz taşımayı başlatma düğmesi](./media/tutorial-move-region-sql/initiate-elastic.png)

1. Bildirim çubuğunda taşıma ilerlemesini izleyin.
1. Bildirimler taşımanın başarılı olduğunu gösterip **Yenile**' ye tıklayın.

> [!NOTE]
> Elastik havuz şimdi bir *kayıt taşıma bekliyor* durumunda.

#### <a name="prepare-database"></a>Veritabanını hazırla

1. **Bölgeler arası**bölümünde veritabanını (demo-test2-SQLDB) seçin ve ardından **hazırla**' ya tıklayın.
2. **Kaynakları hazırlama**bölümünde **hazırla**' ya tıklayın.

    ![Elastik havuzda veritabanını hazırlama düğmesi](./media/tutorial-move-region-sql/prepare-database-elastic.png) 

Hazırlama sırasında hedef veritabanı hedef bölgede oluşturulur ve veri çoğaltma başlatılır. Hazırlandıktan sonra veritabanı, bir *taşıma bekliyor* durumunda olur. 

![Esnek havuzda seçili veritabanını hazırlamak için düğme](./media/tutorial-move-region-sql/initiate-move-pending.png) 

## <a name="move-databases"></a>Veritabanlarını taşıma

Veritabanlarını taşımaya başlayın.
1. **Bölgeler arasında**, durum **başlatma taşıma bekleyen**kaynaklar ' ı seçin. Sonra **taşımayı Başlat**' a tıklayın.
2. **Kaynakları taşıma**bölümünde, **taşımayı Başlat**' a tıklayın.

    ![Taşımayı başlatacak sayfa](./media/tutorial-move-region-sql/initiate-move.png)

3. Bildirim çubuğunda taşıma ilerlemesini izleyin.

> [!NOTE]
> Veritabanları şimdi bir *kayıt taşıma bekliyor* durumunda.


## <a name="discard-or-commit"></a>Atın veya teslim edilsin mi?

İlk taşıma işleminden sonra, taşımayı yürütmek mi yoksa atmak mi istediğinize karar verebilirsiniz. 

- **At**: test ediyorsanız ve kaynak kaynağı gerçekten taşımak istemiyorsanız bir taşımayı atmak isteyebilirsiniz. Taşımayı atmak, kaynağı **başlatma bekleyen**bir durumuna döndürür.
- **COMMIT**: COMMIT, hedef bölgeye taşıma işlemini tamamlar. İşlem tamamlandıktan sonra kaynak kaynak, **bekleyen silme kaynağı**durumunda olur ve silmek istediğinize karar verebilirsiniz.


## <a name="discard-the-move"></a>Taşımayı at 

Taşımayı aşağıdaki şekilde atabilirsiniz:

1. **Bölgeler arasında**, durum **işlemede taşıma bekleyen**kaynaklar ' ı seçin ve **taşımayı at**' a tıklayın.
2. **Taşımayı at**' da **at**' a tıklayın.
3. Bildirim çubuğunda taşıma ilerlemesini izleyin.


> [!NOTE]
> - Kaynakları atladıktan sonra, bir *taşıma beklemede* durumunda olurlar.
> - Yalnızca bir elastik havuz varsa, atma devam ediyor ve hedef bölgede oluşturulan elastik havuz silinir.
> - İşlem *taşıma bekleyen* durumunda ilişkili veritabanlarına sahip bir elastik havuz varsa, elastik havuzu atabilirsiniz.
> - Bir SQL veritabanını iptal ederseniz hedef bölge kaynakları silinmez. 

İptal ettikten sonra taşımayı yeniden başlatmak istiyorsanız SQL veritabanı veya elastik havuz ' ı seçin ve taşımayı yeniden başlatın.


## <a name="commit-the-move"></a>Taşımayı Yürüt

Veritabanlarının ve elastik havuzların taşınması şu şekilde tamamlanıyor:


1. SQL Server *kaynak silme bekleniyor* durumunda olup olmadığını denetleyin.
2. İşlemeden önce veritabanı bağlantı dizelerini hedef bölgeye güncelleştirin.
3. **Bölgeler arasında**, SQL kaynaklarını seçin ve ardından **taşımayı Yürüt**' e tıklayın.
4. **Kaynakları kaydet**' de, **Yürüt**' e tıklayın.

    ![Taşımayı Yürüt](./media/tutorial-move-region-sql/commit-sql-resources.png)

5. Bildirim çubuğunda işlem ilerlemesini izleyin.


> [!NOTE]
> İşleme işlemi sırasında SQL veritabanları için bazı kesinti süreleri oluşur.
> Kaydedilmiş veritabanları ve elastik havuzlar artık bir *silme kaynağı bekliyor* durumundadır.
> İşlemeden sonra, hedef veritabanındaki güvenlik duvarı kuralları, ilkeler ve uyarılar dahil olmak üzere veritabanıyla ilgili ayarları güncelleştirin.


## <a name="delete-source-resources-after-commit"></a>İşlemeden sonra kaynak kaynaklarını Sil

Taşıma işleminden sonra, kaynak bölgedeki kaynakları isteğe bağlı olarak silebilirsiniz. 

1. **Bölgeler arasında**, silmek istediğiniz her kaynak kaynağın adına tıklayın.
2. Her kaynak için Özellikler sayfasında **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure SQL veritabanlarını başka bir Azure bölgesine taşıdı.
> * Azure SQL elastik havuzlarını başka bir bölgeye taşıdı.

Şimdi, Azure VM 'lerini başka bir bölgeye taşımaya çalışıyorsunuz.

> [!div class="nextstepaction"]
> [Azure VM'lerini taşıma](./tutorial-move-region-virtual-machines.md)
