[Read english here](https://github.com/bfloo/punching-bags/blob/main/README.md)\
Merci d'utiliser ce pack Arène de Voilaroc, créé par SirMalo et Flo. Voici quelques instructions pour vous en servir au mieux :





### **Comment puis-je importer cette map dans mon projet ?**



Tout d'abord, dans Pokémon Studio, créez une nouvelle map pour votre projet et sélectionnez "022 Veilstone Gym.tmx" (que vous pouvez renommer comme bon vous semble) située dans "Veilstone Gym Project/Data/Tiled/Maps". Mettez à jour les maps, sauvegardez dans Studio, puis lancez votre jeu pour convertir les map vers RPG Maker XP.



Une fois cela fait, fermez le jeu et allez dans le dossier "Veilstone Gym Project/Data", puis copiez "Map022.rxdata". Collez-le ensuite n'importe où en dehors de votre projet (sur votre Bureau, par exemple) et remplacez le numéro de la map dans le nom du fichier par celui affiché dans RPG Maker (pour le connaître, sélectionnez la map qui vient d'être ajoutée dans RPG Maker et vérifiez le numéro de map indiqué en bas à droite de l'interface). Enfin, coupez/collez le fichier dans le dossier "\[Votre projet]/Data" et remplacez l'ancien .rxdata. Cela aura pour effet de coller tous les évènements dans votre map.



N'oubliez pas d'éditer l'évènement numéro 30 "§ exit door" pour modifier la commande de téléportation vers l'extérieur de votre Arène.



Vous devrez aussi modifier l'évènement de Mélina situé sur la map Exterior : copiez les 4 commandes de script et collez-les dans l'évènement qui téléportera votre joueur à l'intérieur de l'Arène (*a priori*, l'évènement de porte de l'Arène). Ces commandes passent tous les interrupteurs locaux (self-switches) de tous les évènements de sacs de frappe et des piles de pneus sur OFF, pour reset l'Arène à chaque fois que le joueur y rentre.



Enfin, n'oubliez pas de copier le script "00002 SandbagReset.rb" situé dans le dossier "Veilstone Gym Project/scripts" vers le dossier "scripts" de votre propre projet.





### **Comment tout ça fonctionne ?**



Le système global n'est pas très complexe, mais nous avons besoin d'un grand nombre de commandes pour gérer les animations pour chaque position de chaque sac de frappe. Schématiquement, chaque évènement de sac de frappe contient autant de pages que de position possible sur les rails. Puis, sur chaque page, nous allons vérifier l'orientation du joueur quand il touche le sac et jouer l'animation correspondante.



Il existe 3 types d'animations :

* Si le sac de frappe ne peut pas bouger, il se balancera juste un peu.
* Si le sac de frappe peut avancer jusqu'à un autre cran sur les rails, une première "pré-animation" va être jouée, puis le sac de frappe sera déplacé jusqu'au prochain cran d'arrêt, une animation d'arrêt sera jouée, et un interrupteur local sera activé pour passer sur la page correspondant à la nouvelle position du sac.
* Si le sac de frappe peut avancer ET qu'il y a une pile de pneus au bout de la course, l'animation décrite ci-dessus sera jouée, mais l'évènement va vérifier que l'interrupteur local de la pile de pneus est désactivé. Si c'est le cas, un son spécial et une aninmation supplémentaire de pneus qui se cassent seront joués, puis l'interrupteur local des pneus sera activé afin "d'effacer" les pneus et de permettre le passage du joueur.



Comme mentionné ci-avant, pour nous assurer que le joueur ne se retrouve pas bloqué s'il perd un combat dans l'Arène ou s'il sauvegarde puis qu'il recharge le jeu, deux autres systèmes ont été mis en place :

* L'évènement numéro 35, situé au milieu du ring inaccessible en bas à gauche de la map, appelle le script "Sandbag reset" à chaque fois que la map est chargée. Ce script vérifie "manuellement" quel interrupteur local est activé pour chaque sac de frappe et change automatiquement sa position en fonction. Sans ce script, la position des sacs serait réinitialisée à chaque chargement de la map, mais les interrupteurs locaux resteraient activés ou désactivés comme avant, créant des situations impossibles (ex : un sac situé à droite du rail mais réagissant comme s'il était à gauche).
* L'évènement qui se charge de téléporter le joueur à l'intérieur de l'Arène (dans la démo, le sprite de Mélina sur la map "Exterior") se charge également de désactiver l'ensemble des interrupteurs locaux de chaque évènement de sac de frappe et de pile de pneus afin que l'Arène soit remise à 0 à chaque fois que le joueur y entre. À défaut, le joueur pourrait se retrouver bloqué.



Cette Arène contient en plus un autre petit système parallèle : celui des escaliers d'un demi-tile. En empruntant ces escaliers, le joueur sera élevé de 16 pixels, soit un demi-tile dans la mesure où PSDK utilise une grille de 32x32 pixels. Malheureusement, le fonctionnement actuel des systemtags d'escaliers ou de pentes ne nous permet pas de gérer ce genre d'élévation, donc nous avons dû tricher un peu et modifier le "offset\_screen\_y" du joueur, des évènements sur les rings et les plateformes, ainsi que leurs ombres "à la main". Cela se fait de deux manières :

* Quand le joueur entre en contact avec les escaliers dans la bonne direction, une boucle se lance pour modifier progressivement sa hauteur ainsi que celle de son ombre (l'offset y) jusqu'à ce qu'il atteigne -16 (pour une montée) ou 0 (pour une descente).
* L'évènement numéro 8, situé sur le ring en bas à gauche de la map, modifie automatiquement l'offset des Dresseurs situés sur les rings et de la Championne à chaque fois que la map est chargée, ainsi que l'offset de leurs ombres.



Malheureusement, ce système est assez précaire et il **ne fonctionne pas avec les Pokémon followers**. Ainsi, l'évènement de téléportation doit bien désactiver les Pokémon followers pour l'Arène.





### **Comment puis-je créer ma propre disposition ?**



La section précédente devrait être suffisamment claire pour que vous compreniez comment tout fonctionne, mais voici quelques points à avoir en tête si vous voulez créer votre propre puzzle avec des sacs de frappe :

* 1 page = 1 position, et chaque position doit prévoir une animation pour chaque direction, selon que le sac reste immobile, qu'il peut avancer ou qu'il peut avancer ET détruire une pile de pneus. Normalement, la démo contient un exemple d'animation pour chacune de ces situations dans toutes les directions, donc le plus simple est de copier les bonnes parties d'évènements et de simplement modifier la longueur des parcours et des déplacements de caméra.
* Quand il y a une pile de pneus à détruire, assurez-vous que la condition au milieu de l'animation vérifie l'interrupteur local du bon évènement, et que la commande qui active l'interrupteur local le fasse sur le bon évènement.
* Quand votre nouvelle disposition est finalisée, n'oubliez pas de modifier le script pour entrer à la main la position de chaque sac de frappe en fonction de leurs interrupteurs locaux (c'est assez fastidieux...). N'oubliez pas non plus de modifier les 4 lignes de script contenues dans l'évènement de téléportation pour réinitialiser les interrupteurs locaux de tous vos évènements de sacs de frappe et de piles de pneus.



**En cas de besoin, merci de poster un message dans le thread correspondant du salon Ressources du Discord de Pokémon Workshop.**

