---
title: Azure Analiz Hizmetleri veritabanı yedekleme ve geri yükleme | Microsoft Dokümanlar
description: Bu makalede, bir Azure Çözümleme Hizmetleri veritabanından model meta verileri ve verileri yedekleme ve geri yükleme nasıl açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 83da2024ab74b705b45a5891f6b40251020dad31
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408665"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Çözümleme Hizmetleri'nde tabular model veritabanlarını yedeklemek, şirket içi Analiz Hizmetleri ile aynıdır. Birincil fark, yedekleme dosyalarınızı nerede depoladığınızdır. Yedekleme dosyaları, Azure [depolama hesabındaki](../storage/common/storage-create-storage-account.md)bir kapsayıcıya kaydedilmelidir. Zaten sahip olduğunuz bir depolama hesabı ve kapsayıcısı kullanabilir veya sunucunuz için depolama ayarlarını yapılandırırken oluşturulabilir.

> [!NOTE]
> Depolama hesabı oluşturmak, faturalandırılabilir yeni bir hizmete neden olabilir. Daha fazla bilgi için Azure [Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/blobs/)bakın.
> 
> 

Yedekler .abf uzantısı ile kaydedilir. Bellek içi tabular modeller için hem model verileri hem de meta veriler depolanır. DirectQuery tabular modelleri için yalnızca model meta verileri depolanır. Yedeklemeler, seçtiğiniz seçeneklere bağlı olarak sıkıştırılabilir ve şifrelenebilir.


## <a name="configure-storage-settings"></a>Depolama ayarlarını yapılandırma
Yedeklemeden önce, sunucunuz için depolama ayarlarını yapılandırmanız gerekir.


### <a name="to-configure-storage-settings"></a>Depolama ayarlarını yapılandırmak için
1.  Azure portalı> **Ayarlar'da** **Yedekleme'yi**tıklatın.

    ![Ayarlar'daki yedeklemeler](./media/analysis-services-backup/aas-backup-backups.png)

2.  **Etkin'e**tıklayın, ardından **Depolama Ayarları'nı**tıklatın.

    ![Etkinleştirme](./media/analysis-services-backup/aas-backup-enable.png)

3. Depolama hesabınızı seçin veya yeni bir hesap oluşturun.

4. Bir kapsayıcı seçin veya yeni bir kapsayıcı oluşturun.

    ![Kapsayıcı seçme](./media/analysis-services-backup/aas-backup-container.png)

5. Yedekleme ayarlarınızı kaydedin.

    ![Yedekleme ayarlarını kaydetme](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Backup

### <a name="to-backup-by-using-ssms"></a>SSMS kullanarak yedeklemek için

1. SSMS'de, **Yedekleme**> bir veritabanına sağ tıklayın.

2. **Yedekleme Veritabanı** > **Yedekleme dosyasında,** **Gözat'ı**tıklatın.

3. Dosyayı iletişim **kutusu olarak kaydet'te** klasör yolunu doğrulayın ve ardından yedek dosya için bir ad yazın. 

4. Yedekleme **Veritabanı** iletişim kutusunda seçenekleri seçin.

    **Dosya nın üzerine yazılmasına izin ver** - Aynı adı taşıyan yedek dosyaların üzerine yazmak için bu seçeneği seçin. Bu seçenek seçili değilse, kaydettiğiniz dosya, aynı konumda zaten bulunan bir dosyayla aynı ada sahip olamaz.

    **Sıkıştırma uygula** - Yedekleme dosyasını sıkıştırmak için bu seçeneği seçin. Sıkıştırılmış yedekleme dosyaları disk alanı tasarrufu sağlar, ancak biraz daha yüksek CPU kullanımı gerektirir. 

    **Yedekleme dosyasını şifreleme** - Yedekleme dosyasını şifrelemek için bu seçeneği seçin. Bu seçenek, yedekleme dosyasını güvenli hale getirmek için kullanıcı tarafından sağlanan bir parola gerektirir. Parola, yedekleme verilerinin geri yükleme işlemi dışında başka bir yol olarak okunmasını engeller. Yedeklemeleri şifrelemeyi seçerseniz, parolayı güvenli bir konumda saklayın.

5. Yedekleme dosyasını oluşturmak ve kaydetmek için **Tamam'ı** tıklatın.


### <a name="powershell"></a>PowerShell
[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase) cmdlet kullanın.

## <a name="restore"></a>Geri Yükleme
Geri alırken, yedekleme dosyanız sunucunuz için yapılandırdığınız depolama hesabında olmalıdır. Yedek bir dosyayı şirket içi bir konumdan depolama hesabınıza taşımanız gerekiyorsa, [Microsoft Azure Depolama Gezgini'ni](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) veya [AzCopy](../storage/common/storage-use-azcopy.md) komut satırı yardımcı programını kullanın. 



> [!NOTE]
> Şirket içi bir sunucudan geri alıyorsanız, tüm etki alanı kullanıcılarını modelin rollerinden kaldırmanız ve bunları Azure Etkin Dizin kullanıcıları olarak rollere geri eklemeniz gerekir.
> 
> 

### <a name="to-restore-by-using-ssms"></a>SSMS kullanarak geri yüklemek için

1. SSMS'de, **Geri Yükleme**> bir veritabanına sağ tıklayın.

2. Yedekleme **Veritabanı** iletişim kutusunda, **Yedekleme dosyasında** **Gözat'ı**tıklatın.

3. Veritabanı **Dosyalarını Bul** iletişim kutusunda, geri yüklemek istediğiniz dosyayı seçin.

4. **Geri Yükleme veritabanında**veritabanını seçin.

5. Seçenekleri belirtin. Güvenlik seçenekleri, yedekleme yaparken kullandığınız yedekleme seçenekleriyle eşleşmelidir.


### <a name="powershell"></a>PowerShell

[Geri Yükleme-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase) cmdlet kullanın.


## <a name="related-information"></a>İlgili bilgiler

[Azure depolama hesapları](../storage/common/storage-create-storage-account.md)  
[Yüksek kullanılabilirlik](analysis-services-bcdr.md)     
[Azure Analiz Hizmetlerini Yönetme](analysis-services-manage.md)
