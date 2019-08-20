---
title: "Hızlı Başlangıç: Azure SQL veri ambarı 'nda işlem duraklatma ve devam ettirme-Azure portal | Microsoft Docs"
description: Maliyetleri kaydetmek için Azure SQL veri ambarı 'nda işlem duraklatmak için Azure portal kullanın. Veri ambarını kullanmaya hazırsanız işlem işlemine da izin verilmez.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 46fb1fc6455cd35d8bb1eaf0a1b7ee499f5c1caf
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69574877"
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portal Azure SQL veri ambarı için işlem duraklatma ve devam ettirme

Maliyetleri kaydetmek için Azure SQL veri ambarı 'nda işlem duraklatmak için Azure portal kullanın. Veri ambarını kullanmaya hazırsanız [Işlem işlemine](sql-data-warehouse-manage-compute-overview.md) da izin verilmez.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

**Mysampledatawarehouse**adlı bir veri ambarı oluşturmak için [oluşturma ve bağlanma-Portal](create-data-warehouse-portal.md) ' i kullanın. 

## <a name="pause-compute"></a>İşlem Duraklat

Maliyetleri kaydetmek için, talep üzerine işlem kaynaklarını duraklatabilir ve sürdürebilirsiniz. Örneğin, gece ve hafta sonları sırasında veritabanını kullanmıyorsanız, bu zamanlarda duraklatıp gün içinde devam edebilirsiniz. Veritabanı duraklatıldığında işlem kaynakları için ücretlendirilmezsiniz. Ancak, depolama alanı için ücretlendirilmeye devam edersiniz. 

Bir SQL veri ambarını duraklatmak için bu adımları izleyin.

1. Azure portalının sol taraftaki sayfasında **SQL veritabanları**’na tıklayın.
2. **SQL veritabanları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Bu, veri ambarını açar. 
3. **Mysampledatawarehouse** sayfasında, **durumu** **çevrimiçi**olarak görürsünüz.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

4. Veri ambarını duraklatmak için **Duraklat** düğmesine tıklayın. 
5. Devam etmek isteyip istemediğinizi soran bir onay sorusu görüntülenir. Yordamı tamamlamak için **Evet**yüklemesini desteklemesi gerekir.
6. Birkaç dakika bekleyin ve sonra **durumun** duraklatıldığına dikkat edin.

    ![Duraklatılıyor](media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında durum **duraklatılır** ve seçenek düğmesi **başlatılır**.
8. Veri ambarı için işlem kaynakları artık çevrimdışı. Hizmeti sürdürülene kadar işlem için ücretlendirilmezsiniz.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>İşlem işlemine geri dön

Bir SQL veri ambarını yeniden başlatmak için aşağıdaki adımları izleyin.

1. Azure portalının sol taraftaki sayfasında **SQL veritabanları**’na tıklayın.
2. **SQL veritabanları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Bu, veri ambarını açar. 
3. **Mysampledatawarehouse** sayfasında **durum** **duraklatıldı**' ya dikkat edin.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)

4. Veri ambarını yeniden **başlatmak Için Başlat**' a tıklayın. 
5. Başlatmak isteyip istemediğinizi soran bir onay sorusu görüntülenir. Yordamı tamamlamak için **Evet**yüklemesini desteklemesi gerekir.
6. **Durumun** sürdürüldiğine dikkatedin.

    ![Sürdürülüyor](media/pause-and-resume-compute-portal/resuming.png)

7. Veri ambarı yeniden çevrimiçi olduğunda, durum **çevrimiçi** olur ve seçenek düğmesi **duraklatılır**.
8. Veri ambarı için işlem kaynakları artık çevrimiçi ve hizmeti kullanabilirsiniz. İşlem ücretleri sürdürüldü.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Verileri depolama alanında tutmak istiyorsanız, işlem ' i duraklatın.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın ve veri ambarınıza tıklayın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

2. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**’e tıklayın.

3. Oluşturduğunuz SQL Server 'ı kaldırmak için **MyNewServer-20171113.Database.Windows.net**ve ardından **Sil**' e tıklayın.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Artık veri ambarınız için işlem duraklamış ve devam ettirildi. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Verileri bir SQL veri ambarına yükleme](load-data-from-azure-blob-storage-using-polybase.md)
