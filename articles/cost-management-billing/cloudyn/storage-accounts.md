---
title: Azure’da Cloudyn için depolama hesaplarını yapılandırma
description: Bu makalede Cloudyn için Azure depolama hesaplarını ve AWS depolama demetlerini yapılandırma adımları anlatılmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: benshy
ms.custom: secdec18
ROBOTS: NOINDEX
ms.openlocfilehash: c741f303958630415d54cabc62c04430f0b8b89d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474603"
---
# <a name="configure-storage-accounts-for-cloudyn"></a>Cloudyn için depolama hesaplarını yapılandırma

<!--- intent: As a Cloudyn user, I want to configure Cloudyn to use my cloud service provider storage account to store my reports. -->

Cloudyn raporlarını Cloudyn portalına, Azure depolama alanına veya AWS depolama demetlerine kaydedebilirsiniz. Raporlarınızı Cloudyn portalına kaydetmek ücretsizdir. Ancak raporlarınızı bulut hizmeti sağlayıcınızın depolama alanına kaydetmek isteğe bağlı ve ek ücrete tabidir. Bu makale, Azure depolama hesaplarınızı ve Amazon Web Services (AWS) depolama demetlerinizi raporlarınızı depolayacak şekilde yapılandırmanıza yardımcı olacaktır.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure depolama hesabına veya Amazon depolama demetine sahip olmanız gerekir.

Azure depolama hesabınız yoksa bir tane oluşturmanız gerekir. Azure depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md).

AWS Simple Storage Service (S3) demetiniz yoksa bir tane oluşturmanız gerekir. S3 demeti oluşturma hakkında daha fazla bilgi için bkz. [Demet oluşturma](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Azure depolama hesabınızı yapılandırma

Azure depolama hesabınızı Cloudyn tarafından kullanılacak şekilde yapılandırmak oldukça kolaydır. Depolama hesabıyla ilgili ayrıntılı bilgileri alın ve Cloudyn portalına kopyalayın.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. **Tüm Hizmetler**'e tıklayın, **Depolama hesapları**'nı seçin, kullanmak istediğiniz depolama hesabını bulun ve seçin.
3. Depolama hesabı sayfanızın **Ayarlar** bölümünde **Erişim Anahtarları**'na tıklayın.
4. **Depolama hesabı adı** ve key1 altındaki **Bağlantı dizesi** bilgilerini kopyalayın.  
   ![Depolama hesabı adı ve bağlantı dizesi bilgilerini kopyalayın](./media/storage-accounts/azure-storage-access-keys.png)  
5. Cloudyn portalını Azure portalından açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com) sayfasına gidip oturum açın.
6. Dişli simgesine tıklayıp **Rapor Depolama Alanı Yönetimi**'ni seçin.
7. **Yeni ekle +** öğesine tıklayın ve Microsoft Azure'ın seçili olduğundan emin olun. **Ad** alanına Azure depolama hesabınızın adını yapıştırın. **Bağlantı dizenizi** ilgili alana yapıştırın. Bir kapsayıcı adı girin ve ardından **Kaydet**'e tıklayın.  
   ![Azure depolama hesabı adını ve bağlantı dizesini yeni rapor depolama alanı ekle kutusuna yapıştırın](./media/storage-accounts/azure-cloudyn-storage.png)

   Yeni Azure rapor depolama girişi, depolama hesabı listesinde görüntülenir.  
    ![Listedeki yeni Azure rapor depolama girişi](./media/storage-accounts/azure-storage-entry.png)


Artık raporları Azure depolama alanına kaydedebilirsiniz. Herhangi bir rapor açıkken **Eylemler**'e tıklayın ve ardından **Raporu zamanla**'yı seçin. Rapora bir ad verin ve kendi URL'nizi ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet**'i ve depolama hesabını seçin. Rapor dosya adına eklenen bir ön ek girin. CSV veya JSON dosya biçimini seçin ve raporu kaydedin.

## <a name="configure-an-aws-storage-bucket"></a>AWS depolama demeti yapılandırma

Cloudyn mevcut AWS kimlik bilgilerini kullanır: Kullanıcı veya Rol ile raporları demetinize kaydeder. Cloudyn, erişimi test etmek için _check-bucket-permission.txt_ adlı küçük bir metin dosyasını demete kaydetmeye çalışır.

Cloudyn rolüne veya kullanıcısına demetinizde PutObject iznini vermeniz gerekir. Ardından var olan bir demeti kullanabilir veya raporları kaydetmek için yeni bir tane oluşturabilirsiniz. Son olarak depolama sınıfını nasıl yöneteceğinizi belirleyebilir, yaşam döngüsü kuralları koyabilir veya gereksiz dosyaları kaldırabilirsiniz.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>AWS kullanıcınıza veya rolünüze izin atama

Yeni bir ilke oluşturduğunuzda raporu S3 demetine kaydetmek için gerekli olan tam izinleri sağlarsınız.

1. AWS konsolunda oturum açın ve **Hizmetler**'i seçin.
2. Hizmetler listesinde **IAM** girişini seçin.
3. Konsolunun sol tarafında **İlkeler**'i seçin ve **İlke Oluştur**'a tıklayın.
4. **JSON** sekmesine tıklayın.
5. Aşağıdaki ilke, raporu bir S3 demetine kaydetmenizi sağlar. Aşağıdaki ilke örneğini kopyalayıp **JSON** sekmesine yapıştırın. &lt;bucketname&gt; değerini demetinizin adıyla değiştirin.

   ```json
   {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
   }
   ```

6. **İlkeyi gözden geçir**'e tıklayın.  
    ![Örnek bilgileri gösteren AWS JSON ilkesi](./media/storage-accounts/aws-policy.png)  
7. İlkeyi gözden geçir sayfasında ilkeniz için bir ad belirleyin. Örneğin, _CloudynSaveReport2S3_.
8. **İlke oluştur**'a tıklayın.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>İlkeyi hesabınızdaki bir Cloudyn rolüne veya kullanıcısına ekleyin

Yeni ilkeyi eklemek için AWS konsolunu açın ve Cloudyn rolünü veya kullanıcısını düzenleyin.

1. AWS konsolunda oturum açın, **Hizmetler**'i ve ardından hizmetler listesinden **IAM** girişini seçin.
2. Konsolun sol tarafından **Roller** veya **Kullanıcılar**'ı seçin.

**Roller için:**

  1. Cloudyn rolünün adına tıklayın.
  2. **İzinler** sekmesinde **İlke Ekle**'ye tıklayın.
  3. Oluşturduğunuz ilkeyi arayın, seçin ve ardından **İlke Ekle**'ye tıklayın.
    ![Cloudyn rolünüze eklenen örnek ilke](./media/storage-accounts/aws-attach-policy-role.png)

**Kullanıcılar için:**

1. Cloudyn kullanıcısını seçin.
2. **İzinler** sekmesinde **İzin ekle**'ye tıklayın.
3. **İzin Ver** bölümünde **Mevcut ilkeleri doğrudan ekle**'yi seçin.
4. Oluşturduğunuz ilkeyi arayın, seçin ve ardından **İleri: Gözden Geçir**'e tıklayın.
5. Rol adına izin ekle sayfasında **İzin ekle**'ye tıklayın.  
    ![Cloudyn kullanıcısına eklenen örnek ilke](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>İsteğe bağlı: Demet ilkesiyle izin ayarlama

İsterseniz demet ilkesi kullanarak da S3 demetinizde rapor oluşturma izni verebilirsiniz. Klasik S3 görünümünde:

1. Demet oluşturun veya var olanlardan birini seçin.
2. **İzinler** sekmesini seçip **Demet ilkesi**'ne tıklayın.
3. Aşağıdaki örnek ilkeyi kopyalayıp yapıştırın. &lt;bucket\_name&gt; ve &lt;Cloudyn\_principle&gt; yerine demetinizin ARN değerini yazın. Cloudyn tarafından kullanılan rolün veya kullanıcının ARN değerini değiştirin.

   ```
   {
   "Id": "Policy1485775646248",
   "Version": "2012-10-17",
   "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
   ]
   }
   ```

4. Demet ilke düzenleyicisinde **Kaydet**'e tıklayın.

### <a name="add-aws-report-storage-to-cloudyn"></a>Cloudyn'e AWS rapor depolama alanı ekleme

1. Cloudyn portalını Azure portalından açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com) sayfasına gidip oturum açın.
2. Dişli simgesine tıklayıp **Rapor Depolama Alanı Yönetimi**'ni seçin.
3. **Yeni ekle +** öğesine tıklayın ve AWS'nin seçili olduğundan emin olun.
4. Bir hesap ve depolama demeti seçin. AWS depolama demetinin adı otomatik olarak doldurulur.  
    ![Yeni rapor depolama alanı ekle kutusundaki örnek bilgiler](./media/storage-accounts/aws-cloudyn-storage.png)  
5. **Kaydet**'e ve **Tamam**'a tıklayın.

    Yeni AWS rapor depolama girişi, depolama hesabı listesinde görüntülenir.  
    ![Yeni AWS rapor depolama girişi, depolama hesabı listesinde gösterilir](./media/storage-accounts/aws-storage-entry.png)


Artık raporları Azure depolama alanına kaydedebilirsiniz. Herhangi bir rapor açıkken **Eylemler**'e tıklayın ve ardından **Raporu zamanla**'yı seçin. Rapora bir ad verin ve kendi URL'nizi ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet**'i ve depolama hesabını seçin. Rapor dosya adına eklenen bir ön ek girin. CSV veya JSON dosya biçimini seçin ve raporu kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn raporlarının temel yapısı ve işlevleri hakkında bilgi edinmek için bkz. [Cloudyn raporlarını anlama](understanding-cost-reports.md).
