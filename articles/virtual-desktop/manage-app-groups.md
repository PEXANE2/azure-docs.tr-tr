---
title: Windows sanal masaüstü portalı için uygulama gruplarını yönetme-Azure
description: Azure portal ile Windows sanal masaüstü uygulama gruplarını yönetme.
author: Heidilohr
ms.topic: tutorial
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 6baada4c3b376b936f693d45e39e5887e77ef54b
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010098"
---
# <a name="tutorial-manage-app-groups-with-the-azure-portal"></a>Öğretici: Azure portal ile uygulama gruplarını yönetme

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/manage-app-groups-2019.md)bakın.

Yeni bir Windows sanal masaüstü konak havuzu için oluşturulan varsayılan uygulama grubu, tam masaüstünü de yayımlar. Ayrıca, konak havuzu için bir veya daha fazla RemoteApp uygulama grubu oluşturabilirsiniz. Bir RemoteApp uygulama grubu oluşturmak ve tek tek Başlat menüsü uygulamalarını yayımlamak için bu öğreticiyi izleyin.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * RemoteApp grubu oluşturun.
> * RemoteApp programlarına erişim izni verin.

## <a name="create-a-remoteapp-group"></a>RemoteApp grubu oluşturma

Azure portal veya PowerShell 'i kullanarak bir konak havuzu ve oturum ana bilgisayar VM 'Leri zaten oluşturduysanız, Azure portal uygulama gruplarını aşağıdaki işlemle ekleyebilirsiniz:

1.  [Azure portalında](https://portal.azure.com/) oturum açın.

2.  **Windows sanal masaüstü**araması yapın ve seçin.

3.  Sayfanın sol tarafındaki menüde bulunan **uygulama grupları** ' nı seçin ve **+ Ekle**' yi seçin.

4. **Temel bilgiler** sekmesinde, uygulama grubunu oluşturmak istediğiniz abonelik grubunu ve kaynak grubunu seçin. Mevcut bir kaynak grubunu seçmek yerine yeni bir kaynak grubu oluşturmayı da tercih edebilirsiniz.

5. **Konak havuzu**' nun yanındaki açılan menüden Uygulama grubuyla ilişkilendirilecek konak havuzunu seçin.

    >[!NOTE]
    >Uygulama grubuyla ilişkili ana bilgisayar havuzunu seçmeniz gerekir. Uygulama gruplarında bir oturum ana bilgisayardan sunulan uygulamalar veya masaüstleri bulunur ve oturum Konakları, konak havuzlarının bir parçasıdır. Oluşturma sırasında uygulama grubunun bir konak havuzuyla ilişkilendirilmesi gerekir.

    > [!div class="mx-imgBorder"]
    > ![Azure portal temel bilgiler sekmesinin ekran görüntüsü.](media/basics-tab.png)

6. Konak havuzunuza uygulama grupları eklemek istiyorsanız, ekranın sol tarafındaki menüden **konak havuzları** ' nı seçin.

    Ardından, uygulama gruplarını eklemek istediğiniz konak havuzunun adını seçin.

    Bundan sonra, ekranın sol tarafındaki menüden **uygulama grupları** ' nı seçin ve **+ Ekle**' yi seçin.

    Son olarak, içinde uygulama grubunu oluşturmak istediğiniz abonelik grubunu ve kaynak grubunu seçin. Açılan menüden var olan bir kaynak grubunun adını seçebilir ya da yeni oluştur ' u seçerek yenisini **oluşturabilirsiniz** .

      >[!NOTE]
      >Konak havuzunuza uygulama grupları eklediğinizde, bundan sonra gelen uygulama grubuyla ilgili konak havuzu zaten seçilidir.
      >
      > [!div class="mx-imgBorder"]
      >![Ana Havuz önceden seçili olan temel bilgiler sekmesinin ekran görüntüsü.](media/host-pool-selected.png)

7. Uygulama grubu türü altında **RemoteApp** ' i seçin ve ardından RemoteApp 'niz için bir ad girin.

      > [!div class="mx-imgBorder"]
      > ![Uygulama grubu türü alanlarının ekran görüntüsü. "RemoteApp" vurgulanır.](media/remoteapp-button.png)

8.  **Atamalar** sekmesini seçin.

9.  Tek tek kullanıcıları veya Kullanıcı gruplarını uygulama grubuna yayımlamak için **+ Azure AD kullanıcıları veya Kullanıcı grubu Ekle**' yi seçin.

10.  Uygulamaları eklemek istediğiniz kullanıcı sayısını seçin. Tek veya birden çok kullanıcı ve Kullanıcı grubu seçebilirsiniz.

     > [!div class="mx-imgBorder"]
     > ![Kullanıcı seçim menüsünün ekran görüntüsü.](media/select-users.png)

11.  **Seç**’i seçin.

12.  **Uygulamalar** sekmesini seçin ve **+ Uygulama Ekle**' yi seçin.

13.  Başlangıç menüsünden bir uygulama eklemek için:

      - **Uygulama kaynağı** ' na gidin ve açılan menüden **Başlat menüsü** ' nü seçin. Sonra, **uygulama** ' ya gidin ve açılan menüden uygulamayı seçin.

     > [!div class="mx-imgBorder"]
     > ![Başlangıç menüsü seçili olan uygulama ekleme ekranının ekran görüntüsü.](media/add-app-start.png)

      - **Görünen ad**alanında, kullanıcının istemcisinde gösterilecek uygulamanın adını girin.

      - Diğer seçenekleri olduğu gibi bırakın ve **Kaydet**' i seçin.

14. Belirli dosya yolundan bir uygulama eklemek için:

      - **Uygulama kaynağı** ' na gidin ve açılan menüden **dosya yolu** ' nu seçin.

      - Oturum ana bilgisayarındaki uygulamanın yolunu, ilişkili konak havuzuna kayıtlı olarak girin.

      - Uygulama **adı**, **görünen ad**, **simge yolu**ve **simge dizini** alanlarında uygulamanın ayrıntılarını girin.

      - **Kaydet**'i seçin.

     > [!div class="mx-imgBorder"]
     > ![Dosya yolu seçiliyken uygulama ekle sayfasının ekran görüntüsü.](media/add-app-file.png)

     Uygulama grubuna eklemek istediğiniz her uygulama için bu işlemi tekrarlayın.

15.  Sonra, **çalışma alanı** sekmesini seçin.

16.  Uygulama grubunu bir çalışma alanına kaydetmek istiyorsanız, **uygulama grubunu kaydet** ' e gidin ve **Evet**' i seçin. Uygulama grubunu daha sonra kaydetmeyi tercih ediyorsanız **Hayır**' ı seçin.

17.  **Evet**' i seçerseniz, uygulama grubunuzu kaydetmek için var olan bir çalışma alanı seçebilirsiniz.

       >[!NOTE]
       >Uygulama grubunu yalnızca konak havuzuyla aynı konumda oluşturulan çalışma alanlarına kaydedebilirsiniz. Açabilirsiniz. bir çalışma alanına yeni uygulama grubunuz ile aynı konak havuzundan daha önceden kaydolduysanız, seçilir ve düzenleyemezsiniz. Bir konak havuzundaki tüm uygulama gruplarının aynı çalışma alanına kayıtlı olması gerekir.

     > [!div class="mx-imgBorder"]
     > ![Zaten var olan bir çalışma alanı için uygulama grubu kaydet sayfasının ekran görüntüsü. Konak havuzu önceden seçildi.](media/register-existing.png)

18. İsteğe bağlı olarak, çalışma alanınızı düzenlemeyi kolay hale getirmek için etiketler oluşturmak istiyorsanız, **Etiketler** sekmesini seçin ve etiket adlarınızı girin.

19. İşiniz bittiğinde, **gözden geçir + oluştur** sekmesini seçin.

20. Doğrulama işleminin tamamlanabilmesi için bir bit bekleyin. İşlem tamamlandığında, uygulama grubunuzu dağıtmak için **Oluştur** ' u seçin.

Dağıtım işlemi sizin için aşağıdaki işlemleri yapar:

- RemoteApp uygulama grubunu oluşturun.
- Seçilen uygulamalarınızı uygulama grubuna ekleyin.
- Seçtiğiniz kullanıcılara ve Kullanıcı gruplarına yayınlanan uygulama grubunu yayımlayın.
- Bunu yapmayı seçerseniz, uygulama grubunu kaydedin.
- Daha sonra indirileceği ve kaydedebileceğiniz yapılandırmanıza göre Azure Resource Manager şablonuna yönelik bir bağlantı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulama grubu oluşturmayı, RemoteApp programları ile doldurmayı ve uygulama grubuna kullanıcı atamayı öğrendiniz. Doğrulama ana bilgisayar havuzu oluşturmayı öğrenmek için aşağıdaki öğreticiye bakın. Hizmet güncelleştirmelerini üretim ortamınıza göndermeden önce izlemek için bir doğrulama ana bilgisayar havuzu kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Hizmet güncelleştirmelerini doğrulamak için konak havuzu oluşturma](./create-validation-host-pool.md)
