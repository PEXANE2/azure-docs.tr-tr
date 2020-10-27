---
title: Hızlı başlangıç-Azure portal bir blob oluşturun
titleSuffix: Azure Storage
description: Bu hızlı başlangıçta nesne (Blob) depolamada Azure portalı kullanacaksınız. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için Azure portalını kullanırsınız.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.date: 10/19/2020
ms.author: tamram
ms.openlocfilehash: 0bf9d6eb68536588b35df93e13b04841d7868d31
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92547171"
---
# <a name="quickstart-upload-download-and-list-blobs-with-the-azure-portal"></a>Hızlı Başlangıç: Azure portalla kullanarak blobları yükleme, indirme ve listeleme

Bu hızlı başlangıçta, [Azure portalını](https://portal.azure.com/) kullanarak Azure Depolamada bir kapsayıcı oluşturma ve bu kapsayıcı içinde blok bloblarını karşıya yükleme ve indirme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-container"></a>Kapsayıcı oluşturma

Azure portalında bir kapsayıcı oluşturmak için şu adımları izleyin:

1. Azure portalında yeni depolama hesabınıza gidin.
2. Depolama hesabının sol menüsünde, **BLOB hizmeti** bölümüne gidin ve **kapsayıcılar** ' ı seçin.
3. **+ Kapsayıcı** düğmesini seçin.
4. Yeni kapsayıcınız için bir ad yazın. Kapsayıcı adı küçük harflerden oluşmalı ve bir harf veya rakamla başlamalıdır; yalnızca harf, rakam ve tire (-) karakterini içerebilir. Kapsayıcı ve BLOB adları hakkında daha fazla bilgi için bkz. [kapsayıcıları, Blobları ve meta verileri adlandırma ve başvuru](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).
5. Kapsayıcıya genel erişim düzeyini ayarlayın. Varsayılan düzey **Özel (anonim erişim yok)** şeklindedir.
6. Kapsayıcıyı oluşturmak için **Tamam** 'ı seçin.

    :::image type="content" source="media/storage-quickstart-blobs-portal/create-container.png" alt-text="Azure portalında kapsayıcı oluşturmayı gösteren ekran görüntüsü":::

## <a name="upload-a-block-blob"></a>Blok blobunu karşıya yükleme

Blok blobları, bir blob oluşturmak üzere birleştirilmiş veri bloklarından oluşur. Blob depolama kullanan çoğu senaryoda blok blobları kullanılır. Blok blobları, bulutta metin ve dosya, görüntü ve video gibi ikili verileri depolamak için idealdir. Bu hızlı başlangıçta blok blobları ile çalışma hakkında bilgi verilmektedir.

Azure portalında yeni kapsayıcınıza bir blok blobu yüklemek için aşağıdaki adımları izleyin:

1. Azure portalında, önceki bölümde oluşturduğunuz kapsayıcıya gidin.
1. İçerdiği blobların listesini görüntülemek üzere kapsayıcıyı seçin. Bu kapsayıcı yenidir, bu nedenle henüz blob içermemelidir.
1. Karşıya yükleme dikey penceresini açmak **için karşıya yükle düğmesini seçin** ve Blok Blobu olarak karşıya yüklenecek dosyayı bulmak için yerel dosya sisteminize gidin. Alternatif olarak, karşıya yükleme işleminin diğer ayarlarını yapılandırmak için **Gelişmiş** bölümünü genişletebilirsiniz.

    :::image type="content" source="media/storage-quickstart-blobs-portal/upload-blob.png" alt-text="Azure portalında kapsayıcı oluşturmayı gösteren ekran görüntüsü":::

1. Blobu karşıya yüklemek için **karşıya yükle** düğmesini seçin.
1. Bu şekilde istediğiniz sayıda blobu karşıya yükleyin. Artık yeni blobların kapsayıcı içinde listelendiğini görürsünüz.

## <a name="download-a-block-blob"></a>Blok blobu indirme

Bir blok blobunu tarayıcıda görüntülemek veya yerel dosya sisteminize kaydetmek üzere indirebilirsiniz. Bir blok blobu indirmek için şu adımları izleyin:

1. Önceki bölümde karşıya yüklediğiniz blob listesine gidin.
1. İndirmek istediğiniz bloba sağ tıklayın ve **İndir** 'i seçin.

    :::image type="content" source="media/storage-quickstart-blobs-portal/download-blob.png" alt-text="Azure portalında kapsayıcı oluşturmayı gösteren ekran görüntüsü":::

## <a name="delete-a-block-blob"></a>Blok Blobu silme

Azure portal bir veya daha fazla blobu silmek için şu adımları izleyin:

1. Azure portal kapsayıcıya gidin.
1. Kapsayıcıda Blobların listesini görüntüleyin.
1. Listeden bir veya daha fazla BLOB seçmek için onay kutusunu kullanın.
1. Seçili Blobları silmek için **Sil** düğmesini seçin.
1. İletişim kutusunda, silme işlemini onaylayın ve BLOB anlık görüntülerini da silmek isteyip istemediğinizi belirtin.

:::image type="content" source="media/storage-quickstart-blobs-portal/delete-blobs.png" alt-text="Azure portalında kapsayıcı oluşturmayı gösteren ekran görüntüsü":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz tüm kaynakları kaldırmak için kapsayıcıyı silmeniz yeterlidir. Kapsayıcıdaki tüm blob'lar da silinir.

Kapsayıcıyı silmek için:

1. Azure portalında, depolama hesabınızdaki kapsayıcı listesine gidin.
1. Silinecek kapsayıcıyı seçin.
1. **Daha fazla** düğmesini ( **...** ) ve ardından **Sil** ’i seçin.
1. Kapsayıcıyı silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsayıcı oluşturmayı ve Azure portal bir blobu yüklemeyi öğrendiniz. Bir Web uygulamasından blob depolamayla çalışma hakkında bilgi edinmek için, bir depolama hesabına görüntülerin nasıl yükleneceğini gösteren öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Azure depolama ile buluta görüntü verileri yükleme](storage-upload-process-images.md)
