---
title: Azure DevTest Labs için zorunlu yapıtları belirtin | Microsoft Docs
description: Laboratuvardaki sanal makinelere (VM 'Ler) Kullanıcı tarafından seçilen yapıtları yüklemeden önce yüklenmesi gereken zorunlu yapıtları belirtmeyi öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 0677f8bea35cb34735fdcf34e717eea349fad8bf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480312"
---
# <a name="specify-mandatory-artifacts-for-your-lab-in-azure-devtest-labs"></a>Azure DevTest Labs ' de laboratuvarınız için zorunlu yapıtlar belirtin
Laboratuvarın sahibi olarak, laboratuvarda oluşturulan her makineye uygulanan zorunlu yapıtları belirtebilirsiniz. Laboratuvarınızda her makinenin kurumsal ağınıza bağlanmasını istediğiniz bir senaryo düşünün. Bu durumda, makinenin kurumsal etki alanına bağlı olduğundan emin olmak için her laboratuvar kullanıcısının sanal makine oluşturma sırasında bir etki alanına katılması yapıtı eklemesi gerekir. Diğer bir deyişle, laboratuvar kullanıcılarının makinenize zorunlu yapıtlar uygulamayı unutmaları durumunda bir makineyi yeniden oluşturması gerekir. Laboratuvar sahibi olarak, etki alanına katılması yapıtı, laboratuvarınızda zorunlu bir yapıt olarak yaparsınız. Bu adım, her makinenin kurumsal ağa bağlı olduğundan ve laboratuvar kullanıcılarınız için zaman ve çaba tasarrufu sağlar.
 
Diğer zorunlu yapıtlar, Takımınızın kullandığı ortak bir aracı veya her makinenin varsayılan olarak sahip olması gereken platforma ilişkin bir güvenlik paketini içerebilir. Kısaca, laboratuvarınızda her makinenin zorunlu bir yapıtı olması gerekir. Zorunlu yapıtlar uygulanmış bir makineden özel bir görüntü oluşturursanız ve ardından bu görüntüden yeni bir makine oluşturursanız, oluşturma sırasında zorunlu yapıtlar makinede yeniden uygulanır. Bu davranış ayrıca, özel görüntü eski olsa bile, bundan sonra bir makine oluşturduğunuzda, zorunlu yapıların en güncel sürümü, oluşturma akışı sırasında buna uygulanır. 
 
Yalnızca parametresi olmayan yapıtlar zorunlu olanları destekler. Laboratuvar kullanıcılarınızın Laboratuvar oluşturma sırasında ek parametreler girmesi ve bu nedenle VM oluşturma işlemini basit hale getirmelerini gerektirmez. 

## <a name="specify-mandatory-artifacts"></a>Zorunlu yapıtları belirtme
Windows ve Linux makineleri için zorunlu yapıtları ayrı ayrı seçebilirsiniz. Ayrıca, bu yapıtları uygulanmasını istediğiniz sıraya göre de sıralayabilirsiniz. 

1. Laboratuvarınızın giriş sayfasında, **Ayarlar**' ın altında **yapılandırma ve ilkeler** ' i seçin. 
3. **Dış kaynaklar**altında **zorunlu yapıtlar** ' ı seçin. 
4. **Windows** bölümünde veya **Linux** bölümünde **Düzenle** ' yi seçin. Bu örnek, **Windows** seçeneğini kullanır. 

    ![Zorunlu yapıtlar sayfası-Düzenle düğmesi](media/devtest-lab-mandatory-artifacts/mandatory-artifacts-edit-button.png)
4. Bir yapıt seçin. Bu örnek **7-ZIP** seçeneğini kullanır. 
5. **Yapıt Ekle** sayfasında **Ekle**' yi seçin. 

    ![Zorunlu yapıtlar sayfası-7-zip Ekle](media/devtest-lab-mandatory-artifacts/add-seven-zip.png)
6. Başka bir yapıt eklemek için makaleyi seçin ve **Ekle**' yi seçin. Bu örnek, ikinci zorunlu yapıt olarak **Chrome** ekler.

    ![Zorunlu yapıtlar sayfası-Chrome ekleme](media/devtest-lab-mandatory-artifacts/add-chrome.png)
7. **Zorunlu yapıtlar** sayfasında, seçilen yapıların sayısını belirten bir ileti görürsünüz. İletiye tıklarsanız, seçtiğiniz yapıtları görürsünüz. Kaydetmek için **Kaydet** ' i seçin. 

    ![Zorunlu yapıtlar sayfası-yapıtları Kaydet](media/devtest-lab-mandatory-artifacts/save-artifacts.png)
8. Linux VM 'Ler için zorunlu yapıtları belirtmek için adımları tekrarlayın. 
    
    ![Zorunlu yapıtlar sayfası-Windows ve Linux yapıtları](media/devtest-lab-mandatory-artifacts/windows-linux-artifacts.png)
9. Listeden bir yapıyı **silmek** için... seçeneğini belirleyin **. (üç nokta)** satırı sonuna tıklayın ve **Sil**' i seçin. 
10. Listedeki yapıtları **yeniden sıralamak** için yapıtın üzerinde fare üzerine gelin,... seçeneğini belirleyin **. (üç nokta)** Bu, satırın başlangıcında görüntülenir ve öğeyi yeni konuma sürükleyin. 
11. Laboratuvarda zorunlu yapıtları kaydetmek için **Kaydet**' i seçin. 

    ![Zorunlu yapıtlar sayfası-yapıtları laboratuarda Kaydet](media/devtest-lab-mandatory-artifacts/save-to-lab.png)
12. Laboratuvarınızın giriş sayfasına dönmek için **yapılandırma ve ilkeler** sayfasını kapatın (sağ üst köşedeki **X** ' i seçin).  

## <a name="delete-a-mandatory-artifact"></a>Zorunlu yapıtı silme
Bir laboratuvardan zorunlu bir yapıyı silmek için aşağıdaki işlemleri yapın: 

1. **Ayarlar**' ın altında **yapılandırma ve ilkeler '** i seçin. 
2. **Dış kaynaklar**altında **zorunlu yapıtlar** ' ı seçin. 
3. **Windows** bölümünde veya **Linux** bölümünde **Düzenle** ' yi seçin. Bu örnek, **Windows** seçeneğini kullanır. 
4. En üstteki zorunlu yapıt sayısıyla birlikte iletiyi seçin. 

    ![Zorunlu yapıtlar sayfası-iletiyi seçin](media/devtest-lab-mandatory-artifacts/select-message-artifacts.png)
5. **Seçilen yapılar** sayfasında... seçeneğini belirleyin **. (üç nokta)** yapıt silinecek ve **Kaldır**' ı seçin. 
    
    ![Zorunlu yapıtlar sayfası-yapıtı kaldır](media/devtest-lab-mandatory-artifacts/remove-artifact.png)
6. **Seçilen yapılar** sayfasını kapatmak için **Tamam ' ı** seçin. 
7. **Zorunlu yapıtlar** sayfasında **Kaydet** ' i seçin.
8. Gerekirse **Linux** görüntüleri için adımları yineleyin. 
9. Laboratuvardaki tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin. 

## <a name="view-mandatory-artifacts-when-creating-a-vm"></a>VM oluştururken zorunlu yapıtları görüntüleme
Artık bir laboratuvar kullanıcısı olarak laboratuvarda bir VM oluştururken zorunlu yapıtların listesini görebilirsiniz. Laboratuvar sahibiniz tarafından laboratuvarda ayarlanan zorunlu yapıtları düzenleyemez veya silemezsiniz.

1. Laboratuvarınızın giriş sayfasında, menüden **genel bakış** ' ı seçin.
2. Laboratuvara bir VM eklemek için **+ Ekle**' yi seçin. 
3. Bir **temel görüntü**seçin. Bu örnekte **Windows Server, sürüm 1709**kullanılmaktadır.
4. **Yapılar** için, seçilen zorunlu yapıt sayısıyla ilgili bir ileti görtiğine dikkat edin. 
5. **Yapıtları**seçin. 
6. Laboratuvarın yapılandırmasında ve ilkelerinde belirttiğiniz **zorunlu yapıtları** görtığınızdan emin olun. 

    ![VM oluşturma-zorunlu yapıtlar](media/devtest-lab-mandatory-artifacts/create-vm-artifacts.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Laboratuvara git yapıt deposu eklemeyi](devtest-lab-add-artifact-repo.md)öğrenin.

