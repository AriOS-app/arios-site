# Configuration Formspree pour ARiOS Studio

## 📋 Étapes de configuration

### 1. Créer un compte Formspree
1. Allez sur [https://formspree.io](https://formspree.io)
2. Créez un compte gratuit avec l'email `arthur@arios.app`

### 2. Créer un formulaire
1. Dans le dashboard Formspree, cliquez sur "New Form"
2. Nommez votre formulaire "ARiOS Studio Contact"
3. Configurez l'email de destination : `arthur@arios.app`

### 3. Récupérer l'ID du formulaire
1. Formspree vous donnera un endpoint comme : `https://formspree.io/f/xyzabc123`
2. Copiez uniquement l'ID (ex: `xyzabc123`)

### 4. Mettre à jour les fichiers
Remplacez `YOUR_FORM_ID` dans les deux fichiers :
- `/index.html` (ligne 246)
- `/en/index.html` (ligne 246)

Par votre ID Formspree réel.

### 5. Configuration optionnelle dans Formspree

#### Emails de notification personnalisés
- Personnalisez le template d'email dans les paramètres Formspree
- Ajoutez votre logo et couleurs de marque

#### Page de remerciement
Créez une page de remerciement personnalisée :

1. Créez un fichier `/thank-you.html` et `/en/thank-you.html`
2. Dans Formspree, configurez la redirection vers ces pages

#### Protection anti-spam
- Le honeypot (`_gotcha`) est déjà configuré
- Activez reCAPTCHA dans Formspree si nécessaire

## 🎯 Fonctionnalités incluses

- ✅ Envoi direct à votre email
- ✅ Protection anti-spam (honeypot)
- ✅ Subject line personnalisé
- ✅ Reply-to automatique
- ✅ Support multilingue (FR/EN)

## 📊 Limites du plan gratuit

- 50 soumissions par mois
- 1 formulaire
- Pas de fichiers joints

Pour plus de soumissions, passez au plan Formspree payant.

## 🧪 Test

1. Testez d'abord en local
2. Déployez sur votre serveur
3. Faites un test réel de soumission
4. Vérifiez la réception de l'email

## 💡 Alternative AJAX (optionnel)

Si vous voulez éviter la redirection, vous pouvez utiliser l'API Formspree avec AJAX. Voici le code à ajouter dans `script.js` :

```javascript
contactForm?.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const formData = new FormData(contactForm);
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    const originalText = submitBtn.textContent;
    const statusDiv = document.getElementById('form-status');
    const isEnglish = window.location.pathname.includes('/en/');
    
    submitBtn.textContent = isEnglish ? 'Sending...' : 'Envoi en cours...';
    submitBtn.disabled = true;
    
    try {
        const response = await fetch(contactForm.action, {
            method: 'POST',
            body: formData,
            headers: {
                'Accept': 'application/json'
            }
        });
        
        if (response.ok) {
            statusDiv.textContent = isEnglish ? 
                'Thank you! We will get back to you soon.' : 
                'Merci ! Nous vous répondrons rapidement.';
            statusDiv.className = 'form-status success';
            contactForm.reset();
        } else {
            throw new Error('Form submission failed');
        }
    } catch (error) {
        statusDiv.textContent = isEnglish ? 
            'An error occurred. Please try again.' : 
            'Une erreur est survenue. Veuillez réessayer.';
        statusDiv.className = 'form-status error';
    } finally {
        submitBtn.textContent = originalText;
        submitBtn.disabled = false;
        
        setTimeout(() => {
            statusDiv.style.display = 'none';
        }, 5000);
    }
});
```

---

Pour toute question : arthur@arios.app