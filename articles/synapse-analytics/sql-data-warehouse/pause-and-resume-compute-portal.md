---
title: Azure portalı üzerinden Synapse SQL havuzunda hesaplamayı duraklatma ve devam ettirme
description: Maliyetlerden tasarruf etmek için SQL havuzu için hesaplamayı duraklatmak için Azure portalını kullanın. Veri ambarını kullanmaya hazır olduğunuzda bilgi işlem devam edin.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 55e3d5bf4fb63c35d484e4a764c7eeb2e2484fcf
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350967"
---
# <a name="quickstart-pause-and-resume-compute-in-synapse-sql-pool-via-the-azure-portal"></a>Quickstart: Azure portalı üzerinden Synapse SQL havuzunda hesaplamayı duraklatma ve devam ettirme

Synapse SQL havuzu (veri ambarı) bilgi işlem kaynaklarını duraklatmak ve devam ettirmek için Azure portalını kullanabilirsiniz. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

**mySampleDataWarehouse**adlı bir SQL havuzu oluşturmak için [Oluştur ve Bağla - portalı](create-data-warehouse-portal.md) kullanın. 

## <a name="pause-compute"></a>İşlemi duraklatma

Maliyetleri azaltmak için, kaynakları isteğe bağlı olarak duraklatabilir ve yeniden başlatabilirsiniz. Örneğin, veritabanını gece ve hafta sonları kullanmayacaksanız, bu saatlerde veritabanını duraklatabilir ve gün içinde devam ettirebilirsiniz. 
>[!NOTE]
>Veritabanı duraklatılmışken kaynak hesaplama için ücretlendirilmezsiniz. Ancak, depolama için ücretlendirilmeye devam eceksiniz. 

SQL havuzuduraklatmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure portalının sol navigasyon sayfasında **Azure Synapse Analytics'i (eski adıyla SQL DW)** tıklatın.
2. SQL havuzunu açmak için **Azure Synapse Analytics (eski adıyla SQL DW)** sayfasından **mySampleDataWarehouse'u** seçin. 
3. **mySampleDataWarehouse** sayfasında, bildirim **durumu** **Çevrimiçi'** dir.

    ![Çevrimiçi işlem yapın](././media/pause-and-resume-compute-portal/compute-online.png)

4. SQL havuzunu duraklatmak için **Duraklat** düğmesini tıklatın. 
5. Devam etmek isteyip istemediğiniz sorulan bir onay sorusu görüntülenir. **Evet'i**tıklatın.
6. Birkaç dakika bekleyin ve **ardından Durumun** **Duraklatanolduğunu**fark edin.

    ![Duraklatılıyor](./media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında, durum **Duraklatılır** ve seçenek düğmesi **Devam edilir.**
8. SQL havuzunun işlem kaynakları artık çevrimdışı. Hizmete devam edene kadar işlem için ücretlendirilmezsiniz.

    ![Çevrimdışı işlem](././media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Özgeçmiş hesaplama

SQL havuzuna devam etmek için aşağıdaki adımları izleyin.

1. Azure portalının sol sayfasında **Azure Synapse Analytics'i (eski adıyla SQL DW)** tıklatın.
2. SQL havuz sayfasını açmak için **Azure Synapse Analytics (eski adıyla SQL DW)** sayfasından **mySampleDataWarehouse'u** seçin. 
3. **mySampleDataWarehouse** sayfasında, bildirim **Durumu** **Duraklatıldı.**

    ![Çevrimdışı işlem](././media/pause-and-resume-compute-portal/compute-offline.png)

4. SQL havuzuna devam etmek için **Devam edin'i**tıklatın. 
5. Başlamak isteyip istemediğiniz bir onay sorusu görüntülenir. **Evet'i**tıklatın.
6. **Durumun** **Devam**Ettiğini Fark Edin.

    ![Sürdürülüyor](./media/pause-and-resume-compute-portal/resuming.png)

7. SQL havuzu tekrar çevrimiçi olduğunda, durum **Çevrimiçi** ve seçenek düğmesi **Duraklatma'** dır.
8. SQL havuzu için işlem kaynakları artık çevrimiçi ve hizmeti kullanabilirsiniz. İşlem ücretleri devam etti.

    ![Çevrimiçi işlem yapın](././media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve SQL havuzunuzda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Verileri depolamada tutmak istiyorsanız, bilgi işlem'i duraklatın.
- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın ve SQL havuzuna tıklayın.

    ![Kaynakları temizleme](./media/pause-and-resume-compute-portal/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. 

2. İşlem veya depolama için ücretlendirilmemek için SQL havuzunu kaldırmak için **Sil'i**tıklatın.

3. Oluşturduğunuz SQL sunucusunu kaldırmak için **sqlpoolservername.database.windows.net**ve **sil'i**tıklatın.  

   > [!CAUTION]
   > Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Şimdi DURAKladınız ve SQL havuzunuz için işlem devam ettirin. [VERILERI SQL havuzuna](load-data-from-azure-blob-storage-using-polybase.md)nasıl yükleyin hakkında daha fazla bilgi edinmek için sonraki makaleye devam edin. Bilgi işlem yeteneklerini yönetme hakkında daha fazla bilgi için, [bilgi işlem genel bakış](sql-data-warehouse-manage-compute-overview.md) makalesini yönet'e bakın. 

