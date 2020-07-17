---
title: Azure Data Box’ınızdan SMB aracılığıyla veri kopyalama öğreticisi | Microsoft Docs
description: SMB aracılığıyla Azure Data Box'a veri kopyalamayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 07/10/2020
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 1d39b29ba340b34d6f0add8ff5da473408bd6360
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259122"
---
# <a name="tutorial-copy-data-from-azure-data-box-via-smb-preview"></a>Öğretici: SMB aracılığıyla Azure Data Box’tan veri kopyalama (Önizleme)

Bu öğreticide yerel web arabirimini kullanarak bağlantı kurma ve Data Box’ınızdan şirket için sunucuya veri kopyalama işlemi anlatılmaktadır. Data Box cihazı, Azure Depolama hesabınızdan dışarı aktarılan verileri içerir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Data Box'a bağlanma
> * Data Box’tan veri kopyalama

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Azure Data Box siparişi verdiniz.
    - İçeri aktarma siparişi için bkz. [Öğretici: Azure Data Box sipariş etme](data-box-deploy-ordered.md) bölümünü tamamladınız.
    - Dışarı aktarma siparişi için bkz. [Öğretici: Azure Data Box sipariş etme](data-box-deploy-export-ordered.md) bölümünü tamamladınız.
2. Data Box’ı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Data Box’ınızdan verileri kopyalamak istediğiniz bir ana bilgisayarınız var. Ana bilgisayarınız:
   * [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
   * Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanın ancak kopyalama hızınızın etkileneceğini göz önünde bulundurun.

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

[!INCLUDE [data-box-shares](../../includes/data-box-shares.md)]

Windows Server ana bilgisayarı kullanıyorsanız Data Box'a bağlanmak için aşağıdaki adımları izleyin.

1. İlk adım kimlik doğrulamasından geçmek ve oturum başlatmaktır. **Bağlan ve kopyala**'ya gidin. Depolama hesabınızla ilişkilendirilmiş paylaşımların erişim kimlik bilgilerini almak için **Kimlik bilgilerini al**’ı seçin. 

    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-export-copy-data/get-share-credentials-1.png)

2. Paylaşıma erişme ve veri kopyalama iletişim kutusunda paylaşıma karşılık gelen **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. **Tamam**’ı seçin.
    
    ![Paylaşım kimlik bilgilerini alma 2](media/data-box-deploy-export-copy-data/get-share-credentials-2.png)

3. Ana bilgisayarınızdan depolama hesabınızla (aşağıdaki örnekte *exportbvtdataset2*) ilişkili paylaşımlara erişmek için bir komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Veri biçimine bağlı olarak paylaşım yolları şu şekilde olacaktır:
    - Azure Blok blobu - `\\169.254.143.85\exportbvtdataset2_BlockBlob`
    - Azure Sayfa blobu - `\\169.254.143.85\exportbvtdataset2_PageBlob`
    - Azure Dosyalar - `\\169.254.143.85\exportbvtdataset2_AzFile`

4. İstendiğinde paylaşımın parolasını girin. Aşağıdaki örnekte yukarıdaki komutla paylaşıma bağlanma adımları gösterilmektedir.

    ```
    C:\Users\Databoxuser>net use \\169.254.143.85\exportbvtdataset2_BlockBlob /u:exportbvtdataset2
    Enter the password for 'exportbvtdataset2' to connect to '169.254.143.85':
    The command completed successfully.
    ```

4. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>` değerini belirtin. Dosya Gezgini’ni açmak için **Tamam**’ı seçin.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-export-copy-data/connect-shares-file-explorer-1.png)

    Artık paylaşımları klasörler olarak görebilirsiniz.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-export-copy-data/connect-shares-file-explorer-2.png)

    
Bir Linux istemcisi kullanıyorsanız, SMB paylaşımını bağlamak için aşağıdaki komutu kullanın. Aşağıdaki "vers" parametresi, Linux ana bilgisayarınızın desteklediği SMB sürümüdür. Aşağıdaki komutta verilen uygun sürümü takın. Data Box’ın desteklediği SMB sürümleri için bkz. [Linux istemcileri için desteklenen dosya sistemleri](https://docs.microsoft.com/azure/databox/data-box-system-requirements#supported-file-systems-for-linux-clients) 

```console
sudo mount -t nfs -o vers=2.1 169.254.143.85:/exportbvtdataset2_BlockBlob /home/databoxubuntuhost/databox
```

## <a name="copy-data-from-data-box"></a>Data Box’tan veri kopyalama

Data Box paylaşımlarına bağlandıktan sonra veri kopyalamaya başlayabilirsiniz.

[!INCLUDE [data-box-export-review-logs](../../includes/data-box-export-review-logs.md)]


 SMB paylaşımına bağlandıktan sonra verileri kopyalamaya başlayın. Verilerinizi kopyalamak için Robocopy gibi SMB uyumlu herhangi bir dosya kopyalama aracını kullanabilirsiniz. Robocopy ile birden fazla kopyalama işlemini başlatabilirsiniz. 


Robocopy komutu hakkında daha fazla bilgi için bkz. [Robocopy ve birkaç örnek](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

Kopyalama tamamlandıktan sonra **Pano**’ya giderek cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.

Şimdi işleme devam edip Data Box’ınızı Microsoft’a gönderebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Data Box'a bağlanma
> * Data Box’tan veri kopyalama

Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-export-picked-up.md)

