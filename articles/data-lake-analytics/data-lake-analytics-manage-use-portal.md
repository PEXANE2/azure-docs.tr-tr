---
title: Azure portalını kullanarak Azure Veri Gölü Analizi'ni yönetme
description: Bu makalede, Veri Gölü Analizi hesaplarını, veri kaynaklarını, kullanıcıları & işleri yönetmek için Azure portalının nasıl kullanılacağı açıklanmaktadır.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 8a6b7cb3fd45e17b84519efcaa826b569083e156
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265707"
---
# <a name="manage-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Data Lake Analytics'i Azure portalını kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure portalını kullanarak Azure Veri Gölü Analizi hesaplarının, veri kaynaklarının, kullanıcıların ve işlerin nasıl yönetilenbir şekilde yönetilen.


<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics hesaplarını yönetme

### <a name="create-an-account"></a>Hesap oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kaynak** > **İstihbaratı Oluştur + analitik** > **Veri Gölü Analizi'ni**tıklatın.
3. Aşağıdaki öğeler için değerleri seçin: 
   1. **Adı**: Data Lake Analytics hesabının adı.
   2. **Abonelik**: Hesap için kullanılan Azure aboneliği.
   3. **Kaynak Grubu**: Hesabı oluşturacak Azure kaynak grubudur. 
   4. **Konum**: Veri Gölü Analizi hesabı için Azure veri merkezi. 
   5. **Veri Gölü Deposu**: Veri Gölü Analizi hesabı için kullanılacak varsayılan depo. Azure Veri Gölü Deposu hesabı ve Data Lake Analytics hesabı aynı konumda olmalıdır.
4. **Oluştur'u**tıklatın. 

### <a name="delete-a-data-lake-analytics-account"></a>Veri Gölü Analizi hesabını silme

Bir Data Lake Analytics hesabını silmeden önce, varsayılan Data Lake Store hesabını silin.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sil'i**tıklatın.
3. Hesap adını yazın.
4. **Sil'i**tıklatın.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme

Data Lake Analytics aşağıdaki veri kaynaklarını destekler:

* Data Lake Store
* Azure Storage

Veri kaynaklarına göz atmak ve temel dosya yönetimi işlemlerini gerçekleştirmek için Veri Gezgini'ni kullanabilirsiniz. 

### <a name="add-a-data-source"></a>Veri kaynağı ekleme

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Veri Kaynakları'nı**tıklatın.
3. **Veri Kaynağı Ekle'yi**tıklatın.
    
   * Bir Data Lake Store hesabı eklemek için, sorgulayabilmek için hesap adı ve hesabına erişim gerekir.
   * Azure Blob depolama alanı eklemek için depolama hesabına ve hesap anahtarına ihtiyacınız var. Onları bulmak için, portaldaki depo hesabına gidin.

## <a name="set-up-firewall-rules"></a>Güvenlik duvarı kurallarını ayarlama

Ağ düzeyinde Veri Gölü Analytics hesabınıza erişimi daha da kilitlemek için Data Lake Analytics'i kullanabilirsiniz. Güvenlik duvarını etkinleştirebilir, bir IP adresi belirtebilir veya güvenilir müşterileriniz için bir IP adresi aralığı tanımlayabilirsiniz. Bu önlemleri etkinleştirdikten sonra, yalnızca tanımlanan aralıktaki IP adreslerine sahip istemciler mağazaya bağlanabilir.

Azure Veri Fabrikası veya Sanal Taşıtlar gibi diğer Azure hizmetleri Veri Gölü Analizi hesabına bağlanırsa, **Azure Hizmetlerine İzin Ver'in** **açık**olduğundan emin olun. 

### <a name="set-up-a-firewall-rule"></a>Güvenlik duvarı kuralı ayarlama

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Soldaki menüde Güvenlik **Duvarı'nı**tıklatın.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Yeni Veri Gölü kullanıcılarını kolayca sağlamak için **Kullanıcı Ekle Sihirbazı'nı** kullanabilirsiniz.

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. Solda, **Başlarken**altında, **Kullanıcı Sihirbazı Ekle'yi**tıklatın.
3. Bir kullanıcı seçin ve sonra **Seç'i**tıklatın.
4. Bir rol seçin ve sonra **Seç'i**tıklatın. Azure Veri Gölü'nü kullanacak yeni bir geliştirici ayarlamak için **Veri Gölü Analytics Geliştiricisi** rolünü seçin.
5. U-SQL veritabanları için erişim denetim listelerini (ALA'lar) seçin. Seçeneklerinizden memnun **olduğunuzda, Seç'i**tıklatın.
6. Dosyalar için Aç'ları seçin. Varsayılan depo için, kök klasörü "/" ve /sistem klasörü için AK'ları değiştirmeyin. **Seç'i**tıklatın.
7. Seçtiğiniz tüm değişiklikleri gözden geçirin ve ardından **Çalıştır'ı**tıklatın.
8. Sihirbaz bittiğinde, **Bitti'yi**tıklatın.

## <a name="manage-role-based-access-control"></a>Rol Tabanlı Erişim Denetimini Yönetme

Diğer Azure hizmetleri gibi, kullanıcıların hizmetle nasıl etkileşimde olduğunu denetlemek için Role Tabanlı Erişim Denetimi'ni (RBAC) kullanabilirsiniz.

Standart RBAC rolleri aşağıdaki özelliklere sahiptir:
* **Sahibi**: İş gönderebilir, işleri izleyebilir, herhangi bir kullanıcıdan işleri iptal edebilir ve hesabı yapılandırabilir.
* **Katkıda Bulunan**: İş gönderebilir, işleri izleyebilir, herhangi bir kullanıcıdan işleri iptal edebilir ve hesabı yapılandırabilir.
* **Okuyucu**: İşleri izleyebilir.

U-SQL geliştiricilerinin Data Lake Analytics hizmetini kullanmasını sağlamak için Veri Gölü Analytics Geliştiricisi rolünü kullanın. Data Lake Analytics Developer rolünü şu şekilde kullanabilirsiniz:
* İş gönderin.
* İş durumunu ve herhangi bir kullanıcı tarafından gönderilen işlerin ilerlemesini izleyin.
* Herhangi bir kullanıcı tarafından gönderilen işlerden U-SQL komut dosyasına bakın.
* Sadece kendi işinizi iptal edin.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabına kullanıcı veya güvenlik grupları ekleme

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Access denetimi (IAM)** > **Rol ataması ekle'yi**tıklatın.
3. Bir rol seçin.
4. Bir kullanıcı ekleyin.
5. **Tamam**'a tıklayın.

>[!NOTE]
>Bir kullanıcının veya güvenlik grubunun iş göndermesi gerekiyorsa, mağaza hesabından da izin alması gerekir. Daha fazla bilgi için Bkz. [Veri Gölü Deposu'nda depolanan Güvenli veriler.](../data-lake-store/data-lake-store-secure-data.md)
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>İşleri yönetme

### <a name="submit-a-job"></a>İş gönderme

1. Azure portalında Data Lake Analytics hesabınıza gidin.

2. **Yeni İş'i**tıklatın. Her iş için, yapılandırın:

    1. **İş Adı**: İşin adı.
    2. **Öncelik**: Daha düşük sayılar daha yüksek önceliğe sahiptir. İki iş sıraya düşerse, önce daha düşük öncelik değerine sahip olan çalıştırılır.
    3. **Paralellik**: Bu iş için rezerve edilebilen maksimum bilgi işlem sayısıdır.

3. **İşi Gönder**'e tıklayın.

### <a name="monitor-jobs"></a>İşleri izleme

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Tüm İşleri Görüntüle'yi**tıklatın. Hesaptaki tüm etkin ve son zamanlarda tamamlanan işlerin listesi gösterilir.
3. İsteğe bağlı olarak, **Zaman Aralığı,** İş **Adı**ve **Yazar** değerlerine göre işleri bulmanıza yardımcı olmak için **Filtre'yi** tıklatın. 

### <a name="monitoring-pipeline-jobs"></a>Boru hattı işlerinin izlenmesi
Bir ardışık bölümün parçası olan işler, belirli bir senaryoyu gerçekleştirmek için genellikle sıralı olarak birlikte çalışır. Örneğin, müşteri öngörülerini temizleyen, ayıklayan, dönüştüren, kullanımını toplayan bir ardışık pipeline'A sahip olabilirsiniz. İş gönderildiğinde "Pipeline" özelliği kullanılarak boru hattı işleri tanımlanır. ADF V2 kullanılarak zamanlanan işler de bu özelliği otomatik olarak dolduracaktır. 

Ardışık hatların parçası olan U-SQL işlerinin listesini görüntülemek için: 

1. Azure portalında Data Lake Analytics hesaplarınıza gidin.
2. **İş İstatistikleri'ni**tıklatın. "Tüm İşler" sekmesi varsayılan olarak, çalışan, sıraya ve sona eren işlerin listesini gösterir.
3. Pipeline **İşler** sekmesini tıklatın. Her ardışık hat lar için toplu istatistiklerle birlikte bir boru hattı işlerinin listesi gösterilir.

### <a name="monitoring-recurring-jobs"></a>Yinelenen işlerin izlenmesi
Yinelenen bir iş, aynı iş mantığına sahip, ancak her çalıştığında farklı giriş verileri kullanan bir iştir. İdeal olarak, yinelenen işler her zaman başarılı olmalı ve nispeten istikrarlı yürütme süresine sahip olmalıdır; bu davranışların izlenmesi işin sağlıklı olmasını sağlamaya yardımcı olacaktır. Yinelenen işler "Yineleme" özelliği kullanılarak tanımlanır. ADF V2 kullanılarak zamanlanan işler de bu özelliği otomatik olarak dolduracaktır.

Yinelenen U-SQL işlerinin listesini görüntülemek için: 

1. Azure portalında Data Lake Analytics hesaplarınıza gidin.
2. **İş İstatistikleri'ni**tıklatın. "Tüm İşler" sekmesi varsayılan olarak, çalışan, sıraya ve sona eren işlerin listesini gösterir.
3. Yinelenen **İşler** sekmesini tıklatın. Yinelenen işlerin listesi, yinelenen her iş için toplu istatistiklerle birlikte gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics’e Genel Bakış](data-lake-analytics-overview.md)
* [Azure PowerShell'i kullanarak Azure Veri Gölü Analizini yönetin](data-lake-analytics-manage-use-powershell.md)
* [Azure Data Lake Analytics'i ilkeleri kullanarak yönetme](data-lake-analytics-account-policies.md)
