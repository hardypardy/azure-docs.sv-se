---
title: 'Självstudie: Konfigurera Apache Ambari-e-postmeddelanden i Azure HDInsight'
description: Den här artikeln beskriver hur du använder SendGrid med Apache Ambari för e-postaviseringar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: bdce735bdacbe7ff0752650c6949fdb361342c73
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542564"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Självstudie: Konfigurera Apache Ambari-e-postmeddelanden i Azure HDInsight

I den här självstudien konfigurerar du Apache Ambari-e-postmeddelanden med hjälp av SendGrid. [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) fören klar hanteringen och övervakningen av ett HDInsight-kluster genom att tillhandahålla ett LÄTTANVÄNT webb gränssnitt och REST API. Ambari ingår i HDInsight-kluster och används för att övervaka klustret och göra konfigurations ändringar. [SendGrid](https://sendgrid.com/solutions/) är en kostnads fri molnbaserad e-posttjänst som ger tillförlitlig transaktions-e-postleverans, skalbarhet och real tids analys tillsammans med flexibla API: er som gör det enkelt att integrera anpassade. Azure-kunder kan låsa upp 25 000 kostnadsfria e-postmeddelanden varje månad.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Hämta SendGrid användar namn
> * Konfigurera Apache Ambari-e-postmeddelanden

## <a name="prerequisites"></a>Förutsättningar

* Ett SendGrid e-postkonto. Instruktioner finns i [skicka e-post med hjälp av SendGrid med Azure](../sendgrid-dotnet-how-to-send-email.md) .

* An-HDInsight kluster. Se [skapa Apache Hadoop kluster med hjälp av Azure Portal](./hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="obtain-sendgrid-username"></a>Hämta SendGrid användar namn

1. Från [Azure Portal](https://portal.azure.com)navigerar du till din SendGrid-resurs.

1. På sidan Översikt väljer du **Hantera** för att gå till SendGrid-webbsidan för ditt konto.

    ![Översikt över SendGrid i Azure Portal](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. I den vänstra menyn navigerar du till ditt konto namn och sedan **konto information** .

    ![SendGrid instrument panels navigering](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. På sidan **konto information** registrerar du **användar namnet** .

    ![SendGrid-konto information](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Konfigurera e-postavisering om Ambari

1. I en webbläsare går du till `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts` , där `CLUSTERNAME` är namnet på klustret.

1. I list rutan **åtgärder** väljer du **Hantera meddelanden** .

1. Välj ikonen i fönstret **Hantera aviserings meddelanden** **+** .

    ![Skärm bild som visar dialog rutan hantera aviserings meddelanden.](./media/apache-ambari-email/azure-portal-create-notification.png)

1. I dialog rutan **skapa aviserings meddelande** anger du följande information:

    |Egenskap |Beskrivning |
    |---|---|
    |Namn|Ange ett namn för meddelandet.|
    |Grupper|Konfigurera efter behov.|
    |Allvarlighetsgrad|Konfigurera efter behov.|
    |Beskrivning|Valfritt.|
    |Metod|Lämna kvar **via e-post** .|
    |E-postmeddelande till|Ange e-postmeddelanden som ska ta emot meddelanden, avgränsade med kommatecken.|
    |SMTP-Server|`smtp.sendgrid.net`|
    |SMTP-port|25 eller 587 (för okrypterade/TLS-anslutningar).|
    |E-post från|Ange en e-postadress. Adressen behöver inte vara äkta.|
    |Använd autentisering|Markera den här kryss rutan.|
    |Användarnamn|Ange SendGrid användar namn.|
    |Lösenord|Ange det lösen ord som du använde när du skapade SendGrid-resursen i Azure.|
    |Bekräfta lösen ord|Ange lösenordet igen.|
    |Starta TLS|Markera den här kryss rutan|

    ![Skärm bild som visar dialog rutan skapa aviserings meddelande.](./media/apache-ambari-email/ambari-create-alert-notification.png)

    Välj **Spara** . Du kommer tillbaka till fönstret **Hantera aviserings meddelanden** .

1. I fönstret **Hantera aviserings meddelanden** väljer du **Stäng** .

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du lärt dig hur du konfigurerar Apache Ambari-e-postmeddelanden med hjälp av SendGrid. Använd följande för att lära dig mer om Apache Ambari:

* [Hantera HDInsight-kluster med hjälp av Apache Ambari-webbgränssnittet](./hdinsight-hadoop-manage-ambari.md)

* [Skapa ett aviserings meddelande](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)