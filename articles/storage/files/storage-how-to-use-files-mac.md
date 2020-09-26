---
title: macOS’da SMB üzerinden Azure dosya paylaşımını bağlama | Microsoft Docs
description: Finder veya Terminal kullanarak macOS ile SMB üzerinden Azure dosya paylaşımının nasıl bağlanacağını öğrenin. Azure Dosyaları, Microsoft’un kullanımı kolay bulut dosya sistemidir.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/23/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 119f4c0ea434bc431b40c905d9142e187b7d9474
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326074"
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>macOS’da SMB üzerinden Azure dosya paylaşımını bağlama
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları, macOS High Sierra 10.13 + tarafından kullanılabilen sektör standardı SMB 3 protokolüne bağlanabilir. Bu makalede, macOS’ta Bulucu kullanıcı arabirimiyle ve Terminal kullanarak Azure dosya paylaşımını bağlamanın iki farklı yolu gösterilir.

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>macOS’ta Azure dosya paylaşımını bağlama önkoşulları
* **Depolama hesabı adı**: Azure dosya paylaşımını bağlayabilmeniz için depolama hesabınızın adı gerekir.

* **Depolama hesabı anahtarı**: Azure dosya paylaşımını bağlayabilmeniz için birincil (veya ikincil) depolama anahtarı gerekir. SAS anahtarları şu an bağlama için desteklenmemektedir.

* **Bağlantı noktası 445’in açık olduğundan emin olun**: SMB, TCP bağlantı noktası 445 üzerinden iletişim kurar. İstemci makinenizde (Mac) güvenlik duvarının TCP bağlantı noktası 445’i engellemediğinden emin olun.

## <a name="mount-an-azure-file-share-via-finder"></a>Bulucu yoluyla Azure dosya paylaşımını bağlama
1. **Bulucu’yu açın**: Bulucu varsayılan olarak macOS’ta açıktır, ancak bunun seçili durumdaki uygulama olduğundan emin olmak için dock’taki “macOS yüz simgesine” tıklayın:  
    ![macOS yüz simgesi](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)

2. **"Git" menüsünden "sunucuya Bağlan" ı seçin**: ÖNKOŞULLARDAN UNC yolunu kullanarak, başlangıç çift eğik çizgi ( `\\` ) öğesini `smb://` ve tüm diğer ters eğik çizgileri () `\` eğik çizgi () olarak dönüştürün `/` . Bağlantınız şu şekilde görünmelidir: ![“Sunucuya Bağlan” iletişim kutusu](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)

3. **Kullanıcı adı ve parola istendiğinde depolama hesabı adını ve depolama hesabı anahtarını kullanın**: “Sunucuya Bağlan” iletişim kutusunda “Bağlan”a tıkladığınızda kullanıcı adı ve parola istenir (Bu alan otomatik olarak macOS kullanıcı adınızla doldurulur). macOS Anahtarlığınıza depolama hesabı adı/depolama hesabı anahtarı yerleştirebilirsiniz.

4. **Azure dosya paylaşımını istediğiniz gibi kullanın**: Kullanıcı adı ve parola yerine paylaşım adını ve depolama hesabı anahtarını koyduktan sonra paylaşım bağlanacaktır. Bunu, yerel klasör/dosya paylaşımını normalde kullandığınız gibi kullanabilirsiniz. Örneğin, dosyaları dosya paylaşımına sürükleyip bırakabilirsiniz:

    ![Bağlı bir Azure dosya paylaşımının anlık görüntüsü](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Terminal yoluyla Azure dosya paylaşımını bağlama
1.  `<storage-account-name>`, `<storage-account-key>` Ve değerlerini, `<share-name>`   ortamınız için uygun değerlerle değiştirin.

    ```
    open smb://<storage-account-name>:<storage-account-key>@<storage-account-name>.file.core.windows.net/<share-name>
    ```

2. **Azure dosya paylaşımını istediğiniz gibi kullanın**: Azure dosya paylaşımı, önceki komutta belirtilmiş olan bağlama noktasına bağlanır.  

    ![Bağlı Azure dosya paylaşımının anlık görüntüsü](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Mac 'nizi paylaşılan bilgisayarlara ve sunuculara bağlama-Apple desteği](https://support.apple.com/guide/mac-help/connect-mac-shared-computers-servers-mchlp1140/mac)