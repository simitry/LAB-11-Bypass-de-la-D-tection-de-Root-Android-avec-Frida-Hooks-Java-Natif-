# LAB-11-Bypass-de-la-D-tection-de-Root-Android-avec-Frida-Hooks-Java-Natif-

## Description du lab

Ce lab a pour objectif de comprendre comment les applications Android détectent un appareil rooté et comment ces mécanismes peuvent être contournés à l’aide de Frida. L’analyse est réalisée en mode dynamique, en interceptant le comportement de l’application au moment de son exécution.

L’étude porte principalement sur des applications vulnérables comme Uncrackable1 et Firestorm, qui implémentent des mécanismes de détection de root au niveau Java et parfois natif.

## Démarche suivie

Dans un premier temps, l’appareil Android (émulateur) a été connecté via ADB avec le débogage USB activé. Le serveur Frida a ensuite été déployé et lancé sur l’appareil afin de permettre l’instrumentation dynamique.

Une fois la connexion établie, les applications installées ont été listées afin d’identifier les packages cibles. Cela a permis de sélectionner l’application Uncrackable1 pour les tests.

Ensuite, un script Frida a été utilisé pour intercepter les fonctions responsables de la détection du root. Ces fonctions ont été modifiées dynamiquement pour forcer un retour négatif, ce qui empêche l’application de détecter que l’appareil est rooté.

L’application a été lancée directement via Frida, ce qui permet d’injecter le script avant l’exécution du code de sécurité. Cela garantit que les protections sont contournées dès le démarrage.

## Résultats obtenus

Après injection du script, l’application ne détecte plus le root et s’exécute normalement sans afficher d’erreur ni se fermer. Les logs affichés confirment que les fonctions de détection ont été interceptées et modifiées.

L’analyse montre également que certaines bibliothèques de détection comme RootBeer ne sont pas toujours présentes, ce qui nécessite d’adapter les hooks en fonction de l’application ciblée.

# Analyse des captures


<img width="1110" height="584" alt="Screenshot 1" src="https://github.com/user-attachments/assets/8855f178-27c2-47a0-90d6-bb10cf5efe42" />



Cette capture montre le lancement de l’application via Frida. On observe que le processus est correctement attaché et que l’exécution reprend normalement. Un message d’erreur initial indique un problème de syntaxe dans le script, mais celui-ci n’empêche pas l’exécution des hooks.

<img width="1110" height="449" alt="Screenshot2" src="https://github.com/user-attachments/assets/62726142-4213-45a4-94fc-9415663cb42e" />

Les logs indiquent que les hooks ont été installés avec succès. Les messages confirment le contournement des vérifications Java ainsi que l’interception de certaines fonctions critiques comme Runtime.exec.

<img width="485" height="879" alt="Screenshot3" src="https://github.com/user-attachments/assets/fb18135e-4378-4537-9bf8-7496a03e4785" />


# Conclusion

Ce lab démontre l’efficacité de Frida pour analyser et contourner les mécanismes de sécurité des applications Android. L’approche dynamique permet de modifier le comportement de l’application en temps réel sans altérer son code source.

Cette méthode est largement utilisée en audit de sécurité mobile pour identifier les faiblesses des protections mises en place et tester la robustesse des applications face aux attaques.
