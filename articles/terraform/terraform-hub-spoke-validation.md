---
title: Öğretici - Terraform kullanarak Azure'da hub ve konuşan ağı doğrulama
description: Birbirine bağlı tüm sanal ağlarla hub ve kollu ağ topolojisini doğrulamak için öğretici.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 9ba4780c59e5e9da4999573abbc08ecd2738a2cd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74159196"
---
# <a name="tutorial-validate-a-hub-and-spoke-network-in-azure-using-terraform"></a>Öğretici: Terraform'u kullanarak Azure'da hub ve konuşan ağı doğrulama

Bu makalede, bu serinin önceki makalede oluşturulan terraform dosyaları yürütmek. Sonuç, demo sanal ağlar arasındaki bağlantının doğrulanmasıdır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hub VNet'i hub konuşan topolojide uygulamak için HCL'yi (HashiCorp Dili) kullanın
> * Dağıtılacak kaynakları doğrulamak için Terraform planını kullanma
> * Azure'daki kaynakları oluşturmak için Terraform uygulaması kullanın
> * Farklı ağlar arasındaki bağlantıyı doğrulama
> * Tüm kaynakları yok etmek için Terraform'u kullanın

## <a name="prerequisites"></a>Ön koşullar

1. [Azure'da Terraform ile bir hub ve kollu karma ağ topolojisi oluşturun.](./terraform-hub-spoke-introduction.md)
1. [Azure'da Terraform ile şirket içi sanal ağ oluşturun.](./terraform-hub-spoke-on-prem.md)
1. [Azure'da Terraform ile bir hub sanal ağı oluşturun.](./terraform-hub-spoke-hub-network.md)
1. [Azure'da Terraform ile hub sanal ağ cihazı oluşturun.](./terraform-hub-spoke-hub-nva.md)
1. [Azure'da Terraform ile konuşlu sanal ağlar oluşturun.](./terraform-hub-spoke-spoke-network.md)

## <a name="verify-your-configuration"></a>Yapılandırmanızı doğrulayın

[Ön koşulları](#prerequisites)tamamladıktan sonra, uygun config dosyalarının mevcut olduğunu doğrulayın.

1. [Azure portalına](https://portal.azure.com)göz atın.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd hub-spoke
    ```

1. Önceki `ls` öğreticilerde oluşturulan `.tf` config dosyalarının listeli olduğunu doğrulamak için komutu çalıştırın:

    ![Terraform demo config dosyaları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-config-files.png)

## <a name="deploy-the-resources"></a>Kaynakları dağıtma

1. Terraform sağlayıcısını başlatma:
    
    ```bash
    terraform init
    ```
    
    !["Terraform init" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-init.png)
    
1. Yürütmeden `terraform plan` önce dağıtımın etkisini görmek için komutu çalıştırın:

    ```bash
    terraform plan
    ```
    
    !["Terraform plan" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-plan.png)

1. Çözümü dağıtın:

    ```bash
    terraform apply
    ```
    
    Dağıtımı `yes` onaylamak istendiğinde girin.

    !["Terraform apply" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-apply.png)
    
## <a name="test-the-hub-vnet-and-spoke-vnets"></a>Hub VNet'i ve kollu VNet'leri test edin

Bu bölümde, benzetilen şirket içi ortamdan vnet hub'ına bağlantı nın nasıl test edilebildiğini gösterilmektedir.

1. Azure **portalında, onprem-vnet-rg** kaynak grubuna göz atın.

1. **onprem-vnet-rg** sekmesinde, **onprem-vm**adlı VM'yi seçin.

1. **Bağlan**’ı seçin.

1. **VM yerel hesabını kullanarak**metin Girişi'nin yanında, **ssh** komutunu panoya kopyalayın.

1. Linux komut isteminden, simüle edilen şirket içi ortama bağlanmak için çalıştırın. `ssh` Parametre dosyasında `on-prem.tf` belirtilen parolayı kullanın.

1. Hub `ping` VNet'teki atlama kutusu VM'ye bağlantıyı test etmek için komutu çalıştırın:

   ```bash
   ping 10.0.0.68
   ```

1. Her `ping` konuşmada atlama kutusu VM'lere bağlantıyı test etmek için komutu çalıştırın:

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

1. **onprem-vm** sanal makinedeki ssh oturumundan `exit` çıkmak &lt;için enter ve enter> tuşuna basın.

## <a name="troubleshoot-vpn-issues"></a>Sorun giderme VPN sorunları

VPN hatalarını çözme hakkında bilgi için, makaleye bakın, [Karma VPN bağlantısı sorun giderme](/azure/architecture/reference-architectures/hybrid-networking/troubleshoot-vpn).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, öğretici seride oluşturulan kaynakları silin.

1. Planda bildirilen kaynakları kaldırın:

    ```bash
    terraform destroy
    ```

    Kaynakların `yes` kaldırılmasını onaylamak için istendiğinde girin.

1. Dizinleri üst dizine değiştirin:

    ```bash
    cd ..
    ```

1. Dizinini `hub-scope` silin (tüm dosyaları dahil):

    ```bash
    rm -r hub-spoke
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Terraform'u kullanma hakkında daha fazla bilgi edinin](/azure/terraform)