---
title: Quickstart - Azure portalı ile bir leke oluşturma
titleSuffix: Azure Storage
description: Bu hızlı başlangıçta nesne (Blob) depolamada Azure portalı kullanacaksınız. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için Azure portalını kullanırsınız.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: 3ceeb878121782ea3664e1ed62d5d632b606f08e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061388"
---
# <a name="quickstart-upload-download-and-list-blobs-with-the-azure-portal"></a>Hızlı Başlangıç: Azure portalla kullanarak blobları yükleme, indirme ve listeleme

Bu hızlı başlangıçta, [Azure portalını](https://portal.azure.com/) kullanarak Azure Depolamada bir kapsayıcı oluşturma ve bu kapsayıcı içinde blok bloblarını karşıya yükleme ve indirme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Azure portalında bir kapsayıcı oluşturmak için şu adımları izleyin:

1. Azure portalında yeni depolama hesabınıza gidin.
2. Depolama hesabı nın sol menüsünde **Blob servis** bölümüne gidin ve ardından **Kapsayıcılar'ı**seçin.
3. **+ Kapsayıcı** düğmesini seçin.
4. Yeni kapsayıcınız için bir ad yazın. Kapsayıcı adı küçük harflerden oluşmalı ve bir harf veya rakamla başlamalıdır; yalnızca harf, rakam ve tire (-) karakterini içerebilir. Kapsayıcı ve blob adları hakkında daha fazla bilgi için, [bkz.](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
5. Kapsayıcıya genel erişim düzeyini ayarlayın. Varsayılan düzey **Özel (anonim erişim yok)** şeklindedir.
6. Kapsayıcıyı oluşturmak için **Tamam**'ı seçin.

    ![Azure portalında kapsayıcı oluşturmayı gösteren ekran görüntüsü](media/storage-quickstart-blobs-portal/create-container.png)

## <a name="upload-a-block-blob"></a>Blok blobunu karşıya yükleme

Blok blobları, bir blob oluşturmak üzere birleştirilmiş veri bloklarından oluşur. Blob depolama kullanan çoğu senaryoda blok blobları kullanılır. Blok blobları, bulutta metin ve dosya, görüntü ve video gibi ikili verileri depolamak için idealdir. Bu hızlı başlangıçta blok blobları ile çalışma hakkında bilgi verilmektedir. 

Azure portalında yeni kapsayıcınıza bir blok blobu yüklemek için aşağıdaki adımları izleyin:

1. Azure portalında, önceki bölümde oluşturduğunuz kapsayıcıya gidin.
2. İçerdiği blobların listesini görüntülemek üzere kapsayıcıyı seçin. Bu kapsayıcı yeni olduğundan içinde blob olmayacaktır.
3. Yükleme bıçağını açmak için **Yükle** düğmesini seçin
4. Blok blob olarak yüklenir bir dosya bulmak için yerel dosya sisteminize göz atın
     
    ![Yerel sürücünüzden bir blobu karşıya yüklemeyi gösteren ekran görüntüsü](media/storage-quickstart-blobs-portal/upload-blob.png)

5. İsteğe bağlı olarak, kimlik doğrulama türü, erişim katmanı veya sanal klasör yolu gibi diğer ayarı tanımlamak için gelişmiş bölümü genişletin.
6. **Yüklemeyi** gerçekleştirmek için Yükle düğmesini seçin
7. Bu şekilde istediğiniz sayıda blobu karşıya yükleyin. Artık yeni blobların kapsayıcı içinde listelendiğini görürsünüz.

## <a name="download-a-block-blob"></a>Blok blobu indirme

Bir blok blobunu tarayıcıda görüntülemek veya yerel dosya sisteminize kaydetmek üzere indirebilirsiniz. Bir blok blobu indirmek için şu adımları izleyin:

1. Önceki bölümde karşıya yüklediğiniz blob listesine gidin. 
2. İndirmek istediğiniz bloba sağ tıklayın ve **İndir**'i seçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz kaynakları kaldırmak için kapsayıcıyı silebilirsiniz. Kapsayıcıdaki tüm blob'lar da silinir.

Kapsayıcıyı silmek için:

1. Azure portalında, depolama hesabınızdaki kapsayıcı listesine gidin.
2. Silinecek kapsayıcıyı seçin.
3. **Daha fazla** düğmesini (**...**) ve ardından **Sil**’i seçin.
4. Kapsayıcıyı silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları Azure portalla yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-dotnet-how-to-use-blobs.md)

