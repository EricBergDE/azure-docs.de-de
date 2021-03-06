---
title: 'Tutorial: Azure Active Directory-Integration mit SignalFx | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SignalFx konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: jeedes
ms.openlocfilehash: 50a86a01c22450ae2d92e6743fb6de7e652d4017
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Tutorial: Azure Active Directory-Integration mit SignalFx

In diesem Tutorial erfahren Sie, wie Sie SignalFx in Azure Active Directory (Azure AD) integrieren.

Die Integration von SignalFx in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SignalFx hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei SignalFx anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die SignalFx-Integration in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SignalFx-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von SignalFx aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-signalfx-from-the-gallery"></a>Hinzufügen von SignalFx aus dem Katalog
Zum Konfigurieren der Integration von SignalFx in Azure AD müssen Sie SignalFx aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um SignalFx aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SignalFx** ein, wählen Sie im Ergebnisbereich **SignalFx** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SignalFx in der Ergebnisliste](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei SignalFx mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SignalFx als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SignalFx muss eine Linkbeziehung eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit SignalFx zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SignalFx-Testbenutzers](#create-a-signalfx-test-user)**, um ein Pendant von Britta Simon in SignalFx zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SignalFx-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit SignalFx zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SignalFx** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SignalFx** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für SignalFx](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://api.signalfx.com/v1/saml/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > Der vorangehende Wert ist kein echter Wert. Sie aktualisieren den Wert mit der tatsächlichen Antwort-URL. Dies wird später in diesem Tutorial beschrieben.

4. Die SignalFx-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:   

    ![Configure Single Sign-On](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_attribute.png)

5. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ------------------- | -------------------- |    
    | User.FirstName          | user.givenname |
    | User.email          | user.mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | user.surname    |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-saas-signalfx-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren, Attribut hinzufügen](./media/active-directory-saas-signalfx-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.
    
    e. Klicken Sie auf **OK**.
 
6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-signalfx-tutorial/tutorial_general_400.png)

8. Führen Sie zum Generieren der **Metadaten-URL** die folgenden Schritte aus:

    a. Klicken Sie auf **App-Registrierungen**.
    
    ![Configure Single Sign-On](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_appregistrations.png)
   
    b. Klicken Sie auf **Endpunkte**, um das Dialogfeld **Endpunkte** zu öffnen.  
    
    ![Configure Single Sign-On](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_endpointicon.png)

    c. Klicken Sie auf die Schaltfläche „Kopieren“, um die **VERBUNDMETADATENDOKUMENT**-URL zu kopieren und in Editor einzufügen.
    
    ![Configure Single Sign-On](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_endpoint.png)
     
    d. Kehren Sie nun zur Eigenschaftenseite von **SignalFx** zurück, kopieren Sie die **Anwendungs-ID** mithilfe der Schaltfläche **Kopieren**, und fügen Sie sie in Editor ein.
 
    ![Configure Single Sign-On](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_appid.png)

    e. Generieren Sie die **Metadaten-URL** mithilfe des folgenden Musters:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

9. Klicken Sie im Abschnitt **SignalFx-Konfiguration** auf **SignalFx konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** aus dem Abschnitt **Kurzübersicht**.

    ![SignalFx-Konfiguration](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_configure.png) 

10. Melden Sie sich bei Ihrer SignalFx-Unternehmenswebsite als Administrator an.

11. Klicken Sie in SignalFx oben auf **Integrations (Integrationen)**, um die Seite „Integrations“ zu öffnen.

    ![SignalFx-Integration](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_intg.png)

12. Klicken Sie im Abschnitt **Login Services (Anmeldedienste)** auf die Kachel **Azure Active Directory**.
 
    ![SignalFx-SAML](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_saml.png)

13. Klicken Sie auf **NEW INTEGRATION (NEUE INTEGRATION)**, und führen Sie auf der Registerkarte **INSTALL (INSTALLIEREN)** die folgenden Schritte aus:
 
    ![SignalFx-SAML-Integrationsseite](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_azure.png)

    a. Geben Sie im Textfeld **Name** einen neuen Integrationsnamen ein, z.B. **OurOrgName SAML SSO**.

    b. Kopieren Sie den Wert **Integration ID**, und fügen Sie ihn der **Antwort-URL** im Textfeld **Antwort-URL** des Abschnitts **Domäne und URLs für SignalFx** im Azure-Portal wie in `https://api.signalfx.com/v1/saml/acs/<integration ID>` an.

    c. Klicken Sie auf **Upload File (Datei hochladen)**, um das vom Azure-Portal heruntergeladene **Base64-codierte Zertifikat** in das Textfeld **Certificate (Zertifikat)** hochzuladen.

    d. Fügen Sie in das Textfeld **Issuer URL (Aussteller-URL)** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Fügen Sie die **Metadaten-URL**, die Sie im Azure-Portal generiert haben, im Textfeld **Metadata URL (Metadaten-URL)** ein.

    f. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-signalfx-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-signalfx-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-signalfx-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-signalfx-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
  
### <a name="create-a-signalfx-test-user"></a>Erstellen eines SignalFx-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in SignalFx. SignalFx unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf SignalFx ein neuer Benutzer erstellt.

Wenn sich ein Benutzer mit einmaligem Anmelden mit SAML zum ersten Mal bei SignalFx anmeldet, sendet das [SignalFx-Supportteam](mailto:kmazzola@signalfx.com) ihm eine E-Mail mit einem Link, den er anklicken muss, um sich zu authentifizieren. Dies geschieht nur bei der erstmaligen Anmeldung des Benutzers; bei nachfolgenden Anmeldeversuchen ist keine E-Mail-Überprüfung erforderlich.

>[!Note]
>Setzen Sie sich mit dem [SignalFx-Supportteam](mailto:kmazzola@signalfx.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SignalFx gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon SignalFx zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **SignalFx**aus.

    ![SignalFx-Link in der Anwendungsliste](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SignalFx“ klicken, sollten Sie automatisch bei Ihrer SignalFx-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_203.png

