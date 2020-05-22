---
title: storage-files-create-storage-account-portal
description: Azure Dosyaları için depolama hesabı oluşturun.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: a0db3ee9f32be35e62accc5df7b92697a883df20
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83778278"
---
Depolama hesabı, Azure dosya paylaşımını veya bloblar veya sorgular gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda paylaşım olabilir. Bir paylaşım, depolama hesabının kapasite limitlerine kadar sınırsız sayıda dosyayı depolayabilir.

Depolama hesabı oluşturmak için:

1. Sol taraftaki menüden **+** kaynak oluşturmak için öğesini seçin.
1. Arama kutusuna **depolama hesabı** girin, **Depolama hesabı - blob, dosya, tablo, kuyruk** seçeneğini belirleyin ve **Oluştur**’u seçin.
    ![Kaynak araması iletişim kutusunda, depolama hesabı girişinin nasıl göründüğüne dair ekran görüntüsü](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

1. **Ad** alanına *mystorageacct* komutunu yazın ve benzersiz bir ad olduğunu belirten yeşil onay işareti görüntüleninceye kadar komutun sonuna birkaç rastgele sayı yazın. Depolama hesabı adı tamamen küçük harflerden oluşmalı ve genel olarak benzersiz olmalıdır. Depolama hesabınızın adını not edin. Daha sonra bu adı kullanacaksınız. 
1. **Dağıtım modelinde**, **Kaynak Yöneticisi**varsayılan değerini bırakın. Azure Resource Manager ve klasik dağıtım modeli arasındaki farklılıklar hakkında daha fazla bilgi edinmek için bkz. [Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../articles/azure-resource-manager/management/deployment-models.md).
1. **Performans**altında varsayılan değeri **Standart**olarak tutun.
    
    > [!NOTE]
    > Bu hızlı başlangıç standart bir dosya paylaşımı oluşturur, ancak Premium dosya paylaşımlarını kullanmak istiyorsanız, bunun yerine **Premium** ' u seçin.

1. **Hesap türü** bölümünde **StorageV2** seçeneğini belirleyin. Farklı türlerde depolama hesapları hakkında daha fazla bilgi edinmek için bkz. [Azure depolama hesaplarını anlama](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

    > [!NOTE]
    > Bu hızlı başlangıçta, genel amaçlı bir v2 hesabı oluşturulur. Premium dosya paylaşımlarını kullanmak isterseniz, bunun yerine **FileStorage** ' ı seçin.

1. **Çoğaltma** bölümünde **Yerel olarak yedekli depolama (LRS)** seçeneğini belirleyin. 
1. **Güvenli aktarım gereklidir** bölümünde her zaman **Etkin** seçeneğini belirlemenizi öneririz. Bu seçenek hakkında daha fazla bilgi için bkz. [Aktarım sırasında şifrelemeyi anlama](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
1. **Abonelik** bölümünde, depolama hesabını oluşturmak için kullanılan aboneliği seçin. Yalnızca bir aboneliğiniz varsa bu, varsayılan değer olmalıdır.
1. **Kaynak grubu**’nda **Yeni oluştur**’u seçin. Ad için *myResourceGroup* girin.
1. **Konum** bölümünde **Doğu ABD**’yi seçin.
1. **Sanal ağlar** bölümünde varsayılan seçeneği **Devre dışı** olarak değiştirmeden bırakın. 
1. Depolama hesabının kolayca bulunması için **Panoya sabitle**’yi seçin.
1. İşleminiz bittiğinde dağıtımı başlatmak için **Oluştur**’a tıklayın.