---
ms.openlocfilehash: f4a1cc432a50a555fe6e050ca318b4cfaf1092d4
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682255"
---
MP4 dosyaları, OUTPUT_VIDEO_FOLDER_ON_DEVICE anahtarını kullanarak *. env* dosyasında yapılandırdığınız Edge cihazında bir dizine yazılır. Varsayılan değeri kullandıysanız, sonuçlar */var/Media/* klasöründe olmalıdır.

MP4 klibini oynatmak için:

1. Kaynak grubunuza gidin, VM 'yi bulun ve ardından Azure savunma kullanarak bağlanın.

    ![Kaynak grubu](../../../media/quickstarts/resource-group.png)
    
    ![VM](../../../media/quickstarts/virtual-machine.png)
1. [Azure kaynaklarınızı ayarlarken](../../../detect-motion-emit-events-quickstart.md#set-up-azure-resources)oluşturulan kimlik bilgilerini kullanarak oturum açın. 
1. Komut isteminde ilgili dizine gidin. Varsayılan konum */var/Media*' dir. MP4 dosyalarını dizinde görmeniz gerekir.

    ![Çıkış](../../../media/quickstarts/samples-output.png) 

1. Dosyaları yerel makinenize kopyalamak için [Güvenli kopya (SCP)](../../../../../virtual-machines/linux/copy-files-to-linux-vm-using-scp.md) kullanın. 
1. [VLC medya oynatıcı](https://www.videolan.org/vlc/) veya başka bir MP4 oynatıcı kullanarak dosyaları yürütün.
